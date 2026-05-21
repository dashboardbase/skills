# Hosting and HTTP semantics

> **Load this file when:** preparing an endpoint for production hosting, debugging TLS / latency / status-code issues, or deciding how your endpoint should behave under load.

## Hosting requirements

Your widget endpoint must be:

- **Publicly reachable.** Dashboardbase polls from its own infrastructure; localhost or private-network URLs do not work.
- **HTTPS-only** with a valid (non-self-signed) TLS certificate. `http://` is rejected.
- **DNS-stable.** Use a stable hostname; IP-only URLs are accepted but discouraged.
- **Fast.** Aim for a p95 response time under 2 seconds. Hard timeout is 5 seconds.

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

### Example (Node.js / SQL)

```js
app.get("/widgets/revenue", auth, async (req, res) => {
  const range = req.query.dateRange ?? "ThirtyDays";
  const days = { Today: 0, SevenDays: 7, ThirtyDays: 30, SixtyDays: 60, NinetyDays: 90 }[range];
  if (days === undefined) return res.status(400).json({ error: "Unknown dateRange" });

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
| `200 OK` | Render the widget with the returned data. |
| `204 No Content` | Render the widget as empty / "no data". |
| `304 Not Modified` | Keep previous data. Use with `ETag` / `If-None-Match` to save bandwidth (optional). |
| `401 Unauthorized` | Show auth error. **Do not redirect.** |
| `403 Forbidden` | Show auth error. |
| `404 Not Found` | Show "endpoint missing" error. |
| `5xx` | Show server-error state. Dashboardbase will retry on the next poll. |

## Latency budget

- **Target:** p95 < 2 seconds end-to-end (TCP + TLS + your handler).
- **Hard timeout:** 5 seconds. Slower responses are treated as failures.
- **What this means in practice:** the widget endpoint should hit cache or a denormalised store; do not run expensive analytical queries on every poll. Aggregate upstream and serve the result.

## Refresh intervals

Pick the smallest interval that satisfies the data's freshness needs without overloading your upstream:

- `1m` — for rapidly changing metrics (active users, error rate during incidents).
- `5m` — the default. Suitable for most operational metrics.
- `10m` / `30m` — for slow-moving business metrics (MRR, churn, daily totals).

Dashboardbase polls every connected dashboard at this interval, so an endpoint backing 100 dashboards on `1m` receives ~100 requests per minute. Account for fan-out.

## Caching

- **Server-side caching is your responsibility.** A 30-second cache in front of an expensive query is often enough to handle Dashboardbase's poll cadence and any human refreshes.
- **`Cache-Control: max-age=N`** is honoured for re-poll suppression in some cases; do not rely on it for correctness.
- **`ETag` + `If-None-Match`** with `304` responses is supported and saves bandwidth.

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
