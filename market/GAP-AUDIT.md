# Gap audit (mechanism snapshot)

As-of: 2026-09-24 23:16 CST

> Dated snapshot, not the live rulebook. Mechanisms live in `docs/BRIEF-CHECKLIST.md`, `docs/SCHEMA.md`, `docs/alerts-RULES.md`. Near-term dates and alarms live only in `market/calendar/` (importer fills). No personal holdings lists here.

## Landed (mechanism)
- [x] Two-tier briefs; holiday gate (read calendar, do not hardcode holiday dates in process docs)
- [x] Theme status hard updates; alerts LOG; incremental anomaly vs session brief
- [x] Hard-node three-layer watches; short-brief ±40m dedupe; appointment-date forced upgrade
- [x] Process docs forbid stale date/name examples
- [x] Anomaly routine clocks: **single source of truth = enabled cron** (currently weekdays 10:22 / 13:22 / 15:22 Asia/Shanghai); `docs/alerts-RULES.md` must match
- [x] Dashboard fetch paths (Yahoo/Sina/API codes) are **operator-only** — never paste into user-facing brief prose

## Still open (mechanism)
1. Next equity session: live sector hot-log + theme status hard update
2. Hard-node reconcile: read `calendar/` + one-shot routines only; do not duplicate date lists here
3. Importer: seed portfolio / fill calendar / first weekly+monthly+macro calendar runs

## Deliberately not in this file
- Concrete holiday / EGM / NFP clocks → `market/calendar/`
- Portfolio codes → `market/portfolio.md` (personal; not in public template body)
- A second copy of cron times → enabled routines only

Updated: 2026-09-24 23:16 CST
