---
title: "FICC005: Event-to-Signal Workflow - 从市场事件到研究假设、验证与复盘"
date: 2026-07-03 01:30:00 +0800
categories: [Learning, Finance]
tags: [ficc005, ficc, event-to-signal, quant-research, research-os, forecast-ledger, backtesting, bias-diagnosis, human-review, public-safe]
---

这是 `PENGYI_FICC_MAP` 的 `FICC005`。

前面的路径是：

```text
FICC000 -> FICC 总地图
FICC001 -> Fixed Income / Rates / Credit
FICC002 -> Currencies / FX
FICC003 -> Commodities
FICC004 -> Daily FICC Brief Generator
FICC005 -> Event-to-Signal Workflow
```

`FICC004` 解决的是：

```text
每天发生了什么？
这些事件如何被整理成 cross-asset research brief？
```

`FICC005` 解决的是下一层：

```text
从这些事件和 brief 里，如何形成可验证的研究假设？
如何把 hypothesis 变成 public-safe signal candidate？
如何做验证、防偏差、复盘和下一轮研究？
```

公开边界：

```text
This is educational material and research infrastructure thinking.
It is not investment advice, trading advice, or an actionable alpha note.
```

中文边界：

```text
这是公开学习笔记和研究流程设计，不是投资建议。
不包含内部数据、客户信息、未脱敏策略、实盘观点或可交易 alpha。
这里的 signal 指 research signal candidate，不是实盘交易指令。
```

## 一句话总览

Event-to-Signal Workflow 的本质是：

```text
把 FICC 市场事件转化为结构化研究假设，再通过数据验证、偏差诊断、稳健性测试和人工审核，筛选出值得继续研究的 signal candidate。
```

更工程化一点：

```text
Event-to-Signal = event extraction + hypothesis generation + feature design + validation + bias diagnosis + forecast ledger + human review.
```

它不是：

```text
event -> buy / sell
```

而是：

```text
event -> hypothesis -> feature -> test -> diagnosis -> review -> next research plan
```

这就是 quant research 和 AI research agent 的交汇点。

## 为什么需要 Event-to-Signal

Daily Brief 能把市场信息整理清楚。
但如果每天只写 brief，研究还停留在文字层。

真正进入 quant research，需要多走几步：

```text
1. 从事件里抽取可重复模式。
2. 把模式写成明确假设。
3. 把假设翻译成可计算变量。
4. 用历史数据验证。
5. 识别数据泄露、过拟合、选择偏差、交易成本和 regime dependency。
6. 决定是否进入下一轮研究。
```

这就是 Event-to-Signal Workflow。

它对我们尤其重要，因为它把几条主线合起来：

```text
FICC market knowledge
RAG and event extraction
Quant factor research
Forecast ledger
RD-Agent style research automation
Human PM review
```

## Signal 不是交易指令

这里必须先澄清：

```text
signal candidate != trading signal
```

在公开学习笔记里，`signal candidate` 更准确地表示：

```text
一个值得被验证的结构化研究假设。
```

例如：

```text
oil inventory surprise may be associated with short-term crude curve repricing.
```

这不是说：

```text
立刻交易 crude oil futures。
```

而是说：

```text
可以设计一个事件研究，检验 inventory surprise 与 futures curve、calendar spread、energy equity、commodity FX 的后续关系。
```

这种边界非常重要。

公开可展示的是：

```text
research process
data schema
validation logic
bias checklist
evaluation framework
```

不公开的是：

```text
未脱敏 alpha
实盘参数
交易阈值
真实仓位
客户或内部信息
```

## 总体流程

Event-to-Signal 可以设计成十步：

```text
1. Event Intake
2. Event Normalization
3. Asset Mapping
4. Hypothesis Generation
5. Feature Design
6. Label / Outcome Design
7. Validation
8. Bias Diagnosis
9. Forecast Ledger Update
10. Human Review and Next Plan
```

完整链路：

```text
daily brief
  -> structured events
  -> candidate hypotheses
  -> feature definitions
  -> event study / backtest
  -> diagnostic report
  -> forecast ledger
  -> next research plan
```

它的目标不是一次性找到神奇因子。
目标是建立一个持续产生、验证、淘汰、沉淀假设的研究机器。

## Step 1: Event Intake

事件来源可以来自 `FICC004` 的 daily brief：

```text
macro calendar
central bank text
market snapshot
rates move
FX move
commodity inventory report
geopolitical event
cross-asset narrative
human research note
```

第一版事件输入可以很简单：

```text
events.md
market_snapshot.csv
macro_calendar.csv
daily_brief.md
```

重点是每个事件都要有结构。

事件模板：

```text
Event:
  date:
  event_type:
  region:
  asset_class:
  entities:
  description:
  expected:
  actual:
  surprise:
  immediate_market_reaction:
  source:
  confidence:
```

事件不是新闻标题。
事件是可计算研究对象。

## Step 2: Event Normalization

不同事件要变成统一 schema。

基础字段：

```text
event_id
date
timestamp
event_type
region
asset_class
primary_asset
related_assets
macro_variable
surprise_direction
surprise_magnitude
source
source_quality
text_summary
```

示例：

```json
{
  "event_id": "evt_20260703_us_cpi",
  "date": "2026-07-03",
  "event_type": "macro_surprise",
  "region": "US",
  "asset_class": "rates_fx",
  "primary_asset": "US2Y",
  "related_assets": ["US10Y", "DXY", "EURUSD", "Gold"],
  "macro_variable": "CPI",
  "surprise_direction": "upside",
  "surprise_magnitude": "medium",
  "source": "public_macro_calendar",
  "source_quality": "official_or_public",
  "text_summary": "US CPI came above consensus."
}
```

第一版不必追求完美。
但必须统一。

否则后面无法做统计、检索和复盘。

## Step 3: Asset Mapping

事件要映射到资产。

FICC 里常见映射：

| Event Type | Primary Assets | Secondary Assets |
|---|---|---|
| inflation surprise | front-end rates, real yields | USD, gold, credit |
| central bank hawkish signal | rates, curve, FX | credit, equities, commodities |
| growth downside surprise | long-end rates, credit spreads | FX, commodities |
| oil inventory shock | crude futures, time spreads | inflation breakevens, CAD, NOK |
| risk-off shock | credit spreads, USD, JPY, CHF | yields, gold, EM FX |
| USD funding stress | FX basis, USD, EM FX | credit, rates, commodities |

映射不是固定答案。
它是候选链条。

例如：

```text
oil supply shock
  -> Brent futures
  -> Brent curve
  -> inflation expectation
  -> rates
  -> commodity FX
  -> credit sectors
```

系统要输出：

```text
primary affected assets
secondary affected assets
expected mechanism
uncertainty
```

## Step 4: Hypothesis Generation

事件映射后，开始生成假设。

好的 hypothesis 必须具备五个条件：

```text
specific
testable
time-bounded
data-backed
falsifiable
```

坏假设：

```text
oil inventory matters for oil.
```

好假设：

```text
Unexpected crude inventory draws are associated with stronger front-month Brent calendar spreads over the next 1-5 trading days, especially when baseline inventory level is below its seasonal average.
```

坏假设：

```text
hawkish central banks make currency stronger.
```

好假设：

```text
Hawkish central bank surprises are associated with short-horizon appreciation of the local currency when rate differential repricing is confirmed by front-end yield moves and broad risk sentiment is not sharply risk-off.
```

这就是从 intuition 到 research hypothesis。

## Hypothesis 模板

每个假设记录成：

```text
Hypothesis:
  id:
  date_created:
  event_type:
  asset_universe:
  feature_definition:
  outcome_definition:
  horizon:
  expected_relationship:
  mechanism:
  evidence:
  counter_evidence:
  data_required:
  risks:
  status:
```

示例：

```text
Hypothesis:
  event_type: oil_inventory_surprise
  asset_universe: crude futures curve
  feature_definition: inventory surprise relative to consensus or seasonal baseline
  outcome_definition: change in front-month calendar spread
  horizon: 1d, 3d, 5d
  expected_relationship: inventory draw -> spread strength
  mechanism: lower available supply increases scarcity value
  counter_evidence: demand shock, macro risk-off, OPEC headline
  status: candidate
```

## Step 5: Feature Design

假设要变成可计算 feature。

Feature 设计要回答：

```text
这个事件如何数值化？
这个变量在事件发生前能不能知道？
这个变量是否需要标准化？
这个变量是否跨资产可比？
这个变量是否会引入未来信息？
```

常见 feature：

```text
surprise_direction
surprise_magnitude
z_score
event_dummy
pre_event_trend
post_event_reaction
rate_differential_change
curve_slope_change
inventory_deviation
volatility_regime
risk_sentiment_regime
```

FICC feature 可以分层：

```text
event features:
  surprise, event type, source, region

market state features:
  trend, volatility, liquidity, positioning proxy

macro regime features:
  inflation regime, growth regime, central bank cycle

cross-asset confirmation features:
  rates confirmation, FX confirmation, commodity confirmation, credit confirmation
```

Feature 不是越多越好。
第一版要少而清楚。

## Step 6: Outcome Design

Outcome 是你要验证的东西。

常见 outcome：

```text
next-day return
3-day return
5-day return
curve slope change
spread change
volatility change
drawdown
hit rate
information coefficient
event-window abnormal return
```

但 FICC 里不一定只看 return。

也可以看：

```text
yield change
spread widening / tightening
curve steepening / flattening
forward point change
basis change
calendar spread change
inventory-adjusted curve move
```

Outcome 设计最重要的是避免模糊。

坏 outcome：

```text
market reacts.
```

好 outcome：

```text
US2Y yield change from event close to next trading day close.
```

或者：

```text
Brent M1-M2 calendar spread change over 1, 3, and 5 trading days after inventory event.
```

## Step 7: Validation

验证不是直接跑一个回测就完了。

先做 event study。

Event study 问：

```text
事件发生前后，目标资产是否出现稳定的平均反应？
这个反应是否集中在某些 regime？
反应是否被少数极端样本驱动？
不同 horizon 是否一致？
```

基础验证输出：

```text
sample size
mean outcome
median outcome
hit rate
distribution
t-stat or bootstrap confidence
regime split
outlier analysis
transaction-cost awareness
```

第一版 validation report：

```text
Hypothesis:
Sample:
Feature:
Outcome:
Horizon:
Result:
Regime dependency:
Counterexamples:
Bias risks:
Verdict:
Next step:
```

这比单纯贴一张收益曲线更有研究价值。

## Step 8: Bias Diagnosis

FICC event-to-signal 最容易犯的错误是偏差。

必须检查：

```text
look-ahead bias
survivorship bias
selection bias
data snooping
overfitting
multiple testing
publication bias
regime instability
timestamp mismatch
liquidity and transaction cost
calendar effect
revised data problem
```

尤其是宏观数据：

```text
很多 macro data 会 revision。
研究时要区分 real-time vintage 和 revised data。
```

尤其是事件时间：

```text
事件发布时间必须早于用于计算 outcome 的交易窗口。
```

尤其是新闻：

```text
news timestamp 和 market reaction timestamp 必须严格对齐。
```

偏差诊断不做，结果再漂亮都不可信。

## Step 9: Forecast Ledger Update

每个 hypothesis 都要进入 ledger。

Ledger 不只是记录预测。
它记录研究生命史：

```text
created
tested
diagnosed
accepted
rejected
paused
revisited
```

字段：

```text
hypothesis_id
created_at
source_event_id
feature_version
outcome_version
test_period
sample_size
result_summary
bias_diagnosis
human_review
status
next_action
```

状态可以是：

```text
candidate
testing
needs_data
weak_evidence
promising
rejected
monitor_only
production_forbidden_public_demo
```

这里的 `production_forbidden_public_demo` 表示：

```text
只能作为公开教育 demo，不允许当作真实交易系统。
```

## Step 10: Human Review

Human review 要审三件事：

```text
research validity
financial interpretation
public safety
```

Checklist：

```text
Hypothesis specific enough?
Feature available before outcome?
Timestamp correct?
Sample size enough?
Outlier checked?
Regime dependency checked?
Counterexamples included?
Transaction cost considered?
No proprietary alpha leaked?
No trading advice language?
Next action clear?
```

Human review 的角色不是给 AI 打分。
而是做研究质量控制。

```text
AI proposes.
System tests.
Human reviews.
Ledger remembers.
Next round improves.
```

## 系统架构

Event-to-Signal Workflow 可以设计成八层：

```text
Layer 1: Daily Brief Input
Layer 2: Event Store
Layer 3: Hypothesis Generator
Layer 4: Feature Builder
Layer 5: Outcome Builder
Layer 6: Validation Engine
Layer 7: Bias Diagnostic Engine
Layer 8: Ledger and Review
```

文件结构：

```text
ficc_event_to_signal/
  data/
    events.csv
    market_snapshot.csv
    macro_calendar.csv
  hypotheses/
    hypothesis_registry.yaml
  features/
    feature_specs.yaml
  outcomes/
    outcome_specs.yaml
  notebooks/
    event_study_template.ipynb
  reports/
    validation_report_YYYY-MM-DD.md
  ledger/
    forecast_ledger.csv
    research_ledger.csv
  src/
    event_store.py
    hypothesis_generator.py
    feature_builder.py
    outcome_builder.py
    validator.py
    bias_diagnostics.py
    report_writer.py
```

第一版可以完全本地化。
不用复杂平台。

## Agent 分工

可以拆成多个 agent：

```text
Event Agent:
  从 daily brief 里抽取结构化事件。

Hypothesis Agent:
  生成 testable hypothesis。

Feature Agent:
  把 hypothesis 翻译成 feature spec。

Validation Agent:
  执行 event study / simple backtest。

Skeptic Agent:
  检查 bias、反例、样本不足、过拟合。

Report Agent:
  输出 validation report。

Human PM:
  审核 public-safe 边界和研究结论。
```

关键是所有 agent 都要被 harness 约束。

```text
fixed schema
fixed allowed data
no live order
no hidden alpha
evidence required
counter-evidence required
human review required
```

## Harness 规则

FICC Event-to-Signal Harness 应该规定：

```text
Allowed:
  public data
  educational examples
  sanitized notes
  synthetic sample data
  local research ledger

Not allowed:
  client data
  confidential bank data
  proprietary strategy parameters
  live positions
  order instructions
  undisclosed alpha details
```

输出规则：

```text
Every signal candidate must include:
  hypothesis
  feature
  outcome
  horizon
  evidence
  counter-evidence
  bias diagnosis
  status
  next action
```

禁止输出：

```text
buy / sell / hold recommendation
position size
stop loss
take profit
execution instruction
unreviewed live signal
```

这让系统可以公开展示，同时不泄露真实 alpha。

## Signal Card 模板

每个 signal candidate 可以写成一张 card。

```text
Signal Card

ID:
Name:
Created Date:
Source Event:
Asset Class:
Asset Universe:

Research Hypothesis:

Mechanism:

Feature Definition:

Outcome Definition:

Horizon:

Data Required:

Validation Result:

Bias Diagnosis:

Counter-Evidence:

Human Review:

Status:

Next Action:
```

这张 card 可以进入 private research ledger。
公开版本只展示脱敏结构。

## 示例 1: CPI Surprise

事件：

```text
US CPI upside surprise
```

候选假设：

```text
When US CPI surprises to the upside, front-end Treasury yields tend to rise over the short horizon if the surprise is interpreted as policy-relevant and not dominated by growth shock.
```

Feature：

```text
CPI surprise direction
CPI surprise magnitude
pre-event inflation regime
Fed pricing change
```

Outcome：

```text
US2Y yield change over 1d / 3d
DXY change over 1d / 3d
gold change over 1d / 3d
```

Counter-evidence：

```text
If market prices growth scare more strongly, long-end yields may fall.
If CPI components are viewed as one-off, reaction may fade.
If Fed communication offsets the data, rates reaction may reverse.
```

Status：

```text
candidate for event study
```

## 示例 2: Oil Inventory Shock

事件：

```text
crude inventory draw larger than expected
```

候选假设：

```text
Large unexpected crude inventory draws are associated with short-term strengthening of front-month crude calendar spreads, especially when inventories are below seasonal norms.
```

Feature：

```text
inventory surprise
seasonal inventory deviation
front spread pre-event level
oil volatility regime
```

Outcome：

```text
front calendar spread change over 1d / 3d / 5d
Brent or WTI futures return over 1d / 3d
```

Counter-evidence：

```text
inventory draw may be refinery-driven rather than demand-driven.
macro risk-off can dominate commodity-specific signal.
OPEC headlines can overwhelm inventory report.
```

Status：

```text
candidate for commodity event study
```

## 示例 3: Hawkish Central Bank Surprise

事件：

```text
central bank statement more hawkish than expected
```

候选假设：

```text
Hawkish central bank surprises are associated with local currency appreciation when front-end rates confirm policy repricing and global risk sentiment remains stable.
```

Feature：

```text
hawkish text score
front-end rate change
rate differential change
risk sentiment proxy
```

Outcome：

```text
local currency spot return vs USD
forward point change
FX volatility change
```

Counter-evidence：

```text
Currency may weaken if hawkishness is interpreted as policy mistake.
Risk-off can dominate rate differential.
Market may have priced the hawkish shift before the statement.
```

Status：

```text
candidate for FX event study
```

## Backtest 不是终点

很多人会直接跳到 backtest。
但在这个 workflow 里，backtest 只是验证工具之一。

研究顺序应该是：

```text
understand mechanism
define event
define feature
define outcome
run event study
diagnose bias
then consider backtest
```

如果没有机制，只靠搜索历史相关性，很容易变成：

```text
data mining
overfitting
false discovery
```

所以我们要坚持：

```text
mechanism first
test second
```

## Bias Report 模板

每次验证都要输出 bias report。

```text
Bias Diagnostic Report

Hypothesis ID:
Data Window:
Sample Size:

Look-ahead Bias:
  pass / fail / uncertain

Timestamp Alignment:
  pass / fail / uncertain

Survivorship Bias:
  pass / fail / uncertain

Data Revision Risk:
  pass / fail / uncertain

Selection Bias:
  pass / fail / uncertain

Multiple Testing Risk:
  low / medium / high

Outlier Dependency:
  low / medium / high

Regime Dependency:
  low / medium / high

Transaction Cost Sensitivity:
  low / medium / high

Public Safety:
  pass / fail / needs redaction

Reviewer Notes:
```

这会让研究更像专业流程，而不是随手跑图。

## Evaluation

Event-to-Signal 系统要评估的不是收益率。
至少公开阶段不是。

公开阶段评估：

```text
hypothesis quality
schema completeness
evidence grounding
bias diagnosis quality
reproducibility
human review pass rate
research iteration speed
forecast calibration
```

评分表：

| Metric | Question |
|---|---|
| Specificity | 假设是否具体 |
| Testability | 是否可计算验证 |
| Falsifiability | 是否能被证伪 |
| Grounding | 是否有事件和机制支持 |
| Bias Control | 是否检查关键偏差 |
| Reproducibility | 是否能复现 |
| Public Safety | 是否没有泄露 alpha |
| Next Action | 是否明确下一步 |

最终目标是提高研究质量。

## 和 FICC004 的连接

`FICC004` 每天输出：

```text
daily brief
events
cross-asset chains
claims
watchlist
forecast ledger updates
```

`FICC005` 接住这些输出：

```text
events -> hypotheses
claims -> feature specs
watchlist -> research queue
forecast ledger -> calibration
```

两者关系：

```text
FICC004 = daily research memory
FICC005 = hypothesis and validation engine
```

## 和 RD-Agent 的连接

这篇本质上就是一个 FICC 版本的 R&D agent workflow。

对应关系：

```text
自动提出因子假设:
  Hypothesis Agent

自动实现:
  Feature Builder / Outcome Builder

自动回测:
  Validation Engine

自动诊断偏差:
  Bias Diagnostic Engine / Skeptic Agent

自动生成下一轮研究计划:
  Research Ledger / Next Action

人类 PM 审核:
  Human Review
```

这和我们之前设想的 Quant Research OS 完全对齐。

## 和 Credit OS 的连接

公开展示时，这个项目能体现：

```text
domain understanding
research rigor
AI workflow design
quant research process
engineering structure
risk control mindset
public-safe communication
```

它不是展示“我有一个神奇 alpha”。
它展示的是：

```text
我知道如何把市场问题拆成研究问题。
我知道如何验证假设。
我知道如何防止自己被漂亮结果骗。
我知道如何把 AI agent 放进可审计的研究流程。
```

这就是很强的 credit。

## MVP 怎么做

第一版 MVP 可以非常直接：

```text
Input:
  daily_brief.md
  events.csv
  market_snapshot.csv

Process:
  extract events
  generate 3 hypothesis cards
  create feature specs
  create outcome specs
  run placeholder validation or small public-data event study
  generate bias report
  update research ledger

Output:
  signal_cards.md
  validation_report.md
  research_ledger.csv
```

MVP 成功标准：

```text
能从一篇 daily brief 生成结构化 hypothesis。
每个 hypothesis 都有 feature、outcome、horizon、evidence、counter-evidence。
每个 validation report 都包含 bias diagnosis。
所有输出都是 public-safe。
```

## Pseudocode

第一版流程：

```python
def run_event_to_signal(date: str):
    brief = load_daily_brief(date)
    events = extract_structured_events(brief)

    hypotheses = []
    for event in events:
        candidates = generate_hypotheses(event)
        hypotheses.extend(candidates)

    signal_cards = []
    for hypothesis in hypotheses:
        feature_spec = design_feature(hypothesis)
        outcome_spec = design_outcome(hypothesis)
        validation = validate_hypothesis(feature_spec, outcome_spec)
        bias_report = diagnose_bias(feature_spec, outcome_spec, validation)

        card = build_signal_card(
            hypothesis=hypothesis,
            feature_spec=feature_spec,
            outcome_spec=outcome_spec,
            validation=validation,
            bias_report=bias_report,
        )
        signal_cards.append(card)

    reviewed_cards = human_review(signal_cards)
    update_research_ledger(reviewed_cards)
    write_report(reviewed_cards)
```

关键要求：

```text
Every hypothesis must be traceable to an event.
Every feature must be available before the outcome window.
Every result must include a bias report.
Every public output must pass redaction.
```

## 面试怎么讲

如果被问：

```text
你说的 Event-to-Signal Workflow 是什么？
```

可以回答：

```text
它是把 FICC daily brief 里的结构化事件转化为可验证研究假设的流程。系统先抽取 macro、rates、FX、commodities 事件，再映射到相关资产，生成 testable hypothesis；然后定义 feature 和 outcome，做 event study 或 backtest，输出 validation report 和 bias diagnostic report，最后由 human reviewer 决定进入下一轮研究、搁置还是淘汰。它不是自动交易系统，而是研究假设生成和验证系统。
```

如果被问：

```text
你怎么避免数据挖掘？
```

可以回答：

```text
我会坚持 mechanism first、test second。每个 hypothesis 必须来自明确事件和经济机制；每个 feature 必须确认在 outcome window 之前可获得；验证时检查 look-ahead、timestamp alignment、selection bias、multiple testing、outlier dependency、regime dependency 和 transaction cost sensitivity。最后用 forecast ledger 记录结果，避免只记住成功案例。
```

如果被问：

```text
AI agent 在里面具体做什么？
```

可以回答：

```text
Agent 负责结构化和加速研究流程：Event Agent 抽取事件，Hypothesis Agent 生成可测试假设，Feature Agent 定义变量，Validation Agent 跑测试，Skeptic Agent 做偏差诊断，Report Agent 写报告。但所有 agent 都受 schema、allowed data、evidence requirement 和 human review 约束，不允许直接输出交易建议。
```

## 下一步

`FICC005` 完成后，下一篇可以进入：

```text
FICC006:
  FICC RAG / Graph RAG System

FICC007:
  FICC Forecast Ledger and Evaluation

FICC008:
  FICC Interview Playbook
```

如果按 Research OS 的逻辑，下一篇最自然是：

```text
FICC006 -> FICC RAG / Graph RAG System
```

因为 `FICC004` 和 `FICC005` 都需要记忆层和证据层。

## 当前结论

FICC005 的核心是：

```text
event
  -> hypothesis
  -> feature
  -> outcome
  -> validation
  -> bias diagnosis
  -> ledger
  -> human review
  -> next research plan
```

它把 daily brief 从“研究记录”升级成“研究发动机”。

真正重要的不是一次性找到某个信号。
真正重要的是建立一个系统：

```text
持续提出假设。
持续验证假设。
持续诊断偏差。
持续沉淀研究记忆。
持续生成下一轮计划。
```

一句话收束：

```text
Event-to-Signal Workflow turns FICC market events into auditable research hypotheses.
```

中文：

```text
Event-to-Signal Workflow 把 FICC 市场事件变成可审计、可验证、可复盘的研究假设。
```
