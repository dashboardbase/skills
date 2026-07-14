# KPI widget

> **Load this file when:** building a single-number / headline / KPI tile.

## Purpose

Show a single headline number with optional subtitle and trend badge — ideal for MRR, active users, conversion rate, error count, anything you'd put in a 'big number' tile.

In a setup file, this widget's `type` is `kpi` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/kpi.json` for use with a JSON Schema validator:

```json
{
  "required": [
    "header"
  ],
  "type": "object",
  "properties": {
    "header": {
      "type": "object",
      "properties": {
        "title": {
          "type": "string",
          "nullable": true
        },
        "subtitle": {
          "type": "string",
          "nullable": true
        },
        "align": {
          "enum": [
            "Left",
            "Center",
            "Right"
          ],
          "type": "string",
          "nullable": true
        },
        "badge": {
          "required": [
            "text"
          ],
          "type": "object",
          "properties": {
            "text": {
              "minLength": 1,
              "type": "string"
            },
            "icon": {
              "enum": [
                "ArrowUp",
                "ArrowDown"
              ],
              "type": "string",
              "nullable": true
            },
            "color": {
              "enum": [
                "Success",
                "Warning",
                "Danger",
                "Blue",
                "Green",
                "Red",
                "Yellow",
                "Orange",
                "Light",
                "Dark"
              ],
              "type": "string",
              "nullable": true
            },
            "fill": {
              "enum": [
                "Solid",
                "Clear",
                "Outline"
              ],
              "type": "string",
              "nullable": true
            }
          },
          "additionalProperties": false,
          "nullable": true
        },
        "color": {
          "enum": [
            "Success",
            "Warning",
            "Danger",
            "Blue",
            "Green",
            "Red",
            "Yellow",
            "Orange",
            "Light",
            "Dark"
          ],
          "type": "string",
          "nullable": true
        },
        "size": {
          "enum": [
            "S",
            "M",
            "L",
            "XL"
          ],
          "type": "string",
          "nullable": true
        }
      },
      "additionalProperties": false
    }
  },
  "additionalProperties": false
}
```

## Example response

```json
{
  "title": "Total Sales",
  "actions": [
    {
      "title": "View Report",
      "type": "link",
      "url": "https://example.com/kpi-report"
    }
  ],
  "data": {
    "header": {
      "title": "5.4%",
      "subtitle": "vs last month",
      "badge": {
        "text": "-1%",
        "icon": "ArrowDown",
        "color": "Danger"
      }
    }
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/api-key/kpichart' \
-H "x-api-key: test"
```

## Header — headline, subtitle, and colored badge

This widget **requires** a `header` — it is the whole widget. Use the three parts together:

- `title` — the headline number.
- `subtitle` — the plain-text context line. Best use: state the time window (`"Last 7 days"`), and when your endpoint handles `?dateRange=`, echo the selected range here so users can see the filter is applied.
- `badge` — the colored element: `{ "text": "+8%", "icon": "ArrowUp", "color": "Success" }`. Color and icon render **only** on the badge — a trend placed in `subtitle` shows as plain text.

```json
{
  "header": {
    "title": "1,010",
    "subtitle": "Last 7 days",
    "badge": { "text": "+8%", "icon": "ArrowUp", "color": "Success" }
  }
}
```



## Variations

Other shapes and styling for this widget — pick the one closest to your data:

### Monthly recurring revenue

A monetary, point-in-time metric (MRR, ARR, balance) where the headline is a currency value.

```json
{
  "title": "MRR",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/mrr-details"
    }
  ],
  "data": {
    "header": {
      "title": "$12,345",
      "subtitle": "vs last month",
      "badge": {
        "text": "+$1,120",
        "icon": "ArrowUp",
        "color": "Success"
      }
    }
  }
}
```

### Active users (count)

A whole-number count of currently-active entities (active users, online devices, open tickets).

```json
{
  "title": "Daily active users",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/dau-details"
    }
  ],
  "data": {
    "header": {
      "title": "2,543",
      "subtitle": "vs last month",
      "badge": {
        "text": "+12%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    }
  }
}
```

### Alert metric (coloured header + outline badge)

A metric where a high value is bad (churn, error rate) — colour the header and badge to draw the eye.

```json
{
  "title": "Churn rate",
  "actions": [
    {
      "title": "View Cohorts",
      "type": "link",
      "url": "https://example.com/churn"
    }
  ],
  "data": {
    "header": {
      "title": "4.8%",
      "subtitle": "vs last month",
      "badge": {
        "text": "+0.6%",
        "icon": "ArrowUp",
        "color": "Danger",
        "fill": "Outline"
      },
      "color": "Danger"
    }
  }
}
```

### Metric with a critical alert

A metric that has breached a threshold and should surface a critical alert banner.

```json
{
  "title": "Error rate",
  "actions": [
    {
      "title": "View Incidents",
      "type": "link",
      "url": "https://example.com/incidents"
    }
  ],
  "data": {
    "header": {
      "title": "8.2%",
      "subtitle": "vs last hour",
      "badge": {
        "text": "+3.4%",
        "icon": "ArrowUp",
        "color": "Danger"
      },
      "color": "Danger"
    }
  },
  "alert": {
    "active": true,
    "level": "critical",
    "message": "Error rate above the 5% threshold"
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate your response's `data` field against `assets/schemas/kpi.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown`
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Returning the metric as `data.value` — KPI uses `header.title`, not a value field (that's GaugeChart).
- Forgetting `badge.text` when including a badge — `text` is required; icon and color are decoration.
- Putting the trend percentage in `header.subtitle` instead of `header.badge.text` — only badges render with color and icon.
