---
name: Cross-market research workbench
description: >-
  Use this when building or maintaining a market research workbench, writing a
  session/overnight brief, logging sector themes or stock opportunities, or
  reviewing holdings against live events.
---
# Cross-market research workbench

Reusable workflow for A/HK/US (+ macro) research assistants. Always label outputs 非投资建议.

## Workbench layout (create if missing)

- `market/BRIEF-CHECKLIST.md` — hard gates for every brief
- `market/SCHEMA.md` — file contracts
- `market/portfolio.md` — holdings / watchlist / candidates (codes only; sizes optional in `positions.md`)
- `market/tickers/<code>.md` — thesis, catalysts, falsifiers, risks, status
- `market/sectors/hot-log.md` + `opportunity-log.md` + `themes/<slug>.md`
- `market/opportunity-log.md` — stock opportunities (index only after research-first)
- `market/research/` + `sources/` — deep reports and filings notes
- `market/world-tracks/` + `global-hot-log.md` — dynamic geopolitics timeline
- `market/alerts/RULES.md` + `LOG.md` — decision alert rules and history
- `market/GAP-AUDIT.md` — open gaps

## Brief hard gates

1. Live major-event scan first (macro, geopolitics, policy, holdings flashes). If nothing material, say so explicitly.
2. Every hotspot needs **三层传导**: 市场 → 板块 → 持仓/自选/候选 (name codes). Do not dismiss with a bare 弱相关.
3. Sector layer: that day's hot industries are the foundation, then map to holdings/watchlist/candidates. Maintain hot-log for continuity; theme cards for structured opportunities.
4. Geopolitics: discover what is hot → sediment into timeline → active / cooling / archived. No hardcoded region checklist.
5. Judgments use cumulative timeline evidence (prior conclusion → new evidence → keep/change), not latest-snapshot-only.
6. Delivery: default two-tier (short chat + full file). Full chat only when the user asks for 全文/完整审阅.
   Chat/full brief for humans: plain Chinese section titles and status; never paste internal notation like "∪ active", hot-log, last_review into user-facing text. In prose and tables use Chinese names for futures/rates/ETFs (标普期货、十年期美债、美元指数、中概互联网 ETF…); never dump Yahoo/Sina tickers (hf_ES, ^TNX, DX-Y.NYB, KWEB…) or half-names ("Bessent,") into user-facing text. Source lines are short prose, not code strings. File slugs may stay English. Self-review before send.
7. Holiday gate: if A-shares (or relevant market) is closed, skip that market's session work and say so; still cover open markets.

## Opportunity & research rules

- Session briefs may mark 观察候选 only; do not append to stock `opportunity-log` from a session.
- Opportunity-log requires research-first: coherent thesis (catalyst, transmission, financials check, falsifiers) before入库.
- Pure limit-up / momentum is not enough; need a verifiable catalyst beyond price strength.
- Theme cards need: why, catalysts, falsifiers, risks, status (`warming|watching|fading|archived`), linked names, first_seen/last_review, review log. Hot-log is flow; cards are structure. Status must track reality (e.g. fading when the board cools).

## Decision alerts

When conviction is high or holdings face material risk, push stance (add/trim/hold/avoid) with reasons and falsifiers. Still 非投资建议. Ask the user only for info only they have (e.g. real size) or destructive actions. Log alerts in `alerts/LOG.md`.

## Anomaly scans

Run as incremental diffs vs the last session brief. Stay quiet when nothing new. Skip duplicate push if within ~25 minutes of a full session brief.
