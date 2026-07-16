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
    "target": "2026-12-31T23:59:59+00:00",
    "label": "until launch"
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/countdown'
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
