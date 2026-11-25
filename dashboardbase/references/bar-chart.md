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
      "type": "boolean",
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

**Mark a goal or quota** with `target`: a labelled line across the bars at that value (a vertical line when `indexAxis` is `y`), which the value axis always stretches to include.

```json
{
  "labels": ["Alice", "Bob", "Chen"],
  "datasets": [{ "label": "Deals closed", "data": [{"value": 14}, {"value": 9}, {"value": 12}] }],
  "target": { "value": 12, "label": "Quota" }
}
```

**Stack the series** with `stacked: true` when they are parts of one whole in the same unit — MRR by plan, visitors by channel, tickets by priority. The top of each column is the total, so put that total in `header.title`. Do not stack series that do not add up (rates, mixed units, negative values).

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

### Stacked bars in two tones

A monthly total and its two parts in one bar — transfers by device, revenue by new vs existing — in light and grey.

```json
{
  "title": "Monthly File Transfer",
  "actions": [
    {
      "title": "Transfers",
      "type": "link",
      "url": "https://example.com/transfers"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Transfers by device, last 12 months"
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
            "value": 310
          },
          {
            "value": 620
          },
          {
            "value": 520
          },
          {
            "value": 160
          },
          {
            "value": 470
          },
          {
            "value": 460
          },
          {
            "value": 260
          },
          {
            "value": 640
          },
          {
            "value": 380
          },
          {
            "value": 630
          },
          {
            "value": 480
          },
          {
            "value": 640
          }
        ],
        "label": "Desktop",
        "color": "Light"
      },
      {
        "data": [
          {
            "value": 90
          },
          {
            "value": 410
          },
          {
            "value": 250
          },
          {
            "value": 410
          },
          {
            "value": 280
          },
          {
            "value": 290
          },
          {
            "value": 210
          },
          {
            "value": 460
          },
          {
            "value": 320
          },
          {
            "value": 390
          },
          {
            "value": 350
          },
          {
            "value": 430
          }
        ],
        "label": "Mobile",
        "color": "Gray"
      }
    ],
    "stacked": true
  }
}
```

### Thirty days of paired bars

A month of daily volume in two series — input and output, reads and writes — with the axis and grid for reading values off.

```json
{
  "title": "Daily token consumption",
  "actions": [
    {
      "title": "Usage",
      "type": "link",
      "url": "https://example.com/usage"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Input and output token volume across your workspace"
    },
    "labels": [
      "1",
      "2",
      "3",
      "4",
      "5",
      "6",
      "7",
      "8",
      "9",
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
            "value": 520
          },
          {
            "value": 410
          },
          {
            "value": 590
          },
          {
            "value": 1200
          },
          {
            "value": 650
          },
          {
            "value": 500
          },
          {
            "value": 540
          },
          {
            "value": 680
          },
          {
            "value": 1290
          },
          {
            "value": 570
          },
          {
            "value": 600
          },
          {
            "value": 500
          },
          {
            "value": 720
          },
          {
            "value": 650
          },
          {
            "value": 670
          },
          {
            "value": 660
          },
          {
            "value": 740
          },
          {
            "value": 1780
          },
          {
            "value": 500
          },
          {
            "value": 470
          },
          {
            "value": 1250
          },
          {
            "value": 760
          },
          {
            "value": 640
          },
          {
            "value": 570
          },
          {
            "value": 540
          },
          {
            "value": 690
          },
          {
            "value": 1160
          },
          {
            "value": 690
          },
          {
            "value": 580
          },
          {
            "value": 470
          }
        ],
        "label": "Input tokens",
        "color": "Light"
      },
      {
        "data": [
          {
            "value": 240
          },
          {
            "value": 190
          },
          {
            "value": 270
          },
          {
            "value": 560
          },
          {
            "value": 300
          },
          {
            "value": 240
          },
          {
            "value": 250
          },
          {
            "value": 310
          },
          {
            "value": 600
          },
          {
            "value": 270
          },
          {
            "value": 280
          },
          {
            "value": 240
          },
          {
            "value": 330
          },
          {
            "value": 300
          },
          {
            "value": 310
          },
          {
            "value": 300
          },
          {
            "value": 340
          },
          {
            "value": 820
          },
          {
            "value": 240
          },
          {
            "value": 230
          },
          {
            "value": 580
          },
          {
            "value": 350
          },
          {
            "value": 300
          },
          {
            "value": 270
          },
          {
            "value": 260
          },
          {
            "value": 320
          },
          {
            "value": 540
          },
          {
            "value": 330
          },
          {
            "value": 270
          },
          {
            "value": 220
          }
        ],
        "label": "Output tokens",
        "color": "Gray"
      }
    ],
    "ticksY": true,
    "grid": true
  }
}
```

### Ranked locations, horizontal

A ranked breakdown by place or category where names are long — horizontal bars, the largest on top.

```json
{
  "title": "Sales by Location",
  "actions": [
    {
      "title": "Export",
      "type": "link",
      "url": "https://example.com/locations"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Income in the last 28 days"
    },
    "labels": [
      "Canada",
      "Greenland",
      "Russia",
      "China",
      "Australia",
      "Greece",
      "Germany",
      "Brazil"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 1270,
            "prefix": "$",
            "postfix": "K"
          },
          {
            "value": 1200,
            "prefix": "$",
            "postfix": "K"
          },
          {
            "value": 945,
            "prefix": "$",
            "postfix": "K"
          },
          {
            "value": 900,
            "prefix": "$",
            "postfix": "K"
          },
          {
            "value": 675,
            "prefix": "$",
            "postfix": "K"
          },
          {
            "value": 475,
            "prefix": "$",
            "postfix": "K"
          },
          {
            "value": 410,
            "prefix": "$",
            "postfix": "K"
          },
          {
            "value": 360,
            "prefix": "$",
            "postfix": "K"
          }
        ],
        "label": "Sales",
        "color": "Gray"
      }
    ],
    "indexAxis": "y",
    "grid": false
  }
}
```

### Daily stack across five channels

Daily volume split across up to five sources — channels, regions, plans — stacked down the grey ramp (Light, Gray1, Gray, Gray3, Gray4) so every part stays distinct without a hue.

```json
{
  "title": "Clicks by channel",
  "actions": [
    {
      "title": "Links",
      "type": "link",
      "url": "https://example.com/links"
    }
  ],
  "data": {
    "header": {
      "subtitle": "4,957 clicks in the last 14 days. Today is counted until 10:00."
    },
    "labels": [
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
      "Nov 24"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 20
          },
          {
            "value": 10
          },
          {
            "value": 20
          },
          {
            "value": 20
          },
          {
            "value": 20
          },
          {
            "value": 100
          },
          {
            "value": 135
          },
          {
            "value": 140
          },
          {
            "value": 150
          },
          {
            "value": 180
          },
          {
            "value": 120
          },
          {
            "value": 150
          },
          {
            "value": 200
          },
          {
            "value": 90
          }
        ],
        "label": "YouTube",
        "color": "Light"
      },
      {
        "data": [
          {
            "value": 110
          },
          {
            "value": 120
          },
          {
            "value": 100
          },
          {
            "value": 55
          },
          {
            "value": 105
          },
          {
            "value": 95
          },
          {
            "value": 105
          },
          {
            "value": 90
          },
          {
            "value": 95
          },
          {
            "value": 150
          },
          {
            "value": 95
          },
          {
            "value": 110
          },
          {
            "value": 160
          },
          {
            "value": 60
          }
        ],
        "label": "TikTok",
        "color": "Gray1"
      },
      {
        "data": [
          {
            "value": 110
          },
          {
            "value": 140
          },
          {
            "value": 100
          },
          {
            "value": 80
          },
          {
            "value": 90
          },
          {
            "value": 105
          },
          {
            "value": 100
          },
          {
            "value": 100
          },
          {
            "value": 95
          },
          {
            "value": 180
          },
          {
            "value": 150
          },
          {
            "value": 170
          },
          {
            "value": 200
          },
          {
            "value": 70
          }
        ],
        "label": "Instagram",
        "color": "Gray"
      },
      {
        "data": [
          {
            "value": 35
          },
          {
            "value": 35
          },
          {
            "value": 30
          },
          {
            "value": 30
          },
          {
            "value": 40
          },
          {
            "value": 45
          },
          {
            "value": 40
          },
          {
            "value": 50
          },
          {
            "value": 40
          },
          {
            "value": 85
          },
          {
            "value": 45
          },
          {
            "value": 40
          },
          {
            "value": 85
          },
          {
            "value": 15
          }
        ],
        "label": "Blog",
        "color": "Gray3"
      },
      {
        "data": [
          {
            "value": 25
          },
          {
            "value": 25
          },
          {
            "value": 20
          },
          {
            "value": 15
          },
          {
            "value": 20
          },
          {
            "value": 20
          },
          {
            "value": 25
          },
          {
            "value": 25
          },
          {
            "value": 20
          },
          {
            "value": 35
          },
          {
            "value": 30
          },
          {
            "value": 25
          },
          {
            "value": 40
          },
          {
            "value": 10
          }
        ],
        "label": "Newsletter",
        "color": "Gray4"
      }
    ],
    "ticksY": true,
    "grid": true,
    "stacked": true
  }
}
```

### More examples

To see one in full, add `?variant=<id>` to the example request above — it returns that example's complete response.

- `bar-best-day` — **Week with its best day called out.** A week of daily totals where the best day is lit and named beside the weekly total.
- `bar-commission-by-day` — **Thirty days with the total on the right.** A month of daily earnings or orders as one series, with the period total and the best day pinned to the right of the header.
- `bar-attendance-stacked` — **Daily make-up in three tones.** Each day split into three states — on time, late, absent — stacked so the full height is the headcount.
- `bar-green-gradient` — **Bars on a gradient card.** The one chart a board is about — monthly growth on a glowing card, bars in the card's ink.
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
- Stacking series that do not add up — `stacked: true` says the columns are a total. A rate, an average or a series in another unit belongs on a line (`type: "Line"`, `axis: "Right"`) instead.
- Putting a percentage on the same axis as counts in the hundreds — it flattens into a line along the bottom. Give the rate series `axis: "Right"` so it gets its own scale.
- Making every dataset a line — a BarChart needs at least one bar series. For lines only, use a LineChart.
- Combining `type: "Line"` or `axis: "Right"` with horizontal bars (`indexAxis: y`) — a combo chart is vertical only, and the response fails validation.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
