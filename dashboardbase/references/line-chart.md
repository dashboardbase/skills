# LineChart widget

> **Load this file when:** building a line chart / time series / trend over time.

## Purpose

Render one or more series of values across a shared label axis as connected lines — ideal for time-series data (traffic, revenue, latency over time).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract):

```json
{
  "required": [
    "datasets"
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
    "labels": {
      "type": "array",
      "items": {
        "type": "string"
      },
      "nullable": true
    },
    "datasets": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
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
            "nullable": true
          },
          "label": {
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
      }
    },
    "fill": {
      "type": "boolean",
      "nullable": true
    },
    "ticksX": {
      "type": "boolean",
      "nullable": true
    },
    "ticksY": {
      "type": "boolean",
      "nullable": true
    }
  },
  "additionalProperties": false
}
```

## Example response

```json
{
  "title": "Website Traffic",
  "actions": [
    {
      "title": "Analyze Traffic",
      "type": "link",
      "url": "https://example.com/traffic-analysis"
    }
  ],
  "data": {
    "labels": [
      "Mon",
      "Tue",
      "Wed",
      "Thu",
      "Fri",
      "Sat",
      "Sun"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 100
          },
          {
            "value": 120
          },
          {
            "value": 150
          },
          {
            "value": 130
          },
          {
            "value": 160
          },
          {
            "value": 180
          },
          {
            "value": 170
          }
        ],
        "label": "Page Views"
      }
    ]
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/api-key/linechart' \
-H "x-api-key: test"
```

## Multiple series

The canonical example shows a single dataset because colors and multi-series are optional. To draw multiple lines, pass several entries in `datasets`; each has its own `label` and an optional `color`. `labels` is shared across all datasets. Set `fill: true` to fill the area beneath each line.

```json
{
  "labels": ["Mon", "Tue"],
  "datasets": [
    { "label": "Page Views", "color": "Blue", "data": [{"value": 100}, {"value": 120}] },
    { "label": "Unique Visitors", "color": "Green", "data": [{"value": 40}, {"value": 55}] }
  ]
}
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

- Treating `labels` as datetime stamps — Dashboardbase renders them as opaque strings; format them upstream (e.g. `'2026-05-19'` or `'Mon'`).
- Sending uneven dataset lengths — every dataset's `data` length should match `labels`.
- Returning `null` between values to indicate a gap — Dashboardbase expects every index to have a `{ "value": <number> }` entry.
