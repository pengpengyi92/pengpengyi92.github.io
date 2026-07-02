---
title: "FICC008: FICC AI Agent Harness - 从 FICC001-FICC007 到可控研究 Agent 系统"
date: 2026-07-03 03:00:00 +0800
categories: [Learning, Finance]
tags: [ficc008, ficc, ai-agent-harness, agent-workflow, research-os, rag, graph-rag, quant-validation, human-review, public-safe]
---

这是 `PENGYI_FICC_MAP` 的 `FICC008`。

前面的系列已经走到这里：

```text
FICC000 -> FICC 总地图
FICC001 -> Fixed Income / Rates / Credit
FICC002 -> Currencies / FX
FICC003 -> Commodities
FICC004 -> Daily FICC Brief Generator
FICC005 -> Event-to-Signal Workflow
FICC006 -> FICC x AI Research OS
FICC007 -> FICC 系列总复盘
FICC008 -> FICC AI Agent Harness
```

这一篇的主题是：

```text
AI Agent Harness
```

也就是：

```text
如何把 FICC research agent 放进一套可控、可审计、可复盘、可扩展、public-safe 的运行框架里。
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
这里讨论的是 research agent harness，不是自动交易系统。
```

## 一句话总览

FICC AI Agent Harness 的核心是：

```text
用明确的任务边界、数据边界、工具权限、输出 schema、证据要求、偏差检查、人类审核和日志记录，把 AI agent 从“会生成内容”约束成“能可靠完成研究流程”。
```

更工程化一点：

```text
Harness = task contract + data policy + tool policy + memory policy + output schema + validation rules + review rules + audit logs.
```

它解决的问题不是：

```text
让 agent 更会聊天。
```

而是：

```text
让 agent 在真实研究流程中可控地工作。
```

## 为什么 FICC 需要 Harness

FICC 是一个高风险研究场景。

它有几个天然风险：

```text
market data can be noisy
macro events are ambiguous
cross-asset causality is hard
timestamps matter
revised data can mislead
public/private boundary matters
financial language can become advice
```

如果没有 harness，agent 很容易出问题：

```text
1. 把相关性说成因果。
2. 忽略反例。
3. 引用不存在或不可靠来源。
4. 误读货币对方向。
5. 把未验证 hypothesis 写成结论。
6. 把 research note 写成 trading recommendation。
7. 泄露不该公开的 alpha、参数或内部信息。
8. 生成漂亮但不可复盘的文字。
```

所以在 FICC 场景里，harness 不是附加功能。

```text
Harness is the control plane of the research agent.
```

中文：

```text
Harness 是研究 agent 的控制平面。
```

## FICC001-FICC007 如何进入 Harness

前面每篇文章都对应 harness 里的一个部分。

| 文章 | 作用 | 在 Harness 里的位置 |
|---|---|---|
| FICC001 | Fixed Income 知识 | Rates / Credit Agent 的 domain rule |
| FICC002 | FX 知识 | FX Agent 的 domain rule |
| FICC003 | Commodities 知识 | Commodities Agent 的 domain rule |
| FICC004 | Daily Brief | Daily Brief Workflow |
| FICC005 | Event-to-Signal | Hypothesis / Validation Workflow |
| FICC006 | AI Research OS | 总系统架构 |
| FICC007 | Series Summary | Study map / capability map |
| FICC008 | Agent Harness | 控制层、规则层、审计层 |

也就是说：

```text
FICC001-FICC003:
  teach agents what the domain is

FICC004-FICC005:
  teach agents what the workflow is

FICC006-FICC007:
  teach humans what the OS and map are

FICC008:
  defines how agents are allowed to operate
```

## Harness 的核心对象

FICC AI Agent Harness 里至少有八个核心对象。

```text
Task
Input
Tool
Memory
Agent
Output
Validation
Review
```

它们的关系：

```text
Task defines what to do.
Input defines what can be used.
Tool defines what can be called.
Memory defines what can be retrieved.
Agent performs bounded work.
Output must match schema.
Validation checks quality and safety.
Review approves or rejects.
```

如果没有这些对象，agent workflow 就会变成自由发挥。

FICC 场景不能自由发挥。

## Task Contract

每个 agent 任务都必须有 task contract。

模板：

```text
Task Contract:
  task_id:
  task_name:
  objective:
  allowed_inputs:
  forbidden_inputs:
  allowed_tools:
  forbidden_outputs:
  output_schema:
  validation_rules:
  human_review_required:
  logging_required:
```

例子：

```text
Task:
  Generate Daily FICC Brief

Objective:
  Summarize public-safe FI / FX / Commodities market context and produce evidence-grounded daily research brief.

Forbidden:
  No direct trading advice.
  No position sizing.
  No client-specific recommendation.
  No proprietary alpha.
```

这让 agent 明确知道：

```text
做什么
用什么
不能用什么
输出什么格式
由谁审核
```

## Data Policy

金融 agent 的第一条规则是数据边界。

允许：

```text
public market summaries
official reports
public macro data
sanitized notes
synthetic examples
local public-safe learning notes
previous public-safe daily briefs
```

禁止：

```text
client data
internal bank documents
unredacted alpha notes
live positions
private order flow
confidential research
non-public material information
```

数据要分级：

```text
L0:
  public educational notes

L1:
  public official data and reports

L2:
  sanitized personal research notes

L3:
  private research ledger

L4:
  confidential / forbidden for public agent
```

公开网站只允许：

```text
L0-L2
```

私有 research repo 可以包含：

```text
L3
```

但 `L4` 不应该进入这套公开 harness。

## Tool Policy

工具权限必须细分。

不同 agent 可以用不同工具。

| Agent | Allowed Tools | Forbidden Tools |
|---|---|---|
| Daily Brief Agent | public notes, approved RAG, local CSV | trading API, private alpha DB |
| Event Agent | daily brief, event store | live order system |
| Hypothesis Agent | event store, knowledge base | client info |
| Quant Agent | public sample data, synthetic data, validation engine | live strategy deployment |
| Skeptic Agent | validation report, bias checklist | none beyond review scope |
| Report Agent | approved outputs | unredacted private notes |

工具权限的原则：

```text
least privilege
```

也就是：

```text
agent 只拿完成任务所需的最小权限。
```

## Memory Policy

FICC Research OS 需要记忆。
但 memory 也要受控。

Memory 可以分为：

```text
Knowledge Memory:
  FICC000-FICC008 notes

Daily Memory:
  Daily briefs and market snapshots

Event Memory:
  structured event store

Research Memory:
  hypotheses, signal cards, validation reports

Review Memory:
  human review comments and corrections
```

检索必须带 metadata：

```text
source
date
asset_class
topic
public_safety_level
confidence
```

Agent 不应该直接相信 memory。
它应该输出：

```text
retrieved evidence
counter-evidence
source ids
uncertainty
```

这就是 evidence-grounded generation。

## Output Schema

Harness 的关键是输出必须结构化。

不要只让 agent 输出自由文本。

Daily Brief 输出：

```text
date
market_regime
main_narrative
fixed_income_summary
fx_summary
commodities_summary
cross_asset_chain
evidence
counter_evidence
watchlist
human_review_status
```

Hypothesis 输出：

```text
hypothesis_id
source_event
asset_universe
mechanism
feature_definition
outcome_definition
horizon
evidence
counter_evidence
bias_risks
status
```

Validation 输出：

```text
validation_id
hypothesis_id
sample_size
method
result_summary
regime_dependency
outlier_dependency
bias_diagnosis
verdict
next_action
```

Report 输出：

```text
title
summary
public_safety_check
redaction_status
source_links
review_notes
```

结构化输出的意义是：

```text
可以验证
可以复盘
可以检索
可以进入 ledger
可以被下一轮 agent 使用
```

## Validation Rules

每个 agent 输出都要被验证。

通用规则：

```text
Every claim needs evidence.
Every claim needs counter-evidence.
Every hypothesis needs horizon.
Every feature needs timestamp check.
Every validation needs bias diagnosis.
Every public artifact needs redaction check.
```

FICC 特有规则：

```text
FX pair direction must be checked.
Rate move unit must be checked.
Commodity contract and tenor must be checked.
Macro release timestamp must be checked.
Revised data risk must be flagged.
Cross-asset causal chain must include uncertainty.
```

禁止输出：

```text
buy / sell / hold
position size
stop loss
take profit
live signal
client-specific advice
unreviewed actionable recommendation
```

这不是限制能力。
这是让系统可公开、可合作、可长期运行。

## Human Review

Human Review 是 harness 的最后一道质量门。

Review 要看四类问题：

```text
Domain:
  金融解释是否合理？

Evidence:
  证据是否支持结论？

Bias:
  是否存在 look-ahead、selection、timestamp、regime 等问题？

Safety:
  是否越过公开边界？
```

Review checklist：

```text
Source checked?
Timestamp checked?
Asset direction checked?
Evidence included?
Counter-evidence included?
Confidence calibrated?
Bias risks diagnosed?
No trading advice?
No private data?
Next action clear?
```

Human review 不是降低自动化。

它是：

```text
把 agent output 变成 research asset 的必要步骤。
```

## Harness 总架构

整体架构可以这样看：

```text
User / Researcher
  -> Task Contract
  -> Data Policy
  -> Tool Policy
  -> Memory Retrieval
  -> Agent Execution
  -> Output Schema
  -> Validation Rules
  -> Human Review
  -> Ledger Update
  -> Public-safe Artifact
```

每一次 agent run 都要留下：

```text
input snapshot
retrieved evidence
tool calls
intermediate outputs
final output
validation result
review decision
ledger update
```

这就是 audit trail。

没有 audit trail，agent 研究无法复盘。

## Agent 角色设计

FICC Harness 里可以有三类 agent。

第一类：Domain Agent。

```text
Rates Agent
Credit Agent
FX Agent
Commodities Agent
Macro Agent
```

第二类：Workflow Agent。

```text
Daily Brief Agent
Event Extraction Agent
Hypothesis Agent
Feature Builder Agent
Validation Agent
Report Agent
```

第三类：Control Agent。

```text
RAG Agent
Graph Agent
Skeptic Agent
Redaction Agent
Reviewer Assistant
Ledger Agent
```

分工：

```text
Domain Agent knows the market.
Workflow Agent performs the task.
Control Agent checks evidence, bias, safety, and memory.
```

中文：

```text
Domain Agent 管领域。
Workflow Agent 管执行。
Control Agent 管证据、偏差、安全和记忆。
```

## Daily Brief Harness

`FICC004` 对应 Daily Brief Harness。

任务：

```text
Generate daily public-safe FICC brief.
```

输入：

```text
market snapshot
macro calendar
public event notes
RAG evidence pack
previous forecast ledger
```

输出：

```text
daily brief markdown
daily brief JSON
cross-asset chain
watchlist
forecast ledger update
```

验证：

```text
FI / FX / Commodities all covered?
Evidence included?
Counter-evidence included?
No trading advice?
Human review status present?
```

这个 harness 的目标是：

```text
把每日市场信息变成结构化研究记忆。
```

## Event-to-Signal Harness

`FICC005` 对应 Event-to-Signal Harness。

任务：

```text
Turn structured events into testable research hypotheses.
```

输入：

```text
daily brief
event store
asset mapping
RAG evidence
graph context
```

输出：

```text
signal cards
feature specs
outcome specs
validation plan
bias checklist
```

验证：

```text
Hypothesis specific?
Feature available before outcome?
Horizon defined?
Counter-evidence present?
Bias risks listed?
No live signal?
```

这个 harness 的目标是：

```text
把事件变成可验证、可审计、可复盘的研究假设。
```

## Quant Validation Harness

Quant Agent 不能只输出一张好看的图。

Quant Validation Harness 要求：

```text
feature definition
outcome definition
sample period
sample size
event window
regime split
outlier analysis
bias diagnosis
result summary
research verdict
```

Bias 必查：

```text
look-ahead bias
timestamp mismatch
survivorship bias
selection bias
data snooping
multiple testing
revised data risk
transaction cost sensitivity
regime dependency
```

输出 verdict：

```text
rejected
weak evidence
monitor only
promising for further study
needs better data
public demo only
private research only
```

这让 quant workflow 从“跑结果”变成“研究判断”。

## RAG Harness

RAG Agent 不能随便检索、随便拼接。

RAG Harness 要求：

```text
query
retrieval scope
metadata filter
top-k results
source ids
evidence summary
counter-evidence summary
uncertainty
```

RAG 输出不应该是：

```text
我认为市场因为 X 变化。
```

RAG 输出应该是：

```text
Evidence pack:
  supporting evidence
  counter-evidence
  historical analogies
  source ids
  confidence
```

这样后面的 agent 才能基于证据工作。

## Graph Harness

Graph Agent 负责关系。

输入：

```text
event
asset
macro variable
retrieved evidence
```

输出：

```text
nodes
edges
causal_chain_candidates
uncertainty
conflicting_paths
```

例子：

```text
oil supply shock
  -> oil price
  -> inflation expectation
  -> central bank reaction
  -> rates
  -> FX
  -> credit condition
```

Graph Harness 的规则：

```text
Every edge must have relation type.
Every causal chain must include uncertainty.
Every cross-asset claim must include evidence.
Conflicting paths must be preserved.
```

这能避免 agent 把复杂市场写成单一线性故事。

## Skeptic Harness

Skeptic Agent 是非常关键的控制层。

它的任务不是生成新观点。
它的任务是攻击当前观点。

检查：

```text
Does evidence really support the claim?
Could there be another explanation?
Is the sample too small?
Is the event timestamp correct?
Is the conclusion overconfident?
Is this public-safe?
Is this actually trading advice?
```

输出：

```text
failure_modes
counter_evidence
missing_checks
overconfidence_flags
redaction_flags
recommended_revision
```

在 FICC 里，Skeptic Agent 很重要，因为市场叙事很容易变成漂亮故事。

Harness 要强制：

```text
No final report without skeptic pass.
```

## Redaction Harness

公开网站需要 redaction。

Redaction Agent 检查：

```text
proprietary factor name
parameter values
live signal language
position sizing
client mention
internal source
confidential workflow
unapproved dataset
```

公开输出应该保留：

```text
concept
framework
schema
workflow
synthetic example
public-safe template
```

删除或替换：

```text
exact alpha
private data
production parameter
live trading instruction
client detail
```

这就是：

```text
public credit without alpha leakage
```

## Ledger Harness

Ledger Agent 负责让系统有记忆。

它记录：

```text
claim
hypothesis
validation result
bias report
human review
next action
```

每次 agent run 后，Ledger Agent 要问：

```text
What new claim was made?
What evidence supports it?
What counter-evidence challenges it?
Was it reviewed?
What is the next action?
Should this enter public memory or private memory?
```

Ledger 的价值：

```text
让系统不重复犯错。
让研究可以累积。
让 agent 有长期上下文。
让 human reviewer 能看到历史。
```

## Evaluation Harness

Harness 自身也要被评估。

评估维度：

```text
Task completion
Schema validity
Evidence grounding
Counter-evidence quality
Bias detection quality
Public-safety compliance
Human edit distance
Review pass rate
Ledger update quality
```

评分表：

| Metric | Question |
|---|---|
| Schema Validity | 输出是否符合结构 |
| Grounding | claim 是否有证据 |
| Skepticism | 是否主动找反例 |
| Bias Control | 是否检查关键偏差 |
| Safety | 是否 public-safe |
| Usefulness | 是否能推进研究 |
| Review Cost | 人类需要改多少 |

没有 evaluation，harness 只是规则文本。
有 evaluation，harness 才能持续进化。

## MVP 设计

第一版 FICC AI Agent Harness MVP 可以很小。

目录：

```text
ficc_agent_harness/
  policies/
    data_policy.yaml
    tool_policy.yaml
    output_policy.yaml
    public_safety_policy.yaml
  schemas/
    daily_brief.schema.json
    hypothesis.schema.json
    validation_report.schema.json
    review.schema.json
  agents/
    daily_brief_agent.yaml
    event_agent.yaml
    hypothesis_agent.yaml
    skeptic_agent.yaml
    report_agent.yaml
  memory/
    public_notes/
    daily_briefs/
    research_ledger.csv
  outputs/
    daily_brief.md
    signal_cards.md
    review_report.md
```

MVP 流程：

```text
1. 输入 sample FICC event。
2. RAG Agent 检索 FICC000-FICC008。
3. Daily Brief Agent 输出 brief。
4. Event Agent 抽取事件。
5. Hypothesis Agent 生成 signal card。
6. Skeptic Agent 检查反例和偏差。
7. Redaction Agent 检查公开边界。
8. Human Review 通过或退回。
9. Ledger Agent 写入 research ledger。
```

第一版成功标准：

```text
输出结构正确。
证据链清楚。
没有交易建议。
有反例。
有 bias checklist。
有 review status。
能写入 ledger。
```

## Harness 配置示例

一个简化配置：

```yaml
task_id: ficc_daily_brief_public
objective: Generate a public-safe daily FICC research brief.
allowed_inputs:
  - public_market_snapshot
  - public_macro_calendar
  - public_ficc_notes
  - sanitized_event_notes
forbidden_inputs:
  - client_data
  - live_positions
  - proprietary_alpha
allowed_outputs:
  - market_summary
  - evidence_pack
  - counter_evidence
  - watchlist
  - research_hypothesis
forbidden_outputs:
  - buy_sell_recommendation
  - position_size
  - execution_instruction
validation:
  evidence_required: true
  counter_evidence_required: true
  human_review_required: true
  redaction_required: true
```

这类配置以后可以进入真实工程。

## 和 DeepSeek / Codex / Claude Harness 的关系

如果从 DeepSeek PM 的角度看，这个 FICC Harness 很有启发。

一个好的 coding / research agent harness 不是只提供模型。
它要提供：

```text
workspace awareness
task planning
tool permission
file edit policy
evidence tracking
test execution
review gate
artifact generation
audit log
```

映射到 FICC：

```text
workspace awareness:
  知道当前 FICC 系列、learning.html、posts、ledger

task planning:
  知道当前要做 Daily Brief / Event-to-Signal / Research OS

tool permission:
  知道可用 public notes，不可用 private alpha

file edit policy:
  只改相关 posts 和 learning page

evidence tracking:
  每个 market claim 要有 evidence

test execution:
  fenced code check, schema check, validation check

review gate:
  human review before public release

artifact generation:
  markdown, json, report, ledger

audit log:
  git commit, source ids, review notes
```

这就是我们从 coding agent harness 学到的东西：

```text
agent 必须活在一个可控工作台里。
```

## 面试怎么讲

如果被问：

```text
你说的 FICC AI Agent Harness 是什么？
```

可以回答：

```text
它是 FICC AI Research OS 的控制层。前面我把 Fixed Income、FX、Commodities 的知识层搭好，又设计了 Daily Brief 和 Event-to-Signal workflow。Harness 负责把这些 workflow 放进可控框架里：定义任务边界、数据边界、工具权限、memory policy、output schema、validation rules、bias diagnosis、redaction check、human review 和 audit log。目标不是让 agent 自由生成市场观点，而是让 agent 在 public-safe、evidence-grounded、reviewable 的研究流程里工作。
```

如果被问：

```text
为什么不能直接让 agent 写 FICC research？
```

可以回答：

```text
因为 FICC 里因果链复杂，时间戳、数据修订、资产方向、公开边界都很重要。没有 harness，agent 很容易把相关性写成因果、忽略反例、误读 FX pair、输出未验证 hypothesis，甚至把研究笔记写成交易建议。所以必须用 harness 约束输入、工具、输出、验证和 human review。
```

如果被问：

```text
Harness 和普通 prompt 有什么区别？
```

可以回答：

```text
Prompt 只是一次性指令。Harness 是运行框架，包含 task contract、data policy、tool policy、memory policy、output schema、validation rules、review gate 和 audit log。Prompt 告诉模型说什么，harness 决定模型能用什么、必须输出什么、如何检查、如何复盘。
```

## 和 FICC007 的关系

`FICC007` 是总结地图。

它回答：

```text
我们前面做了什么？
这些内容如何组成 AI 金融研究系统？
```

`FICC008` 是控制框架。

它回答：

```text
如果真的让 agent 运行这个系统，如何约束、验证、审核和记录？
```

两者关系：

```text
FICC007 = map
FICC008 = control plane
```

## 当前结论

FICC008 的核心是：

```text
AI agent 不是越自由越好。
在 FICC research 这种复杂高风险场景里，agent 必须被 harness 约束。
```

Harness 要管：

```text
task
data
tool
memory
schema
evidence
bias
redaction
review
ledger
audit
```

这篇把前面 `FICC001-FICC007` 的知识、流程、系统、复盘统一到一个控制层里。

一句话收束：

```text
FICC AI Agent Harness turns a powerful research agent into a controlled, auditable, public-safe research system.
```

中文：

```text
FICC AI Agent Harness 把强大的研究 agent 变成可控、可审计、可复盘、可公开展示的研究系统。
```
