# Table widget

> **Load this file when:** building a table / list / rows-and-columns widget.

## Purpose

Render rows of structured data with optional column labels — ideal for top-N lists, ranked items, status grids, recent activity. A ranked list can carry a quiet bar behind each row (`bar` on one cell, 0–100) so every row reads against the top one, any cell can draw a sparkline of its recent readings (`trend`), any cell can draw an inline progress bar with its percentage (`progress`, as many per row as there are columns for it), and a cell without a picture can lead with a square `icon` tile instead: a bundled icon, a country flag or a person's initials.

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
                "Gray"
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
            "Dark",
            "Gray"
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
                    "Gray"
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
                  }
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
                      }
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
                        "Gray"
                      ],
                      "type": "string",
                      "nullable": true
                    },
                    "smooth": {
                      "type": "boolean",
                      "nullable": true
                    },
                    "fill": {
                      "type": "boolean",
                      "nullable": true
                    },
                    "type": {
                      "enum": [
                        "Line",
                        "Bars"
                      ],
                      "type": "string",
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
                    "Gray"
                  ],
                  "type": "string",
                  "nullable": true
                },
                "shape": {
                  "enum": [
                    "Bar",
                    "Ring"
                  ],
                  "type": "string",
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
                    "Gray"
                  ],
                  "type": "string",
                  "nullable": true
                }
              },
              "additionalProperties": false,
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
                  "Gray"
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
              "Dark",
              "Gray"
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

### Ranked list with row bars

A ranked breakdown (top pages, referrers, countries, customers) where the reader should see each row's size relative to the others at a glance — set `bar` on the cell holding the number, scaled so the top row is 100.

```json
{
  "title": "Top pages",
  "actions": [
    {
      "title": "View all pages",
      "type": "link",
      "url": "https://example.com/analytics/pages"
    }
  ],
  "data": {
    "columns": [
      {
        "text": "Page",
        "width": 60
      },
      {
        "text": "Visitors",
        "width": 20
      },
      {
        "text": "Views",
        "width": 20
      }
    ],
    "rows": [
      [
        {
          "text": "/pricing"
        },
        {
          "text": "2,480",
          "bar": 100
        },
        {
          "text": "3,915"
        }
      ],
      [
        {
          "text": "/"
        },
        {
          "text": "1,962",
          "bar": 79.1
        },
        {
          "text": "2,604"
        }
      ],
      [
        {
          "text": "/docs/getting-started"
        },
        {
          "text": "1,105",
          "bar": 44.6
        },
        {
          "text": "1,870"
        }
      ],
      [
        {
          "text": "/blog/launch-week"
        },
        {
          "text": "640",
          "bar": 25.8
        },
        {
          "text": "712"
        }
      ],
      [
        {
          "text": "/changelog"
        },
        {
          "text": "318",
          "bar": 12.8
        },
        {
          "text": "455"
        }
      ]
    ]
  }
}
```

### Rows with sparklines

A breakdown (plans, products, regions, endpoints) where each row has a recent history worth seeing next to its current value — set `trend` on a cell, oldest reading first, ending with the value the row shows.

```json
{
  "title": "MRR by plan",
  "actions": [
    {
      "title": "View revenue",
      "type": "link",
      "url": "https://example.com/revenue"
    }
  ],
  "data": {
    "columns": [
      {
        "text": "Plan",
        "width": 35
      },
      {
        "text": "Last 10 weeks",
        "width": 30
      },
      {
        "text": "MRR",
        "width": 20
      },
      {
        "text": "Change",
        "width": 15
      }
    ],
    "rows": [
      [
        {
          "text": "Enterprise"
        },
        {
          "trend": {
            "values": [
              18200,
              18900,
              19400,
              19100,
              20300,
              21000,
              21800,
              22600,
              23100,
              24400
            ],
            "color": "Success"
          }
        },
        {
          "text": "$24,400"
        },
        {
          "badge": {
            "text": "+34%",
            "icon": "ArrowUp",
            "color": "Success"
          }
        }
      ],
      [
        {
          "text": "Pro"
        },
        {
          "trend": {
            "values": [
              12100,
              12500,
              12300,
              12900,
              13400,
              13200,
              13900,
              14500,
              14800,
              15200
            ],
            "color": "Success"
          }
        },
        {
          "text": "$15,200"
        },
        {
          "badge": {
            "text": "+26%",
            "icon": "ArrowUp",
            "color": "Success"
          }
        }
      ],
      [
        {
          "text": "Team"
        },
        {
          "trend": {
            "values": [
              8400,
              8300,
              8600,
              8200,
              8100,
              7900,
              8000,
              7700,
              7600,
              7450
            ],
            "color": "Danger"
          }
        },
        {
          "text": "$7,450"
        },
        {
          "badge": {
            "text": "-11%",
            "icon": "ArrowDown",
            "color": "Danger"
          }
        }
      ],
      [
        {
          "text": "Starter"
        },
        {
          "trend": {
            "values": [
              2900,
              3050,
              3000,
              3200,
              3350,
              3300,
              3500,
              3650,
              3700,
              3820
            ],
            "color": "Success"
          }
        },
        {
          "text": "$3,820"
        },
        {
          "badge": {
            "text": "+32%",
            "icon": "ArrowUp",
            "color": "Success"
          }
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

### Progress bars in cells

Rows that each track progress toward something — projects against their fee, tasks against their estimate, goals per team — with one or more bars per row and a colour for the rows that need attention.

```json
{
  "title": "Fee burn by project",
  "actions": [
    {
      "title": "Open projects",
      "type": "link",
      "url": "https://example.com/projects"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Fee used against work done: a project spending faster than it progresses shows red"
    },
    "columns": [
      {
        "text": "Project",
        "width": 22
      },
      {
        "text": "Stage",
        "width": 12
      },
      {
        "text": "Progress",
        "width": 20
      },
      {
        "text": "Fee used",
        "width": 20
      },
      {
        "text": "Fee",
        "width": 13
      },
      {
        "text": "Left",
        "width": 13
      }
    ],
    "rows": [
      [
        {
          "text": "Wren Street café"
        },
        {
          "text": "On site"
        },
        {
          "progress": {
            "value": 86
          }
        },
        {
          "progress": {
            "value": 89
          }
        },
        {
          "text": "£42,800"
        },
        {
          "text": "£4,880"
        }
      ],
      [
        {
          "text": "Mill Lane extension"
        },
        {
          "text": "Handover"
        },
        {
          "progress": {
            "value": 95
          }
        },
        {
          "progress": {
            "value": 97
          }
        },
        {
          "text": "£31,200"
        },
        {
          "text": "£1,060"
        }
      ],
      [
        {
          "text": "Saltmarsh visitor hut"
        },
        {
          "text": "Concept"
        },
        {
          "progress": {
            "value": 31
          }
        },
        {
          "progress": {
            "value": 27
          }
        },
        {
          "text": "£36,500"
        },
        {
          "text": "£26,630"
        }
      ],
      [
        {
          "text": "Cobbler's Yard housing"
        },
        {
          "text": "Technical"
        },
        {
          "progress": {
            "value": 52
          }
        },
        {
          "progress": {
            "value": 71,
            "color": "Danger"
          }
        },
        {
          "text": "£186,400"
        },
        {
          "text": "£54,060"
        }
      ],
      [
        {
          "text": "Larkhill school library"
        },
        {
          "text": "Developed"
        },
        {
          "progress": {
            "value": 48
          }
        },
        {
          "progress": {
            "value": 41
          }
        },
        {
          "text": "£128,000"
        },
        {
          "text": "£75,390"
        }
      ]
    ]
  }
}
```

### People without photos (initials)

A ranked list of people — reviewers, sales reps, contractors — where there is no photo to show: each row leads with a tile of the person's initials.

```json
{
  "title": "Best rated pros",
  "actions": [
    {
      "title": "Directory",
      "type": "link",
      "url": "https://example.com/pros"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Three reviews or more"
    },
    "columns": [
      {
        "text": "Pro",
        "width": 45
      },
      {
        "text": "Trade",
        "width": 25
      },
      {
        "text": "Reviews",
        "width": 15
      },
      {
        "text": "Rating",
        "width": 15
      }
    ],
    "rows": [
      [
        {
          "text": "Neil Ashworth",
          "icon": {
            "initials": "NA"
          }
        },
        {
          "text": "Gardener"
        },
        {
          "text": "5"
        },
        {
          "badge": {
            "text": "5.00",
            "icon": "Star",
            "color": "Light",
            "fill": "Solid"
          }
        }
      ],
      [
        {
          "text": "Callum Reid",
          "icon": {
            "initials": "CR"
          }
        },
        {
          "text": "Tiler"
        },
        {
          "text": "4"
        },
        {
          "badge": {
            "text": "5.00",
            "icon": "Star"
          }
        }
      ],
      [
        {
          "text": "Lena Kowalski",
          "icon": {
            "initials": "LK"
          }
        },
        {
          "text": "Gardener"
        },
        {
          "text": "7"
        },
        {
          "badge": {
            "text": "4.86",
            "icon": "Star"
          }
        }
      ],
      [
        {
          "text": "Sofia Lindgren",
          "icon": {
            "initials": "SL"
          }
        },
        {
          "text": "Painter"
        },
        {
          "text": "5"
        },
        {
          "badge": {
            "text": "4.80",
            "icon": "Star"
          }
        }
      ],
      [
        {
          "text": "Rafael Duarte",
          "icon": {
            "initials": "RD"
          }
        },
        {
          "text": "Locksmith"
        },
        {
          "text": "4"
        },
        {
          "badge": {
            "text": "4.75",
            "icon": "Star"
          }
        }
      ]
    ]
  }
}
```

### Breakdown by country (flags)

A list keyed by country — revenue, users or orders per market — where a flag beside each name makes the list scannable.

```json
{
  "title": "Revenue by country",
  "actions": [
    {
      "title": "Open report",
      "type": "link",
      "url": "https://example.com/reports/countries"
    }
  ],
  "data": {
    "header": {
      "title": "$105,445",
      "subtitle": "Last 30 days"
    },
    "columns": [
      {
        "text": "Country",
        "width": 50
      },
      {
        "text": "Revenue",
        "width": 30
      },
      {
        "text": "Change",
        "width": 20
      }
    ],
    "rows": [
      [
        {
          "text": "United States",
          "icon": {
            "country": "US"
          }
        },
        {
          "text": "$48,210",
          "bar": 100
        },
        {
          "badge": {
            "text": "+12%",
            "icon": "ArrowUp",
            "color": "Success"
          }
        }
      ],
      [
        {
          "text": "Germany",
          "icon": {
            "country": "DE"
          }
        },
        {
          "text": "$21,940",
          "bar": 45.5
        },
        {
          "badge": {
            "text": "+8%",
            "icon": "ArrowUp",
            "color": "Success"
          }
        }
      ],
      [
        {
          "text": "United Kingdom",
          "icon": {
            "country": "GB"
          }
        },
        {
          "text": "$18,305",
          "bar": 38
        },
        {
          "badge": {
            "text": "-3%",
            "icon": "ArrowDown",
            "color": "Danger"
          }
        }
      ],
      [
        {
          "text": "Denmark",
          "icon": {
            "country": "DK"
          }
        },
        {
          "text": "$9,870",
          "bar": 20.5
        },
        {
          "badge": {
            "text": "+21%",
            "icon": "ArrowUp",
            "color": "Success"
          }
        }
      ],
      [
        {
          "text": "Japan",
          "icon": {
            "country": "JP"
          }
        },
        {
          "text": "$7,120",
          "bar": 14.8
        },
        {
          "badge": {
            "text": "-6%",
            "icon": "ArrowDown",
            "color": "Danger"
          }
        }
      ]
    ]
  }
}
```

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
- Returning `rows: []` when there is no data — `Rows` requires at least one row and an empty list is rejected. Return `200` with a single placeholder row saying so (e.g. `No customers yet`), padded with empty cells to match the column count.
- Sending more than 6 header blocks — `header` and `headers` together may hold at most 6, one per pair of grid columns. Everything past the sixth is a validation error, not a silent truncation.
- Sending a raw count in `bar` — it is a percentage of the row's width (0–100), not the value itself. Scale each row against your largest one (`value / max * 100`) so the top row fills the row and the rest read relative to it.
- Setting `bar` on more than one cell in a row — the bar fills the whole row, so a row carries at most one. Put it on the cell holding the number it measures.
- Confusing `bar` with `progress` — `bar` is one quiet bar behind the whole row for ranking; `progress` is a visible bar inside its own cell (`{ "value": 86 }`, or `value` with `max`), drawn with its percentage, and a row may carry several. Colour it with `progress.color` (e.g. `Danger` for a project over budget); without one it takes the card's text colour.
- Sending a cell's readings as objects (`[{ "value": 320 }]`) or formatted strings — like a KPI's, `trend` (or `trend.values`) is a plain array of numbers, oldest first. Send 2 to 120 points; 7–30 reads best in a row.
