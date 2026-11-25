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
                "ArrowDown",
                "Star"
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
                "Dark",
                "Gray",
                "Gray1",
                "Gray3",
                "Gray4"
              ],
              "type": "string",
              "nullable": true
            },
            "fill": {
              "enum": [
                "Solid",
                "Clear",
                "Outline",
                "Dot"
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
            "Dark",
            "Gray",
            "Gray1",
            "Gray3",
            "Gray4"
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
        },
        "unit": {
          "maxLength": 24,
          "type": "string",
          "nullable": true
        },
        "label": {
          "maxLength": 40,
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
                    "Dark",
                    "Gray",
                    "Gray1",
                    "Gray3",
                    "Gray4"
                  ],
                  "type": "string",
                  "nullable": true
                },
                "style": {
                  "enum": [
                    "Solid",
                    "Striped",
                    "Outline"
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
                "Dark",
                "Gray",
                "Gray1",
                "Gray3",
                "Gray4"
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
                  "ArrowDown",
                  "Star"
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
                  "Dark",
                  "Gray",
                  "Gray1",
                  "Gray3",
                  "Gray4"
                ],
                "type": "string",
                "nullable": true
              },
              "fill": {
                "enum": [
                  "Solid",
                  "Clear",
                  "Outline",
                  "Dot"
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
              "Dark",
              "Gray",
              "Gray1",
              "Gray3",
              "Gray4"
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
          },
          "unit": {
            "maxLength": 24,
            "type": "string",
            "nullable": true
          },
          "label": {
            "maxLength": 40,
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

### Cost split by model

Spend or usage split across a few named things — models, vendors, plans — with the share of each.

```json
{
  "title": "Cost Breakdown",
  "actions": [
    {
      "title": "Billing",
      "type": "link",
      "url": "https://example.com/billing"
    }
  ],
  "data": {
    "header": {
      "title": "$97.61",
      "subtitle": "Share of total cost"
    },
    "labels": [
      "GPT-4",
      "Claude 3 Opus",
      "Gemini"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 51.96,
            "prefix": "$"
          },
          {
            "value": 27.68,
            "prefix": "$"
          },
          {
            "value": 17.97,
            "prefix": "$"
          }
        ],
        "label": "Cost",
        "color": [
          "Light",
          "Gray",
          "Dark"
        ]
      }
    ]
  }
}
```

### Share on a gradient card

A share-of-total on a glowing card for the one breakdown a board is about.

```json
{
  "title": "Devices",
  "actions": [
    {
      "title": "Devices",
      "type": "link",
      "url": "https://example.com/devices"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Today's sessions"
    },
    "labels": [
      "Desktop",
      "Mobile",
      "Tablet"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 58,
            "postfix": "%"
          },
          {
            "value": 34,
            "postfix": "%"
          },
          {
            "value": 8,
            "postfix": "%"
          }
        ],
        "label": "Sessions",
        "color": [
          "Light",
          "Gray",
          "Dark"
        ]
      }
    ]
  },
  "color": {
    "name": "Red",
    "style": "Gradient"
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
        "label": "Sessions"
      }
    ]
  }
}
```

### More examples

To see one in full, add `?variant=<id>` to the example request above — it returns that example's complete response.

- `pie-contrast` — **Share on a contrast card.** A share-of-total lifted out of the board on a contrast card.
- `pie-sample-alert` — **Share of total with an info alert.** A breakdown drawn from a sample or estimate — add an info alert so viewers read it with the right caveat.
- `pie-multi-header` — **Breakdown with a header strip.** A share-of-total chart that should also state the total and the leading slice, without a second widget.

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/pie-chart.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown` for a change, `Star` for a rating
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Confusing `color` as a single value — for Pie/Donut it is `List<WidgetDataColor>` (one per slice), not a single color.
- Forgetting that slices sum visually — passing negative values produces undefined rendering.
- Mismatched lengths between `labels`, `data`, and `color` arrays.
- Sending `datasets: []` when there is nothing to break down — it is rejected. Do not reach for `204` either: an empty body cannot be parsed and renders the widget in an error state. Return `200` with a single placeholder slice (e.g. label `"No data"`, value `0`) so the widget says so plainly.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
