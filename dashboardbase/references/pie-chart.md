# PieChart widget

> **Load this file when:** building a pie chart / share-of-total / breakdown.

## Purpose

Render a circular share-of-total visualization where each slice is one entry in the dataset — ideal for market share, traffic source breakdown, category split.

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
        "required": [
          "data",
          "label"
        ],
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
            }
          },
          "label": {
            "minLength": 1,
            "type": "string"
          },
          "color": {
            "type": "array",
            "items": {
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
              "format": "int32"
            },
            "nullable": true
          }
        },
        "additionalProperties": false
      }
    }
  },
  "additionalProperties": false
}
```

## Example response

```json
{
  "title": "Market Share",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/piechart-details"
    }
  ],
  "data": {
    "labels": [
      "Product A",
      "Product B",
      "Product C"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 30
          },
          {
            "value": 50
          },
          {
            "value": 20
          }
        ],
        "label": "Share"
      }
    ]
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/api-key/piechart' \
-H "x-api-key: test"
```

## Multiple series

The canonical example omits `color` because the default palette renders well without one — colors are optional. To color each slice explicitly, pass `color` as a **list** of `WidgetDataColor` values, one per slice, matching the order of `data`.

```json
{
  "labels": ["Product A", "Product B", "Product C"],
  "datasets": [
    {
      "label": "Share",
      "data": [{"value": 30}, {"value": 50}, {"value": 20}],
      "color": ["Blue", "Green", "Yellow"]
    }
  ]
}
```

Pie charts typically use a single dataset; `datasets` is still an array for shape consistency with Bar/Line.

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

- Confusing `color` as a single value — for Pie/Donut it is `List<WidgetDataColor>` (one per slice), not a single color.
- Forgetting that slices sum visually — passing negative values produces undefined rendering.
- Mismatched lengths between `labels`, `data`, and `color` arrays.
