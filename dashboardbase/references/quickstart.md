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
import { createPublicKey, verify } from "node:crypto";

const app = express();
// Dashboardbase signs every request with your workspace's Ed25519 key. Paste your public key below —
// a generated plan already contains it, or print it with
//   curl https://api.dashboardbase.com/keys/<workspace-id>.pem
// (workspace id is at app.dashboardbase.com/workspace). This is a public key. It is safe to commit.
const KEY = createPublicKey(`-----BEGIN PUBLIC KEY-----
MCowBQYDK2VwAyEA<your workspace's key>
-----END PUBLIC KEY-----`);
const TOLERANCE_SECONDS = 300;
// Your own public origin — never derive this from the request's Host header.
const ORIGIN = "https://abc123.ngrok.app";

function auth(req, res, next) {
  const ts = Number(req.get("x-dashboardbase-timestamp"));
  const sig = req.get("x-dashboardbase-signature");
  const prev = req.get("x-dashboardbase-signature-previous");
  if (!ts || !sig) return res.sendStatus(401);
  if (Math.abs(Date.now() / 1000 - ts) > TOLERANCE_SECONDS) return res.sendStatus(401);

  const check = (path, signature) =>
    signature && verify(null, Buffer.from(`${req.method.toUpperCase()}|${ORIGIN}${path}|${ts}`), KEY,
                        Buffer.from(signature, "base64"));

  // No query string, no trailing slash. Frameworks differ on whether the path they hand you is
  // percent-decoded, so try both forms — for a plain ASCII path they are identical.
  const raw = (req.originalUrl.split("?")[0] || "/").replace(/(.)\/$/, "$1");
  // Skip the decoded form when it holds an encoded separator, or decoding could merge path segments.
  const decoded = /%2f|%5c/i.test(raw) ? raw : decodeURIComponent(raw);
  const ok = [raw, decoded].some(p => check(p, sig) || check(p, prev));

  if (!ok) return res.sendStatus(401);
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

> **Prefer one environment variable to a committed key file?** Dashboardbase also sends your workspace's
> Endpoint Secret as `x-dashboardbase-secret` on every request. Read `DASHBOARDBASE_ENDPOINT_SECRET`
> from the environment and compare it instead — a few lines shorter, and equally supported. Implement
> one or the other, never both. See `references/authentication.md`.

## Step 2 — Expose with HTTPS

For local development:

```bash
ngrok http 3000
```

Copy the `https://` URL ngrok prints (e.g. `https://abc123.ngrok.app`).

## Step 3 — Wire in Dashboardbase

1. Create a new dashboard.
2. Add a KPI widget. Set the URL to `https://abc123.ngrok.app/widgets/mrr` and save — no headers to configure, the signature is sent automatically.
3. Add a Table widget. Set the URL to `https://abc123.ngrok.app/widgets/top-customers`, save.

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

Share this file. Ask the user how they want it: you can POST it to `/tools/v1/setup-links` and send them the short URL that comes back — they open it, preview what will be created, and import from there — or they can drag-drop or paste the file themselves. Uploading their config is their call, so ask before you do it. See `references/setup-files.md` for all four options. They provide credentials in the import flow; the file itself stays credential-free.

## Step 5 — Before going live

Walk through `references/go-live-checklist.md`. The key items: HTTPS, secret rotation, monitoring, realistic `refreshInterval` vs upstream cost.

## Where to go from here

- **More widget types:** see `references/<widget>.md` for one file per widget.
- **Serving several widgets from one route:** if you're on a single serverless function, one endpoint taking `?widget=<slug>` may suit you better than a route per widget — see `references/endpoint-layout.md`.
- **Multiple datasources:** see `references/setup-files.md` → "Example with multiple datasources".
- **Real-time notifications:** see `references/events.md` to push events from your backend.
- **Production hosting concerns:** see `references/hosting-and-http.md`.
