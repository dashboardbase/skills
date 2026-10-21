# LineChart widget

> **Load this file when:** building a line chart / time series / trend over time.

## Purpose

Render one or more series of values across a shared label axis as connected lines — ideal for time-series data (traffic, revenue, latency over time).

In a setup file, this widget's `type` is `line` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/line-chart.json` for use with a JSON Schema validator:

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
          "style": {
            "enum": [
              "Solid",
              "Comparison"
            ],
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      }
    },
    "fill": {
      "type": "boolean",
      "nullable": true
    },
    "ticksX": {
      "type": "boolean",
      "nullable": true
    },
    "ticksY": {
      "type": "boolean",
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
    "smooth": {
      "type": "boolean",
      "nullable": true
    },
    "points": {
      "type": "boolean",
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
    }
  },
  "additionalProperties": false
}
```

## Example response

```json
{
  "title": "Website Traffic",
  "actions": [
    {
      "title": "Analyze Traffic",
      "type": "link",
      "url": "https://example.com/traffic-analysis"
    }
  ],
  "data": {
    "header": {
      "title": "1010",
      "subtitle": "Last 7 days",
      "badge": {
        "text": "+70%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "Mon",
      "Tue",
      "Wed",
      "Thu",
      "Fri",
      "Sat",
      "Sun"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 100
          },
          {
            "value": 120
          },
          {
            "value": 150
          },
          {
            "value": 130
          },
          {
            "value": 160
          },
          {
            "value": 180
          },
          {
            "value": 170
          }
        ],
        "label": "Page Views"
      }
    ]
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/api-key/linechart' \
-H "x-api-key: test"
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

The canonical example shows a single dataset because colors and multi-series are optional. To draw multiple lines, pass several entries in `datasets`; each has its own `label` and an optional `color`. `labels` is shared across all datasets. The area beneath each line is filled by default; set `fill: false` for bare lines.

```json
{
  "labels": ["Mon", "Tue"],
  "datasets": [
    { "label": "Page Views", "color": "Blue", "data": [{"value": 100}, {"value": 120}] },
    { "label": "Unique Visitors", "color": "Green", "data": [{"value": 40}, {"value": 55}] }
  ]
}
```

**Compare with a previous period** by adding the earlier series with `style: "Comparison"`. It is drawn as a dashed grey line behind the others — no fill, no dots — and the tooltip shows the change against it ("▲ 12% vs Last week"). Give it a label that names the period, and the same number of points as `labels`.

```json
{
  "labels": ["Mon", "Tue", "Wed"],
  "datasets": [
    { "label": "This week", "data": [{"value": 120}, {"value": 150}, {"value": 170}] },
    { "label": "Last week", "style": "Comparison", "data": [{"value": 100}, {"value": 130}, {"value": 140}] }
  ]
}
```

**Mark a goal** with `target`: a labelled line across the chart at that value, which the value axis always stretches to include.

```json
{
  "labels": ["Jul", "Aug", "Sep"],
  "datasets": [{ "label": "MRR", "data": [{"value": 41000, "prefix": "$"}, {"value": 44500, "prefix": "$"}, {"value": 47200, "prefix": "$"}] }],
  "target": { "value": 50000, "label": "Q3 goal" }
}
```

## Variations

Other shapes and styling for this widget — pick the one closest to your data:

### Multi-series (filled)

Comparing two trends on one chart with a filled area beneath each line.

```json
{
  "title": "Traffic breakdown",
  "actions": [
    {
      "title": "Analyze Traffic",
      "type": "link",
      "url": "https://example.com/traffic-analysis"
    }
  ],
  "data": {
    "header": {
      "title": "1010",
      "subtitle": "Page views, last 7 days",
      "badge": {
        "text": "+70%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "Mon",
      "Tue",
      "Wed",
      "Thu",
      "Fri",
      "Sat",
      "Sun"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 100
          },
          {
            "value": 120
          },
          {
            "value": 150
          },
          {
            "value": 130
          },
          {
            "value": 160
          },
          {
            "value": 180
          },
          {
            "value": 170
          }
        ],
        "label": "Page Views"
      },
      {
        "data": [
          {
            "value": 40
          },
          {
            "value": 48
          },
          {
            "value": 55
          },
          {
            "value": 50
          },
          {
            "value": 62
          },
          {
            "value": 70
          },
          {
            "value": 66
          }
        ],
        "label": "Unique Visitors"
      }
    ],
    "fill": true
  }
}
```

### Latency trend (unit postfix)

An average/percentile metric over time where each point carries a unit (ms, %, $).

```json
{
  "title": "Avg response time",
  "actions": [
    {
      "title": "View Traces",
      "type": "link",
      "url": "https://example.com/traces"
    }
  ],
  "data": {
    "header": {
      "title": "182ms",
      "subtitle": "Last 7 days",
      "badge": {
        "text": "+12ms",
        "icon": "ArrowUp",
        "color": "Danger"
      }
    },
    "labels": [
      "Mon",
      "Tue",
      "Wed",
      "Thu",
      "Fri",
      "Sat",
      "Sun"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 168,
            "postfix": "ms"
          },
          {
            "value": 172,
            "postfix": "ms"
          },
          {
            "value": 165,
            "postfix": "ms"
          },
          {
            "value": 190,
            "postfix": "ms"
          },
          {
            "value": 201,
            "postfix": "ms"
          },
          {
            "value": 178,
            "postfix": "ms"
          },
          {
            "value": 182,
            "postfix": "ms"
          }
        ],
        "label": "p95 latency"
      }
    ]
  }
}
```

### Trend with an info alert

A trend where a spike has a known cause — add an info alert so viewers don't misread it.

```json
{
  "title": "Website Traffic",
  "actions": [
    {
      "title": "Analyze Traffic",
      "type": "link",
      "url": "https://example.com/traffic-analysis"
    }
  ],
  "data": {
    "header": {
      "title": "1480",
      "subtitle": "Last 7 days",
      "badge": {
        "text": "+146%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "Mon",
      "Tue",
      "Wed",
      "Thu",
      "Fri",
      "Sat",
      "Sun"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 100
          },
          {
            "value": 320
          },
          {
            "value": 310
          },
          {
            "value": 280
          },
          {
            "value": 230
          },
          {
            "value": 140
          },
          {
            "value": 100
          }
        ],
        "label": "Page Views"
      }
    ]
  },
  "alert": {
    "active": true,
    "level": "info",
    "message": "Includes the marketing campaign launched Tue"
  }
}
```

### Trend with a header strip

One endpoint should carry several headline numbers above a single trend — visitors, new visitors, engagement and the running total, instead of four separate KPI widgets.

```json
{
  "title": "Audience",
  "actions": [
    {
      "title": "Analyze Traffic",
      "type": "link",
      "url": "https://example.com/traffic-analysis"
    }
  ],
  "data": {
    "header": {
      "title": "395",
      "subtitle": "Visitors",
      "badge": {
        "text": "+348.9%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
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
            "value": 20
          },
          {
            "value": 24
          },
          {
            "value": 28
          },
          {
            "value": 26
          },
          {
            "value": 38
          },
          {
            "value": 40
          },
          {
            "value": 42
          },
          {
            "value": 62
          },
          {
            "value": 58
          },
          {
            "value": 64
          },
          {
            "value": 70
          }
        ],
        "label": "Visitors"
      },
      {
        "data": [
          {
            "value": 12
          },
          {
            "value": 16
          },
          {
            "value": 18
          },
          {
            "value": 22
          },
          {
            "value": 30
          },
          {
            "value": 32
          },
          {
            "value": 30
          },
          {
            "value": 34
          },
          {
            "value": 36
          },
          {
            "value": 44
          },
          {
            "value": 40
          }
        ],
        "label": "New visitors"
      }
    ],
    "fill": true,
    "headers": [
      {
        "title": "932",
        "subtitle": "New visitors",
        "badge": {
          "text": "+565.7%",
          "icon": "ArrowUp",
          "color": "Success"
        }
      },
      {
        "title": "1m 50s",
        "subtitle": "Avg. engagement",
        "badge": {
          "text": "+250.1%",
          "icon": "ArrowUp",
          "color": "Success"
        }
      },
      {
        "title": "150K",
        "subtitle": "Total visitors"
      }
    ]
  }
}
```

### This period vs the last one

A trend that should be read against the same window before it — this week vs last week, this year vs last year.

```json
{
  "title": "Signups",
  "actions": [
    {
      "title": "Open funnel",
      "type": "link",
      "url": "https://example.com/signups"
    }
  ],
  "data": {
    "header": {
      "title": "1,234",
      "subtitle": "This week",
      "badge": {
        "text": "+18%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "Mon",
      "Tue",
      "Wed",
      "Thu",
      "Fri",
      "Sat",
      "Sun"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 142
          },
          {
            "value": 168
          },
          {
            "value": 181
          },
          {
            "value": 176
          },
          {
            "value": 204
          },
          {
            "value": 186
          },
          {
            "value": 177
          }
        ],
        "label": "This week"
      },
      {
        "data": [
          {
            "value": 128
          },
          {
            "value": 139
          },
          {
            "value": 150
          },
          {
            "value": 161
          },
          {
            "value": 158
          },
          {
            "value": 150
          },
          {
            "value": 160
          }
        ],
        "label": "Last week",
        "style": "Comparison"
      }
    ]
  }
}
```

### Trend toward a target

A metric tracked against a goal, quota or budget — MRR toward this quarter's target, spend against a monthly budget.

```json
{
  "title": "MRR",
  "actions": [
    {
      "title": "Open billing",
      "type": "link",
      "url": "https://example.com/billing"
    }
  ],
  "data": {
    "header": {
      "title": "$47,200",
      "subtitle": "Toward the $50k Q3 goal",
      "badge": {
        "text": "+6.1%",
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
            "value": 36800,
            "prefix": "$"
          },
          {
            "value": 38900,
            "prefix": "$"
          },
          {
            "value": 41000,
            "prefix": "$"
          },
          {
            "value": 42700,
            "prefix": "$"
          },
          {
            "value": 44500,
            "prefix": "$"
          },
          {
            "value": 47200,
            "prefix": "$"
          }
        ],
        "label": "MRR"
      }
    ],
    "target": {
      "value": 50000,
      "label": "Q3 goal"
    }
  }
}
```

### Smooth curve, no gridlines

A longer trend read for its shape rather than its exact values — daily active users over two weeks on a wallboard.

```json
{
  "title": "Daily active users",
  "actions": [
    {
      "title": "Open analytics",
      "type": "link",
      "url": "https://example.com/analytics"
    }
  ],
  "data": {
    "header": {
      "title": "2,543",
      "subtitle": "Last 14 days",
      "badge": {
        "text": "+12%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "Sep 23",
      "Sep 24",
      "Sep 25",
      "Sep 26",
      "Sep 27",
      "Sep 28",
      "Sep 29",
      "Sep 30",
      "Oct 1",
      "Oct 2",
      "Oct 3",
      "Oct 4",
      "Oct 5",
      "Oct 6"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 2120
          },
          {
            "value": 2260
          },
          {
            "value": 2190
          },
          {
            "value": 2310
          },
          {
            "value": 2180
          },
          {
            "value": 1960
          },
          {
            "value": 2020
          },
          {
            "value": 2280
          },
          {
            "value": 2410
          },
          {
            "value": 2350
          },
          {
            "value": 2470
          },
          {
            "value": 2390
          },
          {
            "value": 2210
          },
          {
            "value": 2543
          }
        ],
        "label": "Active users"
      }
    ],
    "smooth": true,
    "points": false,
    "grid": false
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/line-chart.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown`
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Sending `labels` as timestamps (`'2026-05-19T00:00:00Z'`) — labels are shown as sent, except that a set made entirely of plain `YYYY-MM-DD` dates is shortened to `'May 19'`. Send plain dates or your own short text (`'Mon'`, `'May 19'`).
- Sending uneven dataset lengths — every dataset's `data` length should match `labels`.
- Returning `null` between values to indicate a gap — Dashboardbase expects every index to have a `{ "value": <number> }` entry.
- Returning `204` (or an empty series) for a period with no activity — emit every bucket in the window with `{ "value": 0 }` instead. A flat zero line is the honest answer and reads as intentional; a blank widget reads as broken.
- Turning `ticksX` off for a long daily series — the labels stay level and thin themselves to fit, so thirty or ninety dates read fine. Leave them on and keep each label short (`"Sep 9"`, not `"2026-09-09T00:00:00Z"`); the dates are what make the trend readable.
- Setting `fill`, `grid` or `points` just to make the chart look finished — omit them. The defaults (filled area, faint value gridlines, dots only on short series, straight segments) are the designed look; the fields exist only to switch an effect off (`fill: false`, `grid: false`, `points: false`) or to ask for curves (`smooth: true`).
- Sending a previous period as an ordinary second series — set `style: "Comparison"` on it so it reads as the baseline (a dashed grey ghost) rather than a competing line, and label it with the period (`"Last week"`, `"2025"`).
- Using a flat second series, or a header block, to show a goal — send `target: { "value": 50000, "label": "Q3 goal" }` instead. It draws one labelled line and keeps it on the chart even when every reading is below it.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
