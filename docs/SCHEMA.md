# 投资研究工作流 Schema

本目录是投资助手“观潮”的研究工作区。所有面向用户的 Markdown 内容使用简体中文；未核验的数字、价格、事件日期和传闻必须明确标注 `TBD`、`待填` 或 `待确认`，不得写成事实。

## 目录与职责

- `tickers/<code>.md`：每个已跟踪标的一份结构化档案（YAML frontmatter + 正文）。
- `portfolio.md`：持仓、自选和已入库候选的目录索引；不重复维护完整逻辑。
- `opportunity-log.md`：已完成深研并进入候选的机会目录；归档文件不得改写。
- `watch-candidates.md`：尚未通过深研门槛的观察池，不计入机会日志。
- `pending-delete.md`：逻辑失效或证伪后待确认移出的标的。
- `research/`：深度研究报告；`research/INDEX.md` 维护状态与路径。
- `calendar/`：宏观日历、标的事件与证伪触发记录；**`global-hot-log.md` + `world-tracks/`** 为全球热点动态时间线（实时发现、沉淀、增删），不是固定地区清单。
- `journal/decisions.md`：所有升级、降级、移出、入库等决策变更日志。
- `comps/`：可比公司与估值草图；`comps/INDEX.md` 为索引。
- `sources/`：按代码归档一手材料；`sources/INDEX.md` 为索引；**`sources/x-watchlist.md`** 为策展 X 发现层观察表（见下方契约）。
- `reviews/`：周报/月报及模板。
- `portfolio-exposure.md`：主题暴露图。
- `positions.md`：持仓数量与成本（**可选**；用户自愿填写，缺失不阻塞判断）。
- `thesis-intake.md`：**已降级为可选冗余**（顶部有废弃说明）。进度以 `tickers/` + `journal/decisions.md` 为准；本文件不再维护，可保留空表防链断。用户独有信息仍可记入 decisions。
- `GAP-AUDIT.md`：工作台缺口审计。
- `sectors.md`：行业/板块中枢；`sectors/hot-log.md` 热门流水；`sectors/opportunity-log.md` 板块机会索引；`sectors/themes/<slug>.md` 主题结构化档案（why/催化/证伪/风险等）；≠个股机会日志。
- `dashboards/`：跨资产仪表盘（`INDEX.md` 索引）。含 `rates-fx.md`、`commodities.md`、`crypto.md`、`funding.md`、`consensus.md`。一致预期只填可核验公开源，否则 `待填`。币圈 24h 旁证嵌 **A/港与美股各时段** 轻量必刷（as-of>6h 兜底，不另开独立例行）；全文宏观表须含比特币/以太，美股全文另含黄金。详见 `BRIEF-CHECKLIST.md` §3b 与 INDEX。
- `alerts/RULES.md`：异动与证伪快检规则（增量差量；与时段简报重叠则只写 LOG）。
- `alerts/LOG.md`：异动窗推送/未达门槛流水（每窗至少一行）。
- `BRIEF-CHECKLIST.md`：时段/快检必做清单（含两档交付、假日门控、主题 status 硬更新）。


## sources/x-watchlist.md 契约

- **用途：** 时段简报的**轻量 X（Twitter）发现层**——嗅正在热什么；**不是**一手，**不是**升格大事件的充分条件。
- **分层：** A 档通讯社/全球要闻；B 档政策·利率·贸易·能源·科技叙事；C 档可选噪声源（默认不扫）。
- **禁止：** 使用操作员个人 Following；禁止未核验「交易所官号」；禁止仅凭社媒帖触发决策告警或单独升格大事件（须通讯社/政府·公司公告/交易所一手交叉）。
- **运维：** 每时段窗抽扫 A+B 子集 + 2–4 关键词；额度不足则跳过并注明；季度剪枝；新号须 API 核验且反复有用才入库。
- 细则与账号表以 `sources/x-watchlist.md` 正文为准。

## 标的档案字段

每个 `tickers/<code>.md` 必填：

- `code`, `name`, `market`
- `status`：`holding` | `watchlist` | `candidate` | `watch_candidate` | `pending_delete`
- `thesis`：一句话投资逻辑
- `catalysts`：可核对催化列表
- `falsifiers`：证伪条件
- `risks`：主要风险
- `research`：深研路径；`candidate` 必须有已完成或可追溯的深研报告
- `last_review`, `review_note`

## 生命周期与 research-first 门槛

1. 盘中热门板块先记入 `sectors/hot-log.md`；有可核对催化的板块主题可记 `sectors/opportunity-log.md`（板块机会 ≠ 个股机会）。
2. 由板块拆出的个股、或简报新提到的个股，先进入 `watch-candidates.md`，不得直接进入候选或个股 `opportunity-log.md`。
3. 完成 `research/<code>.md`，逻辑/证据/证伪/风险自洽后，才可迁入 `candidate`，并更新 `portfolio.md` 与个股 `opportunity-log.md`。
4. 用户确认持有后改为 `holding`；想跟踪但不持有改为 `watchlist`。
5. 证伪或逻辑不再自洽时先改为 `pending_delete`，写明理由；确认后移出。
6. 任何升级/降级/入库/移出必须在 `journal/decisions.md` 留痕。
7. 深研完成时同步 `comps/<code>.md` 与 `sources/<code>/`。


## 时段简报复核规则

**实时大事件优先（动态）：** 每次时段先做**无地区打卡偏置**的实时检索，再对照 `calendar/global-hot-log.md` 与 `world-tracks/` 做新增/升级/降温/归档；简报大事件段按**当前热度**写，禁止固定复读中东/俄乌/台海等名单，也禁止只用利率/指数替代。无新增定价级事件须写明，并对仍 active 的轨道给一句近况。可并行打开 `sources/x-watchlist.md` 做轻量 X 发现（发现候选 only；禁止个人关注列表；禁止仅凭社媒升格）。

**行业与板块硬约束：** 指数/量能之后必须写行业与板块。顺序：①当日热门板块（基础，与持仓无关也要写）②对照 hot-log 看连续/脉冲 ③映射持仓/自选/候选 ④有催化则建 themes 结构化档案（必填关注原因 why 等）并更新索引。不得从大盘直接跳到个股。板块机会不得直接写入个股 opportunity-log。板块机会与热门记录须**随市场动态新增与剔除**（退潮/证伪可归档）。

**时间线证据硬约束：** 简报必须含「时间线」段：上次结论 → 新增证据 → 是否改判；并对照 `calendar/`、`tickers/`、必要时 `journal/decisions.md`。禁止只凭当日碎片改结论。

**持仓逻辑归属：** 持仓/自选 thesis 由观潮分析维护；仓位可选。细则见 `BRIEF-CHECKLIST.md`。


- 每次时段任务读取 `holding`、`watchlist`、`candidate` 的全部档案，进行逻辑认证，输出继续观察或建议进入待删除。
- 简报至少快速查看 `calendar/events.md` 中未来三日（T-3）的近端事件，并查看 `calendar/falsifier-alerts.md` 的未关闭触发。
- 标的状态改变时引用 `journal/decisions.md`；不能以价格涨跌单独替代逻辑证据。
- 事件日期没有一手材料确认时，必须写“待确认日期”，不得编造精确财报日、股东会日或政策发布日期。
- 宏观事件、交易所休市、公司公告与监管/政策材料优先核对 `sources/`；卖方材料只作辅助。


## 假日门控 / 两档简报 / 主题 status / alerts LOG

- **假日门控：** 当日市场是否休市、通是否暂停，**只查** `calendar/macro.md`（及交易所一手公告）；A 股时段例行跳过指数/板块 live 或改短版「港+全球+持仓映射」；禁止假装该市场在交易。**禁止**在 Schema 里写死某个已过/将过的节假日日期当例子。
- **两档简报：** 默认聊天只推「决策卡（若有）+ 一句话 + 时间线改判 + 持仓要点」；全文写入 `briefs/`。仅用户明确要求全文或例行写明「全文进聊天」时全文推送。
- **聊天/全文说人话：** 推给用户的短档与全文（含章节标题）用自然中文；禁止 `∪ active`、内部路径名、英文 status 进正文。行情与来源用人话中文名，禁止 Yahoo/新浪抓取代码串与残缺英文专名进正文。档案文件可保留英文。不必硬编码词表。
- **主题 status 硬更新：** 每时段写 `sectors/hot-log.md` 后必须同步 `sectors/themes/<slug>.md` 的 status/last_review；行业确认回吐/连贯性削弱 → `warming→fading`；退潮持续 → `archived` 并更新 `sectors/opportunity-log.md`。禁止只写流水不改卡。
- **alerts/LOG：** 有推送或明确「未达门槛」的异动窗，追加 `alerts/LOG.md` 一行；与时段简报 ±25 分钟重叠且已覆盖 → 只写 LOG、不重复推送。**短档去重：** 同一时段窗 ±40 分钟内已交付短档（手动或例行）→ 后到者只更新全文并差分，聊天默认静默，有实质增量才补一句。
- **索引同步：** 改 world-tracks 文件 → 同步 `calendar/world-tracks/INDEX.md`；改研究 → 同步 `research/INDEX.md`。
- **观察池入口：** 新强结构可拆龙头且与持仓无关 → 1–3 只进 `watch-candidates`；禁止直接写个股 opportunity-log。

## 日历、证伪与复盘规则

- `calendar/events.md` 状态仅允许 `upcoming`、`watching`、`triggered`、`done`。
- 证伪证据先进入 `calendar/falsifier-alerts.md`，写明触发条件、证据、动作和状态；未核验线索不得标记为 `triggered`。
- 周报使用 `reviews/weekly-YYYY-Www.md`，月报使用 `reviews/monthly-YYYY-MM.md`；统计应能回溯到决策日志与事件日志。
- 配套例行任务：工作日 07:48 事件与证伪扫描；周一 07:48 宏观周历；周五 17:48 周报；每月 1 日 17:48 月报（上海时区）。本 Schema 定义文件与复核规则。

- **硬节点闹钟三层：** 覆盖公司硬检查点与**市场风向级大事件**（峰会正式声明、美联储决议、关键数据、OPEC/复航截止、重大休战制裁窗等）。①确认日/窗默认写入 `events.md`（公司）或 `macro.md`+`world-tracks`（风向级），靠 T-3 扫描；②同时满足「确认日或可执行时间窗 +（持仓一阶/改判 **或** 跨市场风险偏好·利率·油价主叙事）+ 现有例行空档或须 T-0/T-1 核一手」才挂**一次性**盯盘；时段例行已覆盖同一窗则不叠挂；③宽窗口/无截止 watching/已归档/未确认时钟不挂。细节见 `BRIEF-CHECKLIST.md`「硬节点闹钟」。
- 「事件与证伪扫描」每次须核对近端硬节点是否已挂第二层闹钟；缺失则补挂，过期则删除。
- **预约披露日强制升格：** 宽窗口出现精确预约日 / 结果窗时，同窗改 `events.md`、评估第二层一次性盯盘、更新 falsifier 近端索引。
- **流程文档只写机制与阈值：** 具体日期、近端标的名单、节假日实例只进 `calendar/` / `portfolio.md` / 一次性例行；Checklist/Schema/例行 prompt 禁止粘贴会过期的「例：某日…」「近端参考：某某…」。例行对账时**打开日历文件读取**，不依赖 prompt 内嵌名单。

