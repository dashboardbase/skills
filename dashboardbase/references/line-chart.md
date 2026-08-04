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

## Multiple series

The canonical example shows a single dataset because colors and multi-series are optional. To draw multiple lines, pass several entries in `datasets`; each has its own `label` and an optional `color`. `labels` is shared across all datasets. Set `fill: true` to fill the area beneath each line.

```json
{
  "labels": ["Mon", "Tue"],
  "datasets": [
    { "label": "Page Views", "color": "Blue", "data": [{"value": 100}, {"value": 120}] },
    { "label": "Unique Visitors", "color": "Green", "data": [{"value": 40}, {"value": 55}] }
  ]
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

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/line-chart.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown`
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Treating `labels` as datetime stamps — Dashboardbase renders them as opaque strings; format them upstream (e.g. `'2026-05-19'` or `'Mon'`).
- Sending uneven dataset lengths — every dataset's `data` length should match `labels`.
- Returning `null` between values to indicate a gap — Dashboardbase expects every index to have a `{ "value": <number> }` entry.
- Returning `204` (or an empty series) for a period with no activity — emit every bucket in the window with `{ "value": 0 }` instead. A flat zero line is the honest answer and reads as intentional; a blank widget reads as broken.
- Leaving axis ticks on for a long daily series — thirty rotated date labels crowd the axis and swamp the plot. Set `ticksX: false` past roughly a dozen buckets and let `header.subtitle` carry the window ("Last 30 days").
