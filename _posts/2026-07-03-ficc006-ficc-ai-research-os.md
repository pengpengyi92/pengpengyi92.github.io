---
title: "FICC006: FICC x AI Research OS - RAG / Agent / Quant Workflow"
date: 2026-07-03 02:00:00 +0800
categories: [Learning, Finance]
tags: [ficc006, ficc, ai-research-os, rag, graph-rag, agent-workflow, quant-research, research-engineering, forecast-ledger, human-review]
---

这是 `PENGYI_FICC_MAP` 的 `FICC006`。

前面的路线是：

```text
FICC000 -> FICC 总地图
FICC001 -> Fixed Income / Rates / Credit
FICC002 -> Currencies / FX
FICC003 -> Commodities
FICC004 -> Daily FICC Brief Generator
FICC005 -> Event-to-Signal Workflow
FICC006 -> FICC x AI Research OS
```

这一篇要把前面的所有东西合成一个系统：

```text
FICC x AI Research OS
  = RAG memory
  + Graph knowledge
  + Agent workflow
  + Quant validation
  + Forecast ledger
  + Human review
  + Public-safe artifact layer
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

## 一句话总览

FICC x AI Research OS 的核心是：

```text
用 RAG 管知识，用 Graph 管关系，用 Agent 管流程，用 Quant 管验证，用 Ledger 管复盘，用 Human Review 管质量和边界。
```

更工程化一点：

```text
AI Research OS = memory + workflow + validation + evaluation + governance + artifacts.
```

它不是一个“自动交易机器人”。
它是一个研究操作系统：

```text
research question in
  -> retrieve evidence
  -> build event context
  -> generate hypothesis
  -> validate quantitatively
  -> diagnose bias
  -> write report
  -> human review
  -> update memory
  -> next research plan
```

## 为什么是 OS

单篇报告解决的是一次问题。

Research OS 解决的是长期积累问题。

如果没有 OS，每天会变成：

```text
读新闻
写总结
产生想法
忘掉想法
重复读新闻
重复写总结
重复产生想法
```

有 OS 以后，研究变成：

```text
每一次 brief 进入 memory
每一个 event 进入 event store
每一个 hypothesis 进入 research ledger
每一次 validation 进入 evaluation history
每一次 human review 进入 quality feedback
每一次 output 变成可展示 artifact
```

这就是从“学习”变成“资产”的关键。

## 总体架构

FICC x AI Research OS 可以拆成九层：

```text
Layer 1: Data and Document Ingestion
Layer 2: FICC Knowledge Base
Layer 3: RAG Memory
Layer 4: Graph RAG
Layer 5: Agent Workflow
Layer 6: Quant Validation
Layer 7: Forecast / Research Ledger
Layer 8: Evaluation and Governance
Layer 9: Artifact and Presentation Layer
```

完整链路：

```text
public data / notes / reports
  -> normalized knowledge base
  -> RAG and graph memory
  -> daily brief agent
  -> event-to-signal agent
  -> quant validation
  -> bias diagnosis
  -> forecast ledger
  -> public-safe report
  -> human review
```

核心原则：

```text
每一个结论都要可追溯。
每一个假设都要可验证。
每一个结果都要可复盘。
每一个公开输出都要可脱敏。
```

## Layer 1: Data and Document Ingestion

第一层是输入。

FICC 的输入天然是多模态、多来源、多频率的：

```text
market data:
  yields, curves, FX spot, forwards, basis, futures, spreads, vol

macro data:
  CPI, payroll, GDP, PMI, trade balance, inventories, central bank rates

policy text:
  FOMC, ECB, BoJ, PBoC, speeches, minutes, official statements

commodity reports:
  EIA, WASDE, OPEC reports, inventory reports, shipping and weather notes

research text:
  daily briefs, public reports, educational notes, sanitized research memos

internal memory:
  forecast ledger, signal cards, validation reports, human review notes
```

第一版不追求豪华数据源。

第一版要保证：

```text
source clear
timestamp clear
asset clear
unit clear
public/private boundary clear
```

这比堆数据源更重要。

## Layer 2: FICC Knowledge Base

知识库不是简单文件夹。

它应该按研究对象组织：

```text
Fixed Income:
  bonds, rates, yield curve, duration, DV01, credit spread, repo, swaps

FX:
  spot, forward, swap, options, carry, basis, central bank divergence, capital flow

Commodities:
  energy, metals, agriculture, inventory, futures curve, seasonality, geopolitics

Cross-Asset:
  inflation, growth, risk sentiment, dollar liquidity, funding stress, safe haven

Research Methods:
  event study, backtest, bias diagnosis, forecast ledger, human review
```

每个知识条目最好有 metadata：

```text
topic
asset_class
asset
region
time_horizon
source_type
public_safety_level
last_updated
related_events
related_hypotheses
```

这样后面的 RAG 才能精确检索。

## Layer 3: RAG Memory

RAG 的作用是把研究记忆调出来。

它要回答：

```text
这个事件以前出现过吗？
这个变量通常影响哪些资产？
之前我们怎么解释类似市场反应？
有哪些反例？
哪些旧 hypothesis 相关？
哪些 validation report 可以复用？
```

RAG corpus 可以分四层：

```text
Knowledge Notes:
  FICC000 - FICC006

Daily Memory:
  daily briefs and market snapshots

Research Memory:
  signal cards, validation reports, bias reports

Reference Memory:
  official reports, public documentation, public educational materials
```

检索方式不要只有 semantic search。

应该组合：

```text
keyword search
metadata filter
semantic retrieval
time-window retrieval
asset-specific retrieval
event-type retrieval
ledger retrieval
```

例子：

```text
Query:
  USD strengthens while gold also rallies. How to interpret?

Retrieve:
  FICC002 USD liquidity and safe haven section
  FICC003 USD and gold section
  previous daily briefs with USD up / gold up
  risk-off event notes
  real-rate counterexamples
```

这个检索结果会让 agent 不至于只靠当前 prompt 硬猜。

## RAG 的输出要求

RAG 不应该只返回长文本。

它应该返回结构化 evidence pack：

```text
EvidencePack:
  query
  retrieved_items
  key_evidence
  counter_evidence
  historical_analogies
  uncertainty
  source_ids
```

每一个 retrieved item 都要保留：

```text
source
date
topic
asset_class
quote_or_summary
confidence
public_safety_level
```

这对金融研究特别关键。

如果没有 source 和 timestamp，就无法判断证据是否可信。

## Layer 4: Graph RAG

普通 RAG 擅长找文本。
Graph RAG 擅长找关系。

FICC 天然适合图谱，因为它是跨资产因果网络：

```text
central bank -> policy rate -> yield curve -> FX forward -> carry
oil price -> inflation -> central bank reaction -> rates -> FX
USD liquidity -> EM FX -> credit spread -> risk sentiment
inventory shock -> commodity curve -> producer margin -> credit risk
```

核心节点：

```text
Asset
Currency
Country
CentralBank
MacroVariable
Commodity
Curve
Event
Narrative
Hypothesis
Feature
Outcome
Evidence
Forecast
Report
```

核心关系：

```text
Event -> affects -> Asset
MacroVariable -> drives -> Narrative
CentralBank -> sets -> PolicyRate
PolicyRate -> affects -> YieldCurve
YieldCurve -> affects -> FXForward
Commodity -> affects -> Inflation
Inflation -> affects -> CentralBankReaction
Currency -> forms_pair -> CurrencyPair
Evidence -> supports -> Hypothesis
CounterEvidence -> challenges -> Hypothesis
ValidationReport -> evaluates -> Hypothesis
Forecast -> reviews -> Claim
```

Graph RAG 能回答：

```text
今天这个事件会影响哪些二阶变量？
这个 hypothesis 依赖哪些宏观机制？
哪个 evidence 支持它，哪个 evidence 挑战它？
哪些旧 validation report 和它相似？
```

这是 FICC Research OS 的记忆骨架。

## Layer 5: Agent Workflow

Agent 负责把流程自动化，但不能失控。

推荐 agent 分工：

```text
Ingestion Agent:
  收集和标准化输入。

RAG Agent:
  根据问题检索知识、证据和历史案例。

Graph Agent:
  查找跨资产关系和因果路径。

Daily Brief Agent:
  生成 FICC daily brief。

Event Agent:
  抽取 structured events。

Hypothesis Agent:
  生成 testable research hypotheses。

Quant Agent:
  设计 feature / outcome / validation plan。

Skeptic Agent:
  检查反例、偏差、过拟合和因果错误。

Report Agent:
  生成 markdown / json / table outputs。

Human Reviewer:
  审核质量、边界和发布。
```

关键不是 agent 数量。
关键是每个 agent 的输入输出必须固定。

```text
agent output must be schema-valid
agent conclusion must be evidence-grounded
agent forecast must be ledger-tracked
agent public output must be human-reviewed
```

## Agent Harness

FICC Agent Harness 应该规定：

```text
Allowed Inputs:
  public data
  public documents
  sanitized notes
  approved local research memory
  synthetic examples

Forbidden Inputs:
  confidential bank data
  client information
  proprietary strategy parameters
  live positions
  unredacted alpha notes

Allowed Outputs:
  educational explanations
  research hypotheses
  validation plans
  public-safe reports
  bias diagnostic reports

Forbidden Outputs:
  direct trading advice
  position sizing
  live order instructions
  undisclosed alpha details
  client-specific recommendations
```

Validation rules：

```text
Every claim needs evidence.
Every hypothesis needs counter-evidence.
Every feature needs timestamp check.
Every validation needs bias report.
Every public report needs redaction check.
```

这就是 harness 的价值：

```text
让 agent 高效，但不越界。
```

## Layer 6: Quant Workflow

Quant workflow 接住 agent 的研究假设。

路径：

```text
event
  -> hypothesis
  -> feature
  -> outcome
  -> event study
  -> backtest prototype
  -> bias diagnosis
  -> research verdict
```

和 `FICC005` 的连接：

```text
FICC005:
  Event-to-Signal Workflow

FICC006:
  把这个 workflow 放入完整 AI Research OS。
```

Quant 层不应该直接追求“漂亮收益曲线”。

它先要输出：

```text
validation report
bias diagnostic report
regime dependency report
feature version
outcome version
research verdict
next action
```

研究结论可以分级：

```text
rejected
weak evidence
monitor only
promising for further study
needs better data
needs regime split
public demo only
private research only
```

这个分级比“赚没赚钱”更重要。

## Layer 7: Forecast / Research Ledger

Ledger 是 OS 的长期记忆。

它至少要记录三类对象：

```text
Claim Ledger:
  daily brief 里的市场解释和预测。

Hypothesis Ledger:
  event-to-signal 生成的 research hypothesis。

Validation Ledger:
  每次 event study / backtest / bias diagnosis 的结果。
```

字段：

```text
id
date
source
asset_class
claim_or_hypothesis
evidence
counter_evidence
horizon
confidence
validation_status
outcome
review_notes
next_action
```

Ledger 的核心作用：

```text
防止只记住成功案例。
防止每天重复研究同一个问题。
让研究判断可以被校准。
让 agent 有长期记忆。
```

这也是 AI Scientist 的核心能力：

```text
not just generate ideas
but remember, test, and improve ideas
```

## Layer 8: Evaluation and Governance

AI Research OS 必须有评估。

评估分四层：

```text
RAG evaluation
Agent evaluation
Quant evaluation
Human review evaluation
```

RAG 评估：

```text
retrieval relevance
source grounding
timestamp correctness
coverage
counter-evidence retrieval
```

Agent 评估：

```text
schema validity
evidence usage
reasoning consistency
overconfidence control
public-safety compliance
```

Quant 评估：

```text
sample size
out-of-sample behavior
bias diagnosis
regime stability
transaction-cost awareness
feature availability
```

Human review 评估：

```text
edit distance
review pass rate
common failure modes
redaction issues
repeated mistakes
```

Governance 不是为了降低速度。
Governance 是为了让系统能长期跑。

```text
fast output without review becomes noise.
fast output with review becomes compounding asset.
```

## Layer 9: Artifact and Presentation Layer

最终要沉淀成可展示资产。

公开层可以展示：

```text
learning notes
system architecture
public-safe demo
sanitized templates
synthetic examples
validation framework
bias checklist
research reports
GitHub README
website posts
```

私有层保留：

```text
unredacted signal cards
private data experiments
real parameter choices
private research ledger
collaboration notes
career transition materials
```

这就是 public credit 和 private alpha 的分离。

公开展示的重点不是：

```text
我发现了什么可交易秘密。
```

而是：

```text
我能把复杂金融研究问题工程化、系统化、可审计化。
```

## MVP 设计

第一版 MVP 可以很小：

```text
ficc_ai_research_os/
  data/
    sample_market_snapshot.csv
    sample_events.csv
  docs/
    ficc000.md
    ficc001.md
    ficc002.md
    ficc003.md
  memory/
    daily_briefs/
    signal_cards/
    validation_reports/
  ledger/
    forecast_ledger.csv
    research_ledger.csv
  src/
    ingest.py
    retrieve.py
    graph.py
    brief_agent.py
    hypothesis_agent.py
    validate.py
    review.py
  outputs/
    daily_brief.md
    signal_cards.md
    validation_report.md
```

MVP 不要求接入真实交易系统。

MVP 只要求完成闭环：

```text
input sample events
  -> retrieve related FICC knowledge
  -> generate daily brief
  -> generate hypotheses
  -> create signal cards
  -> run simple validation placeholder
  -> produce bias report
  -> update ledger
  -> create public-safe report
```

这个闭环足够展示：

```text
AI + Finance + Quant + Research Engineering
```

## System Interface

第一版接口可以是 CLI：

```text
python -m ficc_os ingest --date 2026-07-03
python -m ficc_os brief --date 2026-07-03
python -m ficc_os hypotheses --date 2026-07-03
python -m ficc_os validate --hypothesis-id H001
python -m ficc_os review --date 2026-07-03
```

也可以是一个 pipeline：

```text
ficc-os run-daily --date 2026-07-03
```

输出：

```text
outputs/daily_brief_2026-07-03.md
outputs/signal_cards_2026-07-03.md
outputs/validation_report_2026-07-03.md
ledger/forecast_ledger.csv
ledger/research_ledger.csv
```

核心不是 CLI 多漂亮。
核心是每一步都有 artifact。

## Pseudocode

系统主流程：

```python
def run_ficc_ai_research_os(date: str):
    inputs = ingest_public_safe_inputs(date)
    knowledge = load_ficc_knowledge_base()
    memory = load_research_memory()
    ledger = load_ledgers()

    evidence_pack = retrieve_relevant_context(
        inputs=inputs,
        knowledge=knowledge,
        memory=memory,
    )

    graph_context = build_cross_asset_graph_context(
        inputs=inputs,
        evidence_pack=evidence_pack,
    )

    daily_brief = generate_daily_brief(
        inputs=inputs,
        evidence_pack=evidence_pack,
        graph_context=graph_context,
    )

    events = extract_events(daily_brief)
    hypotheses = generate_hypotheses(events, evidence_pack, graph_context)
    signal_cards = build_signal_cards(hypotheses)

    validation_reports = []
    for card in signal_cards:
        report = validate_signal_candidate(card)
        report = diagnose_bias(report)
        validation_reports.append(report)

    reviewed_outputs = human_review(
        daily_brief=daily_brief,
        signal_cards=signal_cards,
        validation_reports=validation_reports,
    )

    update_ledgers(reviewed_outputs)
    publish_public_safe_artifacts(reviewed_outputs)
```

关键要求：

```text
No artifact without source.
No hypothesis without mechanism.
No validation without bias report.
No public output without review.
```

## 和前几篇的关系

`FICC004` 是：

```text
Daily Brief Generator
```

它回答：

```text
今天发生了什么？
跨资产叙事是什么？
证据和反例是什么？
```

`FICC005` 是：

```text
Event-to-Signal Workflow
```

它回答：

```text
如何从事件生成可验证 hypothesis？
如何定义 feature / outcome？
如何验证和诊断偏差？
```

`FICC006` 是：

```text
AI Research OS
```

它回答：

```text
如何把 RAG、Graph、Agent、Quant、Ledger、Review 和 Artifact 合成长期运行的研究系统？
```

三者关系：

```text
FICC004 = daily memory
FICC005 = research engine
FICC006 = operating system
```

## 面试怎么讲

如果被问：

```text
你说的 FICC x AI Research OS 是什么？
```

可以回答：

```text
我把它理解成一个面向 FICC research 的研究操作系统。底层是 public-safe 的 FICC knowledge base 和 research memory；中间用 RAG 和 Graph RAG 做证据检索和跨资产关系建模；上层用 agent workflow 生成 daily brief、抽取事件、提出 hypothesis、设计 feature 和 outcome；然后通过 quant validation、bias diagnosis、forecast ledger 和 human review 完成闭环。它不是自动交易系统，而是把金融研究过程结构化、可追踪、可验证、可复盘的系统。
```

如果被问：

```text
RAG 在这个系统里有什么用？
```

可以回答：

```text
RAG 用来把当前事件放回历史和知识上下文。比如今天出现 USD 和 gold 同涨，RAG 会检索以前的 safe haven、real rate、USD liquidity、risk-off 案例，返回 evidence 和 counter-evidence。它不是替代判断，而是给 agent 和研究员提供可追溯的证据包。
```

如果被问：

```text
Agent 在这个系统里有什么风险？
```

可以回答：

```text
主要风险是幻觉、过度自信、因果误判和越过金融合规边界。所以我会用 harness 约束 agent：固定输入输出 schema，限制数据源，要求每个 claim 有 evidence 和 counter-evidence，要求每个 validation 有 bias report，公开输出必须经过 human review，不允许直接输出交易建议、仓位或执行指令。
```

如果被问：

```text
Quant workflow 在里面怎么接？
```

可以回答：

```text
Daily brief 产生 structured events，Event-to-Signal workflow 把事件变成 testable hypothesis，再定义 feature、outcome 和 horizon，做 event study 或 backtest prototype，然后输出 validation report 和 bias diagnostic report。最后结果进入 forecast / research ledger，用于后续复盘和下一轮研究计划。
```

## 对我们的意义

这一篇对我们很关键。

因为它把几条主线统一了：

```text
FICC domain knowledge
AI agent harness
RAG / Graph RAG
Quant research
RD-Agent style automation
Forecast ledger
Credit OS
Public website artifacts
```

这就是我们要做的能力组合：

```text
懂金融场景。
懂 AI 系统。
懂 quant 验证。
懂工程交付。
懂公开呈现。
懂边界控制。
```

它不是银行里的重复流程。
它是可以迁移到 AI lab、quant team、research engineering、AI for finance 的系统能力。

## 下一步

`FICC006` 之后，最自然继续：

```text
FICC007:
  FICC Forecast Ledger and Evaluation

FICC008:
  FICC Interview Playbook

FICC009:
  FICC AI Research OS MVP Spec
```

如果按工程路线，下一篇可以做：

```text
FICC007 -> Forecast Ledger and Evaluation
```

因为没有 evaluation，OS 只是自动生成内容。
有 evaluation，OS 才能真正进化。

## 当前结论

FICC x AI Research OS 可以压成：

```text
RAG:
  retrieve evidence and historical context

Graph:
  model cross-asset relationships

Agent:
  automate structured research workflow

Quant:
  validate hypotheses and diagnose bias

Ledger:
  remember claims, outcomes, and lessons

Human Review:
  control quality, safety, and public boundary

Artifact Layer:
  turn research into public-safe credit
```

一句话收束：

```text
FICC x AI Research OS turns market knowledge into a compounding research machine.
```

中文：

```text
FICC x AI Research OS 把市场知识变成能够持续积累、验证、复盘和展示的研究机器。
```
