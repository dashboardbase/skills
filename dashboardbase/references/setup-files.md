# Setup files

> **Load this file when:** writing, importing, or exporting a declarative dashboard configuration, or when a user mentions "setup file", "dashboard JSON", or "wire widgets from a file".

A setup file is a single JSON document that describes a Dashboardbase dashboard — its widgets, where each widget's data comes from, and how often to refresh. Setup files are how you ship a dashboard configuration alongside your application code, so a user can "bring up the same dashboard" in their Dashboardbase organisation in one step.

## Schema reference

The full JSON Schema is bundled at `assets/setup-file.schema.json`. Load it when you need to validate a setup file or look up an obscure field. The top-level shape:

```json
{
  "$schema": "https://api.dashboardbase.com/setup-file/schema/v1.json",
  "version": 1,
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "MRR Overview",
  "baseUrl": "https://api.example.com",
  "datasources": [
    { "id": "stripe", "baseUrl": "https://api.stripe.com" }
  ],
  "refreshInterval": "5m",
  "mappings": []
}
```

- `version` is currently always `1`. Omit to default.
- `id` is the target dashboard UUID. If absent, a new dashboard is created on import. The value above is a placeholder — replace with a real UUID or omit the field.
- `name` (optional) is the human-readable dashboard name, so the file on disk carries which dashboard it configures.
- `baseUrl` is the default base URL for mappings without an explicit `datasourceId` or absolute `path`.
- `datasources[]` declares named datasources. Each `mapping` may reference one by `datasourceId`. Credentials live in Dashboardbase, never in the setup file.
- `refreshInterval` is one of `1m`, `5m`, `10m`, `30m`.
- `mappings[]` is the array of widget-to-endpoint wires — each entry is in one of four states described below.

## Widget `type` values

Mappings that create widgets (States B, C, and D) declare the widget with a `type` slug. These are the canonical values:

| Widget | Setup-file `type` | Reference |
| --- | --- | --- |
| BarChart | `bar` | `references/bar-chart.md` |
| DonutChart | `donut` | `references/donut-chart.md` |
| GaugeChart | `gauge` | `references/gauge-chart.md` |
| KPI | `kpi` | `references/kpi.md` |
| LineChart | `line` | `references/line-chart.md` |
| PieChart | `pie` | `references/pie-chart.md` |
| Status | `status` | `references/status.md` |
| Table | `table` | `references/table.md` |

## The four mapping states

Each entry in `mappings[]` is in exactly one of four states. You author States A–C by hand; State D is what the export endpoint produces.

### State A — Wire existing widget

Connect a widget that already exists in the dashboard to an endpoint. Requires `widgetId` and `path`. Must not include `type` or `plan`.

```json
{
  "$schema": "https://api.dashboardbase.com/setup-file/schema/v1.json",
  "version": 1,
  "id": "8f2e3a60-5975-4c63-9a84-2b638a5e3d3e",
  "baseUrl": "https://api.example.com",
  "mappings": [
    {
      "widgetId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "path": "/metrics/mrr"
    },
    {
      "widgetId": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb",
      "path": "/metrics/active-users"
    },
    {
      "widgetId": "cccccccc-cccc-cccc-cccc-cccccccccccc",
      "path": "/metrics/churn-rate"
    }
  ]
}
```

### State B — Create new wired widget

Create a new widget and point it at an endpoint. Requires `type` and `path`. Typically also includes `size` and `position` — copy those from a layout slot (see "Recommended layouts" below). This example uses the *Starter Kit* layout: two `Kpi` slots, the `Primary` slot for the line chart, and the `Secondary` slot for the table.

```json
{
  "$schema": "https://api.dashboardbase.com/setup-file/schema/v1.json",
  "version": 1,
  "baseUrl": "https://api.example.com",
  "refreshInterval": "5m",
  "mappings": [
    {
      "type": "kpi",
      "path": "/metrics/mrr",
      "size": { "w": 3, "h": 1 },
      "position": { "x": 0, "y": 0 }
    },
    {
      "type": "kpi",
      "path": "/metrics/active-users",
      "size": { "w": 3, "h": 1 },
      "position": { "x": 3, "y": 0 }
    },
    {
      "type": "line",
      "path": "/metrics/revenue-over-time",
      "size": { "w": 8, "h": 5 },
      "position": { "x": 0, "y": 1 }
    },
    {
      "type": "table",
      "path": "/metrics/top-customers",
      "size": { "w": 4, "h": 5 },
      "position": { "x": 8, "y": 1 }
    }
  ]
}
```

### State C — Create plan-only widget

Describe a widget you intend to build but for which no endpoint exists yet. Requires `type` and `plan` (free-text description of the data the widget should display). Useful for sketching dashboards before implementation. This example uses the *Starter Kit* layout: two `Kpi` slots, `Primary` for the line chart, `Secondary` for the pie chart.

```json
{
  "$schema": "https://api.dashboardbase.com/setup-file/schema/v1.json",
  "version": 1,
  "mappings": [
    {
      "type": "kpi",
      "size": { "w": 3, "h": 1 },
      "position": { "x": 0, "y": 0 },
      "plan": {
        "goal": "Monthly recurring revenue",
        "dateRange": "current month",
        "groupBy": null,
        "dataSource": "stripe"
      }
    },
    {
      "type": "kpi",
      "size": { "w": 3, "h": 1 },
      "position": { "x": 3, "y": 0 },
      "plan": {
        "goal": "Active users today",
        "dateRange": "today",
        "groupBy": null,
        "dataSource": "database"
      }
    },
    {
      "type": "line",
      "size": { "w": 8, "h": 5 },
      "position": { "x": 0, "y": 1 },
      "plan": {
        "goal": "Revenue over time",
        "dateRange": "last 30 days",
        "groupBy": "day",
        "dataSource": "stripe"
      }
    },
    {
      "type": "pie",
      "size": { "w": 4, "h": 5 },
      "position": { "x": 8, "y": 1 },
      "plan": {
        "goal": "Revenue breakdown by plan",
        "dateRange": "current month",
        "groupBy": "plan",
        "dataSource": "stripe"
      }
    }
  ]
}
```

### State D — Recreate existing widget (export format)

An existing widget exported with its full spec, so the dashboard can be recreated elsewhere: `widgetId` plus `type` / `size` / `position`, and either a `path` (wired widget) or a `plan` (plan widget). This is the shape the export endpoint produces — you rarely author it by hand, but you will encounter it when editing an exported file (see "Exporting an existing dashboard" below). This example's grid values are the *Spotlight* layout's `Kpi` and `Primary` slots.

```json
{
  "$schema": "https://api.dashboardbase.com/setup-file/schema/v1.json",
  "version": 1,
  "id": "8f2e3a60-5975-4c63-9a84-2b638a5e3d3e",
  "name": "MRR Overview",
  "baseUrl": "https://api.example.com",
  "refreshInterval": "5m",
  "mappings": [
    {
      "widgetId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "type": "kpi",
      "path": "/metrics/mrr",
      "size": { "w": 4, "h": 1 },
      "position": { "x": 8, "y": 0 }
    },
    {
      "widgetId": "bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb",
      "type": "line",
      "size": { "w": 8, "h": 6 },
      "position": { "x": 0, "y": 0 },
      "plan": {
        "goal": "Revenue over time",
        "dateRange": "last 30 days",
        "groupBy": "day",
        "dataSource": "stripe"
      }
    }
  ]
}
```

## Recommended layouts

When you are **creating new widgets** (mapping State B or State C), don't invent `position` / `size` values — start from one of the known-good layouts below so the imported dashboard looks right immediately.

Grid constraints (every layout slot below satisfies them):

- The grid is **12 columns wide**. A widget's `size.w` must be **3–12** columns and `size.h` must be **1–8** rows.
- If a mapping omits `size`, the widget gets its type's default: **3×1** for compact single-value widgets (KPI-style), **6×4** for everything else (charts, tables).
- The bundled `assets/setup-file.schema.json` flags out-of-range values, but the server-side import does **not** reject them — they are clamped when the widget is added, so the imported dashboard silently differs from your file. Stay in range.

How to use them:

1. List the widgets you're adding and note each one's slot role from the affinity table below (a single-number widget like KPI / Status / Gauge is a `Kpi` slot; a hero chart like Line / Bar is `Primary`; supporting charts and tables like Table / Pie / Donut are `Secondary`).
2. Pick the smallest layout whose slots cover that mix — leftover slots are fine to leave unused.
3. Assign each widget to a slot whose role it fits, and copy that slot's `position` and `size` straight into the mapping. The user can drag things around afterward.

Worked examples:

- **LineChart + Table** → *Two Columns*: the line chart takes the `Primary` slot (`position {x:0,y:0}`, `size {w:8,h:6}`), the table the `Secondary` slot (`position {x:8,y:0}`, `size {w:4,h:6}`).
- **LineChart + Table + KPI** → *Spotlight*: the KPI drops into the small `Kpi` slot, the line chart into `Primary`, the table into `Secondary`.

### Which widget fits which slot

| Slot role | Recommended widgets |
| --- | --- |
| `Kpi` | GaugeChart, KPI, Status |
| `Primary` | BarChart, LineChart, Table |
| `Secondary` | BarChart, DonutChart, GaugeChart, PieChart, Table |

### Layouts

#### Starter Kit

A balanced dashboard with KPIs and charts to get you started.

| Slot | position | size | role |
| --- | --- | --- | --- |
| 1 | `x:0 y:0` | `w:3 h:1` | `Kpi` |
| 2 | `x:3 y:0` | `w:3 h:1` | `Kpi` |
| 3 | `x:6 y:0` | `w:3 h:1` | `Kpi` |
| 4 | `x:9 y:0` | `w:3 h:1` | `Kpi` |
| 5 | `x:0 y:1` | `w:8 h:5` | `Primary` |
| 6 | `x:8 y:1` | `w:4 h:5` | `Secondary` |

#### Reversed Starter Kit

A starter kit with the second row's column order reversed.

| Slot | position | size | role |
| --- | --- | --- | --- |
| 1 | `x:0 y:0` | `w:3 h:1` | `Kpi` |
| 2 | `x:3 y:0` | `w:3 h:1` | `Kpi` |
| 3 | `x:6 y:0` | `w:3 h:1` | `Kpi` |
| 4 | `x:9 y:0` | `w:3 h:1` | `Kpi` |
| 5 | `x:0 y:1` | `w:4 h:5` | `Secondary` |
| 6 | `x:4 y:1` | `w:8 h:5` | `Primary` |

#### Starter Kit Single Column

A starter kit with a single large column in the second row.

| Slot | position | size | role |
| --- | --- | --- | --- |
| 1 | `x:0 y:0` | `w:3 h:1` | `Kpi` |
| 2 | `x:3 y:0` | `w:3 h:1` | `Kpi` |
| 3 | `x:6 y:0` | `w:3 h:1` | `Kpi` |
| 4 | `x:9 y:0` | `w:3 h:1` | `Kpi` |
| 5 | `x:0 y:1` | `w:12 h:5` | `Primary` |

#### Starter Kit - Two Rows

A starter kit with the second row split into two.

| Slot | position | size | role |
| --- | --- | --- | --- |
| 1 | `x:0 y:0` | `w:3 h:1` | `Kpi` |
| 2 | `x:3 y:0` | `w:3 h:1` | `Kpi` |
| 3 | `x:6 y:0` | `w:3 h:1` | `Kpi` |
| 4 | `x:9 y:0` | `w:3 h:1` | `Kpi` |
| 5 | `x:0 y:1` | `w:12 h:2` | `Primary` |
| 6 | `x:0 y:3` | `w:12 h:3` | `Secondary` |

#### Starter Kit - Three Columns

A starter kit with three columns in the second row.

| Slot | position | size | role |
| --- | --- | --- | --- |
| 1 | `x:0 y:0` | `w:3 h:1` | `Kpi` |
| 2 | `x:3 y:0` | `w:3 h:1` | `Kpi` |
| 3 | `x:6 y:0` | `w:3 h:1` | `Kpi` |
| 4 | `x:9 y:0` | `w:3 h:1` | `Kpi` |
| 5 | `x:0 y:1` | `w:4 h:5` | `Primary` |
| 6 | `x:4 y:1` | `w:4 h:5` | `Secondary` |
| 7 | `x:8 y:1` | `w:4 h:5` | `Secondary` |

#### Two Columns

Two full-height columns side by side, one wide and one narrow.

| Slot | position | size | role |
| --- | --- | --- | --- |
| 1 | `x:0 y:0` | `w:8 h:6` | `Primary` |
| 2 | `x:8 y:0` | `w:4 h:6` | `Secondary` |

#### Spotlight

A wide full-height panel beside a stacked pair in a narrow column.

| Slot | position | size | role |
| --- | --- | --- | --- |
| 1 | `x:0 y:0` | `w:8 h:6` | `Primary` |
| 2 | `x:8 y:0` | `w:4 h:1` | `Kpi` |
| 3 | `x:8 y:1` | `w:4 h:5` | `Secondary` |

#### Reversed Spotlight

A stacked pair in a narrow column beside a wide full-height panel.

| Slot | position | size | role |
| --- | --- | --- | --- |
| 1 | `x:0 y:0` | `w:4 h:1` | `Kpi` |
| 2 | `x:0 y:1` | `w:4 h:5` | `Secondary` |
| 3 | `x:4 y:0` | `w:8 h:6` | `Primary` |

### When no layout fits

The layouts above top out at four `Kpi` slots, one `Primary`, and two `Secondary` slots. When the widget mix is bigger, extend the closest layout instead of inventing a grid from scratch — keep the existing slots as-is and add rows below them:

- **More than four KPI-role widgets** → repeat the KPI row: four more `w:3 h:1` slots at `x:0 / 3 / 6 / 9` on the next row, shifting every later row's `y` down by 1.
- **More than one Primary-role widget** → give each extra one its own full-width row (`w:12 h:5`), or pair two side by side as `w:6 h:5`.
- **More Secondary-role widgets than slots** → tile the extras in rows of three `w:4 h:5` or two `w:6 h:5` below the layout.

Rules that keep an extended layout valid: each added row starts at `y` = the previous row's `y` + its `h` (so nothing overlaps), each row's widths sum to 12 columns, and every slot stays inside the size constraints listed above. Tell the user which layout you extended and how, so they can rearrange in Dashboardbase if they prefer.

## Example with multiple datasources

Grid values from the *Starter Kit* layout (two `Kpi` slots plus `Primary`):

```json
{
  "$schema": "https://api.dashboardbase.com/setup-file/schema/v1.json",
  "version": 1,
  "datasources": [
    {
      "id": "stripe",
      "baseUrl": "https://api.stripe.com"
    },
    {
      "id": "posthog",
      "baseUrl": "https://app.posthog.com"
    }
  ],
  "refreshInterval": "10m",
  "mappings": [
    {
      "type": "kpi",
      "path": "/v1/metrics/mrr",
      "datasourceId": "stripe",
      "size": { "w": 3, "h": 1 },
      "position": { "x": 0, "y": 0 }
    },
    {
      "type": "kpi",
      "path": "/api/projects/default/insights/trend",
      "datasourceId": "posthog",
      "size": { "w": 3, "h": 1 },
      "position": { "x": 3, "y": 0 }
    },
    {
      "type": "line",
      "path": "/v1/metrics/revenue-over-time",
      "datasourceId": "stripe",
      "size": { "w": 8, "h": 5 },
      "position": { "x": 0, "y": 1 }
    }
  ]
}
```

## Example with mixed states

Grid values from the *Starter Kit* layout — the new KPI takes the second `Kpi` slot (the wired State A widget already occupies the first) and the planned bar chart takes `Primary`:

```json
{
  "$schema": "https://api.dashboardbase.com/setup-file/schema/v1.json",
  "version": 1,
  "id": "8f2e3a60-5975-4c63-9a84-2b638a5e3d3e",
  "baseUrl": "https://api.example.com",
  "refreshInterval": "5m",
  "mappings": [
    {
      "widgetId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "path": "/metrics/mrr"
    },
    {
      "type": "kpi",
      "path": "/metrics/active-users",
      "size": { "w": 3, "h": 1 },
      "position": { "x": 3, "y": 0 }
    },
    {
      "type": "bar",
      "size": { "w": 8, "h": 5 },
      "position": { "x": 0, "y": 1 },
      "plan": {
        "goal": "Signups by country",
        "dateRange": "last 7 days",
        "groupBy": "country",
        "dataSource": "database"
      }
    }
  ]
}
```

## Where to keep the file

Save the setup file in your repo as `.dashboardbase/<slug>.json`, where `<slug>` is your dashboard name in lowercase with hyphens (e.g. `.dashboardbase/mrr-overview.json`). Keeping configs under `.dashboardbase/` lets you version-control all your dashboard configs alongside your code, one file per dashboard.

## Reuse the base URL from an existing setup file

Before writing a **new** setup file, check whether the user already has one — look in the `.dashboardbase/` directory (and any other setup-file JSON the project keeps). If one exists, reuse its connection details instead of asking the user again or falling back to a placeholder like `https://api.example.com`:

- Copy the top-level `baseUrl` so every new mapping points at the same host the user has already configured.
- Carry over any `datasources[]` entries and reference them by `datasourceId` when the new widgets are served by one of those same datasources.
- Only prompt the user for a base URL when no existing setup file is found and you cannot otherwise infer the host (for example from env files, app config, or an existing HTTP client in the codebase).

This keeps every dashboard in the repo pointed at the same API and saves the user from re-entering a URL they have already set up.

## How to load your setup file into Dashboardbase

After generating the JSON, a user has three ways to import it:

### Option 1 — Drag-and-drop the file

In the Dashboardbase web app, open the dashboard import area and drop the `.json` file onto the drop zone. Dashboardbase validates it server-side and creates / updates the dashboard.

### Option 2 — Paste the raw JSON

Some import dialogs accept pasted text. Copy the file contents and paste them into the import input field; Dashboardbase parses and validates server-side.

### Option 3 — POST to the validate endpoint (for tools)

```http
POST /bff/v1/organizations/{orgId}/setup-files/validate
```

Accepts either:

- **Raw body** with any `Content-Type` other than `application/json`. The entire body is treated as the setup file text.
- **JSON body** with `Content-Type: application/json` and the shape `{ "content": "<setup file text>" }`.

Response (`ValidateSetupFileResponse`):

```json
{
  "success": true,
  "errors": [
    { "path": "/mappings/0", "field": "widgetId", "message": "...", "line": 5, "column": 12 }
  ],
  "warnings": [
    { "path": "/refreshInterval", "message": "..." }
  ],
  "summary": {
    "mode": "newDashboard",
    "isPlanOnly": false,
    "isWiringExisting": false,
    "isMixed": false,
    "widgetCount": 4,
    "widgetsByType": { "kpi": 2, "line": 1, "table": 1 },
    "uniqueHosts": ["api.example.com"],
    "datasourceIdsReferenced": [],
    "existingDashboardId": null,
    "requiresAuth": true,
    "authPromptTargets": [{ "kind": "host", "value": "api.example.com" }]
  },
  "prettyPrinted": "<the file re-serialised with consistent formatting>"
}
```

Use `summary.requiresAuth` and `summary.authPromptTargets` to decide whether to prompt the user for credentials before importing. Use `errors[]` to surface line-and-column-precise validation feedback inside your tool.

## Exporting an existing dashboard

To turn an existing dashboard into a setup file (State D mappings, one per widget):

```http
POST /bff/v1/organizations/{orgId}/dashboards/{dashboardId}/export-setup-file
```

The response's `data` field contains the setup-file JSON with every widget's full spec. Save it under `.dashboardbase/` to version-control the dashboard, or import it into another organisation to clone it. Exported files never contain credentials.

## Common mistakes

- **Mixing State A and State B in the same mapping.** A mapping with both `widgetId` and `type` but **without** the full State D export shape (`size`, `position`, and one of `path` / `plan`) is invalid — either wire an existing widget (State A: `widgetId` + `path`, no `type`) or create a new one (State B: `type` + `path`). Only exported State D mappings legitimately carry both.
- **Including credentials in the file.** Setup files are designed to be shareable; credentials are configured in Dashboardbase per-datasource.
- **Using a `refreshInterval` outside the allowed set.** Only `1m`, `5m`, `10m`, `30m`.
- **Forgetting `$schema`.** Optional but recommended — editors with JSON Schema support will offer completion when it's present.
- **Relative `path` without `baseUrl` or `datasourceId`.** Either set a top-level `baseUrl`, declare a `datasourceId`, or use an absolute URL in `path`.
