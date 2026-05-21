# DonutChart widget

> **Load this file when:** building a donut chart / ring breakdown.

## Purpose

Same as PieChart but rendered with a hollow centre — useful when you want a 'ring' visual style for a category breakdown.

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
  "title": "Product Sales Distribution",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/donut-details"
    }
  ],
  "data": {
    "labels": [
      "Electronics",
      "Clothing",
      "Home Goods",
      "Books"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 300
          },
          {
            "value": 50
          },
          {
            "value": 100
          },
          {
            "value": 75
          }
        ],
        "label": "Sales"
      }
    ]
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/basic-auth/donutchart' \
-u "test:test"
```

## Multiple series

The canonical example omits `color` — the default palette renders well without one. To color each slice explicitly, pass `color` as a **list** of `WidgetDataColor` values, one per slice, matching the order of `data`.

```json
{
  "labels": ["Electronics", "Clothing", "Home Goods", "Books"],
  "datasets": [
    {
      "label": "Sales",
      "data": [{"value": 300}, {"value": 50}, {"value": 100}, {"value": 75}],
      "color": ["Blue", "Green", "Orange", "Yellow"]
    }
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

- Treating `color` as a single value — for Donut it is a list of `WidgetDataColor`, one per slice.
- Negative values produce undefined rendering — pass only non-negative numbers.
- Mismatched lengths between `labels`, `data`, and `color` arrays.
