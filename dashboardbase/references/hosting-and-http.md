# Hosting and HTTP semantics

> **Load this file when:** preparing an endpoint for production hosting, debugging TLS / latency / status-code issues, or deciding how your endpoint should behave under load.

## Hosting requirements

Your widget endpoint must be:

- **Publicly reachable.** Dashboardbase polls from its own infrastructure; localhost or private-network URLs do not work.
- **HTTPS-only** with a valid (non-self-signed) TLS certificate. `http://` is rejected.
- **DNS-stable.** Use a stable hostname; IP-only URLs are accepted but discouraged.
- **Fast.** Aim for a p95 response time under 2 seconds. Hard timeout is 10 seconds per attempt.

## HTTP method and idempotency

- Dashboardbase uses `GET` for widget polls. Other methods are not used.
- Your endpoint **must be idempotent**: the same request at the same time should yield the same data. Do not mutate state from a widget endpoint.

## The `dateRange` query parameter

When the dashboard has a date range selector active, Dashboardbase appends `?dateRange=<value>` to every poll. Supported values:

| `DateRange` value |
| --- |
| `Today` |
| `SevenDays` |
| `ThirtyDays` |
| `SixtyDays` |
| `NinetyDays` |

If your endpoint ignores this parameter, the widget always shows the same data and the user's date selector appears broken. Handle it explicitly.

`dateRange` is appended to whatever URL the widget is configured with, so if that URL already carries a query string the poll arrives as `?widget=mrr&dateRange=SevenDays`. Read both parameters — see `references/endpoint-layout.md`.

### Mapping values to time windows

Treat the values as windows ending "now":

| Value | Window |
|---|---|
| `Today` | Start of today (server time) → now |
| `SevenDays` | now − 7 days → now |
| `ThirtyDays` | now − 30 days → now |
| `SixtyDays` | now − 60 days → now |
| `NinetyDays` | now − 90 days → now |
| _(absent)_ | Your sensible default — typically `ThirtyDays` for trend widgets, "all-time" or "today" for counters |

Whichever window you resolve, echo it in the response's `header.subtitle` (e.g. `"Last 7 days"`) — it's the only way viewers can see the date filter was applied.

### Example (Node.js / SQL)

```js
app.get("/widgets/revenue", auth, async (req, res) => {
  const range = req.query.dateRange ?? "ThirtyDays";
  // Fall back rather than reject: a 400 puts the widget in an error state, and an
  // unrecognised value should degrade to your default window, not break the tile.
  const days = { Today: 0, SevenDays: 7, ThirtyDays: 30, SixtyDays: 60, NinetyDays: 90 }[range] ?? 30;

  const since = days === 0
    ? new Date(new Date().setHours(0, 0, 0, 0))
    : new Date(Date.now() - days * 24 * 60 * 60 * 1000);

  const total = await db.sum("amount").from("payments").where("created_at", ">=", since);
  res.json({ title: "Revenue", data: { header: { title: `$${total}` } } });
});
```

### Caching

- Cache keys must include `dateRange` — otherwise `Today` and `NinetyDays` collide and you serve wrong data.
- Different widgets on the same dashboard share the date selector, so multiple endpoints will receive the same `dateRange` on the same poll cycle. Coordinate caches if they query the same upstream.

### Backward compatibility

If you ship an endpoint without `dateRange` handling and add it later, no API change is needed — Dashboardbase always sends it (when active) regardless of whether you read it. Simply start reading the parameter when ready.

## Status codes

Dashboardbase interprets responses as follows:

| Status | Effect on widget |
|---|---|
| `200 OK` with a JSON body | Render the widget with the returned data. **This is the only success.** |
| `204 No Content` | Error state. A 204 has no body, and Dashboardbase parses the body of every 2xx — an empty one fails to parse. See gotcha 8 for what to send instead. |
| `304 Not Modified` | Error state. Only 2xx is treated as a response at all; a 304 is handled as a failed poll. Do not build conditional-request handling against Dashboardbase. |
| `3xx` redirects | Error state. Redirects are never followed (a redirect would be fetched after the SSRF guard passed). Serve the data at the configured URL. |
| `401 Unauthorized` | Show auth error. **Do not redirect.** |
| `403 Forbidden` | Show auth error. |
| `404 Not Found` | Show "endpoint missing" error. |
| `5xx` / `408` / `429` | Show server-error state — but see "Retries" below: these are retried immediately, not just on the next poll. |

Anything that is not `200` with a parseable JSON body puts the widget in an error state. There is no
"empty" or "unchanged" outcome in the contract: when you have nothing to show, say so inside a `200`
response — gotcha 8 gives the shape per widget type.

## Retries

A failed poll is not one request. Transient failures — `5xx`, `408`, `429`, and connection errors —
are retried **3 times** with 2s / 4s / 8s backoff, each attempt under its own 10-second timeout. So a
single poll of a dead or rate-limiting endpoint means **4 requests over roughly 50 seconds**.

- Do not rate-limit Dashboardbase on a budget that assumes one request per poll.
- Returning `429` does not slow Dashboardbase down — it is treated as transient and retried.
- Make the endpoint idempotent. It is a `GET`, so it should be anyway, but the retry makes it load-bearing.

## Latency budget

- **Target:** p95 < 2 seconds end-to-end (TCP + TLS + your handler).
- **Hard timeout:** 10 seconds per attempt. A slower response is a failed attempt and is retried (see "Retries" above), so a consistently slow endpoint burns ~50 seconds before the widget errors.
- **What this means in practice:** the widget endpoint should hit cache or a denormalised store; do not run expensive analytical queries on every poll. Aggregate upstream and serve the result.

## Refresh intervals

Pick the smallest interval that satisfies the data's freshness needs without overloading your upstream:

- `1m` — for rapidly changing metrics (active users, error rate during incidents).
- `5m` — the default. Suitable for most operational metrics.
- `10m` / `30m` — for slow-moving business metrics (MRR, churn, daily totals).

Dashboardbase polls every connected dashboard at this interval, so an endpoint backing 100 dashboards on `1m` receives ~100 requests per minute. Account for fan-out.

## Caching

- **Server-side caching is your responsibility, and it is the only caching there is.** A 30-second cache in front of an expensive query is usually enough to absorb Dashboardbase's poll cadence, any human refreshes, and the retry burst above.
- **Response cache headers are ignored.** Dashboardbase does not keep an HTTP cache: `Cache-Control`, `Expires`, `ETag` and `Last-Modified` have no effect on polling, and `If-None-Match` is never sent. Serving `304` will error the widget (see the status table).
- **Key your own cache by `dateRange`** — see gotcha 13. Two windows sharing a cache entry serves the wrong data.

## Compression

- `gzip` / `br` are supported. Enable compression for responses over a few KB.
- Most widget responses are small (< 5 KB); compression is optional.

## CORS

Dashboardbase polls server-to-server, so **CORS headers are not needed** on the widget endpoint. Browser-side fetches (e.g. for local testing) need CORS, but that's a development concern, not a production one.

## Error responses

Return a problem payload alongside non-`200` status codes for easier debugging:

```json
{ "error": "Invalid x-api-key" }
```

The body is not rendered by Dashboardbase, but it surfaces in logs and integration tests.

## Rate limiting your own endpoint

If you expose the same endpoint publicly, apply rate limiting **before** Dashboardbase's request — but be careful to allow Dashboardbase's poll rate. Either:

- **Allowlist Dashboardbase's IP range** in your rate limiter, OR
- **Use a separate hostname** for Dashboardbase polls (e.g. `dashboardbase.your-api.example.com`) without a rate limit.

## Common mistakes

- **Returning HTML on auth failure.** Auth middleware that returns a login HTML page makes Dashboardbase render garbage. Return JSON + `401`.
- **Redirecting (`301` / `302`) on auth failure.** Dashboardbase does not follow auth redirects. Return `401` directly.
- **Slow synchronous queries.** Running an analytical query on every poll exhausts your database. Cache or denormalise.
- **Returning ISO-8601 dates as JavaScript `Date` toString output.** Some libraries serialise as `"Mon May 19 2026 …"`; stick to `"2026-05-19T12:00:00Z"`.
- **Non-deterministic order.** Sorting rows differently on each poll causes flicker. Sort consistently.
