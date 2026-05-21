# Status widget

> **Load this file when:** building a status / health / up-or-down indicator.

## Purpose

Show a binary health indicator (Ok / Error) with optional header context — ideal for service status, deploy state, alert summaries.

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract):

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
          "format": "int32",
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
              "format": "int32",
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
              "format": "int32",
              "nullable": true
            },
            "fill": {
              "enum": [
                "Solid",
                "Clear",
                "Outline"
              ],
              "type": "string",
              "format": "int32",
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
          "format": "int32",
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
          "format": "int32",
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
      "type": "string",
      "format": "int32"
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



## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. JSON-validate your response against the resolved schema before declaring done.

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
