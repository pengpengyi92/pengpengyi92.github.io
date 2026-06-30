---
title: "HKUDS042: Agent Product Phase Review 作为 Pengyi Research OS Agent Product Stack 阶段复盘"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds042, hkuds, agent-product, phase-review, research-os, quant-os, ai-organization]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS042`。

```text
HKUDS042 -> Agent Product Phase Review
```

这一篇不是单个 repo 拆解，而是对 `HKUDS032-041` 的阶段复盘。

`HKUDS032` 定义了 Agent Product / Workspace 路线：

```text
HKUDS033 ClawTeam
HKUDS034 ClawWork
HKUDS035 FastAgent
HKUDS036 Litewrite
HKUDS037 OpenPhone
HKUDS038 MoChat
HKUDS039 UpSkill Revisited
HKUDS040 VideoAgent
HKUDS041 Auto-Deep-Research / DeepResearch-Eval
HKUDS042 Agent Product Phase Review
```

注意，实际推进过程中编号有一个小调整：

```text
HKUDS038 -> MoChat
HKUDS039 -> UpSkill Revisited
```

原 StudyMap4 里写的是 `HKUDS038 UpSkill`、`HKUDS039 MoChat`，实际产出时为了先接 communication interface，先做了 MoChat，再回到 UpSkill 二刷。

这不影响主线。

核心仍然是：

```text
从 agent capability
走向 agent product
走向 personal Research OS
```

## 阶段总判断

这一阶段最重要的结论是：

```text
agent 不是一个模型，也不是一个聊天框。
agent 要成为产品，必须拥有 owner、role、workspace、tools、memory、communication、artifact、evaluation 和 audit trail。
```

换句话说：

```text
Agent Product = model + workflow + surface + state + accountability
```

`HKUDS033-041` 正好从不同方向补齐这件事。

| Layer | Project | Question |
|---|---|---|
| Team / Org | `ClawTeam` | agent 如何组成可分工、可监控、可交接的团队 |
| Work / Contract | `ClawWork` | agent 如何完成任务、交付 artifact、承担成本和质量评价 |
| Runtime / Execution | `FastAgent` | agent 如何规划、执行、验证，并统一调用工具 |
| Output Workspace | `Litewrite` | agent 如何生成 paper、blog、proposal、report 等正式产物 |
| Real-world Interface | `OpenPhone` | agent 如何进入手机和真实 app 界面 |
| Communication | `MoChat` | agent 如何进入 IM、协作、机会流和人类关系网络 |
| Skill Growth | `UpSkill` | agent 如何从失败中沉淀 reusable skill |
| Video / Meeting | `VideoAgent` | agent 如何处理访谈、课程、会议、视频材料 |
| Deep Research / Eval | `Auto-Deep-Research` + `DeepResearch-Eval` | agent 如何生成研究报告并评估质量 |

这就是一个 Research OS 的 agent product stack。

## 为什么这条线重要

我们前面看过很多 HKUDS 项目：

```text
LightRAG
RAG-Anything
MiniRAG
VideoRAG
AutoAgent
OpenHarness
AnyTool
AI-Researcher
DeepCode
FutureShow
GraphAgent
RecLM
KGRec
...
```

这些项目解决了：

```text
记忆
检索
工具调用
代码理解
图推理
推荐建模
预测判断
研究自动化
```

但这些仍然偏能力层。

Agent Product / Workspace 系列问的是另一个问题：

```text
这些能力如何进入一个人每天真实使用的工作台？
```

对我们来说，这个问题比“某个模型强不强”更重要。

因为我们的目标不是收藏 demo。

我们的目标是：

```text
做一个能支撑 AI scientist、quant research、RA / PhD 申请、开源项目产出和职业机会管理的个人生产系统。
```

## Layer 1: AI Organization

`HKUDS033 ClawTeam` 给的是组织层。

一句话：

```text
ClawTeam = framework-agnostic multi-agent team coordination layer
```

它的关键不是“多 agent 聊天”，而是：

```text
team schema
task schema
inbox
plan
lifecycle
git worktree isolation
tmux / subprocess backend
board / Web UI
MCP tools
agent skill
team templates
```

对 Pengyi Research OS 的启发是：

```text
AI scientist 不应该是一个单体 agent。
它应该是一个可分工的 research team。
```

最小团队可以是：

```text
PM Agent
Research Agent
Developer Agent
Reviewer Agent
Writing Agent
Outreach Agent
```

Quant Research OS 则可以是：

```text
PM Agent
Factor Ideation Agent
Data Agent
Backtest Agent
Risk Agent
Report Agent
```

人类 owner 负责最终方向、质量和资源分配。

## Layer 2: Work and Accountability

`HKUDS034 ClawWork` 给的是工作与经济责任层。

一句话：

```text
ClawWork = AI coworker economic accountability layer
```

它把 agent 从 assistant 推向 coworker。

核心是：

```text
真实任务
职业 rubric
交付 artifact
质量评分
token / API cost
balance
income
survival
dashboard
```

这和我们一直讨论的：

```text
contract
credit
cashflow
position
```

是同一类问题。

一个 agent 不能只证明“我会说”。

它要证明：

```text
我能完成任务。
我能交付文件。
我能被评价。
我能控制成本。
我能产生收益。
```

对 Quant OS 来说，这意味着每次研究都要有：

```text
research budget
compute cost
data cost
backtest artifact
risk score
expected value
PM decision
```

## Layer 3: Execution Engine

`HKUDS035 FastAgent` 给的是执行引擎。

一句话：

```text
FastAgent = planning + grounding + evaluation + unified tool execution
```

它把 agent 工作拆成：

```text
HostAgent      -> plan
GroundingAgent -> execute
EvalAgent      -> verify
```

再用：

```text
Kanban workflow
WorkflowEngine
GroundingClient
Smart Tool RAG
ToolQualityManager
Memory / ContentProcessor
recording / audit trail
```

把执行过程产品化。

这对我们自己的系统非常实用。

Research OS 不是只要“能调用工具”。

它要记录：

```text
谁规划了任务
调用了哪些工具
工具成功率如何
执行结果是什么
谁验证了结果
哪里失败了
下一次如何改进
```

也就是：

```text
execution audit trail
```

## Layer 4: Output Workspace

`HKUDS036 Litewrite` 给的是输出工作台。

一句话：

```text
Litewrite = AI-powered collaborative LaTeX and research writing workspace
```

它的重要性在于：

```text
研究最终必须落成 artifact。
```

Artifact 可以是：

```text
paper
technical report
blog
proposal
research statement
CV / PS / RP
quant memo
backtest report
PR description
weekly review
```

没有 output workspace，agent 只是临时对话。

有 output workspace，agent 才能进入：

```text
draft
review
edit
compile
publish
version
```

这也是我们网站持续更新的意义：

```text
每一次学习都要变成 public research asset。
```

## Layer 5: Real-world Interface

`HKUDS037 OpenPhone` 给的是真实 app / mobile interface。

一句话：

```text
OpenPhone = real-world mobile app operation and phone agent layer
```

它提醒我们：

```text
agent 不能只活在浏览器、代码仓库和 markdown 里。
```

真实机会来自：

```text
导师沟通
RA 套磁
quant senior 咨询
面试
微信群 / IM
银行 OA / CRM
broker app
邮件
日历
电话
```

Research OS 如果未来要成为真实生产力系统，必须能进入这些界面。

但 v0 阶段不需要先做完整 phone agent。

我们现在更现实的做法是：

```text
先把对话准备、meeting note、follow-up、联系人状态和机会 pipeline 做好。
```

OpenPhone 是后续扩展方向，不是当前第一优先级。

## Layer 6: Communication Interface

`HKUDS038 MoChat` 给的是沟通入口。

一句话：

```text
MoChat = agent-native IM, networking wingman, and AI organization interface
```

它的重要启发是：

```text
agent 需要进入人类协作网络。
```

MoChat 里的关键对象包括：

```text
identity
token
DM
group
panel
session
real-time event
owner binding
Socket.IO cursor persistence
message dedupe
mention detection
delay buffer
```

对我们来说，communication layer 可以服务：

```text
RA / PhD outreach
quant senior networking
open-source maintainer communication
PR / issue follow-up
mentor update
opportunity tracking
```

这和我们最近一直强调的“回到组织、卖方、人链接、企业链接”高度一致。

AI 解放生产力，不是为了永远躲在本地。

它应该帮助我们更高质量地进入真实关系网络。

## Layer 7: Skill Growth

`HKUDS039 UpSkill Revisited` 给的是 skill growth layer。

一句话：

```text
UpSkill = failure-to-skill distillation and long-term agent capability compounding
```

它把一次失败变成：

```text
session trace
Teacher diagnosis
SKILL.md generation
Student validation
persistent skill store
future retrieval
```

对我们来说，这件事非常重要。

我们每天都在 coding、写文章、读 repo、做网站、准备申请。

如果每一次失败都只是解决一次，那复利很低。

如果每一次失败都变成 skill：

```text
website publish skill
repo study skill
quant backtest sanity skill
research memo evaluation skill
outreach email skill
PR review skill
```

那 Research OS 会越用越强。

## Layer 8: Video and Meeting Workflow

`HKUDS040 VideoAgent` 给的是视频和会议工作流。

一句话：

```text
VideoAgent = natural language video workflow agent
```

它连接的是：

```text
访谈
课程
讲座
会议
seminar
路演
technical talk
AI researcher interview
quant discussion
```

这些内容不应该只是“看过了”。

它们应该进入：

```text
transcript
timestamped evidence
summary
QA
research note
follow-up task
study map
skill
```

对我们来说，这一点很直接：

```text
田渊栋访谈、硅谷 101、导师 talk、quant senior 沟通，都可以成为 Research OS 的输入。
```

VideoAgent 提醒我们：

```text
AI scientist 的知识输入不只是 paper 和 repo。
视频、会议、人际沟通也是高价值输入。
```

## Layer 9: Deep Research and Evaluation

`HKUDS041 Auto-Deep-Research / DeepResearch-Eval` 给的是 deep research loop。

一句话：

```text
Auto-Deep-Research = producer
DeepResearch-Eval  = evaluator
```

合起来就是：

```text
question
-> plan
-> search / read / code
-> evidence
-> report
-> quality score
-> redundancy score
-> factual support
-> gap diagnosis
-> next plan
```

这是 AI scientist 的核心闭环。

没有 producer，就没有研究产出。

没有 evaluator，就没有质量控制。

没有 next plan，就没有研究迭代。

人类 PM 的角色是：

```text
设定方向
判断重要性
校准评价
决定资源
批准发布
决定下一轮
```

## 总架构

可以把这一阶段压成一张架构图：

```text
Pengyi Research OS Agent Product Stack

1. Organization Layer
   ClawTeam
   -> roles / team / tasks / inbox / worktree / board

2. Work Accountability Layer
   ClawWork
   -> contract / artifact / quality / cost / income / ROI

3. Execution Layer
   FastAgent
   -> planner / executor / evaluator / tools / audit trail

4. Output Layer
   Litewrite
   -> report / blog / paper / proposal / public artifact

5. Real-world Interface Layer
   OpenPhone
   -> app / phone / mobile workflows / external operations

6. Communication Layer
   MoChat
   -> IM / networking / opportunity / human-agent collaboration

7. Skill Growth Layer
   UpSkill
   -> failure / diagnosis / skill / validation / reuse

8. Video and Meeting Layer
   VideoAgent
   -> transcript / meeting intelligence / multimodal artifact

9. Deep Research Layer
   Auto-Deep-Research + DeepResearch-Eval
   -> report generation / evaluation / next research plan
```

这就是我们之后做 `Pengyi Research OS v0` 的参考框架。

## Product Principles

这一阶段得出的 agent product 原则：

| Principle | Meaning |
|---|---|
| Owner | 每个 agent 必须服务一个明确的人类 owner 或组织 owner |
| Role | agent 要有角色边界，不能所有事都混在一个对话里 |
| Workspace | agent 要有文件、任务、状态、artifact 的工作区 |
| Tool Schema | tool 调用必须结构化、可审计、可复用 |
| Memory | 重要上下文要沉淀，不应只停留在一次 session |
| Skill | 失败要变成可复用技能 |
| Communication | agent 要能和人、团队、外部机会流对接 |
| Artifact | 工作必须落成文件、报告、代码、表格、PR 等产物 |
| Evaluation | 每个产物都要有质量闸门 |
| Audit Trail | 关键行动、工具调用、来源和决策要可追踪 |

如果没有这些，agent 很容易退化成：

```text
能聊，但不能交付。
能演示，但不能进入生产。
```

## Pengyi Research OS v0 优先级

我们不应该一开始做全量系统。

更合理的 v0 是：

```text
1. Website / Notes / Output
2. Repo Study Workflow
3. Skill Library
4. Communication / Outreach Tracker
5. Deep Research Loop
6. Quant R&D Loop
```

### Priority 1: Website / Notes / Output

这是现在已经在做的。

目标：

```text
每个学习和项目都变成 public-safe artifact。
```

对象：

```text
blog post
study map
technical note
project report
portfolio page
```

为什么优先：

```text
它直接服务 RA / PhD / open-source / quant opportunity。
```

### Priority 2: Repo Study Workflow

我们已经形成了一个流程：

```text
pull / fetch repo
inspect structure
read README and key files
write Local Snapshot
explain project use
explain implementation
extract components
map to Research OS / Quant OS
publish to website
```

下一步要把这个变成模板和 skill。

### Priority 3: Skill Library

把重复工作沉淀成：

```text
repo-study skill
website-publish skill
research-report skill
quant-interview-prep skill
outreach-email skill
PR-proposal skill
```

这就是 UpSkill 对我们的现实价值。

### Priority 4: Communication / Outreach Tracker

对象包括：

```text
黄超老师
金敦泓老师
子健哥
RA / PhD 导师
open-source maintainers
quant teams
```

每个人要有：

```text
profile
relationship state
pitch
questions
meeting notes
follow-up
next action
```

这比做复杂 phone agent 更紧急。

### Priority 5: Deep Research Loop

先做轻量版：

```text
question.md
sources.jsonl
notes.md
report.md
eval.md
next_plan.md
```

然后逐步自动化：

```text
source collection
claim extraction
quality scoring
factuality check
human PM review
```

### Priority 6: Quant R&D Loop

这是我们的长期核心。

最小闭环：

```text
factor_hypothesis.md
data_plan.md
implementation.py
backtest_report.md
bias_diagnosis.md
pm_review.md
next_experiment.md
```

它要继承 deep research loop，但加上 quant-specific evaluator。

## 暂时不要过度建设的部分

有些方向很酷，但不是 v0 第一优先：

| Direction | Why Later |
|---|---|
| Full mobile phone agent | 实装成本高，当前先做 communication tracker 更现实 |
| Full video editing stack | 当前先处理 transcript / note / evidence，视频剪辑后置 |
| Full multi-agent org | 先用单人 PM + 几个明确 workflow，避免过早复杂化 |
| Full automated web browsing | 先用人工收集 + structured sources，保证质量 |
| Full quant live trading | 先做 research / backtest / diagnosis，实盘需要数据、风控和合规 |

当前重点应该是：

```text
可产出
可复用
可展示
可评估
```

## Human PM 的核心地位

这一阶段反复出现一个结论：

```text
human PM review 很关键。
```

原因很简单：

agent 可以：

```text
生成很多
搜索很多
执行很多
总结很多
评估很多
```

但人类 PM 要判断：

```text
这个问题值不值得做？
这个方向是不是战略重点？
这个结果能不能发布？
这个实验是否该继续？
这个机会是否值得投入？
这个合作对象是否匹配？
```

所以我们的 Research OS 不是替代自己。

它是放大自己。

最合适的结构是：

```text
Human PM
  -> decides direction, constraints, quality bar

Agents
  -> produce evidence, code, reports, drafts, follow-ups

Evaluation Layer
  -> scores, checks, surfaces risks

Human PM
  -> accepts / rejects / revises / launches next round
```

## Readiness Checklist

之后我们每做一个 agent workflow，都可以用这张 checklist：

| Question | Required |
|---|---|
| Who owns this workflow? | human owner / PM |
| What is the input? | task, files, links, repo, data |
| What is the output artifact? | report, code, PR, memo, email |
| Where is state stored? | folder, YAML, JSONL, database |
| What tools are allowed? | shell, browser, files, API, backtest |
| How is quality evaluated? | rubric, tests, reviewer, metrics |
| What is the audit trail? | logs, sources, tool calls, diffs |
| What happens after completion? | publish, review, next plan |
| What happens after failure? | diagnosis, skill, retry plan |

这张表可以成为 Pengyi Research OS 的产品门槛。

## Next Roadmap

Agent Product 阶段结束后，后面有几条路线可以继续：

```text
1. HKUDS 总结总表
2. Urban / Spatiotemporal 系列
3. Graph / Rec 进一步二刷
4. Research OS v0 原型
5. Quant R&D Agent 原型
6. Open-source PR contribution track
```

其中当前最自然的下一步就是：

```text
HKUDS043 -> 总结 HKUDS000-042 的作用清单
```

这样我们会有一张完整地图，知道自己已经看了什么，每个项目在 Research OS / Quant OS 中对应什么模块。

## 最后总结

`HKUDS042` 的核心结论：

```text
Agent Product / Workspace 系列让我们从“看 agent repo”进入“设计 personal Research OS”。
```

这一阶段给出的总架构是：

```text
organization
-> work accountability
-> execution
-> output workspace
-> real-world interface
-> communication
-> skill growth
-> video / meeting workflow
-> deep research / evaluation
```

对我们来说，最重要的是落到 v0：

```text
先把网站输出、repo study、skill library、communication tracker、deep research loop 和 quant R&D loop 打通。
```

这才是从学习 HKUDS 到建设 Pengyi Research OS 的真正转化。
