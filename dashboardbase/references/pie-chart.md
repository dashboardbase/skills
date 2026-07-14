# PieChart widget

> **Load this file when:** building a pie chart / share-of-total / breakdown.

## Purpose

Render a circular share-of-total visualization where each slice is one entry in the dataset — ideal for market share, traffic source breakdown, category split.

In a setup file, this widget's `type` is `pie` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/pie-chart.json` for use with a JSON Schema validator:

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
    "labels": {
      "type": "array",
      "items": {
        "type": "string"
      },
      "nullable": true
    },
    "datasets": {
      "minItems": 1,
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
              "type": "string"
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

## Header — headline, subtitle, and colored badge

The `header` block is optional in the schema — **include it anyway**. Without it the widget renders as a bare plot; the header is what makes it read well at a glance. Use the three parts together:

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

## Variations

Other shapes and styling for this widget — pick the one closest to your data:

### Explicitly coloured slices

You want each slice in a specific colour rather than the default palette.

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
        "label": "Share",
        "color": [
          "Blue",
          "Green",
          "Yellow"
        ]
      }
    ]
  }
}
```

### Platform breakdown

Distribution of users/sessions across platforms or segments.

```json
{
  "title": "OS usage",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/os-usage"
    }
  ],
  "data": {
    "labels": [
      "iOS",
      "Android",
      "Web"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 58
          },
          {
            "value": 34
          },
          {
            "value": 8
          }
        ],
        "label": "Sessions",
        "color": [
          "Blue",
          "Green",
          "Orange"
        ]
      }
    ]
  }
}
```

### Share of total with an info alert

A breakdown drawn from a sample or estimate — add an info alert so viewers read it with the right caveat.

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
        "label": "Share",
        "color": [
          "Blue",
          "Green",
          "Yellow"
        ]
      }
    ]
  },
  "alert": {
    "active": true,
    "level": "info",
    "message": "Shares are based on a 7-day sample"
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate your response's `data` field against `assets/schemas/pie-chart.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

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
