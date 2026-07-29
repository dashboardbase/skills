# Extended quickstart

> **Load this file when:** a developer is starting from zero and wants a worked example beyond the inline KPI quickstart in `SKILL.md`. Walks through wiring two widgets (KPI + Table) end-to-end.

This guide builds a small Node.js service that exposes two widget endpoints — a KPI and a Table — wires them to Dashboardbase, and ends with a setup file you can share.

## Prerequisites

- Node 20+ (or any HTTPS-capable runtime; the principles are language-agnostic)
- A Dashboardbase organisation with permission to create dashboards
- A public HTTPS URL for your local service (e.g. via [ngrok](https://ngrok.com) for development)

## Step 1 — Skeleton

```js
import express from "express";

const app = express();
const KEY = process.env.DASHBOARDBASE_KEY;

function auth(req, res, next) {
  if (req.get("x-api-key") !== KEY) return res.sendStatus(401);
  next();
}

app.get("/widgets/mrr", auth, (_req, res) => {
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

app.get("/widgets/top-customers", auth, (_req, res) => {
  res.json({
    title: "Top customers",
    data: {
      headers: [
        { text: "Name", width: 50 },
        { text: "Plan", width: 25 },
        { text: "MRR", width: 25 }
      ],
      rows: [
        [
          { text: "Acme Corp" },
          { text: "Enterprise", badge: { text: "Pro", color: "Success", fill: "Solid" } },
          { text: "$2,400" }
        ],
        [
          { text: "Initech" },
          { text: "Team", badge: { text: "Pro", color: "Blue" } },
          { text: "$900" }
        ]
      ]
    }
  });
});

app.listen(3000);
```

## Step 2 — Expose with HTTPS

For local development:

```bash
ngrok http 3000
```

Copy the `https://` URL ngrok prints (e.g. `https://abc123.ngrok.app`).

## Step 3 — Wire in Dashboardbase

1. Create a new dashboard.
2. Add a KPI widget. Set the URL to `https://abc123.ngrok.app/widgets/mrr`, add header `x-api-key` with your key, save.
3. Add a Table widget. Set the URL to `https://abc123.ngrok.app/widgets/top-customers`, add the same header, save.

Both widgets should render within a few seconds.

## Step 4 — Ship as a setup file

To let users reproduce the same dashboard with one click, export a setup file. The `position` / `size` values are copied from the *Spotlight* layout in `references/setup-files.md` → "Recommended layouts" (the table takes the `Primary` slot, the KPI the `Kpi` slot) — don't invent grid values:

```json
{
  "$schema": "https://api.dashboardbase.com/setup-file/schema/v1.json",
  "version": 1,
  "baseUrl": "https://your-production-host.example.com",
  "refreshInterval": "5m",
  "mappings": [
    {
      "type": "kpi",
      "path": "/widgets/mrr",
      "size": { "w": 4, "h": 1 },
      "position": { "x": 8, "y": 0 }
    },
    {
      "type": "table",
      "path": "/widgets/top-customers",
      "size": { "w": 8, "h": 6 },
      "position": { "x": 0, "y": 0 }
    }
  ]
}
```

Save this file in your repo as `.dashboardbase/<slug>.json`, where `<slug>` is your dashboard name in lowercase with hyphens (e.g. `.dashboardbase/mrr-overview.json`). Keeping it in `.dashboardbase/` lets you version-control all your dashboard configs alongside your code.

Share this file. Users drag-drop it into Dashboardbase (or paste it; see `references/setup-files.md`). They provide credentials in the import flow; the file itself stays credential-free.

## Step 5 — Before going live

Walk through `references/go-live-checklist.md`. The key items: HTTPS, secret rotation, monitoring, realistic `refreshInterval` vs upstream cost.

## Where to go from here

- **More widget types:** see `references/<widget>.md` for one file per widget.
- **Serving several widgets from one route:** if you're on a single serverless function, one endpoint taking `?widget=<slug>` may suit you better than a route per widget — see `references/endpoint-layout.md`.
- **Multiple datasources:** see `references/setup-files.md` → "Example with multiple datasources".
- **Real-time notifications:** see `references/events.md` to push events from your backend.
- **Production hosting concerns:** see `references/hosting-and-http.md`.
