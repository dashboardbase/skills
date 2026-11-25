# Table widget

> **Load this file when:** building a table / list / rows-and-columns widget.

## Purpose

Render rows of structured data with optional column labels — ideal for top-N lists, ranked items, status grids, recent activity. A ranked list can carry a quiet bar behind each row (`bar` on one cell, 0–100) so every row reads against the top one, any cell can draw a sparkline of its recent readings (`trend`), any cell can draw an inline progress bar with its percentage (`progress`, as many per row as there are columns for it), a cell without a picture can lead with a square `icon` tile instead: a bundled icon, a country flag or a person's initials, any cell can carry a muted second line (`subtitle`) under its text — an email under a name, a price under a product — and a row can end in a ⋮ menu of its own links (`actions` on one cell, the same shape as the widget's).

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
              "nullable": true,
              "x-placeholder": true
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
            "link": {
              "type": "string",
              "nullable": true,
              "x-placeholder": true
            },
            "bar": {
              "maximum": 100,
              "minimum": 0,
              "type": "number",
              "format": "double",
              "nullable": true
            },
            "trend": {
              "oneOf": [
                {
                  "maxItems": 120,
                  "minItems": 2,
                  "type": "array",
                  "items": {
                    "type": "number",
                    "format": "double"
                  },
                  "x-placeholder": true
                },
                {
                  "required": [
                    "values"
                  ],
                  "type": "object",
                  "properties": {
                    "values": {
                      "maxItems": 120,
                      "minItems": 2,
                      "type": "array",
                      "items": {
                        "type": "number",
                        "format": "double"
                      },
                      "x-placeholder": true
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
                    "smooth": {
                      "title": "Smooth line",
                      "type": "boolean",
                      "default": false,
                      "nullable": true
                    },
                    "fill": {
                      "title": "Filled area",
                      "type": "boolean",
                      "default": true,
                      "nullable": true
                    },
                    "type": {
                      "enum": [
                        "Line",
                        "Bars"
                      ],
                      "type": "string",
                      "title": "Trend style",
                      "nullable": true
                    }
                  },
                  "additionalProperties": false
                }
              ],
              "nullable": true
            },
            "progress": {
              "required": [
                "value"
              ],
              "type": "object",
              "properties": {
                "value": {
                  "type": "number",
                  "format": "double"
                },
                "max": {
                  "type": "number",
                  "format": "double",
                  "nullable": true
                },
                "label": {
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
                "shape": {
                  "enum": [
                    "Bar",
                    "Ring",
                    "Meter"
                  ],
                  "type": "string",
                  "nullable": true
                },
                "display": {
                  "maxLength": 24,
                  "type": "string",
                  "nullable": true
                },
                "marker": {
                  "required": [
                    "value"
                  ],
                  "type": "object",
                  "properties": {
                    "value": {
                      "type": "number",
                      "format": "double"
                    },
                    "label": {
                      "maxLength": 32,
                      "type": "string",
                      "nullable": true
                    }
                  },
                  "additionalProperties": false,
                  "nullable": true
                }
              },
              "additionalProperties": false,
              "nullable": true
            },
            "icon": {
              "type": "object",
              "properties": {
                "name": {
                  "enum": [
                    "Analytics",
                    "Bag",
                    "BarChart",
                    "Briefcase",
                    "Bug",
                    "Calendar",
                    "Card",
                    "Cart",
                    "Cash",
                    "Chat",
                    "CheckCircle",
                    "Cloud",
                    "Code",
                    "Cube",
                    "Desktop",
                    "Download",
                    "Eye",
                    "Flash",
                    "Gift",
                    "Globe",
                    "Heart",
                    "Home",
                    "Location",
                    "Mail",
                    "Megaphone",
                    "Notifications",
                    "People",
                    "Person",
                    "PersonAdd",
                    "Phone",
                    "PieChart",
                    "Pulse",
                    "Receipt",
                    "Rocket",
                    "Server",
                    "Shield",
                    "Speedometer",
                    "Star",
                    "Ticket",
                    "Time",
                    "TrendingDown",
                    "TrendingUp",
                    "Trophy",
                    "Wallet",
                    "Warning"
                  ],
                  "type": "string",
                  "nullable": true
                },
                "country": {
                  "type": "string",
                  "nullable": true
                },
                "initials": {
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
                }
              },
              "additionalProperties": false,
              "nullable": true
            },
            "subtitle": {
              "type": "string",
              "nullable": true
            },
            "actions": {
              "type": "array",
              "items": {
                "type": "object",
                "properties": {
                  "title": {
                    "type": "string",
                    "nullable": true
                  },
                  "type": {
                    "enum": [
                      "link"
                    ],
                    "type": "string"
                  },
                  "url": {
                    "type": "string",
                    "nullable": true,
                    "x-placeholder": true
                  }
                },
                "additionalProperties": false
              },
              "nullable": true
            },
            "avatars": {
              "required": [
                "items"
              ],
              "type": "object",
              "properties": {
                "items": {
                  "maxItems": 10,
                  "minItems": 1,
                  "type": "array",
                  "items": {
                    "type": "object",
                    "properties": {
                      "initials": {
                        "type": "string",
                        "nullable": true
                      },
                      "imageUrl": {
                        "type": "string",
                        "nullable": true,
                        "x-placeholder": true
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
                      }
                    },
                    "additionalProperties": false
                  }
                },
                "more": {
                  "maximum": 2147483647,
                  "minimum": 0,
                  "type": "integer",
                  "format": "int32",
                  "nullable": true
                }
              },
              "additionalProperties": false,
              "nullable": true,
              "x-placeholder": true
            }
          },
          "additionalProperties": false
        }
      },
      "x-placeholder": true
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
    "pagination": {
      "required": [
        "page",
        "pageSize"
      ],
      "type": "object",
      "properties": {
        "page": {
          "type": "integer",
          "format": "int32"
        },
        "pageSize": {
          "type": "integer",
          "format": "int32"
        },
        "total": {
          "type": "integer",
          "format": "int32",
          "nullable": true
        },
        "hasMore": {
          "type": "boolean",
          "nullable": true
        }
      },
      "additionalProperties": false,
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

## Sparkline cells

Any cell can carry `trend` — the same as a KPI's: the recent readings, oldest first, ending with the reading the row shows, as a plain array of numbers or, when the line needs styling, an object with that array in `values` and `color`, `smooth` and `fill` beside it. It is drawn as a small sparkline in that cell, the same line a KPI draws beside its number, coloured like the cell's badge with a soft fill under it. Give the trend its own column (a cell with only `trend`) or put it beside a value (a cell with `text` and `trend`; the line sits before the text so numbers stay right-aligned). On a phone a line that shares its cell with a value is left out so the columns still fit; one in its own column always shows. Like a KPI it is drawn with straight segments; set `smooth: true` for a curve, `color` to pick the line's colour (a cell without a badge or `color` draws it in the card's own text colour), and `fill: false` for a bare line. Each row's line is scaled to its own readings, so it shows the row's shape, not its size against the other rows — use `bar` for that.

```json
{
  "columns": [{ "text": "Product", "width": 45 }, { "text": "Last 14 days", "width": 30 }, { "text": "Revenue", "width": 25 }],
  "rows": [
    [{ "text": "Pro plan" }, { "trend": { "values": [320, 340, 335, 360, 390, 410, 440], "color": "Success" } }, { "text": "$4,400" }],
    [{ "text": "Starter" }, { "trend": [120, 118, 125, 131, 128, 140, 152] }, { "text": "$1,520" }]
  ]
}
```

## Icon tiles

Where a row has no picture to show, a cell can lead with a square `icon` tile instead — drawn in the
same rounded box as an `imageUrl`, so a leaderboard of people without photos or a breakdown by
country reads as cleanly as a product list with thumbnails. Set **exactly one** of:

- `name` — one of the bundled icons: `Analytics`, `Bag`, `BarChart`, `Briefcase`, `Bug`, `Calendar`, `Card`, `Cart`, `Cash`, `Chat`, `CheckCircle`, `Cloud`, `Code`, `Cube`, `Desktop`, `Download`, `Eye`, `Flash`, `Gift`, `Globe`, `Heart`, `Home`, `Location`, `Mail`, `Megaphone`, `Notifications`, `People`, `Person`, `PersonAdd`, `Phone`, `PieChart`, `Pulse`, `Receipt`, `Rocket`, `Server`, `Shield`, `Speedometer`, `Star`, `Ticket`, `Time`, `TrendingDown`, `TrendingUp`, `Trophy`, `Wallet`, or `Warning`. The same names as the envelope's title `icon`.
- `country` — a flag, as a two-letter ISO 3166-1 alpha-2 code in either case (`"DK"`, `"us"`). The United Kingdom is `GB`, not `UK`. A code without a flag is shown as its two letters and flagged by the validator.
- `initials` — one to 3 characters (`"NA"`, `"ML"`), shown in capitals. Send the initials themselves, not the full name — the tile does not work them out.

`color` is optional and tints the tile with any `WidgetDataColor` (e.g. `"Success"`); without it the
tile is the card's quiet neutral. Put the name or label in the same cell's `text` — the tile sits
before it. When a cell has both `imageUrl` and `icon`, the image is drawn and the tile takes over if
the image fails to load.

```json
[
  { "text": "Neil Ashworth", "icon": { "initials": "NA" } },
  { "text": "Denmark", "icon": { "country": "DK" } },
  { "text": "Orders", "icon": { "name": "Cart", "color": "Success" } }
]
```

Country codes with a flag:

`AD`, `AE`, `AF`, `AG`, `AI`, `AL`, `AM`, `AO`, `AQ`, `AR`, `AS`, `AT`, `AU`, `AW`, `AX`, `AZ`, `BA`, `BB`, `BD`, `BE`, `BF`, `BG`, `BH`, `BI`, `BJ`, `BL`, `BM`, `BN`, `BO`, `BQ`, `BR`, `BS`, `BT`, `BV`, `BW`, `BY`, `BZ`, `CA`, `CC`, `CD`, `CF`, `CG`, `CH`, `CI`, `CK`, `CL`, `CM`, `CN`, `CO`, `CP`, `CR`, `CU`, `CV`, `CW`, `CX`, `CY`, `CZ`, `DE`, `DG`, `DJ`, `DK`, `DM`, `DO`, `DZ`, `EC`, `EE`, `EG`, `EH`, `ER`, `ES`, `ET`, `EU`, `FI`, `FJ`, `FK`, `FM`, `FO`, `FR`, `GA`, `GB`, `GD`, `GE`, `GF`, `GG`, `GH`, `GI`, `GL`, `GM`, `GN`, `GP`, `GQ`, `GR`, `GS`, `GT`, `GU`, `GW`, `GY`, `HK`, `HM`, `HN`, `HR`, `HT`, `HU`, `IC`, `ID`, `IE`, `IL`, `IM`, `IN`, `IO`, `IQ`, `IR`, `IS`, `IT`, `JE`, `JM`, `JO`, `JP`, `KE`, `KG`, `KH`, `KI`, `KM`, `KN`, `KP`, `KR`, `KW`, `KY`, `KZ`, `LA`, `LB`, `LC`, `LI`, `LK`, `LR`, `LS`, `LT`, `LU`, `LV`, `LY`, `MA`, `MC`, `MD`, `ME`, `MF`, `MG`, `MH`, `MK`, `ML`, `MM`, `MN`, `MO`, `MP`, `MQ`, `MR`, `MS`, `MT`, `MU`, `MV`, `MW`, `MX`, `MY`, `MZ`, `NA`, `NC`, `NE`, `NF`, `NG`, `NI`, `NL`, `NO`, `NP`, `NR`, `NU`, `NZ`, `OM`, `PA`, `PC`, `PE`, `PF`, `PG`, `PH`, `PK`, `PL`, `PM`, `PN`, `PR`, `PS`, `PT`, `PW`, `PY`, `QA`, `RE`, `RO`, `RS`, `RU`, `RW`, `SA`, `SB`, `SC`, `SD`, `SE`, `SG`, `SH`, `SI`, `SJ`, `SK`, `SL`, `SM`, `SN`, `SO`, `SR`, `SS`, `ST`, `SV`, `SX`, `SY`, `SZ`, `TC`, `TD`, `TF`, `TG`, `TH`, `TJ`, `TK`, `TL`, `TM`, `TN`, `TO`, `TR`, `TT`, `TV`, `TW`, `TZ`, `UA`, `UG`, `UM`, `UN`, `US`, `UY`, `UZ`, `VA`, `VC`, `VE`, `VG`, `VI`, `VN`, `VU`, `WF`, `WS`, `XK`, `YE`, `YT`, `ZA`, `ZM`, `ZW`

## Variations

Other shapes and styling for this widget — pick the one closest to your data:

### Payments with status pills

Recent transactions or orders where each row ends in a calm status — Success, Processing, Failed — as a dot pill.

```json
{
  "title": "Latest Payments",
  "actions": [
    {
      "title": "View all",
      "type": "link",
      "url": "https://example.com/payments"
    }
  ],
  "data": {
    "header": {
      "subtitle": "See recent payments from your customers here."
    },
    "columns": [
      {
        "text": "Customer",
        "width": 30
      },
      {
        "text": "Email",
        "width": 32
      },
      {
        "text": "Amount",
        "width": 18
      },
      {
        "text": "Status",
        "width": 20
      }
    ],
    "rows": [
      [
        {
          "text": "Kenneth Thompson"
        },
        {
          "text": "ken99@yahoo.com"
        },
        {
          "text": "$316.00"
        },
        {
          "badge": {
            "text": "Success",
            "color": "Success",
            "fill": "Dot"
          },
          "actions": [
            {
              "title": "View payment",
              "type": "link",
              "url": "https://example.com/payments/1/view-payment"
            },
            {
              "title": "Refund",
              "type": "link",
              "url": "https://example.com/payments/1/refund"
            }
          ]
        }
      ],
      [
        {
          "text": "Abraham Lincoln"
        },
        {
          "text": "abe45@gmail.com"
        },
        {
          "text": "$242.00"
        },
        {
          "badge": {
            "text": "Success",
            "color": "Success",
            "fill": "Dot"
          },
          "actions": [
            {
              "title": "View payment",
              "type": "link",
              "url": "https://example.com/payments/2/view-payment"
            },
            {
              "title": "Refund",
              "type": "link",
              "url": "https://example.com/payments/2/refund"
            }
          ]
        }
      ],
      [
        {
          "text": "Monserrat Rodriguez"
        },
        {
          "text": "monserrat44@gmail.com"
        },
        {
          "text": "$837.00"
        },
        {
          "badge": {
            "text": "Processing",
            "color": "Blue",
            "fill": "Dot"
          },
          "actions": [
            {
              "title": "View payment",
              "type": "link",
              "url": "https://example.com/payments/3/view-payment"
            },
            {
              "title": "Refund",
              "type": "link",
              "url": "https://example.com/payments/3/refund"
            }
          ]
        }
      ],
      [
        {
          "text": "Silas Johnson"
        },
        {
          "text": "silas22@gmail.com"
        },
        {
          "text": "$874.00"
        },
        {
          "badge": {
            "text": "Success",
            "color": "Success",
            "fill": "Dot"
          },
          "actions": [
            {
              "title": "View payment",
              "type": "link",
              "url": "https://example.com/payments/4/view-payment"
            },
            {
              "title": "Refund",
              "type": "link",
              "url": "https://example.com/payments/4/refund"
            }
          ]
        }
      ],
      [
        {
          "text": "Carmella DeVito"
        },
        {
          "text": "carmella@hotmail.com"
        },
        {
          "text": "$721.00"
        },
        {
          "badge": {
            "text": "Failed",
            "color": "Danger",
            "fill": "Dot"
          },
          "actions": [
            {
              "title": "View payment",
              "type": "link",
              "url": "https://example.com/payments/5/view-payment"
            },
            {
              "title": "Refund",
              "type": "link",
              "url": "https://example.com/payments/5/refund"
            }
          ]
        }
      ],
      [
        {
          "text": "Maria Garcia"
        },
        {
          "text": "maria@gmail.com"
        },
        {
          "text": "$529.00"
        },
        {
          "badge": {
            "text": "Success",
            "color": "Success",
            "fill": "Dot"
          },
          "actions": [
            {
              "title": "View payment",
              "type": "link",
              "url": "https://example.com/payments/6/view-payment"
            },
            {
              "title": "Refund",
              "type": "link",
              "url": "https://example.com/payments/6/refund"
            }
          ]
        }
      ]
    ]
  }
}
```

### Orders with people and a second line

An order or ticket list where each row names a person and an item — initials tiles, the item under the order number, a status pill.

```json
{
  "title": "Recent Orders",
  "actions": [
    {
      "title": "All orders",
      "type": "link",
      "url": "https://example.com/orders"
    }
  ],
  "data": {
    "header": {
      "subtitle": "16 orders in the last 30 days"
    },
    "columns": [
      {
        "text": "Order",
        "width": 22
      },
      {
        "text": "Customer",
        "width": 38
      },
      {
        "text": "Status",
        "width": 20
      },
      {
        "text": "Amount",
        "width": 20
      }
    ],
    "rows": [
      [
        {
          "text": "#1023",
          "subtitle": "Tire Doodad"
        },
        {
          "text": "Theodore Bell",
          "icon": {
            "initials": "TB"
          }
        },
        {
          "badge": {
            "text": "Processing",
            "color": "Warning",
            "fill": "Dot"
          }
        },
        {
          "text": "$300.00"
        }
      ],
      [
        {
          "text": "#2045",
          "subtitle": "Engine Kit"
        },
        {
          "text": "Amelia Grant",
          "icon": {
            "initials": "AG"
          }
        },
        {
          "badge": {
            "text": "Paid",
            "color": "Success",
            "fill": "Dot"
          }
        },
        {
          "text": "$450.00"
        }
      ],
      [
        {
          "text": "#3067",
          "subtitle": "Brake Pads"
        },
        {
          "text": "Eleanor Woods",
          "icon": {
            "initials": "EW"
          }
        },
        {
          "badge": {
            "text": "Delivered",
            "color": "Blue",
            "fill": "Dot"
          }
        },
        {
          "text": "$820.00"
        }
      ],
      [
        {
          "text": "#4089",
          "subtitle": "Roof Rack"
        },
        {
          "text": "Marcus Hale",
          "icon": {
            "initials": "MH"
          }
        },
        {
          "badge": {
            "text": "Paid",
            "color": "Success",
            "fill": "Dot"
          }
        },
        {
          "text": "$129.00"
        }
      ],
      [
        {
          "text": "#5012",
          "subtitle": "Floor Mats"
        },
        {
          "text": "Priya Shah",
          "icon": {
            "initials": "PS"
          }
        },
        {
          "badge": {
            "text": "Refunded",
            "color": "Gray",
            "fill": "Dot"
          }
        },
        {
          "text": "$64.00"
        }
      ]
    ]
  }
}
```

### Best sellers with units and revenue

Top products ranked by revenue — the price under the name, units sold as a bar against the best seller, and the total.

```json
{
  "title": "Best Selling Products",
  "actions": [
    {
      "title": "All products",
      "type": "link",
      "url": "https://example.com/products"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Top products by revenue"
    },
    "columns": [
      {
        "text": "Product",
        "width": 40
      },
      {
        "text": "Units sold",
        "width": 32
      },
      {
        "text": "Revenue",
        "width": 28
      }
    ],
    "rows": [
      [
        {
          "text": "Red Sneakers",
          "icon": {
            "name": "Bag",
            "color": "Red"
          },
          "subtitle": "$402.00"
        },
        {
          "progress": {
            "value": 40,
            "max": 40,
            "label": "40"
          }
        },
        {
          "text": "$16,080.00"
        }
      ],
      [
        {
          "text": "Red Scarf",
          "icon": {
            "name": "Gift",
            "color": "Orange"
          },
          "subtitle": "$280.00"
        },
        {
          "progress": {
            "value": 37,
            "max": 40,
            "label": "37"
          }
        },
        {
          "text": "$10,360.00"
        }
      ],
      [
        {
          "text": "Bicycle",
          "icon": {
            "name": "Rocket",
            "color": "Blue"
          },
          "subtitle": "$310.00"
        },
        {
          "progress": {
            "value": 25,
            "max": 40,
            "label": "25"
          }
        },
        {
          "text": "$7,750.00"
        }
      ],
      [
        {
          "text": "Black T-Shirt",
          "icon": {
            "name": "Bag"
          },
          "subtitle": "$24.00"
        },
        {
          "progress": {
            "value": 274,
            "max": 316,
            "label": "274"
          }
        },
        {
          "text": "$6,576.00"
        }
      ],
      [
        {
          "text": "Jeans",
          "icon": {
            "name": "Bag",
            "color": "Blue"
          },
          "subtitle": "$59.00"
        },
        {
          "progress": {
            "value": 95,
            "max": 316,
            "label": "95"
          }
        },
        {
          "text": "$5,605.00"
        }
      ]
    ]
  }
}
```

### Wide list with trends and two-line cells

A full-width list where every row is a thing with its own performance — links, campaigns, products — the identifier under the name, a sparkline beside the volume, rates and a status.

```json
{
  "title": "Your links",
  "actions": [
    {
      "title": "New link",
      "type": "link",
      "url": "https://example.com/links"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Clicks, conversions and EPC for the last 30 days. Commission is all time."
    },
    "columns": [
      {
        "text": "Link",
        "width": 20
      },
      {
        "text": "Campaign",
        "width": 15
      },
      {
        "text": "Channel",
        "width": 10
      },
      {
        "text": "Clicks (30d)",
        "width": 14
      },
      {
        "text": "Conversions",
        "width": 9
      },
      {
        "text": "Conv. rate",
        "width": 8
      },
      {
        "text": "EPC",
        "width": 6
      },
      {
        "text": "Commission",
        "width": 10
      },
      {
        "text": "Status",
        "width": 8
      }
    ],
    "rows": [
      [
        {
          "text": "Aria ANC review video",
          "subtitle": "go.tobyb.link/aria-review"
        },
        {
          "text": "Northwind Audio",
          "subtitle": "8% per sale"
        },
        {
          "text": "YouTube"
        },
        {
          "text": "1,324",
          "trend": {
            "values": [
              40,
              38,
              42,
              39,
              41,
              44,
              52
            ],
            "color": "Light"
          }
        },
        {
          "text": "44"
        },
        {
          "text": "3.32%"
        },
        {
          "text": "$0.59"
        },
        {
          "text": "$3,430.50"
        },
        {
          "badge": {
            "text": "Active",
            "color": "Success",
            "fill": "Dot"
          },
          "actions": [
            {
              "title": "Edit link",
              "type": "link",
              "url": "https://example.com/links/1/edit-link"
            },
            {
              "title": "Pause",
              "type": "link",
              "url": "https://example.com/links/1/pause"
            }
          ]
        }
      ],
      [
        {
          "text": "Black Friday story",
          "subtitle": "go.tobyb.link/bf-audio"
        },
        {
          "text": "Northwind Audio",
          "subtitle": "8% per sale"
        },
        {
          "text": "Instagram"
        },
        {
          "text": "660",
          "trend": {
            "values": [
              2,
              2,
              3,
              10,
              18,
              20,
              22
            ],
            "color": "Light"
          }
        },
        {
          "text": "20"
        },
        {
          "text": "3.03%"
        },
        {
          "text": "$0.60"
        },
        {
          "text": "$395.07"
        },
        {
          "badge": {
            "text": "Active",
            "color": "Success",
            "fill": "Dot"
          },
          "actions": [
            {
              "title": "Edit link",
              "type": "link",
              "url": "https://example.com/links/2/edit-link"
            },
            {
              "title": "Pause",
              "type": "link",
              "url": "https://example.com/links/2/pause"
            }
          ]
        }
      ],
      [
        {
          "text": "Morning routine TikTok",
          "subtitle": "go.tobyb.link/glow"
        },
        {
          "text": "Lumen Skincare",
          "subtitle": "15% per sale"
        },
        {
          "text": "TikTok"
        },
        {
          "text": "2,290",
          "trend": {
            "values": [
              60,
              55,
              64,
              58,
              66,
              60,
              70
            ],
            "color": "Light"
          }
        },
        {
          "text": "113"
        },
        {
          "text": "4.93%"
        },
        {
          "text": "$0.49"
        },
        {
          "text": "$4,561.71"
        },
        {
          "badge": {
            "text": "Active",
            "color": "Success",
            "fill": "Dot"
          },
          "actions": [
            {
              "title": "Edit link",
              "type": "link",
              "url": "https://example.com/links/3/edit-link"
            },
            {
              "title": "Pause",
              "type": "link",
              "url": "https://example.com/links/3/pause"
            }
          ]
        }
      ],
      [
        {
          "text": "SPF reel",
          "subtitle": "go.tobyb.link/spf"
        },
        {
          "text": "Lumen Skincare",
          "subtitle": "15% per sale"
        },
        {
          "text": "Instagram"
        },
        {
          "text": "119",
          "trend": {
            "values": [
              4,
              4,
              4,
              4,
              4,
              4,
              4
            ],
            "color": "Light"
          }
        },
        {
          "text": "6"
        },
        {
          "text": "5.04%"
        },
        {
          "text": "$0.40"
        },
        {
          "text": "$629.56"
        },
        {
          "badge": {
            "text": "Paused",
            "color": "Gray",
            "fill": "Dot"
          },
          "actions": [
            {
              "title": "Edit link",
              "type": "link",
              "url": "https://example.com/links/4/edit-link"
            },
            {
              "title": "Pause",
              "type": "link",
              "url": "https://example.com/links/4/pause"
            }
          ]
        }
      ],
      [
        {
          "text": "Winter hiking kit guide",
          "subtitle": "go.tobyb.link/winter-kit"
        },
        {
          "text": "Trailhead Outdoor",
          "subtitle": "10% per sale"
        },
        {
          "text": "Blog"
        },
        {
          "text": "1,045",
          "trend": {
            "values": [
              30,
              34,
              33,
              36,
              35,
              34,
              30
            ],
            "color": "Light"
          }
        },
        {
          "text": "24"
        },
        {
          "text": "2.30%"
        },
        {
          "text": "$0.62"
        },
        {
          "text": "$1,004.26"
        },
        {
          "badge": {
            "text": "Active",
            "color": "Success",
            "fill": "Dot"
          },
          "actions": [
            {
              "title": "Edit link",
              "type": "link",
              "url": "https://example.com/links/5/edit-link"
            },
            {
              "title": "Pause",
              "type": "link",
              "url": "https://example.com/links/5/pause"
            }
          ]
        }
      ],
      [
        {
          "text": "Tools I use newsletter",
          "subtitle": "go.tobyb.link/pixelforge"
        },
        {
          "text": "PixelForge",
          "subtitle": "$45 flat per signup"
        },
        {
          "text": "Newsletter"
        },
        {
          "text": "963",
          "trend": {
            "values": [
              28,
              30,
              27,
              33,
              29,
              35,
              31
            ],
            "color": "Light"
          }
        },
        {
          "text": "20"
        },
        {
          "text": "2.08%"
        },
        {
          "text": "$0.93"
        },
        {
          "text": "$3,555.00"
        },
        {
          "badge": {
            "text": "Active",
            "color": "Success",
            "fill": "Dot"
          },
          "actions": [
            {
              "title": "Edit link",
              "type": "link",
              "url": "https://example.com/links/6/edit-link"
            },
            {
              "title": "Pause",
              "type": "link",
              "url": "https://example.com/links/6/pause"
            }
          ]
        }
      ]
    ]
  }
}
```

### Listings with the people on each

Properties, deals or projects where who is involved matters — the address under the name, a stack of avatars with a count, and a status.

```json
{
  "title": "Active Listings",
  "actions": [
    {
      "title": "All listings",
      "type": "link",
      "url": "https://example.com/listings"
    }
  ],
  "data": {
    "header": {
      "subtitle": "7 properties on the market"
    },
    "columns": [
      {
        "text": "Property",
        "width": 34
      },
      {
        "text": "Type",
        "width": 12
      },
      {
        "text": "Cost",
        "width": 12
      },
      {
        "text": "Active leads",
        "width": 18
      },
      {
        "text": "Views",
        "width": 10
      },
      {
        "text": "Status",
        "width": 14
      }
    ],
    "rows": [
      [
        {
          "text": "Summer House",
          "icon": {
            "name": "Home"
          },
          "subtitle": "Jl. Mencari Cinta Sejati, Jakarta"
        },
        {
          "text": "House"
        },
        {
          "text": "$655K"
        },
        {
          "avatars": {
            "items": [
              {
                "initials": "JC"
              },
              {
                "initials": "MR"
              }
            ],
            "more": 9
          }
        },
        {
          "text": "267"
        },
        {
          "badge": {
            "text": "Available",
            "color": "Success",
            "fill": "Dot"
          }
        }
      ],
      [
        {
          "text": "Cheery Castle",
          "icon": {
            "name": "Home"
          },
          "subtitle": "Jl. Pantai Kuta, Bali"
        },
        {
          "text": "House"
        },
        {
          "text": "$602K"
        },
        {
          "avatars": {
            "items": [
              {
                "initials": "HS"
              },
              {
                "initials": "EC"
              }
            ],
            "more": 52
          }
        },
        {
          "text": "891"
        },
        {
          "badge": {
            "text": "24/28 Occupied",
            "color": "Blue",
            "fill": "Dot"
          }
        }
      ],
      [
        {
          "text": "Lazy Shore Palace",
          "icon": {
            "name": "Briefcase"
          },
          "subtitle": "Jl. Sudirman, Jakarta"
        },
        {
          "text": "Medical"
        },
        {
          "text": "$842K"
        },
        {
          "avatars": {
            "items": [
              {
                "initials": "AK"
              },
              {
                "initials": "TB"
              }
            ],
            "more": 6
          }
        },
        {
          "text": "124"
        },
        {
          "badge": {
            "text": "Available",
            "color": "Success",
            "fill": "Dot"
          }
        }
      ],
      [
        {
          "text": "Willow Creek Villa",
          "icon": {
            "name": "Briefcase"
          },
          "subtitle": "Jl. Gatot Subroto, Bandung"
        },
        {
          "text": "Office"
        },
        {
          "text": "$748K"
        },
        {
          "avatars": {
            "items": [
              {
                "initials": "DM"
              },
              {
                "initials": "LB"
              }
            ],
            "more": 33
          }
        },
        {
          "text": "502"
        },
        {
          "badge": {
            "text": "Available",
            "color": "Success",
            "fill": "Dot"
          }
        }
      ]
    ]
  }
}
```

### People directory with role and status

A team or customer roster — initials, the email under the name, a role, a department tag and an employment status.

```json
{
  "title": "All Employees",
  "actions": [
    {
      "title": "Directory",
      "type": "link",
      "url": "https://example.com/employees"
    }
  ],
  "data": {
    "columns": [
      {
        "text": "Employee",
        "width": 36
      },
      {
        "text": "Role",
        "width": 24
      },
      {
        "text": "Department",
        "width": 22
      },
      {
        "text": "Status",
        "width": 18
      }
    ],
    "rows": [
      [
        {
          "text": "Justin Vetrovs",
          "icon": {
            "initials": "JV"
          },
          "subtitle": "justinve@gmail.com"
        },
        {
          "text": "Project Manager"
        },
        {
          "badge": {
            "text": "Team Projects",
            "color": "Gray",
            "fill": "Outline"
          }
        },
        {
          "badge": {
            "text": "Full-time",
            "color": "Success",
            "fill": "Dot"
          }
        }
      ],
      [
        {
          "text": "Ahmad Kenter",
          "icon": {
            "initials": "AK"
          },
          "subtitle": "kenterah@gmail.com"
        },
        {
          "text": "Web Designer"
        },
        {
          "badge": {
            "text": "Head of Projects",
            "color": "Gray",
            "fill": "Outline"
          }
        },
        {
          "badge": {
            "text": "Full-time",
            "color": "Success",
            "fill": "Dot"
          }
        }
      ],
      [
        {
          "text": "Davis Herwitz",
          "icon": {
            "initials": "DH"
          },
          "subtitle": "davishe@gmail.com"
        },
        {
          "text": "Marketing Coordinator"
        },
        {
          "badge": {
            "text": "Client & Team",
            "color": "Gray",
            "fill": "Outline"
          }
        },
        {
          "badge": {
            "text": "Full-time",
            "color": "Success",
            "fill": "Dot"
          }
        }
      ],
      [
        {
          "text": "Marcus George",
          "icon": {
            "initials": "MG"
          },
          "subtitle": "marcusg@gmail.com"
        },
        {
          "text": "Product Designer"
        },
        {
          "badge": {
            "text": "Case Study",
            "color": "Gray",
            "fill": "Outline"
          }
        },
        {
          "badge": {
            "text": "Freelance",
            "color": "Warning",
            "fill": "Dot"
          }
        }
      ],
      [
        {
          "text": "Lydia Bergson",
          "icon": {
            "initials": "LB"
          },
          "subtitle": "lydiab@gmail.com"
        },
        {
          "text": "Web Designer"
        },
        {
          "badge": {
            "text": "Design System",
            "color": "Gray",
            "fill": "Outline"
          }
        },
        {
          "badge": {
            "text": "Freelance",
            "color": "Warning",
            "fill": "Dot"
          }
        }
      ]
    ]
  }
}
```

### More examples

To see one in full, add `?variant=<id>` to the example request above — it returns that example's complete response.

- `table-bookings` — **Bookings with type and status pills.** A reservation or ticket list with two tagged columns — the kind of thing booked and where it stands.
- `table-recent-files` — **Recent files with type icons.** A recent-activity list where an icon says what each row is — file types, event kinds — with size and date beside it.
- `table-transactions` — **Transactions in and out.** A ledger where money moves both ways — an arrow tile per direction, the state and date under the description, incoming amounts in green.
- `table-projects` — **Projects with status and progress.** A portfolio list — the client, the deadline, where it stands and how far along it is.
- `table-countries-change` — **Countries with change under each.** Sales or traffic by country where each row carries its own change — the flag, the change as a second line, the total on the right.
- `table-campaign-funnel` — **Funnel steps with change.** A campaign or funnel read top to bottom — an icon per step, the count, and the change in colour.
- `table-leads-contrast` — **Lead list on a contrast card.** A short follow-up list that should stand out from the board's charts — people with where they are, on a contrast card.
- `table-team-gradient` — **Team list on a gradient card.** A small roster that is the board's focus — who is on call, who joined — on a gradient card.
- `table-top-customers` — **Ranked top-N list.** A leaderboard / top-N list of text values with one badge column for a status or tier.
- `table-row-bars` — **Ranked list with row bars.** A ranked breakdown (top pages, referrers, countries, customers) where the reader should see each row's size relative to the others at a glance — set `bar` on the cell holding the number, scaled so the top row is 100.
- `table-sparklines` — **Rows with sparklines.** A breakdown (plans, products, regions, endpoints) where each row has a recent history worth seeing next to its current value — set `trend` on a cell, oldest reading first, ending with the value the row shows.
- `table-status-grid` — **Status grid (coloured badges).** A grid of services/checks where each row's badge colour communicates health.
- `table-incident-alert` — **Status grid with a warning alert.** A grid of services where some are degraded — summarise the impact in a warning alert above the table.
- `table-multi-header` — **Rows under a header strip.** A list that should be read against its totals — put the summary numbers in the header strip instead of building separate KPI widgets beside the table.
- `table-progress` — **Progress bars in cells.** Rows that each track progress toward something — projects against their fee, tasks against their estimate, goals per team — with one or more bars per row and a colour for the rows that need attention.
- `table-initials` — **People without photos (initials).** A ranked list of people — reviewers, sales reps, contractors — where there is no photo to show: each row leads with a tile of the person's initials.
- `table-countries` — **Breakdown by country (flags).** A list keyed by country — revenue, users or orders per market — where a flag beside each name makes the list scannable.

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/table.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown` for a change, `Star` for a rating
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Rows is `List<List<Column>>` (a 2-D array), not a list of objects — each row is an array of column cells.
- Putting column labels in `headers` — that is now the header strip. Column labels live in `columns`; a legacy `headers: [{text, width}]` is still accepted and read as `columns`, but new endpoints should send `columns`.
- Column widths in `columns` must sum to ~100 (they're treated as percentages).
- Putting a URL in `text` instead of `link` — set `link` for clickable cells; `text` is the visible label.
- Using `imageUrl` for inline icons — it renders as an image; for badges use `badge`, and for an icon, a country flag or a person's initials in the picture's place use `icon` (`{ "name": "People" }`, `{ "country": "DK" }` or `{ "initials": "NA" }`).
- Setting more than one of `icon.name`, `icon.country` and `icon.initials` — a tile draws one thing. `name` is one of the title icons, `country` a two-letter ISO code, `initials` at most 3 characters.
- Squeezing two facts into one cell's `text` (`"Toby Belhome · contact@bundui.io"`) — put the second in `subtitle`, drawn as a muted line under the text.
- Colouring a status column with Solid badges — for a calm status pill set `badge.fill: "Dot"`: a hairline pill with a coloured dot (`color`) and neutral text, the way `Paid`, `Processing` and `Failed` read in a payments table.
- Listing people as text (`"Jessica, John +9"`) — send `avatars`: `{ "items": [{ "initials": "JC" }, { "imageUrl": "https://…" }], "more": 9 }` draws up to 4 overlapping round avatars and a "+9".
- Linking a row's actions from its `text` or a `link` column ("Edit", "Refund") — put them in one cell's `actions`, the same `{ "title", "type": "link", "url" }` list the widget's own menu takes, and the row ends in a ⋮ that opens them (at most 6, one cell per row).
- Returning `rows: []` when there is no data — `Rows` requires at least one row and an empty list is rejected. Return `200` with a single placeholder row saying so (e.g. `No customers yet`), padded with empty cells to match the column count.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
- Sending a raw count in `bar` — it is a percentage of the row's width (0–100), not the value itself. Scale each row against your largest one (`value / max * 100`) so the top row fills the row and the rest read relative to it.
- Setting `bar` on more than one cell in a row — the bar fills the whole row, so a row carries at most one. Put it on the cell holding the number it measures.
- Confusing `bar` with `progress` — `bar` is one quiet bar behind the whole row for ranking; `progress` is a visible bar inside its own cell (`{ "value": 86 }`, or `value` with `max`), drawn with its percentage, and a row may carry several. Colour it with `progress.color` (e.g. `Danger` for a project over budget); without one it takes the card's text colour.
- Sending a cell's readings as objects (`[{ "value": 320 }]`) or formatted strings — like a KPI's, `trend` (or `trend.values`) is a plain array of numbers, oldest first. Send 2 to 120 points; 7–30 reads best in a row.
