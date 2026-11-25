# KPI widget

> **Load this file when:** building a single-number / headline / KPI tile.

## Purpose

Show a single headline number with optional subtitle and trend badge — ideal for MRR, active users, conversion rate, error count, anything you'd put in a 'big number' tile. Add an optional `progress` field to turn it into a progress-to-goal tile (e.g. 'MRR $8.2k / $10k') with a right-aligned cap label and a thin bar under the number (or `shape: Meter` for a row of segmented ticks). Add `segments` to split a total into 2–6 parts on one bar with a legend (Online / Offline, storage by type). Add an optional `trend` (the recent readings, oldest first) to draw a sparkline beside the number.

In a setup file, this widget's `type` is `kpi` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/kpi.json` for use with a JSON Schema validator:

```json
{
  "required": [
    "header"
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
      "additionalProperties": false
    },
    "progress": {
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
      "additionalProperties": false,
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
    "trend": {
      "oneOf": [
        {
          "maxItems": 120,
          "minItems": 2,
          "type": "array",
          "items": {
            "type": "number",
            "format": "double"
          },
          "x-placeholder": true
        },
        {
          "required": [
            "values"
          ],
          "type": "object",
          "properties": {
            "values": {
              "maxItems": 120,
              "minItems": 2,
              "type": "array",
              "items": {
                "type": "number",
                "format": "double"
              },
              "x-placeholder": true
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
            "smooth": {
              "title": "Smooth line",
              "type": "boolean",
              "default": false,
              "nullable": true
            },
            "fill": {
              "title": "Filled area",
              "type": "boolean",
              "default": true,
              "nullable": true
            },
            "type": {
              "enum": [
                "Line",
                "Bars"
              ],
              "type": "string",
              "title": "Trend style",
              "nullable": true
            }
          },
          "additionalProperties": false
        }
      ],
      "nullable": true
    },
    "segments": {
      "maxItems": 6,
      "type": "array",
      "items": {
        "required": [
          "label",
          "value"
        ],
        "type": "object",
        "properties": {
          "label": {
            "minLength": 1,
            "type": "string"
          },
          "value": {
            "type": "number",
            "format": "double"
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
          "display": {
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "nullable": true
    },
    "strip": {
      "enum": [
        "Headline",
        "Equal"
      ],
      "type": "string",
      "nullable": true
    }
  },
  "additionalProperties": false
}
```

## Example response

```json
{
  "title": "Conversion rate",
  "actions": [
    {
      "title": "View Report",
      "type": "link",
      "url": "https://example.com/kpi-report"
    }
  ],
  "data": {
    "header": {
      "title": "3.2%",
      "subtitle": "vs last month",
      "badge": {
        "text": "-0.4%",
        "icon": "ArrowDown",
        "color": "Danger"
      }
    }
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/api-key/kpichart' \
-H "x-api-key: test"
```

## Header — headline, subtitle, and colored badge

This widget **requires** a `header` — it is the whole widget. Use the three parts together:

- `title` — the headline number.
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

## Sparkline

`trend` is the recent readings, oldest first, ending with the reading the headline shows — the last 7 days, the last 12 weeks. Send them as a plain array of numbers, or as an object when the line needs styling: `values` holds the same array, and `color`, `smooth` and `fill` sit beside it. It is drawn as a small line beside the headline (or under it on a taller card), coloured like the badge unless `color` says otherwise (e.g. `Light` for a white line under a green badge), with a soft fill under it (`fill: false` for a bare line) and no marker on the latest point — the headline beside it already says "now". Like a LineChart it is drawn with straight segments; set `smooth: true` for a curve. It is decoration on top of the number: it is left out where the tile has no room for it (a narrow 2-column tile, a phone showing two KPIs side by side), and it is not drawn next to `progress` or a `headers` strip. A Table cell takes the same `trend`.

```json
{
  "header": { "title": "$31,460", "subtitle": "vs last month", "badge": { "text": "+8%", "icon": "ArrowUp", "color": "Success" } },
  "trend": [25800, 26400, 27300, 28100, 29400, 30100, 31460]
}
```

Styled — a white curve with no fill:

```json
{
  "trend": { "values": [25800, 26400, 27300, 28100, 29400, 30100, 31460], "color": "Light", "smooth": true, "fill": false }
}
```



## Variations

Other shapes and styling for this widget — pick the one closest to your data:

### Revenue with an icon and trend

The everyday headline tile of a KPI row — a currency total, a small icon naming it, and the change against last period in colour.

```json
{
  "title": "Total Revenue",
  "actions": [
    {
      "title": "View report",
      "type": "link",
      "url": "https://example.com/revenue"
    }
  ],
  "data": {
    "header": {
      "title": "$45,232",
      "subtitle": "vs last month",
      "badge": {
        "text": "+20.1%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    }
  },
  "icon": "Cash"
}
```

### Count with a unit and a detail line

A count that needs its unit to read (files, minutes, seats) plus one line of context under it — storage used, share of the total.

```json
{
  "title": "Documents",
  "actions": [
    {
      "title": "View more",
      "type": "link",
      "url": "https://example.com/files/documents"
    }
  ],
  "data": {
    "header": {
      "title": "1,390",
      "subtitle": "200 GB · 16% of used storage",
      "unit": "files"
    }
  },
  "icon": "Cloud"
}
```

### Change pill beside the number

A product-analytics tile where the change is the point: an outlined pill with the arrow, and a plain caption under the number.

```json
{
  "title": "Daily active users",
  "actions": [
    {
      "title": "Open analytics",
      "type": "link",
      "url": "https://example.com/dau"
    }
  ],
  "data": {
    "header": {
      "title": "3,450",
      "subtitle": "vs last week",
      "badge": {
        "text": "+12.1%",
        "icon": "ArrowUp",
        "color": "Success",
        "fill": "Outline"
      }
    }
  }
}
```

### Usage against a cap as a meter

Consumption toward a plan limit or quota — tokens, API calls, seats — drawn as a row of segmented ticks under the number.

```json
{
  "title": "Volume",
  "actions": [
    {
      "title": "Usage",
      "type": "link",
      "url": "https://example.com/usage"
    }
  ],
  "data": {
    "header": {
      "title": "2,450,230",
      "subtitle": "Total tokens generated",
      "unit": "of 5,000,000"
    },
    "progress": {
      "value": 2450230,
      "max": 5000000,
      "color": "Green",
      "shape": "Meter"
    }
  }
}
```

### Total split into two channels

A total that is the sum of two or three parts the reader compares — online vs offline, new vs returning — as one split bar with a legend.

```json
{
  "title": "Bookings",
  "actions": [
    {
      "title": "Open bookings",
      "type": "link",
      "url": "https://example.com/bookings"
    }
  ],
  "data": {
    "header": {
      "title": "20,395",
      "subtitle": "total bookings vs previous period",
      "badge": {
        "text": "+11.2%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "segments": [
      {
        "label": "Online",
        "value": 14839,
        "color": "Light",
        "display": "14,839"
      },
      {
        "label": "Offline",
        "value": 5556,
        "color": "Gray",
        "display": "5,556"
      }
    ]
  }
}
```

### Score out of a maximum

A rating or score that only reads against its scale — 8.9 out of 10, 4.5 out of 5 — with the change since last period.

```json
{
  "title": "Team satisfaction",
  "actions": [
    {
      "title": "View survey",
      "type": "link",
      "url": "https://example.com/survey"
    }
  ],
  "data": {
    "header": {
      "title": "8.9",
      "subtitle": "vs last quarter",
      "badge": {
        "text": "+0.2",
        "icon": "ArrowUp",
        "color": "Success"
      },
      "unit": "/10"
    }
  },
  "icon": "Star"
}
```

### More examples

To see one in full, add `?variant=<id>` to the example request above — it returns that example's complete response.

- `kpi-stat-strip` — **Row of stats in one card.** Four to six headline numbers that belong together — orders, sales, commission, refund rate — as one joined card with a hairline between each, instead of a row of separate tiles.
- `kpi-links-strip` — **Stat strip with a unit and change.** A summary strip over a list page — active out of allowed, clicks, rates — where one block reads against a cap and the rest carry their change.
- `kpi-leads-down` — **Count that fell, in red.** A count tile in a row of four where this one moved the wrong way — the red change tells the reader where to look first.
- `kpi-balance-currency` — **Balance with its currency code.** A balance or account total where the currency code belongs beside the amount, not in it — one tile per currency.
- `kpi-goal-meter-gradient` — **Goal meter on a gradient card.** The one goal the board is about — a hit rate or target share on a gradient card so it leads the row.
- `kpi-split-storage` — **Storage by type on one bar.** Capacity used, broken down by what fills it — storage by file type, budget by department — with what is left as the last part.
- `kpi-split-attendance` — **Share of the day in three parts.** A rate whose make-up matters as much as the headline — on time, day off, sick — in one bar under the number.
- `kpi-attendance-contrast` — **Headline on a contrast card.** The KPI a room should read first, lifted out of the row on a contrast card that stays legible on any Scene.
- `kpi-saas-mrr` — **Monthly recurring revenue.** A monetary, point-in-time metric (MRR, ARR, balance) where the headline is a currency value.
- `kpi-app-dau` — **Active users (count).** A whole-number count of currently-active entities (active users, online devices, open tickets).
- `kpi-churn-alert` — **Alert metric (outline badge).** A metric where a high value is bad (churn, error rate) — colour the badge to draw the eye.
- `kpi-error-rate-alert` — **Metric with a critical alert.** A metric that has breached a threshold and should surface a critical alert banner.
- `kpi-mrr-goal` — **Progress toward a goal (MRR / cap).** A headline number that is tracking toward a goal or cap — 'MRR $8.2k / $10k'. Add the optional `progress` field.
- `kpi-multi-header` — **Several totals in one tile.** Several totals that belong together — pages, users, organizations, dashboards — in one widget and one endpoint instead of a row of separate KPI tiles.
- `kpi-sparkline` — **Headline with a sparkline.** A headline number whose recent shape matters as much as its value — MRR over the last weeks, signups over the last days — without giving it a whole chart.
- `kpi-icon-hint` — **Title with an icon and an explanation.** A row of headline tiles where an icon helps tell them apart, or a metric whose definition viewers ask about (what counts as revenue, which orders are included).
- `kpi-color-light` — **A white card that leads the board.** The one headline number the room should read first — revenue year to date, MRR — lifted off a dark board in a white card.
- `kpi-color-contrast` — **A card that stays in focus on any Scene.** The headline number on a dashboard that may be shown on more than one Scene — white on the dark board, near-black on a light one, glass on Glass — so it always stands out from the cards around it.
- `kpi-color-gradient` — **Card shaded in one colour.** A headline number in a brand colour, shaded from a lighter corner to a deeper one — the Gradient card style. Use it for one card at most; the rest of the board stays flat.
- `kpi-color-dark` — **A dark card that leads the board.** The headline number on a light board, or a second lead number beside a white card — set in a near-black card that stands out on any Scene.
- `kpi-trend-bars` — **Headline with small bars.** A headline number whose last few periods read better as separate bars than as a line — this month against the two before, or a run of days where each day stands on its own.
- `kpi-ring` — **Headline with a progress ring.** A headline number that is also a share of something — units sold against stock, seats filled, a quota reached — shown as a small ring with its percentage beside the number.

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/kpi.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown` for a change, `Star` for a rating
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Returning the metric as `data.value` — KPI uses `header.title`, not a value field (that's GaugeChart).
- Forgetting `badge.text` when including a badge — `text` is required; icon and color are decoration.
- Putting the trend percentage in `header.subtitle` instead of `header.badge.text` — only badges render with color and icon.
- Setting `progress.max` to `0` or a `value` above `max` — when `progress` is present `max` must be `> 0` and `value` non-negative.
- Sending `trendColor` / `trendSmooth` beside `trend` — styling lives on the trend itself: switch to the object form, `"trend": { "values": [9800, 10150], "color": "Light", "smooth": true }`.
- Sending the readings as objects (`[{ "value": 9800 }]`) or as formatted strings — `trend` (or `trend.values`) is a plain array of numbers, oldest first. Send 2 to 120 points; 7–30 reads best.
- Ending `trend` on a different period than the headline — its last point is read as "now" beside the number, so it should be the reading `header.title` shows.
- Drawing bars with a second widget — a KPI's `trend` takes `"type": "Bars"` (`"trend": { "values": [31, 38, 44], "type": "Bars" }`) for a row of small bars beside the number instead of a line: three for this month against the last two, a dozen or more for a run of days.
- Putting a ring progress next to a sparkline — `progress.shape: "Ring"` draws the goal as a small ring beside the number, in the same place a trend goes, so send one or the other. Without `shape` (or with `Bar`) it stays the thin bar under the number.
- Building a split bar (Orders 62% / Visits 25%, Online / Offline, storage by file type) out of a `progress` — send `segments` instead: 2–6 parts, each `{ "label": "Online", "value": 14839, "color": "Light" }` (add `display` to show a formatted value such as `"522 GB"`). Widths are each part's share of the sum and the legend shows the share; it replaces `progress` and `trend`. Give the tile two rows — the legend sits under the bar — and keep it to four parts on a narrow tile.
- Writing the unit into the headline (`"901 files"`, `"8.9/10"`) — put the number in `header.title` and the suffix in `header.unit` (`"files"`, `"/10"`, `"USD"`, `"of 5,000,000"`); it is drawn smaller and muted so the number stays the headline.
- Building a row of separate KPI tiles for 4–6 numbers of equal weight (orders, sales, commission, refund rate) — send one KPI with `header` plus `headers` and `"strip": "Equal"`: every block at the same size across the card's width, a hairline between each. Name each block with `label` (`"Orders"` above `"315"`) and keep `subtitle` for the comparison, or leave it out so a one-row tile still fits.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
