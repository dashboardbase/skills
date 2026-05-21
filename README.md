
<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="logo-light.png">
    <img src="logo-dark.png" width="180" alt="Dashboardbase">
  </picture>
</p>

<h1 align="center">Dashboardbase Skill</h1>

<p align="center">
  <strong>Turn any REST API into a live dashboard — without writing frontend code.</strong>
</p>

<p align="center">
  <a href="https://agentskills.io"><img src="https://img.shields.io/badge/Agent%20Skills-compatible-2563eb" alt="Agent Skills compatible"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-22c55e" alt="MIT License"></a>
  <a href="https://dashboardbase.com"><img src="https://img.shields.io/badge/dashboardbase.com-0a0a0a" alt="Dashboardbase"></a>
</p>

This is the official [Agent Skill](https://agentskills.io) for [Dashboardbase](https://dashboardbase.com). Add it to your AI tool (Claude Code, Cursor, or any skills-compatible agent) and it will scaffold API endpoints that return data correctly shaped for Dashboardbase widgets — KPIs, charts, tables, gauges, and more — on the first try.

> Generating a dashboard takes 5 minutes. Owning one takes forever. Dashboardbase hosts, renders, and ships your dashboards to mobile. This skill teaches your AI tool the JSON contract so the endpoints are right the first time.

---

## What this skill does

When this skill is active, your AI agent knows:

- **The JSON response contract** — the `title` / `actions` / `data` envelope every endpoint returns.
- **Per-widget schemas** — exactly what shape a KPI, Line Chart, Bar Chart, Pie Chart, Donut Chart, Gauge Chart, Table, or Status widget expects.
- **Authentication patterns** — API key via `x-api-key`, Basic Auth, and custom Authorization headers.
- **Error handling** — status codes, the `dateRange` query parameter, and how widgets degrade gracefully.
- **Webhook reactions** — the payload format for triggering real-time reactions on a live dashboard.
- **Setup files** — the declarative JSON format that provisions an entire dashboard's widgets and datasources in one drop.

Ask your agent _"build a Dashboardbase KPI endpoint for monthly revenue in Node"_ and you get a working, correctly-shaped endpoint — not a guess.

## Before / after

**Without the skill** — your agent invents a JSON shape, you paste it into Dashboardbase, the widget shows an error, you go read the docs, you fix it, repeat.

**With the skill** — your agent returns the exact contract, the widget renders live data immediately.

---

## Install

> The skill lives in the [`dashboardbase/`](dashboardbase/) folder of this repo. The folder name matches the skill's `name:` field, as required by the Agent Skills spec.

### Claude Code

Clone the repo and copy the skill into your skills directory:

```bash
git clone https://github.com/dashboardbase/dashboardbase-skill
cp -r dashboardbase-skill/dashboardbase ~/.claude/skills/
```

The skill activates automatically when you ask Claude Code to build a Dashboardbase endpoint.

> Skills directory paths can change between releases — check the [Claude Code skills docs](https://docs.claude.com) for the current location if the above doesn't pick it up.

### Cursor

Clone the repo and point Cursor at the skill folder:

```bash
git clone https://github.com/dashboardbase/dashboardbase-skill
```

Then load `dashboardbase-skill/dashboardbase/SKILL.md` into your Cursor skills/rules setup. See the [Agent Skills client showcase](https://agentskills.io) for your client's exact configuration.

### Any other skills-compatible agent

The skill is a standard [Agent Skills](https://agentskills.io) folder. Clone the repo and load the `dashboardbase/` directory however your client loads skills.

### No skills support? Paste it in.

If your AI tool doesn't support Agent Skills, paste the contents of [`dashboardbase/SKILL.md`](dashboardbase/SKILL.md) into the chat before asking it to build an endpoint. It works as a plain prompt.

### Reference by URL

The skill is fetchable directly. Point an agent at the raw file:

```
https://raw.githubusercontent.com/dashboardbase/dashboardbase-skill/main/dashboardbase/SKILL.md
```

---

## What's in the repo

```
dashboardbase-skill/
├── dashboardbase/              # The skill itself (auto-generated)
│   ├── SKILL.md                # Metadata + core instructions
│   ├── references/             # Per-widget schemas, auth, errors, webhooks
│   └── assets/                 # Setup-file JSON schema
├── assets/                     # Repo branding (logo)
├── README.md
├── LICENSE
└── .gitignore
```

## Keeping it current

This skill is **auto-generated from the Dashboardbase backend**. Widget schemas and example responses come straight from the production API contract, so the skill never drifts from what the platform actually accepts. Releases are tagged — track a [tagged version](https://github.com/dashboardbase/dashboardbase-skill/releases) for a stable reference, or `main` for the latest.

## Related

- **[Dashboardbase](https://dashboardbase.com)** — the product. Build, host, and share dashboards from your APIs.
- **[Documentation](https://dashboardbase.com/documentation)** — widget reference, JSON contract, webhook setup.
- **Dashboardbase MCP server** _(coming soon)_ — lets an agent talk to your Dashboardbase workspace directly, rather than just scaffold endpoints.

## Contributing

Found a gap or an error? Open an issue. The skill files are generated upstream — fixes to schemas or examples land in the Dashboardbase backend, but issues here are the right place to report them.

## License

MIT — see [LICENSE](LICENSE). Fork it, adapt it, ship it.

The MIT license covers the skill files in this repo. "Dashboardbase" is a trademark of Dashboardbase — see [dashboardbase.com](https://dashboardbase.com). You're free to use and adapt the skill; please don't use the name or branding in a way that implies official affiliation.
