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
              "Dark"
            ],
            "type": "string",
            "nullable": true
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

### Bars under a header strip

A list of capacity bars that should be read against the aggregate — total used and remaining above the rows.

```json
{
  "title": "Storage by service",
  "actions": [
    {
      "title": "Open storage",
      "type": "link",
      "url": "https://example.com/storage"
    }
  ],
  "data": {
    "header": {
      "title": "1.8 TB",
      "subtitle": "Used",
      "badge": {
        "text": "72%",
        "color": "Warning"
      }
    },
    "items": [
      {
        "value": 820,
        "max": 1000,
        "label": "Media"
      },
      {
        "value": 640,
        "max": 1000,
        "label": "Backups"
      },
      {
        "value": 340,
        "max": 500,
        "label": "Logs"
      }
    ],
    "headers": [
      {
        "title": "700 GB",
        "subtitle": "Free"
      },
      {
        "title": "2.5 TB",
        "subtitle": "Quota"
      }
    ]
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/progress-list.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown`
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Leaving a row's `label` empty — every bar in the list must be labelled.
- Sending `value` greater than `max`, or a `max` of `0` — when `max` is set it must be `> 0` and `0 <= value <= max`.
- Using this for a single bar — a single goal bar belongs on the KPI widget's `progress` field.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
