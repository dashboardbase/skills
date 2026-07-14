# Gotchas

> **Load this file when:** your endpoint returns `200 OK` but the widget shows an error, renders incorrectly, or behaves unexpectedly.

These are the environment-specific facts that defy reasonable assumptions — the things that will trip up a developer if they don't know to look for them.

## 1. Enum values are case-sensitive

`"color": "success"` is **invalid**. The only accepted spelling is `"Success"`. Same applies to every enum: `Align`, `Size`, `Icon`, `Fill`, `WidgetActionType`, `WidgetStatusIndicator`. See SKILL.md for the authoritative tables.

## 2. `additionalProperties: false` — extra fields cause silent render failures

Each widget schema has `additionalProperties: false`. If your response contains a key not in the schema (a typo, an extra debug field, a wrapper from your serialiser), Dashboardbase rejects the response. Strip unknown fields before responding.

Common offenders:
- `_id`, `__v` from MongoDB
- `created_at`, `updated_at` audit fields
- Wrapper objects from REST framework conventions (e.g. `{ "data": { ... }, "meta": { ... } }` when only `data` is expected)

## 3. `null` is not the same as omitted

Most optional fields should be **omitted**, not set to `null`. Returning `"color": null` may render differently from omitting `color`. When in doubt, omit.

The exception is fields that the schema explicitly marks as `nullable: true` — those tolerate `null`.

## 4. ISO-8601 datetimes only

If you put dates in `labels` for a LineChart, use ISO-8601 strings (`"2026-05-19T12:00:00Z"`) or pre-formatted strings (`"Mon"`, `"2026-05-19"`). Avoid JavaScript `Date.toString()` output (`"Mon May 19 2026 12:00:00 GMT+0000"`) — Dashboardbase treats labels as opaque strings.

## 5. Action URLs must be absolute HTTPS

```json
{ "actions": [{ "title": "Open", "type": "link", "url": "https://example.com" }] }
```

Relative URLs (`/details`) and `http://` URLs are rejected. Always use a fully-qualified `https://` URL.

## 6. `datasets` is always an array

Even a single-series chart uses `"datasets": [{ ... }]`, not `"datasets": { ... }`. Multi-series means multiple entries in the array.

## 7. Pie/Donut `color` is a list

For PieChart and DonutChart datasets, `color` is `List<WidgetDataColor>` (one per slice), **not** a single value. For Bar/Line, `color` on the dataset is a single value (the whole series).

## 8. Empty arrays are different from missing arrays

For `Table`, `Rows: []` is a validation error (must contain at least one row). For chart datasets, an empty `data: []` is also rejected. If you have no data to show, return `204 No Content` instead.

## 9. Non-finite numbers fail JSON

`Infinity`, `-Infinity`, and `NaN` are not valid JSON. Convert these to numbers (e.g. `0` for `NaN`, your domain's maximum for `Infinity`) before serialising.

## 10. Truncation of large payloads

Responses larger than ~256 KB may be truncated by intermediate proxies. Most widget payloads are well under this, but a Table with hundreds of rows can exceed it — paginate upstream and show the top N.

## 11. Auth middleware that redirects (`302`)

If your auth layer redirects unauthenticated requests to a login page, Dashboardbase does not follow the redirect — the widget shows an opaque error. Return `401` directly with a JSON body.

## 12. Non-deterministic sort order causes flicker

If `Rows` in a Table change order between polls (because your query sorts by `created_at` and two rows share a timestamp), the table flickers as Dashboardbase re-renders. Sort by a stable, unique field.

## 13. Ignoring the `dateRange` query parameter

When a dashboard has a date selector, Dashboardbase appends `?dateRange=<value>` to every poll, where `<value>` is one of `Today`, `SevenDays`, `ThirtyDays`, `SixtyDays`, or `NinetyDays`. If your endpoint ignores it, the widget never responds to the user's date selection — the data looks frozen. Read the parameter and map it to a time window (see `references/hosting-and-http.md`). Also make sure your **cache key includes `dateRange`** — otherwise `Today` and `NinetyDays` collide and you serve the wrong data.

## 14. Handling `dateRange` correctly, but not showing it

Even when your endpoint filters by `dateRange` correctly, nothing on the widget tells the viewer which window they're looking at — it can look like the selector was ignored even when it wasn't. Echo the active window in `header.subtitle` (e.g. `"Last 7 days"`), and remember the subtitle is plain text: colored accents belong on `header.badge`, not in the subtitle string.
