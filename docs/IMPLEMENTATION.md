# 观潮 · 实现交接（IMPLEMENTATION）

> **As-of：** 2026-09-25（Asia/Shanghai）  
> **读者：** 要从零重建并运维「观潮」跨市场研究工作台的实现 Agent（Grok Bot / 兼容 Agent）。  
> **配套：** `DESIGN.md`（为什么）· 本文件（如何建）· `BRIEF-CHECKLIST.md` / `SCHEMA.md`（运行时硬规则）。  
> **边界：** 描述研究流程与文件契约；不提供行情接口、不替券商下单。全文结论一律标注**非投资建议**。

公开入口：[GitHub `junit/guanchao-research-workbench`](https://github.com/junit/guanchao-research-workbench) · [Grok Bot 模板](https://x.ai/bot/nylU6e_GXKCzJvLLxN6qW)。

---

## 0. 给实现者的一句话

你要建造的是：**一个 Grok Bot（或兼容 Agent）+ 其下 `market/` Markdown 研究工作台**。Agent 按 Asia/Shanghai 例行按时段对照持仓/自选写简报，把大事件→板块→标的的传导与改判写进可复查档案；配套文件分工为——`DESIGN.md` 讲哲学与演进，**本文件讲从零落地步骤**，`BRIEF-CHECKLIST.md` / `SCHEMA.md` / `alerts/RULES.md` 是每次开火时必须打开的运行时规则。禁止把本交接文档当成「现行规则全文粘贴」；冲突时以清单与 Schema 为准。

---

## 1. 目标与非目标

### 目标

- 按时段（A/港/美 + 宏观）对照持仓与自选，输出**两档交付**的研究简报（聊天短档 + `briefs/` 全文）。
- 维护可复查的标的档案、板块主题、全球热点时间线、证伪与决策日志。
- 在高确信度机会或持仓重大风险时给出**决策倾向告警**（仍为非投资建议）。
- 用假日门控、短档去重、异动差量、硬节点三层闹钟避免噪音与漏检。

### 非目标

- **不是**券商、下单通道或持仓备份盘。
- **不是**实时行情 API 产品；数据来源为操作员当下使用的公开检索 + 雅虎/新浪/东财等公开页（抓取代码只进 `dashboards/INDEX`，禁止进用户可见简报）。
- **不是**把盘中热度直接写成个股机会日志。
- **不是**独立 24h 加密刷屏 cron，也不是固定「中东/俄乌/台海…」地区打卡表。

---

## 2. 平台与前置

- **运行时：** Grok Bot / Cursor Agent，需支持：例行（cron，时区 Asia/Shanghai）、Skills、持久记忆、Web 检索。
- **可选起点：**
  1. Clone <https://github.com/junit/guanchao-research-workbench>；或
  2. 导入模板 <https://x.ai/bot/nylU6e_GXKCzJvLLxN6qW>。
- **默认时区：** `Asia/Shanghai`（CST / UTC+8）。用户若主要看美股可改，但例行表须整体平移并同步改 `alerts/RULES.md` 异动钟。
- **鉴权：** 研究流**不需要**券商 API Key。付费行情账号若有，由用户自管，禁止写入公开仓。
- **数据：** 公开网页检索与公开行情页；未核验数字标 `待填`/`TBD`/`待确认`，禁止编造精确财报日或休市 live。

---

## 3. 从零落地顺序（强制逐步）

实现 Agent **必须按序**执行，不可跳步宣称完成。

1. **创建 Agent 人设**  
   名称默认「观潮」（导入方可自定）；描述：跨市场（A/港/美/宏观）投资研究助手；每次输出标注非投资建议；不是券商。

2. **安装/复制两门技能**  
   - `cross-market-research-workbench`（研究台硬门槛摘要）  
   - `getting-started-with` / `getting-started-with-guanchao`（入门问答）  
   正文可从开源仓 `skills/` 或本机 `agent-data/workflows/` 复制；技能是摘要，**每次开火仍须打开** `BRIEF-CHECKLIST` / `SCHEMA`。

3. **按 SCHEMA 播种 `market/` 树**  
   见第 4 节清单：建空目录与占位文件。私有工作台填真实档案；公开仓只留 EXAMPLE 骨架。

4. **复制流程文档（可同步层）**  
   从开源 `docs/` 或权威私有副本复制到工作台根（或 `market/` 旁/内约定路径）：  
   `BRIEF-CHECKLIST.md`、`SCHEMA.md`、`alerts/RULES.md`、`THEME-TEMPLATE.md`（或 `docs/THEME-TEMPLATE.md`）、`reviews/weekly-TEMPLATE.md`、`reviews/monthly-TEMPLATE.md`。  
   可选：`DESIGN.md`、`GAP-AUDIT.md`（快照，非现行规则）。

5. **入门问答（getting-started，每轮一问）**  
   顺序固定：时区 → 要覆盖的市场 → 持仓/自选代码 → 简报节奏（全套或精简）→ 决策告警开/关。  
   **禁止**一次抛多个问题；**禁止**替用户编造仓位数量。

6. **写 `portfolio.md` + 空 ticker 档案**  
   仅用用户给出的代码建 `tickers/<code>.md` stub（必填 frontmatter 字段见 SCHEMA）；`positions.md` 保持可选空白。无 invented sizes。

7. **播种日历与仪表**  
   - `calendar/macro.md`：写入「休市/通安排」机制说明位（具体假日日后维护，不写进 Checklist）。  
   - 空表：`events.md`、`falsifier-alerts.md`、`global-hot-log.md`、`world-tracks/INDEX.md`。  
   - `dashboards/`：`INDEX.md` + `rates-fx.md` / `commodities.md` / `crypto.md` / `funding.md` / `consensus.md` 空壳（as-of 待填）。

8. **创建例行全集（第 6 节）或用户选定的精简子集**  
   Cron 一律 Asia/Shanghai。Prompt **只写机制**（打开 calendar/ 读日期），禁止内嵌会过期的「例：某日休市」「近端：某某股东会」。

9. **写入约定记忆（第 12 节）**  
   只存恒常机制常量：两档交付、说人话、三层传导、假日门控、币圈嵌时段、硬节点三层、反陈旧、短档 ±40m 去重、异动钟单一真相源等。

10. **冒烟验收（第 13 节）**  
    跑一次 live 大事件扫描 + 短样例简报；核对两档交付、说人话、文末非投资建议、币圈轻刷回写、假日门控逻辑可读。

---

## 4. 目录树（创建清单）

在 Agent 工作区创建如下树（路径相对工作台根；私有 Agent 常见为 `market/`）。标注：

- **P** = 流程/可同步（开源可公开）  
- **F** = 私人填充（真实持仓、简报全文、深研正文；公开仓仅 EXAMPLE/空壳）

```
market/
├── BRIEF-CHECKLIST.md          # P 运行时硬清单（权威）
├── SCHEMA.md                   # P 文件契约
├── DESIGN.md                   # P 设计叙事（可选但推荐）
├── GAP-AUDIT.md                # P/F 缺口快照（机制可公开；近端名单勿贴）
├── portfolio.md                # F 持仓/自选/候选索引
├── positions.md                # F 可选仓位
├── portfolio-exposure.md       # F 可选主题暴露
├── watch-candidates.md         # F 观察池
├── opportunity-log.md          # F 个股机会索引（research-first）
├── pending-delete.md           # F 待移出
├── sectors.md                  # F 行业中枢
├── thesis-intake.md            # 可选冗余（可空表+废弃说明）
├── tickers/                    # F <code>.md 档案
├── briefs/                     # F 时段全文 YYYY-MM-DD-<slug>.md
├── sectors/
│   ├── hot-log.md              # F 热门流水
│   ├── opportunity-log.md      # F 板块机会索引
│   └── themes/<slug>.md        # F 主题卡（用 THEME-TEMPLATE）
├── calendar/
│   ├── macro.md                # F 宏观/休市（假日门控唯一日历源）
│   ├── events.md               # F 公司硬节点
│   ├── falsifier-alerts.md     # F 证伪触发
│   ├── global-hot-log.md       # F 全球热点流水
│   └── world-tracks/
│       ├── INDEX.md            # F 轨道索引（改轨必同步）
│       └── <track>.md          # F 单轨道
├── dashboards/
│   ├── INDEX.md                # P/F 索引；抓取代码仅此可见
│   ├── rates-fx.md
│   ├── commodities.md
│   ├── crypto.md               # 各时段轻刷
│   ├── funding.md
│   └── consensus.md
├── alerts/
│   ├── RULES.md                # P 异动/决策门槛；异动钟单一真相源
│   └── LOG.md                  # F 每异动窗至少一行
├── research/
│   ├── INDEX.md                # F 深研索引
│   └── <code>.md               # F 深研正文
├── sources/
│   ├── INDEX.md
│   ├── x-watchlist.md          # P 策展 X 发现层（非个人关注；非一手）
│   └── <code>/                 # F 一手摘录
├── comps/
│   ├── INDEX.md
│   └── <code>.md               # F 可比草图
├── reviews/
│   ├── weekly-TEMPLATE.md      # P
│   ├── monthly-TEMPLATE.md     # P
│   ├── weekly-YYYY-Www.md      # F
│   └── monthly-YYYY-MM.md      # F
└── journal/
    └── decisions.md            # F 升级/降级/决策倾向留痕
```

开源仓还可在仓库根保留：

```
docs/BRIEF-CHECKLIST.md, SCHEMA.md, DESIGN.md, ROUTINES.md,
     THEME-TEMPLATE.md, alerts-RULES.md, IMPLEMENTATION.md
market/sources/x-watchlist.md   # P 策展发现层（无私人关注列表）
skills/cross-market-research-workbench.md
skills/getting-started-with-guanchao.md
market/   # 空骨架 + EXAMPLE
```

创建时：`mkdir -p` 上述目录；对空目录放 `.gitkeep`；表格文件写表头即可，禁止预填虚构行情。

---

## 5. 规范文档优先级

规则冲突时，**严格按下列优先级**（高覆盖低）：

1. **`BRIEF-CHECKLIST.md`**（时段/快检现行硬门槛）  
2. **`SCHEMA.md`**（文件契约与生命周期）  
3. **`alerts/RULES.md`**（异动差量、决策告警、LOG、异动钟）  
4. **Skill 摘要**（`cross-market-research-workbench`）  
5. **`DESIGN.md` 叙事**（哲学与演进；不替代硬规则）  
6. **`GAP-AUDIT.md`**（带时点快照；开放项指引，非现行规则）

实现要求：

- 每次例行/手动简报**必须打开** Checklist + Schema（及本窗相关 calendar/alerts），禁止凭记忆复述近端日期。  
- **禁止**在 Checklist / Schema / 例行 prompt / 本交接的「机制段」粘贴会过期的节假日实例或近端标的名单；时效只进 `calendar/`、`portfolio.md`、自删一次性例行。  
- 异动时刻：**唯一真相源 = 已启用的「异动与证伪快检」cron**；改 cron 须同窗改 `alerts/RULES.md`，禁止第二套钟表。

完整条文见 sibling 文件，本交接只给算法与落地步骤。

---

## 6. 例行完整规格

时区一律 **Asia/Shanghai**。下列 Cron 与公开 `docs/ROUTINES.md` 对齐；用户可精简，但精简后须在记忆中写明「已暂停哪些」。

**每条例行共用硬前缀（写入每个 prompt 开头）：**

> 打开并遵守 `market/BRIEF-CHECKLIST.md` 与 `market/SCHEMA.md`。假日门控只查 `calendar/macro.md`（必要时对照交易所公告）。币圈：本窗轻量刷新 `dashboards/crypto.md`（as-of>6h 必须重刷）；A 休市仍刷加密旁证。大事件扫描可并行打开 `sources/x-watchlist.md` 做轻量 X 发现（发现候选 only；禁止个人关注列表；禁止仅凭社媒升格；连接/额度不足则注明跳过、不阻塞）。两档交付：默认聊天短档（决策卡若有 + 一句话结论 + 时间线改判 + 持仓要点），全文写入 `briefs/YYYY-MM-DD-<slug>.md`；仅用户要「全文」或本 prompt 写明「全文进聊天」才贴全文。说人话：中文品名，禁止 Yahoo/新浪抓取代码串与内部记号进聊天。近端日期与硬节点只读 `calendar/`，禁止臆造。文末非投资建议。

---

### 6.1 A/港盘前

- **Cron：** 工作日 `08:51`  
- **目的：** 隔夜/全球扫描 → A/港开盘议程。  
- **Prompt intent（可粘贴）：**

```
【A/港盘前】执行共用硬前缀。本窗 slug 建议 a-hk-premarket。
1) 假日门控：读 calendar/macro.md；若 A 休市则跳过 A live 指数/板块或改短版「港+全球+持仓映射」，禁止假装 A 在交易。
2) 实时大事件扫描（无地区打卡偏置）→ 打开 sources/x-watchlist.md 轻量 X 发现（A+B 子集 + 2–4 关键词；发现候选 only；禁个人关注/禁仅社媒升格）→ 对照 global-hot-log 与 world-tracks 增删降温。
3) 指数与量能（开市市场）；刷新 crypto；宏观旁证。
4) 行业板块（热门基础→hot-log→映射持仓）并同步 themes status。
5) 持仓/自选/候选逻辑复查；T-3 扫 events + falsifier。
6) 回写日历/仪表/hot-log；有决策告警则固定格式推送并记 decisions。
7) 短档去重：若 ±40 分钟内已交付同窗短档，只更新全文并差分，聊天默认静默。
无实质内容且未达告警门槛时：仍写全文骨架与回写，短档可极简；禁止空喊「无变化」刷屏。
```

### 6.2 A/港开盘

- **Cron：** 工作日 `09:35`  
- **目的：** 开盘价与早盘板块传导。  
- **Prompt intent：**

```
【A/港开盘】共用硬前缀。slug: a-hk-open。
假日门控后：抓早盘指数/量能/最强最弱板块；三层传导；crypto 轻刷；写 briefs 全文 + 短档；同步 hot-log 与 themes；回写 global-hot-log/world-tracks。开市才写 live，休市不编造。
```

### 6.3 A股上午盘中

- **Cron：** 工作日 `10:54`  
- **目的：** 上午盘中脉冲。  
- **Prompt intent：**

```
【A股上午盘中】共用硬前缀。slug: a-morning-mid。
相对盘前/开盘做增量：新热点、板块轮动、持仓映射变化；crypto 轻刷；两档交付；主题 status 硬更新。
```

### 6.4 A股上午收盘

- **Cron：** 工作日 `11:35`  
- **目的：** 上午收盘小结。  
- **Prompt intent：**

```
【A股上午收盘】共用硬前缀。slug: a-morning-close。
上午量能与板块连贯性对照 hot-log；时间线改判；crypto；全文+短档；回写。
```

### 6.5 A/港下午开盘

- **Cron：** 工作日 `12:52`  
- **目的：** 午后开盘再定位。  
- **Prompt intent：**

```
【A/港下午开盘】共用硬前缀。slug: a-hk-afternoon-open。
午间新闻与外盘变化；下午议程；假日门控；crypto；板块→持仓；两档交付。
```

### 6.6 A股下午盘中

- **Cron：** 工作日 `14:15`  
- **目的：** 尾盘前检查。  
- **Prompt intent：**

```
【A股下午盘中】共用硬前缀。slug: a-afternoon-mid。
尾盘前增量差量；证伪近端；crypto；主题 status；两档交付。
```

### 6.7 A股收盘

- **Cron：** 工作日 `15:05`  
- **目的：** 全日收盘闭环与回写。  
- **Prompt intent：**

```
【A股收盘】共用硬前缀。slug: a-close。
全日指数/量能/板块总结；hot-log 一行 + themes 同步；持仓影响与候选复查；crypto；日历与 decisions 回写；全文必须完整。短档去重规则适用。
```

### 6.8 港股收盘

- **Cron：** 工作日 `16:05`  
- **目的：** 港股收盘与南向/中概映射。  
- **Prompt intent：**

```
【港股收盘】共用硬前缀。slug: hk-close。
港股指数与热门板块；映射持仓/自选中的港股与相关中概逻辑；crypto；两档交付；回写。
```

### 6.9 美股盘前

- **Cron：** 工作日 `16:08`  
- **目的：** 美股盘前准备（常接港收）。  
- **Prompt intent：**

```
【美股盘前】共用硬前缀。slug: us-premarket。
美股期货/利率/美元/油；黄金旁证（全文必有黄金行）；crypto 轻刷；隔夜亚洲与美盘议程；持仓美股映射；两档交付。
```

### 6.10 美股开盘

- **Cron：** 工作日 `21:35`  
- **目的：** 美股开盘传导。  
- **Prompt intent：**

```
【美股开盘】共用硬前缀。slug: us-open。
开盘指数与板块；大事件三层传导；黄金+crypto；持仓点名；全文+短档；themes/hot-log 若涉美主题则同步。
```

### 6.11 美股盘中

- **Cron：** 周二至周六 `00:13`  
- **目的：** 美股盘中脉冲。  
- **Prompt intent：**

```
【美股盘中】共用硬前缀。slug: us-mid。
相对开盘增量；利率/油汇/黄金/crypto；持仓与证伪；两档交付；短档 ±40m 去重。
```

### 6.12 美股收盘

- **Cron：** 周二至周六 `04:05`  
- **目的：** 美股收盘 + 仪表刷新。  
- **Prompt intent：**

```
【美股收盘】共用硬前缀。slug: us-close。
收盘总结；刷新 dashboards（rates-fx/commodities/crypto 等能取则取）；黄金+BTC/ETH 进全文宏观表；回写日历与 hot-log；两档交付。
```

### 6.13 美股盘后汇总

- **Cron：** 工作日 `07:02`  
- **目的：** 隔夜亚洲开盘前汇总。  
- **Prompt intent：**

```
【美股盘后汇总】共用硬前缀。slug: us-afterhours。
美股盘后要点→今日 A/港议程；crypto；读 macro 休市安排；T-3 提醒；短档指向「今日关注」；全文落盘。
```

### 6.14 事件与证伪扫描

- **Cron：** 工作日 `07:48`  
- **目的：** 近端日历 + 证伪 + **硬节点第二层对账**。  
- **Prompt intent：**

```
【事件与证伪扫描】共用硬前缀。slug: event-falsifier-scan。
打开 calendar/events.md、macro.md、falsifier-alerts.md、world-tracks/INDEX。
1) 扫描未来约 T-3 近端事件与未关闭证伪；无确认日期写「待确认」，禁止编造精确日。
2) 硬节点三层对账（见 BRIEF-CHECKLIST）：
   - 缺第二层闹钟且门槛满足 → 创建一次性例行（命名 硬节点·<简称><月日><事件>，跑完/过期自删）；
   - 已被后续 A/港/美时段例行覆盖同一窗 → 只记日历，不叠挂；
   - 已过期一次性例行 → 删除。
3) 宽窗口若出现预约披露精确日：同窗改 events、评估第二层、更新 falsifier 近端索引。
4) 默认聊天：仅当有新触发/新闹钟/需用户确认的待删时短推；否则可静默但全文/回写仍要做。
```

### 6.15 异动与证伪快检

- **Cron：** 工作日 `10:22` / `13:22` / `15:22`（**改点必须同窗改 alerts/RULES.md**）  
- **目的：** 相对上一简报的增量差量；命中才推。  
- **Prompt intent：**

```
【异动与证伪快检】打开 alerts/RULES.md 与 BRIEF-CHECKLIST。做增量差量，不要重推简报已写脉冲。
触发：持仓/自选/候选约 ±5% 及以上（相对上窗新增或显著加深）、证伪触碰、主题突发政策价催化、或决策告警门槛（机会级/风险级）。
与时段简报窗口重叠（±25 分钟）且简报已覆盖 → 本窗只追加 alerts/LOG.md 一行（动作=简报已覆盖-仅LOG），不向用户重复推送。
无命中：不发「无变化」，但仍须 LOG 一行「未达门槛」。
命中决策告警：固定格式推送 + journal/decisions.md。触及风险偏好差量时刷 crypto。
```

### 6.16 宏观周历

- **Cron：** 周一 `07:48`  
- **目的：** 刷新 `calendar/macro.md` 当周宏观/休市/关键数据窗。  
- **Prompt intent：**

```
【宏观周历】打开 SCHEMA 与既有 macro.md。用公开源刷新本周利率/CPI/央行/已发现地缘窗与交易所休市安排；未确认标待确认。回写 macro.md；必要时升/建 world-tracks。禁止把具体周历粘进 Checklist。短档可汇报「本周宏观已更新」要点。
```

### 6.17 研究员周报

- **Cron：** 周五 `17:48`  
- **目的：** 正式周报 + GAP 对照。  
- **Prompt intent：**

```
【研究员周报】按 reviews/weekly-TEMPLATE.md 写 reviews/weekly-YYYY-Www.md。
回溯 journal/decisions、events、falsifier、hot-log、主题 status 变迁、持仓逻辑变化。更新 GAP-AUDIT 机制向开放项（不贴会过期的近端名单）。两档：短档给人结论，全文进 reviews。非投资建议。
```

### 6.18 研究员月报

- **Cron：** 每月 1 日 `17:48`  
- **目的：** 月度复盘。  
- **Prompt intent：**

```
【研究员月报】按 reviews/monthly-TEMPLATE.md 写 reviews/monthly-YYYY-MM.md。
汇总主题生命周期、机会入库/待删、证伪命中、宏观主叙事变迁；对照 GAP。短档+全文；非投资建议。
```

### 6.19 机会深研队列

- **Cron：** 工作日 `15:43`  
- **目的：** research-first 深潜，观察候选 → 自洽报告后才入库。  
- **Prompt intent：**

```
【机会深研队列】打开 watch-candidates.md 与 research/INDEX.md。
挑选高优先级观察候选：写/补 research/<code>.md（逻辑、催化、传导、财务核对、证伪、风险自洽）。
仅当自洽后：迁 candidate、更新 portfolio.md 与个股 opportunity-log.md、同步 comps/sources、记 decisions。
禁止因当日涨停/热度直接写 opportunity-log。无合适标的：静默或短记「本队列无达标项」，勿硬凑。
```

---

### 6.20 硬节点一次性例行（程序，非固定名单）

**禁止**在公开仓或 prompt 里维护「近端闹钟名单」。按 Checklist 第二层门槛，由「事件与证伪扫描」或发现确认日的时段任务创建。

**创建步骤：**

1. 确认：日期或可执行时间窗已确认（非「Q4 待定」）。  
2. 影响面二选一：（a）对持仓/自选/候选一阶影响或会改决策态度；**或**（b）风向级——显著改变跨市场风险偏好/利率主锚/油价航运叙事。  
3. 落点落在现有盘中例行空档，或必须在 T-0/T-1 单独核一手；若下一档 A/港/美时段已覆盖同一窗 → **不叠挂**。  
4. 创建**一次性**例行：  
   - 命名：`硬节点·<简称><月日><事件>`（例：`硬节点·EXAMPLE0930财报`——此处 EXAMPLE 仅为格式示意）。  
   - Prompt：到点核一手公告/声明/数据 → 回写 `events.md` 或 `macro.md`、`falsifier-alerts` / `world-tracks` → 达告警门槛才推决策态度；未达保持安静 → **跑完或过期自删**。  
5. 同一节点禁止重复挂；过期未删的由晨扫删除。

**第三层不要挂：** 宽窗口财报季、无截止 watching、已归档主题、普通新闻、未确认时钟的「可能有声明」。

---

## 7. 单次时段简报算法

按 `BRIEF-CHECKLIST` 顺序 1→10 执行；下列为实现伪码。全文路径：`briefs/YYYY-MM-DD-<slug>.md`（slug 见第 6 节；手动补跑可加后缀如 `-v2`）。

```
READ BRIEF-CHECKLIST, SCHEMA, portfolio, 相关 tickers,
     calendar/macro+events+falsifier+global-hot-log+world-tracks/INDEX,
     sources/x-watchlist, sectors/hot-log+themes, dashboards/crypto(+美股则 commodities/rates),
     最近同窗 brief（供去重/差量）

IF market_closed(today, market) per macro.md:
    SKIP live index/sectors for that market OR write short「开市市场+全球+持仓映射」
    NEVER invent tape for closed market
    # 假日门控不跳过 crypto

1. LIVE major-event scan (no region checklist)
   open sources/x-watchlist.md → light X scan (Tier A+B subset + 2–4 keywords)
   hits = 发现候选 only; NEVER personal Following; NEVER promote on social alone
   (X down / low credits → note skip in methods; do not block brief)
   discover → diff vs hot-log/tracks → add/upgrade/cooling/archive
   each item: 是什么 / 最新状态(来源时点) / 相对上次 / 三层传导

2. 时间线段: 上次结论 → 新证据 → 是否改判

3. 指数与量能（开市市场）

3b. 刷新 crypto（及门槛内黄金）; as-of>6h 必须重刷
    全文宏观表: BTC/ETH 必有; 美股全文另加黄金
    短档: 默认一行币; 升格大事件仅当 |24h|≥5% 或 vs上窗≥3% 或硬新闻或明显同向/背离

4. 行业板块: 热门基础(与持仓无关也写) → hot-log 连贯性
   → 映射持仓/自选/候选 → 可选 themes 卡
   WRITE hot-log 一行后 MUST sync themes status/last_review
   新强结构可拆龙头 → 1–3 只进 watch-candidates（禁止直接 opportunity-log）

5–7. 持仓影响 / 自选 / 候选复查（禁只因涨跌改结论）

8. 近端回写: global-hot-log, world-tracks(+INDEX), calendar 其余,
            journal/decisions, sectors/hot-log, 必要时 sectors/opportunity-log,
            dashboards as-of

9. 决策告警若命中: 固定格式推送 + decisions（见 RULES）

10. 个股 opportunity-log: 默认不加

DELIVERY:
  full = write briefs/YYYY-MM-DD-<slug>.md
  IF ±40m 内已交付同窗短档:
      update full + diff; chat silent UNLESS material delta
  ELSE:
      chat = 短档（决策卡? + 一句话 + 时间线改判 + 持仓要点）
  说人话自检后发送
  文末非投资建议
```

---

## 8. 异动 / 决策告警算法

权威细节见 `alerts/RULES.md`。实现摘要：

**异动窗：**

1. 相对上一时段简报（或上一异动窗）做**增量差量**。  
2. 若与时段简报 ±**25** 分钟重叠且简报已覆盖同一结论 → **只写 LOG，不推送**。  
3. 触发才通知：约 ±5% 新增/加深、证伪触碰、主题突发催化、或决策告警门槛；地缘若构成持仓重大风险即使未达 ±5% 也按决策告警评估。  
4. 无命中：不发「无变化」，但 **LOG 必有一行**「未达门槛」。

**LOG 表头：**

```
| 时刻(CST) | 窗 | 动作 | 标的/主题 | 摘要 | 推送? |
```

动作枚举：`推送` | `维持观察` | `未达门槛` | `简报已覆盖-仅LOG`。

**决策告警输出格式（推送时必含）：**

- 告警级别：机会 / 风险 / 兼有  
- 标的或主题  
- 决策倾向：加仓观察 / 维持 / 减仓或规避 / 等待硬检查点  
- 依据（1–3 条）+ 对照档案先前结论  
- 证伪/失效条件  
- 仓位说明（`positions.md` 空则写「仓位未知，仅逻辑倾向」）  
- 文末：**非投资建议**

回写：`alerts/LOG.md`、必要时 `falsifier-alerts.md`、`tickers/`、`journal/decisions.md`、themes。

---

## 9. 硬节点三层算法

```
ON discover(confirmed_date_or_executable_window) of
   (company_checkpoint OR regime_event):

  # 第一层（默认）
  WRITE calendar/events.md (company) OR macro.md + world-tracks (regime)
  IF date unconfirmed: write「待确认」; NEVER invent exact day
  Rely on morning scan + session T-3 countdowns
  # 不另开例行

  # 预约披露日强制升格
  IF vague_window gains appointment_date:
     harden events.md; evaluate layer-2; refresh falsifier near-term index
     SAME turn

  # 第二层（一次性）——须同时满足
  IF confirmed_window
     AND (holdings_first_order_or_stance_change OR regime_narrative)
     AND (gap_vs_session_crons OR need_dedicated_T0_T1):
        IF next_session_cron covers same window:
           DO NOT stack; calendar + brief countdown only
        ELSE:
           CREATE one-shot routine name=硬节点·<简称><月日><事件>
           ON fire: verify primary source → writebacks
                    → push stance only if alert threshold
                    → self-delete after run or past deadline
           NEVER duplicate same node

  # 第三层（禁止挂）
  IF vague_Q_window OR open_ended_watching OR archived
     OR ordinary_news_covered_by_next_session OR unconfirmed_clock:
        DO NOT schedule

MORNING「事件与证伪扫描」:
  reconcile calendar vs enabled one-shots
  missing layer-2 → create; covered → calendar only; expired → delete
```

临近升格：T-3 进简报倒计时；T-1/T-0 才考虑第二层。风向级与公司硬节点同一套门槛。

---

## 10. 机会与主题生命周期

**个股：**

1. 盘中/简报 → 最多记入 `watch-candidates.md`（粗线索）。  
2. `机会深研队列` 或手动深研 → `research/<code>.md` 自洽。  
3. 迁 `candidate`，更新 `portfolio.md` + 个股 `opportunity-log.md`；同步 comps/sources；`journal/decisions.md` 留痕。  
4. 用户确认持有 → `holding`；只跟踪 → `watchlist`。  
5. 证伪/逻辑失效 → `pending_delete` → 确认后移出。  

**禁止：** 时段简报直接追加个股 `opportunity-log`；纯涨停/动量不足以上库。

**板块主题：**

- `sectors/hot-log.md` = 流水连贯性（每时段至少一行摘要）。  
- `sectors/themes/<slug>.md` = 结构（why/催化/证伪/风险/status）。  
- status：`warming` | `watching` | `fading` | `archived`（模板亦可含 `active`/`invalidated`，以 Checklist 硬门槛为准做 warming→fading→archived）。  
- 写完 hot-log **必须**同步主题卡 status/last_review；退潮持续 → archived 并更新 `sectors/opportunity-log.md`。  
- 板块机会 ≠ 个股机会；有催化建主题卡，拆龙头进观察池而非个股机会日志。

**全球热点：** `global-hot-log` + `world-tracks` 动态增删（active/cooling/archived）；改轨道文件必须同步 `INDEX.md`。

---

## 11. 用户可见文案规范

- **说人话：** 主题、状态、章节标题用自然中文。禁止 `∪ active`、`hot-log`、`last_review`、裸英文 status/slug、路径堆砌出现在给人看的正文。  
- **中文品名：** 标普期货、纳指期货、美油、布伦特、十年期美债收益率、美元指数、恐慌指数、比特币、以太坊、黄金等。  
- **禁止**把 `ES=F`、`^TNX`、`DX-Y.NYB`、`KWEB`、`hf_ES` 等抓取代码堆进聊天或全文；代码仅可出现在 `dashboards/INDEX` 操作员区。  
- 来源写成「新浪期货约 xx:xx；雅虎约 xx:xx；路透/财新」短句，禁止一整行代码串。  
- A/港数字代码可保留，首次宜带中文简称。  
- 人名写可读全称，禁止残缺片段。  
- 交付前通读：删电报腔、并集/箭头内部符号、不可读代码堆。  
- **文末固定：非投资建议。** 数字标明来源与时点。

---

## 12. 约定记忆建议列表

实现完成后，向 Agent 持久记忆写入下列**机制向**常量（勿写入具体持仓代码或近端日期）：

- 默认时区 Asia/Shanghai；休市只查 `calendar/macro.md`。  
- 两档交付：默认短档 + `briefs/` 全文；全文进聊天仅用户明确要求或例行写明。  
- 说人话 + 中文品名；抓取代码不出用户正文。  
- 大事件三层传导：市场 → 板块 → 持仓/自选/候选点名。  
- 时间线累积改判；禁止单日碎片拍板。  
- 地缘动态时间线，无固定地区打卡表。  
- 假日门控：休市不编造该市场 live；其它开市照写。  
- 币圈嵌所有 A/港/美时段轻刷；不另开 24h cron；as-of>6h 必重刷；假日门控不跳过币圈。  
- 短档币升格门槛：|24h|≥5% 或 vs 上窗≥3% 或硬新闻或明显风险偏好同向/背离。  
- 主题：写 hot-log 后必须同步 themes status。  
- 个股机会 research-first；时段只进 watch-candidates。  
- 异动：增量差量；±25m 与简报重叠则只 LOG；无命中也写 LOG 一行。  
- 短档 ±40m 去重：后到者更新全文并差分，聊天默认静默。  
- 硬节点三层 + 晨扫对账；命名 `硬节点·…`；跑完自删；不叠挂时段已覆盖窗。  
- 异动钟单一真相源 = 启用「异动与证伪快检」cron；改点同步 RULES。  
- 流程文档反陈旧：Checklist/Schema/prompt 不贴过期日期实例；打开 calendar 读。  
- 决策告警有门槛；日常波动静默；一律非投资建议。  
- 规范优先级：BRIEF-CHECKLIST > SCHEMA > alerts/RULES > skill > DESIGN。

---

## 13. 验收清单（实现完成定义）

实现 Agent 在宣称「已落地」前须全部勾选：

- [ ] Agent 人设已建；两门技能已安装且可触发。  
- [ ] `market/` 目录树按第 4 节存在；流程文档（Checklist/Schema/RULES/模板）就位。  
- [ ] 入门五问已完成（或用户明确跳过并记录默认值）；`portfolio.md` 与 ticker stubs 仅含用户代码。  
- [ ] 第 6 节例行已创建（全集或用户确认的精简子集）；cron 时区 Asia/Shanghai。  
- [ ] 约定记忆已写入（第 12 节机制常量）。  
- [ ] **假日门控：** 在 macro.md 标为休市的日期，对应市场例行跳过 live 或不编造涨跌（可用历史休市日回放或模拟）。  
- [ ] **两档交付：** 样例简报聊天为短档，`briefs/` 存在全文文件。  
- [ ] **说人话：** 样例中无抓取代码串、无 `∪ active` 等内部记号。  
- [ ] **币圈嵌套：** 样例回写 `dashboards/crypto.md` as-of；A 休市逻辑下仍要求刷加密。  
- [ ] **异动 LOG：** 模拟一次未达门槛窗，`alerts/LOG.md` 有「未达门槛」行。  
- [ ] **硬节点对账：** 「事件与证伪扫描」prompt 含打开 calendar 与补挂/删过期逻辑；无固定近端名单写死在 prompt。  
- [ ] **反陈旧：** Checklist/Schema/例行 prompt 中无「例：某日休市」类过期实例。  
- [ ] **机会门槛：** 文档/记忆明确禁止时段直接写个股 opportunity-log。  
- [ ] 样例输出文末含 **非投资建议**。  
- [ ] 公开同步若执行：仅流程文档；无真实持仓/私有简报/个人闹钟名单。

---

## 14. 公开 vs 私有 / 与 DESIGN 关系

| 层 | 公开（开源仓 / 模板） | 私有工作台 |
|----|----------------------|------------|
| 流程 | Checklist、Schema、RULES、ROUTINES、THEME-TEMPLATE、IMPLEMENTATION、skills、空 market 骨架 | 同左 + 本地修订 |
| 数据 | EXAMPLE 占位 | 真实 portfolio、tickers、briefs、research、sources、个人硬节点一次性例行 |
| 叙事 | `DESIGN.md` 哲学与演进 | 可保留私有 GAP 细节 |

- **`DESIGN.md`：** 为什么这样设计、演进史、刻意不做的事。  
- **本文件 `IMPLEMENTATION.md`：** 另一个 Agent 如何从零重建与验收。  
- **Checklist / Schema / RULES：** 每次开火的现行硬规则（优先于本文件叙述）。  

Standing preference：与开源仓同步时**只推流程文档**，不同步私人持仓与简报。

---

## 15. 常见失败模式

实现与运维时主动避免：

1. **叠挂硬节点：** 时段例行已覆盖同一窗仍挂一次性闹钟 → 刷屏。应日历倒计时即可。  
2. **编造财报/股东会精确日：** 未一手确认却写成事实。应「待确认」。  
3. **聊天倾倒 Yahoo/新浪代码：** 把 `^TNX`、`ES=F` 当行情表。代码只留 INDEX。  
4. **时段热度直写 opportunity-log：** 跳过深研。只进 watch-candidates。  
5. **第二套异动钟：** RULES 写一套、cron 另一套。改点必须同窗双改。  
6. **A 休市跳过币圈：** 假日门控不适用加密旁证。  
7. **±40m 内连发两遍几乎相同短档：** 后到者应静默只更全文。  
8. **只写 hot-log 不改 themes status：** 主题卡与现实脱节。  
9. **把 Checklist 当近端名单墙：** 粘贴「本周股东会：…」导致文档自腐。  
10. **无命中异动窗不写 LOG：** 失去审计轨迹。  
11. **地缘固定地区打卡：** 每次必勾中东/俄乌等。应动态发现。  
12. **默认把全文塞进聊天：** 违反两档交付。  
13. **公开仓提交真实仓位或个人闹钟名单。**  
14. **用涨跌单独替代逻辑证据改判 holding/candidate。**

---

## 附录 A · 数据源说明（无虚构 API）

- 操作员通过 **Web 检索** 与 **雅虎 / 新浪 / 东财等公开页面** 取数（与当前观潮操作方式一致）。  
- 具体抓取符号与接口名**仅**记录在 `dashboards/INDEX.md` 供执行，**不得**出现在用户可见短档/全文。  
- 接口空值或无法核验 → 写 `待填`，不伪造一致预期或北向净额。  
- 本交接**不**提供、不捏造私有 REST SDK 或券商接口。

## 附录 B · 简报文件命名

| 场景 | 建议 slug 示例 |
|------|----------------|
| A/港盘前 | `a-hk-premarket` |
| A/港开盘 | `a-hk-open` |
| A 上午盘中/收盘 | `a-morning-mid` / `a-morning-close` |
| A/港下午开盘 | `a-hk-afternoon-open` |
| A 下午盘中/收盘 | `a-afternoon-mid` / `a-close` |
| 港收 | `hk-close` |
| 美股盘前/开/中/收/盘后 | `us-premarket` / `us-open` / `us-mid` / `us-close` / `us-afterhours` |
| 事件证伪扫描 | `event-falsifier-scan` |
| 手动补跑 | 同 slug 或加 `-v2` |

完整文件名：`briefs/YYYY-MM-DD-<slug>.md`。

---

非投资建议；本文描述工作流实现与运维交接，不构成任何买卖建议。数字与事件须标明来源与时点。
