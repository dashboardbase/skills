# ProgressList widget

> **Load this file when:** building a list of labelled progress / goal / capacity bars.

## Purpose

Render a stacked list of labelled progress bars — ideal for storage per service, goals per team, or usage per plan. Each row is the same 'progress toward a cap' shape as the optional KPI progress bar, just repeated.

In a setup file, this widget's `type` is `progress` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/progress-list.json` for use with a JSON Schema validator:

```json
{
  "required": [
    "items"
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
    "items": {
      "minItems": 1,
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
          "max": {
            "type": "number",
            "format": "double",
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
              "Dark",
              "Gray",
              "Gray1",
              "Gray3",
              "Gray4"
            ],
            "type": "string",
            "nullable": true
          },
          "shape": {
            "enum": [
              "Bar",
              "Ring",
              "Meter"
            ],
            "type": "string",
            "nullable": true
          },
          "display": {
            "maxLength": 24,
            "type": "string",
            "nullable": true
          },
          "marker": {
            "required": [
              "value"
            ],
            "type": "object",
            "properties": {
              "value": {
                "type": "number",
                "format": "double"
              },
              "label": {
                "maxLength": 32,
                "type": "string",
                "nullable": true
              }
            },
            "additionalProperties": false,
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "x-placeholder": true
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
  "title": "Storage",
  "actions": [
    {
      "title": "Manage",
      "type": "link",
      "url": "https://example.com/storage"
    }
  ],
  "data": {
    "header": {
      "title": "Storage by service",
      "subtitle": "Last synced 5m ago"
    },
    "items": [
      {
        "value": 820,
        "max": 1000,
        "label": "Postgres"
      },
      {
        "value": 340,
        "max": 500,
        "label": "Blob"
      },
      {
        "value": 470,
        "max": 500,
        "label": "Redis"
      }
    ]
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/progresslist'
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





## Variations

Other shapes and styling for this widget — pick the one closest to your data:

### Ratings by star

A rating distribution — five stars down to one — each bar against the most common rating with its count beside it, the average and total above.

```json
{
  "title": "Customer Reviews",
  "actions": [
    {
      "title": "View all",
      "type": "link",
      "url": "https://example.com/reviews"
    }
  ],
  "data": {
    "header": {
      "title": "4.5",
      "subtitle": "Based on 7,875 verified purchases",
      "badge": {
        "text": "★★★★★",
        "color": "Yellow",
        "fill": "Clear"
      },
      "unit": "out of 5"
    },
    "items": [
      {
        "value": 4000,
        "max": 4000,
        "label": "5 stars",
        "color": "Light",
        "display": "4,000"
      },
      {
        "value": 2100,
        "max": 4000,
        "label": "4 stars",
        "color": "Light",
        "display": "2,100"
      },
      {
        "value": 800,
        "max": 4000,
        "label": "3 stars",
        "color": "Light",
        "display": "800"
      },
      {
        "value": 631,
        "max": 4000,
        "label": "2 stars",
        "color": "Light",
        "display": "631"
      },
      {
        "value": 344,
        "max": 4000,
        "label": "1 star",
        "color": "Light",
        "display": "344"
      }
    ]
  }
}
```

### Usage against a benchmark

One person or team against a benchmark — each bar their share, a tick where the benchmark sits, and both figures under it.

```json
{
  "title": "Model usage comparison",
  "actions": [
    {
      "title": "Usage",
      "type": "link",
      "url": "https://example.com/usage"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Sarah vs team benchmark"
    },
    "items": [
      {
        "value": 45,
        "label": "GPT-4o",
        "color": "Light",
        "display": "Sarah 45%",
        "marker": {
          "value": 75,
          "label": "Benchmark 75%"
        }
      },
      {
        "value": 37,
        "label": "Deep research",
        "color": "Light",
        "display": "Sarah 37%",
        "marker": {
          "value": 55,
          "label": "Benchmark 55%"
        }
      },
      {
        "value": 62,
        "label": "Claude",
        "color": "Light",
        "display": "Sarah 62%",
        "marker": {
          "value": 48,
          "label": "Benchmark 48%"
        }
      }
    ]
  }
}
```

### Totals by year

A few periods compared as bars against the best one — projects per year, revenue per quarter.

```json
{
  "title": "Achievement by Year",
  "actions": [
    {
      "title": "Reports",
      "type": "link",
      "url": "https://example.com/reports"
    }
  ],
  "data": {
    "header": {
      "title": "57",
      "subtitle": "in 2024",
      "unit": "projects"
    },
    "items": [
      {
        "value": 57,
        "max": 57,
        "label": "2024",
        "color": "Light",
        "display": "57 projects"
      },
      {
        "value": 29,
        "max": 57,
        "label": "2023",
        "color": "Gray",
        "display": "29 projects"
      },
      {
        "value": 35,
        "max": 57,
        "label": "2022",
        "color": "Gray",
        "display": "35 projects"
      },
      {
        "value": 41,
        "max": 57,
        "label": "2021",
        "color": "Gray",
        "display": "41 projects"
      }
    ]
  }
}
```

### Goals on a gradient card

Team or quarterly goals that are the board's focus, on a gradient card.

```json
{
  "title": "Quarter goals",
  "actions": [
    {
      "title": "Goals",
      "type": "link",
      "url": "https://example.com/goals"
    }
  ],
  "data": {
    "items": [
      {
        "value": 82,
        "label": "New revenue",
        "color": "Light"
      },
      {
        "value": 64,
        "label": "Expansion",
        "color": "Light"
      },
      {
        "value": 41,
        "label": "Partnerships",
        "color": "Light"
      }
    ]
  },
  "color": {
    "name": "Orange",
    "style": "Gradient"
  }
}
```

### Goals per team

Progress toward a set of goals or quotas, one bar per owner.

```json
{
  "title": "Quarterly goals",
  "data": {
    "items": [
      {
        "value": 82,
        "max": 100,
        "label": "Sales"
      },
      {
        "value": 45,
        "max": 100,
        "label": "Marketing"
      },
      {
        "value": 63,
        "max": 100,
        "label": "Support"
      }
    ]
  }
}
```

### More examples

To see one in full, add `?variant=<id>` to the example request above — it returns that example's complete response.

- `progress-contrast` — **Bars on a contrast card.** A short list of progress bars lifted out of the board on a contrast card.
- `progress-multi-header` — **Bars under a header strip.** A list of capacity bars that should be read against the aggregate — total used and remaining above the rows.

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/progress-list.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown` for a change, `Star` for a rating
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Leaving a row's `label` empty — every bar in the list must be labelled.
- Formatting the count into the label (`"5 stars (4,000)"`) when the row should show it instead of a percentage — send `display: "4,000"` and it replaces the percentage on the right.
- Drawing a benchmark as a second row — send `marker: { "value": 75, "label": "Benchmark 75%" }` on the row and it is drawn as a tick across the bar, with its label under it.
- Sending `value` greater than `max`, or a `max` of `0` — when `max` is set it must be `> 0` and `0 <= value <= max`.
- Using this for a single bar — a single goal bar belongs on the KPI widget's `progress` field.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
