# Table widget

> **Load this file when:** building a table / list / rows-and-columns widget.

## Purpose

Render rows of structured data with optional column labels — ideal for top-N lists, ranked items, status grids, recent activity.

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
    "columns": {
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
        "additionalProperties": false
      },
      "nullable": true
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
    "header": {
      "title": "128",
      "subtitle": "Products in stock"
    },
    "columns": [
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
    "columns": [
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
    "columns": [
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
    "columns": [
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

### Rows under a header strip

A list that should be read against its totals — put the summary numbers in the header strip instead of building separate KPI widgets beside the table.

```json
{
  "title": "Open invoices",
  "actions": [
    {
      "title": "Open billing",
      "type": "link",
      "url": "https://example.com/billing"
    }
  ],
  "data": {
    "header": {
      "title": "$18,420",
      "subtitle": "Outstanding",
      "badge": {
        "text": "+4.2%",
        "icon": "ArrowUp",
        "color": "Warning"
      }
    },
    "columns": [
      {
        "text": "Customer",
        "width": 50
      },
      {
        "text": "Due",
        "width": 25
      },
      {
        "text": "Amount",
        "width": 25
      }
    ],
    "rows": [
      [
        {
          "text": "Nimbus"
        },
        {
          "badge": {
            "text": "Overdue",
            "color": "Danger",
            "fill": "Outline"
          }
        },
        {
          "text": "$8,200"
        }
      ],
      [
        {
          "text": "Fernway"
        },
        {
          "text": "in 4 days"
        },
        {
          "text": "$6,100"
        }
      ],
      [
        {
          "text": "Orbit Labs"
        },
        {
          "text": "in 11 days"
        },
        {
          "text": "$4,120"
        }
      ]
    ],
    "headers": [
      {
        "title": "3",
        "subtitle": "Invoices"
      },
      {
        "title": "$8,200",
        "subtitle": "Overdue",
        "badge": {
          "text": "1 customer",
          "color": "Danger"
        }
      }
    ]
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/table.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

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
- Putting column labels in `headers` — that is now the header strip. Column labels live in `columns`; a legacy `headers: [{text, width}]` is still accepted and read as `columns`, but new endpoints should send `columns`.
- Column widths in `columns` must sum to ~100 (they're treated as percentages).
- Putting a URL in `text` instead of `link` — set `link` for clickable cells; `text` is the visible label.
- Using `imageUrl` for inline icons — it renders as an image; for badges use `badge`.
- Returning `rows: []` when there is no data — `Rows` requires at least one row and an empty list is rejected. Return `200` with a single placeholder row saying so (e.g. `No customers yet`), padded with empty cells to match the column count.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
