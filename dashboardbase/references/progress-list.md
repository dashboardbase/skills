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
        "label": "Postgres",
        "color": "Blue"
      },
      {
        "value": 340,
        "max": 500,
        "label": "Blob",
        "color": "Green"
      },
      {
        "value": 470,
        "max": 500,
        "label": "Redis",
        "color": "Danger"
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
        "label": "Sales",
        "color": "Success"
      },
      {
        "value": 45,
        "max": 100,
        "label": "Marketing",
        "color": "Warning"
      },
      {
        "value": 63,
        "max": 100,
        "label": "Support",
        "color": "Blue"
      }
    ]
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate your response's `data` field against `assets/schemas/progress-list.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

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
