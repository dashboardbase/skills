# Table widget

> **Load this file when:** building a table / list / rows-and-columns widget.

## Purpose

Render rows of structured data with optional column headers — ideal for top-N lists, ranked items, status grids, recent activity.

In a setup file, this widget's `type` is `table` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/table.json` for use with a JSON Schema validator:

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
      "minItems": 1,
      "type": "array",
      "items": {
        "minItems": 1,
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
  "title": "Products",
  "actions": [
    {
      "title": "View All Products",
      "type": "link",
      "url": "https://example.com/products"
    }
  ],
  "data": {
    "headers": [
      {
        "text": "Product",
        "width": 40
      },
      {
        "text": "Status",
        "width": 25
      },
      {
        "text": "Price",
        "width": 20
      },
      {
        "text": "Link",
        "width": 15
      }
    ],
    "rows": [
      [
        {
          "text": "Graphic T-Shirt",
          "imageUrl": "https://app.dashboardbase.com/assets/example/product1.jpeg"
        },
        {
          "text": "In Stock",
          "badge": {
            "text": "Available",
            "icon": "ArrowUp",
            "color": "Success"
          }
        },
        {
          "text": "$29"
        },
        {
          "text": "View",
          "link": "https://example.com/products/graphic-t-shirt"
        }
      ],
      [
        {
          "text": "Slim Fit Pants",
          "imageUrl": "https://app.dashboardbase.com/assets/example/product2.jpeg"
        },
        {
          "text": "Low Stock",
          "badge": {
            "text": "Few left",
            "color": "Warning"
          }
        },
        {
          "text": "$59"
        },
        {
          "text": "View",
          "link": "https://example.com/products/slim-fit-pants"
        }
      ],
      [
        {
          "text": "Zip-Up Hoodie",
          "imageUrl": "https://app.dashboardbase.com/assets/example/product3.jpeg"
        },
        {
          "text": "Out of Stock",
          "badge": {
            "text": "Sold out",
            "icon": "ArrowDown",
            "color": "Danger",
            "fill": "Solid"
          }
        },
        {
          "text": "$79"
        },
        {
          "text": "View",
          "link": "https://example.com/products/zip-up-hoodie"
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

### Ranked top-N list

A leaderboard / top-N list of text values with one badge column for a status or tier.

```json
{
  "title": "Top customers",
  "actions": [
    {
      "title": "View All",
      "type": "link",
      "url": "https://example.com/customers"
    }
  ],
  "data": {
    "headers": [
      {
        "text": "Customer",
        "width": 45
      },
      {
        "text": "Plan",
        "width": 30
      },
      {
        "text": "MRR",
        "width": 25
      }
    ],
    "rows": [
      [
        {
          "text": "Acme Corp",
          "link": "https://example.com/customers/acme"
        },
        {
          "text": "Enterprise",
          "badge": {
            "text": "Enterprise",
            "color": "Dark"
          }
        },
        {
          "text": "$1,200"
        }
      ],
      [
        {
          "text": "Globex",
          "link": "https://example.com/customers/globex"
        },
        {
          "text": "Pro",
          "badge": {
            "text": "Pro",
            "color": "Success"
          }
        },
        {
          "text": "$480"
        }
      ],
      [
        {
          "text": "Initech",
          "link": "https://example.com/customers/initech"
        },
        {
          "text": "Pro",
          "badge": {
            "text": "Pro",
            "color": "Success"
          }
        },
        {
          "text": "$480"
        }
      ]
    ]
  }
}
```

### Status grid (coloured badges)

A grid of services/checks where each row's badge colour communicates health.

```json
{
  "title": "Service health",
  "actions": [
    {
      "title": "Open status page",
      "type": "link",
      "url": "https://example.com/status"
    }
  ],
  "data": {
    "headers": [
      {
        "text": "Service",
        "width": 55
      },
      {
        "text": "State",
        "width": 45
      }
    ],
    "rows": [
      [
        {
          "text": "API"
        },
        {
          "badge": {
            "text": "Operational",
            "color": "Success"
          }
        }
      ],
      [
        {
          "text": "Workers"
        },
        {
          "badge": {
            "text": "Degraded",
            "color": "Warning",
            "fill": "Outline"
          }
        }
      ],
      [
        {
          "text": "Webhooks"
        },
        {
          "badge": {
            "text": "Down",
            "icon": "ArrowDown",
            "color": "Danger",
            "fill": "Solid"
          }
        }
      ]
    ]
  }
}
```

### Status grid with a warning alert

A grid of services where some are degraded — summarise the impact in a warning alert above the table.

```json
{
  "title": "Service health",
  "actions": [
    {
      "title": "Open status page",
      "type": "link",
      "url": "https://example.com/status"
    }
  ],
  "data": {
    "headers": [
      {
        "text": "Service",
        "width": 55
      },
      {
        "text": "State",
        "width": 45
      }
    ],
    "rows": [
      [
        {
          "text": "API"
        },
        {
          "badge": {
            "text": "Operational",
            "color": "Success"
          }
        }
      ],
      [
        {
          "text": "Workers"
        },
        {
          "badge": {
            "text": "Degraded",
            "color": "Warning",
            "fill": "Outline"
          }
        }
      ],
      [
        {
          "text": "Search"
        },
        {
          "badge": {
            "text": "Degraded",
            "color": "Warning",
            "fill": "Outline"
          }
        }
      ]
    ]
  },
  "alert": {
    "active": true,
    "level": "warning",
    "message": "2 services reporting degraded performance"
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate your response's `data` field against `assets/schemas/table.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

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
