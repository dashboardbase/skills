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
    "url": {
      "minLength": 1,
      "type": "string"
    },
    "alt": {
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
    "url": "https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExbnJpeXpybnFkY2ptN2Q1ZGF0dnVqMTl0Z250MG0wdHpnN21qbHIyeCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/GbH8vRmrNHdVZhouBt/giphy.gif",
    "alt": "Celebration GIF"
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/imagechart'
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
