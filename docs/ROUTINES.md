# Session routines (Asia/Shanghai)

These cron intents match the Grok Bot template. Schedules are suggestions; adjust to your timezone.

| Name | When (CST) | Intent |
|------|------------|--------|
| A/港盘前 | Weekdays 08:51 | Overnight/global scan → A/HK open agenda |
| A/港开盘 | Weekdays 09:35 | First prints + early sector transmission |
| A股上午盘中 | Weekdays 10:54 | Mid-morning A pulse |
| A股上午收盘 | Weekdays 11:35 | AM wrap |
| A/港下午开盘 | Weekdays 12:52 | Afternoon reopen |
| A股下午盘中 | Weekdays 14:15 | Late-day check |
| A股收盘 | Weekdays 15:05 | Full day close + writebacks |
| 港股收盘 | Weekdays 16:05 | HK close |
| 美股盘前 | Weekdays 16:08 | US pre-market prep |
| 美股开盘 | Weekdays 21:35 | US open |
| 美股盘中 | Tue–Sat 00:13 | US mid-session |
| 美股收盘 | Tue–Sat 04:05 | US close + dashboards refresh |
| 美股盘后汇总 | Weekdays 07:02 | Overnight Asia wrap |
| 事件与证伪扫描 | Weekdays 07:48 | Calendar + falsifier scan |
| 异动与证伪快检 | Weekdays 10:22 / 13:22 / 15:22 | Incremental alerts only on hits |
| 宏观周历 | Monday 07:48 | Refresh `calendar/macro.md` |
| 研究员周报 | Friday 17:48 | Formal weekly body + GAP |
| 研究员月报 | 1st 17:48 | Formal monthly body + GAP |
| 机会深研队列 | Weekdays 15:43 | Research-first deep-dives for high-opportunity names |

Hard rules for every fire: `BRIEF-CHECKLIST.md`, holiday gate, two-tier delivery, 非投资建议. Light X discovery via `market/sources/x-watchlist.md` (发现候选 only; skip if no credits).

## Hard-node one-shots (importer-created)

Not listed above as fixed crons. When `BRIEF-CHECKLIST` layer-2 gates fire, create a **self-deleting** one-shot for that confirmed window (company checkpoint or regime event). Morning「事件与证伪扫描」reconciles by reading `calendar/`—do not embed near-term name lists in routine prompts. Personal holdings-tied alarms stay out of this public repo.
