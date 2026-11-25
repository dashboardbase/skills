# List widget

> **Load this file when:** building a list of items — reminders, recent activity, people to follow up, an agenda, latest reviews.

## Purpose

Show a short list of things, each a row of its own rather than a row of numbers: a title and a muted second line, led by a picture, an icon tile, a date tile (`OCT` over `08`) or a coloured priority dot, and ending in a value (`16 mins ago`, `$1,999.00`), a badge or a stack of avatars. A row can be marked `done` (struck through, with a check) and can link somewhere. Use it for reminders, recent activity, leads, upcoming meetings or a support queue — anything a Table would turn into columns that don't line up.

In a setup file, this widget's `type` is `list` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/list.json` for use with a JSON Schema validator:

```json
{
  "required": [
    "items"
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
    "items": {
      "minItems": 1,
      "type": "array",
      "items": {
        "required": [
          "title"
        ],
        "type": "object",
        "properties": {
          "title": {
            "minLength": 1,
            "type": "string"
          },
          "subtitle": {
            "type": "string",
            "nullable": true
          },
          "imageUrl": {
            "type": "string",
            "nullable": true,
            "x-placeholder": true
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
          "date": {
            "required": [
              "day",
              "month"
            ],
            "type": "object",
            "properties": {
              "month": {
                "maxLength": 4,
                "minLength": 1,
                "type": "string"
              },
              "day": {
                "maxLength": 2,
                "minLength": 1,
                "type": "string"
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
          "value": {
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
          },
          "done": {
            "type": "boolean",
            "nullable": true
          },
          "link": {
            "type": "string",
            "nullable": true,
            "x-placeholder": true
          }
        },
        "additionalProperties": false
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
    }
  },
  "additionalProperties": false
}
```

## Example response

```json
{
  "title": "Reminders",
  "actions": [
    {
      "title": "All reminders",
      "type": "link",
      "url": "https://example.com/reminders"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Upcoming this month"
    },
    "items": [
      {
        "title": "Follow up with client",
        "subtitle": "Follow up with Mr. Aditya about payment status.",
        "date": {
          "month": "Oct",
          "day": "08"
        },
        "avatars": {
          "items": [
            {
              "initials": "AK"
            },
            {
              "initials": "MR"
            }
          ]
        }
      },
      {
        "title": "Upload new photos",
        "subtitle": "Upload new listing photos for the new villa launch.",
        "date": {
          "month": "Oct",
          "day": "12"
        },
        "avatars": {
          "items": [
            {
              "initials": "JC"
            }
          ]
        }
      },
      {
        "title": "Review sales contract",
        "subtitle": "Review and sign contract draft for The Somerset sale.",
        "date": {
          "month": "Oct",
          "day": "17"
        },
        "avatars": {
          "items": [
            {
              "initials": "SL"
            },
            {
              "initials": "EW"
            }
          ]
        }
      },
      {
        "title": "Schedule property viewing",
        "subtitle": "Arrange a viewing of Cheery Castle for the Chen family.",
        "date": {
          "month": "Oct",
          "day": "21"
        },
        "avatars": {
          "items": [
            {
              "initials": "HC"
            }
          ]
        }
      },
      {
        "title": "Renew listing agreement",
        "subtitle": "Renew the exclusive listing agreement for Summer House.",
        "date": {
          "month": "Oct",
          "day": "28"
        },
        "avatars": {
          "items": [
            {
              "initials": "DM"
            },
            {
              "initials": "TK"
            }
          ]
        }
      }
    ]
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/list'
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

### Recent activity with people

A feed of the latest requests, signups or comments — who, what they asked for, and how long ago.

```json
{
  "title": "Recent activities",
  "actions": [
    {
      "title": "View all",
      "type": "link",
      "url": "https://example.com/activity"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Latest guest requests"
    },
    "items": [
      {
        "title": "Wade Warren",
        "subtitle": "Room 2747 · requested a coffee and water",
        "icon": {
          "initials": "WW"
        },
        "value": "16 mins ago"
      },
      {
        "title": "Esther Howard",
        "subtitle": "Room 3565 · book and manage conference",
        "icon": {
          "initials": "EH"
        },
        "value": "24 mins ago"
      },
      {
        "title": "Leslie Alexander",
        "subtitle": "Room 3546 · information about local tours",
        "icon": {
          "initials": "LA"
        },
        "value": "32 mins ago"
      },
      {
        "title": "Guy Hawkins",
        "subtitle": "Room 5654 · late checkout",
        "icon": {
          "initials": "GH"
        },
        "value": "48 mins ago"
      }
    ]
  }
}
```

### Tasks with priority and done

A to-do list where priority matters — a coloured dot per task, the due time and category under it, finished ones struck through.

```json
{
  "title": "Reminders",
  "actions": [
    {
      "title": "Set reminder",
      "type": "link",
      "url": "https://example.com/tasks"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Your upcoming tasks and meetings"
    },
    "items": [
      {
        "title": "Create a design training for beginners.",
        "subtitle": "Low Priority · Today, 12:30 · Design Education",
        "color": "Gray"
      },
      {
        "title": "Have a meeting with the new design team.",
        "subtitle": "Medium Priority · Today, 10:00 · Meeting",
        "color": "Warning",
        "done": true
      },
      {
        "title": "Respond to customer support emails.",
        "subtitle": "High Priority · Tomorrow, 16:30 · Customer Support",
        "color": "Danger"
      },
      {
        "title": "Review the Q4 project budget with finance.",
        "subtitle": "Medium Priority · Thu, 11:00 · Planning",
        "color": "Warning"
      },
      {
        "title": "Share sprint retrospective notes with the team.",
        "subtitle": "Low Priority · Fri, 15:00 · Team",
        "color": "Gray"
      }
    ]
  }
}
```

### People to follow up

A short list of people — leads, new signups, at-risk accounts — with where they are and when they last got in touch.

```json
{
  "title": "Leads",
  "actions": [
    {
      "title": "All leads",
      "type": "link",
      "url": "https://example.com/leads"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Recent inquiries to follow up"
    },
    "items": [
      {
        "title": "Jessica Chen",
        "subtitle": "New York, Albany",
        "icon": {
          "initials": "JC"
        },
        "value": "2d ago"
      },
      {
        "title": "John Doe",
        "subtitle": "California, LA",
        "icon": {
          "initials": "JD"
        },
        "value": "3d ago"
      },
      {
        "title": "Hailee S.",
        "subtitle": "New York, Troy",
        "icon": {
          "initials": "HS"
        },
        "value": "5d ago"
      },
      {
        "title": "Evan Chris",
        "subtitle": "Ohio, Columbus",
        "icon": {
          "initials": "EC"
        },
        "value": "1w ago"
      }
    ]
  }
}
```

### Today's agenda

Meetings or events coming up — an icon for the kind, where and when under the title, a link to join.

```json
{
  "title": "Work Calendar",
  "actions": [
    {
      "title": "Calendar",
      "type": "link",
      "url": "https://example.com/calendar"
    }
  ],
  "data": {
    "items": [
      {
        "title": "Employee Safety Workshop",
        "subtitle": "Online · 01:30 PM",
        "icon": {
          "name": "Desktop"
        },
        "link": "https://example.com/meet/safety"
      },
      {
        "title": "Team Huddle",
        "subtitle": "Online · 08:30 AM",
        "icon": {
          "name": "People"
        },
        "link": "https://example.com/meet/huddle"
      },
      {
        "title": "Business Presentation",
        "subtitle": "Conference Room · 11:00 AM",
        "icon": {
          "name": "Megaphone"
        },
        "link": "https://example.com/meet/presentation"
      },
      {
        "title": "Monthly Evaluation",
        "subtitle": "Today, 08:30 AM – 10:30 AM",
        "icon": {
          "name": "Calendar"
        },
        "avatars": {
          "items": [
            {
              "initials": "AL"
            },
            {
              "initials": "BK"
            },
            {
              "initials": "CM"
            },
            {
              "initials": "DN"
            }
          ],
          "more": 36
        }
      }
    ]
  }
}
```

### Countries with value and change

A ranked list where each row is a place — the flag, its change under the name, the total on the right.

```json
{
  "title": "Sales by Countries",
  "actions": [
    {
      "title": "View all",
      "type": "link",
      "url": "https://example.com/countries"
    }
  ],
  "data": {
    "header": {
      "subtitle": "Last 28 days"
    },
    "items": [
      {
        "title": "United States",
        "subtitle": "+27.4% vs last month",
        "icon": {
          "country": "US"
        },
        "value": "$1,999.00"
      },
      {
        "title": "Brazil",
        "subtitle": "+20.1% vs last month",
        "icon": {
          "country": "BR"
        },
        "value": "$39.00"
      },
      {
        "title": "India",
        "subtitle": "-5% vs last month",
        "icon": {
          "country": "IN"
        },
        "value": "$299.00"
      },
      {
        "title": "Australia",
        "subtitle": "+10.9% vs last month",
        "icon": {
          "country": "AU"
        },
        "value": "$99.00"
      }
    ]
  }
}
```

### Steps with a value and change

Named steps or channels with a number and how it moved — an icon each, the count, and the change in colour.

```json
{
  "title": "Monthly Campaign State",
  "actions": [
    {
      "title": "Campaigns",
      "type": "link",
      "url": "https://example.com/campaigns"
    }
  ],
  "data": {
    "header": {
      "subtitle": "8.5K social visitors"
    },
    "items": [
      {
        "title": "Emails",
        "icon": {
          "name": "Mail"
        },
        "value": "1,503",
        "badge": {
          "text": "-0.3%",
          "color": "Danger",
          "fill": "Clear"
        }
      },
      {
        "title": "Opened",
        "icon": {
          "name": "Eye"
        },
        "value": "6,043",
        "badge": {
          "text": "+2.1%",
          "color": "Success",
          "fill": "Clear"
        }
      },
      {
        "title": "Clicked",
        "icon": {
          "name": "Flash"
        },
        "value": "600",
        "badge": {
          "text": "-2.1%",
          "color": "Danger",
          "fill": "Clear"
        }
      },
      {
        "title": "Subscribe",
        "icon": {
          "name": "PersonAdd"
        },
        "value": "490",
        "badge": {
          "text": "+8.5%",
          "color": "Success",
          "fill": "Clear"
        }
      }
    ]
  }
}
```

### More examples

To see one in full, add `?variant=<id>` to the example request above — it returns that example's complete response.

- `list-gradient` — **Agenda on a gradient card.** The one list a board is about — today's meetings, the on-call rota — on a gradient card.
- `list-contrast` — **Tasks on a contrast card.** A short to-do list lifted out of the board on a contrast card.

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, dashboardbase renders the widget in an error state. Before declaring done, validate the response. If the `validate_widget_response` tool is available, call it with the full response body — that checks against the live contract. Otherwise validate the response's `data` field against `assets/schemas/list.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown` for a change, `Star` for a rating
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Using a Table for reminders or activity — rows that are a title, a detail line and one trailing value read better as a List: no column labels, a second line under each title, and a leading date or picture.
- Setting more than one leading element on a row — `imageUrl`, `icon`, `date` and `color` all draw in front of the title; send one. `color` alone draws a small priority dot.
- Writing the date into the title (`"08 Oct — Follow up with client"`) — send `date: { "month": "Oct", "day": "08" }` and it is drawn as a tile in front of the row.
- Putting the trailing time or amount in `subtitle` — `value` is drawn on the right, `subtitle` under the title.
- Sending every past item — a List shows what fits and fades the rest; send the 5–10 the reader needs, most urgent first.
