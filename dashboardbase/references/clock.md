# Clock widget

> **Load this file when:** building a clock / current-time / timezone tile.

## Purpose

Show the current time and date for a given timezone — a kiosk/TV-wall staple. The endpoint only states the timezone and formatting; the ticking clock is rendered client-side, so no polling is needed. Set the optional `mode` to `Digital` for a seven-segment display or `Analog` for a rendered clock face; omit it or send `Text` for the plain typographic time.

In a setup file, this widget's `type` is `clock` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/clock.json` for use with a JSON Schema validator:

```json
{
  "type": "object",
  "properties": {
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
    },
    "mode": {
      "enum": [
        "Text",
        "Digital",
        "Analog"
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
  "title": "Copenhagen",
  "data": {
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

### Digital clock

A seven-segment digital display for a control room or NOC wall.

```json
{
  "title": "London",
  "data": {
    "timeZone": "Europe/London",
    "hourCycle": 24,
    "showDate": true,
    "label": "GMT",
    "mode": "Digital"
  }
}
```

### Analog clock

A rendered clock face for a lobby or reception display.

```json
{
  "title": "Tokyo",
  "data": {
    "timeZone": "Asia/Tokyo",
    "hourCycle": 12,
    "showDate": false,
    "label": "JST",
    "mode": "Analog"
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/clock.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

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
- Sending a `mode` outside `Text` / `Digital` / `Analog` — it is nullable, so omit it for the default (text) rendering.
