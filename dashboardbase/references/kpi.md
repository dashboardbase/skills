# KPI widget

> **Load this file when:** building a single-number / headline / KPI tile.

## Purpose

Show a single headline number with optional subtitle and trend badge — ideal for MRR, active users, conversion rate, error count, anything you'd put in a 'big number' tile.

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract):

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
          "format": "int32",
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
              "format": "int32",
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
              "format": "int32",
              "nullable": true
            },
            "fill": {
              "enum": [
                "Solid",
                "Clear",
                "Outline"
              ],
              "type": "string",
              "format": "int32",
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
          "format": "int32",
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
          "format": "int32",
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



## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. JSON-validate your response against the resolved schema before declaring done.

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
