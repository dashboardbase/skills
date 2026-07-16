# Countdown widget

> **Load this file when:** building a countdown / days-until / time-remaining tile.

## Purpose

Count down to (or up from) a target timestamp — ideal for launches, deadlines, quarter-end, or a 'days since last incident' streak. The endpoint returns the target instant; the ticking is client-side, so no polling is needed to stay live.

In a setup file, this widget's `type` is `countdown` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/countdown.json` for use with a JSON Schema validator:

```json
{
  "required": [
    "target"
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
    "target": {
      "type": "string",
      "format": "date-time"
    },
    "label": {
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
  "title": "Launch day",
  "actions": [
    {
      "title": "View Roadmap",
      "type": "link",
      "url": "https://example.com/roadmap"
    }
  ],
  "data": {
    "header": {
      "title": "Public launch",
      "subtitle": "v1.0 goes live"
    },
    "target": "2026-12-31T23:59:59+00:00",
    "label": "until launch"
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/countdown'
```

## Header — headline, subtitle, and colored badge

The `header` block is optional in the schema — **include it anyway**. Without it the widget renders as a bare plot; the header is what makes it read well at a glance. Use the three parts together:

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

### Days since last incident

A streak that counts up from a past moment — days since the last incident/outage.

```json
{
  "title": "Reliability",
  "data": {
    "header": {
      "title": "Days since last incident"
    },
    "target": "2026-01-01T00:00:00+00:00",
    "label": "since last incident"
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate your response's `data` field against `assets/schemas/countdown.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown`
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Sending `target` as a local date without an offset — use an ISO-8601 instant (e.g. `2026-01-01T00:00:00Z`) so every viewer counts down to the same moment.
- Trying to send the remaining days/hours as the value — return the `target` timestamp and let the client tick.
- Omitting `target` — it is required.
