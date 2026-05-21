---
name: dashboardbase
description: Use when building, wiring, or debugging an HTTP endpoint that Dashboardbase will poll to render a widget (BarChart, DonutChart, GaugeChart, KPI, LineChart, PieChart, Status, Table), or when writing a Dashboardbase setup file, configuring authentication, or sending reactions/notifications. Covers the response envelope, JSON schemas per widget, the recommended authentication method (API key via the `x-api-key` header), refresh intervals, hosting requirements, and a go-live checklist.
license: MIT
metadata:
  source-repo: dashboardbase-api
  generated-at: "2026-05-21T07:20:06Z"
  spec-version: "1.0"
---

# Dashboardbase Developer Skill

This skill teaches you to build HTTP endpoints whose JSON responses Dashboardbase can render as widgets, plus the setup-file format used to wire those endpoints into a dashboard.

## Mental model

Dashboardbase periodically polls each widget's configured HTTPS endpoint at the dashboard's `refreshInterval` (one of `1m`, `5m`, `10m`, `30m`). Your endpoint returns JSON in a fixed envelope; Dashboardbase parses it and renders the widget. There is no streaming and no callback — your endpoint is just a regular `GET` that returns the current snapshot of data.

## Response envelope (always true)

Every widget endpoint returns this shape:

```json
{
  "title": "Optional widget title",
  "actions": [
    { "title": "View Details", "type": "link", "url": "https://example.com/details" }
  ],
  "data": {}
}
```

- `title` (optional string) overrides the widget title configured in Dashboardbase.
- `actions` (optional array) renders link actions in the widget header. `type` is currently always `"link"`. `url` must be an absolute `https://` URL.
- `data` (required) is the widget-type-specific payload — see `references/<widget>.md` for each widget's exact shape.

## Request: the `dateRange` query parameter

Dashboardbase appends a `?dateRange=<value>` query parameter to your endpoint URL whenever the dashboard has a date range selector active. If you ignore it, your endpoint returns the same data regardless of what the user picks — silent footgun.

Supported values:

| `DateRange` value |
| --- |
| `Today` |
| `SevenDays` |
| `ThirtyDays` |
| `SixtyDays` |
| `NinetyDays` |

Your endpoint should map these to a time window (e.g. `SevenDays` → "last 7 days, ending now") and filter the underlying data accordingly. `Today` means the current calendar day. If the parameter is absent, return your sensible default (typically `ThirtyDays` for trend widgets, "all-time" for KPI counts).

```bash
curl -H "x-api-key: $KEY" "https://your-api.example.com/widgets/revenue?dateRange=SevenDays"
```

See `references/hosting-and-http.md` for handling defaults, mapping the values to SQL window functions, and caching considerations.

## Quickstart — your first KPI in 5 minutes (Node.js)

```js
import express from "express";
const app = express();

app.get("/widgets/mrr", (req, res) => {
  if (req.get("x-api-key") !== process.env.DASHBOARDBASE_KEY) return res.sendStatus(401);
  res.json({
    title: "MRR",
    actions: [{ title: "Open Stripe", type: "link", url: "https://dashboard.stripe.com" }],
    data: {
      header: {
        title: "$42,300",
        subtitle: "vs last month",
        badge: { text: "+8%", icon: "ArrowUp", color: "Success" }
      }
    }
  });
});

app.listen(3000);
```

Verify it:

```bash
curl -H "x-api-key: $DASHBOARDBASE_KEY" https://your-api.example.com/widgets/mrr
```

Connect it in Dashboardbase: create a KPI widget, point it at this URL with the `x-api-key` header, save. See `references/quickstart.md` for an extended walkthrough including a Table widget.

## When to load which reference

Load only what the current task needs — these files are progressive disclosure, not preloaded.

| Trigger | File |
|---|---|
| Building a **BarChart / DonutChart / GaugeChart / KPI / LineChart / PieChart / Status / Table** endpoint | `references/<widget>.md` (e.g. `references/kpi.md`, `references/bar-chart.md`) |
| Writing a **setup file** (declarative dashboard config) | `references/setup-files.md` (and `assets/setup-file.schema.json` for full schema) |
| Choosing or rotating **authentication** | `references/authentication.md` |
| Pushing **reactions / notifications / sounds** to a dashboard | `references/reactions.md` |
| Production **hosting** / TLS / status codes / latency | `references/hosting-and-http.md` |
| Endpoint returns 200 but widget is broken — **debugging** | `references/gotchas.md` |
| Shipping to **production** | `references/go-live-checklist.md` |
| **First-time** users wanting an extended walkthrough | `references/quickstart.md` |

The widget reference filenames are: `bar-chart.md`, `donut-chart.md`, `gauge-chart.md`, `kpi.md`, `line-chart.md`, `pie-chart.md`, `status.md`, `table.md`.

## Always-true rules (no need to read anything to apply these)

- **HTTPS only.** Endpoints must be reachable over `https://` with a valid certificate.
- **Respond within 5 seconds.** Dashboardbase treats slow responses as failures.
- **Return `200 OK` with valid JSON** on success. `204 No Content` renders the widget as empty, and `304 Not Modified` keeps previous data — both are valid non-error responses (see `references/hosting-and-http.md`). Any `4xx` / `5xx` status renders the widget in an error state.
- **Refresh intervals** are limited to `1m`, `5m`, `10m`, `30m`.
- **Enums are case-sensitive.** Use `"Success"`, not `"success"`. See the styling tables below.
- **`additionalProperties: false`.** Extra fields not in the schema cause render failures. Strip them before responding.
- **Idempotent `GET`.** The same request should yield the same data (modulo time). Do not mutate state.
- **Recommended auth:** API key via the `x-api-key` header. See `references/authentication.md` for alternatives.

## Validation loop — before declaring done

1. `curl` the endpoint and confirm it returns `200` in under 5 seconds.
2. JSON-validate the response against the relevant widget schema in `references/<widget>.md`.
3. Open the widget in Dashboardbase and confirm it renders. If it doesn't, load `references/gotchas.md`.
4. Confirm the configured `refreshInterval` is realistic given upstream rate limits.

## Top 5 gotchas (full list: `references/gotchas.md`)

1. **Enum casing.** `"color": "success"` is invalid — use `"Success"`.
2. **Extra fields fail silently.** `additionalProperties: false` means an unrecognised key turns the render off. Strip them.
3. **Action URLs must be absolute HTTPS.** Relative URLs and `http://` are rejected.
4. **`datasets` is always an array.** Even single-series charts use `"datasets": [{ ... }]`. Multi-series is just multiple entries — see each chart widget's "Multiple series" section.
5. **`null` ≠ missing.** Most optional fields should be omitted rather than set to `null`. Returning `"color": null` may render differently from omitting `color`.

## Styling enums (authoritative — these are the only valid values)

These tables are reflected from the source — every value listed here is accepted, every value not listed is rejected.

### `WidgetDataColor` (used by badges, datasets, indicators)

| `WidgetDataColor` value |
| --- |
| `Success` |
| `Warning` |
| `Danger` |
| `Blue` |
| `Green` |
| `Red` |
| `Yellow` |
| `Orange` |
| `Light` |
| `Dark` |

### `WidgetDataAlign` (text alignment)

| `WidgetDataAlign` value |
| --- |
| `Left` |
| `Center` |
| `Right` |

### `WidgetDataSize` (text/icon size)

| `WidgetDataSize` value |
| --- |
| `S` |
| `M` |
| `L` |
| `XL` |

### `WidgetDataIcon` (badge / value icon)

| `WidgetDataIcon` value |
| --- |
| `ArrowUp` |
| `ArrowDown` |

### `WidgetDataFill` (badge fill style)

| `WidgetDataFill` value |
| --- |
| `Solid` |
| `Clear` |
| `Outline` |

### `WidgetActionType` (action types)

| `WidgetActionType` value |
| --- |
| `link` |

## Supported widgets

- BarChart
- DonutChart
- GaugeChart
- KPI
- LineChart
- PieChart
- Status
- Table

Each has its own reference file under `references/` — see the filename list above.

---

*Skill generated at `2026-05-21T07:20:06Z` from the Dashboardbase API contract.*
