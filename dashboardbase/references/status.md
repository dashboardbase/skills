# Status widget

> **Load this file when:** building a status / health / up-or-down indicator.

## Purpose

Show a health indicator (Ok / Warning / Error) with optional header context — ideal for service status, deploy state, alert summaries. `Warning` is the degraded state between the two: something is slow or partly failing but the service is up. Set the optional `mode` to choose how the indicator is drawn: omit it or send `Default` for the plain status dot, `Heartbeat` for an ECG-style pulse that beats, or one of the illustrated modes — `Flower`, `Coffee`, `Weather`, `RubberDuck`, `Rocket`, `OpenSign`, `DeliveryVan`, `ShoppingBag` — an outlined, animated object that reacts to the status (a flower that droops when degraded and wilts when down, a van that slows and then parks with its hazard lights on), made for a one-widget board on a TV. Illustrated modes show a short sentence under the object: omit `caption` for the built-in sentence for that mode and status, send your own text to replace it, or send an empty string to show no sentence. Add an optional `legend` to list the items behind the overall status under the indicator — one entry per service or check, each with its own `color` (`Success`, `Warning`, `Danger`, …) and an optional `value` — styled like a chart legend.

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
                "ArrowDown",
                "Star"
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
                "Dark",
                "Gray",
                "Gray1",
                "Gray3",
                "Gray4"
              ],
              "type": "string",
              "nullable": true
            },
            "fill": {
              "enum": [
                "Solid",
                "Clear",
                "Outline",
                "Dot"
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
            "Dark",
            "Gray",
            "Gray1",
            "Gray3",
            "Gray4"
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
        },
        "unit": {
          "maxLength": 24,
          "type": "string",
          "nullable": true
        },
        "label": {
          "maxLength": 40,
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
        "Error",
        "Warning"
      ],
      "type": "string",
      "x-placeholder": true
    },
    "mode": {
      "enum": [
        "Default",
        "Heartbeat",
        "Flower",
        "Coffee",
        "Weather",
        "RubberDuck",
        "Rocket",
        "OpenSign",
        "DeliveryVan",
        "ShoppingBag"
      ],
      "type": "string",
      "nullable": true
    },
    "headers": {
      "maxItems": 6,
      "type": "array",
      "items": {
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
                  "ArrowDown",
                  "Star"
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
                  "Dark",
                  "Gray",
                  "Gray1",
                  "Gray3",
                  "Gray4"
                ],
                "type": "string",
                "nullable": true
              },
              "fill": {
                "enum": [
                  "Solid",
                  "Clear",
                  "Outline",
                  "Dot"
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
              "Dark",
              "Gray",
              "Gray1",
              "Gray3",
              "Gray4"
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
          },
          "unit": {
            "maxLength": 24,
            "type": "string",
            "nullable": true
          },
          "label": {
            "maxLength": 40,
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "nullable": true
    },
    "legend": {
      "maxItems": 24,
      "type": "array",
      "items": {
        "required": [
          "label"
        ],
        "type": "object",
        "properties": {
          "label": {
            "maxLength": 60,
            "minLength": 1,
            "type": "string"
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
              "Dark",
              "Gray",
              "Gray1",
              "Gray3",
              "Gray4"
            ],
            "type": "string",
            "nullable": true
          },
          "value": {
            "maxLength": 24,
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      },
      "nullable": true
    },
    "caption": {
      "maxLength": 80,
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

### More than one header — the header strip

`headers` takes an array of the same block, so one endpoint can carry several headline numbers
above the widget instead of needing a separate KPI widget for each. `header` is merged in **first**,
so `header` plus `headers` is one list: send only `header`, only `headers`, or both.

The strip lays its blocks out in **equal columns**, and `header` plus `headers` may total at most
**6** — one per pair of grid columns. A seventh is a validation error, not a silent truncation. On a
phone the strip wraps to two per row and the widget grows taller to fit, so a six-block strip stays
readable there too.

```json
{
  "header": { "title": "395", "subtitle": "Visitors", "badge": { "text": "+348.9%", "icon": "ArrowUp", "color": "Success" } },
  "headers": [
    { "title": "932", "subtitle": "New visitors", "badge": { "text": "+565.7%", "icon": "ArrowUp", "color": "Success" } },
    { "title": "1m 50s", "subtitle": "Avg. engagement" },
    { "title": "150K", "subtitle": "Total visitors" }
  ]
}
```

Every block in `headers` needs a `title` — an entry without one renders as an empty column, so it is
rejected.

## Legend

**A health board with a legend.** The indicator is the overall verdict; `legend` names what it is made of. Give each entry a `label`, a `color` for its own state and, optionally, a short `value` (a latency, an uptime). Omit `color` for a neutral entry.

```json
{
  "status": "Error",
  "header": { "title": "Degraded", "subtitle": "1 of 4 services failing" },
  "legend": [
    { "label": "API", "color": "Success", "value": "112 ms" },
    { "label": "Database", "color": "Success", "value": "8 ms" },
    { "label": "Queue", "color": "Warning", "value": "1.2k jobs" },
    { "label": "Payments", "color": "Danger", "value": "Down" }
  ]
}
```

Give the widget the whole board (12 columns, full height) to use it as a full-screen health dashboard on a TV.



## Variations

Other shapes and styling for this widget — pick the one closest to your data:

### Healthy on a gradient card

The one system the room watches, on a gradient card that turns the whole tile into the signal.

```json
{
  "title": "Payments API",
  "data": {
    "status": "Ok",
    "caption": "All checks passing"
  },
  "color": {
    "name": "Green",
    "style": "Gradient"
  }
}
```

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
      "subtitle": "All checks passing"
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
      "subtitle": "All checks failing"
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
      "subtitle": "3 failing checks"
    },
    "status": "Error"
  }
}
```

### Overall health with a legend per service

A health board where one indicator gives the overall verdict and a `legend` lists each service or check with its own colour — give it the whole board to use it as a full-screen health dashboard.

```json
{
  "title": "Platform health",
  "actions": [
    {
      "title": "Open Status Page",
      "type": "link",
      "url": "https://example.com/status-page"
    }
  ],
  "data": {
    "header": {
      "title": "Degraded",
      "subtitle": "1 of 5 services failing"
    },
    "status": "Error",
    "legend": [
      {
        "label": "API",
        "color": "Success",
        "value": "112 ms"
      },
      {
        "label": "Database",
        "color": "Success",
        "value": "8 ms"
      },
      {
        "label": "Queue",
        "color": "Warning",
        "value": "1.2k jobs"
      },
      {
        "label": "Payments",
        "color": "Danger",
        "value": "Down"
      },
      {
        "label": "CDN",
        "color": "Success",
        "value": "99.99%"
      }
    ]
  }
}
```

### Degraded (slow but up)

A service is up but slow or partly failing — send `Warning` instead of `Error` so the board shows the degraded state, not an outage.

```json
{
  "title": "Search API",
  "actions": [
    {
      "title": "Check Logs",
      "type": "link",
      "url": "https://example.com/status-logs"
    }
  ],
  "data": {
    "status": "Warning"
  }
}
```

### More examples

To see one in full, add `?variant=<id>` to the example request above — it returns that example's complete response.

- `status-contrast` — **Status on a contrast card.** A status tile lifted out of a row on a contrast card.
- `status-outage-alert` — **Outage with a critical alert.** A service is fully down — pair the Error indicator with a critical alert spelling out the impact.
- `status-multi-header` — **Healthy with a header strip.** A health tile that should also carry uptime and time-since-last-incident, instead of three separate tiles.
- `status-flower` — **Flower (illustrated).** A friendly one-widget board for a TV — a flower that blooms while everything is healthy, droops when degraded and wilts when down. Omit `caption` for the built-in sentence.
- `status-coffee` — **Coffee (illustrated).** A light-hearted office board — steam rises while all is well and stops when something is down.
- `status-weather-warning` — **Weather, degraded (illustrated).** A forecast-style board — clear skies when healthy, a cloud over the sun when degraded, a storm when down. Here a custom `caption` names the slow service.
- `status-rubber-duck-error` — **Rubber duck, down (illustrated).** An engineering-team board — the duck bobs calmly when there is nothing to debug and starts talking when something breaks. Pairs well with a `legend` of the services behind it.
- `status-rocket` — **Rocket (illustrated).** A launch or deploy board — the rocket hovers on its flame while healthy, sputters when degraded and lands when down.
- `status-open-sign-error` — **Open sign, down (illustrated).** A shop board — the sign reads OPEN while checkout works and flips to BACK SOON when it is down.
- `status-delivery-van-warning` — **Delivery van, degraded (illustrated).** An order-fulfilment board — the van drives while orders flow, slows when degraded and parks with its hazard lights on when down.
- `status-shopping-bag` — **Shopping bag (illustrated).** A storefront board — products drop into the bag as orders come in, slow to a trickle when degraded and stop when checkout is down.
- `status-flower-no-caption` — **Flower with a header and no caption.** An illustrated mode under a header, with the sentence turned off — send `caption` as an empty string to show the illustration alone.

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/status.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown` for a change, `Star` for a rating
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Sending a string for `status` outside `Ok` / `Warning` / `Error` — values are case-sensitive and limited to the `WidgetStatusIndicator` enum.
- Sending `Error` for a service that is only slow — use `Warning`, so the illustrated modes show the degraded state (a drooping flower, a slower van) instead of the outage.
- Using KPI shape for a health indicator — Status renders an explicit status indicator, KPI does not.
- Sending a `mode` outside the `WidgetStatusMode` enum — it is nullable, so omit it for the default rendering.
- Sending `caption: null` to hide the sentence under an illustrated mode — null means "use the built-in sentence". Send an empty string `""` to show none.
- Writing a long paragraph in `caption` — it is one short sentence under the illustration, at most 80 characters.
- Encoding each item's health in its `label` (`'API ✅'`) — send `legend` entries with a `color` instead, so the state reads as a swatch and the overall `status` stays the verdict.
- Sending more than 24 `legend` entries, or one without a `label` — the legend is a summary under the indicator, not a full check list.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
