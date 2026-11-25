# ContributionsGrid widget

> **Load this file when:** building a GitHub-style contributions / activity heatmap grid.

## Purpose

Render a GitHub-style grid of day cells shaded by an intensity value — ideal for activity streaks, deploys per day, commits, or any 'per-day count over time' series.

In a setup file, this widget's `type` is `contributions` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/contributions-grid.json` for use with a JSON Schema validator:

```json
{
  "required": [
    "cells"
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
    "cells": {
      "minItems": 1,
      "type": "array",
      "items": {
        "required": [
          "date",
          "value"
        ],
        "type": "object",
        "properties": {
          "date": {
            "type": "string",
            "format": "date"
          },
          "value": {
            "type": "number",
            "format": "double"
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
  "title": "Deploys",
  "actions": [
    {
      "title": "View Pipeline",
      "type": "link",
      "url": "https://example.com/pipeline"
    }
  ],
  "data": {
    "header": {
      "title": "142",
      "subtitle": "Last 14 days"
    },
    "cells": [
      {
        "date": "2026-01-01",
        "value": 2
      },
      {
        "date": "2026-01-02",
        "value": 0
      },
      {
        "date": "2026-01-03",
        "value": 5
      },
      {
        "date": "2026-01-04",
        "value": 1
      },
      {
        "date": "2026-01-05",
        "value": 0
      },
      {
        "date": "2026-01-06",
        "value": 8
      },
      {
        "date": "2026-01-07",
        "value": 3
      },
      {
        "date": "2026-01-08",
        "value": 4
      },
      {
        "date": "2026-01-09",
        "value": 0
      },
      {
        "date": "2026-01-10",
        "value": 6
      },
      {
        "date": "2026-01-11",
        "value": 2
      },
      {
        "date": "2026-01-12",
        "value": 1
      },
      {
        "date": "2026-01-13",
        "value": 7
      },
      {
        "date": "2026-01-14",
        "value": 3
      }
    ]
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/contributionsgrid'
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

### Activity grid on a gradient card

A streak or activity history that is the board's focus, on a gradient card.

```json
{
  "title": "Workouts",
  "data": {
    "header": {
      "title": "214",
      "subtitle": "sessions this year"
    },
    "cells": [
      {
        "date": "2026-01-01",
        "value": 2
      },
      {
        "date": "2026-01-02",
        "value": 3
      },
      {
        "date": "2026-01-03",
        "value": 2
      },
      {
        "date": "2026-01-04",
        "value": 0
      },
      {
        "date": "2026-01-05",
        "value": 3
      },
      {
        "date": "2026-01-06",
        "value": 1
      },
      {
        "date": "2026-01-07",
        "value": 2
      },
      {
        "date": "2026-01-08",
        "value": 3
      },
      {
        "date": "2026-01-09",
        "value": 0
      },
      {
        "date": "2026-01-10",
        "value": 2
      },
      {
        "date": "2026-01-11",
        "value": 3
      },
      {
        "date": "2026-01-12",
        "value": 2
      },
      {
        "date": "2026-01-13",
        "value": 1
      },
      {
        "date": "2026-01-14",
        "value": 1
      },
      {
        "date": "2026-01-15",
        "value": 3
      },
      {
        "date": "2026-01-16",
        "value": 2
      },
      {
        "date": "2026-01-17",
        "value": 0
      },
      {
        "date": "2026-01-18",
        "value": 2
      },
      {
        "date": "2026-01-19",
        "value": 0
      },
      {
        "date": "2026-01-20",
        "value": 3
      },
      {
        "date": "2026-01-21",
        "value": 0
      },
      {
        "date": "2026-01-22",
        "value": 0
      },
      {
        "date": "2026-01-23",
        "value": 3
      },
      {
        "date": "2026-01-24",
        "value": 3
      },
      {
        "date": "2026-01-25",
        "value": 1
      },
      {
        "date": "2026-01-26",
        "value": 1
      },
      {
        "date": "2026-01-27",
        "value": 2
      },
      {
        "date": "2026-01-28",
        "value": 0
      },
      {
        "date": "2026-01-29",
        "value": 0
      },
      {
        "date": "2026-01-30",
        "value": 3
      },
      {
        "date": "2026-01-31",
        "value": 1
      },
      {
        "date": "2026-02-01",
        "value": 3
      },
      {
        "date": "2026-02-02",
        "value": 0
      },
      {
        "date": "2026-02-03",
        "value": 1
      },
      {
        "date": "2026-02-04",
        "value": 2
      },
      {
        "date": "2026-02-05",
        "value": 1
      },
      {
        "date": "2026-02-06",
        "value": 0
      },
      {
        "date": "2026-02-07",
        "value": 3
      },
      {
        "date": "2026-02-08",
        "value": 1
      },
      {
        "date": "2026-02-09",
        "value": 2
      },
      {
        "date": "2026-02-10",
        "value": 2
      },
      {
        "date": "2026-02-11",
        "value": 1
      },
      {
        "date": "2026-02-12",
        "value": 3
      },
      {
        "date": "2026-02-13",
        "value": 0
      },
      {
        "date": "2026-02-14",
        "value": 3
      },
      {
        "date": "2026-02-15",
        "value": 3
      },
      {
        "date": "2026-02-16",
        "value": 0
      },
      {
        "date": "2026-02-17",
        "value": 2
      },
      {
        "date": "2026-02-18",
        "value": 3
      },
      {
        "date": "2026-02-19",
        "value": 1
      },
      {
        "date": "2026-02-20",
        "value": 1
      },
      {
        "date": "2026-02-21",
        "value": 1
      },
      {
        "date": "2026-02-22",
        "value": 3
      },
      {
        "date": "2026-02-23",
        "value": 3
      },
      {
        "date": "2026-02-24",
        "value": 2
      },
      {
        "date": "2026-02-25",
        "value": 2
      },
      {
        "date": "2026-02-26",
        "value": 3
      },
      {
        "date": "2026-02-27",
        "value": 2
      },
      {
        "date": "2026-02-28",
        "value": 3
      },
      {
        "date": "2026-03-01",
        "value": 1
      },
      {
        "date": "2026-03-02",
        "value": 3
      },
      {
        "date": "2026-03-03",
        "value": 1
      },
      {
        "date": "2026-03-04",
        "value": 2
      },
      {
        "date": "2026-03-05",
        "value": 1
      },
      {
        "date": "2026-03-06",
        "value": 0
      },
      {
        "date": "2026-03-07",
        "value": 3
      },
      {
        "date": "2026-03-08",
        "value": 2
      },
      {
        "date": "2026-03-09",
        "value": 3
      },
      {
        "date": "2026-03-10",
        "value": 2
      },
      {
        "date": "2026-03-11",
        "value": 1
      },
      {
        "date": "2026-03-12",
        "value": 2
      },
      {
        "date": "2026-03-13",
        "value": 0
      },
      {
        "date": "2026-03-14",
        "value": 1
      },
      {
        "date": "2026-03-15",
        "value": 2
      },
      {
        "date": "2026-03-16",
        "value": 1
      },
      {
        "date": "2026-03-17",
        "value": 2
      },
      {
        "date": "2026-03-18",
        "value": 0
      },
      {
        "date": "2026-03-19",
        "value": 2
      },
      {
        "date": "2026-03-20",
        "value": 0
      },
      {
        "date": "2026-03-21",
        "value": 2
      },
      {
        "date": "2026-03-22",
        "value": 1
      },
      {
        "date": "2026-03-23",
        "value": 3
      },
      {
        "date": "2026-03-24",
        "value": 1
      },
      {
        "date": "2026-03-25",
        "value": 3
      },
      {
        "date": "2026-03-26",
        "value": 1
      },
      {
        "date": "2026-03-27",
        "value": 0
      },
      {
        "date": "2026-03-28",
        "value": 3
      },
      {
        "date": "2026-03-29",
        "value": 0
      },
      {
        "date": "2026-03-30",
        "value": 3
      },
      {
        "date": "2026-03-31",
        "value": 3
      },
      {
        "date": "2026-04-01",
        "value": 2
      },
      {
        "date": "2026-04-02",
        "value": 1
      },
      {
        "date": "2026-04-03",
        "value": 0
      },
      {
        "date": "2026-04-04",
        "value": 3
      },
      {
        "date": "2026-04-05",
        "value": 2
      },
      {
        "date": "2026-04-06",
        "value": 1
      },
      {
        "date": "2026-04-07",
        "value": 2
      },
      {
        "date": "2026-04-08",
        "value": 2
      },
      {
        "date": "2026-04-09",
        "value": 0
      },
      {
        "date": "2026-04-10",
        "value": 3
      },
      {
        "date": "2026-04-11",
        "value": 0
      },
      {
        "date": "2026-04-12",
        "value": 0
      },
      {
        "date": "2026-04-13",
        "value": 3
      },
      {
        "date": "2026-04-14",
        "value": 1
      },
      {
        "date": "2026-04-15",
        "value": 3
      },
      {
        "date": "2026-04-16",
        "value": 1
      },
      {
        "date": "2026-04-17",
        "value": 0
      },
      {
        "date": "2026-04-18",
        "value": 1
      },
      {
        "date": "2026-04-19",
        "value": 2
      },
      {
        "date": "2026-04-20",
        "value": 3
      },
      {
        "date": "2026-04-21",
        "value": 1
      },
      {
        "date": "2026-04-22",
        "value": 1
      },
      {
        "date": "2026-04-23",
        "value": 2
      },
      {
        "date": "2026-04-24",
        "value": 0
      },
      {
        "date": "2026-04-25",
        "value": 0
      },
      {
        "date": "2026-04-26",
        "value": 0
      },
      {
        "date": "2026-04-27",
        "value": 3
      },
      {
        "date": "2026-04-28",
        "value": 3
      },
      {
        "date": "2026-04-29",
        "value": 3
      },
      {
        "date": "2026-04-30",
        "value": 3
      },
      {
        "date": "2026-05-01",
        "value": 3
      },
      {
        "date": "2026-05-02",
        "value": 1
      },
      {
        "date": "2026-05-03",
        "value": 0
      },
      {
        "date": "2026-05-04",
        "value": 2
      },
      {
        "date": "2026-05-05",
        "value": 3
      },
      {
        "date": "2026-05-06",
        "value": 0
      },
      {
        "date": "2026-05-07",
        "value": 3
      },
      {
        "date": "2026-05-08",
        "value": 1
      },
      {
        "date": "2026-05-09",
        "value": 3
      },
      {
        "date": "2026-05-10",
        "value": 0
      },
      {
        "date": "2026-05-11",
        "value": 3
      },
      {
        "date": "2026-05-12",
        "value": 2
      },
      {
        "date": "2026-05-13",
        "value": 2
      },
      {
        "date": "2026-05-14",
        "value": 0
      },
      {
        "date": "2026-05-15",
        "value": 1
      },
      {
        "date": "2026-05-16",
        "value": 0
      },
      {
        "date": "2026-05-17",
        "value": 3
      },
      {
        "date": "2026-05-18",
        "value": 3
      },
      {
        "date": "2026-05-19",
        "value": 3
      },
      {
        "date": "2026-05-20",
        "value": 0
      },
      {
        "date": "2026-05-21",
        "value": 3
      },
      {
        "date": "2026-05-22",
        "value": 2
      },
      {
        "date": "2026-05-23",
        "value": 2
      },
      {
        "date": "2026-05-24",
        "value": 3
      },
      {
        "date": "2026-05-25",
        "value": 2
      },
      {
        "date": "2026-05-26",
        "value": 1
      },
      {
        "date": "2026-05-27",
        "value": 3
      },
      {
        "date": "2026-05-28",
        "value": 3
      },
      {
        "date": "2026-05-29",
        "value": 3
      },
      {
        "date": "2026-05-30",
        "value": 2
      },
      {
        "date": "2026-05-31",
        "value": 1
      },
      {
        "date": "2026-06-01",
        "value": 3
      },
      {
        "date": "2026-06-02",
        "value": 2
      },
      {
        "date": "2026-06-03",
        "value": 0
      },
      {
        "date": "2026-06-04",
        "value": 0
      },
      {
        "date": "2026-06-05",
        "value": 2
      },
      {
        "date": "2026-06-06",
        "value": 0
      },
      {
        "date": "2026-06-07",
        "value": 0
      },
      {
        "date": "2026-06-08",
        "value": 2
      },
      {
        "date": "2026-06-09",
        "value": 3
      },
      {
        "date": "2026-06-10",
        "value": 1
      },
      {
        "date": "2026-06-11",
        "value": 2
      },
      {
        "date": "2026-06-12",
        "value": 2
      },
      {
        "date": "2026-06-13",
        "value": 2
      },
      {
        "date": "2026-06-14",
        "value": 0
      },
      {
        "date": "2026-06-15",
        "value": 3
      },
      {
        "date": "2026-06-16",
        "value": 2
      },
      {
        "date": "2026-06-17",
        "value": 1
      },
      {
        "date": "2026-06-18",
        "value": 3
      },
      {
        "date": "2026-06-19",
        "value": 3
      },
      {
        "date": "2026-06-20",
        "value": 0
      },
      {
        "date": "2026-06-21",
        "value": 3
      },
      {
        "date": "2026-06-22",
        "value": 0
      },
      {
        "date": "2026-06-23",
        "value": 3
      },
      {
        "date": "2026-06-24",
        "value": 0
      },
      {
        "date": "2026-06-25",
        "value": 1
      },
      {
        "date": "2026-06-26",
        "value": 3
      },
      {
        "date": "2026-06-27",
        "value": 0
      },
      {
        "date": "2026-06-28",
        "value": 0
      },
      {
        "date": "2026-06-29",
        "value": 2
      },
      {
        "date": "2026-06-30",
        "value": 0
      },
      {
        "date": "2026-07-01",
        "value": 3
      },
      {
        "date": "2026-07-02",
        "value": 2
      },
      {
        "date": "2026-07-03",
        "value": 0
      },
      {
        "date": "2026-07-04",
        "value": 2
      },
      {
        "date": "2026-07-05",
        "value": 0
      },
      {
        "date": "2026-07-06",
        "value": 0
      },
      {
        "date": "2026-07-07",
        "value": 1
      },
      {
        "date": "2026-07-08",
        "value": 0
      },
      {
        "date": "2026-07-09",
        "value": 2
      },
      {
        "date": "2026-07-10",
        "value": 3
      },
      {
        "date": "2026-07-11",
        "value": 2
      },
      {
        "date": "2026-07-12",
        "value": 1
      },
      {
        "date": "2026-07-13",
        "value": 0
      },
      {
        "date": "2026-07-14",
        "value": 2
      },
      {
        "date": "2026-07-15",
        "value": 1
      },
      {
        "date": "2026-07-16",
        "value": 3
      },
      {
        "date": "2026-07-17",
        "value": 0
      },
      {
        "date": "2026-07-18",
        "value": 3
      },
      {
        "date": "2026-07-19",
        "value": 3
      },
      {
        "date": "2026-07-20",
        "value": 2
      },
      {
        "date": "2026-07-21",
        "value": 1
      },
      {
        "date": "2026-07-22",
        "value": 0
      },
      {
        "date": "2026-07-23",
        "value": 3
      },
      {
        "date": "2026-07-24",
        "value": 2
      },
      {
        "date": "2026-07-25",
        "value": 1
      },
      {
        "date": "2026-07-26",
        "value": 2
      },
      {
        "date": "2026-07-27",
        "value": 2
      },
      {
        "date": "2026-07-28",
        "value": 0
      },
      {
        "date": "2026-07-29",
        "value": 2
      },
      {
        "date": "2026-07-30",
        "value": 3
      },
      {
        "date": "2026-07-31",
        "value": 3
      },
      {
        "date": "2026-08-01",
        "value": 0
      },
      {
        "date": "2026-08-02",
        "value": 0
      },
      {
        "date": "2026-08-03",
        "value": 2
      },
      {
        "date": "2026-08-04",
        "value": 1
      },
      {
        "date": "2026-08-05",
        "value": 3
      },
      {
        "date": "2026-08-06",
        "value": 2
      },
      {
        "date": "2026-08-07",
        "value": 3
      },
      {
        "date": "2026-08-08",
        "value": 3
      },
      {
        "date": "2026-08-09",
        "value": 3
      },
      {
        "date": "2026-08-10",
        "value": 1
      },
      {
        "date": "2026-08-11",
        "value": 1
      },
      {
        "date": "2026-08-12",
        "value": 0
      },
      {
        "date": "2026-08-13",
        "value": 3
      },
      {
        "date": "2026-08-14",
        "value": 2
      },
      {
        "date": "2026-08-15",
        "value": 2
      },
      {
        "date": "2026-08-16",
        "value": 0
      },
      {
        "date": "2026-08-17",
        "value": 1
      },
      {
        "date": "2026-08-18",
        "value": 2
      },
      {
        "date": "2026-08-19",
        "value": 2
      },
      {
        "date": "2026-08-20",
        "value": 3
      },
      {
        "date": "2026-08-21",
        "value": 1
      },
      {
        "date": "2026-08-22",
        "value": 1
      },
      {
        "date": "2026-08-23",
        "value": 3
      },
      {
        "date": "2026-08-24",
        "value": 3
      },
      {
        "date": "2026-08-25",
        "value": 3
      },
      {
        "date": "2026-08-26",
        "value": 3
      },
      {
        "date": "2026-08-27",
        "value": 2
      },
      {
        "date": "2026-08-28",
        "value": 1
      },
      {
        "date": "2026-08-29",
        "value": 2
      },
      {
        "date": "2026-08-30",
        "value": 1
      },
      {
        "date": "2026-08-31",
        "value": 1
      },
      {
        "date": "2026-09-01",
        "value": 1
      },
      {
        "date": "2026-09-02",
        "value": 3
      },
      {
        "date": "2026-09-03",
        "value": 0
      },
      {
        "date": "2026-09-04",
        "value": 2
      },
      {
        "date": "2026-09-05",
        "value": 1
      },
      {
        "date": "2026-09-06",
        "value": 1
      },
      {
        "date": "2026-09-07",
        "value": 0
      },
      {
        "date": "2026-09-08",
        "value": 1
      },
      {
        "date": "2026-09-09",
        "value": 1
      },
      {
        "date": "2026-09-10",
        "value": 0
      },
      {
        "date": "2026-09-11",
        "value": 3
      },
      {
        "date": "2026-09-12",
        "value": 0
      },
      {
        "date": "2026-09-13",
        "value": 1
      },
      {
        "date": "2026-09-14",
        "value": 3
      },
      {
        "date": "2026-09-15",
        "value": 0
      },
      {
        "date": "2026-09-16",
        "value": 3
      },
      {
        "date": "2026-09-17",
        "value": 3
      },
      {
        "date": "2026-09-18",
        "value": 3
      },
      {
        "date": "2026-09-19",
        "value": 2
      },
      {
        "date": "2026-09-20",
        "value": 1
      },
      {
        "date": "2026-09-21",
        "value": 1
      },
      {
        "date": "2026-09-22",
        "value": 0
      },
      {
        "date": "2026-09-23",
        "value": 0
      },
      {
        "date": "2026-09-24",
        "value": 1
      },
      {
        "date": "2026-09-25",
        "value": 2
      },
      {
        "date": "2026-09-26",
        "value": 0
      },
      {
        "date": "2026-09-27",
        "value": 2
      },
      {
        "date": "2026-09-28",
        "value": 2
      },
      {
        "date": "2026-09-29",
        "value": 0
      },
      {
        "date": "2026-09-30",
        "value": 0
      },
      {
        "date": "2026-10-01",
        "value": 2
      },
      {
        "date": "2026-10-02",
        "value": 1
      },
      {
        "date": "2026-10-03",
        "value": 1
      },
      {
        "date": "2026-10-04",
        "value": 3
      },
      {
        "date": "2026-10-05",
        "value": 3
      },
      {
        "date": "2026-10-06",
        "value": 1
      },
      {
        "date": "2026-10-07",
        "value": 3
      },
      {
        "date": "2026-10-08",
        "value": 0
      },
      {
        "date": "2026-10-09",
        "value": 0
      },
      {
        "date": "2026-10-10",
        "value": 2
      },
      {
        "date": "2026-10-11",
        "value": 0
      },
      {
        "date": "2026-10-12",
        "value": 1
      },
      {
        "date": "2026-10-13",
        "value": 1
      },
      {
        "date": "2026-10-14",
        "value": 2
      },
      {
        "date": "2026-10-15",
        "value": 1
      },
      {
        "date": "2026-10-16",
        "value": 1
      },
      {
        "date": "2026-10-17",
        "value": 2
      },
      {
        "date": "2026-10-18",
        "value": 3
      },
      {
        "date": "2026-10-19",
        "value": 3
      },
      {
        "date": "2026-10-20",
        "value": 0
      },
      {
        "date": "2026-10-21",
        "value": 2
      },
      {
        "date": "2026-10-22",
        "value": 1
      },
      {
        "date": "2026-10-23",
        "value": 3
      },
      {
        "date": "2026-10-24",
        "value": 3
      },
      {
        "date": "2026-10-25",
        "value": 3
      },
      {
        "date": "2026-10-26",
        "value": 1
      },
      {
        "date": "2026-10-27",
        "value": 2
      },
      {
        "date": "2026-10-28",
        "value": 2
      },
      {
        "date": "2026-10-29",
        "value": 3
      },
      {
        "date": "2026-10-30",
        "value": 0
      },
      {
        "date": "2026-10-31",
        "value": 2
      },
      {
        "date": "2026-11-01",
        "value": 1
      },
      {
        "date": "2026-11-02",
        "value": 1
      },
      {
        "date": "2026-11-03",
        "value": 0
      },
      {
        "date": "2026-11-04",
        "value": 2
      },
      {
        "date": "2026-11-05",
        "value": 2
      },
      {
        "date": "2026-11-06",
        "value": 2
      },
      {
        "date": "2026-11-07",
        "value": 1
      },
      {
        "date": "2026-11-08",
        "value": 0
      },
      {
        "date": "2026-11-09",
        "value": 3
      },
      {
        "date": "2026-11-10",
        "value": 3
      },
      {
        "date": "2026-11-11",
        "value": 3
      },
      {
        "date": "2026-11-12",
        "value": 2
      },
      {
        "date": "2026-11-13",
        "value": 3
      },
      {
        "date": "2026-11-14",
        "value": 1
      },
      {
        "date": "2026-11-15",
        "value": 0
      },
      {
        "date": "2026-11-16",
        "value": 2
      },
      {
        "date": "2026-11-17",
        "value": 2
      },
      {
        "date": "2026-11-18",
        "value": 0
      },
      {
        "date": "2026-11-19",
        "value": 1
      },
      {
        "date": "2026-11-20",
        "value": 1
      },
      {
        "date": "2026-11-21",
        "value": 3
      },
      {
        "date": "2026-11-22",
        "value": 3
      },
      {
        "date": "2026-11-23",
        "value": 3
      },
      {
        "date": "2026-11-24",
        "value": 1
      },
      {
        "date": "2026-11-25",
        "value": 0
      },
      {
        "date": "2026-11-26",
        "value": 1
      },
      {
        "date": "2026-11-27",
        "value": 2
      },
      {
        "date": "2026-11-28",
        "value": 0
      },
      {
        "date": "2026-11-29",
        "value": 2
      },
      {
        "date": "2026-11-30",
        "value": 2
      },
      {
        "date": "2026-12-01",
        "value": 0
      },
      {
        "date": "2026-12-02",
        "value": 1
      },
      {
        "date": "2026-12-03",
        "value": 3
      },
      {
        "date": "2026-12-04",
        "value": 3
      },
      {
        "date": "2026-12-05",
        "value": 2
      },
      {
        "date": "2026-12-06",
        "value": 3
      },
      {
        "date": "2026-12-07",
        "value": 0
      },
      {
        "date": "2026-12-08",
        "value": 3
      },
      {
        "date": "2026-12-09",
        "value": 3
      },
      {
        "date": "2026-12-10",
        "value": 3
      },
      {
        "date": "2026-12-11",
        "value": 1
      },
      {
        "date": "2026-12-12",
        "value": 3
      },
      {
        "date": "2026-12-13",
        "value": 3
      },
      {
        "date": "2026-12-14",
        "value": 0
      },
      {
        "date": "2026-12-15",
        "value": 3
      },
      {
        "date": "2026-12-16",
        "value": 2
      },
      {
        "date": "2026-12-17",
        "value": 0
      },
      {
        "date": "2026-12-18",
        "value": 2
      },
      {
        "date": "2026-12-19",
        "value": 3
      },
      {
        "date": "2026-12-20",
        "value": 2
      },
      {
        "date": "2026-12-21",
        "value": 0
      },
      {
        "date": "2026-12-22",
        "value": 2
      },
      {
        "date": "2026-12-23",
        "value": 1
      },
      {
        "date": "2026-12-24",
        "value": 3
      },
      {
        "date": "2026-12-25",
        "value": 3
      },
      {
        "date": "2026-12-26",
        "value": 2
      },
      {
        "date": "2026-12-27",
        "value": 1
      },
      {
        "date": "2026-12-28",
        "value": 3
      },
      {
        "date": "2026-12-29",
        "value": 1
      },
      {
        "date": "2026-12-30",
        "value": 1
      }
    ]
  },
  "color": {
    "name": "Green",
    "style": "Gradient"
  }
}
```

### More examples

To see one in full, add `?variant=<id>` to the example request above — it returns that example's complete response.

- `contributions-contrast` — **Activity grid on a contrast card.** An activity history lifted out of the board on a contrast card.
- `contributions-multi-header` — **Activity grid with a header strip.** A per-day activity heatmap that should also state the total, the current streak and the best day.

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/contributions-grid.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown` for a change, `Star` for a rating
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Sending `value` as a string — cell values must be numbers.
- Using a negative `value` — intensities must be `>= 0` (0 renders as the empty cell).
- Sending fewer than one cell — `cells` must contain at least one `{ date, value }` entry.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
