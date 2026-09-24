# Guanchao (观潮) · Cross-market research workbench

[中文](README.md) · [MIT License](LICENSE)

This repository publishes **research process docs and an empty workbench skeleton** (A-shares / Hong Kong / US / macro). It is **not** a live market-data API and **not** a brokerage.

**Guanchao** is designed for [Grok Bot](https://cursor.com): session briefs (pre-open through close), holdings/watchlist mapping, sector theme cards, opportunity logs, and optional decision stances — always labeled **not investment advice**.

## Two ways to use it

| Path | Best for | You get |
|------|----------|---------|
| **Grok Bot public template** (recommended) | People with Grok Bot / Cursor | A ready “观潮” assistant with routines, skills, and workflow memories |
| **This repo** | Self-hosted notes / custom agents | Checklists under `docs/`, empty `market/` tree, skill Markdown |

They complement each other: the template **runs**; the repo is **reviewable and forkable**.

## Import the Grok Bot public template

> The author exports a **Public** template from Grok Bot. If you do not have an import link yet, ask the author for the published share URL, or search for **观潮 / Guanchao** in the Grok Bot template gallery.

### 1. Import

1. Open the author’s **Guanchao** public template link (or pick **观潮** from the in-app template library).
2. Confirm import. Grok Bot copies profile, skills, routines, and shareable memories — **not** the author’s real portfolio or private briefs.
3. On first chat, the bot runs **Getting started with 观潮**. Answer **one question at a time**:
   - Timezone (default `Asia/Shanghai`)
   - Markets to cover (A / HK / US / macro, …)
   - Holdings and watchlist tickers (do not invent position sizes)
   - Brief cadence (full session set vs lighter)
   - Whether to enable decision alerts (add/trim/hold/avoid stances; still not advice)

### 2. Right after import

1. Ask the assistant to seed a `market/` workbench matching this repo (or clone this repo as a reference).
2. Run one sample brief against [docs/BRIEF-CHECKLIST.md](docs/BRIEF-CHECKLIST.md); confirm two-tier delivery (short chat + full file under `briefs/`).
3. Enable or pause routines; put holidays into `market/calendar/`.
4. Keep this repo handy: [junit/guanchao-research-workbench](https://github.com/junit/guanchao-research-workbench).

### 3. Template vs this repository

- **Template only:** live Grok Bot cron routines, first-run onboarding, packaged workflow memories.
- **Repo only:** versioned checklists/SCHEMA, forkable empty `market/`, auditable skill Markdown.
- **Neither should hold:** your real sizes, broker secrets, or paid data credentials — keep those private.

## Layout

| Path | Purpose |
|------|---------|
| [docs/BRIEF-CHECKLIST.md](docs/BRIEF-CHECKLIST.md) | Hard gates for every session brief |
| [docs/SCHEMA.md](docs/SCHEMA.md) | File contracts |
| [docs/THEME-TEMPLATE.md](docs/THEME-TEMPLATE.md) | Sector theme card template |
| [docs/alerts-RULES.md](docs/alerts-RULES.md) | Decision-alert rules |
| [docs/ROUTINES.md](docs/ROUTINES.md) | Suggested routines (`Asia/Shanghai`) |
| [skills/](skills/) | Getting-started + workbench skill recipes |
| [market/](market/) | Empty skeleton (`EXAMPLE.*` placeholders only) |

## Core conventions (short)

1. Start with a **live major-event scan**; geopolitics is discovered dynamically — not a fixed region checklist.
2. Every hotspot needs **market → sector → holdings/watchlist/candidates** transmission.
3. Stock `opportunity-log` is **research-first**; session briefs may only mark watch candidates.
4. Default **two-tier delivery**; never invent live tape for a closed market.

## Topics

`investment-research` · `stock-market` · `a-shares` · `hong-kong-stocks` · `us-stocks` · `grok-bot` · `agent-skills` · `markdown`

## License

[MIT](LICENSE) © 2026 junit

## Disclaimer

Docs and the Guanchao template are for research-process reference only and **are not investment advice**. You own your decisions and outcomes.
