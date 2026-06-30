---
title: "HKUDS032: StudyMap4 - Agent Product / Workspace 系列路线图"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds032, hkuds, studymap4, agent-product, agent-workspace, clawteam, clawwork, fastagent, litewrite, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第四张学习地图。

```text
HKUDS032 -> StudyMap4
```

这篇不是单个 repo 详解。

它的作用是重新规划 `HKUDS032` 之后的路线：

```text
先看 Agent Product / Workspace 系列。
```

原本第三张地图里，`HKUDS020` 之后的路线包括：

```text
Forecasting
Video / Multimodal RAG
Code Intelligence
Agent Workspace
Graph / Knowledge Graph
Recommendation / Finance-adjacent
Spatiotemporal / Urban AI
Agent Product / Workspace
```

我们已经完成了其中几条关键主线：

```text
HKUDS020 FutureShow
HKUDS021 VideoRAG
HKUDS022 FastCode
HKUDS023 OpenSpace
HKUDS024 GraphAgent
HKUDS025 OpenGraph
HKUDS026 GraphGPT
HKUDS027 HiGPT
HKUDS028 RecLM
HKUDS029 XRec
HKUDS030 AutoCF
HKUDS031 KGRec
```

其中 `HKUDS028-031` 把 Recommendation / Finance-adjacent 主线补得比较完整：

```text
AutoCF -> sparse interaction representation
KGRec  -> KG rationale and UI-KG alignment
RecLM  -> semantic profile and ranking augmentation
XRec   -> collaborative-grounded natural language explanation
```

现在我们暂时不急着进入：

```text
UrbanGPT / OpenCity / EasyST / AutoST
```

而是先切到：

```text
Agent Product / Workspace
```

原因很直接：

```text
我们现在需要的不只是算法能力。
我们需要能把 AI scientist / Research OS 真正产品化、工作台化、日常化。
```

## 为什么现在切 Agent Product

前面我们已经看过很多 agent infrastructure。

已完成的基础层包括：

```text
HKUDS003 nanobot
HKUDS004 CLI-Anything
HKUDS006 AgentSpace
HKUDS008 AutoAgent
HKUDS015 OpenHarness
HKUDS016 UpSkill
HKUDS017 AnyTool
HKUDS023 OpenSpace
```

这些解决的是：

```text
agent 怎么运行
agent 怎么调用工具
agent 怎么接 workspace
agent 怎么演化 skill
agent 怎么做 harness
agent 怎么成为个人 shell
```

但下一阶段要解决的是另一类问题：

```text
agent 怎么变成产品？
agent 怎么进入协作组织？
agent 怎么进入日常工作台？
agent 怎么写作、沟通、打电话、开会、做 deep research？
agent 怎么支撑个人和组织的生产力闭环？
```

这就是 Agent Product / Workspace 系列的意义。

它不是从算法论文角度看 agent。

它是从：

```text
product surface
workflow loop
human-AI collaboration
organizational deployment
research artifact production
```

来看 agent。

对我们自己的目标来说，这条线非常关键：

```text
Pengyi Research OS
Pengyi Quant Research OS
AI Scientist personal workspace
public research portfolio
RA / PhD / quant opportunity pipeline
open-source project execution system
```

这些都不是单个模型能解决的。

它们需要一组产品化 agent workspace。

## 本地 Repo Inventory

本地 HKUDS 工作区里，Agent Product / Workspace 相关 repo 已确认存在：

| Repo | Role in This Series |
|---|---|
| `ClawTeam` | 多 agent team / AI organization / 协作团队形态 |
| `ClawWork` | agent workspace / work execution / 工作台 |
| `FastAgent` | 快速 agent runtime / lightweight agent product shell |
| `Litewrite` | 写作产品 / research writing / blog and material generation |
| `OpenPhone` | 通讯 agent / phone agent / 对外链接与 follow-up |
| `UpSkill` | skill learning / failure-to-skill / 自我成长系统 |
| `MoChat` | chat product / workspace chat / 多模态入口 |
| `VideoAgent` | video agent / meeting and interview intelligence |
| `Auto-Deep-Research` | practical deep research assistant |
| `DeepResearch-Eval` | deep research output evaluation |

另外，已经做过但会在本系列中作为基础设施引用的 repo：

| Repo | 已完成编号 | 本系列中的位置 |
|---|---|---|
| `nanobot` | `HKUDS003` | personal agent shell |
| `CLI-Anything` | `HKUDS004` | computer / CLI action layer |
| `AgentSpace` | `HKUDS006` | organizational agent workspace |
| `AutoAgent` | `HKUDS008` | agent framework / factory |
| `OpenHarness` | `HKUDS015` | agent harness runtime |
| `AnyTool` | `HKUDS017` | universal tool-use routing |
| `OpenSpace` | `HKUDS023` | self-evolving agent workspace |

所以 StudyMap4 不是从零开始。

它是在前面 agent infrastructure 的基础上，进入：

```text
agent productization layer
```

## 新路线总表

后续先按这张表推进。

| 编号 | Repo / Topic | 系列 | 为什么看 |
|---|---|---|---|
| `HKUDS032` | `StudyMap4` | Roadmap | 明确 Agent Product / Workspace 这一阶段路线 |
| `HKUDS033` | `ClawTeam` | Agent Organization | 多 agent team、AI organization、协作组织形态 |
| `HKUDS034` | `ClawWork` | Agent Workspace | 工作台、任务执行、组织协作、agent-native work OS |
| `HKUDS035` | `FastAgent` | Agent Runtime / Product Shell | 快速 agent runtime，轻量产品化封装 |
| `HKUDS036` | `Litewrite` | Writing Product | 写作、blog、research statement、paper note、申请材料 |
| `HKUDS037` | `OpenPhone` | Communication Agent | 电话、沟通、follow-up、对外链接 |
| `HKUDS038` | `UpSkill` | Skill Growth | failure-to-skill、自我提升、个人长期能力系统 |
| `HKUDS039` | `MoChat` | Chat Workspace | chat product、多模态入口、个人 AI OS interface |
| `HKUDS040` | `VideoAgent` | Video / Meeting Agent | 访谈、课程、会议、视频知识工作流 |
| `HKUDS041` | `Auto-Deep-Research` / `DeepResearch-Eval` | Deep Research Product | 自动研究和研究报告评估闭环 |
| `HKUDS042` | `Agent Product Phase Review` | Review Map | 总结 Agent Product 如何接入 Pengyi Research OS |

一句话版本：

```text
HKUDS032 之后先做 Agent Product / Workspace：
ClawTeam -> ClawWork -> FastAgent -> Litewrite -> OpenPhone -> UpSkill -> MoChat -> VideoAgent -> Auto-Deep-Research / DeepResearch-Eval -> Phase Review
```

## Series Position

这条线和前面的 HKUDS 学习不是割裂的。

我们可以把前面已完成内容压成五层。

第一层，Knowledge / Memory：

```text
LightRAG
RAG-Anything
MiniRAG
VideoRAG
```

第二层，Action / Tool：

```text
CLI-Anything
AnyTool
OpenHarness
```

第三层，Agent / Workspace Infrastructure：

```text
nanobot
AgentSpace
AutoAgent
OpenSpace
```

第四层，Research / Engineering Intelligence：

```text
DeepCode
AI-Researcher
Auto-Deep-Research
DeepResearch-Eval
FastCode
Paper2Slides
```

第五层，Graph / Recommendation / Decision Intelligence：

```text
GraphAgent
OpenGraph
GraphGPT
HiGPT
FutureShow
RecLM
XRec
AutoCF
KGRec
```

现在 Agent Product / Workspace 要做的是：

```text
把这些能力放进人真正每天使用的 product surface。
```

也就是：

```text
not just capability
but usable workflow
```

## HKUDS033: ClawTeam

`ClawTeam` 应该作为这一阶段第一篇。

原因是它最像：

```text
AI organization
```

我们要看的重点不是“几个 agent 聊天”。

而是：

```text
team structure
role assignment
task decomposition
multi-agent collaboration
human supervision
workflow artifacts
```

对 Pengyi Research OS 来说，`ClawTeam` 可以映射成：

```text
Research Team Agent
```

例如：

```text
PM Agent
Research Agent
Developer Agent
Reviewer Agent
Data Agent
Writing Agent
```

这正好接我们之前一直说的：

```text
R&D Agent for Quant Research
```

未来的 Quant Research OS 不应该只有一个 monolithic agent。

更合理的是：

```text
一组可分工、可审计、可交接、可复盘的 research team agents。
```

所以 `ClawTeam` 是本阶段最合适的起点。

## HKUDS034: ClawWork

`ClawWork` 紧跟 `ClawTeam`。

如果 `ClawTeam` 关注：

```text
team
```

那么 `ClawWork` 很可能关注：

```text
work
workspace
task execution
```

我们要看它如何把 agent 从“对话”推进到“工作”：

```text
任务如何创建？
任务如何分配？
结果如何记录？
人如何介入？
工作台如何组织上下文？
agent 的输出如何变成 artifact？
```

对我们来说，`ClawWork` 可以映射成：

```text
Research Workbench
```

也就是：

```text
idea
-> task
-> experiment
-> code
-> report
-> review
-> next plan
```

这和我们的 Research OS 主线非常贴。

## HKUDS035: FastAgent

`FastAgent` 放在第三篇。

原因是我们看完组织和工作台后，需要看：

```text
agent runtime / product shell
```

很多 agent 系统的问题不是能力不够，而是太重、太慢、太难接入真实工作流。

`FastAgent` 要重点看：

```text
runtime abstraction
agent config
tool invocation
session management
developer experience
deployment simplicity
```

对 Pengyi Research OS 来说，它可能提供：

```text
轻量 agent product skeleton
```

也就是我们未来自己做：

```text
Pengyi Research OS desktop / CLI / web workspace
```

时可以参考的 runtime 设计。

## HKUDS036: Litewrite

`Litewrite` 很重要。

因为我们现在已经在持续把学习输出变成：

```text
blog
study map
technical notes
RA / PhD materials
research statement
project README
public narrative
```

写作不是附属能力。

对 AI scientist 来说，写作就是研究闭环的一部分：

```text
thinking
structuring
communicating
publishing
applying
fundraising / career positioning
```

`Litewrite` 可以接：

```text
technical blog writing
paper note writing
CV / PS / RP material drafting
research update writing
PR description writing
weekly review writing
```

这会直接服务我们的网站和申请材料。

## HKUDS037: OpenPhone

`OpenPhone` 这一篇会接“人与组织链接”。

我们不能只在电脑前做系统。

真实机会来自：

```text
导师沟通
RA 套磁
quant senior 咨询
开源项目维护者交流
PM / founder / recruiter follow-up
```

电话、语音、沟通 agent 的价值在于：

```text
prepare talking points
record conversation
extract commitments
generate follow-up
track relationship state
```

对我们来说，它可以映射成：

```text
Opportunity Communication OS
```

这条线非常现实。

## HKUDS038: UpSkill

`UpSkill` 之前已经作为 `HKUDS016` 做过一篇，但在 Agent Product 系列里可以重新引用。

如果需要独立再做，可以做：

```text
HKUDS038: UpSkill Revisited
```

这次关注点不是底层算法，而是：

```text
如何把失败变成 skill
如何把工作流沉淀成 personal capability
如何形成长期 self-improvement loop
```

对我们来说，这就是：

```text
只要在 coding，就每天都能复利进步。
```

但这个复利不能只靠情绪。

它要被系统化：

```text
failure
-> diagnosis
-> skill extraction
-> reusable workflow
-> next task improvement
```

## HKUDS039: MoChat

`MoChat` 可以作为 chat workspace 入口来看。

我们未来自己的 Research OS 需要一个入口：

```text
chat
task
memory
tools
files
research artifacts
```

chat 不只是聊天。

它可能是：

```text
personal AI OS interface
```

要重点看：

```text
conversation state
workspace context
multimodal input
tool integration
artifact output
session memory
```

这能帮助我们设计自己的个人 AI scientist workspace。

## HKUDS040: VideoAgent

`VideoAgent` 接的是视频和会议。

我们现在已经很重视：

```text
访谈
课程
讲座
会议
路演
技术分享
```

前面 `VideoRAG` 解决了：

```text
video knowledge ingestion
```

`VideoAgent` 要看的是：

```text
agent 如何围绕视频执行任务？
```

例如：

```text
看访谈
抽取 research worldview
生成学习笔记
定位重要片段
形成 follow-up tasks
接入 personal knowledge base
```

这很适合我们之前提到的：

```text
把田渊栋访谈、硅谷101、课程、seminar 变成研究输入。
```

## HKUDS041: Auto-Deep-Research / DeepResearch-Eval

这两个我们之前已经分别做过：

```text
HKUDS012 Auto-Deep-Research
HKUDS013 DeepResearch-Eval
```

但在 Agent Product 系列最后，可以把它们重新组合成：

```text
Deep Research Product Loop
```

重点不是重复拆 repo。

而是总结：

```text
deep research assistant
-> report generation
-> evidence grounding
-> evaluation
-> iteration
```

这正好接我们的 AI scientist 目标：

```text
自动研究
自动写报告
自动评估
自动发现下一轮问题
```

它可以作为 Agent Product 系列和 AI Scientist 系列的桥。

## HKUDS042: Agent Product Phase Review

最后做一个阶段复盘：

```text
HKUDS042 -> Agent Product Phase Review
```

这篇要回答：

```text
Agent Product 到底怎么接入 Pengyi Research OS？
哪些 repo 是 runtime？
哪些 repo 是 workspace？
哪些 repo 是 communication？
哪些 repo 是 writing？
哪些 repo 是 skill learning？
哪些 repo 是 deep research？
我们自己的系统 v0 应该先做哪一块？
```

目标是产出一个清晰架构：

```text
Pengyi Research OS Agent Product Layer

1. Team Layer
2. Workspace Layer
3. Runtime Layer
4. Writing Layer
5. Communication Layer
6. Skill Layer
7. Chat Interface Layer
8. Video / Meeting Layer
9. Deep Research Layer
```

这会成为我们后面真正做产品原型的设计依据。

## 暂时后移的主线

为了保持当前阶段聚焦，下面这些先后移：

```text
UrbanGPT
OpenCity
EasyST
AutoST
```

不是因为它们不重要。

它们仍然很有价值，尤其是：

```text
spatiotemporal forecasting
urban intelligence
graph time series
traffic / mobility / regional signal
```

这些和 quant 时间序列、cross-asset relation、macro / regional data 都有关系。

但当前我们的优先级是：

```text
先把 Research OS 的产品形态打通。
```

等 Agent Product 系列做完，再回到：

```text
Spatiotemporal / Urban AI
```

会更顺。

## 最终路线

所以从现在开始，新的短期路线是：

```text
HKUDS032 StudyMap4
HKUDS033 ClawTeam
HKUDS034 ClawWork
HKUDS035 FastAgent
HKUDS036 Litewrite
HKUDS037 OpenPhone
HKUDS038 UpSkill Revisited
HKUDS039 MoChat
HKUDS040 VideoAgent
HKUDS041 Auto-Deep-Research + DeepResearch-Eval Product Loop
HKUDS042 Agent Product Phase Review
```

这一段的目标不是多写几篇笔记。

它的目标是：

```text
把 HKUDS 的 agent product / workspace 项目，
转化成 Pengyi Research OS 的产品架构参考。
```

也就是：

```text
从研究能力
到日常工作台
到个人 AI scientist operating system
```

这是 StudyMap4 的核心意义。
