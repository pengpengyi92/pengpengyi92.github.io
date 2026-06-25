---
title: "HKUDS0000: 中场 Map - 四大主线、已做 Repo 与下一阶段路线"
date: 2026-06-25 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds0000, hkuds, midgame-map, research-os, quant-research-os, ai-scientist, agent-workspace, rag, roadmap]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的中场 map。

```text
HKUDS0000 -> Midgame Map
```

它不替代 `HKUDS000`，也不是某个单独 repo 的深挖。

`HKUDS000` 是第一张地图：我们刚开始看 HKUDS 时，先确认本地有哪些 repo、第一批优先看什么。

`HKUDS0000` 是中场地图：我们已经连续完成 `HKUDS000` 到 `HKUDS013`，现在需要把这些学习沉淀成几条明确主线。

到这里，我们已经不是在“随机看项目”。

我们已经形成了四条主线：

```text
1. Quant / Finance
2. Research OS / AI Scientist
3. Agent Framework / Workspace
4. RAG / Knowledge
```

这四条线共同服务一个更大的目标：

```text
Pengyi Research OS
+ Pengyi Quant Research OS
+ Pengyi AI Scientist Stack
```

## 当前已完成总览

目前已经完成的 HKUDS numbered posts：

| ID | Repo / Topic | 我们给它的定位 |
|---|---|---|
| HKUDS000 | Study Map | HKUDS 第一阶段总地图 |
| HKUDS001 | LightRAG | graph-based research memory |
| HKUDS002 | Vibe-Trading | agentic quant research workflow |
| HKUDS003 | nanobot | personal always-on agent shell |
| HKUDS004 | CLI-Anything | agent-native software action layer |
| HKUDS005 | AI-Trader | agent-native live trading platform layer |
| HKUDS006 | AgentSpace | organizational agent workspace |
| HKUDS007 | RAG-Anything | multimodal document ingestion |
| HKUDS008 | AutoAgent | agent framework / agent factory |
| HKUDS009 | DeepCode | paper-to-code / research-to-code implementation |
| HKUDS010 | AI-Researcher | autonomous scientific discovery workflow |
| HKUDS011 | DeepInnovator | scientific idea model training |
| HKUDS012 | Auto-Deep-Research | practical deep research assistant |
| HKUDS013 | DeepResearch-Eval | report evaluation + factuality checking |

这已经形成一个非常完整的系统闭环：

```text
knowledge ingestion
-> research memory
-> personal agent shell
-> tool / software action
-> quant workflow
-> live trading platform
-> organizational workspace
-> agent factory
-> research-to-code
-> autonomous scientific discovery
-> idea model training
-> deep research assistant
-> report evaluator
```

这一段是 HKUDS 学习的第一阶段成果。

## 四大主线

现在按我们的目标，把 HKUDS repo 重新分类。

### 主线一：Quant / Finance

目标：

```text
建立 Pengyi Quant Research OS
```

这条线关注：

```text
market data
research autopilot
factor hypothesis
backtest
broker / account
live trading
report library
risk diagnosis
```

#### 已做

| Repo | 状态 | 价值 |
|---|---|---|
| `Vibe-Trading` | 已做 HKUDS002 | quant research workflow / Research Autopilot / backtest layer |
| `AI-Trader` | 已做 HKUDS005 | agent-native live trading platform / strategy competition / copy trading |

`Vibe-Trading` 更像：

```text
Quant research workflow + data/backtest/report layer
```

`AI-Trader` 更像：

```text
Live trading platform + agent strategy operation layer
```

这两个项目给我们的启发是：

```text
Research 和 Trading 要分层。
```

研究层要回答：

```text
这个想法有没有逻辑？
数据在哪里？
怎么回测？
哪里可能过拟合？
能不能复现？
```

交易层要回答：

```text
如何接账户？
如何授权？
如何执行？
如何监控？
如何风控？
如何审计？
```

#### 后续可做

| Repo | 为什么值得做 |
|---|---|
| `FutureShow` | real-world forecasting，适合研究 prediction / forecast agent |
| `RecLM` | recommendation instruction tuning，可借鉴金融推荐 / 策略推荐 |
| `LLMRec` | LLM + graph augmentation for recommendation |
| `RLMRec` | representation learning with LLM for recommendation |
| `EasyRec` | simple yet effective language model for recommendation |
| `XRec` | explainable recommendation，适合可解释策略推荐 |

Quant / Finance 线下一步不一定马上看传统推荐系统，但这批 repo 可以作为：

```text
factor recommendation
asset recommendation
strategy recommendation
research idea recommendation
```

的参考。

## 主线二：Research OS / AI Scientist

目标：

```text
建立 Pengyi AI Scientist Stack
```

这条线关注：

```text
paper reading
deep research
idea generation
research planning
implementation
experiment analysis
paper/report writing
evaluation
personal tutoring
scientific output
```

#### 已做

| Repo | 状态 | 价值 |
|---|---|---|
| `DeepCode` | 已做 HKUDS009 | paper-to-code / research-to-code |
| `AI-Researcher` | 已做 HKUDS010 | autonomous scientific discovery workflow |
| `DeepInnovator` | 已做 HKUDS011 | train model to generate better scientific ideas |
| `Auto-Deep-Research` | 已做 HKUDS012 | practical deep research assistant |
| `DeepResearch-Eval` | 已做 HKUDS013 | report quality evaluation + factuality checking |

这条线现在已经非常强：

```text
Auto-Deep-Research -> 生成深度研究报告
DeepResearch-Eval  -> 评估报告质量和事实性
DeepCode           -> 把研究需求转成代码
AI-Researcher      -> 跑完整科研 workflow
DeepInnovator      -> 训练更会产生 idea 的模型
```

它已经接近一个 AI scientist loop：

```text
read
-> think
-> propose
-> implement
-> evaluate
-> write
-> improve
```

#### 后续可做

| Repo | 为什么值得做 |
|---|---|
| `DeepTutor` | 个性化学习 agent，适合构建 AI scientist self-training loop |
| `Paper2Slides` | paper -> presentation，把科研结果转成表达输出 |
| `Litewrite` | writing agent，适合 research statement / paper / blog 写作 |
| `VideoRAG` | 视频资料 RAG，适合课程、访谈、讲座学习 |
| `VideoAgent` | video understanding/editing/remaking，适合视频型知识处理 |

这条线下一篇最适合：

```text
HKUDS014 -> DeepTutor
```

原因是它承接我们当前最核心的个人目标：

```text
把自己训练成 AI scientist。
```

DeepTutor 可以帮助我们思考：

```text
如何把论文、项目、课程、代码练习组织成个性化学习系统？
如何让 agent 变成 tutor，而不是只做工具？
如何把学习过程变成可评估、可反馈、可迭代的 loop？
```

## 主线三：Agent Framework / Workspace

目标：

```text
建立 Pengyi Agent Runtime + Workspace + Skill System
```

这条线关注：

```text
agent loop
tool calling
MCP
workspace
multi-agent collaboration
approval
audit
skills
failure-to-skill
self-evolving agents
```

#### 已做

| Repo | 状态 | 价值 |
|---|---|---|
| `nanobot` | 已做 HKUDS003 | personal agent shell / always-on workspace |
| `CLI-Anything` | 已做 HKUDS004 | software action layer / agent-native CLI ecosystem |
| `AgentSpace` | 已做 HKUDS006 | human + agents in one workspace |
| `AutoAgent` | 已做 HKUDS008 | fully automated agent factory / zero-code workflow creation |

这条线给我们的核心启发：

```text
agent 不是只有 prompt。
agent 是 runtime + tools + memory + workspace + permission + audit + skill。
```

`nanobot` 更偏个人：

```text
personal agent shell
```

`AgentSpace` 更偏组织：

```text
human + agents + workspace
```

`CLI-Anything` 更偏工具生态：

```text
make software agent-native
```

`AutoAgent` 更偏生产 agent：

```text
create agents / tools / workflows automatically
```

#### 后续可做

| Repo | 为什么值得做 |
|---|---|
| `OpenHarness` | agent harness / built-in personal agent，补 agent runtime 管理层 |
| `OpenSpace` | smarter, low-cost, self-evolving agents，接 AgentSpace / AutoAgent |
| `UpSkill` | failure -> reusable skill，和 Codex skill / agent 进化高度相关 |
| `AnyTool` | universal tool-use layer，补工具抽象 |
| `ClawTeam` | agent swarm intelligence，多 agent 协作 |
| `ClawWork` | AI coworker / automation product，组织生产力视角 |
| `MoChat` | social agent，适合沟通型 agent |
| `CatchMe` | personal agent personalization，适合个人长期记忆与习惯适配 |

这条线下一阶段非常关键。

因为如果我们要真正做 `Pengyi Research OS`，不能只是有 RAG 和几个工具，而是要有：

```text
workspace
roles
agents
tools
skills
logs
approvals
review
deploy
cost control
```

我建议在 `DeepTutor` 后面接：

```text
HKUDS015 -> OpenHarness
HKUDS016 -> UpSkill
HKUDS017 -> AnyTool
```

这样 agent runtime、skill evolution、tool-use layer 会补齐。

## 主线四：RAG / Knowledge

目标：

```text
建立 Pengyi Knowledge Layer / Evidence Vault
```

这条线关注：

```text
document ingestion
multimodal parsing
knowledge graph
vector retrieval
source grounding
long-term memory
small-model RAG
video RAG
graph foundation model
```

#### 已做

| Repo | 状态 | 价值 |
|---|---|---|
| `LightRAG` | 已做 HKUDS001 | graph-based RAG / research memory |
| `RAG-Anything` | 已做 HKUDS007 | multimodal document ingestion / all-in-one RAG |

这两个是我们的知识基建核心：

```text
RAG-Anything -> 把 PDF / Office / image / table / formula 等复杂资料吃进来
LightRAG     -> 把知识组织成 graph + vector + KV memory，并支持 query
```

对于我们来说，它们可以支撑：

```text
paper memory
project memory
CV / PS / RP material memory
quant research evidence vault
company / lab / professor knowledge base
```

#### 后续可做

| Repo | 为什么值得做 |
|---|---|
| `MiniRAG` | small-model RAG / low-cost RAG，适合低成本知识系统 |
| `VideoRAG` | chat with videos，适合课程、访谈、讲座学习 |
| `GraphAgent` | agentic graph language assistant，接知识图谱问答 |
| `GraphGPT` | graph instruction tuning for LLMs |
| `OpenGraph` | graph foundation models |
| `AnyGraph` | graph foundation model in the wild |
| `Awesome-LLM4Graph-Papers` | LLM4Graph survey / paper map |
| `Awesome-LLM4Urban-Papers` | urban / spatio-temporal paper map |

这条线要服务两个方向：

```text
Research OS 的 evidence memory
Quant OS 的 market / paper / report / project knowledge memory
```

如果要继续补 RAG，建议：

```text
HKUDS018 -> MiniRAG
HKUDS019 -> VideoRAG
HKUDS020 -> GraphAgent
```

## 当前四条线的完成度

| 主线 | 已做核心 | 当前完成度 | 下一步 |
|---|---|---:|---|
| Quant / Finance | `Vibe-Trading`, `AI-Trader` | 40% | 看 `FutureShow` 或 recommendation 系列 |
| Research OS / AI Scientist | `DeepCode`, `AI-Researcher`, `DeepInnovator`, `Auto-Deep-Research`, `DeepResearch-Eval` | 65% | 看 `DeepTutor`, `Paper2Slides`, `Litewrite` |
| Agent Framework / Workspace | `nanobot`, `CLI-Anything`, `AgentSpace`, `AutoAgent` | 55% | 看 `OpenHarness`, `UpSkill`, `AnyTool`, `OpenSpace` |
| RAG / Knowledge | `LightRAG`, `RAG-Anything` | 45% | 看 `MiniRAG`, `VideoRAG`, `GraphAgent` |

这张表说明：我们不是平均推进。

我们现在最强的是：

```text
Research OS / AI Scientist
```

最贴近量化落地的是：

```text
Quant / Finance
```

最需要补工程底盘的是：

```text
Agent Framework / Workspace
```

最需要继续沉淀长期资产的是：

```text
RAG / Knowledge
```

## 下一阶段推荐编号

我建议后续这样排：

| Next ID | Repo | 主线 | 理由 |
|---|---|---|---|
| HKUDS014 | `DeepTutor` | Research OS / AI Scientist | 个性化学习 agent，直接服务 AI scientist self-training |
| HKUDS015 | `OpenHarness` | Agent Framework / Workspace | agent harness / runtime 管理层 |
| HKUDS016 | `UpSkill` | Agent Framework / Workspace | failure -> reusable skill，和 Codex skill 高度相关 |
| HKUDS017 | `AnyTool` | Agent Framework / Workspace | universal tool-use layer |
| HKUDS018 | `MiniRAG` | RAG / Knowledge | low-cost / small-model RAG |
| HKUDS019 | `Paper2Slides` | Research OS / AI Scientist | paper -> presentation，科研表达输出 |
| HKUDS020 | `FutureShow` | Quant / Finance | forecasting agent，接量化/预测系统 |
| HKUDS021 | `VideoRAG` | RAG / Knowledge | 视频资料进入知识系统 |
| HKUDS022 | `FastCode` | Research OS / Coding | code understanding acceleration |
| HKUDS023 | `OpenSpace` | Agent Framework / Workspace | self-evolving agent space |

这里的顺序不是唯一的，但它符合我们当前最重要的目标：

```text
先增强自己学习和科研输出能力
再补 agent runtime / skill / tools
再补 RAG 和 quant forecast
```

## 我们已经形成的系统图

如果把当前已做的项目放到 Pengyi Research OS 里，大概是：

```text
Pengyi Research OS

1. Knowledge Layer
   - RAG-Anything
   - LightRAG

2. Personal Agent Layer
   - nanobot

3. Tool Action Layer
   - CLI-Anything

4. Quant Research Layer
   - Vibe-Trading

5. Live Trading Layer
   - AI-Trader

6. Workspace / Organization Layer
   - AgentSpace

7. Agent Factory Layer
   - AutoAgent

8. Research-to-Code Layer
   - DeepCode

9. Autonomous Research Layer
   - AI-Researcher

10. Idea Model Layer
   - DeepInnovator

11. Deep Research Product Layer
   - Auto-Deep-Research

12. Report Evaluation Layer
   - DeepResearch-Eval
```

这个系统图已经有很强的叙事力。

它不是单一 repo。

它是一个完整 stack：

```text
data / knowledge
-> agent
-> tools
-> workflow
-> workspace
-> research
-> code
-> evaluation
-> human PM
```

## 对 Quant Research OS 的映射

如果专门映射到量化：

```text
Quant Research OS

Knowledge Intake
  - RAG-Anything
  - LightRAG

Public Research
  - Auto-Deep-Research
  - DeepResearch-Eval

Hypothesis / Idea
  - DeepInnovator
  - AI-Researcher

Implementation
  - DeepCode
  - CLI-Anything
  - AutoAgent

Backtest / Trading Workflow
  - Vibe-Trading

Live / Platform
  - AI-Trader

Agent Workspace
  - nanobot
  - AgentSpace

Human PM Review
  - report evaluator
  - factuality checking
  - approval / audit / next-round plan
```

这正好对应我们之前说的：

```text
R&D Agent for Quant Research
= 自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ 人类 PM 审核
```

现在要补一句：

```text
+ 自动评估研究报告和事实可靠性
```

## 对个人成长的映射

这条 HKUDS 学习路线也不是只为了项目地图。

它也是我们的个人成长路线：

```text
成为 AI scientist
成为 full-stack research engineer
成为能做顶会项目的人
成为能构建自己开源系统的人
```

对应能力是：

| 能力 | HKUDS 参考 |
|---|---|
| 读论文和资料 | Auto-Deep-Research, LightRAG, RAG-Anything |
| 组织知识 | LightRAG, MiniRAG, GraphAgent |
| 写代码实现 | DeepCode, FastCode, CLI-Anything |
| 设计 agent | AutoAgent, nanobot, OpenHarness |
| 组织多 agent 工作 | AgentSpace, OpenSpace, ClawTeam |
| 做科研流程 | AI-Researcher, DeepInnovator |
| 做研究评估 | DeepResearch-Eval |
| 做量化系统 | Vibe-Trading, AI-Trader |
| 做表达输出 | Paper2Slides, Litewrite |
| 训练自己 | DeepTutor, UpSkill |

所以 `HKUDS0000` 的意义是：

```text
我们已经从看项目，进入到构建自己的能力地图。
```

## 中场结论

`HKUDS0000` 的一句话总结：

```text
HKUDS000-013 已经为 Pengyi Research OS 搭出了第一版骨架：
RAG/knowledge、agent runtime、tool action、quant workflow、workspace、research automation、
idea generation、deep research 和 report evaluation 都已经有对应参考实现。
```

下一阶段要做的不是继续无序扩张，而是围绕四条主线补关键缺口：

```text
Quant / Finance        -> FutureShow, RecLM, XRec
Research OS / AI Sci   -> DeepTutor, Paper2Slides, Litewrite
Agent Framework        -> OpenHarness, UpSkill, AnyTool, OpenSpace
RAG / Knowledge        -> MiniRAG, VideoRAG, GraphAgent
```

我建议下一篇：

```text
HKUDS014 -> DeepTutor
```

原因很明确：

```text
我们正在把自己训练成 AI scientist。
DeepTutor 正好对应 personal learning / personalized tutoring / self-training loop。
```

这会把 HKUDS 学习从“看项目”推进到“用项目训练自己”。
