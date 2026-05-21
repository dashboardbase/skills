# Table widget

> **Load this file when:** building a table / list / rows-and-columns widget.

## Purpose

Render rows of structured data with optional column headers — ideal for top-N lists, ranked items, status grids, recent activity.

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract):

```json
{
  "required": [
    "rows"
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
    "headers": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "text": {
            "type": "string",
            "nullable": true
          },
          "width": {
            "type": "integer",
            "format": "int32",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "nullable": true
    },
    "rows": {
      "type": "array",
      "items": {
        "type": "array",
        "items": {
          "type": "object",
          "properties": {
            "text": {
              "type": "string",
              "nullable": true
            },
            "imageUrl": {
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
            "link": {
              "type": "string",
              "nullable": true
            }
          },
          "additionalProperties": false
        }
      }
    }
  },
  "additionalProperties": false
}
```

## Example response

```json
{
  "title": "Tasks",
  "actions": [
    {
      "title": "View All Tasks",
      "type": "link",
      "url": "https://example.com/all-tasks"
    }
  ],
  "data": {
    "headers": [
      {
        "text": "Name",
        "width": 30
      },
      {
        "text": "Status",
        "width": 30
      },
      {
        "text": "Progress",
        "width": 40
      }
    ],
    "rows": [
      [
        {
          "text": "Task A"
        },
        {
          "text": "Completed",
          "badge": {
            "text": "Done",
            "icon": "ArrowUp",
            "color": "Success"
          }
        },
        {
          "text": "100%"
        }
      ],
      [
        {
          "text": "Task B"
        },
        {
          "text": "In Progress",
          "badge": {
            "text": "Working",
            "color": "Warning"
          }
        },
        {
          "text": "50%"
        }
      ],
      [
        {
          "text": "Task C"
        },
        {
          "text": "Failed",
          "badge": {
            "text": "Done",
            "icon": "ArrowDown",
            "color": "Danger",
            "fill": "Solid"
          }
        },
        {
          "text": "0%"
        }
      ],
      [
        {
          "text": "Task D"
        },
        {
          "text": "Completed",
          "badge": {
            "text": "Done",
            "color": "Success",
            "fill": "Outline"
          }
        },
        {
          "text": "100%"
        }
      ]
    ]
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/tablechart'
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

- Rows is `List<List<Column>>` (a 2-D array), not a list of objects — each row is an array of column cells.
- Column widths in `headers` must sum to ~100 (they're treated as percentages).
- Putting a URL in `text` instead of `link` — set `link` for clickable cells; `text` is the visible label.
- Using `imageUrl` for inline icons — it renders as an image; for badges use `badge`.
