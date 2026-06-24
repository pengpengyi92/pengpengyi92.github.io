---
title: "HKUDS002: Vibe-Trading 作为 Agentic Quant Research Workflow"
date: 2026-06-24 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds002, hkuds, vibe-trading, quant-research, trading-agent, backtesting, research-autopilot, ai-infrastructure]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第三篇。

```text
HKUDS002 -> Vibe-Trading
```

上一篇 `HKUDS001` 讲的是 `LightRAG`。
我的定位是：

```text
LightRAG = Research Memory Layer
```

这一篇讲 `Vibe-Trading`。
我的定位是：

```text
Vibe-Trading = Agentic Quant Research Workflow Layer
```

如果说 LightRAG 负责“记住、检索、引用、组织知识”，那么 Vibe-Trading 负责“把金融问题变成可运行的分析、回测、报告、团队讨论和研究证据”。

这对我们非常关键。
因为我们要做的 `Pengyi Quant Research OS v0` 不是静态知识库，而是：

```text
idea
  -> hypothesis
  -> data
  -> signal
  -> backtest
  -> diagnosis
  -> report
  -> next plan
  -> memory
```

Vibe-Trading 正好覆盖中间最难的一段：

```text
natural-language finance question
  -> agent planning
  -> tool calls
  -> data loading
  -> strategy code
  -> backtest
  -> validation
  -> artifacts
  -> reports
```

所以这篇的核心结论是：

```text
Vibe-Trading 是 HKUDS 生态里最接近我们 Quant R&D Agent 目标的项目。
```

## Local snapshot

我读的是本地 HKUDS 工作区里的 `Vibe-Trading` 快照。

| Item | Value |
|---|---|
| repo | `Vibe-Trading` |
| branch | `main` |
| status | clean |
| local head | `c5b9e13` |
| latest local commit | `docs(news): add 2026-06-23 entry across all 5 READMEs (#298)` |
| package | `vibe-trading-ai` |
| version | `0.1.10` |
| license | MIT |
| Python | `>=3.11` |

本地规模：

| Metric | Count |
|---|---:|
| total files | 1547 |
| Markdown files | 374 |
| Python files | 992 |
| TypeScript files | 75 |
| JSON files | 7 |

关键目录：

```text
Vibe-Trading/
  agent/
    api_server.py
    mcp_server.py
    src/
      agent/
      tools/
      factors/
      swarm/
      memory/
      goal/
      hypotheses/
      trading/
      live/
      scheduled_research/
      skills/
    backtest/
      engines/
      loaders/
      optimizers/
    cli/
  frontend/
  wiki/
  assets/
```

这不是单点工具。
它是一个完整的 AI trading research workspace。

## What it is

README 给 Vibe-Trading 的定义非常直接：

```text
open-source research workspace for turning finance questions into runnable analysis
```

更具体地说，它连接了：

```text
natural-language prompts
market-data loaders
strategy generation
backtest engines
reports
exports
persistent research memory
multi-agent teams
MCP tools
broker connectors
```

所以它的本质不是：

```text
一个 trading bot
```

更准确是：

```text
一个 agentic finance research operating system
```

它可以做的事情包括：

| Task | Output |
|---|---|
| ask a trading question | market research, data, docs, reusable session context |
| backtest a strategy idea | strategy code, metrics, benchmark, validation artifacts, run card |
| review own trades | broker journal parsing, behavior diagnostics, shadow strategy |
| improve repeated research | persistent memory, editable skills, reusable workflows |
| run analyst teams | investment / quant / crypto / macro / risk swarms |
| ship artifacts | reports, Pine Script, TDX, MT5, MCP tools |
| bench alpha zoo | IC / IR / alive-reversed-dead categorization |

这个项目的价值在于：

```text
它把 quant research 的“想法到证据”做成了 agent workflow。
```

## Why it matters

我们之前一直在讲：

```text
R&D Agent for Quant Research
  = 自动提出因子假设
  + 自动实现
  + 自动回测
  + 自动诊断偏差
  + 自动生成下一轮研究计划
  + 人类 PM 审核
```

Vibe-Trading 已经具备其中很多拼图：

| Our R&D Agent target | Vibe-Trading component |
|---|---|
| 自动提出/保存假设 | `HypothesisRegistry`, `create_hypothesis`, `search_hypotheses` |
| 研究目标管理 | `GoalStore`, `start_research_goal`, evidence ledger |
| 自动生成 config | `generate_backtest_config` |
| 自动实现策略骨架 | `scaffold_signal_engine` |
| 自动回测 | `backtest`, `backtest.runner`, multiple engines |
| 诊断和归因 | run cards, metrics, benchmark, attribution, reports |
| 多角色评审 | 29 swarm presets |
| 人类 PM 审核 | goal criteria, evidence, audit rows, live mandate |
| 研究记忆 | persistent memory, session trace, run artifacts |

这说明 Vibe-Trading 不是我们要“从零复刻”的东西。
它更像一个可学习、可借鉴、可接入、可贡献的开源训练场。

## System architecture

我会把它拆成八层：

```text
Layer 1: user surfaces
Layer 2: session and agent runtime
Layer 3: tool registry and skills
Layer 4: data and document grounding
Layer 5: backtest and alpha research
Layer 6: swarm research teams
Layer 7: memory / goal / hypothesis / evidence
Layer 8: safety / live trading boundary
```

完整路径可以这样理解：

```text
CLI / Web UI / REST API / MCP client
  -> session runtime
  -> AgentLoop
  -> ToolRegistry
  -> finance skills + local tools + optional external MCP tools
  -> data loaders / document readers / web readers
  -> signal generation / backtest / alpha bench / shadow account
  -> artifacts / run cards / reports
  -> persistent memory / goal evidence / hypothesis registry
```

对我们来说，这个架构最重要的不是“有很多功能”，而是它把研究过程变成了可保存、可复查、可继续的 workflow。

## Entry surfaces

Vibe-Trading 有多个入口。

| Surface | Purpose |
|---|---|
| `vibe-trading` | interactive CLI / TUI |
| `vibe-trading run -p "..."` | script-friendly natural-language research |
| `vibe-trading serve` | FastAPI backend + Web UI |
| `vibe-trading-mcp` | expose Vibe-Trading tools to MCP clients |
| `frontend/` | React Web UI |
| `wiki/` | public docs / alpha library / research lab |

这说明它不是只给一个使用场景设计。
它同时服务：

```text
terminal user
browser user
REST API caller
MCP-compatible agent
developer
research contributor
```

这对开源项目很重要。
一个真正有生命力的 AI infra project 往往不是只有 notebook，而是有：

```text
CLI
API
UI
MCP
docs
tests
deployment path
```

Vibe-Trading 这点做得很完整。

## Core modules

本地代码结构里，最关键的模块如下。

| Module | Role |
|---|---|
| `agent/api_server.py` | FastAPI server，sessions/runs/upload/swarm/alpha/live/scheduled research endpoints |
| `agent/mcp_server.py` | FastMCP server，把研究工具暴露给 Claude Desktop、OpenClaw、Cursor 等 |
| `agent/src/agent/loop.py` | ReAct agent core loop |
| `agent/src/agent/tools.py` | `BaseTool` + `ToolRegistry` |
| `agent/src/tools/` | agent 可调用的本地工具 |
| `agent/backtest/runner.py` | 回测入口，读取 `config.json` 和 `signal_engine.py` |
| `agent/backtest/loaders/registry.py` | 数据源注册和 fallback chains |
| `agent/backtest/engines/` | 多市场回测引擎 |
| `agent/src/factors/` | Alpha Zoo registry、bench、compare |
| `agent/src/swarm/` | 多智能体团队 preset 和 DAG runtime |
| `agent/src/memory/` | file-based persistent memory |
| `agent/src/goal/` | research goal / evidence ledger |
| `agent/src/hypotheses/` | durable hypothesis registry |
| `agent/src/trading/` | connector-first trading read interface |
| `agent/src/live/` | mandate、order guard、halt、audit |
| `frontend/src/pages/` | Agent、AlphaZoo、Compare、Correlation、Reports、RunDetail、Runtime、Settings |

从模块名就能看出来：

```text
agent
tools
backtest
factors
swarm
memory
goal
hypotheses
live
```

这些刚好就是一个量化研究 agent 系统需要的骨架。

## Agent loop

`agent/src/agent/loop.py` 是核心。

它实现的是 ReAct tool-calling agent：

```text
user message
  -> context builder
  -> LLM stream_chat
  -> tool calls
  -> execute tools
  -> append tool results
  -> iterate
  -> final answer
```

但它比简单 ReAct 多很多工程细节：

| Mechanism | Why it matters |
|---|---|
| streaming text / reasoning events | Web UI 和 CLI 可以实时显示进度 |
| cancellation checkpoint | 用户可以中断长任务 |
| background task notifications | 长任务结果可以回流到下一轮 |
| context microcompact | 自动剪掉旧 tool result，避免上下文爆炸 |
| context collapse | 长文本折叠，降低 API cost |
| auto compact tool | 让模型主动请求压缩 |
| read/write batching | 只读工具并行，写工具串行 |
| tool heartbeat / progress | 长工具不会像卡死 |
| trace writer | 每轮 tool call 和结果可审计 |
| llm usage tracking | token usage 可复盘 |

最值得学习的是 read/write batching：

```text
readonly tools -> parallel
write tools    -> serial
```

这很工程化。
比如查数据、读文件、看新闻可以并行；写文件、执行回测、修改状态必须串行。

这和我们之后写 R&D Agent 时的工具调度原则一致：

```text
parallelize safe reads
serialize state-changing writes
```

## Tool registry

工具系统由两个类支撑：

```python
class BaseTool(ABC)
class ToolRegistry
```

`BaseTool` 规定每个工具必须有：

```text
name
description
parameters
repeatable
is_readonly
execute()
```

`ToolRegistry` 负责：

```text
register tool
get tool
export OpenAI function schema
execute tool
guarantee JSON error envelope
```

工具自动发现逻辑在 `agent/src/tools/__init__.py`：

```text
import all modules in src.tools
collect BaseTool subclasses
skip unavailable tools
inject memory/session/swarm dependencies where needed
append external MCP tools when configured
```

这给我们的启发是：

```text
工具系统必须同时是 LLM-readable 和 engineer-readable。
```

LLM 需要 schema。
工程师需要隔离、副作用标记、测试、注册和错误边界。

## Finance skills

Vibe-Trading 本地有 79 个 finance skills。

它们不是 Python tool，而是 methodology docs / templates / domain knowledge。

本地分类覆盖：

```text
data-routing
strategy-generate
technical-basic
multi-factor
factor-research
macro-analysis
valuation-model
credit-analysis
options-strategy
crypto desk
flow analysis
report-generate
backtest-diagnose
shadow-account
trade-journal
```

这和 LLMQuant 的 `skills` 很像。
区别是 Vibe-Trading 的 skills 更偏交易研究工作流，LLMQuant 的 skills 更偏 agent/finance tooling ecosystem。

对我们来说，skills 是非常重要的抽象：

```text
tool = executable capability
skill = reusable methodology
memory = persisted context
goal = current research contract
hypothesis = durable research thesis
```

这四个东西要分开。
很多 agent 项目失败，就是因为把所有东西都塞进 prompt。

## Data layer

Vibe-Trading 的 data layer 是核心能力之一。

它支持 18 个市场数据源：

```text
tushare
okx
yfinance
akshare
baostock
tencent
mootdx
ccxt
futu
eastmoney
sina
stooq
yahoo
finnhub
alphavantage
tiingo
fmp
local
```

`backtest/loaders/registry.py` 里有 `VALID_SOURCES` 和 `FALLBACK_CHAINS`。

fallback 不是随便排的，而是按市场和风险分链：

| Market | Fallback chain |
|---|---|
| A-share | `tencent -> mootdx -> eastmoney -> baostock -> akshare -> tushare -> local` |
| US equity | `yahoo -> stooq -> sina -> eastmoney -> yfinance -> tiingo -> fmp -> finnhub -> alphavantage -> akshare -> local` |
| HK equity | `eastmoney -> yahoo -> futu -> yfinance -> akshare -> local` |
| crypto | `okx -> ccxt -> yfinance -> local` |
| futures / fund / macro / forex | `tushare/akshare/local` variants |

这个设计对我们入门 quant 很现实。
我们之前一直说：

```text
quant 最大的问题之一是数据源。
```

Vibe-Trading 给了一个实用答案：

```text
不要先追求完美数据源。
先做 loader abstraction + fallback + local data bridge.
```

也就是：

```text
public/free sources for demo
optional premium sources for quality
local loader for private / institutional / cleaned data
```

这里尤其值得注意的是 `local`：

```text
local loader reads your own CSV / Parquet / DuckDB files
explicit local request never silently falls back to network
```

这对我们未来接入真实数据很重要。
如果我们有自己的 factor data、WorldQuant 脱敏数据、或者私有行情数据，就应该走 local loader，而不是混在 public data fallback 里。

## Backtest layer

回测入口是：

```text
agent/backtest/runner.py
```

它读取：

```text
config.json
code/signal_engine.py
```

然后：

```text
validate config
select loader by source
import signal_engine
run matching engine
collect metrics and artifacts
```

Vibe-Trading 的回测引擎目录包括：

| Engine file | Meaning |
|---|---|
| `china_a.py` | A 股日频回测 |
| `global_equity.py` | 全球股票 |
| `crypto.py` | 加密货币 |
| `china_futures.py` | 中国期货 |
| `global_futures.py` | 全球期货 |
| `forex.py` | 外汇 |
| `options_portfolio.py` | 期权组合 |
| `composite.py` | 跨市场组合回测 |
| `futures_base.py` | 期货公共基类 |
| `base.py` | 基础引擎 |

这说明它的回测不是单一资产类别。
它设计上就是 cross-market。

回测工具 `BacktestTool` 做了几个基本安全动作：

```text
validate run_dir
check config.json exists
check source in VALID_SOURCES
check code/signal_engine.py exists
run subprocess runner
return stdout/stderr/artifacts/run_dir
```

`runner.py` 还对 `signal_engine.py` 做 AST 层面的安全检查，限制 import-time executable statements。
这很重要，因为 agent 生成代码后再执行，一定要有边界。

对我们自己的 R&D Agent 来说，最小可复用接口可以借鉴：

```text
run_dir/
  config.json
  code/
    signal_engine.py
  artifacts/
  run_card.json
  run_card.md
```

这样每一次研究都能落地成可复查的 run。

## Research Autopilot

这是我认为 Vibe-Trading 最接近我们目标的部分。

`agent/src/tools/autopilot_tool.py` 的文件说明是：

```text
Research Autopilot: goal-hypothesis bridge + backtest config generation.

Phase 1: Connects the Hypothesis Registry to the Research Goal runtime.
Phase 2: Auto-generates backtest config.json from hypothesis metadata.
Phase 3: Scaffolds a contract-correct signal_engine.py stub and links
         backtest run-card metrics back to the hypothesis.
```

这个链路非常重要：

```text
hypothesis
  -> research goal
  -> backtest config
  -> signal engine stub
  -> backtest
  -> run card
  -> link evidence back to hypothesis
```

对应工具：

| Tool | Role |
|---|---|
| `run_research_autopilot` | 从已保存 hypothesis 创建 research goal |
| `generate_backtest_config` | 根据 hypothesis 生成 `config.json` |
| `scaffold_signal_engine` | 写 contract-correct `signal_engine.py` stub |
| `link_autopilot_backtest` | 把 run-card metrics 链回 hypothesis |

这已经是一个 R&D loop。

我们自己的版本可以在它的基础上抽象：

```text
Factor Hypothesis
  -> data contract
  -> factor implementation
  -> backtest
  -> diagnostics
  -> PM review
  -> next hypothesis
```

所以 `Research Autopilot` 是 HKUDS002 这篇最该记住的关键词。

## Hypothesis registry

Vibe-Trading 有 durable research hypothesis registry。

核心数据模型：

```text
hypothesis_id
title
thesis
status
universe
signal_definition
data_sources
skills
run_cards
invalidation_notes
created_at / updated_at
```

状态包括：

```text
exploring
testing
validated
rejected
monitoring
```

这和我们做研究很贴近。
一个 idea 不能只存在聊天记录里。
它应该变成 hypothesis object。

我们未来可以定义：

```text
PengyiFactorHypothesis:
  factor_name
  economic_intuition
  expected_direction
  universe
  rebalance_frequency
  data_fields
  neutralization
  risk_controls
  evaluation_metrics
  invalidation_criteria
```

Vibe-Trading 已经给了一个轻量版本。

## Goal and evidence ledger

`agent/src/goal/models.py` 里有完整的研究目标模型：

```text
GoalRecord
GoalClaim
GoalCriterion
EvidenceInput
EvidenceRecord
AuditRow
```

它还区分 risk tier：

```text
research_general
market_specific_short_term
personalized_advice_or_position_sizing
live_trading_or_execution
```

这点非常好。
因为 finance agent 不能只追求自动化。
必须有：

```text
claim
criterion
evidence
audit
risk tier
```

对我们来说，这就是“人类 PM 审核”的技术形态。

我们可以把 PM review 做成：

```text
每个 hypothesis 必须有 criteria
每条 conclusion 必须 link evidence
每个 backtest 必须有 artifact hash
每次 promotion 必须有 audit row
```

这会让我们的 R&D Agent 从玩具变成研究系统。

## Alpha Zoo

Vibe-Trading 里有一个很大的 Alpha Zoo。

README 当前写的是：

```text
456 pre-built quant alphas
```

来源：

| Zoo | Count | Source |
|---|---:|---|
| `qlib158` | 154 | Microsoft Qlib Alpha158 |
| `alpha101` | 101 | Kakushadze 101 Formulaic Alphas |
| `gtja191` | 191 | 国泰君安 191 短周期因子 |
| `academic` | 10 | FF5、Carhart、Jegadeesh、52-week high、Amihud、skew 等 |

代码层面，`factors/registry.py` 做了很多严谨设计：

| Design | Meaning |
|---|---|
| `AlphaMeta` pydantic schema | 每个 alpha 元数据严格校验 |
| `__alpha_meta__` dict literal | 源文件里保存元数据 |
| AST scan metadata | 不 import 代码也能读 metadata |
| module path validation | 限制 zoo 和 alpha id 格式 |
| lazy import on compute | 只有计算时才加载 alpha |
| `health()` / manifest | 可诊断、可导出 |

这对因子库工程非常有启发。

因子不是只写公式。
因子应该有：

```text
id
theme
formula
required columns
universe
frequency
decay horizon
warmup bars
notes
license/source
lookahead test
```

我们之后整理 WorldQuant / 自己的因子，也应该按这个思路做 metadata。

## Swarm teams

Vibe-Trading 本地有 29 个 swarm presets。

例如：

```text
investment_committee
global_equities_desk
crypto_trading_desk
earnings_research_desk
macro_rates_fx_desk
quant_strategy_desk
risk_committee
factor_research_committee
ml_quant_lab
statistical_arbitrage_desk
technical_analysis_panel
```

`swarm/runtime.py` 里实现的是 DAG orchestration：

```text
preset YAML
  -> build run
  -> validate DAG
  -> compute topological layers
  -> run tasks in parallel within layer
  -> serialize dependencies across layers
  -> persist events and run state
```

这比“开几个 agent 一起聊天”强很多。
它的关键是：

```text
agent team = DAG workflow, not random group chat
```

这对我们设计 PM review 很有启发。

未来可以有：

```text
factor_researcher
data_engineer
backtest_engineer
risk_reviewer
PM
```

它们不是同时乱说，而是按 DAG 工作：

```text
researcher proposes hypothesis
data engineer checks data availability
backtest engineer implements and runs
risk reviewer diagnoses bias
PM approves next action
```

这就是我们想要的 R&D team agent。

## MCP design

`agent/mcp_server.py` 用 FastMCP 暴露 54 个工具。

README 列出的工具包括：

```text
list_skills
load_skill
start_research_goal
get_research_goal
add_goal_evidence
update_research_goal_status
backtest
factor_analysis
analyze_options
pattern_recognition
read_url
read_document
web_search
write_file
read_file
trading_connections
trading_select_connection
trading_check
trading_account
trading_positions
trading_orders
trading_quote
trading_history
list_swarm_presets
run_swarm
get_market_data
...
```

MCP server 文件里明确写了一个安全边界：

```text
Every exposed tool is read-only or research-only;
no order-placing or order-cancelling tool is ever surfaced via MCP.
```

这非常关键。
金融 agent 的 MCP 工具必须首先默认研究用途。
否则很容易从“研究助手”滑到“交易执行风险”。

我们自己的公开 demo 也应该遵守：

```text
public tool = research only
private live layer = explicit consent + mandate + halt + audit
```

## Live trading boundary

Vibe-Trading 不是完全不能接 broker。
它有 `agent/src/trading/` 和 `agent/src/live/`。

支持 connector-first trading operations，例如：

```text
check_connection
get_account
get_positions
get_open_orders
get_quote
get_history
place_order
```

但 live order 不是裸奔。
`live/order_guard.py` 里定义了 pre-trade enforcement gate。

执行顺序是 fail-closed：

```text
load mandate
check expiry
check halt flag
extract order intent
read positions and balance
check mandate
ALLOW / DENY / PAUSE_FOR_REAUTH
write audit event
```

这给我们一个很好的边界设计：

```text
研究系统可以开放。
交易执行必须收紧。
```

我们第一阶段绝不需要 live trading。
但要学习它的 trust layer：

```text
mandate
consent
kill switch
audit
position/balance check
notional cap
fail closed
```

这些思想在任何高风险 AI 系统里都通用。

## Web UI

前端是 React 19 + Vite。

页面包括：

| Page | Role |
|---|---|
| `Agent.tsx` | agent chat / research interaction |
| `AlphaZoo.tsx` | alpha library |
| `Compare.tsx` | alpha compare |
| `Correlation.tsx` | correlation heatmap |
| `Reports.tsx` | run report library |
| `RunDetail.tsx` | run details and artifacts |
| `Runtime.tsx` | runtime status |
| `Settings.tsx` | LLM/data source settings |

这说明 Vibe-Trading 的 artifacts 是产品化呈现的：

```text
not just stdout
not just notebook
but run library + detail pages + reports + charts
```

我们的网站以后也可以学习这个思路。
不是只发 blog，而是逐步形成：

```text
project map
run reports
factor library
experiment ledger
research statements
public demo dashboard
```

## Shadow Account

Shadow Account 是一个很有意思的产品功能。

流程是：

```text
broker export / trade journal
  -> parse trades
  -> profile behavior
  -> detect biases
  -> extract implicit rules
  -> backtest shadow strategy
  -> compare actual vs rule-based path
  -> report
```

它适合个人交易者。
但对我们也有启发：

```text
从真实行为数据中反推策略规则。
```

这和银行/真实行业场景也能连接。
真实数据不一定是标准化干净数据。
它可能是：

```text
broker export
trade journal
customer behavior
manual decision trace
portfolio adjustment record
credit approval notes
```

AI 系统要能把这些转成：

```text
behavior profile
rule extraction
counterfactual backtest
diagnostic report
```

这就是 AI 解放生产力的实际形式。

## Vibe-Trading x LightRAG

现在我们已经看了 `LightRAG` 和 `Vibe-Trading`。
二者可以这样组合：

```text
LightRAG      -> remember and retrieve source-grounded research context
Vibe-Trading  -> run finance research workflows and generate artifacts
```

更具体：

| Need | LightRAG | Vibe-Trading |
|---|---|---|
| 保存论文、笔记、报告 | yes | partial |
| source-grounded retrieval | yes | no / limited |
| entity-relation graph | yes | no |
| market data loader | no | yes |
| backtest | no | yes |
| alpha zoo | no | yes |
| swarm research team | no | yes |
| run artifacts | partial | yes |
| research memory | knowledge memory | workflow memory |

组合后的系统：

```text
papers / notes / docs
  -> LightRAG
  -> retrieve evidence and prior work
  -> Vibe-Trading creates hypothesis and backtest
  -> Vibe-Trading outputs run card / report
  -> LightRAG indexes report back into memory
```

这就是闭环：

```text
knowledge -> action -> artifact -> memory
```

## Vibe-Trading x LLMQuant

LLMQuant 和 Vibe-Trading 也很互补。

| LLMQuant component | Vibe-Trading counterpart |
|---|---|
| `data-mcp` | Vibe data loaders / market data tools |
| `quant-mind` | Vibe hypothesis / skills / alpha metadata |
| `Magents` | Vibe backtest engines / swarm teams |
| `awesome-trading-agents` | Vibe as one concrete trading agent workspace |
| finance knowledge layer | Vibe skills and research workflows |

我现在会这样定位：

```text
LLMQuant = finance-agent research ecosystem and knowledge stack
Vibe-Trading = executable finance research agent workspace
LightRAG = source-grounded memory substrate
```

这三个结合就是：

```text
Pengyi Quant Research OS v0
```

## First demo plan

不要一上来做 live trading。
第一阶段应该做 research-only demo。

建议 demo：

```text
pengyi-vibe-research-demo
```

目标：

```text
用 Vibe-Trading 跑一个从 hypothesis 到 backtest report 的最小闭环。
```

步骤：

| Step | Action |
|---|---|
| 1 | 本地安装 `vibe-trading-ai` 或直接用 clone |
| 2 | 选择 free data source，先用 US/HK/crypto 或 A 股免费 fallback |
| 3 | 创建一个简单 hypothesis，例如 momentum / reversal |
| 4 | 用 `generate_backtest_config` 生成 config |
| 5 | 用 `scaffold_signal_engine` 生成 signal engine stub |
| 6 | 手动检查 signal code，避免黑箱执行 |
| 7 | 运行 `backtest` |
| 8 | 读取 run card / report |
| 9 | 用 LightRAG index 这次 run 的结果 |
| 10 | 写一篇公开 demo blog |

第一批问题可以是：

```text
1. 一个简单 moving average strategy 在 BTC 2024 年表现如何？
2. GTJA191 里哪些 reversal 因子在 CSI300 上 IC 更稳定？
3. 同一个 hypothesis 用 naive backtest 和 benchmark comparison 有什么差异？
4. strategy result 的主要 drawdown 来自什么市场阶段？
5. 下一轮应该改 signal、universe、frequency 还是 risk control？
```

这就非常接近我们想做的 Quant R&D Agent。

## What to learn first

Vibe-Trading 太大，不能一口吃完。
我建议先按这个顺序学：

| Priority | Module | Reason |
|---|---|---|
| 1 | `tools/autopilot_tool.py` | 最接近 R&D Agent |
| 2 | `hypotheses/registry.py` | 学 hypothesis object |
| 3 | `goal/models.py` + goal tools | 学 PM review / evidence ledger |
| 4 | `backtest/runner.py` | 学 config + signal_engine contract |
| 5 | `backtest/loaders/registry.py` | 学数据源 abstraction |
| 6 | `factors/registry.py` | 学因子 metadata / alpha zoo |
| 7 | `swarm/runtime.py` | 学 multi-agent DAG |
| 8 | `agent/loop.py` | 学 tool-calling runtime |
| 9 | `mcp_server.py` | 学如何把研究能力暴露给外部 agent |
| 10 | `live/order_guard.py` | 学高风险执行边界 |

这个顺序比直接读全部代码更现实。

## Cautions

Vibe-Trading 很强，但要保持清醒。

| Risk | Note |
|---|---|
| data quality | public data fallback 方便，但质量、复权、停牌、时区都要核验 |
| lookahead bias | Alpha Zoo 有 guard，但自定义策略仍然要自己审 |
| execution realism | 回测不是实盘，滑点、费用、成交量、冲击成本要单独处理 |
| LLM hallucination | agent 可能生成看似合理但错误的策略解释 |
| generated code | agent 生成的 `signal_engine.py` 要人工 review |
| live trading | 第一阶段不要碰，公开 demo 只做 research-only |
| secrets | 不要把 API key、broker export、私有数据放进公开 repo |

对我们当前阶段来说，最正确的使用方式是：

```text
research and simulation only
```

也就是：

```text
learn system architecture
run public-safe demos
extract R&D Agent design
contribute docs/tests/examples
```

## PR opportunities

如果我们要给 Vibe-Trading 提 PR，应该从真实使用中找问题。

可能方向：

| PR idea | Why it fits us |
|---|---|
| Windows setup notes | 我们本地就是 Windows，可以真实验证 |
| Research Autopilot tutorial | 对新用户很有帮助，也和我们目标一致 |
| local data bridge example | 我们关心真实数据源接入 |
| Chinese quant research demo | 中文用户、A 股、因子研究都相关 |
| Alpha Zoo metadata docs | 因子库学习和整理很需要 |
| LightRAG integration note | 把 run reports index into research memory |
| bias diagnosis checklist | 对 PM review 很有价值 |

原则仍然是：

```text
Use first.
Find real friction.
Submit focused PR.
```

## Personal synthesis

Vibe-Trading 给我的最大启发是：

```text
Quant research agent 不能只会聊天。
它必须能落到工具、数据、代码、回测和证据。
```

真正的研究系统应该能说清楚：

```text
我的假设是什么？
用了什么数据？
策略代码在哪里？
回测配置是什么？
指标是什么？
证据在哪里？
失败原因是什么？
下一轮计划是什么？
谁审核了？
```

Vibe-Trading 已经把很多答案做成了工程模块。

所以我现在对 HKUDS 第一阶段三件套的理解是：

```text
LightRAG     -> research memory
Vibe-Trading -> quant research workflow
nanobot      -> personal agent shell
```

如果再接上 LLMQuant：

```text
LLMQuant     -> finance agent ecosystem / quant knowledge
```

我们的路线会变得很清楚：

```text
Knowledge Layer  -> LightRAG + QuantMind
Workflow Layer   -> Vibe-Trading + Magents
Agent Layer      -> nanobot + custom R&D Agent
Finance Layer    -> LLMQuant + Vibe data/backtest/factors
Public Layer     -> website + reports + GitHub PRs
```

这就是 `Pengyi Quant Research OS v0` 的雏形。

## Next

HKUDS 第一阶段继续：

```text
HKUDS000 -> Study Map
HKUDS001 -> LightRAG
HKUDS002 -> Vibe-Trading
HKUDS003 -> nanobot
HKUDS004 -> HKUDS x LLMQuant x Pengyi Research OS integration
```

下一篇做 `nanobot`。
重点看它能不能作为：

```text
personal always-on agent shell
lightweight local agent runtime
Pengyi Research OS 的个人入口
```
