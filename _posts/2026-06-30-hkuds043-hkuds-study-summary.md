---
title: "HKUDS043: HKUDS 学习总览与 000-042 项目作用清单"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds043, hkuds, study-summary, research-os, quant-os, roadmap]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS043`。

```text
HKUDS043 -> HKUDS study summary
```

这一篇做一个总整理：

```text
把 HKUDS000-042 目前所有学习节点列出来，
说明每一个项目 / map 的作用，
并把它们重新映射到 Pengyi Research OS 和 Quant Research OS。
```

这不是终点。

这是阶段性索引。

我们后面继续做 HKUDS044+ 时，可以回到这篇看：

```text
已经看过什么
每个项目解决什么问题
哪些模块可以复用
哪些主线还没推进
```

## 总体分层

目前已经看的 HKUDS 内容可以分成七大类：

| Layer | Projects |
|---|---|
| Study Maps | `HKUDS000`, `HKUDS0000`, `HKUDS00000`, `HKUDS032`, `HKUDS042`, `HKUDS043` |
| Knowledge / RAG | `LightRAG`, `RAG-Anything`, `MiniRAG`, `VideoRAG` |
| Quant / Forecasting | `VIBE-TRADING`, `AI-Trader`, `FutureShow` |
| Agent Runtime / Workspace | `nanobot`, `CLI-Anything`, `AgentSpace`, `AutoAgent`, `OpenHarness`, `AnyTool`, `OpenSpace` |
| Research / AI Scientist | `DeepCode`, `AI-Researcher`, `DeepInnovator`, `Auto-Deep-Research`, `DeepResearch-Eval`, `DeepTutor`, `Paper2Slides`, `FastCode` |
| Graph / Recommendation | `GraphAgent`, `OpenGraph`, `GraphGPT`, `HiGPT`, `RecLM`, `XRec`, `AutoCF`, `KGRec` |
| Agent Product | `ClawTeam`, `ClawWork`, `FastAgent`, `Litewrite`, `OpenPhone`, `MoChat`, `UpSkill Revisited`, `VideoAgent` |

压成一句话：

```text
HKUDS 学习线已经从 knowledge / RAG，
走到 agent infrastructure，
走到 research automation，
再走到 graph / recommendation / decision intelligence，
最后进入 agent product / personal Research OS。
```

## 000-042 作用清单

| 编号 | Repo / Topic | 作用 |
|---|---|---|
| `HKUDS000` | Study Map | 第一张 HKUDS 总览图，启动 `PENGYI_HKUDS_STUDYMAP`，把 HKUDS repo 作为长期学习和 Research OS 参考库。 |
| `HKUDS001` | `LightRAG` | 轻量图增强 RAG，把知识切成 entity / relation / graph / text chunk，启发 Research OS 的长期知识记忆层。 |
| `HKUDS002` | `VIBE-TRADING` | Agentic quant research workflow，把交易想法、策略生成、回测和报告联系起来，是 Quant OS 的早期关键参考。 |
| `HKUDS003` | `nanobot` | Personal agent shell / always-on workspace，让 agent 具备本地入口、工具、消息和持续工作形态。 |
| `HKUDS004` | `CLI-Anything` | Agent-native software action layer，把 CLI / GUI 操作转成 agent 可执行动作，服务真实软件操作。 |
| `HKUDS005` | `AI-Trader` | Agent-native live trading platform layer，把 LLM agent 放入交易系统、市场分析、执行和风险控制场景。 |
| `HKUDS006` | `AgentSpace` | Organizational agent workspace，让多个 agent / task / file / workspace 进入组织式协作。 |
| `HKUDS007` | `RAG-Anything` | Multimodal document ingestion，把 PDF、图、表、公式、文档转成统一 RAG 输入，是资料整理层。 |
| `HKUDS008` | `AutoAgent` | Self-developing agent factory，让 agent 能创建 workflow、工具和应用，是 Auto-Deep-Research 的底座。 |
| `HKUDS0000` | Midgame Map | 中场地图，把前面 agent/RAG/research 项目重新分组，规划后续主线。 |
| `HKUDS009` | `DeepCode` | Paper2Code / agentic coding implementation layer，把论文方法转成代码，是 research engineering 关键模块。 |
| `HKUDS010` | `AI-Researcher` | Autonomous scientific discovery benchmark，关注 AI researcher 如何提出问题、实验和研究产出。 |
| `HKUDS011` | `DeepInnovator` | Scientific idea foundation model / innovation training layer，关注 idea generation 和 scientific creativity。 |
| `HKUDS012` | `Auto-Deep-Research` | Open deep research product，把 web/file/code agent 组织成可用 deep research assistant。 |
| `HKUDS013` | `DeepResearch-Eval` | Report-centric evaluation，评估 deep research 报告质量、冗余和事实支撑。 |
| `HKUDS014` | `DeepTutor` | Agent-native personalized tutoring，用 tutor agent 支撑 AI scientist 自我训练和学习反馈。 |
| `HKUDS015` | `OpenHarness` | Agent harness runtime，为 agent 提供可运行、可评测、可复用的基础执行环境。 |
| `HKUDS016` | `UpSkill` | Failure-to-skill distillation，把 agent 失败轨迹转化为可复用 skill。 |
| `HKUDS00000` | StudyMap3 | 第三张学习地图，规划 HKUDS020 之后的 forecasting、video、code、graph、rec、agent product 等主线。 |
| `HKUDS017` | `AnyTool` | Universal tool-use layer，通过 tool routing 和 capability matching 让 agent 更好选择工具。 |
| `HKUDS018` | `MiniRAG` | Lightweight graph RAG / on-device knowledge layer，为小规模、本地化知识系统提供参考。 |
| `HKUDS019` | `Paper2Slides` | Research-to-presentation artifact generator，把 paper / research note 转成 slides，服务公开表达和申请材料。 |
| `HKUDS020` | `FutureShow` | Forecasting agent benchmark / quant judgment layer，连接预测、forecast agent 和 prediction market 思维。 |
| `HKUDS021` | `VideoRAG` | Extreme long-context video memory，把长视频切成 timestamped multimodal knowledge object。 |
| `HKUDS022` | `FastCode` | Repo-level code intelligence acceleration，加速代码阅读、理解和 research engineering。 |
| `HKUDS023` | `OpenSpace` | Self-evolving agent workspace / skill economy，让 agent workspace 能自我演化和沉淀技能。 |
| `HKUDS024` | `GraphAgent` | Agentic graph language assistant，让 LLM agent 能理解和操作图任务。 |
| `HKUDS025` | `OpenGraph` | Open graph foundation model，关注 zero-shot graph generalization 和通用图智能。 |
| `HKUDS026` | `GraphGPT` | Graph instruction tuning，把图结构和语言模型对齐，是 graph-language alignment 代表。 |
| `HKUDS027` | `HiGPT` | Heterogeneous graph language model，处理异构图和多类型节点关系。 |
| `HKUDS028` | `RecLM` | Recommendation instruction tuning，把用户 profile、item、ranking 融入语言模型推荐。 |
| `HKUDS029` | `XRec` | Explainable recommendation，把协同信号转成自然语言解释，连接推荐和可解释决策。 |
| `HKUDS030` | `AutoCF` | Automated self-supervised collaborative filtering，为稀疏交互建模和推荐 backbone 提供参考。 |
| `HKUDS031` | `KGRec` | Knowledge graph self-supervised rationalization，把 KG rationale 和 recommendation 结合。 |
| `HKUDS032` | StudyMap4 | 第四张学习地图，切入 Agent Product / Workspace 系列。 |
| `HKUDS033` | `ClawTeam` | AI organization layer，让 agent 组成团队，具备 team/task/inbox/plan/worktree/board。 |
| `HKUDS034` | `ClawWork` | AI coworker economic accountability，让 agent 任务有成本、交付、质量、收益和生存压力。 |
| `HKUDS035` | `FastAgent` | Agent execution engine，用 planner / executor / evaluator、Tool RAG 和 audit trail 支撑真实任务。 |
| `HKUDS036` | `Litewrite` | AI research writing workspace，承接 paper、blog、proposal、report、LaTeX 和 public artifact。 |
| `HKUDS037` | `OpenPhone` | AI phone / real-world mobile interface，让 agent 进入手机、app 和真实外部操作。 |
| `HKUDS038` | `MoChat` | Agent-native IM / networking interface，让 agent 进入沟通、机会流和协作网络。 |
| `HKUDS039` | `UpSkill Revisited` | Agent skill growth layer，二刷 UpSkill，把失败沉淀成 Research OS 的长期复利。 |
| `HKUDS040` | `VideoAgent` | Agentic video workflow，处理访谈、课程、会议、视频理解、总结、QA 和剪辑任务。 |
| `HKUDS041` | `Auto-Deep-Research / DeepResearch-Eval Revisited` | Deep Research Product Loop，把 producer 和 evaluator 合并成 AI scientist research loop。 |
| `HKUDS042` | Agent Product Phase Review | 复盘 Agent Product / Workspace 系列，抽象出 Pengyi Research OS Agent Product Stack。 |
| `HKUDS043` | HKUDS Study Summary | 当前总索引，列出所有 HKUDS 节点作用，服务后续路线规划。 |

## Map 型文章

有几篇不是单一 repo，而是路线图：

| Map | Role |
|---|---|
| `HKUDS000` | 启动总览，确认 HKUDS 是长期学习对象 |
| `HKUDS0000` | 中场地图，梳理前期 repo 的结构 |
| `HKUDS00000` | 第三张地图，规划 HKUDS020 之后的几大主线 |
| `HKUDS032` | 第四张地图，切到 Agent Product / Workspace |
| `HKUDS042` | Agent Product 阶段复盘 |
| `HKUDS043` | 当前总索引 |

这些 map 的作用是：

```text
避免只是连续看 repo。
每隔一段时间要把 repo 重新压缩成架构。
```

这是 Research OS 本身的一部分。

## Knowledge / RAG 主线

已经看过：

```text
LightRAG
RAG-Anything
MiniRAG
VideoRAG
```

它们共同回答：

```text
知识如何进入系统？
文档如何被解析？
多模态材料如何被索引？
长视频如何变成可检索 evidence？
小型本地知识库如何运行？
```

对 Pengyi Research OS 的作用：

```text
notes
papers
PDFs
blogs
videos
meeting transcripts
project READMEs
CV / PS / RP materials
```

都应该变成可检索、可引用、可复用的 knowledge object。

对 Quant OS 的作用：

```text
研报
公告
paper
factor notes
market commentary
WorldQuant factor explanation
backtest report
```

也需要进入结构化记忆。

## Quant / Forecasting 主线

已经看过：

```text
VIBE-TRADING
AI-Trader
FutureShow
```

它们共同回答：

```text
AI agent 如何进入交易和预测？
策略想法如何生成？
市场判断如何评估？
交易系统如何连接研究、执行和风险？
```

对我们来说，这是 Quant Research OS 的直接参考。

最重要的转化是：

```text
factor idea
-> implementation
-> backtest
-> diagnosis
-> report
-> PM decision
```

这条线还远没有结束。

后续可以继续看更多 forecasting、time series、spatiotemporal 和 finance-adjacent repo。

## Agent Infrastructure 主线

已经看过：

```text
nanobot
CLI-Anything
AgentSpace
AutoAgent
OpenHarness
AnyTool
OpenSpace
```

它们共同回答：

```text
agent 如何运行？
agent 如何调用工具？
agent 如何管理 workspace？
agent 如何和 CLI / GUI / MCP / harness 连接？
agent 如何自我演化？
```

这是所有产品层的底座。

如果没有这一层，后面的 Research OS 只能停留在 prompt。

## Research / AI Scientist 主线

已经看过：

```text
DeepCode
AI-Researcher
DeepInnovator
Auto-Deep-Research
DeepResearch-Eval
DeepTutor
Paper2Slides
FastCode
```

它们共同回答：

```text
AI 如何帮助研究？
如何读论文？
如何写代码？
如何提出 idea？
如何做 deep research？
如何评估报告？
如何教学和自我训练？
如何把研究转成 slides？
如何加速代码理解？
```

这条线和我们的身份目标最一致：

```text
成为能在顶会产出、能做开源 project、能持续自我强化的 AI scientist。
```

最核心的闭环是：

```text
idea
-> evidence
-> implementation
-> experiment
-> report
-> evaluation
-> next idea
```

## Graph / Recommendation 主线

已经看过：

```text
GraphAgent
OpenGraph
GraphGPT
HiGPT
RecLM
XRec
AutoCF
KGRec
```

它们共同回答：

```text
图结构如何和语言模型结合？
用户 / item / relation / KG 如何进入推荐系统？
协同过滤和语言解释如何结合？
异构关系如何建模？
```

这条线对 Quant OS 也有潜在价值。

因为市场本身就是图：

```text
stock-stock relation
industry graph
supply chain graph
news-event-company graph
fund holding graph
analyst coverage graph
factor correlation graph
```

推荐系统里的：

```text
user-item interaction
collaborative signal
explainable ranking
KG rationale
```

可以类比到金融里的：

```text
asset-signal interaction
factor-asset exposure
strategy recommendation
portfolio explanation
```

## Agent Product 主线

已经看过：

```text
ClawTeam
ClawWork
FastAgent
Litewrite
OpenPhone
MoChat
UpSkill Revisited
VideoAgent
Auto-Deep-Research / DeepResearch-Eval Revisited
Agent Product Phase Review
```

它们共同回答：

```text
agent 如何成为产品？
agent 如何进入组织、任务、工作台、沟通、写作、视频、研究和评估？
```

这一阶段最大的收获是：

```text
Research OS 不是一个模型。
Research OS 是一套 agent product stack。
```

包括：

```text
organization
work accountability
execution
output workspace
real-world interface
communication
skill growth
video / meeting workflow
deep research / evaluation
```

## 对 Pengyi Research OS 的总映射

可以把目前所有学习成果映射成：

| Research OS Module | HKUDS References |
|---|---|
| Knowledge Memory | `LightRAG`, `RAG-Anything`, `MiniRAG`, `VideoRAG` |
| Agent Shell | `nanobot`, `AutoAgent`, `OpenHarness` |
| Tool / Action Layer | `CLI-Anything`, `AnyTool`, `FastAgent` |
| Workspace / Organization | `AgentSpace`, `OpenSpace`, `ClawTeam`, `ClawWork` |
| Research Automation | `AI-Researcher`, `DeepInnovator`, `Auto-Deep-Research`, `DeepResearch-Eval` |
| Code Research Engineering | `DeepCode`, `FastCode` |
| Writing / Publication | `Litewrite`, `Paper2Slides` |
| Communication / Opportunity | `MoChat`, `OpenPhone` |
| Video / Meeting Intelligence | `VideoRAG`, `VideoAgent` |
| Skill Growth | `UpSkill`, `UpSkill Revisited` |
| Decision Intelligence | `FutureShow`, `GraphAgent`, `RecLM`, `XRec`, `AutoCF`, `KGRec` |
| Quant Research | `VIBE-TRADING`, `AI-Trader`, `FutureShow`, graph/rec projects |

这张表就是我们后续做系统原型的模块图。

## 对 Quant Research OS 的总映射

Quant OS 可以这样拆：

| Quant OS Need | HKUDS Inspiration |
|---|---|
| 因子假设生成 | `VIBE-TRADING`, `AI-Researcher`, `DeepInnovator`, `Auto-Deep-Research` |
| 数据源调研 | `Auto-Deep-Research`, `RAG-Anything`, `LightRAG` |
| 代码实现 | `DeepCode`, `FastCode`, `FastAgent` |
| 回测执行 | `AI-Trader`, `VIBE-TRADING`, `FastAgent` |
| 结果报告 | `Litewrite`, `Paper2Slides`, `DeepResearch-Eval` |
| 偏差诊断 | `DeepResearch-Eval` plus future quant evaluator |
| 市场预测 | `FutureShow` |
| 关系建模 | `GraphAgent`, `OpenGraph`, `GraphGPT`, `HiGPT` |
| 信号推荐 | `RecLM`, `XRec`, `AutoCF`, `KGRec` |
| 团队协作 | `ClawTeam`, `ClawWork` |
| 技能复利 | `UpSkill` |

这对应我们最初定义的：

```text
R&D Agent for Quant Research
= 自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ 人类 PM 审核
```

现在 HKUDS 已经给了很多积木。

我们要做的是组合、裁剪、产品化。

## 已经形成的学习方法

通过 HKUDS000-043，我们已经形成了一套 repo 学习方法：

```text
1. 看本地是否已有 repo
2. fetch / 检查版本
3. 读 README
4. 看目录结构
5. 找主入口
6. 找核心类 / 脚本 / 配置
7. 理解项目用途
8. 理解实现方式
9. 抽象关键组件
10. 映射到 Research OS / Quant OS
11. 提出可 PR 的方向
12. 发布成网站文章
```

这本身就是一个 skill。

可以命名为：

```text
repo-study-to-public-research-asset
```

## 后续可以继续做什么

后面的 HKUDS044+ 有几个自然方向：

```text
1. Urban / Spatiotemporal 系列
2. Graph / Recommendation 二刷
3. Quant / Forecasting 深挖
4. Research OS v0 原型
5. Quant R&D Agent 原型
6. HKUDS PR contribution track
```

### Urban / Spatiotemporal

候选：

```text
UrbanGPT
OpenCity
EasyST
AutoST
FlashST
STExplainer
```

为什么值得看：

```text
时空预测、城市智能、交通流、区域数据和金融时间序列有共通结构。
```

### Graph / Recommendation 二刷

候选：

```text
AnyGraph
LightGCL
LightGNN
SSLRec
LLMRec
RecGPT
UnlearnRec
```

为什么值得看：

```text
图和推荐是 quant signal / portfolio / market relation 建模的重要类比。
```

### Research OS v0 原型

候选工作：

```text
task.yaml schema
sources.jsonl schema
report.md template
eval.json template
human_review.md template
website publish workflow
```

这是最贴近我们当前产出的方向。

### Quant R&D Agent 原型

候选工作：

```text
factor_hypothesis.md
data_plan.md
implementation.py
backtest_report.md
bias_diagnosis.md
pm_review.md
next_experiment.md
```

这是长期核心方向。

## 当前阶段最重要的三条结论

第一：

```text
HKUDS 不是一堆孤立项目。
它是一套 AI agent / knowledge / research / graph / product 的项目宇宙。
```

第二：

```text
我们已经可以把它们转化成 Pengyi Research OS 的模块图。
```

第三：

```text
下一步不是无限阅读，而是开始把学习方法、网站输出、skill library、deep research loop 和 quant R&D loop 产品化。
```

## 最后总结

`HKUDS043` 的作用就是把前面所有节点压缩成一张总地图。

目前我们已经完成：

```text
HKUDS000-043
```

其中：

```text
000 / 0000 / 00000 / 032 / 042 / 043 是地图和复盘
001-031 覆盖 RAG、agent、research、graph、recommendation、forecasting
033-041 覆盖 Agent Product / Workspace
```

这批学习的真正意义不是“看完了很多 repo”。

真正意义是：

```text
我们开始拥有一套可复用的 AI scientist / Quant Research OS 架构语言。
```

后面要做的是把它落到自己的系统：

```text
Pengyi Research OS v0
Pengyi Quant Research OS v0
public research portfolio
open-source contribution track
RA / PhD / quant opportunity pipeline
```

这就是 HKUDS 学习线目前的阶段成果。
