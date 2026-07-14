# DonutChart widget

> **Load this file when:** building a donut chart / ring breakdown.

## Purpose

Same as PieChart but rendered with a hollow centre — useful when you want a 'ring' visual style for a category breakdown.

In a setup file, this widget's `type` is `donut` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/donut-chart.json` for use with a JSON Schema validator:

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

## Variations

Other shapes and styling for this widget — pick the one closest to your data:

### Explicitly coloured ring

You want each ring segment in a specific colour rather than the default palette.

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
        "label": "Sales",
        "color": [
          "Blue",
          "Green",
          "Orange",
          "Yellow"
        ]
      }
    ]
  }
}
```

### Subscription plan mix

Share of customers (or revenue) across subscription tiers.

```json
{
  "title": "Plan mix",
  "actions": [
    {
      "title": "View Plans",
      "type": "link",
      "url": "https://example.com/plans"
    }
  ],
  "data": {
    "labels": [
      "Free",
      "Pro",
      "Enterprise"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 420
          },
          {
            "value": 180
          },
          {
            "value": 36
          }
        ],
        "label": "Customers",
        "color": [
          "Light",
          "Blue",
          "Dark"
        ]
      }
    ]
  }
}
```

### Ring breakdown with a success alert

A tier breakdown where a target was hit — surface a success alert above the ring.

```json
{
  "title": "Plan mix",
  "actions": [
    {
      "title": "View Plans",
      "type": "link",
      "url": "https://example.com/plans"
    }
  ],
  "data": {
    "labels": [
      "Free",
      "Pro",
      "Enterprise"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 380
          },
          {
            "value": 240
          },
          {
            "value": 48
          }
        ],
        "label": "Customers",
        "color": [
          "Light",
          "Blue",
          "Dark"
        ]
      }
    ]
  },
  "alert": {
    "active": true,
    "level": "success",
    "message": "Pro plan adoption hit the quarterly goal"
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate your response's `data` field against `assets/schemas/donut-chart.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

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
