# Status widget

> **Load this file when:** building a status / health / up-or-down indicator.

## Purpose

Show a binary health indicator (Ok / Error) with optional header context — ideal for service status, deploy state, alert summaries. Set the optional `mode` to `Heartbeat` to have the client render the indicator as an ECG-style pulse that beats (a healthy service visibly 'lives' on a TV wall); omit it or send `Default` for the plain Ok/Error pill.

In a setup file, this widget's `type` is `status` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/status.json` for use with a JSON Schema validator:

```json
{
  "required": [
    "status"
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
    "status": {
      "enum": [
        "Ok",
        "Error"
      ],
      "type": "string"
    },
    "mode": {
      "enum": [
        "Default",
        "Heartbeat"
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
  "title": "Status",
  "actions": [
    {
      "title": "Check Logs",
      "type": "link",
      "url": "https://example.com/status-logs"
    }
  ],
  "data": {
    "status": "Ok"
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/statuschart'
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

### Healthy heartbeat (pulsing)

A healthy service you want to visibly 'beat' on a TV wall — set `mode` to `Heartbeat`.

```json
{
  "title": "API health",
  "actions": [
    {
      "title": "Check Logs",
      "type": "link",
      "url": "https://example.com/status-logs"
    }
  ],
  "data": {
    "header": {
      "title": "Operational",
      "subtitle": "All checks passing",
      "color": "Success"
    },
    "status": "Ok",
    "mode": "Heartbeat"
  }
}
```

### Unhealthy heartbeat (flatlining)

A failing service you still want to render as an ECG pulse on a TV wall — set `mode` to `Heartbeat` with an `Error` status so the beat flatlines.

```json
{
  "title": "API health",
  "actions": [
    {
      "title": "Check Logs",
      "type": "link",
      "url": "https://example.com/status-logs"
    }
  ],
  "data": {
    "header": {
      "title": "Not operational",
      "subtitle": "All checks failing",
      "color": "Danger"
    },
    "status": "Error",
    "mode": "Heartbeat"
  }
}
```

### Failing (with header context)

A service is down/erroring — pair the Error indicator with a header naming the service.

```json
{
  "title": "Payments API",
  "actions": [
    {
      "title": "Check Logs",
      "type": "link",
      "url": "https://example.com/status-logs"
    }
  ],
  "data": {
    "header": {
      "title": "Degraded",
      "subtitle": "3 failing checks",
      "color": "Danger"
    },
    "status": "Error"
  }
}
```

### Outage with a critical alert

A service is fully down — pair the Error indicator with a critical alert spelling out the impact.

```json
{
  "title": "Payments API",
  "actions": [
    {
      "title": "Open Incident",
      "type": "link",
      "url": "https://example.com/incident"
    }
  ],
  "data": {
    "header": {
      "title": "Down",
      "subtitle": "All checks failing",
      "color": "Danger"
    },
    "status": "Error"
  },
  "alert": {
    "active": true,
    "level": "critical",
    "message": "Payments API has been down for 8 minutes"
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate your response's `data` field against `assets/schemas/status.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown`
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Sending a string for `status` outside `Ok` / `Error` — values are case-sensitive and limited to the `WidgetStatusIndicator` enum.
- Using KPI shape for a binary indicator — Status renders an explicit Ok/Error pill, KPI does not.
- Sending a `mode` outside `Default` / `Heartbeat` — it is nullable, so omit it for the default rendering.
