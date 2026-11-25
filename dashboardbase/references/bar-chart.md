# BarChart widget

> **Load this file when:** building a bar chart / grouped or stacked bars / comparison by category / bars with a rate line (combo chart).

## Purpose

Render one or more series of vertical bars across a shared label axis — ideal for 'X by Y' comparisons (monthly sales, items per category, multi-region comparison). Bars can be stacked to show a total and what it is made of, and a series can be drawn as a line over the bars (on its own right-hand axis) to read a rate against the volume beneath it.

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
            },
            "nullable": true,
            "x-placeholder": true
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
              "Dark",
              "Gray",
              "Gray1",
              "Gray3",
              "Gray4"
            ],
            "type": "string",
            "nullable": true
          },
          "type": {
            "enum": [
              "Bar",
              "Line"
            ],
            "type": "string",
            "nullable": true
          },
          "axis": {
            "enum": [
              "Left",
              "Right"
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
    "ticksX": {
      "title": "Category labels",
      "type": "boolean",
      "nullable": true
    },
    "ticksY": {
      "title": "Value labels",
      "type": "boolean",
      "nullable": true
    },
    "indexAxis": {
      "enum": [
        "x",
        "y"
      ],
      "type": "string",
      "title": "Bar direction",
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
    },
    "grid": {
      "title": "Gridlines",
      "type": "boolean",
      "default": true,
      "nullable": true
    },
    "target": {
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
          "maxLength": 40,
          "type": "string",
          "nullable": true
        }
      },
      "additionalProperties": false,
      "nullable": true
    },
    "stacked": {
      "title": "Stacked bars",
      "type": "boolean",
      "default": false,
      "nullable": true
    },
    "barWidth": {
      "enum": [
        "Regular",
        "Slim"
      ],
      "type": "string",
      "title": "Bar width",
      "default": 0,
      "nullable": true
    },
    "barShape": {
      "enum": [
        "Rounded",
        "Pill"
      ],
      "type": "string",
      "title": "Bar ends",
      "default": 0,
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
        "label": "Online"
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
        "label": "In store"
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

The canonical example shows two datasets ("Online" and "In store") with no explicit colors — colors are optional and the default palette renders well. To color a series explicitly, set `color` on the dataset; to recolor an individual bar, set `color` on a single `WidgetDataValue` inside `data`. The `labels` array is shared and should match the length of each dataset's `data` array.

```json
{
  "labels": ["Jan", "Feb"],
  "datasets": [
    { "label": "Online", "color": "Blue", "data": [{"value": 65}, {"value": 59}] },
    { "label": "In store", "color": "Green", "data": [{"value": 28}, {"value": 48}] }
  ]
}
```

**Mark a goal or quota** with `target`: a labelled line across the bars at that value (a vertical line when `indexAxis` is `y`), which the value axis always stretches to include.

```json
{
  "labels": ["Alice", "Bob", "Chen"],
  "datasets": [{ "label": "Deals closed", "data": [{"value": 14}, {"value": 9}, {"value": 12}] }],
  "target": { "value": 12, "label": "Quota" }
}
```

**Stack the series** with `stacked: true` when they are parts of one whole in the same unit — MRR by plan, visitors by channel, tickets by priority. The top of each column is the total, so put that total in `header.title`. Do not stack series that do not add up (rates, mixed units).

```json
{
  "stacked": true,
  "labels": ["Jul", "Aug", "Sep"],
  "datasets": [
    { "label": "Starter", "data": [{"value": 8000}, {"value": 8400}, {"value": 9200}] },
    { "label": "Team", "data": [{"value": 16000}, {"value": 18000}, {"value": 19000}] },
    { "label": "Business", "data": [{"value": 15000}, {"value": 17000}, {"value": 20000}] }
  ]
}
```

**Negative values** are drawn below a zero line — profit and loss, net cash flow, gains and losses. Colour each bar by its sign through the value's `color`. In a stacked chart a negative series stacks downward from zero on its own, so a column can carry what came in above the line and what went out below it.

```json
{
  "stacked": true,
  "barShape": "Pill",
  "labels": ["Q1", "Q2", "Q3"],
  "datasets": [
    { "label": "Inflow", "data": [{"value": 42000, "prefix": "$"}, {"value": 51000, "prefix": "$"}, {"value": 48000, "prefix": "$"}] },
    { "label": "Outflow", "data": [{"value": -18000, "prefix": "$"}, {"value": -26000, "prefix": "$"}, {"value": -21000, "prefix": "$"}] }
  ]
}
```

**Shape the bars** with `barWidth: "Slim"` (thin bars, wide gaps — a long daily series) and `barShape: "Pill"` (both ends rounded, lifted off zero — bars above and below the axis read as separate pills).

**Draw a rate over the bars** (a combo chart) by giving one dataset `type: "Line"`. When its values are on a different scale from the bars — a percentage over counts — also set `axis: "Right"`, so it gets its own scale on the right edge. A unit such as `%` goes in each value's `postfix` and is used for that axis and the tooltip. Line series are never stacked, and `target` always measures against the left axis.

```json
{
  "labels": ["W39", "W40", "W41"],
  "datasets": [
    { "label": "Signups", "data": [{"value": 250}, {"value": 290}, {"value": 320}] },
    { "label": "Conversion", "type": "Line", "axis": "Right", "data": [{"value": 3.9, "postfix": "%"}, {"value": 4.4, "postfix": "%"}, {"value": 4.7, "postfix": "%"}] }
  ]
}
```



## Variations

Other shapes and styling for this widget — pick the one closest to your data:

### Monthly bars, latest one lit

A run of months where this month is the headline — dark bars with the current one in the card's ink, the total and change above.

```json
{
  "title": "Subscriptions",
  "actions": [
    {
      "title": "Subscribers",
      "type": "link",
      "url": "https://example.com/subscribers"
    }
  ],
  "data": {
    "header": {
      "title": "+4,850",
      "subtitle": "vs last month",
      "badge": {
        "text": "+18.1%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "Apr",
      "May",
      "Jun",
      "Jul",
      "Aug",
      "Sep",
      "Oct",
      "Nov"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 3100
          },
          {
            "value": 3900
          },
          {
            "value": 2700
          },
          {
            "value": 3600
          },
          {
            "value": 2500
          },
          {
            "value": 3000
          },
          {
            "value": 3800
          },
          {
            "value": 4850,
            "color": "Light"
          }
        ],
        "label": "New subscribers",
        "color": "Gray"
      }
    ],
    "ticksY": false,
    "grid": false
  }
}
```

### Inflow over outflow as pills

Money in and money out per period on one chart — inflow rising above zero, outflow hanging below it, each bar a pill.

```json
{
  "title": "Cash flow",
  "actions": [
    {
      "title": "Statements",
      "type": "link",
      "url": "https://example.com/cash-flow"
    }
  ],
  "data": {
    "header": {
      "title": "$608,000",
      "subtitle": "Net inflow vs last year",
      "badge": {
        "text": "+87.7%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "Q1 '22",
      "Q2 '22",
      "Q3 '22",
      "Q4 '22",
      "Q1 '23",
      "Q2 '23",
      "Q3 '23",
      "Q4 '23",
      "Q1 '24",
      "Q2 '24",
      "Q3 '24"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 31000,
            "prefix": "$"
          },
          {
            "value": 38000,
            "prefix": "$"
          },
          {
            "value": 41000,
            "prefix": "$"
          },
          {
            "value": 46000,
            "prefix": "$"
          },
          {
            "value": 44000,
            "prefix": "$"
          },
          {
            "value": 55000,
            "prefix": "$"
          },
          {
            "value": 60000,
            "prefix": "$"
          },
          {
            "value": 58000,
            "prefix": "$"
          },
          {
            "value": 66000,
            "prefix": "$"
          },
          {
            "value": 70000,
            "prefix": "$"
          },
          {
            "value": 69000,
            "prefix": "$"
          }
        ],
        "label": "Inflow",
        "color": "Light"
      },
      {
        "data": [
          {
            "value": -14000,
            "prefix": "$"
          },
          {
            "value": -17000,
            "prefix": "$"
          },
          {
            "value": -15000,
            "prefix": "$"
          },
          {
            "value": -19000,
            "prefix": "$"
          },
          {
            "value": -16000,
            "prefix": "$"
          },
          {
            "value": -21000,
            "prefix": "$"
          },
          {
            "value": -18000,
            "prefix": "$"
          },
          {
            "value": -24000,
            "prefix": "$"
          },
          {
            "value": -23000,
            "prefix": "$"
          },
          {
            "value": -22000,
            "prefix": "$"
          },
          {
            "value": -20000,
            "prefix": "$"
          }
        ],
        "label": "Outflow",
        "color": "Gray3"
      }
    ],
    "ticksY": true,
    "grid": true,
    "stacked": true,
    "barShape": "Pill"
  }
}
```

### Slim daily bars with today lit

A long daily series that should read as a rhythm — thirty thin bars in grey, today in the card's ink.

```json
{
  "title": "Gross volume",
  "actions": [
    {
      "title": "Payments",
      "type": "link",
      "url": "https://example.com/payments"
    }
  ],
  "data": {
    "header": {
      "title": "$3,528,199",
      "subtitle": "$3,077,692 last month",
      "badge": {
        "text": "+14.6%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "Nov 1",
      "Nov 2",
      "Nov 3",
      "Nov 4",
      "Nov 5",
      "Nov 6",
      "Nov 7",
      "Nov 8",
      "Nov 9",
      "Nov 10",
      "Nov 11",
      "Nov 12",
      "Nov 13",
      "Nov 14",
      "Nov 15",
      "Nov 16",
      "Nov 17",
      "Nov 18",
      "Nov 19",
      "Nov 20",
      "Nov 21",
      "Nov 22",
      "Nov 23",
      "Nov 24",
      "Nov 25",
      "Nov 26",
      "Nov 27",
      "Nov 28",
      "Nov 29",
      "Nov 30"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 92000,
            "prefix": "$"
          },
          {
            "value": 118000,
            "prefix": "$"
          },
          {
            "value": 104000,
            "prefix": "$"
          },
          {
            "value": 131000,
            "prefix": "$"
          },
          {
            "value": 97000,
            "prefix": "$"
          },
          {
            "value": 88000,
            "prefix": "$"
          },
          {
            "value": 142000,
            "prefix": "$"
          },
          {
            "value": 126000,
            "prefix": "$"
          },
          {
            "value": 109000,
            "prefix": "$"
          },
          {
            "value": 95000,
            "prefix": "$"
          },
          {
            "value": 151000,
            "prefix": "$"
          },
          {
            "value": 133000,
            "prefix": "$"
          },
          {
            "value": 120000,
            "prefix": "$"
          },
          {
            "value": 101000,
            "prefix": "$"
          },
          {
            "value": 98000,
            "prefix": "$"
          },
          {
            "value": 137000,
            "prefix": "$"
          },
          {
            "value": 159000,
            "prefix": "$"
          },
          {
            "value": 128000,
            "prefix": "$"
          },
          {
            "value": 112000,
            "prefix": "$"
          },
          {
            "value": 104000,
            "prefix": "$"
          },
          {
            "value": 146000,
            "prefix": "$"
          },
          {
            "value": 162000,
            "prefix": "$"
          },
          {
            "value": 139000,
            "prefix": "$"
          },
          {
            "value": 118000,
            "prefix": "$"
          },
          {
            "value": 107000,
            "prefix": "$"
          },
          {
            "value": 152000,
            "prefix": "$"
          },
          {
            "value": 168000,
            "prefix": "$"
          },
          {
            "value": 141000,
            "prefix": "$"
          },
          {
            "value": 126000,
            "prefix": "$"
          },
          {
            "value": 173000,
            "prefix": "$",
            "color": "Light"
          }
        ],
        "label": "Gross volume",
        "color": "Gray"
      }
    ],
    "ticksY": true,
    "grid": true,
    "barWidth": "Slim"
  }
}
```

### Two series with their totals

Two channels side by side per month — desktop and mobile, plan A and plan B — with each series' total in the header.

```json
{
  "title": "Total Revenue",
  "actions": [
    {
      "title": "Revenue",
      "type": "link",
      "url": "https://example.com/revenue"
    }
  ],
  "data": {
    "header": {
      "title": "24,828",
      "subtitle": "Desktop"
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
            "value": 3200
          },
          {
            "value": 4800
          },
          {
            "value": 4600
          },
          {
            "value": 2600
          },
          {
            "value": 2300
          },
          {
            "value": 4900
          }
        ],
        "label": "Desktop",
        "color": "Light"
      },
      {
        "data": [
          {
            "value": 2900
          },
          {
            "value": 3400
          },
          {
            "value": 2100
          },
          {
            "value": 3700
          },
          {
            "value": 2700
          },
          {
            "value": 2600
          }
        ],
        "label": "Mobile",
        "color": "Gray"
      }
    ],
    "headers": [
      {
        "title": "25,010",
        "subtitle": "Mobile"
      }
    ],
    "grid": true
  }
}
```

### Profit and loss by month

A result that can go either way — net profit, cash flow, net new customers — with the losing months hanging below a zero line.

```json
{
  "title": "Net profit",
  "actions": [
    {
      "title": "P&L",
      "type": "link",
      "url": "https://example.com/profit-and-loss"
    }
  ],
  "data": {
    "header": {
      "title": "$48,600",
      "subtitle": "Net, last 12 months",
      "badge": {
        "text": "+$12.4k",
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
      "Jun",
      "Jul",
      "Aug",
      "Sep",
      "Oct",
      "Nov",
      "Dec"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 8200,
            "prefix": "$",
            "color": "Success"
          },
          {
            "value": 6400,
            "prefix": "$",
            "color": "Success"
          },
          {
            "value": -3100,
            "prefix": "$",
            "color": "Danger"
          },
          {
            "value": 4800,
            "prefix": "$",
            "color": "Success"
          },
          {
            "value": 9100,
            "prefix": "$",
            "color": "Success"
          },
          {
            "value": -5600,
            "prefix": "$",
            "color": "Danger"
          },
          {
            "value": -2200,
            "prefix": "$",
            "color": "Danger"
          },
          {
            "value": 7300,
            "prefix": "$",
            "color": "Success"
          },
          {
            "value": 10400,
            "prefix": "$",
            "color": "Success"
          },
          {
            "value": 6900,
            "prefix": "$",
            "color": "Success"
          },
          {
            "value": -4100,
            "prefix": "$",
            "color": "Danger"
          },
          {
            "value": 6400,
            "prefix": "$",
            "color": "Success"
          }
        ],
        "label": "Net profit"
      }
    ],
    "ticksY": true,
    "grid": true
  }
}
```

### Daily make-up above and below zero

A month of days split into what counts for you above the line and what counts against you below it — occupied and available rooms over rooms not ready.

```json
{
  "title": "Occupancy",
  "actions": [
    {
      "title": "Rooms",
      "type": "link",
      "url": "https://example.com/rooms"
    }
  ],
  "data": {
    "header": {
      "title": "78%",
      "subtitle": "Occupancy rate, November",
      "badge": {
        "text": "+3.2%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "01",
      "02",
      "03",
      "04",
      "05",
      "06",
      "07",
      "08",
      "09",
      "10",
      "11",
      "12",
      "13",
      "14",
      "15",
      "16",
      "17",
      "18",
      "19",
      "20",
      "21",
      "22",
      "23",
      "24",
      "25",
      "26",
      "27",
      "28",
      "29",
      "30"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 95
          },
          {
            "value": 78
          },
          {
            "value": 60
          },
          {
            "value": 75
          },
          {
            "value": 100
          },
          {
            "value": 112
          },
          {
            "value": 80
          },
          {
            "value": 92
          },
          {
            "value": 98
          },
          {
            "value": 110
          },
          {
            "value": 84
          },
          {
            "value": 108
          },
          {
            "value": 114
          },
          {
            "value": 120
          },
          {
            "value": 118
          },
          {
            "value": 88
          },
          {
            "value": 84
          },
          {
            "value": 136
          },
          {
            "value": 140
          },
          {
            "value": 132
          },
          {
            "value": 96
          },
          {
            "value": 108
          },
          {
            "value": 92
          },
          {
            "value": 102
          },
          {
            "value": 126
          },
          {
            "value": 124
          },
          {
            "value": 138
          },
          {
            "value": 128
          },
          {
            "value": 108
          },
          {
            "value": 114
          }
        ],
        "label": "Occupied",
        "color": "Light"
      },
      {
        "data": [
          {
            "value": 25
          },
          {
            "value": 30
          },
          {
            "value": 30
          },
          {
            "value": 28
          },
          {
            "value": 30
          },
          {
            "value": 25
          },
          {
            "value": 35
          },
          {
            "value": 28
          },
          {
            "value": 26
          },
          {
            "value": 20
          },
          {
            "value": 22
          },
          {
            "value": 22
          },
          {
            "value": 30
          },
          {
            "value": 30
          },
          {
            "value": 30
          },
          {
            "value": 35
          },
          {
            "value": 30
          },
          {
            "value": 24
          },
          {
            "value": 30
          },
          {
            "value": 26
          },
          {
            "value": 34
          },
          {
            "value": 34
          },
          {
            "value": 34
          },
          {
            "value": 30
          },
          {
            "value": 22
          },
          {
            "value": 24
          },
          {
            "value": 28
          },
          {
            "value": 32
          },
          {
            "value": 40
          },
          {
            "value": 30
          }
        ],
        "label": "Available",
        "color": "Gray"
      },
      {
        "data": [
          {
            "value": -30
          },
          {
            "value": -28
          },
          {
            "value": -25
          },
          {
            "value": -40
          },
          {
            "value": -32
          },
          {
            "value": -30
          },
          {
            "value": -22
          },
          {
            "value": -28
          },
          {
            "value": -26
          },
          {
            "value": -24
          },
          {
            "value": -36
          },
          {
            "value": -30
          },
          {
            "value": -32
          },
          {
            "value": -26
          },
          {
            "value": -22
          },
          {
            "value": -24
          },
          {
            "value": -18
          },
          {
            "value": -36
          },
          {
            "value": -46
          },
          {
            "value": -34
          },
          {
            "value": -30
          },
          {
            "value": -28
          },
          {
            "value": -34
          },
          {
            "value": -46
          },
          {
            "value": -24
          },
          {
            "value": -28
          },
          {
            "value": -22
          },
          {
            "value": -30
          },
          {
            "value": -48
          },
          {
            "value": -26
          }
        ],
        "label": "Not ready",
        "color": "Gray3"
      }
    ],
    "ticksY": true,
    "grid": true,
    "stacked": true,
    "barWidth": "Slim"
  }
}
```

### More examples

To see one in full, add `?variant=<id>` to the example request above — it returns that example's complete response.

- `bar-stacked-two-tone` — **Stacked bars in two tones.** A monthly total and its two parts in one bar — transfers by device, revenue by new vs existing — in light and grey.
- `bar-best-day` — **Week with its best day called out.** A week of daily totals where the best day is lit and named beside the weekly total.
- `bar-daily-tokens` — **Thirty days of paired bars.** A month of daily volume in two series — input and output, reads and writes — with the axis and grid for reading values off.
- `bar-horizontal-locations` — **Ranked locations, horizontal.** A ranked breakdown by place or category where names are long — horizontal bars, the largest on top.
- `bar-commission-by-day` — **Thirty days with the total on the right.** A month of daily earnings or orders as one series, with the period total and the best day pinned to the right of the header.
- `bar-clicks-by-channel` — **Daily stack across five channels.** Daily volume split across up to five sources — channels, regions, plans — stacked down the grey ramp (Light, Gray1, Gray, Gray3, Gray4) so every part stays distinct without a hue.
- `bar-attendance-stacked` — **Daily make-up in three tones.** Each day split into three states — on time, late, absent — stacked so the full height is the headcount.
- `bar-green-gradient` — **Bars on a gradient card.** The one chart a board is about — a week of revenue on a gradient card, bars in the card's ink.
- `bar-contrast` — **Bars on a contrast card.** A comparison chart lifted out of the board on a contrast card — it reads first on any Scene.
- `bar-revenue-by-category` — **Single series breakdown.** A single metric broken down by category (revenue per category, signups per plan).
- `bar-multi-series` — **Segment split.** Comparing two segments of the same total side by side (new vs returning, mobile vs desktop).
- `bar-target-alert` — **Comparison with a success alert.** A breakdown where every category beat its goal — celebrate it with a success alert.
- `bar-horizontal` — **Horizontal bars (indexAxis: y).** Long category labels or a ranking that reads better horizontally — set `indexAxis` to `y`.
- `bar-multi-header` — **Comparison with a full header strip.** A wide chart that should also carry the board's summary row — up to six headline numbers above one plot.
- `bar-quota-target` — **Comparison against a quota.** Categories measured against one shared goal — deals per rep against quota, tickets per agent against an SLA.
- `bar-stacked-by-plan` — **Stacked total and its make-up.** Series that are parts of one total in the same unit — MRR by plan, visitors by channel, tickets by priority. Set `stacked: true`.
- `bar-combo-rate` — **Volume with a rate line (combo).** A count and a rate that explain each other — signups and conversion, orders and average order value, requests and error rate. Give the rate `type: "Line"` and `axis: "Right"`.
- `bar-icon-hint` — **Bars with a title icon and an explanation.** A breakdown whose numbers need a one-line definition (which orders count, which time zone the days are in), with an icon matching the rest of the row.
- `bar-color` — **Bars on a coloured card.** A breakdown that should anchor the board — a weekly total in a dark card on a light board, or a brand-coloured card on a dark one.
- `bar-highlight-latest` — **Bars with the latest one highlighted.** A run of periods where the current one is what the reader came for — this week against the last twelve — drawn in grey with the latest bar in the card's ink.
- `bar-striped` — **Striped bars with this month solid.** A year by month where the current month is the point: the past months quiet and hatched, this month solid, and the forecast months still to come as dashed outlines, with the average as a line.

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/bar-chart.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown` for a change, `Star` for a rating
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Faking a forecast or a quiet series with a lighter colour — a dataset's `style` hatches its bars (`Striped`) or draws only a dashed outline (`Outline`), and a single value's `style` overrides it: a run of striped months with the current one `Solid` and the months still to come `Outline` is one dataset.
- Mismatched lengths: if `labels` has 6 entries, every dataset's `data` should have 6 entries.
- Sending `value` as a string (`"65"`) — values must be numbers.
- Setting `color` inside `data` items when you meant the whole series — series color goes on the dataset.
- Wanting horizontal bars? Set `indexAxis` to `y` — do not look for a separate horizontal-bar widget type.
- Returning `204` (or an empty series) for a period with no activity — emit every bucket in the window with `{ "value": 0 }` instead. Flat zero bars are the honest answer and read as intentional; a blank widget reads as broken.
- Turning `ticksX` off for a long daily series — the labels stay level and thin themselves to fit, so thirty or ninety dates read fine. Leave them on and keep each label short (`"Sep 9"`, not `"2026-09-09T00:00:00Z"`); the dates are what make the trend readable.
- Setting `grid` just to make the chart look finished — omit it. Faint value gridlines are the default; `grid: false` only exists to switch them off.
- Drawing a goal as an extra dataset of identical bars — send `target: { "value": 12, "label": "Quota" }` instead. It draws one labelled line across every bar and keeps it in view even when no bar reaches it.
- Shifting values up so they stay positive, or sending a loss as a positive bar in red — send the negative number. The chart draws a zero line and hangs the bar below it.
- Stacking series that do not add up — `stacked: true` says the columns are a total. A rate, an average or a series in another unit belongs on a line (`type: "Line"`, `axis: "Right"`) instead.
- Putting a percentage on the same axis as counts in the hundreds — it flattens into a line along the bottom. Give the rate series `axis: "Right"` so it gets its own scale.
- Making every dataset a line — a BarChart needs at least one bar series. For lines only, use a LineChart.
- Combining `type: "Line"` or `axis: "Right"` with horizontal bars (`indexAxis: y`) — a combo chart is vertical only, and the response fails validation.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
