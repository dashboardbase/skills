# Image widget

> **Load this file when:** building an image / meme / GIF / picture tile.

## Purpose

Show an external image, meme or GIF by URL — ideal for team-culture dashboards. Dashboardbase serves the image through a hardened server-side proxy (`/bff/v1/widget-image?url=...`) so the viewer's browser never contacts the origin directly; return the original HTTPS `url` here and the app rewrites it to the proxy.

In a setup file, this widget's `type` is `image` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/image.json` for use with a JSON Schema validator:

```json
{
  "required": [
    "url"
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
    "url": {
      "minLength": 1,
      "type": "string"
    },
    "alt": {
      "type": "string",
      "nullable": true
    },
    "caption": {
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
  "title": "Ship it",
  "data": {
    "header": {
      "title": "Release vibes"
    },
    "url": "https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExbnJpeXpybnFkY2ptN2Q1ZGF0dnVqMTl0Z250MG0wdHpnN21qbHIyeCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/GbH8vRmrNHdVZhouBt/giphy.gif",
    "alt": "Celebration GIF",
    "caption": "We shipped v1.0 \uD83D\uDE80"
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/imagechart'
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





## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate your response's `data` field against `assets/schemas/image.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown`
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Sending a non-HTTPS `url` — only `https://` image URLs are accepted.
- Pointing `url` at an SVG — the proxy serves only raster images (png, jpeg, gif, webp); SVG is rejected for safety.
- Expecting a URL that redirects (302) to a CDN to work — the proxy does not follow redirects; use the final image URL.
