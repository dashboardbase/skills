# Endpoint layout: one per widget, or one shared endpoint

> **Load this file when:** deciding how many endpoints to build for a dashboard, implementing a single endpoint that serves several widgets, or wiring `?widget=` query strings into a setup file.

Dashboardbase polls one URL per widget. Nothing requires those URLs to be distinct *paths* — only distinct *URLs*. That gives you two layouts.

| | One endpoint per widget (default) | One shared endpoint |
|---|---|---|
| URL | `GET /api/v1/dashboard/widget/mrr` | `GET /api/v1/dashboard/widget?widget=mrr` |
| Routes to register | one per widget | one, total |
| Handler | one per widget, each returning one shape | one, dispatching on `widget` |
| Adding a widget | add a route | add a case |

Both are fully supported. Nothing else in Dashboardbase changes: the response envelope, the widget schemas, authentication, refresh intervals, and the setup file are identical either way.

## Which to choose

**Default to one endpoint per widget.** It is the recommended layout because:

- **Reuse across dashboards.** A dedicated endpoint can back the same widget on many different dashboards without any conditional branching on your side.
- **Smaller backend code.** Each endpoint does one thing and returns one shape, so there's no big switch statement deciding what to return.
- **Easy to wire up.** Each widget points at its own URL, so mapping a widget to its data is a single, obvious step.

**Choose one shared endpoint when the project pushes you there:**

- The app is deployed as a **single serverless function** or a single handler file (Vercel, Netlify, Lambda, Cloud Functions) and every new route means a new deployment unit.
- Registering routes is expensive — a gateway with per-route configuration, per-route auth policies, or a route budget.
- The widget handlers already share almost all their code (same database, same auth, same date-range parsing) and splitting them would mostly duplicate boilerplate.
- **The user asks for it.** This is a preference, not a correctness question — build what they want.

If the project doesn't obviously call for one, ask before building. Don't silently pick the shared layout to save yourself typing.

## Slugs

The shared layout needs a stable identifier per widget. Use a slug:

- lowercase, words joined with hyphens: `mrr`, `signups-per-day`, `top-customers`
- derived from what the widget *shows*, not from the widget type — `revenue`, not `kpi-1`
- **stable forever.** The slug is baked into the setup file and therefore into the dashboard's configuration. Renaming one means re-importing the setup file.
- unique within the endpoint, and never reused for a different metric.

The same slug is also the natural last path segment in the per-widget layout (`/widget/mrr`), which is what makes moving between layouts a pure URL edit.

## Implementing a shared endpoint

One route, a dispatch map, and an explicit failure for unknown values.

```js
const widgets = {
  "mrr":            (range) => buildMrr(range),
  "signups-per-day":(range) => buildSignups(range),
  "top-customers":  (range) => buildTopCustomers(range),
};

app.get("/api/v1/dashboard/widget", async (req, res) => {
  if (req.get("x-api-key") !== process.env.DASHBOARDBASE_KEY) return res.sendStatus(401);

  const build = widgets[req.query.widget];
  if (!build) return res.status(400).json({ error: `Unknown widget '${req.query.widget}'` });

  res.json(await build(req.query.dateRange ?? "ThirtyDays"));
});
```

The same shape in ASP.NET Minimal APIs:

```csharp
app.MapGet("/api/v1/dashboard/widget", async (string? widget, string? dateRange, CancellationToken ct) =>
{
    var range = WidgetDateRange.Parse(dateRange);
    return widget switch
    {
        "mrr" => Results.Ok(await BuildMrr(range, ct)),
        "signups-per-day" => Results.Ok(await BuildSignups(range, ct)),
        "top-customers" => Results.Ok(await BuildTopCustomers(range, ct)),
        _ => Results.BadRequest($"Unknown widget '{widget}'")
    };
});
```

Rules that hold regardless of language:

- **Read `widget` first, then `dateRange`.** They are independent parameters; `dateRange` never identifies the widget.
- **Unknown `widget` returns `400`.** Never `200` with an empty body — that renders as a blank widget and hides the typo.
- **`additionalProperties: false` still applies per widget type.** A shared handler makes it easy to leak a field from one widget's builder into another's payload. Each `?widget=` branch returns a different envelope, so validate every branch separately — one `validate_widget_response` call per branch if that tool is available, otherwise each branch's `data` against `assets/schemas/<widget>.json`.
- **One auth check now covers every widget.** That is a simplification, but it also means one mistake exposes all of them. See `references/authentication.md`.
- **`title`, `actions` and `alert` are per-widget** — build them inside each branch, not once at the top.

## How it lands in the setup file

`baseUrl` stays the host (plus any prefix every mapping shares). The discriminator goes in each mapping's `path`:

```json
{
  "baseUrl": "https://api.example.com",
  "mappings": [
    { "type": "kpi",  "path": "/api/v1/dashboard/widget?widget=mrr" },
    { "type": "line", "path": "/api/v1/dashboard/widget?widget=signups-per-day" },
    { "type": "table","path": "/api/v1/dashboard/widget?widget=top-customers" }
  ]
}
```

A `path` containing a query string is valid, validates, and round-trips through export unchanged. When the dashboard has a date range selector active, Dashboardbase appends `dateRange` to whatever URL you configured — with `&`, because a query string is already there:

```
GET https://api.example.com/api/v1/dashboard/widget?widget=mrr&dateRange=SevenDays
```

See `references/setup-files.md` for the full mapping states and a complete shared-endpoint example.

## Switching layouts later

Moving between layouts is a URL change and nothing more:

1. Implement the routes in the new layout.
2. Edit each mapping's `path` in the setup file (`/widget/mrr` ⟷ `/widget?widget=mrr`).
3. Re-import.

Widgets keep their ids, their positions, and their history — no widget is recreated. You can also mix layouts in one dashboard: some mappings pointing at dedicated paths, others at a shared endpoint, even across different `datasources[]` entries.

## Common mistakes

- **Putting the discriminator in `baseUrl`.** `baseUrl` is shared by every mapping, so `https://api.example.com?widget=mrr` sends every widget the same value. It belongs in `path`.
- **Reusing `dateRange` as the discriminator.** Dashboardbase controls that parameter; it will be overwritten.
- **Naming the parameter something Dashboardbase sends.** Avoid `dateRange`. `widget` is the conventional name; anything else you pick works as long as the setup file and the handler agree.
- **Returning `200` for an unknown widget.** The widget renders empty and the misconfiguration is invisible. Return `400`.
- **Forgetting one branch's `header`.** With N shapes behind one route it's easy to ship one without the styling the rest have. Check every branch against "Make it look good" in `SKILL.md`.
