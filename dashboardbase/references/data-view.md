# View data

> **Load this file when:** adding "View data" to a widget endpoint — the table of rows behind a KPI, chart or list that opens from the widget's ⋮ menu — or working out why "View data" says the endpoint does not support it.

Every widget's ⋮ menu has **View data**. It opens a large table — full screen on a phone — with a search box and pages, filled by **the widget's own endpoint**: dashboardbase calls the same URL, with the same authentication, plus `view=data`. There is no second endpoint to register and nothing to configure.

An endpoint that ignores `view=data` keeps answering with its usual KPI or chart, and dashboardbase shows "this endpoint doesn't support View data yet" instead of the widget. That is how support is detected — so the existing response must stay exactly as it is when `view` is absent.

A **Table** widget already answers with a table, so its View data works the moment it exists: dashboardbase shows the rows it returns and searches and pages them itself.

## The request

```
GET https://api.example.com/widgets/orders?dateRange=SevenDays&view=data&page=2&pageSize=50&search=acme&audience=member
```

| Parameter | Value |
|---|---|
| `view` | Always `data` on a View data request. Branch on it at the top of the handler. |
| `page` | 1-based page number. |
| `pageSize` | Rows per page: 50 by default, never more than 100. |
| `search` | What the viewer typed. Absent when the box is empty; at most 200 characters. |
| `audience` | `member` — a signed-in member of the workspace. `public` — a public share link or a TV, where anyone can read the screen. |
| `dateRange` | The dashboard's range, exactly as on the widget's normal request (`Today`, `SevenDays`, `ThirtyDays`, `SixtyDays` or `NinetyDays`), or absent. |

Treat every value as untrusted: clamp `page` to ≥ 1 and `pageSize` to 1–100, and only ever pass `search` to a query as a bound parameter.

## The response

A complete **Table** response — whatever type the widget itself is — with `data.pagination` saying which slice the rows are:

```json
{
  "title": "Orders",
  "data": {
    "columns": [
      { "text": "Order", "width": 16 },
      { "text": "Customer", "width": 30 },
      { "text": "Total", "width": 18 },
      { "text": "Status", "width": 16 },
      { "text": "Placed", "width": 20 }
    ],
    "rows": [
      [{ "text": "#48210" }, { "text": "Ava Jensen" }, { "text": "$120" }, { "badge": { "text": "Paid", "color": "Success" } }, { "text": "Nov 22, 14:05" }],
      [{ "text": "#48209" }, { "text": "Noah Kim" }, { "text": "$65" }, { "badge": { "text": "Refunded", "color": "Danger" } }, { "text": "Nov 22, 13:41" }]
    ],
    "pagination": { "page": 1, "pageSize": 50, "total": 1234 }
  }
}
```

`pagination`:

- `page` (required) — the page you returned.
- `pageSize` (required) — the page size you used.
- `total` — rows matching the search across all pages. When counting is expensive, leave it out and send `hasMore: true|false` instead; the table then offers Next while `hasMore` is true.

Rules:

- Every Table cell works here — `text`, `badge`, `imageUrl`, `icon`, `link`, `progress`, `trend`, `bar`. See `references/table.md`.
- `rows: []` is allowed on a data-view page (a search that matches nothing); dashboardbase says so. A Table widget on the board still needs at least one row.
- A small list (a few hundred rows) may be returned whole with no `pagination`: dashboardbase then searches and pages it in the browser. Past 1,000 rows it is cut short, and a paginated page is cut to the `pageSize` dashboardbase asked for. Dashboardbase also keeps the first 20 columns and cuts cell and badge text at 120 characters with "…" — cells do not wrap, so send a short label and put the long text behind a `link`.
- `title` is optional; without it the widget's own title is used.

## How to implement it

1. **Branch first.** `if (view === 'data') return dataView(query)` before the existing code, which stays untouched.
2. **Same rows as the card.** Use the source, filters and `dateRange` window the widget aggregates, so the rows add up to its number.
3. **Search, sort, page — in that order, in the database.** Match case-insensitively on the columns a person would type (names, ids, emails, statuses); sort stably (newest first is a good default); `LIMIT`/`OFFSET` or a cursor. Return `total` from the same filtered query.
4. **Respect `audience`.** `public` rows are readable by anyone holding the share link or standing in front of the TV. Mask or drop personal data — emails, full names, addresses — and anything internal. When in doubt, `public` gets less. `audience` describes the viewer; it is not a credential — the query string is not covered by the request signature, so anyone able to call the endpoint can send `audience=member`. The rows are only as private as the endpoint's authentication: answer `view=data` only after the signature (or Endpoint Secret) check passes, the same as the widget. An unauthenticated endpoint that answers `view=data` publishes its rows to the internet.
5. **Format for reading.** Cells are shown as sent: `$1,240`, `Nov 22, 14:05`, `3.4%`.
6. **Keep it fast.** It runs while a person waits, on every page change and search.

## Verify

Validate the page as a **Table**, whatever the widget's own type — and use a search that matches, because the validator rejects an empty `rows`:

```bash
curl -sS -X POST https://api.dashboardbase.com/tools/v1/validate/widget-response \
  -H 'Content-Type: application/json' \
  -d '{"widgetType":"Table","response":<THE VIEW=DATA RESPONSE BODY>}'
```

Then call the endpoint without `view` and validate that response with the widget's own type, to prove the board is unchanged.

## Troubleshooting

| View data shows | Cause |
|---|---|
| "doesn't support View data yet" | The response to `view=data` had no `data.rows` array — the handler never branched, or a gateway/cache in front stripped the query string. |
| An error | The endpoint failed or the table did not validate. Owners see the status and body under Details. |
| The same page every time | The endpoint (or a CDN) ignores `page` — check it is read and part of any cache key. |
