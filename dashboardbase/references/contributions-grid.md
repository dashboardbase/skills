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

### Activity grid with a header strip

A per-day activity heatmap that should also state the total, the current streak and the best day.

```json
{
  "title": "Deploys",
  "actions": [
    {
      "title": "Open pipeline",
      "type": "link",
      "url": "https://example.com/pipeline"
    }
  ],
  "data": {
    "header": {
      "title": "142",
      "subtitle": "Last 14 days",
      "badge": {
        "text": "+18%",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "cells": [
      {
        "date": "2026-01-01",
        "value": 4
      },
      {
        "date": "2026-01-02",
        "value": 0
      },
      {
        "date": "2026-01-03",
        "value": 6
      },
      {
        "date": "2026-01-04",
        "value": 9
      },
      {
        "date": "2026-01-05",
        "value": 12
      },
      {
        "date": "2026-01-06",
        "value": 8
      },
      {
        "date": "2026-01-07",
        "value": 14
      },
      {
        "date": "2026-01-08",
        "value": 11
      },
      {
        "date": "2026-01-09",
        "value": 17
      },
      {
        "date": "2026-01-10",
        "value": 21
      },
      {
        "date": "2026-01-11",
        "value": 13
      },
      {
        "date": "2026-01-12",
        "value": 9
      },
      {
        "date": "2026-01-13",
        "value": 10
      },
      {
        "date": "2026-01-14",
        "value": 8
      }
    ],
    "headers": [
      {
        "title": "9 days",
        "subtitle": "Current streak"
      },
      {
        "title": "21",
        "subtitle": "Best day"
      }
    ]
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/contributions-grid.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown`
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Sending `value` as a string — cell values must be numbers.
- Using a negative `value` — intensities must be `>= 0` (0 renders as the empty cell).
- Sending fewer than one cell — `cells` must contain at least one `{ date, value }` entry.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
