# Changing an endpoint that is already live

> **Load this file when:** a widget endpoint already exists and is being polled, and the task is to change what it returns — add a header, a badge, an action link, a colour, another bar or line, another column, or an alert.

Building the first version of an endpoint and changing the tenth are different jobs. The endpoint is already wired to a widget someone is looking at, so the question is not "what shape should this be" but "what is the smallest change that gets the new thing on screen without breaking what is already there".

Nothing here replaces the per-widget references — every shape lives in `references/<widget>.md`. This file is the map from *what the user asked for* to *which part of the payload changes*, plus the two things you cannot see from the JSON: what is safe to change on a live endpoint, and when the setup file has to change too.

## 1. Orient before you edit

1. **Read `.dashboardbase/*.json` in the repo.** The setup file names each widget's `type` and the endpoint it is wired to — that is the fastest way to know which widget you are editing and which reference to load. It is also the file you may have to update in step 4.
2. **No setup file?** `curl` the live endpoint and match the shape of its `data` object against the widget references. The `data` keys are distinctive: `datasets` + `labels` is a chart, `rows` + `columns` is a Table, `value` + `max` is a Gauge.
3. **Load that widget's `references/<widget>.md`** and keep it open. Its "Resolved JSON schema" is the contract, and its "Variations" section usually already shows the thing being asked for.

Do not guess the widget type from the endpoint's name. A route called `/widgets/revenue` can be a KPI, a BarChart or a LineChart, and each takes a different `data` shape.

## 2. Find where the change goes

| The ask | What changes | Where to look |
| --- | --- | --- |
| Headline number, subtitle, or a coloured trend badge | `data.header` — `title`, `subtitle`, `badge` | `references/<widget>.md` → "Header" |
| A badge icon (`ArrowUp` / `ArrowDown`) | `data.header.badge.icon` | `SKILL.md` → `WidgetDataIcon` |
| A click-through link on the widget | envelope `actions` (**not** inside `data`) | `SKILL.md` → "Response envelope" |
| Change a colour | `badge.color`, dataset `color`, indicator `color` | `SKILL.md` → `WidgetDataColor` |
| Another bar, another line, a second series | a new entry in `data.datasets` | `references/<widget>.md` → "Multiple series" |
| Another column, row, slice, or list item | the widget's own arrays in `data` | `references/<widget>.md` → schema + "Variations" |
| Raise an alert from the endpoint | envelope `alert` **and a toggle in the editor** | `references/alerting.md` |
| Honour the dashboard's date-range selector | the `?dateRange=` query parameter | `references/hosting-and-http.md` |
| Split one endpoint into several, or merge several into one | route shape, then the setup file | `references/endpoint-layout.md` |

Two placements catch people out every time:

- **`actions` is an envelope key, not a `data` key.** Putting it inside `data` fails `additionalProperties: false` and the widget stops rendering.
- **`alert` is an envelope key too**, and lowercase, unlike every styling enum.

## 3. What is safe to change on a live endpoint

The widget is being polled right now, so a bad deploy shows up as a broken tile rather than a failed build.

- **Keep the envelope to its four keys** — `title`, `actions`, `data`, `alert`. Adding a fifth is the most common way to break a working widget.
- **Adding is safer than renaming.** A new optional field inside `data` renders on the next poll. Renaming or removing a key the widget already renders blanks that part of the tile.
- **Keep parallel arrays in step.** On a chart, `labels` and every dataset's `data` array must stay the same length. Adding a series means adding a whole `datasets` entry — not lengthening one array and leaving the others behind.
- **Changing the widget *type* is not an edit.** A KPI that should become a LineChart is a different `data` contract, a different setup-file `type`, and a different widget in Dashboardbase. Treat it as a new widget and retire the old one.
- **Ship the change and the styling together.** A new series with no `color` picks a default that may collide with an existing one; give every series its own `color` when you add the second.
- **Do not change what the endpoint *means* without saying so.** Switching a KPI from all-time to last-30-days is invisible in the JSON and silently changes what the number on the wall means. Tell the user.

## 4. Does the setup file need updating?

| The change | Setup file | What the user has to do |
| --- | --- | --- |
| Anything inside `data`, `title`, `actions`, or `alert` | No change | Nothing — the next poll picks it up |
| New endpoint, or the endpoint's URL / query string changed | Yes | Re-import, or edit the datasource URL in the app |
| A new widget on the dashboard | Yes | Re-import, or add the widget in the app |
| Widget changed type | Yes | Re-import, or replace the widget in the app |
| Turning on alerting | No | **Enable alerts on that widget in the editor** — see `references/alerting.md` |

When the setup file does change, update `.dashboardbase/<slug>.json` in the repo in the same commit as the endpoint change, then hand it over the same way as a new dashboard (`references/setup-files.md` → "How to load your setup file into Dashboardbase"). Keeping the file in sync is what makes the dashboard reproducible; a repo whose setup file no longer matches the live dashboard is worse than no setup file.

## 5. Verify the change

1. `curl` the live endpoint and look at the **whole** response, not just the part you changed — it is easy to add a `header` and drop a key from `data` in the same edit.
2. Validate it. If `validate_widget_response` is available, pass it the full envelope; otherwise validate `data` against `assets/schemas/<widget>.json`.
3. Reload the widget in Dashboardbase and confirm the new element renders. If the tile went blank, the change added a key the schema does not allow — `references/gotchas.md` § 2.
4. If you touched `dateRange` handling, check the endpoint under two different ranges, not one.

## Common mistakes

- **Rewriting the endpoint instead of editing it.** The user asked for a badge, not a new handler. Match the surrounding code — the existing endpoint already shows the project's conventions for auth, config and response building.
- **Adding `actions` or `alert` inside `data`.** Both are envelope keys.
- **Adding a series without extending `labels`.** The chart renders with the new series truncated to the old label count.
- **Enabling alerting in code only.** The endpoint returning `alert` is half the job; the widget's alert toggle is the other half.
- **Editing the live dashboard in the app and leaving the setup file behind.** The next import then undoes the change.
