# Dashboard recipes

> **Load this file when:** designing a whole dashboard — the user named a domain ("a SaaS dashboard", "something for our store", "an ops board for the office TV") or asked for "a dashboard" without saying what goes on it.

A recipe is a finished design: which widgets, which type each one is, and where each sits. Every recipe below fills the board exactly — 12 columns wide, 6 rows tall, no empty slot — so it looks right the moment it is imported. Start from the closest one and adapt it to the data the project really has; do not make the user design the board.

## How to use a recipe

1. **Pick the closest recipe** by what the user's product *is*, not by the words they used. A booking platform asking for "our numbers" is the *Business overview*; an API company is *Service health*.
2. **Map each widget to data the project has.** Look at the models, the billing or analytics integration, the admin pages and any existing reports. Rename widgets to the project's own vocabulary ("Bookings", not "Orders").
3. **Swap what the project cannot feed** for something it can, keeping the widget's slot and a type that fits it (the "Swap for" column). Never keep a widget whose number you would have to invent.
4. **Show the user the list** — title · widget type · what it shows — and build on a yes. One short confirmation, not a questionnaire.
5. **Copy `position` / `size` exactly** into the setup file. They are the layout slots from `references/setup-files.md` → "Recommended layouts".

Fewer headline numbers than the recipe has? Do not leave the slot empty — widen the rest so the row still spans 12 columns: three tiles are `w:4` at `x:0 / 4 / 8`, two are `w:6` at `x:0 / 6`. The full rules are in `references/setup-files.md` → "Composition rules".

## SaaS metrics

For a subscription product. Layout: *Starter Kit*.

| Widget | Type | position | size | What the endpoint returns | Swap for |
| --- | --- | --- | --- | --- | --- |
| MRR | `kpi` | `x:0 y:0` | `w:3 h:1` | Current MRR, badge vs last month, `trend` of the last 12 readings | ARR |
| Active customers | `kpi` | `x:3 y:0` | `w:3 h:1` | Paying customers now, badge vs last month | Trials in progress |
| ARPU | `kpi` | `x:6 y:0` | `w:3 h:1` | Average revenue per customer | New signups this week |
| Churn | `kpi` | `x:9 y:0` | `w:3 h:1` | Churn rate this month; badge `Danger` when it rises | Net revenue retention |
| MRR over time | `line` | `x:0 y:1` | `w:8 h:5` | Monthly MRR for the last 12 months; `target` if there is a goal | `bar`: new signups per week |
| Top customers | `table` | `x:8 y:1` | `w:4 h:5` | Top 10 by MRR: name, plan, MRR | `donut`: MRR by plan |

## Store / e-commerce

For anything that takes orders. Layout: *Starter Kit*.

| Widget | Type | position | size | What the endpoint returns | Swap for |
| --- | --- | --- | --- | --- | --- |
| Revenue | `kpi` | `x:0 y:0` | `w:3 h:1` | Revenue for the selected range, badge vs the previous one | — |
| Orders | `kpi` | `x:3 y:0` | `w:3 h:1` | Order count, badge vs previous | Bookings, deliveries |
| Average order value | `kpi` | `x:6 y:0` | `w:3 h:1` | Revenue ÷ orders | Items per order |
| Conversion rate | `kpi` | `x:9 y:0` | `w:3 h:1` | Orders ÷ sessions, as a percentage | Refund rate (`Danger` when rising) |
| Revenue per day | `line` | `x:0 y:1` | `w:8 h:5` | Daily revenue; a `Comparison` dataset for the previous period | `bar`: orders per day |
| Revenue by category | `donut` | `x:8 y:1` | `w:4 h:5` | Up to 6 categories that sum to the total | `table`: best-selling products |

## Service health / ops

For an API, a backend or infrastructure. Layout: *Starter Kit*. This is the recipe that most often belongs on a TV.

| Widget | Type | position | size | What the endpoint returns | Swap for |
| --- | --- | --- | --- | --- | --- |
| System health | `status` | `x:0 y:0` | `w:3 h:1` | One overall state: `Ok` / `Warning` (degraded) / `Error` | — |
| Uptime | `kpi` | `x:3 y:0` | `w:3 h:1` | Uptime % over the range | Deploys this week |
| Response time | `kpi` | `x:6 y:0` | `w:3 h:1` | Average or p95 latency, badge vs previous | Queue depth |
| Error rate | `kpi` | `x:9 y:0` | `w:3 h:1` | Errors ÷ requests; badge `Danger` when rising; `alert` above the threshold | Open incidents |
| Requests and errors | `line` | `x:0 y:1` | `w:8 h:5` | Error rate (or latency) over time; `target` for the SLA line | `bar` with a `Line` rate on the right axis |
| Capacity | `progress` | `x:8 y:1` | `w:4 h:5` | Disk, memory, quota — each as used ÷ limit | `table`: recent incidents |

## Product / mobile app

For usage analytics. Layout: *Starter Kit*.

| Widget | Type | position | size | What the endpoint returns | Swap for |
| --- | --- | --- | --- | --- | --- |
| Daily active users | `kpi` | `x:0 y:0` | `w:3 h:1` | DAU today, badge vs last week, `trend` | Weekly active users |
| New signups | `kpi` | `x:3 y:0` | `w:3 h:1` | Signups in the range | Installs |
| Conversion to paid | `kpi` | `x:6 y:0` | `w:3 h:1` | Paid ÷ signups | Activation rate |
| Crash-free sessions | `kpi` | `x:9 y:0` | `w:3 h:1` | Percentage; badge `Danger` when it drops | App rating |
| Sessions started | `line` | `x:0 y:1` | `w:8 h:5` | Sessions per day | `contributions`: activity per day over months |
| Platform split | `donut` | `x:8 y:1` | `w:4 h:5` | iOS / Android / web share | `table`: most-used features |

## Business overview / client report

For a founder, a team lead or a client who wants "how is it doing" on one screen. Layout: *Starter Kit*.

| Widget | Type | position | size | What the endpoint returns | Swap for |
| --- | --- | --- | --- | --- | --- |
| Revenue this month | `kpi` | `x:0 y:0` | `w:3 h:1` | Month-to-date revenue with `progress` towards the monthly goal | — |
| The core activity today | `kpi` | `x:3 y:0` | `w:3 h:1` | Bookings / orders / signups today, badge vs the same day last week | — |
| Open issues | `kpi` | `x:6 y:0` | `w:3 h:1` | Open tickets; badge `Danger` when rising | Unpaid invoices |
| App health | `status` | `x:9 y:0` | `w:3 h:1` | `Ok` when it is up, `Error` when it is not | `countdown` to a launch |
| Activity over time | `line` | `x:0 y:1` | `w:8 h:5` | The core activity per day for the range | `bar`: per week or per month |
| Goals | `progress` | `x:8 y:1` | `w:4 h:5` | Quarterly goals, each as current ÷ target | `table`: latest customers |

## Wallboard (office TV)

For a screen read from across the room: big numbers, one chart, nothing dense. Layout: *Six Across*. Compact tiles only in the top row — KPI, Status, Clock, Countdown.

| Widget | Type | position | size | What the endpoint returns |
| --- | --- | --- | --- | --- |
| Headline 1–4 | `kpi` | `x:0 / 2 / 4 / 6  y:0` | `w:2 h:1` | The four numbers the room cares about; short titles, no `trend` (too narrow) |
| System health | `status` | `x:8 y:0` | `w:2 h:1` | One overall state: `Ok` / `Warning` / `Error` |
| Clock | `clock` | `x:10 y:0` | `w:2 h:1` | The office timezone |
| The main chart | `line` or `bar` | `x:0 y:1` | `w:12 h:5` | The one trend everyone watches; short labels, a `target` line if there is a goal |

## One metric in focus

For "I just want to watch signups". Layout: *Spotlight* — or *Single Panel* (`x:0 y:0`, `w:12 h:6`) when it really is one chart and nothing else.

| Widget | Type | position | size | What the endpoint returns |
| --- | --- | --- | --- | --- |
| The metric over time | `line` | `x:0 y:0` | `w:8 h:6` | The series, with `header` + `headers` for its totals |
| The number now | `kpi` | `x:8 y:0` | `w:4 h:1` | Today's value, badge vs yesterday |
| The latest rows | `table` | `x:8 y:1` | `w:4 h:5` | The 10 most recent signups / orders / events |

## A recipe as a setup file

The *SaaS metrics* recipe, written as plan-only mappings — it imports and shows the layout before a single endpoint exists. Once an endpoint is built, replace that mapping's `plan` with its `path` (and add a top-level `baseUrl`); `type`, `size` and `position` stay as they are.

```json
{
  "$schema": "https://api.dashboardbase.com/setup-file/schema/v1.json",
  "version": 1,
  "name": "SaaS Metrics",
  "refreshInterval": "10m",
  "mappings": [
    {
      "type": "kpi",
      "size": { "w": 3, "h": 1 },
      "position": { "x": 0, "y": 0 },
      "plan": { "goal": "Monthly recurring revenue, with the change vs last month", "dateRange": "current month", "groupBy": null, "dataSource": "stripe" }
    },
    {
      "type": "kpi",
      "size": { "w": 3, "h": 1 },
      "position": { "x": 3, "y": 0 },
      "plan": { "goal": "Active paying customers", "dateRange": "today", "groupBy": null, "dataSource": "database" }
    },
    {
      "type": "kpi",
      "size": { "w": 3, "h": 1 },
      "position": { "x": 6, "y": 0 },
      "plan": { "goal": "Average revenue per customer", "dateRange": "current month", "groupBy": null, "dataSource": "stripe" }
    },
    {
      "type": "kpi",
      "size": { "w": 3, "h": 1 },
      "position": { "x": 9, "y": 0 },
      "plan": { "goal": "Customer churn rate", "dateRange": "current month", "groupBy": null, "dataSource": "stripe" }
    },
    {
      "type": "line",
      "size": { "w": 8, "h": 5 },
      "position": { "x": 0, "y": 1 },
      "plan": { "goal": "MRR over time", "dateRange": "last 12 months", "groupBy": "month", "dataSource": "stripe" }
    },
    {
      "type": "table",
      "size": { "w": 4, "h": 5 },
      "position": { "x": 8, "y": 1 },
      "plan": { "goal": "Top 10 customers by MRR", "dateRange": "current month", "groupBy": "customer", "dataSource": "stripe" }
    }
  ]
}
```

## When nothing here fits

Design from the data instead: list what the project can count, pick the widget type per item from `SKILL.md` → "Choose widgets", then lay them out with `references/setup-files.md` → "Choosing a layout" and "Composition rules". The shape that almost always works is the one every recipe above shares — a row of headline numbers, one main chart, one supporting widget.
