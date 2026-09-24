
<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="logo-light.png">
    <img src="logo-dark.png" width="180" alt="dashboardbase">
  </picture>
</p>

<h1 align="center">dashboardbase SKILL</h1>

<p align="center">
  <strong>Turn any REST API into a live dashboard — without writing frontend code.</strong>
</p>

<p align="center">
  <a href="https://skills.sh/dashboardbase/skills"><img src="https://skills.sh/b/dashboardbase/skills" alt="skills.sh install count"></a>
  <a href="https://agentskills.io"><img src="https://img.shields.io/badge/Agent%20Skills-compatible-2563eb" alt="Agent Skills compatible"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-22c55e" alt="MIT License"></a>
  <a href="https://dashboardbase.com"><img src="https://img.shields.io/badge/dashboardbase.com-0a0a0a" alt="Dashboardbase"></a>
</p>

This is the official [Agent Skill](https://agentskills.io) for [dashboardbase](https://dashboardbase.com). Add it to your AI tool (Claude Code, Cursor, or any skills-compatible agent) and it will scaffold API endpoints that return data correctly shaped for dashboardbase widgets — KPIs, charts, tables, gauges, and more — on the first try.

> Generating a dashboard takes 5 minutes. Owning one takes forever. dashboardbase hosts, renders, and ships your dashboards to mobile. This skill teaches your AI tool the JSON contract so the endpoints are right the first time.

---

## What this skill does

When this skill is active, your AI agent knows:

- **The JSON response contract** — the `title` / `actions` / `data` / `alert` envelope every endpoint returns.
- **Per-widget schemas** — exactly what shape each of the 14 widgets expects: KPI, Line Chart, Bar Chart, Pie Chart, Donut Chart, Gauge Chart, Table, Status, Progress List, Contributions Grid, Text, Clock, Countdown and Image.
- **Authentication** — verifying the Ed25519 signature Dashboardbase sends on every request (recommended: nothing to configure, no secret to hold), the Endpoint Secret, API keys and Basic Auth — and how to change methods on a live endpoint without breaking it.
- **Error handling** — status codes, the `dateRange` query parameter, and how widgets degrade gracefully.
- **Alerts and events** — the `alert` object that turns a widget red and sends notifications, and the payload for pushing real-time events to a live dashboard.
- **Setup files** — the declarative JSON format that provisions an entire dashboard's widgets and datasources in one drop, plus recommended layouts and a one-call import link to hand the finished dashboard over.

Ask your agent _"build a dashboardbase KPI endpoint for monthly revenue in Node"_ and you get a working, correctly-shaped endpoint — not a guess.

## Before / after

**Without the skill** — your agent invents a JSON shape, you paste it into dashboardbase, the widget shows an error, you go read the docs, you fix it, repeat.

**With the skill** — your agent returns the exact contract, the widget renders live data immediately.

---

## Install

### Recommended: one command

```bash
npx skills add dashboardbase/skills
```

This installs the skill into the correct directory for your agent (Claude Code, Cursor, and [70+ others](https://github.com/vercel-labs/skills#supported-agents)) and keeps it up to date. Pick a specific agent with `--agent`, e.g. `npx skills add dashboardbase/skills --agent claude-code`. See [skills](https://github.com/vercel-labs/skills) / [skills.sh](https://skills.sh) for details.

> The skill lives in the [`dashboardbase/`](dashboardbase/) folder of this repo. The folder name matches the skill's `name:` field, as required by the Agent Skills spec.

### Claude Code (manual)

Clone the repo and copy the skill into your skills directory:

```bash
git clone https://github.com/dashboardbase/skills
cp -r skills/dashboardbase ~/.claude/skills/
```

To share the skill with a team instead, copy it into a project's `.claude/skills/` directory — it then ships with the repo and is picked up by everyone working in it.

The skill activates automatically when you ask Claude Code to build a dashboardbase endpoint.

> Skills directory paths can change between releases — check the [Claude Code skills docs](https://docs.claude.com) for the current location if the above doesn't pick it up.

### Cursor

Clone the repo and point Cursor at the skill folder:

```bash
git clone https://github.com/dashboardbase/skills
```

Then load `skills/dashboardbase/SKILL.md` into your Cursor skills/rules setup. See the [Agent Skills client showcase](https://agentskills.io) for your client's exact configuration.

### Any other skills-compatible agent

The skill is a standard [Agent Skills](https://agentskills.io) folder. Clone the repo and load the `dashboardbase/` directory however your client loads skills.

### No skills support? Paste it in.

If your AI tool doesn't support Agent Skills, paste the contents of [`dashboardbase/SKILL.md`](dashboardbase/SKILL.md) into the chat before asking it to build an endpoint. It works as a plain prompt.

### Reference by URL

The skill is fetchable directly. Point an agent at the raw file:

```
https://raw.githubusercontent.com/dashboardbase/skills/main/dashboardbase/SKILL.md
```

---

## What's in the repo

```
skills/
├── dashboardbase/              # The skill itself (auto-generated)
│   ├── SKILL.md                # Metadata + core instructions
│   ├── references/             # Per-widget guides, setup files, auth, events, gotchas
│   └── assets/                 # JSON schemas (per-widget + setup file)
├── logo-dark.png               # Repo branding
├── logo-light.png
├── README.md
└── LICENSE
```

## Keeping it current

This skill is **auto-generated from the dashboardbase backend**. Widget schemas and example responses come straight from the production API contract, so the skill never drifts from what the platform actually accepts. Track `main` for the latest — it updates automatically as the contract evolves.

## Related

- **[Dashboardbase](https://dashboardbase.com)** — the product. Build, host, and share dashboards from your APIs.
- **[Documentation](https://app.dashboardbase.com/documentation)** — widget reference, JSON contract, webhook setup.
- **[Dashboardbase MCP server](https://github.com/dashboardbase/mcp)** — lets your agent check its own work: it validates widget responses and setup files against the live contract. Install with `claude mcp add dashboardbase -- npx -y @dashboardbase/mcp`. The skill uses it automatically when it is available.

## Contributing

Found a gap or an error? Open an issue. The skill files are generated upstream — fixes to schemas or examples land in the dashboardbase backend, but issues here are the right place to report them.

## License

MIT — see [LICENSE](LICENSE). Fork it, adapt it, ship it.

The MIT license covers the skill files in this repo. "dashboardbase" is a trademark of dashboardbase — see [dashboardbase.com](https://dashboardbase.com). You're free to use and adapt the skill; please don't use the name or branding in a way that implies official affiliation.
