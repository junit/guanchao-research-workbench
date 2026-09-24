# 观潮 · Cross-market research workbench

开源的是**研究台文档与骨架**（A股 / 港股 / 美股 / 宏观），不是实时行情接口，也不是券商下单工具。

**观潮** is a Grok Bot–oriented investment research assistant workflow: session briefs, sector themes, holdings mapping, and decision alerts — always labeled **非投资建议 / not investment advice**.

## Two ways to use this

1. **Grok Bot public template** — import the bot inside Grok Bot (routines + skills + memories). That path is product-native and separate from this repo.
2. **This repository** — copy the `market/` skeleton and `docs/` checklists into your own agent or notes vault.

## What’s inside

| Path | Purpose |
|------|---------|
| `docs/BRIEF-CHECKLIST.md` | Hard gates for every session brief |
| `docs/SCHEMA.md` | File contracts |
| `docs/THEME-TEMPLATE.md` | Sector theme card template |
| `docs/alerts-RULES.md` | Decision-alert rules |
| `docs/ROUTINES.md` | Suggested weekday cron intents (Asia/Shanghai) |
| `skills/` | Getting-started + workbench skill recipes |
| `market/` | Empty/example workbench skeleton |

Personal holdings, live briefs, and deep-research notes were **not** published.

## Core conventions

- Live major-event scan first; geopolitics is **discovered**, not a fixed region checklist.
- Every hotspot: **市场 → 板块 → 持仓/自选/候选** (three-layer transmission).
- Opportunity-log is **research-first**; session briefs only mark 观察候选.
- Default delivery is **two-tier** (short chat + full file); holiday gate skips inventing closed-market tape.

## License

MIT — see `LICENSE`.

## Disclaimer

本仓库内容仅供研究流程参考，**不构成投资建议**。市场有风险，决策自行负责。
