---
title: "PENGYI_HARNESS_MAP000: Harness 总览 - Agent / Research / Quant / Tool / Memory / Product 六类 Harness"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-harness-map, harness-map000, harness, agent-harness, research-harness, quant-harness, tool-harness, memory-harness, product-harness, research-os, quant-os]
---

这是一个新的系列：

```text
PENGYI_HARNESS_MAP
```

这一篇是：

```text
PENGYI_HARNESS_MAP000 -> Harness 总览
```

主题：

```text
Harness 总览：
Agent / Research / Quant / Tool / Memory / Product 六类 Harness
```

这一篇的目标不是拆某一个 repo。

目标是把我们前面在 HKUDS、LLMQuant、X2Strategy 里反复遇到的一个共同概念抽出来：

```text
Harness
```

也就是：

```text
让 AI 系统从“能回答”变成“能执行、能约束、能记录、能评估、能复现、能迭代”的外壳和测试台。
```

## 一句话定义

我现在对 harness 的定义是：

```text
Harness = intelligence around an LLM is made executable, observable, governable, and repeatable.
```

用中文说：

```text
Harness 是把 LLM 包成真正 agent / research worker / quant worker / product worker 的运行外壳。
```

LLM 本身提供的是：

```text
reasoning
language
planning
generalization
```

但真正工作需要：

```text
task spec
tool access
permission
state
memory
trace
artifact
evaluation
diagnosis
recovery
human review
```

这些不是模型权重里天然存在的。

这些是 harness 要提供的。

## 为什么现在必须单独做 Harness Map

我们之前已经看过很多项目。

HKUDS 里有：

```text
OpenHarness
AutoAgent
OpenSpace
FastAgent
Auto-Deep-Research
DeepResearch-Eval
MGP
CatchMe
SepLLM
ClawWork
Litewrite
```

LLMQuant 里有：

```text
data-mcp
skills
quant-mind
Magents
awesome-trading-agents
```

X2Strategy 给我们的启发是：

```text
strategy idea
  -> StrategySpec
  -> code
  -> backtest
  -> diagnosis
```

这些项目看起来方向不同。

但它们背后其实都在做一件事：

```text
把开放式智能放进一个可执行、可验证、可复用的结构。
```

这就是 harness。

如果不单独抽象 harness，我们会一直停留在：

```text
这个 repo 很酷。
那个 repo 也很酷。
```

但如果抽象成 harness，我们就能看到：

```text
哪些项目负责执行？
哪些项目负责评测？
哪些项目负责工具？
哪些项目负责记忆？
哪些项目负责产品工作台？
哪些项目负责量化实验？
```

这会直接服务我们的目标：

```text
Pengyi Research OS
Pengyi Quant Research OS
R&D Agent for Quant Research
AI Scientist workflow
```

## Harness 不是简单 Framework

先把边界说清楚。

```text
Framework:
  提供代码组织方式和 API。

Runtime:
  负责程序运行。

Benchmark:
  负责评测。

Sandbox:
  负责隔离环境。

Harness:
  把 task、runtime、tools、state、trace、artifact、evaluation、memory、human review 组织成闭环。
```

所以 harness 比 framework 更贴近“工作现场”。

一个 harness 通常包括：

```text
1. Task Contract
2. Tool Contract
3. Execution Runtime
4. Permission Boundary
5. State Store
6. Trace / Log
7. Artifact Protocol
8. Evaluation
9. Diagnosis
10. Memory Writeback
11. Human Review
12. Recovery / Retry
```

这就是为什么我说：

```text
Harness 是 AI system 的生产线外壳。
```

## 六类 Harness 总览

我把当前对我们最重要的 harness 分成六类：

```text
1. Agent Execution Harness
2. Research Task Harness
3. Quant / Trading Harness
4. Tool Harness
5. Memory Harness
6. Product / Workspace Harness
```

总表：

| Harness Type | 核心问题 | 代表项目 | 对 Pengyi 的作用 |
|---|---|---|---|
| Agent Execution Harness | agent 如何规划、执行、调工具、记录轨迹、恢复任务 | OpenHarness, FastAgent, AutoAgent | 底层 agent runtime |
| Research Task Harness | research report / paper / deep research 如何生成、评估、迭代 | Auto-Deep-Research, DeepResearch-Eval, AI-Researcher, DeepCode | AI scientist workflow |
| Quant / Trading Harness | 策略想法如何变成 spec、代码、回测、诊断、PM review | Magents, X2Strategy, Vibe-Trading, AI-Trader, LLMQuant skills | Quant R&D Agent |
| Tool Harness | 工具如何被发现、路由、授权、调用、验证 | CLI-Anything, AnyTool, data-mcp, MCP ecosystem | agent 的手和外部接口 |
| Memory Harness | 记忆如何写入、压缩、检索、治理、审计、撤回 | MGP, CatchMe, SepLLM, LightRAG, MiniRAG | Research OS memory layer |
| Product / Workspace Harness | agent 如何进入真实产品、团队、沟通、写作、产出 | AgentSpace, OpenSpace, ClawTeam, ClawWork, Litewrite, MoChat, VideoAgent, ViMax | 个人生产力和组织 interface |

后面逐类展开。

## 1. Agent Execution Harness

一句话：

```text
Agent Execution Harness = 让 agent 真的能执行任务的底层外壳。
```

它回答的问题是：

```text
agent 如何接收任务？
如何规划？
如何调用 shell / file / browser / MCP / GUI？
如何处理权限？
如何记录工具调用？
如何恢复失败？
如何管理上下文？
如何输出 artifact？
```

代表项目：

```text
OpenHarness
FastAgent
AutoAgent
OpenHarness ohmo
```

### OpenHarness

`OpenHarness` 给了最直接的定义：

```text
An Agent Harness is the complete infrastructure that wraps around an LLM to make it a functional agent.
```

它补的是：

```text
model loop
streaming tool-use
permissions
skills
plugins
MCP
memory
compaction
tasks
swarm
provider auth
TUI
personal agent app
```

对我们来说，`OpenHarness` 是：

```text
Pengyi Agent Harness Runtime 的底座参考。
```

### FastAgent

`FastAgent` 更像一个高层 execution engine。

它的结构是：

```text
HostAgent planning
GroundingAgent execution
EvalAgent verification
Kanban workflow
Shell / GUI / MCP / Web / System tools
Smart Tool RAG
memory compression
recording / audit trail
```

它告诉我们：

```text
一个真实 agent 不应该只有 planner。
它还要有 executor 和 evaluator。
```

这可以抽象成：

```text
Plan -> Act -> Verify -> Record -> Iterate
```

### AutoAgent

`AutoAgent` 的启发是：

```text
agent 不只是执行已有 workflow。
agent 也可以生成 workflow / tool / agent。
```

这意味着 harness 还需要支持：

```text
workflow creation
tool registration
agent instantiation
validation
reuse
```

所以 Agent Execution Harness 的最小结构是：

```text
TaskSpec
Planner
Executor
ToolRouter
PermissionManager
TraceRecorder
Evaluator
ArtifactStore
RecoveryPolicy
```

## 2. Research Task Harness

一句话：

```text
Research Task Harness = 让 research agent 生成、评估、修正研究产物的闭环。
```

它回答的问题是：

```text
research question 如何被拆解？
证据如何收集？
报告如何生成？
事实如何检查？
冗余如何诊断？
下一轮研究计划如何生成？
人类如何审核？
```

代表项目：

```text
Auto-Deep-Research
DeepResearch-Eval
AI-Researcher
DeepCode
DeepInnovator
Paper2Slides
Litewrite
```

### Auto-Deep-Research

`Auto-Deep-Research` 代表：

```text
deep research producer harness
```

它负责：

```text
question
  -> plan
  -> search
  -> read
  -> file surf
  -> code
  -> evidence
  -> synthesis
  -> report
```

这不是普通问答。

这是 research production harness。

### DeepResearch-Eval

`DeepResearch-Eval` 是：

```text
report-centric evaluation harness
```

它评估：

```text
quality
structure
coverage
redundancy
factuality
support
```

它对我们很关键，因为一个 AI scientist 不能只会生成报告。

它还要能回答：

```text
这个报告真的靠谱吗？
哪里重复？
哪里缺证据？
哪里事实不稳？
下一轮应该补什么？
```

### DeepCode

`DeepCode` 给的是：

```text
paper-to-code harness
```

也就是：

```text
paper method
  -> implementation plan
  -> code generation
  -> run
  -> debug
  -> compare with expected behavior
```

这对 AI scientist 非常核心。

因为研究最后一定要落到：

```text
可运行代码
可验证实验
可复现实证
```

Research Task Harness 的最小结构是：

```text
ResearchQuestion
EvidencePlan
SourceCollector
ReadingAgent
SynthesisAgent
ArtifactWriter
FactChecker
QualityJudge
GapDiagnoser
NextPlanGenerator
HumanReviewer
```

## 3. Quant / Trading Harness

一句话：

```text
Quant / Trading Harness = 让策略想法变成可执行、可回测、可诊断、可审核的研究对象。
```

它回答的问题是：

```text
因子想法如何结构化？
策略假设如何变成 spec？
代码如何生成？
回测如何执行？
交易成本如何处理？
leakage 如何诊断？
风险如何约束？
PM 如何审核？
```

代表项目：

```text
Magents
X2Strategy
Vibe-Trading
AI-Trader
LLMQuant skills
LLMQuant data-mcp
QuantMind
```

### Magents

`Magents` 是：

```text
multi-agent trading simulation / backtesting harness
```

它把策略研究从：

```text
signal -> return curve
```

推进到：

```text
market data event
  -> strategy pod
  -> signal agent
  -> execution agent
  -> order event
  -> fill event
  -> portfolio update
  -> risk validation
  -> performance report
```

这就是量化研究必须进入的现实层。

### X2Strategy

`X2Strategy` 的核心启发是：

```text
strategy idea
  -> StrategySpec
  -> code
  -> backtest
  -> diagnosis
```

它是：

```text
strategy compiler harness
```

也就是说，它把自然语言或论文里的策略想法，变成结构化 spec，再进入工程验证。

### Vibe-Trading / AI-Trader

`Vibe-Trading` 更像：

```text
agentic quant research workflow harness
```

`AI-Trader` 更像：

```text
agent-native live trading platform harness
```

它们共同提醒我们：

```text
研究、回测、交易、风控、报告，不应该割裂。
```

Quant / Trading Harness 的最小结构是：

```text
FactorHypothesis
StrategySpec
DataContract
UniverseDefinition
FeatureBuilder
BacktestRunner
CostModel
RiskModel
LeakageChecker
PerformanceReporter
BiasDiagnoser
PMReview
NextResearchPlan
```

这正好对应我们之前定义的 R&D Agent：

```text
自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ 人类 PM 审核
```

## 4. Tool Harness

一句话：

```text
Tool Harness = 让 agent 能稳定、安全、可追踪地使用外部工具。
```

它回答的问题是：

```text
工具从哪里来？
工具 schema 是什么？
什么时候调用哪个工具？
权限如何控制？
结果如何验证？
失败如何 fallback？
工具调用如何记录？
```

代表项目：

```text
CLI-Anything
AnyTool
LLMQuant data-mcp
MCP ecosystem
OpenHarness tool layer
FastAgent grounding backends
```

### CLI-Anything

`CLI-Anything` 的启发是：

```text
现实软件动作要变成 agent-native action。
```

它让 agent 不只是写文字，还能进入：

```text
CLI
local software
desktop operations
scriptable actions
```

### AnyTool

`AnyTool` 的重点是：

```text
universal tool-use layer
capability routing
tool matching
```

也就是：

```text
给定任务，agent 怎么知道该用哪个工具？
```

### data-mcp

LLMQuant 的 `data-mcp` 是 finance tool harness。

它把：

```text
wiki
papers
equity prices
crypto data
macro indicators
SEC filings
13F
ETF holdings
```

封装成 agent 可调用工具。

Tool Harness 的最小结构是：

```text
ToolRegistry
ToolSchema
CapabilityIndex
ToolRouter
PermissionPolicy
ExecutionAdapter
ResultValidator
ToolTrace
FallbackPolicy
```

没有 Tool Harness，agent 就会变成：

```text
会说话，但没有手。
```

## 5. Memory Harness

一句话：

```text
Memory Harness = 让 agent 的记忆可以被写入、压缩、检索、治理、审计和撤回。
```

它回答的问题是：

```text
记什么？
怎么压缩？
怎么召回？
谁能读？
谁能写？
怎么过期？
怎么撤回？
怎么删除？
怎么审计？
```

代表项目：

```text
MGP
CatchMe
SepLLM
LightRAG
MiniRAG
RAG-Anything
VideoRAG
QuantMind
```

### CatchMe

`CatchMe` 是：

```text
personal digital footprint recorder
```

它解决：

```text
原始行为和上下文怎么捕获？
```

对我们来说就是：

```text
research black box
```

记录我们如何学习、如何 coding、如何搜索、如何生成产物。

### SepLLM

`SepLLM` 是：

```text
long context / KV cache compression harness
```

它解决：

```text
长期上下文太大怎么办？
哪些 token 应该留在 active context？
哪些可以进入冷存储？
```

### MGP

`MGP` 是最关键的 memory governance harness。

它解决：

```text
谁可以写？
谁可以读？
记忆如何过期？
记忆如何撤回？
记忆如何删除？
记忆如何审计？
不同 backend 如何保持同一个 contract？
```

### LightRAG / MiniRAG / RAG-Anything / VideoRAG

这些提供不同形式的 knowledge memory：

```text
LightRAG:
  graph-enhanced textual knowledge memory

MiniRAG:
  lightweight local graph RAG memory

RAG-Anything:
  multimodal document memory

VideoRAG:
  long video memory
```

Memory Harness 的最小结构是：

```text
MemoryCandidate
MemoryObject
IngestionPolicy
CompressionPolicy
RetrievalPolicy
AccessPolicy
LifecyclePolicy
AuditLog
BackendAdapter
RecallInterface
DeletionInterface
```

没有 Memory Harness，Research OS 就没有长期复利。

## 6. Product / Workspace Harness

一句话：

```text
Product / Workspace Harness = 让 agent 进入真实工作场景、组织场景和产出场景。
```

它回答的问题是：

```text
agent 在哪里工作？
和谁协作？
如何接任务？
如何交付？
如何沟通？
如何写作？
如何生成视频？
如何进入真实组织？
```

代表项目：

```text
AgentSpace
OpenSpace
ClawTeam
ClawWork
Litewrite
OpenPhone
MoChat
VideoAgent
ViMax
```

### AgentSpace / OpenSpace

它们是 workspace harness。

重点是：

```text
agent 不只是一个会话。
agent 需要 workspace、task、file、memory、skill、state。
```

### ClawTeam / ClawWork

`ClawTeam` 是：

```text
AI organization layer
```

`ClawWork` 是：

```text
AI coworker economic accountability layer
```

它们让 agent 从单点工具进入：

```text
team
role
accountability
delivery
economic pressure
```

这对我们理解组织很重要。

### Litewrite

`Litewrite` 是 writing workspace harness。

它把：

```text
paper
blog
proposal
report
LaTeX
public artifact
```

变成 agent 可协作产出场景。

### MoChat / OpenPhone

它们让 agent 进入真实 communication interface：

```text
IM
phone
mobile app
networking
opportunity flow
```

### VideoAgent / ViMax

它们补齐 multimodal production harness：

```text
video understanding
meeting intelligence
video QA
video generation
research explainer
demo video
```

Product / Workspace Harness 的最小结构是：

```text
Workspace
Role
TaskBoard
Inbox
ArtifactStore
CommunicationChannel
WritingSurface
ReviewFlow
DeliveryContract
AccountabilityMetric
```

没有 Product Harness，agent 就停留在实验室。

有了 Product Harness，agent 才能进入真实工作。

## 六类 Harness 的关系

六类 harness 不是并列孤岛。

它们应该组成一张系统图。

```text
Product / Workspace Harness
  gives the real work surface

Agent Execution Harness
  runs the agent loop

Tool Harness
  gives agent hands

Memory Harness
  gives long-term state and recall

Research Task Harness
  turns research questions into artifacts

Quant / Trading Harness
  turns strategy hypotheses into experiments and PM decisions
```

组合起来：

```text
user / PM / researcher
  -> workspace
  -> task spec
  -> agent execution
  -> tool calls
  -> memory recall
  -> artifact generation
  -> evaluation
  -> diagnosis
  -> human review
  -> memory writeback
  -> next task
```

这就是我们要的：

```text
Pengyi Research OS Harness
```

## Pengyi Harness OS 抽象

我现在会把自己的目标定义成：

```text
Pengyi Harness OS =
  a thin but strict layer that turns AI tasks into executable, auditable, reusable research workflows.
```

它不需要一开始就很大。

但必须有几个核心对象。

### 1. TaskSpec

```text
task_id
task_type
owner
objective
input_artifacts
required_tools
success_criteria
risk_level
public_private_boundary
deadline
```

### 2. ToolContract

```text
tool_name
capability
input_schema
output_schema
permission
freshness
failure_mode
validation_rule
```

### 3. ExecutionTrace

```text
step_id
agent_role
action
tool_call
input
output
timestamp
error
retry
cost
```

### 4. Artifact

```text
artifact_id
artifact_type
path
source_task
source_evidence
summary
version
public_safe
```

### 5. EvaluationReport

```text
quality_score
factuality
coverage
redundancy
missing_evidence
runtime_errors
test_results
```

### 6. DiagnosisReport

```text
failure_reason
bias
leakage
robustness
cost
risk
next_fix
```

### 7. MemoryWriteback

```text
memory_candidate
sensitivity
retention_policy
evidence_refs
scope
audit_id
```

### 8. HumanReview

```text
reviewer
decision
comments
approved_actions
blocked_actions
next_plan
```

这几个对象组合起来，就是：

```text
task -> execution -> artifact -> evaluation -> diagnosis -> review -> memory
```

## 对 Quant Research OS 的专门映射

如果把 harness 用到量化研究：

```text
Quant Research Harness =
  Research Task Harness
  + Quant / Trading Harness
  + Tool Harness
  + Memory Harness
```

最小链路：

```text
FactorHypothesis
  -> DataContract
  -> FeatureBuilder
  -> BacktestRunner
  -> PerformanceReport
  -> BiasDiagnosis
  -> PMReview
  -> MemoryWriteback
```

具体字段：

```text
factor_name
universe
rebalance_frequency
lookback_window
target
neutralization
transaction_cost
slippage
turnover
IC
rankIC
Sharpe
max_drawdown
capacity
leakage_check
regime_split
```

这比单纯说：

```text
让 AI 自动做量化
```

要严肃得多。

因为真正的 quant harness 必须能回答：

```text
数据有没有未来函数？
回测是否可复现？
交易成本是否计入？
因子是否过拟合？
不同市场 regime 是否稳定？
PM 为什么应该相信这个结果？
```

## 对 AI Scientist 的专门映射

如果把 harness 用到 AI scientist：

```text
AI Scientist Harness =
  Research Task Harness
  + Agent Execution Harness
  + Tool Harness
  + Memory Harness
```

最小链路：

```text
ResearchQuestion
  -> LiteratureSearch
  -> PaperReading
  -> IdeaGeneration
  -> ExperimentPlan
  -> CodeImplementation
  -> Evaluation
  -> Report
  -> PeerReview
  -> NextExperiment
```

这就是我们冲顶会必须要的东西。

顶会不是靠灵感一闪。

顶会需要：

```text
问题
文献
方法
实验
代码
对比
消融
复现
写作
review response
```

AI Scientist Harness 的价值就是把这条链路标准化。

## 对个人生产力的映射

我们也可以把 harness 用到自己每天的工作。

```text
Personal Productivity Harness =
  Product / Workspace Harness
  + Agent Execution Harness
  + Memory Harness
```

任务可以是：

```text
写一篇学习笔记
整理一个 repo
准备一次沟通
生成一份 CV package
投递 RA
复盘工作经历
准备 quant interview
```

每个任务都应该有：

```text
objective
input
output
deadline
private/public boundary
artifact path
review status
next action
```

这就是我们一直在做的网站和笔记系统。

但下一步要从人工习惯升级成：

```text
task harness
```

## Harness 的设计原则

我总结十条原则。

### 1. Task first

不要先问模型。

先定义任务。

```text
What is the objective?
What is the expected artifact?
What counts as done?
```

### 2. Contract first

每个工具、数据、artifact 都要有 contract。

```text
input
output
permission
validation
failure
```

### 3. Trace everything

没有 trace，就没有复现。

没有复现，就没有 research。

### 4. Evaluate outputs, not vibes

不要只觉得结果“看起来不错”。

要有：

```text
quality check
factuality check
test result
backtest metric
human review
```

### 5. Memory must be governed

长期记忆不是随便存。

要有：

```text
scope
sensitivity
retention
audit
deletion
```

### 6. Tools need permissions

agent 能做事，就必须有权限边界。

尤其是：

```text
file write
shell
network
trading
email
private notes
```

### 7. Human review is a feature

人类 PM 审核不是低效。

它是高风险任务的安全阀。

### 8. Artifacts are first-class

最终结果不是聊天记录。

最终结果是：

```text
md
pdf
code
report
dataset
chart
backtest artifact
website post
PR
```

### 9. Small harness beats huge platform

一开始不要造巨型平台。

先造：

```text
TaskSpec + Trace + Artifact + Evaluation + Review
```

能跑起来最重要。

### 10. Harness compounds

每次任务完成后，都应该产生：

```text
artifact
memory
lesson
template
next plan
```

这就是复利。

## 现在我们已经有的 Harness 资产

已经看过并能复用的项目：

| Area | Existing Notes |
|---|---|
| Agent Runtime | `HKUDS015 OpenHarness`, `HKUDS035 FastAgent`, `HKUDS008 AutoAgent` |
| Research Evaluation | `HKUDS012 Auto-Deep-Research`, `HKUDS013 DeepResearch-Eval`, `HKUDS041 Revisited` |
| Quant / Trading | `LLMQUANT004 Magents`, `HKUDS002 Vibe-Trading`, `HKUDS005 AI-Trader`, X2Strategy notes |
| Tooling | `HKUDS004 CLI-Anything`, `HKUDS017 AnyTool`, `LLMQUANT001 data-mcp` |
| Memory | `HKUDS045 CatchMe`, `HKUDS047 SepLLM`, `HKUDS048 MGP`, `HKUDS001 LightRAG` |
| Product | `HKUDS033-042 Agent Product Series`, `HKUDS044 ViMax` |

现在做 `PENGYI_HARNESS_MAP000` 的意义是：

```text
把这些分散资产统一到一个 harness 概念下。
```

## 后续 Harness 系列

如果继续做这个系列，我建议：

```text
PENGYI_HARNESS_MAP000:
  Harness 总览

PENGYI_HARNESS_MAP001:
  Agent Execution Harness
  OpenHarness + FastAgent + AutoAgent

PENGYI_HARNESS_MAP002:
  Research Task Harness
  Auto-Deep-Research + DeepResearch-Eval + DeepCode

PENGYI_HARNESS_MAP003:
  Quant / Trading Harness
  Magents + X2Strategy + Vibe-Trading + AI-Trader

PENGYI_HARNESS_MAP004:
  Tool Harness
  CLI-Anything + AnyTool + data-mcp + MCP

PENGYI_HARNESS_MAP005:
  Memory Harness
  MGP + CatchMe + SepLLM + LightRAG

PENGYI_HARNESS_MAP006:
  Product / Workspace Harness
  AgentSpace + OpenSpace + ClawTeam + Litewrite + MoChat + ViMax
```

最后可以做：

```text
PENGYI_HARNESS_MAP007:
  Pengyi Research OS Harness v0 Design
```

## 最小可做 Demo

最小 demo 不要太大。

可以先做：

```text
harness-demo/
  tasks/
    task_001.yaml
  artifacts/
    task_001_report.md
  traces/
    task_001_trace.jsonl
  reviews/
    task_001_pm_review.md
  memory/
    task_001_memory_candidate.md
```

第一个任务可以是：

```text
把一篇 HKUDS / LLMQuant 学习笔记转成：
  1. summary
  2. system module mapping
  3. next action
  4. public-safe website post
  5. memory candidate
```

这个 demo 小，但结构完整。

它验证：

```text
TaskSpec
Artifact
Trace
Review
Memory
```

这就是 harness 的最小闭环。

## 一句话总结

```text
Harness 是我们从“学习很多 AI 项目”走向“构建自己的 Research OS”的中间抽象。
```

没有 harness：

```text
LLM 只是会回答。
```

有了 harness：

```text
LLM 可以进入任务、工具、记忆、评估、产出和组织。
```

对我们来说，最终目标是：

```text
Pengyi Research OS Harness
  = Agent Execution
  + Research Task
  + Quant Trading
  + Tool
  + Memory
  + Product Workspace
```

这就是 `PENGYI_HARNESS_MAP000` 的核心结论。
