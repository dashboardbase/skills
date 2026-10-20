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

### Comparison against a quota

Categories measured against one shared goal — deals per rep against quota, tickets per agent against an SLA.

```json
{
  "title": "Deals closed",
  "actions": [
    {
      "title": "Open pipeline",
      "type": "link",
      "url": "https://example.com/pipeline"
    }
  ],
  "data": {
    "header": {
      "title": "58",
      "subtitle": "This quarter",
      "badge": {
        "text": "3 of 5 at quota",
        "color": "Success"
      }
    },
    "labels": [
      "Alice",
      "Bob",
      "Chen",
      "Dana",
      "Eli"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 14
          },
          {
            "value": 9
          },
          {
            "value": 12
          },
          {
            "value": 15
          },
          {
            "value": 8
          }
        ],
        "label": "Deals"
      }
    ],
    "target": {
      "value": 12,
      "label": "Quota"
    }
  }
}
```

### Stacked total and its make-up

Series that are parts of one total in the same unit — MRR by plan, visitors by channel, tickets by priority. Set `stacked: true`.

```json
{
  "title": "MRR by plan",
  "actions": [
    {
      "title": "Open billing",
      "type": "link",
      "url": "https://example.com/billing"
    }
  ],
  "data": {
    "header": {
      "title": "$48.2k",
      "subtitle": "MRR, Sep",
      "badge": {
        "text": "+12%",
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
      "Sep"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 6000,
            "prefix": "$"
          },
          {
            "value": 7000,
            "prefix": "$"
          },
          {
            "value": 7000,
            "prefix": "$"
          },
          {
            "value": 8000,
            "prefix": "$"
          },
          {
            "value": 8000,
            "prefix": "$"
          },
          {
            "value": 9200,
            "prefix": "$"
          }
        ],
        "label": "Starter"
      },
      {
        "data": [
          {
            "value": 12000,
            "prefix": "$"
          },
          {
            "value": 13000,
            "prefix": "$"
          },
          {
            "value": 15000,
            "prefix": "$"
          },
          {
            "value": 16000,
            "prefix": "$"
          },
          {
            "value": 18000,
            "prefix": "$"
          },
          {
            "value": 19000,
            "prefix": "$"
          }
        ],
        "label": "Team"
      },
      {
        "data": [
          {
            "value": 10000,
            "prefix": "$"
          },
          {
            "value": 11000,
            "prefix": "$"
          },
          {
            "value": 13000,
            "prefix": "$"
          },
          {
            "value": 15000,
            "prefix": "$"
          },
          {
            "value": 17000,
            "prefix": "$"
          },
          {
            "value": 20000,
            "prefix": "$"
          }
        ],
        "label": "Business"
      }
    ],
    "stacked": true
  }
}
```

### Volume with a rate line (combo)

A count and a rate that explain each other — signups and conversion, orders and average order value, requests and error rate. Give the rate `type: "Line"` and `axis: "Right"`.

```json
{
  "title": "Signups and conversion",
  "actions": [
    {
      "title": "Open funnel",
      "type": "link",
      "url": "https://example.com/funnel"
    }
  ],
  "data": {
    "header": {
      "title": "320",
      "subtitle": "Signups this week",
      "badge": {
        "text": "+10%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "W34",
      "W35",
      "W36",
      "W37",
      "W38",
      "W39",
      "W40",
      "W41"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 180
          },
          {
            "value": 210
          },
          {
            "value": 195
          },
          {
            "value": 240
          },
          {
            "value": 260
          },
          {
            "value": 250
          },
          {
            "value": 290
          },
          {
            "value": 320
          }
        ],
        "label": "Signups"
      },
      {
        "data": [
          {
            "value": 3.1,
            "postfix": "%"
          },
          {
            "value": 3.4,
            "postfix": "%"
          },
          {
            "value": 3.2,
            "postfix": "%"
          },
          {
            "value": 3.8,
            "postfix": "%"
          },
          {
            "value": 4,
            "postfix": "%"
          },
          {
            "value": 3.9,
            "postfix": "%"
          },
          {
            "value": 4.4,
            "postfix": "%"
          },
          {
            "value": 4.7,
            "postfix": "%"
          }
        ],
        "label": "Conversion",
        "type": "Line",
        "axis": "Right"
      }
    ],
    "headers": [
      {
        "title": "4.7%",
        "subtitle": "Trial to paid"
      }
    ]
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/bar-chart.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

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
- Turning `ticksX` off for a long daily series — the labels stay level and thin themselves to fit, so thirty or ninety dates read fine. Leave them on and keep each label short (`"Sep 9"`, not `"2026-09-09T00:00:00Z"`); the dates are what make the trend readable.
- Setting `grid` just to make the chart look finished — omit it. Faint value gridlines are the default; `grid: false` only exists to switch them off.
- Drawing a goal as an extra dataset of identical bars — send `target: { "value": 12, "label": "Quota" }` instead. It draws one labelled line across every bar and keeps it in view even when no bar reaches it.
- Stacking series that do not add up — `stacked: true` says the columns are a total. A rate, an average or a series in another unit belongs on a line (`type: "Line"`, `axis: "Right"`) instead.
- Putting a percentage on the same axis as counts in the hundreds — it flattens into a line along the bottom. Give the rate series `axis: "Right"` so it gets its own scale.
- Making every dataset a line — a BarChart needs at least one bar series. For lines only, use a LineChart.
- Combining `type: "Line"` or `axis: "Right"` with horizontal bars (`indexAxis: y`) — a combo chart is vertical only, and the response fails validation.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
