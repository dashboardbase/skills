# GaugeChart widget

> **Load this file when:** building a gauge / dial / progress indicator.

## Purpose

Show a single numeric value as a dial filling from 0 to `maxValue` — ideal for utilization, progress toward a quota, or health scores.

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract):

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
          "format": "int32",
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

- Returning `value` outside `[0, maxValue]` — Dashboardbase will reject the response. Clamp upstream.
- Setting `maxValue` to `0` — must be strictly greater than `0`.
- Forgetting the `prefix`/`postfix` on `value` (e.g. `$` or `%`) — these go on `WidgetDataValue`, not on the header.
