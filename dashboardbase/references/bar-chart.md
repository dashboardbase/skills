# BarChart widget

> **Load this file when:** building a bar chart / grouped bars / comparison by category.

## Purpose

Render one or more series of vertical bars across a shared label axis — ideal for 'X by Y' comparisons (monthly sales, items per category, multi-region comparison).

In a setup file, this widget's `type` is `bar` (see `references/setup-files.md`).

## Resolved JSON schema

The `data` field of the response envelope must match this schema (all `$ref`s are inlined here, so this is the complete contract). A standalone copy is bundled at `assets/schemas/bar-chart.json` for use with a JSON Schema validator:

```json
{
  "required": [
    "datasets"
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
    "labels": {
      "type": "array",
      "items": {
        "type": "string"
      },
      "nullable": true
    },
    "datasets": {
      "minItems": 1,
      "type": "array",
      "items": {
        "required": [
          "label"
        ],
        "type": "object",
        "properties": {
          "data": {
            "type": "array",
            "items": {
              "required": [
                "value"
              ],
              "type": "object",
              "properties": {
                "value": {
                  "type": "number",
                  "format": "double"
                },
                "prefix": {
                  "type": "string",
                  "nullable": true
                },
                "postfix": {
                  "type": "string",
                  "nullable": true
                },
                "note": {
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
                }
              },
              "additionalProperties": false
            },
            "nullable": true
          },
          "label": {
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
              "Dark"
            ],
            "type": "string",
            "nullable": true
          }
        },
        "additionalProperties": false
      }
    },
    "ticksX": {
      "type": "boolean",
      "nullable": true
    },
    "ticksY": {
      "type": "boolean",
      "nullable": true
    },
    "indexAxis": {
      "enum": [
        "x",
        "y"
      ],
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
  "title": "Monthly Sales",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/barchart-details"
    }
  ],
  "data": {
    "header": {
      "title": "644",
      "subtitle": "Last 6 months",
      "badge": {
        "text": "-42%",
        "icon": "ArrowDown",
        "color": "Danger"
      }
    },
    "labels": [
      "Jan",
      "Feb",
      "Mar",
      "Apr",
      "May",
      "Jun"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 65
          },
          {
            "value": 59
          },
          {
            "value": 80
          },
          {
            "value": 81
          },
          {
            "value": 56
          },
          {
            "value": 55
          }
        ],
        "label": "Series A"
      },
      {
        "data": [
          {
            "value": 28
          },
          {
            "value": 48
          },
          {
            "value": 40
          },
          {
            "value": 19
          },
          {
            "value": 86
          },
          {
            "value": 27
          }
        ],
        "label": "Series B"
      }
    ]
  }
}
```

## Example request

```bash
curl -X GET 'https://api.dashboardbase.com/example/basic-auth/barchart' \
-u "test:test"
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

## Multiple series

The canonical example shows two datasets ("Series A" and "Series B") with no explicit colors — colors are optional and the default palette renders well. To color a series explicitly, set `color` on the dataset; to recolor an individual bar, set `color` on a single `WidgetDataValue` inside `data`. The `labels` array is shared and should match the length of each dataset's `data` array.

```json
{
  "labels": ["Jan", "Feb"],
  "datasets": [
    { "label": "Series A", "color": "Blue", "data": [{"value": 65}, {"value": 59}] },
    { "label": "Series B", "color": "Green", "data": [{"value": 28}, {"value": 48}] }
  ]
}
```

## Variations

Other shapes and styling for this widget — pick the one closest to your data:

### Single coloured series

A single metric broken down by category (revenue per category, signups per plan).

```json
{
  "title": "Revenue by category",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/revenue-by-category"
    }
  ],
  "data": {
    "header": {
      "title": "$48,200",
      "subtitle": "Last 30 days",
      "badge": {
        "text": "+$3,100",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "Electronics",
      "Clothing",
      "Home",
      "Books",
      "Toys"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 18200
          },
          {
            "value": 12400
          },
          {
            "value": 9100
          },
          {
            "value": 5300
          },
          {
            "value": 3200
          }
        ],
        "label": "Revenue",
        "color": "Blue"
      }
    ]
  }
}
```

### Explicitly coloured series

You want brand/semantic colours on each series rather than the default palette.

```json
{
  "title": "New vs returning",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/new-vs-returning"
    }
  ],
  "data": {
    "labels": [
      "Mon",
      "Tue",
      "Wed",
      "Thu",
      "Fri"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 40
          },
          {
            "value": 52
          },
          {
            "value": 48
          },
          {
            "value": 61
          },
          {
            "value": 55
          }
        ],
        "label": "New",
        "color": "Blue"
      },
      {
        "data": [
          {
            "value": 22
          },
          {
            "value": 28
          },
          {
            "value": 31
          },
          {
            "value": 26
          },
          {
            "value": 35
          }
        ],
        "label": "Returning",
        "color": "Green"
      }
    ]
  }
}
```

### Comparison with a success alert

A breakdown where every category beat its goal — celebrate it with a success alert.

```json
{
  "title": "Revenue by category",
  "actions": [
    {
      "title": "View Details",
      "type": "link",
      "url": "https://example.com/revenue-by-category"
    }
  ],
  "data": {
    "header": {
      "title": "$52,800",
      "subtitle": "Last 30 days",
      "badge": {
        "text": "+$7,700",
        "icon": "ArrowUp",
        "color": "Success"
      }
    },
    "labels": [
      "Electronics",
      "Clothing",
      "Home",
      "Books",
      "Toys"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 19800
          },
          {
            "value": 13100
          },
          {
            "value": 9600
          },
          {
            "value": 6300
          },
          {
            "value": 4000
          }
        ],
        "label": "Revenue",
        "color": "Green"
      }
    ]
  },
  "alert": {
    "active": true,
    "level": "success",
    "message": "Every category beat last month's revenue"
  }
}
```

### Horizontal bars (indexAxis: y)

Long category labels or a ranking that reads better horizontally — set `indexAxis` to `y`.

```json
{
  "title": "Top referrers",
  "data": {
    "header": {
      "title": "Top referrers",
      "subtitle": "Last 30 days"
    },
    "labels": [
      "Google",
      "Direct",
      "Twitter / X",
      "Newsletter",
      "Product Hunt"
    ],
    "datasets": [
      {
        "data": [
          {
            "value": 4200
          },
          {
            "value": 3100
          },
          {
            "value": 1800
          },
          {
            "value": 1200
          },
          {
            "value": 640
          }
        ],
        "label": "Visits",
        "color": "Blue"
      }
    ],
    "indexAxis": "y"
  }
}
```

## Validation

The contract enforces the constraints declared in the schema above (required fields, value ranges, enum values). If the response does not satisfy them, Dashboardbase renders the widget in an error state. Before declaring done, validate your response's `data` field against `assets/schemas/bar-chart.json` with any JSON Schema validator (e.g. `ajv`, python `jsonschema`).

## Styling

Styling fields use the enums documented in `SKILL.md`:

- `WidgetDataColor` — color of badges, datasets, values
- `WidgetDataAlign` — text alignment
- `WidgetDataSize` — text/icon size
- `WidgetDataIcon` — `ArrowUp` / `ArrowDown`
- `WidgetDataFill` — badge fill style

All values are case-sensitive (`"Success"`, not `"success"`).

## Common mistakes

- Mismatched lengths: if `labels` has 6 entries, every dataset's `data` should have 6 entries.
- Sending `value` as a string (`"65"`) — values must be numbers.
- Setting `color` inside `data` items when you meant the whole series — series color goes on the dataset.
- Wanting horizontal bars? Set `indexAxis` to `y` — do not look for a separate horizontal-bar widget type.
