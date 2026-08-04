# Gotchas

> **Load this file when:** your endpoint returns `200 OK` but the widget shows an error, renders incorrectly, or behaves unexpectedly.

These are the environment-specific facts that defy reasonable assumptions — the things that will trip up a developer if they don't know to look for them.

## 1. Enum values are case-sensitive

`"color": "success"` is **invalid**. The only accepted spelling is `"Success"`. Same applies to every enum: `Align`, `Size`, `Icon`, `Fill`, `WidgetActionType`, `WidgetStatusIndicator`. See SKILL.md for the authoritative tables.

## 2. `additionalProperties: false` — extra fields cause silent render failures

Each widget schema has `additionalProperties: false` — it applies to the `data` payload. If `data` contains a key not in the schema (a typo, an extra debug field, a wrapper from your serialiser), Dashboardbase rejects the response. Strip unknown fields before responding. At the envelope level the allowed fields are exactly `title`, `actions`, `data`, and `alert` (see the response-envelope section in `SKILL.md`).

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

For `Table`, `"rows": []` is a validation error (must contain at least one row, and every row must contain at least one cell). For charts, an empty `"datasets": []` is likewise rejected. A dataset whose `data` is `[]` passes validation but draws an empty series.

What to send when there is nothing to show depends on whether the data has a natural zero shape:

- **A chart bucketed over a date range** (LineChart / BarChart over days, weeks or months, ContributionsGrid) — emit **every** bucket in the requested window with `{ "value": 0 }`, including when every bucket is zero. A flat zero line is the honest answer and reads as intentional; a blank widget reads as broken. Never collapse a quiet period to `204`.
- **A gauge** — send `value: 0` against the real `maxValue`. Do not signal emptiness with `maxValue: 0`; that is rejected.
- **Data with no zero shape** (a Table with no rows, a Pie/Donut with no categories) — return `204 No Content` and let Dashboardbase render the widget as empty. There is no honest way to draw a share-of-total of nothing.

## 9. Non-finite numbers fail JSON

`Infinity`, `-Infinity`, and `NaN` are not valid JSON. Convert these to numbers (e.g. `0` for `NaN`, your domain's maximum for `Infinity`) before serialising.

## 10. Truncation of large payloads

Responses larger than ~256 KB may be truncated by intermediate proxies. Most widget payloads are well under this, but a Table with hundreds of rows can exceed it — paginate upstream and show the top N.

## 11. Auth middleware that redirects (`302`)

If your auth layer redirects unauthenticated requests to a login page, Dashboardbase does not follow the redirect — the widget shows an opaque error. Return `401` directly with a JSON body.

## 12. Non-deterministic sort order causes flicker

If `rows` in a Table change order between polls (because your query sorts by `created_at` and two rows share a timestamp), the table flickers as Dashboardbase re-renders. Sort by a stable, unique field.

## 13. Ignoring the `dateRange` query parameter

When a dashboard has a date selector, Dashboardbase appends `?dateRange=<value>` to every poll, where `<value>` is one of `Today`, `SevenDays`, `ThirtyDays`, `SixtyDays`, or `NinetyDays`. If your endpoint ignores it, the widget never responds to the user's date selection — the data looks frozen. Read the parameter and map it to a time window (see `references/hosting-and-http.md`). Also make sure your **cache key includes `dateRange`** — otherwise `Today` and `NinetyDays` collide and you serve the wrong data.

## 14. Handling `dateRange` correctly, but not showing it

Even when your endpoint filters by `dateRange` correctly, nothing on the widget tells the viewer which window they're looking at — it can look like the selector was ignored even when it wasn't. Echo the active window in `header.subtitle` (e.g. `"Last 7 days"`), and remember the subtitle is plain text: colored accents belong on `header.badge`, not in the subtitle string.

## 15. A shared endpoint returning the wrong widget's shape

When one endpoint serves several widgets via `?widget=<slug>`, a widget that renders as an error (or as another widget's data) usually means the handler branched on the wrong thing. Check that it reads `widget` — not `dateRange` — to pick the payload, that every slug in the setup file has a matching case, and that an unknown slug returns `400` rather than `200` with an empty body, which renders as a blank widget and hides the typo. `additionalProperties: false` still applies per branch: validate each one separately — one `validate_widget_response` call per branch if that tool is available, otherwise against `assets/schemas/<widget>.json`. See `references/endpoint-layout.md`.

## 16. The chart is correct but the widget looks broken

A time-series widget that returns valid, accurate data can still render as an empty box with a crowd of
rotated date labels along the bottom. Three independent omissions compound into it, and all three are
worth fixing together:

- **No `header`.** The widget has no headline and no context line, so there is nothing to read when the
  plot itself is flat. LineChart and BarChart should always carry one — see "Make it look good" in
  `SKILL.md`. (On non-plotting widgets the header stays optional.)
- **A series that is entirely zeros.** This is usually correct (nothing happened in the window) but the
  line sits on the baseline and is easy to miss. Keep returning the zero-filled series — it is the
  honest answer — and let the `header` carry the meaning: `title` = `"0"`, `subtitle` = the window.
  Do not switch to `204`; see gotcha 8.
- **Dense axis ticks.** Thirty daily labels rotated 45° eat a third of the tile and swamp the plot. Set
  `ticksX: false` past roughly a dozen buckets; `header.subtitle` already states the window.

A widget with a header reading "0" over "Last 30 days" and a clean axis says "nothing happened,
and we know it". The same endpoint without those three says "this is broken".
