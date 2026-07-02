---
title: "HKUDS052: HKUDS Quant 系列专题总结 - Vibe-Trading / AI-Trader / FutureShow / UrbanGPT 对比"
date: 2026-07-02 00:00:00 +0800
categories: [Learning, Quant Research]
tags: [pengyi-hkuds-studymap, hkuds052, hkuds, quant, trading, forecasting, vibe-trading, ai-trader, futureshow, urbangpt, quant-os, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS052`。

在 `HKUDS050` 里，`HKUDS052` 曾经被预告给 `OpenCity`。
现在我们再插入一篇专题总结：

```text
HKUDS052 -> HKUDS Quant / Forecasting / Trading 系列专题总结
```

原因很直接：

```text
HKUDS 里和 quant 有关的项目已经不是单点启发。
它们已经能拼出一个 Quant Research OS 的系统骨架。
```

所以这篇先不继续单仓推进，而是把之前已经看过的 HKUDS quant-relevant 项目重新放在一张图里。

核心项目：

```text
HKUDS002 -> Vibe-Trading
HKUDS005 -> AI-Trader
HKUDS020 -> FutureShow
HKUDS049 -> UrbanGPT
```

强相关支撑项目：

```text
HKUDS001 -> LightRAG
HKUDS007 -> RAG-Anything
HKUDS018 -> MiniRAG
HKUDS021 -> VideoRAG

HKUDS024 -> GraphAgent
HKUDS025 -> OpenGraph
HKUDS026 -> GraphGPT
HKUDS027 -> HiGPT

HKUDS028 -> RecLM
HKUDS029 -> XRec
HKUDS030 -> AutoCF
HKUDS031 -> KGRec

HKUDS015 -> OpenHarness
HKUDS017 -> AnyTool
HKUDS035 -> FastAgent
HKUDS008 -> AutoAgent

HKUDS045 -> CatchMe
HKUDS047 -> SepLLM
HKUDS048 -> MGP
```

这篇目标是回答：

```text
HKUDS 给我们的 Quant Research OS 到底有哪些启发？
Vibe-Trading / AI-Trader / FutureShow / UrbanGPT 分别解决哪一层？
它们和 LLMQuant、X2Strategy、QuantMind 怎么拼起来？
我们自己的 Pengyi Quant Research OS v0 应该怎么落地？
```

## 一句话总览

我现在对 HKUDS Quant 系列的判断是：

```text
HKUDS Quant stack = research workflow + agent trading platform + forecasting benchmark + spatio-temporal model + RAG/graph/memory/harness support.
```

中文：

```text
HKUDS 不是只给了一个 trading bot。
它给的是 AI-native quant research system 的若干关键模块：
让 agent 读材料、提假设、调工具、跑回测、做预测、进入平台、接受评估、沉淀记忆、输出 artifact。
```

如果压成一张图：

```text
finance papers / reports / videos / market docs
  -> RAG / Graph / Memory layer

natural language quant question
  -> Vibe-Trading research workflow

hypothesis / strategy / backtest
  -> strategy artifact and research ledger

event / market judgment
  -> FutureShow forecast ledger

agent identity / signal / paper trading / challenge
  -> AI-Trader platform layer

time-series / spatio-temporal tensor
  -> UrbanGPT-style numeric forecasting model

all outputs
  -> Research OS / Quant OS / website / pitch / interview narrative
```

这就是我们要学的东西。

不是单纯会写一个策略。
而是会搭一个系统，让策略研究能够自动化、可复现、可解释、可审计、可展示。

## 为什么要单独总结 Quant 系列

之前我们分散看 HKUDS 时，每个项目都有自己的主题：

```text
Vibe-Trading -> quant research workflow
AI-Trader    -> agent-native trading platform
FutureShow   -> forecasting benchmark
UrbanGPT     -> spatio-temporal LLM forecasting
LightRAG     -> knowledge memory
GraphAgent   -> graph reasoning
FastAgent    -> tool execution harness
MGP          -> memory governance
```

分开看时，它们像很多 repo。

合起来看时，它们像一个操作系统。

这个系统回答的是：

```text
一个 AI quant researcher 怎么工作？
一个 trading agent 怎么被评估？
一个 forecasting agent 怎么留下可验证记录？
一个 market model 怎么把结构化数值信号接进 LLM？
一个 research output 怎么变成 public credit？
```

这就是 `Pengyi Quant Research OS` 的核心问题。

## 核心四件套

先把四个核心项目放在一起：

| HKUDS | 项目 | 一句话定位 | 在 Quant OS 里的位置 |
|---|---|---|---|
| `HKUDS002` | `Vibe-Trading` | 把自然语言金融问题转成数据、策略、回测、报告和研究证据 | Research workflow / backtest layer |
| `HKUDS005` | `AI-Trader` | 让 agent 注册、发信号、paper trade、copy trade、比赛和被研究 | Trading platform / agent society layer |
| `HKUDS020` | `FutureShow` | 用 prediction market 事件评估 agent 的未来判断能力 | Forecast benchmark / judgment ledger |
| `HKUDS049` | `UrbanGPT` | 把时空张量编码成 LLM token 并接预测头 | Numeric forecasting / market tensor model layer |

这四个项目的关系不是互相替代。

它们是四层：

```text
Vibe-Trading:
  research workflow

AI-Trader:
  platform and social trading environment

FutureShow:
  verifiable forecast evaluation

UrbanGPT:
  domain tensor -> LLM -> numeric prediction architecture
```

## Vibe-Trading

`Vibe-Trading` 是最接近我们早期 `Quant R&D Agent` 想象的 HKUDS 项目。

它解决的问题是：

```text
把一个自然语言金融问题，变成可运行、可回测、可记录、可复盘的研究流程。
```

它不是普通聊天机器人。

它更像：

```text
agentic finance research workspace
```

它的核心链路可以写成：

```text
question
  -> research goal
  -> hypothesis
  -> market data loading
  -> strategy / signal code
  -> backtest config
  -> backtest execution
  -> run card
  -> evidence ledger
  -> next research step
```

这正好对应我们之前提出的：

```text
R&D Agent for Quant Research
  = 自动提出因子假设
  + 自动实现
  + 自动回测
  + 自动诊断偏差
  + 自动生成下一轮研究计划
  + 人类 PM 审核
```

### Vibe-Trading 的关键组件

我们之前看过的关键点包括：

```text
CLI / TUI
FastAPI backend
Web UI
MCP server
finance skills
data loaders
backtest runner
multiple backtest engines
research autopilot
hypothesis registry
goal and evidence ledger
alpha zoo
swarm teams
shadow account
live trading boundary
```

最关键的不是某个工具。

最关键的是它已经把 quant research 的对象拆成了系统对象：

```text
hypothesis
backtest_config
signal_engine
run_card
metrics
evidence
artifact
```

这对我们非常重要。

因为一个真正的 Quant OS 不能只有 notebook。

Notebook 可以做探索。
但 OS 需要把探索变成可追踪对象。

### Vibe-Trading 给我们的最大启发

第一，quant agent 必须有 workflow contract。

```text
输入是什么？
输出是什么？
中间 artifact 是什么？
失败怎么记录？
下一轮怎么生成？
```

第二，数据源必须被抽象。

Vibe-Trading 里的 data loader / fallback chain 给了一个实用模式：

```text
public source
  -> fallback source
  -> local cache
  -> private loader
```

我们以后接 WorldQuant 脱敏样本、公开行情、FICC 公开资料、研报摘要、宏观数据时，都应该走类似的 DataAccess 层。

第三，alpha / factor 必须有 metadata。

一个因子不能只是函数。

它应该有：

```text
factor_id
description
universe
frequency
lookback
inputs
neutralization
expected behavior
risk notes
implementation path
test result
owner / source
```

这就是以后 `Pengyi Quant Research OS` 的 factor registry。

### Vibe-Trading 不应该被误读

它不是让我们第一阶段去做真实下单。

第一阶段应该是：

```text
research-only
public-safe
backtest-first
human-reviewed
no autonomous real-money execution
```

这条边界必须清楚。

做 quant 系统时，最容易犯的错误就是过早碰 live trading。

我们的第一阶段目标不是赚钱展示。

第一阶段目标是展示：

```text
research engineering ability
factor thinking
data discipline
evaluation discipline
agent workflow design
public artifact production
```

## AI-Trader

`AI-Trader` 和 `Vibe-Trading` 很容易混在一起。

但两者关注点不一样。

```text
Vibe-Trading:
  how to do agentic quant research

AI-Trader:
  where trading agents live, signal, compete, interact, and get studied
```

`AI-Trader` 更像：

```text
agent-native live trading platform layer
```

它的核心命题是：

```text
人类有交易平台，agent 也需要自己的交易平台。
```

这个想法非常关键。

因为当 agent 数量变多后，问题不再只是“单个 agent 会不会生成策略”。

问题变成：

```text
agent 如何拥有身份？
agent 如何发布观点？
agent 如何记录操作？
agent 如何和其他 agent 互动？
agent 如何被排名？
agent 如何被复制跟随？
agent 的行为如何导出给研究者分析？
```

### AI-Trader 的关键对象

`AI-Trader` 把 agent 的表达拆成几类：

```text
strategy signal
discussion signal
realtime operation
position
order
portfolio
leaderboard
follow relation
copy trading relation
challenge
experiment
research export
```

这说明它不是单纯策略生成器。

它是一个 agent trading society。

### AI-Trader 对 Quant OS 的启发

对我们的系统来说，`AI-Trader` 提醒我们：

```text
Quant OS 不能只停在文件夹和脚本。
它最终应该有平台层。
```

平台层至少包括：

```text
agent identity
strategy publication
paper portfolio
experiment board
leaderboard
risk dashboard
discussion thread
human review
research export
```

这对 public credit 很关键。

因为外部世界不容易理解一个本地 notebook。

但外部世界可以理解：

```text
一个 agent dashboard
一个 strategy card
一个 backtest report
一个 leaderboard
一个 forecast ledger
一个 research export
```

### AI-Trader 和真实交易边界

`AI-Trader` 的启发很强，但边界也要清楚：

```text
paper trading != live trading
copy trading can amplify bad signals
agent ranking can create perverse incentives
real-money broker integration has security and legal risk
```

所以我们的第一阶段只做：

```text
paper portfolio
simulated order
public-safe dataset
human PM approval
risk limit
research export
```

这才是合理路线。

## FutureShow

`FutureShow` 是 HKUDS quant 系列里最容易被低估的项目。

它不是传统交易系统。

它解决的是一个更底层的问题：

```text
如何评估 agent 对未来事件的判断能力？
```

这个问题对 quant 非常重要。

因为 quant 本质上是在做带约束的未来判断：

```text
未来收益
未来波动
未来风险
未来事件概率
未来 regime
未来资金流
```

`FutureShow` 用 prediction market 把判断能力变成可记录对象。

核心链路：

```text
Polymarket event
  -> agent reads market context
  -> agent outputs YES / NO / ABSTAIN
  -> store timestamped forecast
  -> compare with market consensus
  -> wait outcome
  -> evaluate accuracy / prediction value
  -> dashboard
```

### Forecast Ledger

`FutureShow` 最重要的工程思想是 forecast ledger。

每次预测都应该留下：

```text
timestamp
event_id
question
market_probability
agent_prediction
agent_confidence
reasoning
evidence
model_version
outcome
score
```

这比普通 prompt 预测严肃得多。

因为它能回答：

```text
你当时到底预测了什么？
你当时看到的市场共识是什么？
你是领先市场，还是跟随市场？
你的预测有没有实际信息增量？
结果出来后怎么复盘？
```

### Prediction Market 的意义

Prediction market 的价格可以理解成 crowd consensus。

这给 agent evaluation 提供了一个强 baseline。

普通 benchmark 是：

```text
agent vs static label
```

FutureShow 更接近：

```text
agent vs market-implied probability
```

这对 quant 很关键。

因为交易策略不是和 0 比。

交易策略要和：

```text
market consensus
benchmark
risk model
transaction cost
alternative strategy
capacity constraint
```

比较。

### FutureShow 对 Quant OS 的启发

我们自己的 Quant OS 需要一个 `JudgmentLedger`。

它不只记录回测结果，也记录研究判断。

例如：

```text
macro_view:
  next 3 months rates volatility likely higher

factor_view:
  short-term reversal decay may weaken in high volatility regime

strategy_view:
  trend following expected to outperform mean reversion this month

risk_view:
  drawdown risk elevated due to sector crowding
```

每个 view 都应该有：

```text
timestamp
evidence
confidence
market baseline
test plan
future check date
outcome
review
```

这就是 quant judgment 的可审计化。

## UrbanGPT

`UrbanGPT` 表面是 Urban / Spatio-temporal AI。

但它对 Quant OS 的意义很大。

因为它回答了一个很关键的问题：

```text
如何把结构化时空张量接入 LLM？
```

金融市场不是纯文本。

金融市场有大量结构化信号：

```text
price tensor
volume tensor
order flow tensor
factor tensor
risk exposure tensor
cross-asset relation
calendar event
sector / industry / macro context
```

如果只让 LLM 读文字，它永远停在解释层。

如果能把 market tensor 编码成 token representation，再接 LLM 和 prediction head，就进入更严肃的建模层。

### UrbanGPT 的核心结构

我们之前把 UrbanGPT 理解成：

```text
spatio-temporal data
  -> ST encoder
  -> ST projector
  -> special tokens
  -> LLM
  -> forecasting token hidden state
  -> numeric prediction head
  -> MAE / RMSE / MAPE / F1 evaluation
```

它不是简单 prompt：

```text
请预测明天交通流量
```

而是把真实数值数据编码进模型。

这点对 quant 很关键。

### QuantGPT 映射

UrbanGPT 可以迁移成一个 `QuantGPT` 原型：

```text
market / factor tensor
  -> market encoder
  -> market projector
  -> market special tokens
  -> LLM
  -> forecasting / alpha token hidden state
  -> numeric head
  -> backtest evaluator
```

对应关系：

| UrbanGPT | Quant OS |
|---|---|
| region | asset / sector / market |
| time interval | bar interval / trading session / earnings calendar |
| traffic flow | return / volume / volatility / factor exposure |
| POI context | sector / macro / event / news context |
| ST encoder | market encoder |
| forecasting token | alpha / risk / forecast token |
| prediction head | return / risk / regime / rank head |

### UrbanGPT 的边界

UrbanGPT 不是直接 alpha。

迁移到 finance 时必须增加：

```text
walk-forward validation
point-in-time data
survivorship bias control
transaction cost
turnover
capacity
market impact
risk exposure
benchmark comparison
out-of-sample check
```

否则 numeric prediction accuracy 不等于 strategy profitability。

这一点必须写在系统设计里。

## 四者横向对比

把四个核心项目横向放在一起：

| 维度 | Vibe-Trading | AI-Trader | FutureShow | UrbanGPT |
|---|---|---|---|---|
| 核心问题 | 怎么做 quant research | agent 在哪里交易和互动 | 怎么评估未来判断 | 怎么把时空张量接进 LLM |
| 输入 | finance question / market data / hypothesis | agent signal / operation / portfolio | prediction market event | ST tensor / instruction |
| 输出 | strategy code / backtest / run card | signal / paper position / leaderboard / export | timestamped forecast / score | numeric prediction |
| 关键对象 | hypothesis / config / run card | identity / signal / copy / challenge | forecast ledger / market baseline | ST token / forecast token / prediction head |
| 最像什么 | research workflow OS | trading agent platform | judgment benchmark | domain foundation model |
| 对 Quant OS 位置 | strategy research layer | platform layer | evaluation layer | model layer |
| 第一阶段可复刻性 | 高 | 中 | 高 | 中到高 |
| 最大风险 | backtest bias | live trading / incentive risk | event selection bias | data leakage / finance 迁移过度 |

我的判断：

```text
先做 Vibe-Trading + FutureShow 风格的小闭环。
再接 AI-Trader 风格的平台展示。
UrbanGPT 风格模型层可以作为第二阶段研究原型。
```

## HKUDS Quant Stack 的支撑层

四个核心项目之外，HKUDS 还有很多支撑模块。

这些模块决定 Quant OS 能不能真正跑起来。

## RAG / Knowledge Layer

相关项目：

```text
LightRAG
RAG-Anything
MiniRAG
VideoRAG
```

在 Quant OS 中，它们负责：

```text
financial reports
strategy papers
public filings
macro notes
research memos
meeting transcripts
interview videos
seminar videos
factor notes
```

它们不直接交易。

但它们负责让 agent 有知识底座。

对应结构：

```text
RAG-Anything:
  ingest PDF / table / formula / image / paper

LightRAG:
  graph-based text knowledge memory

MiniRAG:
  lightweight local memory

VideoRAG:
  timestamped video / lecture / interview memory
```

对 quant 来说：

```text
没有 knowledge grounding 的 quant agent 容易胡说。
没有 source citation 的 quant report 很难被 PM 信任。
没有 memory 的 research loop 无法复利。
```

## Graph Layer

相关项目：

```text
GraphAgent
OpenGraph
GraphGPT
HiGPT
```

它们给 Quant OS 的启发是：

```text
market is a graph
```

金融世界不是独立时间序列。

它是：

```text
company -> supplier -> customer
company -> sector -> theme
asset -> factor -> risk exposure
event -> asset -> impact path
analyst report -> company -> thesis
portfolio -> position -> risk cluster
```

Graph 系列可以进入：

```text
asset graph
factor graph
event graph
research lineage graph
portfolio risk graph
strategy dependency graph
```

这对 R&D Agent 很重要。

因为很多策略失败不是单点参数问题，而是关系理解问题。

例如：

```text
factor crowding
sector exposure
liquidity cluster
macro beta leakage
supplier/customer chain event propagation
```

这些都需要 graph reasoning。

## Recommendation / Ranking Layer

相关项目：

```text
RecLM
XRec
AutoCF
KGRec
```

推荐系统表面看起来离 quant 有距离。

但它们其实回答的是：

```text
如何从稀疏行为、用户偏好、图关系和语义表示里做排序决策？
```

Quant 里也有排序：

```text
stock ranking
factor ranking
strategy ranking
paper ranking
research idea ranking
risk alert ranking
portfolio candidate ranking
```

推荐系统的思想可以迁移到：

```text
PM preference modeling
factor profile
strategy profile
research idea recommendation
analyst note retrieval ranking
asset candidate ranking
```

尤其是 XRec 的 explainable recommendation，对 quant report 很有用。

因为最终我们不只是要给一个排序。

还要说明：

```text
为什么这个信号值得看？
为什么这个策略应该进入下一轮？
为什么这个风险需要优先处理？
```

## Agent Harness / Tool Layer

相关项目：

```text
OpenHarness
AnyTool
FastAgent
AutoAgent
CLI-Anything
nanobot
```

这些项目给 Quant OS 的执行壳。

Quant agent 不是一个 prompt。

它需要：

```text
tool registry
permission control
MCP integration
shell / file / web / API execution
state logging
task planning
step evaluation
failure recovery
human approval
```

对应到 quant：

```text
download data
clean data
inspect schema
generate factor
run backtest
plot metrics
write report
open PR
publish artifact
```

这些都要通过 harness 管起来。

## Memory / Governance Layer

相关项目：

```text
CatchMe
SepLLM
MGP
```

Quant OS 的记忆不能只是聊天记录。

它应该分成：

```text
raw event memory
research session memory
factor memory
strategy memory
backtest artifact memory
decision memory
PM review memory
policy / audit memory
```

`CatchMe` 给 personal digital footprint recorder。

`SepLLM` 给 long-context / KV cache / memory compression 启发。

`MGP` 给 memory governance protocol：

```text
policy
adapter
audit
return mode
expire
revoke
delete
purge
```

这对金融系统非常关键。

因为数据权限、隐私、合规、溯源都不能靠口头管理。

## Artifact / Presentation Layer

相关项目：

```text
Paper2Slides
Litewrite
ViMax
VideoAgent
FastCode
DeepCode
```

Quant research 最后必须产出 artifact。

常见 artifact：

```text
research memo
backtest report
factor card
strategy card
risk review
slide deck
demo website
code repo
PR
interview story
```

这也是 credit OS 的关键。

一个结果如果不能被清楚展示，就很难形成外部授信。

所以 Quant OS 不只是内部自动化系统。

它也必须是：

```text
public-safe presentation system
```

## 和 LLMQuant 的关系

HKUDS 给我们的是 AI infrastructure 和 agent system 视角。

LLMQuant 给我们的是 finance domain ecosystem 视角。

对应关系：

| LLMQuant | HKUDS 对应 | 合起来的意义 |
|---|---|---|
| `data-mcp` | Vibe-Trading data loaders / AnyTool | 数据与工具访问层 |
| `skills` | OpenHarness / FastAgent skills | finance workflow routing |
| `QuantMind` | LightRAG / GraphAgent / MGP | 金融知识结构化与记忆治理 |
| `Magents` | Vibe-Trading backtest / AI-Trader paper trading | 策略执行与模拟层 |
| `awesome-trading-agents` | HKUDS project map | ecosystem radar |
| finance docs / quant-wiki | RAG-Anything / LightRAG | domain grounding corpus |

我的理解：

```text
LLMQuant = finance agent ecosystem and domain stack
HKUDS = AI agent infrastructure and research/product stack
```

我们要做的是把两者拼起来。

## 和 X2Strategy 的关系

`X2Strategy` 是另一个关键拼图。

它解决的问题是：

```text
paper / idea -> structured strategy spec -> executable code -> validation -> backtest diagnosis
```

它更像：

```text
paper-to-strategy compiler
```

和 HKUDS 四件套对比：

| 项目 | 位置 |
|---|---|
| X2Strategy | strategy compiler layer |
| Vibe-Trading | research workflow and backtest workspace |
| AI-Trader | agent trading platform |
| FutureShow | forecast / judgment benchmark |
| UrbanGPT | numeric forecasting model pattern |
| QuantMind | financial knowledge structuring |
| LightRAG / RAG-Anything | retrieval and grounding |

一个完整链路可以是：

```text
paper / report / idea
  -> RAG-Anything / QuantMind
  -> X2Strategy paper2spec
  -> Vibe-Trading backtest workflow
  -> FutureShow-style judgment ledger
  -> AI-Trader-style paper trading platform
  -> Litewrite / Paper2Slides / website artifact
```

这就是 `Pengyi Quant Research OS` 的完整想象。

## Pengyi Quant Research OS v0

我建议我们把自己的 v0 拆成十层。

```text
1. EvidenceAccess
2. KnowledgeMemory
3. HypothesisRegistry
4. StrategySpecCompiler
5. ImplementationHarness
6. BacktestEvaluator
7. ForecastLedger
8. RiskAndBiasDiagnosis
9. HumanPMReview
10. ArtifactPublisher
```

每一层都有现成参照。

| Pengyi Layer | 参考项目 |
|---|---|
| EvidenceAccess | LLMQuant data-mcp, Vibe data loaders, AnyTool |
| KnowledgeMemory | QuantMind, LightRAG, RAG-Anything, MiniRAG |
| HypothesisRegistry | Vibe-Trading research autopilot |
| StrategySpecCompiler | X2Strategy |
| ImplementationHarness | OpenHarness, FastAgent, AutoAgent |
| BacktestEvaluator | Vibe-Trading, Magents |
| ForecastLedger | FutureShow |
| RiskAndBiasDiagnosis | Vibe backtest diagnosis, MLRL evaluation, quant checklist |
| HumanPMReview | AI-Trader platform, research review workflow |
| ArtifactPublisher | Litewrite, Paper2Slides, website, Cloudflare |

## v0 的最小工程闭环

第一版不要做太大。

最小闭环可以是：

```text
Input:
  one public strategy idea
  one public dataset
  one simple universe

Process:
  write hypothesis
  generate strategy spec
  implement signal
  run backtest
  diagnose bias
  write next plan
  generate report

Output:
  factor card
  backtest card
  research memo
  website post
```

建议目录：

```text
pengyi-quant-research-os/
  data/
    raw/
    processed/
    cache/

  research/
    hypotheses/
    specs/
    runs/
    reports/

  src/
    data_access/
    factors/
    strategies/
    backtest/
    evaluation/
    reporting/

  artifacts/
    factor_cards/
    backtest_reports/
    plots/
    exports/

  docs/
    design.md
    public_safe_boundary.md
    pm_review_checklist.md
```

第一条 demo 可以非常朴素：

```text
moving average crossover
or volatility breakout
or simple reversal factor
```

重点不是策略复杂。

重点是系统完整：

```text
数据可追踪
假设可解释
实现可复现
回测可审计
偏差可诊断
报告可展示
下一轮可生成
```

## v0 的数据边界

数据是 quant 最大的问题之一。

第一阶段建议：

```text
public data only
no scraped private platform data
no proprietary factor leakage
no employer confidential data
no account-level sensitive data
```

可用数据方向：

```text
public OHLCV
public macro data
public crypto data
public filings / reports
synthetic factor examples
sanitized WorldQuant-style toy examples
paper reproduction datasets
```

如果以后整理 WorldQuant 因子，要严格做 public-safe：

```text
only toy examples
only transformed public-safe templates
no private contest data
no platform-protected content
no account-specific result leakage
```

这个边界比“酷”更重要。

能长期做公开项目，靠的是 discipline。

## v0 的评估纪律

Quant OS 必须防止常见偏差。

基础 checklist：

```text
look-ahead bias
survivorship bias
data snooping
multiple testing
overfitting
transaction cost
slippage
turnover
liquidity
capacity
regime dependence
benchmark mismatch
risk exposure
calendar alignment
point-in-time availability
```

每个 backtest report 至少要问：

```text
数据是否 point-in-time？
信号是否用了未来信息？
样本外表现如何？
换手率是否过高？
收益来自 beta 还是 alpha？
交易成本后是否还成立？
在哪些 regime 失效？
是否和已有 factor 高度相关？
```

这就是我们说的“拷打”。

不是为了否定自己。

是为了让系统结果更可信。

## Quant Agent 的角色拆分

一个完整 Quant OS 里，agent 不应该只有一个。

可以拆成：

```text
ResearcherAgent:
  propose hypotheses

DataAgent:
  fetch and validate data

ImplementerAgent:
  write factor / strategy code

BacktestAgent:
  run simulation and metrics

BiasDoctorAgent:
  diagnose leakage / overfit / cost / regime risk

PMReviewAgent:
  challenge assumptions and decide next round

ReportAgent:
  generate factor card / memo / website post
```

这和 Jump Trading / quant team 里的 research 与 develop 分工有相似性。

也和我们之前说的 RD 很契合：

```text
Research:
  what hypothesis is worth testing?

Develop:
  can it be implemented, tested, diagnosed, deployed as a reliable artifact?
```

## 对外展示怎么做

Quant 项目最容易做成“我本地跑过一个 notebook”。

这不够。

对外展示需要：

```text
README
architecture diagram
public-safe demo
factor card
backtest report
limitations
reproducible command
website article
small demo video
interview talking points
```

一个 strategy card 可以包括：

```text
strategy_id
idea
economic intuition
data source
universe
signal definition
portfolio construction
backtest period
metrics
risk analysis
known failure modes
next experiments
```

这就是 credit OS。

不是只做事情。

还要让事情可验证、可理解、可传播。

## 面试可用表达

如果被问：

```text
你怎么看 AI + Quant Research？
```

可以这样答：

```text
我不把 AI quant 理解成一个聊天机器人直接给交易信号。
我更倾向于把它设计成 Quant Research OS。

这个 OS 需要把金融材料、市场数据、研究假设、策略 spec、代码实现、回测评估、偏差诊断、forecast ledger、人类 PM review 和报告产出串成闭环。

我研究过 HKUDS 里的 Vibe-Trading、AI-Trader、FutureShow 和 UrbanGPT。
Vibe-Trading 给了 research workflow 和 backtest layer。
AI-Trader 给了 agent-native trading platform 和 paper trading society。
FutureShow 给了 timestamped forecast ledger 和 prediction market baseline。
UrbanGPT 给了 structured time-series tensor 接入 LLM 并用 prediction head 做数值预测的架构。

我想做的是把这些思想和 LLMQuant / QuantMind / X2Strategy 结合起来，做一个 public-safe 的 Quant Research OS 原型。
```

这段可以直接用于 AI / Quant / Research Engineer / RA 面试。

## 不能吹过头的地方

我们要保持清醒。

不能把下面这些说成已经完成：

```text
real-money trading system
production-grade alpha engine
institutional data pipeline
guaranteed profitable strategy
fully autonomous trading agent
```

我们现在更准确的定位是：

```text
AI-native quant research workflow designer
research engineering learner
public-safe quant system builder
agent harness + finance workflow integrator
```

这已经足够强。

关键是别把边界说坏。

## 当前最值得做的 demo

我建议 Quant OS v0 的第一版 demo 这样做：

```text
Demo name:
  Pengyi Quant Research OS v0

Question:
  Can a simple public market signal be turned into a reproducible research object?

Input:
  public OHLCV data
  one simple hypothesis

Workflow:
  hypothesis -> strategy spec -> implementation -> backtest -> diagnosis -> report -> next plan

Output:
  local repo
  README
  factor card
  backtest report
  website write-up
```

可以先做三条 baseline：

```text
moving average trend
short-term reversal
volatility breakout
```

每条都严格记录：

```text
what was tested
what data was used
what assumptions were made
what failed
what next
```

这比一上来做复杂模型更好。

因为它能展示系统能力。

## 对 HKUDS 后续路线的影响

由于 `HKUDS052` 被我们用来做 Quant 系列专题总结，后面的 Urban / ST 单仓继续顺延：

```text
HKUDS053 -> OpenCity
HKUDS054 -> EasyST
HKUDS055 -> AutoST
```

这样路线更清晰：

```text
HKUDS051 -> RAG summary
HKUDS052 -> Quant summary
HKUDS053+ -> Urban / ST forecasting continuation
```

也就是：

```text
先把已做主线收束成专题总结。
再继续开新单仓。
```

## 当前结论

HKUDS Quant 系列给我们的不是一个单点项目，而是一组系统启发：

```text
Vibe-Trading:
  quant research workflow and backtest layer

AI-Trader:
  agent-native trading platform and paper trading society

FutureShow:
  timestamped forecast ledger and market baseline evaluation

UrbanGPT:
  structured time-series tensor -> LLM -> numeric prediction head

RAG / Graph / Harness / Memory:
  knowledge, relation, execution, governance, and artifact layers
```

所以我们的下一步不是空喊“AI quant”。

下一步应该是做一个小而完整的：

```text
Pengyi Quant Research OS v0
```

第一阶段目标：

```text
public-safe
research-only
reproducible
artifact-rich
human-reviewed
```

这条线很清楚。

它能同时服务：

```text
quant job interview
AI research role
RA / PhD application
open-source project
personal credit OS
```

这就是 HKUDS052 的核心结论。
