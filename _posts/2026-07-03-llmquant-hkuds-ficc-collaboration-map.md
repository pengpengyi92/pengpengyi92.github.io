---
title: "LLMQuant x HKUDS x FICC: AI 金融研究系统合作地图"
date: 2026-07-03 03:30:00 +0800
categories: [Learning, Research OS]
tags: [llmquant, hkuds, ficc, ai-research-os, rag, graph-rag, agent-harness, quant-research, finance-ai, collaboration-map]
---

这是一篇专门整理：

```text
LLMQuant x HKUDS x FICC
```

三者如何合作的 learning note。

前面我们已经分别做过：

```text
LLMQuant:
  finance / quant domain layer

HKUDS:
  general AI infra / RAG / agent / evaluation layer

FICC:
  real financial market scenario
```

现在要把三者合起来。

一句话总结：

```text
LLMQuant 提供金融量化研究语境，HKUDS 提供 AI infra / RAG / agent / evaluation 方法，FICC 提供真实金融市场任务场景。
```

更系统一点：

```text
LLMQuant x HKUDS x FICC
  = finance domain knowledge
  + AI research infrastructure
  + real market workflow
  + public-safe research artifacts
```

公开边界：

```text
This is educational material and research infrastructure thinking.
It is not investment advice, trading advice, or an actionable alpha note.
```

中文边界：

```text
这是公开学习笔记和研究系统设计，不是投资建议。
不包含内部数据、客户信息、未脱敏策略、实盘观点或可交易 alpha。
```

## 总体判断

LLMQuant、HKUDS、FICC 不是三条孤立线。

它们可以组成一个很完整的 AI Finance Research OS：

```text
FICC:
  真实金融研究任务

LLMQuant:
  金融语义、量化任务、策略研究、数据接口、agent workflow

HKUDS:
  RAG、Graph、Agent、Harness、Evaluation、Workspace、Artifact
```

合在一起：

```text
FICC event
  -> LLMQuant finance understanding
  -> HKUDS RAG / agent / harness infra
  -> quant hypothesis
  -> validation and bias diagnosis
  -> public-safe research artifact
```

我们要做的不是简单“套项目”。
我们要做的是：

```text
用 FICC 作为真实场景，把 LLMQuant 和 HKUDS 的能力重新组织成一个可运行、可复盘、可展示的研究系统。
```

## 三者角色分工

| 方向 | 角色 | 代表内容 | 在系统里的作用 |
|---|---|---|---|
| FICC | 场景层 | FI / FX / Commodities / Daily Brief / Event-to-Signal | 提供真实金融研究问题 |
| LLMQuant | 金融领域层 | QuantMind / data-mcp / Skills / Magents / awesome-trading-agents | 提供 finance / quant workflow |
| HKUDS | AI 基础设施层 | LightRAG / RAG-Anything / AutoAgent / OpenHarness / DeepResearch-Eval | 提供 RAG、agent、evaluation、harness |

可以压成：

```text
FICC asks the question.
LLMQuant provides finance-native structure.
HKUDS provides AI-native infrastructure.
```

中文：

```text
FICC 出题。
LLMQuant 给金融结构。
HKUDS 给 AI 基础设施。
```

## Layer 1: Data Layer

FICC 需要的数据包括：

```text
rates
yield curve
credit spreads
FX spot / forward / swap
cross-currency basis
commodity futures
inventory reports
macro calendar
central bank text
news and events
```

LLMQuant 里最直接可合作的是：

```text
data-mcp
```

它对应 FICC 的数据接口层：

```text
FICC market data
  -> MCP-style data access
  -> agent-readable schema
  -> daily brief / event store / validation engine
```

HKUDS 对应的是工具化和接口化：

```text
CLI-Anything
AnyTool
AutoAgent
AgentSpace
```

这些项目启发我们：

```text
不要让 agent 靠自然语言猜数据。
要让 agent 通过明确工具接口拿数据。
```

FICC 合作形态：

```text
FICC Data MCP
  - rates endpoint
  - FX endpoint
  - commodities endpoint
  - macro calendar endpoint
  - event store endpoint
  - forecast ledger endpoint
```

## Layer 2: Knowledge Layer

FICC 最大的问题不是没有信息。
是信息太散：

```text
央行 statement
macro releases
EIA inventory report
WASDE
research reports
market commentary
historical daily briefs
```

LLMQuant 里最重要的是：

```text
QuantMind
finance knowledge layer
```

它适合做：

```text
FICC concept cards
FICC event cards
FICC report cards
FICC hypothesis cards
FICC strategy logic cards
```

HKUDS 里最适合合作的是：

```text
LightRAG
RAG-Anything
MiniRAG
VideoRAG
```

对应能力：

```text
LightRAG:
  fast graph-aware retrieval

RAG-Anything:
  multimodal document understanding: PDF, table, chart, formula

MiniRAG:
  lightweight retrieval memory

VideoRAG:
  视频、访谈、课程、讲座进入知识系统
```

FICC 合作形态：

```text
FICC Knowledge RAG
  = QuantMind finance schema
  + LightRAG graph retrieval
  + RAG-Anything document ingestion
  + MiniRAG lightweight memory
```

用途：

```text
Daily Brief evidence retrieval
central bank stance retrieval
commodity report parsing
historical analogies
counter-evidence search
```

## Layer 3: Graph Layer

FICC 是天然图结构。

关系包括：

```text
central bank -> policy rate -> yield curve -> FX
oil price -> inflation -> rates -> FX -> credit
inventory shock -> futures curve -> commodity FX
USD liquidity -> EM FX -> credit spreads
```

LLMQuant 的合作点：

```text
QuantMind semantic knowledge graph
finance-domain entity and relation schema
```

HKUDS 的合作点：

```text
GraphAgent
OpenGraph
GraphGPT
HiGPT
```

它们启发：

```text
不要只做 text RAG。
要做 FICC cross-asset graph。
```

FICC Graph 可以包含节点：

```text
Asset
Currency
Country
CentralBank
MacroVariable
Commodity
Curve
Event
Claim
Hypothesis
Evidence
ValidationReport
```

关系：

```text
Event -> affects -> Asset
CentralBank -> sets -> PolicyRate
PolicyRate -> affects -> YieldCurve
YieldCurve -> affects -> FXForward
CommodityShock -> affects -> Inflation
Inflation -> affects -> Rates
Evidence -> supports -> Hypothesis
CounterEvidence -> challenges -> Hypothesis
ValidationReport -> evaluates -> Hypothesis
```

合作形态：

```text
FICC Graph RAG
  = QuantMind finance semantics
  + HKUDS graph intelligence
  + FICC cross-asset causality
```

## Layer 4: Agent Layer

FICC Research OS 需要多个 agent。

LLMQuant 合作点：

```text
Skills
Magents
awesome-trading-agents
strategy and risk workflows
```

HKUDS 合作点：

```text
AutoAgent
AgentSpace
OpenSpace
nanobot
FastAgent
ClawTeam / ClawWork
```

这些项目可以启发我们设计：

```text
Rates Agent
FX Agent
Commodities Agent
Macro Agent
Daily Brief Agent
Event Agent
Hypothesis Agent
Validation Agent
Skeptic Agent
Redaction Agent
Ledger Agent
```

关键不是 agent 多。
关键是 agent 必须有 harness：

```text
fixed task
fixed input
fixed output schema
evidence required
counter-evidence required
human review required
```

FICC 合作形态：

```text
FICC Multi-Agent Research Workflow
  = LLMQuant finance task routing
  + HKUDS agent workspace
  + FICC domain agents
```

## Layer 5: Harness Layer

这是 `FICC008` 直接对应的层。

LLMQuant 的 finance agent 如果要用于 FICC，必须有：

```text
data boundary
tool boundary
memory boundary
output schema
bias diagnosis
public-safety review
```

HKUDS 里最相关的是：

```text
OpenHarness
DeepResearch-Eval
```

OpenHarness 对应：

```text
task contract
tool permission
schema validation
review gate
audit log
```

DeepResearch-Eval 对应：

```text
research quality evaluation
evidence grounding
answer faithfulness
counter-evidence coverage
human edit distance
```

FICC 合作形态：

```text
FICC Agent Harness
  = OpenHarness-style control plane
  + LLMQuant finance workflow
  + FICC public-safe policy
```

这个方向非常重要。

因为金融 agent 最大问题不是“能不能生成内容”。
而是：

```text
能不能在边界内生成可审计的研究内容。
```

## Layer 6: Quant Workflow Layer

FICC 最后要进入 quant research。

LLMQuant 直接相关：

```text
Magents
awesome-trading-agents
strategy workflows
factor / strategy hypothesis direction
```

HKUDS 相关：

```text
Vibe-Trading
AI-Trader
FutureShow
AI-Researcher
Auto-Deep-Research
DeepResearch-Eval
```

FICC 对应：

```text
Event-to-Signal Workflow
Forecast Ledger
Validation Report
Bias Diagnosis
```

合作形态：

```text
FICC Event-to-Signal Engine
  = LLMQuant quant strategy workflow
  + HKUDS AI-Researcher loop
  + HKUDS DeepResearch-Eval
  + FICC event store
```

标准流程：

```text
FICC event
  -> hypothesis
  -> feature
  -> outcome
  -> event study
  -> bias diagnosis
  -> research verdict
  -> forecast ledger
```

公开边界：

```text
signal candidate != trading signal
```

公开展示的是：

```text
research hypothesis
validation framework
bias checklist
synthetic demo
```

不是：

```text
live alpha
position sizing
execution instruction
```

## Layer 7: Evaluation Layer

没有 evaluation，Research OS 只是内容生成器。

LLMQuant 可以提供：

```text
finance-specific evaluation:
  data validity
  backtest health
  strategy risk check
  factor robustness
```

HKUDS 可以提供：

```text
DeepResearch-Eval
OpenHarness
research agent evaluation
```

FICC 需要的 evaluation：

```text
Daily Brief Quality
Evidence Grounding
Counter-Evidence Coverage
Cross-Asset Consistency
Timestamp Correctness
Bias Diagnosis Quality
Forecast Calibration
Human Review Pass Rate
Public-Safety Compliance
```

合作形态：

```text
FICC Research Evaluation Suite
  = DeepResearch-Eval style metrics
  + OpenHarness review gate
  + LLMQuant finance robustness checks
```

这能把 FICC 系统从 demo 提升成真正 research infra。

## Layer 8: Artifact Layer

研究要沉淀成可展示资产。

LLMQuant 可以输出：

```text
finance research notes
strategy specs
factor cards
risk reports
quant workflow docs
```

HKUDS 可以参考：

```text
Paper2Slides
Litewrite
DeepResearch reports
agent product workflow
```

FICC 可以输出：

```text
Daily FICC Brief
FICC Event Card
FICC Signal Candidate Card
FICC Bias Report
FICC Forecast Ledger
FICC AI Research OS Demo
FICC Agent Harness Spec
```

合作形态：

```text
FICC Public Artifact Layer
  = public-safe research outputs
  + website learning posts
  + GitHub project docs
  + private research ledger separation
```

这直接服务我们的 Credit OS。

## 总比对表

| FICC 需求 | LLMQuant 侧 | HKUDS 侧 | 合作结果 |
|---|---|---|---|
| 数据接口 | data-mcp | CLI-Anything / AnyTool | FICC Data MCP |
| 金融知识 | QuantMind | LightRAG / RAG-Anything | FICC Knowledge RAG |
| 图谱关系 | QuantMind semantic KG | GraphAgent / OpenGraph / GraphGPT | FICC Graph RAG |
| Agent workflow | Skills / Magents | AutoAgent / AgentSpace / OpenSpace | FICC Multi-Agent Workflow |
| Harness 控制 | finance workflow rules | OpenHarness | FICC Agent Harness |
| 研究自动化 | quant R&D direction | AI-Researcher / Auto-Deep-Research | FICC R&D Agent |
| 交易研究 | Magents / awesome-trading-agents | Vibe-Trading / AI-Trader | FICC Event-to-Signal |
| 预测复盘 | research ledger direction | FutureShow | FICC Forecast Ledger |
| 评估 | backtest / risk check | DeepResearch-Eval | FICC Research Evaluation |
| 公开表达 | strategy notes | Paper2Slides / Litewrite | FICC Public Artifact Layer |

## 优先合作路线

如果按我们当前目标排序，优先级是：

```text
Priority 1:
  QuantMind + LightRAG + RAG-Anything
  -> FICC Knowledge RAG

Priority 2:
  data-mcp + AnyTool / CLI-Anything
  -> FICC Data MCP

Priority 3:
  OpenHarness + DeepResearch-Eval
  -> FICC Agent Harness / Evaluation

Priority 4:
  AI-Researcher + Magents + Vibe-Trading
  -> FICC Event-to-Signal Workflow

Priority 5:
  FutureShow
  -> Forecast Ledger / Prediction Evaluation
```

一句话：

```text
先做知识和 harness，再做 research workflow，再做 quant validation。
```

不要一上来就冲实盘。

先把：

```text
knowledge
evidence
harness
evaluation
ledger
```

打牢。

## 一个具体合作 Demo

可以做一个公开安全 demo：

```text
FICC Daily Research OS Demo
```

输入：

```text
sample market snapshot
sample macro calendar
sample central bank statement
sample EIA-style inventory note
sample FICC learning notes
```

系统：

```text
LLMQuant data-mcp style input
QuantMind-style finance knowledge card
HKUDS LightRAG-style retrieval
HKUDS GraphAgent-style cross-asset graph
HKUDS OpenHarness-style control policy
HKUDS DeepResearch-Eval-style evaluation
```

输出：

```text
Daily FICC Brief
Event Cards
Signal Candidate Cards
Evidence Pack
Counter-Evidence Pack
Bias Diagnostic Report
Forecast Ledger Update
Public-safe Website Post
```

这个 demo 非常适合展示：

```text
AI agent + FICC domain + quant research + public-safe engineering
```

## 对外叙事

对外可以这样讲：

```text
I am building a public-safe FICC AI Research OS.
It combines LLMQuant-style finance research workflow, HKUDS-style RAG / Graph / Agent / Evaluation infrastructure, and real FICC market tasks such as daily brief generation, event-to-signal workflow, forecast ledger, and human-reviewed research artifacts.
```

中文：

```text
我在构建一个公开安全的 FICC AI Research OS。
它把 LLMQuant 的金融量化研究工作流、HKUDS 的 RAG / Graph / Agent / Evaluation 基础设施，以及 FICC 的真实金融研究场景结合起来，形成 daily brief、event-to-signal、forecast ledger 和 human-reviewed research artifact 的闭环。
```

## 面试表达

如果被问：

```text
LLMQuant、HKUDS 和 FICC 怎么结合？
```

可以回答：

```text
我会把 FICC 看成真实金融场景，把 LLMQuant 看成 finance-native research workflow，把 HKUDS 看成 general AI research infrastructure。具体来说，LLMQuant 的 QuantMind 和 data-mcp 可以提供金融知识和数据接口；HKUDS 的 LightRAG、RAG-Anything、GraphAgent 可以提供检索和图谱能力；OpenHarness 和 DeepResearch-Eval 可以提供 agent 控制和评估；AI-Researcher、Vibe-Trading、AI-Trader 可以启发 event-to-signal 和 quant validation workflow。最终合成一个 FICC AI Research OS：从 public-safe data ingestion，到 daily brief，到 event-to-signal，到 validation、bias diagnosis、forecast ledger 和 human review。
```

如果被问：

```text
这个系统和普通金融 RAG 有什么区别？
```

可以回答：

```text
普通金融 RAG 主要回答问题或总结文档。这个系统不止做问答，而是把 RAG 作为 research memory，把 Graph RAG 作为 cross-asset relationship layer，把 agent workflow 作为研究执行层，把 quant validation 作为验证层，把 harness 和 human review 作为控制层，把 forecast ledger 作为复盘层。所以它是一个研究操作系统，而不是单点 RAG 应用。
```

## 当前结论

LLMQuant、HKUDS、FICC 的合作可以压成：

```text
LLMQuant:
  finance domain and quant workflow

HKUDS:
  AI infra, RAG, graph, agent, harness, evaluation

FICC:
  real financial research scenario
```

最终合成：

```text
FICC AI Research OS
  = FICC scenario
  + LLMQuant finance workflow
  + HKUDS AI infrastructure
  + public-safe research artifacts
```

一句话收束：

```text
LLMQuant gives the finance brain, HKUDS gives the AI infrastructure, and FICC gives the real market training ground.
```

中文：

```text
LLMQuant 给金融大脑，HKUDS 给 AI 基础设施，FICC 给真实市场训练场。
```
