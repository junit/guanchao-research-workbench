# 观潮 · 跨市场投资研究台

[English](README.en.md) · [MIT License](LICENSE)

开源的是**研究流程文档与目录骨架**（A股 / 港股 / 美股 / 宏观），不是实时行情接口，也不是券商交易工具。

**观潮**面向 Grok Bot：按时段做盘前/盘中/收盘简报，对照持仓与自选，沉淀板块主题与机会日志，并在关键节点给出决策倾向——一律标注 **非投资建议**。

## 一键导入 Grok Bot 模板

**公开模板链接：** [https://x.ai/bot/nylU6e_GXKCzJvLLxN6qW](https://x.ai/bot/nylU6e_GXKCzJvLLxN6qW)

在浏览器打开即可导入「观潮」。导入后按入门问答配置时区、市场、持仓与简报节奏（详见下方「接入」）。

## 两种用法

| 方式 | 适合谁 | 你得到什么 |
|------|--------|------------|
| **Grok Bot 公开模板**（推荐开箱即用） | 已有 Grok Bot / Cursor 账号 | 带例行、技能、工作流记忆的「观潮」助手 |
| **本仓库** | 想自建笔记库 / 自写 Agent | `docs/` 清单 + `market/` 空骨架 + 技能正文 |

两者互补：模板负责「跑起来」；仓库负责「流程可审、可 fork」。

## 接入 Grok Bot 公开模板

> **已发布模板：** [https://x.ai/bot/nylU6e_GXKCzJvLLxN6qW](https://x.ai/bot/nylU6e_GXKCzJvLLxN6qW)  
> 也可在 Grok Bot 模板广场搜索「观潮」。

### 1. 导入

1. 打开公开模板链接：[https://x.ai/bot/nylU6e_GXKCzJvLLxN6qW](https://x.ai/bot/nylU6e_GXKCzJvLLxN6qW)（或在 Grok Bot 内从模板库选择「观潮」）。
2. 确认导入。系统会复制机器人的档案名、描述、技能、例行与可公开记忆；**不会**带上原作者的真实持仓与私有简报。
3. 导入后，机器人会走入门技能 **Getting started with 观潮**，请按提示**一次只答一个问题**：
   - 时区（默认 `Asia/Shanghai`）
   - 要覆盖的市场（A / 港 / 美 / 宏观等）
   - 持仓与自选代码（不要编造仓位数量）
   - 简报节奏（全套时段例行，或精简版）
   - 是否开启决策告警（加仓/减仓/持有/回避倾向，仍为非投资建议）

### 2. 导入后建议立刻做的事

1. 让助手按本仓库的 `market/` 结构初始化工作台（或直接 clone 本仓库作参考）。
2. 对照 [docs/BRIEF-CHECKLIST.md](docs/BRIEF-CHECKLIST.md) 跑一次「样例简报」，确认两档交付（聊天短档 + `briefs/` 全文）。
3. 按需启用/暂停例行；时区与休市日历写进 `market/calendar/`。
4. 需要对照文档时，打开本仓库：[junit/guanchao-research-workbench](https://github.com/junit/guanchao-research-workbench)。

### 3. 模板与本仓库的边界

- **模板有、仓库没有：** 已排好的 Grok Bot 例行（cron）、可运行的入门对话、打包进机器人的工作流记忆。
- **仓库有、模板不替代：** 可版本管理的清单与 SCHEMA、可 fork 的空 `market/`、可审的技能 Markdown。
- **两边都没有：** 你的真实仓位、券商密钥、付费行情账号——请自行保管，勿提交到公开仓库。

## 仓库结构

| 路径 | 说明 |
|------|------|
| [docs/BRIEF-CHECKLIST.md](docs/BRIEF-CHECKLIST.md) | 时段简报硬门槛 |
| [docs/SCHEMA.md](docs/SCHEMA.md) | 文件约定 |
| [docs/THEME-TEMPLATE.md](docs/THEME-TEMPLATE.md) | 板块主题卡模板 |
| [docs/alerts-RULES.md](docs/alerts-RULES.md) | 决策告警规则 |
| [docs/ROUTINES.md](docs/ROUTINES.md) | 建议例行（`Asia/Shanghai`） |
| [skills/](skills/) | 入门技能 + 研究台技能正文 |
| [market/](market/) | 空骨架（示例代码已标明 EXAMPLE） |

## 核心约定（摘要）

1. 先做**实时大事件扫描**；地缘是动态发现，不是固定地区打卡表。
2. 每个热点写清 **市场 → 板块 → 持仓/自选/候选** 三层传导。
3. 个股 `opportunity-log`：**research-first**；盘中简报只能标「观察候选」。
4. 默认**两档交付**（聊天短档 + `briefs/` 全文）；面向用户的正文须**说人话**（自然中文），禁止把 Yahoo/新浪抓取代码原样堆进聊天或全文；休市日禁止编造该市场 live 行情。

## 近期更新

- 简报交付强化：**说人话** + 两档（短档/全文）规则已写入 `docs/BRIEF-CHECKLIST.md`、`docs/SCHEMA.md` 与研究台技能。
- 跨资产旁证：A/港与美股每一份时段简报都轻量刷新币圈 24h 行情，不另开独立例行；as-of 超过 6 小时仅作刷新失败时的陈旧兜底；A/港与美股全文均纳入比特币/以太，美股全文另纳入黄金，短档每次保留一条币圈线，达到阈值才升格大事件。

## Topics

`investment-research` · `stock-market` · `a-shares` · `hong-kong-stocks` · `us-stocks` · `grok-bot` · `agent-skills` · `markdown`

## License

[MIT](LICENSE) © 2026 junit

## 免责声明

本仓库与「观潮」模板仅供研究流程参考，**不构成投资建议**。市场有风险，决策与盈亏自行负责。
