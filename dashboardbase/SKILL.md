---
name: dashboardbase
description: Use when designing or creating a dashboardbase dashboard end-to-end (which widgets, which layout — with or without an account), or when building, wiring, or debugging an HTTP endpoint that dashboardbase polls to render a widget (BarChart, Clock, ContributionsGrid, Countdown, DonutChart, GaugeChart, Image, KPI, LineChart, PieChart, ProgressList, Status, Table, Text), writing a setup file or setup link, configuring or changing authentication, or sending events/notifications. Use it too when changing a live endpoint: adding alerting, action links, a header or badge, another chart series, or an event webhook. Covers the full workflow (design from a ready-made recipe or the project's data, choose widgets, pick a layout, build endpoints, write the setup file, validate, hand over as a link or a file), the response envelope, JSON schemas per widget, the recommended authentication (verifying the Ed25519 signature dashboardbase already sends), refresh intervals, hosting requirements, and a go-live checklist.
license: Proprietary. Free to use; not open source. LICENSE has complete terms.
metadata:
  source-repo: dashboardbase-api
  generated-at: "2026-11-06T06:54:00Z"
  api-version: "1.0.0"
  spec-version: "1.0"
---

# dashboardbase Developer Skill

This skill teaches you to design a dashboardbase dashboard — which widgets, in which layout — to build the HTTP endpoints whose JSON responses dashboardbase renders as those widgets, and to hand the result over as a setup file the user uploads or a setup link they click.

## Mental model

dashboardbase periodically polls each widget's configured HTTPS endpoint at the dashboard's `refreshInterval` (one of `1m`, `5m`, `10m`, `30m`). Your endpoint returns JSON in a fixed envelope; dashboardbase parses it and renders the widget. There is no streaming and no callback — your endpoint is just a regular `GET` that returns the current snapshot of data.

## Creating a dashboard end-to-end

When the task is a whole dashboard (not a single widget), follow this workflow:

1. **Design the dashboard first — propose, don't interview.** A request like "make me a dashboard" gets a concrete proposal, not a list of questions. Work out what the product is and who will read the board, then:
   - **A domain is recognisable** (SaaS, a store, an API or backend, an app, a client report, an office TV) → load `references/dashboard-recipes.md` and start from the matching recipe: a finished widget list with a layout that already fills the board.
   - **No recipe fits** → design from the project itself. Read the models, the billing or analytics integration, the admin pages and existing reports, list what can actually be counted, and pick a widget for each (step 2).
   - **Aim for one screen:** a row of 3–6 headline numbers, one main chart, one or two supporting widgets — 5 to 8 widgets in total. A board that needs scrolling, or a ninth widget, is usually two dashboards.
   - **Only show what the project can feed.** A widget whose number you would have to invent is left out or swapped, never faked.

   Show the user the proposal as a short list — title · widget type · what it shows — name the recipe or layout, and build on a yes.
2. **Choose widgets.** Pick one widget type per *thing being shown* — not one per number. Several numbers describing the same data belong in one widget's `headers` strip above it (up to 6 blocks including `header`), so "pageviews per day, plus totals for pages, users and organizations" is **one** LineChart, not a LineChart and three KPI tiles. Reach for a second widget when the second thing has its own shape — a list, a breakdown, a status — not when it is another number about the first.

   Match the widget to the question the data answers:

   | The data answers… | Use | Rather than |
   | --- | --- | --- |
   | "What is the number right now?" | **KPI** — add `trend` for its recent shape, `progress` for a goal | A chart with one point |
   | "How is it changing over time?" (7+ points) | **LineChart** | A BarChart of 30 daily bars |
   | "How do a few categories or periods compare?" | **BarChart** — stacked for parts of a total | A pie with more than 6 slices |
   | "How is one total split?" (2–6 parts) | **DonutChart** or **PieChart** | Either, for values that don't sum to a whole |
   | "How close to a limit or a goal?" | **GaugeChart** for one, **ProgressList** for several | One KPI per goal |
   | "Which ones?" — rows with a few attributes, a top 10 | **Table** | A chart with 10 unlabelled series |
   | "Is it OK?" | **Status** — one indicator per widget | A KPI showing the word "Up" |
   | "How active, day by day, over months?" | **ContributionsGrid** | A 365-point line |
   | A date to count down to, the time, a notice, a picture | **Countdown**, **Clock**, **Text**, **Image** | — |

   The full list, with each widget's setup-file `type`:

- **BarChart** (setup-file type `bar`) — building a bar chart / grouped or stacked bars / comparison by category / bars with a rate line (combo chart).
- **Clock** (setup-file type `clock`) — building a clock / current-time / timezone tile.
- **ContributionsGrid** (setup-file type `contributions`) — building a GitHub-style contributions / activity heatmap grid.
- **Countdown** (setup-file type `countdown`) — building a countdown / days-until / time-remaining tile.
- **DonutChart** (setup-file type `donut`) — building a donut chart / ring breakdown.
- **GaugeChart** (setup-file type `gauge`) — building a gauge / dial / progress indicator.
- **Image** (setup-file type `image`) — building an image / meme / GIF / picture tile.
- **KPI** (setup-file type `kpi`) — building a single-number / headline / KPI tile.
- **LineChart** (setup-file type `line`) — building a line chart / time series / trend over time.
- **PieChart** (setup-file type `pie`) — building a pie chart / share-of-total / breakdown.
- **ProgressList** (setup-file type `progress`) — building a list of labelled progress / goal / capacity bars.
- **Status** (setup-file type `status`) — building a status / health / up-or-down indicator.
- **Table** (setup-file type `table`) — building a table / list / rows-and-columns widget.
- **Text** (setup-file type `text`) — building a text / quote / announcement / message-of-the-day tile.

<details>
<summary><b>One widget or four? — a worked example</b></summary>

> "Line chart of pageviews per day, also summarize total pages, total users, total organizations"

That is one LineChart. The series is pageviews per day; the three totals are the header strip:

```json
{
  "title": "Traffic",
  "data": {
    "header": { "title": "12,480", "subtitle": "Pageviews", "badge": { "text": "+8%", "icon": "ArrowUp", "color": "Success" } },
    "headers": [
      { "title": "1,284", "subtitle": "Total pages" },
      { "title": "318", "subtitle": "Total users" },
      { "title": "96", "subtitle": "Total organizations" }
    ],
    "labels": ["2026-05-15", "2026-05-16"],
    "datasets": [{ "label": "Pageviews", "data": [{ "value": 1840 }, { "value": 1610 }] }]
  }
}
```

One endpoint, one widget, one slot on the grid — instead of four endpoints and four tiles.

</details>

3. **Pick a layout, then make it fill the board.** A recipe already carries its layout. Otherwise load `references/setup-files.md` → "Recommended layouts", choose from "Choosing a layout" and copy each slot's `position` / `size` into the setup file — don't invent grid values, and put a widget only in a slot whose "fits" column names it. Then apply that file's "Composition rules": every row spans all 12 columns, the board is 6 rows tall, and no slot is left empty — an unused slot is a hole in the finished dashboard, so widen its neighbours instead. If no layout covers the mix, extend the closest one per "When no layout fits". Name the layout you picked when handing over (e.g. "arranged as *Spotlight*") so the user knows what to expect and can ask for a different arrangement.
4. **Choose an endpoint layout, then build or plan each endpoint.** Default to **one endpoint per widget** — dedicated endpoints can back the same widget on several dashboards, keep each handler to one response shape, and make wiring a single obvious step. Switch to **one shared endpoint** taking `?widget=<slug>` when the project is a single serverless function or one handler file, or when the user asks for it. Ask which they want when the project doesn't make it obvious, and load `references/endpoint-layout.md` before implementing the shared layout.
   - Endpoints already exist → wire them with mapping State A (existing widgets) or State B (new widgets); see `references/setup-files.md`.
   - You're building them now → implement them in the chosen layout using each widget's `references/<widget>.md`, then map with State B.
   - Endpoints can't be built yet → describe each widget with a State C `plan` mapping; the setup file still imports as the finished layout, each widget described, to implement later.
   - The endpoint can be built but its data source can't be reached yet → offer the user a choice: wait, or return realistic sample data in the right shape now (hardcoded, with a `TODO` naming the real source) so the dashboard is connected and looks finished today. Only on their say-so, and say at handover exactly which widgets show sample numbers — never pass them off as real.
5. **Write the setup file** per `references/setup-files.md` and save it as `.dashboardbase/<dashboard-slug>.json` in the repo — named after the dashboard, never a generic `dashboard.json`. Write it as part of the build, as soon as the endpoint paths are settled; only the handover in step 7 waits for live endpoints. If that path already exists and describes a different dashboard, pick a distinct slug rather than overwriting it — two near-identical file names in one directory is the failure mode to avoid. That directory is also the first thing to read when you join an existing project: an existing setup file gives you the `baseUrl` and the path convention outright.
6. **Validate before handing over.**
   - **If the `validate_widget_response` and `validate_setup_file` tools are available** (the dashboardbase MCP server is installed), use them — they check against the live contract, so they cannot drift from what the platform accepts. Pass `validate_widget_response` the **full response envelope** (`title` / `actions` / `data` / `alert`), not just `data`. No account or API key is needed.
   - **Otherwise**, validate each endpoint response's `data` against `assets/schemas/<widget>.json`, and the setup file against `assets/setup-file.schema.json` (or `POST /bff/v1/organizations/{orgId}/setup-files/validate`).

   Then run the validation loop below.
7. **Hand it over — a dashboard is not delivered until the user knows how to open it.** There are two routes and the choice is the user's — **never upload their config without being asked to**:

   > The dashboard config is saved at `.dashboardbase/<slug>.json`. I can upload it and give you a link — you click it, see a preview and import it — or you can upload the file in dashboardbase yourself. Shall I create the link?

   **Route 1 — a setup link they click.** If the `create_setup_link` tool is available (the dashboardbase MCP server), call it with the file — it posts it and hands back the link with what to tell the user. Otherwise `POST` the file as the raw body to `https://api.dashboardbase.com/tools/v1/setup-links` — no account, no API key, no org ID, and no `{"content": …}` wrapper. You get back `id`, `url` (e.g. `https://app.dashboardbase.com/i/V1v3rZ8Qk2mN4pR6tY8uWx`) and `expiresAt`.

   **Route 2 — they upload the file.** Nothing to post. Tell them where the file is and what to do with it: sign in at app.dashboardbase.com, press **Import config** on the home page, then drop `.dashboardbase/<slug>.json` onto the box, press **Upload file**, or paste its contents.

   **Either way, say what happens next** — a bare link or a bare path is not a handover. In your own words:

   - **Clicking the link is safe.** It opens a preview — the dashboard's name, its widgets and the hosts they pull from. Nothing is created until they press **Import to a workspace**.
   - **No account is needed to look, and none is needed yet.** Pressing Import asks them to sign in or sign up; a first-time user creates a workspace and is brought straight back to the import with the file already loaded.
   - **The import takes it from there:** credentials if the endpoints need any (entered there, never in the file), a test call to every endpoint, then **Import**. The dashboard opens in the editor with every widget placed; **Publish** makes it live.
   - **The link expires in 48 hours** and works more than once, so it can be forwarded to teammates. It is a handover, not storage — the file stays in the repo and a fresh link is one call away.
   - **The link is the only thing protecting the file:** anyone holding the URL can read it. Send it directly rather than pasting it into a public issue or channel.

   If the POST comes back `400` with `reason: "credentials_detected"`, the file carries something credential-shaped — a `headers` object, an `apiKey`, a `user:password@host` base URL, a `?api_key=…` parameter, or a pasted provider key. The response lists the offending fields. **Remove them and re-post; do not try to work around the check.**

   Full request/response, limits and failure cases: `references/setup-files.md` → "How to load your setup file into dashboardbase".

## No dashboardbase account yet

Nothing in this skill needs an account until the moment of import, so never stop to ask for one. Without it you can still design the board, build and validate every endpoint, write the setup file and create the setup link; the user signs up when they press Import (a free trial, no card needed). Three things change:

- **Authenticate with the Endpoint Secret, not request signing.** The signing key only exists once there is a workspace, so there is nothing to verify against yet. Write the verifier to read `DASHBOARDBASE_ENDPOINT_SECRET` from the environment and fail closed while it is unset (`references/authentication.md` → "Endpoint Secret"). An endpoint that must already be protected by something the user controls can use an API key header instead — they choose the value and enter it in the import flow.
- **Tell the user the one step that remains after signing up:** copy the Endpoint Secret from **Workspace → Secrets** (app.dashboardbase.com/workspace) into the endpoint's environment and redeploy. Until then the import still goes through, but the widgets show an authentication error — say so, so it does not look broken.
- **Let them see the result before signing up.** Paste any endpoint response into app.dashboardbase.com/validate — it needs no account, and its Preview tab renders the JSON as the real widget.

Endpoints cannot be built yet either? Hand over a plan-only setup file (State C, see `references/dashboard-recipes.md` → "A recipe as a setup file"): it imports as the finished layout with each widget described, ready to be wired later.

## Changing an endpoint that is already live

Not every task is a new dashboard. Adding a header or badge, an action link, a different colour, another bar or line, another table column, an alert, or another way of authenticating to an endpoint that is already being polled is the same contract, approached from the other end: find the smallest change, keep the rest of the payload intact, and know whether anything outside the code has to change too.

Load `references/modifying-endpoints.md` — it maps each kind of ask to the part of the payload it touches, lists what is safe to change on a live endpoint, and says when the setup file has to be updated and re-imported. For an auth change specifically, load `references/authentication.md` → "Changing or adding an authentication method on a live endpoint".

Two things are worth knowing without loading anything:

- **Adding alerting is half code, half configuration.** Returning `alert` in the envelope does nothing until the user enables alerts on that widget in the dashboard editor, and the dashboard is published. Always say so when you hand the change over. See `references/alerting.md`.
- **Changing how an endpoint authenticates is code *and* configuration, in that order.** Deploy the endpoint accepting both the old and the new credential, update the credential on every datasource pointing at it, and only then drop the old one. Reverse that order and every widget on the endpoint 401s. Request signing and the Endpoint Secret are the exceptions with nothing to configure — both are already sent on every request. See `references/authentication.md`.
- **A payload-only change needs no re-import.** Anything inside `title`, `actions`, `data` or `alert` is picked up on the next poll. Only a new endpoint, a changed URL, or a new or retyped widget means updating `.dashboardbase/<slug>.json` and importing it again.

## Response envelope (always true)

Every widget endpoint returns this shape:

```json
{
  "title": "Optional widget title",
  "actions": [
    { "title": "View Details", "type": "link", "url": "https://example.com/details" }
  ],
  "data": {},
  "alert": { "active": true, "level": "critical", "message": "Error rate above the 5% threshold" }
}
```

- `title` (optional string) overrides the widget title configured in dashboardbase.
- `actions` (optional array) renders link actions in the widget header. `type` is currently always `"link"`. `url` must be an absolute `https://` URL.
- `data` (required) is the widget-type-specific payload — see `references/<widget>.md` for each widget's exact shape.
- `alert` (optional object) renders an alert banner on the widget and feeds alert notifications. Shape: `active` (boolean — `true` shows the banner, `false` clears it), `level` (one of `info`, `success`, `warning`, or `critical` — note these are lowercase, unlike the styling enums), and `message` (string; truncated beyond 200 characters). Works the same on every widget type.

These four keys are the only ones allowed at the envelope level; `additionalProperties: false` applies to the `data` payload (see gotcha 2 below).

## Request: the `dateRange` query parameter

dashboardbase appends a `?dateRange=<value>` query parameter to your endpoint URL whenever the dashboard has a date range selector active. If you ignore it, your endpoint returns the same data regardless of what the user picks — silent footgun.

Supported values:

| `DateRange` value |
| --- |
| `Today` |
| `SevenDays` |
| `ThirtyDays` |
| `SixtyDays` |
| `NinetyDays` |

Not every widget has a period: **Clock**, **Countdown** and **Image** have no time window and no `header`, so their endpoints take no `dateRange`, and their payload carries no headline, badge or date semantics — ignore the parameter if it arrives. Everywhere else, your endpoint should map these values to a time window (e.g. `SevenDays` → "last 7 days, ending now") and filter the underlying data accordingly. `Today` means the current calendar day. If the parameter is absent, return your sensible default (typically `ThirtyDays` for trend widgets, "all-time" for KPI counts). Echo the selected window in the widget's `header.subtitle` (e.g. `"Last 7 days"`) so viewers can see which range is applied.

```bash
curl "https://your-api.example.com/widgets/revenue?dateRange=SevenDays"
```

See `references/hosting-and-http.md` for handling defaults, mapping the values to SQL window functions, and caching considerations.

## Quickstart — your first KPI in 5 minutes (Node.js)

```js
import express from "express";
const app = express();

app.get("/widgets/mrr", (req, res) => {
  // dashboardbase signs every request; verify it. See references/quickstart.md for verifyDashboardbase.
  if (!verifyDashboardbase(req)) return res.sendStatus(401);
  res.json({
    title: "MRR",
    actions: [{ title: "Open Stripe", type: "link", url: "https://dashboard.stripe.com" }],
    data: {
      header: {
        title: "$42,300",
        subtitle: "vs last month",
        badge: { text: "+8%", icon: "ArrowUp", color: "Success" }
      }
    }
  });
});

app.listen(3000);
```

Verify it:

```bash
curl https://your-api.example.com/widgets/mrr
```

Connect it in dashboardbase: create a KPI widget, point it at this URL and save — no headers to configure, the signature is sent automatically. The `KEY` constant needs your workspace's public key: ask the user to paste it from **Workspace → Secrets → Signing Key** (app.dashboardbase.com/workspace) before you write the verifier, and check what they paste per `references/authentication.md` → "Accepting a pasted key". See `references/quickstart.md` for the full walkthrough including a Table widget.

## Make it look good

Dashboards read dramatically better when every widget uses the styling surface the contract provides:

- **Always include `header` on KPI, LineChart and BarChart** (`title` + `subtitle` + `badge`), even though the schema marks it optional on the charts: `title` = the headline aggregate for the period, `subtitle` = the context line (`"Last 7 days"`), `badge` = the colored trend (`{ "text": "+8%", "icon": "ArrowUp", "color": "Success" }`). A plotted series without a headline and a period is the single biggest cause of a dashboard looking unfinished. On every other widget a `header` is genuinely optional — a pie, gauge, table or status tile reads fine without one, so add it only when there is a headline worth showing. Clock, Countdown and Image have no `header` at all in their contract; adding one there fails `additionalProperties: false`.
- **Reach for `headers` before you reach for another widget.** Every widget that takes a `header` also takes `headers` — an array of the same block, rendered as an equal-width strip above the content. `header` merges in front of it, so `header` plus `headers` may total at most **6**. Four numbers above one line chart is one endpoint and one widget, not four KPI tiles and four endpoints; it is usually the right shape when the numbers describe the same thing the chart plots. On a phone the strip wraps to two per row and the widget grows taller to fit.
- **Color lives on badges and datasets, not in text.** `subtitle` renders as plain text; use `badge.color` with `ArrowUp` / `ArrowDown` for the colored accent, and per-dataset `color` to distinguish chart series.
- **Echo the active date range in `header.subtitle`** when handling `?dateRange=` — it shows users their filter is applied.
- **Keep axis labels on, and short.** LineChart and BarChart show the category labels by default; they stay level and thin themselves to fit, so a 30- or 90-day daily series reads fine. Send short labels (`"Sep 9"`, `"Mon"`, `"Jan"`), not timestamps. `ticksX: false` is for a sparkline-style tile with no room for an axis; `ticksY: true` adds value labels to the left.
- **Leave the chart's look to the defaults.** A LineChart is filled, drawn with straight segments, shows dots only on a short series and carries faint value gridlines without being asked; a BarChart carries the same gridlines. `fill`, `smooth`, `points` and `grid` exist to opt out (or into curves) — omit them unless the developer asks for a different look.
- **Give a KPI its recent shape with `trend`** when the endpoint has the readings anyway: `"trend": [...]`, a plain array of numbers oldest first, ending with the headline value — or `{ "values": [...], "color", "smooth", "fill" }` when the line needs styling. A Table cell takes the same `trend`. It draws a sparkline beside the number where the tile has room and is left out where it doesn't — never move a number into a chart to get one.
- **Show a goal or a baseline as data, not decoration.** A goal, quota, budget or SLA goes in `target: { "value": 50000, "label": "Q3 goal" }` on a LineChart or BarChart — one labelled line the axis always stretches to include. A previous period on a LineChart is an extra dataset with `"style": "Comparison"` (a dashed grey line behind the current one, compared in the tooltip). Send either only when the goal asks for it; never fake one with a flat or duplicate series.
- **Stack parts of a total; put a rate on its own axis.** When BarChart series are parts of one whole in the same unit (MRR by plan, visitors by channel), send `"stacked": true` and the total in `header.title`. When the goal pairs a volume with a rate (signups and conversion, orders and AOV), keep the volume as bars and send the rate as a dataset with `"type": "Line"` and `"axis": "Right"`, its unit in each value's `postfix`. Never stack rates or mixed units, and never plot a percentage on the same axis as counts.
- **Add link `actions`** so a widget clicks through to the underlying tool (Stripe, your admin, a runbook).
- **Design for the big screen too.** A dashboard can be shown on a wall-mounted TV (paired as a display device) and viewed from across a room. Nothing extra to build — a TV renders the same widget endpoints — but the styling above is exactly what makes it read at a distance: large headline numbers, high-contrast badges, and an echoed date range beat dense tables and tiny labels on a 55" screen.

Each widget reference's "Header" section shows the exact shape.

## When to load which reference

Load only what the current task needs — these files are progressive disclosure, not preloaded.

| Trigger | File |
|---|---|
| Creating a **complete dashboard** from scratch | Follow "Creating a dashboard end-to-end" above, with `references/dashboard-recipes.md` for what goes on it and `references/setup-files.md` for layouts and the setup file |
| **Deciding what goes on a dashboard** — the user named a domain, or only said "a dashboard" | `references/dashboard-recipes.md` |
| The user has **no dashboardbase account** yet | "No dashboardbase account yet" above |
| Building a **BarChart / Clock / ContributionsGrid / Countdown / DonutChart / GaugeChart / Image / KPI / LineChart / PieChart / ProgressList / Status / Table / Text** endpoint | `references/<widget>.md` (e.g. `references/kpi.md`, `references/bar-chart.md`) |
| Writing a **setup file** (declarative dashboard config) | `references/setup-files.md` (and `assets/setup-file.schema.json` for full schema) |
| **Handing a finished dashboard to the user** — a setup link they click, or the file they upload | `references/setup-files.md` → "How to load your setup file into dashboardbase" |
| Deciding between **one endpoint per widget** and **one shared endpoint** (`?widget=`) | `references/endpoint-layout.md` |
| Choosing, changing or adding **authentication** — a new method, a second method alongside it, or a rotation | `references/authentication.md` |
| **Changing an endpoint that is already live** — adding a header or a header strip, badge, action link, colour, another series or column | `references/modifying-endpoints.md` |
| Adding **alerting** to a widget, or an alert that isn't firing | `references/alerting.md` |
| Pushing **events / notifications / sounds** to a dashboard, or wiring an event webhook into an existing codebase | `references/events.md` |
| Production **hosting** / TLS / status codes / latency | `references/hosting-and-http.md` |
| Endpoint returns 200 but widget is broken — **debugging** | `references/gotchas.md` |
| Shipping to **production** | `references/go-live-checklist.md` |
| **First-time** users wanting an extended walkthrough | `references/quickstart.md` |

The widget reference filenames are: `bar-chart.md`, `clock.md`, `contributions-grid.md`, `countdown.md`, `donut-chart.md`, `gauge-chart.md`, `image.md`, `kpi.md`, `line-chart.md`, `pie-chart.md`, `progress-list.md`, `status.md`, `table.md`, `text.md`.

## Always-true rules (no need to read anything to apply these)

- **HTTPS only.** Endpoints must be reachable over `https://` with a valid certificate.
- **Respond within 10 seconds.** A slower attempt is a failure. Transient failures are retried 3 times with backoff, so one poll of a struggling endpoint is 4 requests over ~50 seconds — budget rate limits accordingly (`references/hosting-and-http.md`).
- **`200 OK` with a JSON body is the only success.** There is no "empty" or "unchanged" outcome: `204 No Content` and `304 Not Modified` both render the widget in an error state, redirects are never followed, and any `4xx` / `5xx` errors too. When you have nothing to show, say so inside a `200` — see gotcha 8 in `references/gotchas.md`.
- **Refresh intervals** are limited to `1m`, `5m`, `10m`, `30m`.
- **Enums are case-sensitive.** Use `"Success"`, not `"success"`. See the styling tables below. (The one lowercase enum is the envelope's `alert.level`: `"critical"`, not `"Critical"`.)
- **`additionalProperties: false`.** Extra fields not in the schema cause render failures. Strip them before responding.
- **Idempotent `GET`.** The same request should yield the same data (modulo time). Do not mutate state.
- **Query strings in a widget URL are preserved.** `dateRange` is appended to whatever URL you configure, so a widget wired to `?widget=mrr` is polled as `?widget=mrr&dateRange=SevenDays`. Read both.
- **Know what the widget exposes.** Before settling on an auth method, state in one line what the endpoint actually returns. A widget serving personal data — names, email addresses, customer records — or anything else the project would not publish deserves a deliberate decision rather than the default, so call it out to the user; the project's own `SECURITY.md` may describe the feed as aggregate-only.
- **An existing convention beats the recommended default.** Where the project already checks a particular auth header on its other widget endpoints, or already uses a route prefix or folder layout, reuse it and ask "reuse X?" rather than offering the menu below.
- **Alerting needs a toggle you cannot set from code.** An `alert` in the envelope renders and feeds notifications only once the user enables alerts on that widget in the dashboard editor, on a published dashboard. Returning `alert` is never enough on its own — tell the user to switch it on.
- **Recommended auth:** verify the Ed25519 signature. dashboardbase signs every request with your workspace's key and sends `x-dashboardbase-timestamp` and `x-dashboardbase-signature` — you verify against a public key committed to the repo, so there is no secret to hold and nothing to configure on the datasource. **Get the key before writing the verifier:** a generated plan or issue already carries it; otherwise ask the user to paste it from **Workspace → Secrets → Signing Key** (app.dashboardbase.com/workspace), and normalise what arrives per `references/authentication.md` → "Accepting a pasted key" — never scaffold around a placeholder. Prefer the Endpoint Secret instead if one environment variable suits you better; implement exactly one. API key and basic auth still work and are sent in addition. See `references/authentication.md`.

## Validation loop — before declaring done

1. `curl` the endpoint and confirm it returns `200` with a JSON body, in under 10 seconds.
2. Validate the response. If `validate_widget_response` is available, call it with the **full response body** — it checks the envelope and the `data` payload against the live contract. Otherwise JSON-validate the response's `data` field against the bundled schema at `assets/schemas/<widget>.json` (the same schema is shown in `references/<widget>.md`).
3. Confirm it renders: open the widget in dashboardbase, or — before the dashboard is imported, or with no account — paste the response into app.dashboardbase.com/validate and look at its Preview tab. If it doesn't, load `references/gotchas.md`.
4. Confirm the configured `refreshInterval` is realistic given upstream rate limits.

## Top 5 gotchas (full list: `references/gotchas.md`)

1. **Enum casing.** `"color": "success"` is invalid — use `"Success"`.
2. **Extra fields fail silently.** `additionalProperties: false` applies to the `data` payload — an unrecognised key inside `data` turns the render off. Strip them. The envelope level allows exactly `title`, `actions`, `data`, and `alert`.
3. **Action URLs must be absolute HTTPS.** Relative URLs and `http://` are rejected.
4. **`datasets` is always an array.** Even single-series charts use `"datasets": [{ ... }]`. Multi-series is just multiple entries — see each chart widget's "Multiple series" section.
5. **`null` ≠ missing.** Most optional fields should be omitted rather than set to `null`. Returning `"color": null` may render differently from omitting `color`.

## Styling enums (authoritative — these are the only valid values)

These tables are reflected from the source — every value listed here is accepted, every value not listed is rejected.

### `WidgetDataColor` (used by badges, datasets, indicators)

| `WidgetDataColor` value |
| --- |
| `Success` |
| `Warning` |
| `Danger` |
| `Blue` |
| `Green` |
| `Red` |
| `Yellow` |
| `Orange` |
| `Light` |
| `Dark` |

### `WidgetDataAlign` (text alignment)

| `WidgetDataAlign` value |
| --- |
| `Left` |
| `Center` |
| `Right` |

### `WidgetDataSize` (text/icon size)

| `WidgetDataSize` value |
| --- |
| `S` |
| `M` |
| `L` |
| `XL` |

### `WidgetDataIcon` (badge / value icon)

| `WidgetDataIcon` value |
| --- |
| `ArrowUp` |
| `ArrowDown` |

### `WidgetDataFill` (badge fill style)

| `WidgetDataFill` value |
| --- |
| `Solid` |
| `Clear` |
| `Outline` |

### `WidgetActionType` (action types)

| `WidgetActionType` value |
| --- |
| `link` |

### `WidgetAlertLevel` (envelope `alert.level`)

Unlike the styling enums above, alert levels are **lowercase**:

| `WidgetAlertLevel` value |
| --- |
| `info` |
| `success` |
| `warning` |
| `critical` |

## Supported widgets

- BarChart
- Clock
- ContributionsGrid
- Countdown
- DonutChart
- GaugeChart
- Image
- KPI
- LineChart
- PieChart
- ProgressList
- Status
- Table
- Text

Each has its own reference file under `references/` — see the filename list above.

---

*Skill generated at `2026-11-06T06:54:00Z` from the dashboardbase API contract.*
