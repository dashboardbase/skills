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
- `baseUrl` is the default base URL for mappings without an explicit `datasourceId` or absolute `path`.
- `datasources[]` declares named datasources. Each `mapping` may reference one by `datasourceId`. Credentials live in Dashboardbase, never in the setup file.
- `refreshInterval` is one of `1m`, `5m`, `10m`, `30m`.
- `mappings[]` is the array of widget-to-endpoint wires — each entry is in one of three states described below.

## The three mapping states

Each entry in `mappings[]` is in exactly one of three states.

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

Create a new widget and point it at an endpoint. Requires `type` and `path`. Typically also includes `size` and `position`.

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
      "size": { "w": 3, "h": 2 },
      "position": { "x": 0, "y": 0 }
    },
    {
      "type": "kpi",
      "path": "/metrics/active-users",
      "size": { "w": 3, "h": 2 },
      "position": { "x": 3, "y": 0 }
    },
    {
      "type": "line",
      "path": "/metrics/revenue-over-time",
      "size": { "w": 12, "h": 3 },
      "position": { "x": 0, "y": 2 }
    },
    {
      "type": "table",
      "path": "/metrics/top-customers",
      "size": { "w": 6, "h": 3 },
      "position": { "x": 0, "y": 5 }
    }
  ]
}
```

### State C — Create plan-only widget

Describe a widget you intend to build but for which no endpoint exists yet. Requires `type` and `plan` (free-text description of the data the widget should display). Useful for sketching dashboards before implementation.

```json
{
  "$schema": "https://api.dashboardbase.com/setup-file/schema/v1.json",
  "version": 1,
  "mappings": [
    {
      "type": "kpi",
      "size": { "w": 3, "h": 2 },
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
      "size": { "w": 3, "h": 2 },
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
      "size": { "w": 12, "h": 3 },
      "position": { "x": 0, "y": 2 },
      "plan": {
        "goal": "Revenue over time",
        "dateRange": "last 30 days",
        "groupBy": "day",
        "dataSource": "stripe"
      }
    },
    {
      "type": "pie",
      "size": { "w": 6, "h": 3 },
      "position": { "x": 0, "y": 5 },
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

## Example with multiple datasources

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
      "size": { "w": 3, "h": 2 },
      "position": { "x": 0, "y": 0 }
    },
    {
      "type": "kpi",
      "path": "/api/projects/default/insights/trend",
      "datasourceId": "posthog",
      "size": { "w": 3, "h": 2 },
      "position": { "x": 3, "y": 0 }
    },
    {
      "type": "line",
      "path": "/v1/metrics/revenue-over-time",
      "datasourceId": "stripe",
      "size": { "w": 12, "h": 3 },
      "position": { "x": 0, "y": 2 }
    }
  ]
}
```

## Example with mixed states

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
      "size": { "w": 3, "h": 2 },
      "position": { "x": 3, "y": 0 }
    },
    {
      "type": "bar",
      "size": { "w": 12, "h": 3 },
      "position": { "x": 0, "y": 2 },
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

## Common mistakes

- **Mixing State A and State B in the same mapping.** A mapping with both `widgetId` and `type` is invalid — pick one.
- **Including credentials in the file.** Setup files are designed to be shareable; credentials are configured in Dashboardbase per-datasource.
- **Using a `refreshInterval` outside the allowed set.** Only `1m`, `5m`, `10m`, `30m`.
- **Forgetting `$schema`.** Optional but recommended — editors with JSON Schema support will offer completion when it's present.
- **Relative `path` without `baseUrl` or `datasourceId`.** Either set a top-level `baseUrl`, declare a `datasourceId`, or use an absolute URL in `path`.
