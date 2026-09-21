# BarChart widget

> **Load this file when:** building a bar chart / grouped bars / comparison by category.

## Purpose

Render one or more series of vertical bars across a shared label axis — ideal for 'X by Y' comparisons (monthly sales, items per category, multi-region comparison).

In a setup file, this widget's `type` is `bar` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/bar-chart.json` for use with a JSON Schema validator:

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
            },
            "nullable": true
          },
          "label": {
            "minLength": 1,
            "type": "string"
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
    "ticksX": {
      "type": "boolean",
      "nullable": true
    },
    "ticksY": {
      "type": "boolean",
      "nullable": true
    },
    "indexAxis": {
      "enum": [
        "x",
        "y"
      ],
      "type": "string",
      "nullable": true
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
  "title": "Monthly Sales",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/barchart-details"
    }
  ],
  "data": {
    "header": {
      "title": "644",
      "subtitle": "Last 6 months",
      "badge": {
        "text": "-42%",
        "icon": "ArrowDown",
        "color": "Danger"
      }
    },
    "labels": [
      "Jan",
      "Feb",
      "Mar",
      "Apr",
      "May",
      "Jun"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 65
          },
          {
            "value": 59
          },
          {
            "value": 80
          },
          {
            "value": 81
          },
          {
            "value": 56
          },
          {
            "value": 55
          }
        ],
        "label": "Series A"
      },
      {
        "data": [
          {
            "value": 28
          },
          {
            "value": 48
          },
          {
            "value": 40
          },
          {
            "value": 19
          },
          {
            "value": 86
          },
          {
            "value": 27
          }
        ],
        "label": "Series B"
      }
    ]
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/basic-auth/barchart' \
-u "test:test"
```

## Header — headline, subtitle, and colored badge

The `header` block is optional in the schema — **always include it anyway**. Without it the chart is a bare plot with no headline and no period; the header is what makes it read at a glance. Use the three parts together:

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

The canonical example shows two datasets ("Series A" and "Series B") with no explicit colors — colors are optional and the default palette renders well. To color a series explicitly, set `color` on the dataset; to recolor an individual bar, set `color` on a single `WidgetDataValue` inside `data`. The `labels` array is shared and should match the length of each dataset's `data` array.

```json
{
  "labels": ["Jan", "Feb"],
  "datasets": [
    { "label": "Series A", "color": "Blue", "data": [{"value": 65}, {"value": 59}] },
    { "label": "Series B", "color": "Green", "data": [{"value": 28}, {"value": 48}] }
  ]
}
```

## Variations

Other shapes and styling for this widget — pick the one closest to your data:

### Single series breakdown

A single metric broken down by category (revenue per category, signups per plan).

```json
{
  "title": "Revenue by category",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/revenue-by-category"
    }
  ],
  "data": {
    "header": {
      "title": "$48,200",
      "subtitle": "Last 30 days",
      "badge": {
        "text": "+$3,100",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "Electronics",
      "Clothing",
      "Home",
      "Books",
      "Toys"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 18200
          },
          {
            "value": 12400
          },
          {
            "value": 9100
          },
          {
            "value": 5300
          },
          {
            "value": 3200
          }
        ],
        "label": "Revenue"
      }
    ]
  }
}
```

### Segment split

Comparing two segments of the same total side by side (new vs returning, mobile vs desktop).

```json
{
  "title": "New vs returning",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/new-vs-returning"
    }
  ],
  "data": {
    "header": {
      "title": "398",
      "subtitle": "Visitors Mon–Fri, 64% new"
    },
    "labels": [
      "Mon",
      "Tue",
      "Wed",
      "Thu",
      "Fri"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 40
          },
          {
            "value": 52
          },
          {
            "value": 48
          },
          {
            "value": 61
          },
          {
            "value": 55
          }
        ],
        "label": "New"
      },
      {
        "data": [
          {
            "value": 22
          },
          {
            "value": 28
          },
          {
            "value": 31
          },
          {
            "value": 26
          },
          {
            "value": 35
          }
        ],
        "label": "Returning"
      }
    ]
  }
}
```

### Comparison with a success alert

A breakdown where every category beat its goal — celebrate it with a success alert.

```json
{
  "title": "Revenue by category",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/revenue-by-category"
    }
  ],
  "data": {
    "header": {
      "title": "$52,800",
      "subtitle": "Last 30 days",
      "badge": {
        "text": "+$7,700",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "Electronics",
      "Clothing",
      "Home",
      "Books",
      "Toys"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 19800
          },
          {
            "value": 13100
          },
          {
            "value": 9600
          },
          {
            "value": 6300
          },
          {
            "value": 4000
          }
        ],
        "label": "Revenue"
      }
    ]
  },
  "alert": {
    "active": true,
    "level": "success",
    "message": "Every category beat last month's revenue"
  }
}
```

### Horizontal bars (indexAxis: y)

Long category labels or a ranking that reads better horizontally — set `indexAxis` to `y`.

```json
{
  "title": "Top referrers",
  "data": {
    "header": {
      "title": "Top referrers",
      "subtitle": "Last 30 days"
    },
    "labels": [
      "Google",
      "Direct",
      "Twitter / X",
      "Newsletter",
      "Product Hunt"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 4200
          },
          {
            "value": 3100
          },
          {
            "value": 1800
          },
          {
            "value": 1200
          },
          {
            "value": 640
          }
        ],
        "label": "Visits"
      }
    ],
    "indexAxis": "y"
  }
}
```

### Comparison with a full header strip

A wide chart that should also carry the board's summary row — up to six headline numbers above one plot.

```json
{
  "title": "Revenue overview",
  "actions": [
    {
      "title": "Open report",
      "type": "link",
      "url": "https://example.com/revenue"
    }
  ],
  "data": {
    "header": {
      "title": "$48.2k",
      "subtitle": "MRR",
      "badge": {
        "text": "+8.1%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "Jan",
      "Feb",
      "Mar",
      "Apr",
      "May",
      "Jun"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 31200
          },
          {
            "value": 34100
          },
          {
            "value": 37800
          },
          {
            "value": 41500
          },
          {
            "value": 44900
          },
          {
            "value": 48200
          }
        ],
        "label": "MRR"
      }
    ],
    "ticksX": true,
    "headers": [
      {
        "title": "$3.1k",
        "subtitle": "Net new",
        "badge": {
          "text": "+12%",
          "icon": "ArrowUp",
          "color": "Success"
        }
      },
      {
        "title": "1,284",
        "subtitle": "Customers"
      },
      {
        "title": "$37.5",
        "subtitle": "ARPU",
        "badge": {
          "text": "-1.4%",
          "icon": "ArrowDown",
          "color": "Danger"
        }
      },
      {
        "title": "2.1%",
        "subtitle": "Churn",
        "badge": {
          "text": "-0.3pp",
          "icon": "ArrowDown",
          "color": "Success"
        }
      },
      {
        "title": "$578k",
        "subtitle": "ARR"
      }
    ]
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/bar-chart.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown`
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Mismatched lengths: if `labels` has 6 entries, every dataset's `data` should have 6 entries.
- Sending `value` as a string (`"65"`) — values must be numbers.
- Setting `color` inside `data` items when you meant the whole series — series color goes on the dataset.
- Wanting horizontal bars? Set `indexAxis` to `y` — do not look for a separate horizontal-bar widget type.
- Returning `204` (or an empty series) for a period with no activity — emit every bucket in the window with `{ "value": 0 }` instead. Flat zero bars are the honest answer and read as intentional; a blank widget reads as broken.
- Leaving axis ticks on for a long daily series — thirty rotated date labels crowd the axis and swamp the plot. Set `ticksX: false` past roughly a dozen buckets and let `header.subtitle` carry the window ("Last 30 days").
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
