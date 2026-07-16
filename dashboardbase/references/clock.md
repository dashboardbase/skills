# Clock widget

> **Load this file when:** building a clock / current-time / timezone tile.

## Purpose

Show the current time and date for a given timezone — a kiosk/TV-wall staple. The endpoint only states the timezone and formatting; the ticking clock is rendered client-side, so no polling is needed.

In a setup file, this widget's `type` is `clock` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/clock.json` for use with a JSON Schema validator:

```json
{
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
    "timeZone": {
      "type": "string",
      "nullable": true
    },
    "hourCycle": {
      "type": "integer",
      "format": "int32",
      "nullable": true
    },
    "showDate": {
      "type": "boolean",
      "nullable": true
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
  "title": "Copenhagen",
  "data": {
    "header": {
      "title": "HQ"
    },
    "timeZone": "Europe/Copenhagen",
    "hourCycle": 24,
    "showDate": true,
    "label": "CET"
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/clock'
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

### Remote-team clock (12h)

Showing a second timezone for a distributed team, in 12-hour format.

```json
{
  "title": "New York",
  "data": {
    "timeZone": "America/New_York",
    "hourCycle": 12,
    "showDate": false,
    "label": "ET"
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate your response's `data` field against `assets/schemas/clock.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown`
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Sending a Windows/abbreviated zone (e.g. `PST`) — use an IANA timezone id such as `Europe/Copenhagen` or `America/New_York`.
- Sending `hourCycle` other than `12` or `24`.
- Returning a formatted time string — return the timezone/format and let the client keep it ticking.
