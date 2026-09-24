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
8. Cross-asset side checks (gold + crypto): crypto trades 24h — do **not** add a separate always-on cron. **Light-refresh `dashboards/crypto.md` on every A/HK and US session brief** (preopen/open/mid/close; US afterhours/evening too). If as-of is >6h stale, must refresh before writing. **Full** briefs (A/HK and US): BTC/ETH row required; US full also needs gold. **Short** briefs every session: default one crypto macro line; promote to 大事件 only if |24h|≥5%, vs prior brief jump ≥3%, hard regulatory/ETF/exchange news, or clear risk-on/off co-move with equity/futures that needs explanation. A-share holiday gate does **not** skip crypto.

## Opportunity & research rules

- Session briefs may mark 观察候选 only; do not append to stock `opportunity-log` from a session.
- Opportunity-log requires research-first: coherent thesis (catalyst, transmission, financials check, falsifiers) before入库.
- Pure limit-up / momentum is not enough; need a verifiable catalyst beyond price strength.
- Theme cards need: why, catalysts, falsifiers, risks, status (`warming|watching|fading|archived`), linked names, first_seen/last_review, review log. Hot-log is flow; cards are structure. Status must track reality (e.g. fading when the board cools).

## Decision alerts

When conviction is high or holdings face material risk, push stance (add/trim/hold/avoid) with reasons and falsifiers. Still 非投资建议. Ask the user only for info only they have (e.g. real size) or destructive actions. Log alerts in `alerts/LOG.md`.

## Anomaly scans

Run as incremental diffs vs the last session brief. Stay quiet when nothing new. Skip duplicate push if within ~25 minutes of a full session brief.

## Hard-node watches (calendar + one-shot routines)

Covers **company hard checkpoints** and **market-regime / risk-appetite events** (summit formal statements, FOMC + presser, key CPI/NFP, OPEC or Hormuz reopen deadlines, major ceasefire/sanctions windows). See live `BRIEF-CHECKLIST.md`.

Three layers:
1. **Default:** confirmed date or executable window → `calendar/events.md` (company) or `macro.md` + `world-tracks/` (regime). Cover via T-3 in morning scan + session briefs. No extra routine.
2. **One-shot routine** when all of: confirmed date/window; **either** first-order holdings/watch/candidate impact or stance change **or** regime-shifting risk appetite / rates / oil narrative; **and** a gap vs existing session crons (or dedicated T-0/T-1 needed). If the next A/HK/US session routine already covers the same window, do **not** stack—calendar + brief countdown only. Fire → verify primary source → write calendar/tracks/falsifiers → push stance only if alert threshold hit; stay quiet otherwise; **self-delete after run or past deadline**. Never duplicate the same node.
3. **Do not schedule:** vague Q4 windows, open-ended watching, archived themes, unconfirmed “maybe a statement”, ordinary news the next session brief will cover.

Morning「事件与证伪扫描」must reconcile: missing layer-2 watches → create; covered by session cron → calendar only; expired → delete.

## Short-brief dedupe + earnings-date upgrade

- Same session window ±40 minutes: if a short brief was already delivered (manual catch-up or routine), the later run updates the full `briefs/` file and diffs silently; chat stays quiet unless material delta (new primary statement, falsifier hit, decision alert, regime narrative change).
- When a vague reporting window gets a **confirmed appointment date**, same turn: harden `events.md`, evaluate layer-2 one-shot, refresh falsifier near-term index.
