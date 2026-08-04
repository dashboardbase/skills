# GaugeChart widget

> **Load this file when:** building a gauge / dial / progress indicator.

## Purpose

Show a single numeric value as a dial filling from 0 to `maxValue` — ideal for utilization, progress toward a quota, or health scores.

In a setup file, this widget's `type` is `gauge` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/gauge-chart.json` for use with a JSON Schema validator:

```json
{
  "required": [
    "maxValue",
    "value"
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
      "additionalProperties": false,
      "nullable": true
    },
    "value": {
      "required": [
        "value"
      ],
      "type": "object",
      "properties": {
        "value": {
          "type": "number",
          "format": "double"
        },
        "prefix": {
          "type": "string",
          "nullable": true
        },
        "postfix": {
          "type": "string",
          "nullable": true
        },
        "note": {
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
        }
      },
      "additionalProperties": false
    },
    "maxValue": {
      "type": "number",
      "format": "double"
    }
  },
  "additionalProperties": false
}
```

## Example response

```json
{
  "title": "Server Load",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/gauge-details"
    }
  ],
  "data": {
    "value": {
      "value": 75
    },
    "maxValue": 100
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/basic-auth/gaugechart' \
-u "test:test"
```

## Header — headline, subtitle, and colored badge

The `header` block is **optional**, and this widget reads fine without one — several of the bundled examples omit it. Include it when there is a headline worth showing above the widget. Use the three parts together:

- `title` — the headline number or aggregate of the series (e.g. `"1,010"` total).
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

### Percentage with unit postfix

A health/SLA percentage where the value carries a unit and the header restates it.

```json
{
  "title": "Uptime",
  "actions": [
    {
      "title": "View Status",
      "type": "link",
      "url": "https://example.com/status"
    }
  ],
  "data": {
    "header": {
      "title": "99.9%",
      "subtitle": "Last 30 days"
    },
    "value": {
      "value": 99.9,
      "postfix": "%"
    },
    "maxValue": 100
  }
}
```

### Progress toward a quota

Progress toward a goal where the value can sit anywhere from 0 to the target.

```json
{
  "title": "Quarterly target",
  "actions": [
    {
      "title": "View Goals",
      "type": "link",
      "url": "https://example.com/goals"
    }
  ],
  "data": {
    "header": {
      "title": "62 / 100",
      "subtitle": "Q2 new customers"
    },
    "value": {
      "value": 62
    },
    "maxValue": 100
  }
}
```

### Utilisation with a warning alert

A utilisation dial nearing its ceiling — raise a warning alert before it maxes out.

```json
{
  "title": "Server Load",
  "actions": [
    {
      "title": "View Metrics",
      "type": "link",
      "url": "https://example.com/metrics"
    }
  ],
  "data": {
    "header": {
      "title": "87%",
      "subtitle": "Current"
    },
    "value": {
      "value": 87,
      "postfix": "%"
    },
    "maxValue": 100
  },
  "alert": {
    "active": true,
    "level": "warning",
    "message": "Server load above 80%"
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/gauge-chart.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown`
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Returning `value` outside `[0, maxValue]` — Dashboardbase will reject the response. Clamp upstream.
- Setting `maxValue` to `0` — must be strictly greater than `0`.
- Forgetting the `prefix`/`postfix` on `value` (e.g. `$` or `%`) — these go on `WidgetDataValue`, not on the header.
- Returning `204` when the measured quantity is zero — send `value: 0` against the real `maxValue` instead. An empty dial reads as broken; a dial sitting at zero reads as "nothing yet". Reserve `204` for when you genuinely cannot measure, and never signal it with `maxValue: 0` (that is rejected).
