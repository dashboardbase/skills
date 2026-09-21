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
      "minItems": 1,
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
    },
    "headers": {
      "maxItems": 6,
      "type": "array",
      "items": {
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
      },
      "nullable": true
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
    "header": {
      "title": "$52,500",
      "subtitle": "Sales, last 30 days"
    },
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

### More than one header — the header strip

`headers` takes an array of the same block, so one endpoint can carry several headline numbers
above the widget instead of needing a separate KPI widget for each. `header` is merged in **first**,
so `header` plus `headers` is one list: send only `header`, only `headers`, or both.

The strip lays its blocks out in **equal columns**, and `header` plus `headers` may total at most
**6** — one per pair of grid columns. A seventh is a validation error, not a silent truncation. On a
phone the strip wraps to two per row and the widget grows taller to fit, so a six-block strip stays
readable there too.

```json
{
  "header": { "title": "395", "subtitle": "Visitors", "badge": { "text": "+348.9%", "icon": "ArrowUp", "color": "Success" } },
  "headers": [
    { "title": "932", "subtitle": "New visitors", "badge": { "text": "+565.7%", "icon": "ArrowUp", "color": "Success" } },
    { "title": "1m 50s", "subtitle": "Avg. engagement" },
    { "title": "150K", "subtitle": "Total visitors" }
  ]
}
```

Every block in `headers` needs a `title` — an entry without one renders as an empty column, so it is
rejected.

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
        "label": "Customers"
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
        "label": "Customers"
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

### Ring with a header strip

A category ring that should also carry the total and the top category as headline numbers.

```json
{
  "title": "Sales by category",
  "actions": [
    {
      "title": "Open catalog",
      "type": "link",
      "url": "https://example.com/catalog"
    }
  ],
  "data": {
    "header": {
      "title": "$52,500",
      "subtitle": "Sales",
      "badge": {
        "text": "+6.2%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
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
            "value": 30000
          },
          {
            "value": 5000
          },
          {
            "value": 10000
          },
          {
            "value": 7500
          }
        ],
        "label": "Sales"
      }
    ],
    "headers": [
      {
        "title": "Electronics",
        "subtitle": "Top category"
      },
      {
        "title": "1,942",
        "subtitle": "Orders",
        "badge": {
          "text": "+3.1%",
          "icon": "ArrowUp",
          "color": "Success"
        }
      }
    ]
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/donut-chart.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

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
- Sending `datasets: []` when there is nothing to break down — it is rejected. Do not reach for `204` either: an empty body cannot be parsed and renders the widget in an error state. Return `200` with a single placeholder slice (e.g. label `"No data"`, value `0`) so the widget says so plainly.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
