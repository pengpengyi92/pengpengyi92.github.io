---
title: "LLMQUANT000: PENGYI_LLMQUANT_STUDYMAP"
date: 2026-06-23 00:00:00 +0800
categories: [Learning, Quant Research]
tags: [pengyi-llmquant-studymap, llmquant000, llmquant, quant-research, research-os, ai-scientist]
---

这个系列先从 LLMQuant 开始。

系列名：

```text
PENGYI_LLMQUANT_STUDYMAP
```

这篇是：

```text
LLMQUANT000 -> Study Map
```

它不是单个项目详解，而是整个 LLMQuant 项目宇宙的总览地图。后续再用 `LLMQUANT001`、`LLMQUANT002`、`LLMQUANT003` 一篇一篇拆具体项目。

目前对我来说，LLMQuant 比较核心，因为它刚好站在我最想打通的位置：

```text
AI agent
  + quantitative finance
  + data infrastructure
  + research workflow
  + strategy experiment
  + public research output
```

它不是一个孤立 repo，而更像一个 AI-native finance research ecosystem。

## 为什么先做 LLMQuant

我现在的主线越来越清楚：

```text
AI Scientist
  -> Quant Research OS
  -> R&D Agent
  -> paper / news / report to structured knowledge
  -> factor / strategy hypothesis
  -> implementation
  -> backtest
  -> bias diagnosis
  -> next research plan
  -> human PM review
```

这个 loop 不是一个普通 chatbot 可以完成的。

它需要几个底层能力：

- 数据接口；
- 金融知识结构化；
- agent workflow；
- research memory；
- 回测与风控；
- 研究产物记录；
- public-safe 写作输出。

LLMQuant 的项目组合刚好覆盖这些层。

所以我不应该只是“看看 LLMQuant 有哪些 repo”。我应该把它当成一个训练场：

```text
读项目
  -> 抽象架构
  -> 学 workflow
  -> 找可复用组件
  -> 接入自己的 Research OS
  -> 做 public-safe demo
  -> 形成文章和开源资产
```

## 当前项目快照

这篇基于我当前本地 `LLMQUANT` 工作区快照整理。

当前一级项目可以分成 12 个部分：

| Project | Role |
|---|---|
| `.github` | LLMQuant 组织 profile 和产品矩阵入口 |
| `awesome-trading-agents` | trading agent 生态地图 |
| `data-mcp` | agent 可调用的金融数据 MCP server |
| `docs` | Finance Context 文档站 |
| `llmquant-book` | AI + Quant 教材和长文内容 |
| `Magents` | multi-agent trading simulation / backtesting framework |
| `quant-mind` | 金融论文、新闻、报告的结构化知识抽取与检索 |
| `quant-wiki.git` | Quant Wiki 知识库镜像 |
| `skills` | 金融 agent workflow skill catalog |
| `llmquantpengyinotes` | 我的 LLMQuant 学习笔记和报告库 |
| `llmquantpengyiproject` | 我的 Quant R&D Agent / Research OS / Auto Paper / Auto CV 项目集 |
| `llmquantpengyistrategy` | 我的 WorldQuant-style factor research 策略实验室 |

我先把上游项目和自己的本地扩展分清楚。

上游 LLMQuant 是学习对象和基础设施。

我的 `llmquantpengyi*` 是在它之上长出来的个人研究系统，不应该混在一起讲成官方项目。

## 四层理解

我现在会把 LLMQuant 分成四层。

```text
1. Community and knowledge entry
2. Data and knowledge infrastructure
3. Agent workflow and trading research
4. Pengyi local extension layer
```

展开来看：

| Layer | Projects | Meaning |
|---|---|---|
| 社区与知识入口层 | `.github`, `docs`, `llmquant-book`, `quant-wiki.git` | 组织叙事、金融知识、教育内容、长期知识资产 |
| 数据与知识基础设施层 | `data-mcp`, `quant-mind` | 金融数据工具、论文/新闻/报告结构化、检索和 memory |
| Agent workflow 与交易研究层 | `skills`, `Magents`, `awesome-trading-agents` | 金融任务 workflow、多 agent 策略模拟、生态雷达 |
| Pengyi 本地扩展层 | `llmquantpengyinotes`, `llmquantpengyiproject`, `llmquantpengyistrategy` | 我的 Research OS、R&D Agent、因子实验、笔记和职业叙事 |

这个结构很重要。

如果没有第一层，项目没有叙事和入口。

如果没有第二层，agent 没有数据和知识 grounding。

如果没有第三层，agent 不知道金融任务应该怎么做。

如果没有第四层，学习不会变成我自己的资产。

## 总架构图

我对 LLMQuant 的总理解是：

```text
paper / report / market idea
  -> QuantMind extracts structured knowledge
  -> data-mcp retrieves source-grounded financial data
  -> skills routes domain workflows
  -> Quant R&D Agent proposes hypotheses and tasks
  -> Research OS validates and records experiment contracts
  -> Magents or factor runner executes simulation / backtest
  -> Bias Diagnoser checks leakage, robustness, cost, risk
  -> Human PM review decides next action
  -> notes / website / CV package record public-safe artifacts
```

这就是我最想要的方向。

不是“让 AI 自动炒股”。

而是：

```text
让 AI 帮我持续生产可审计、可复现、可迭代的量化研究资产。
```

## 第一优先级项目

### 1. `data-mcp`

`data-mcp` 是数据工具层。

它把金融数据、论文、wiki、宏观指标、SEC filings、13F、ETF、股票和 crypto 数据封装成 agent 可调用工具。

对我来说，它的重要性非常直接：

```text
agent 不能只靠记忆做金融研究
agent 需要可追踪、可调用、可复现的数据接口
```

未来我的 Research OS 里，所有金融研究任务都应该先问：

```text
这个任务需要哪些 data-mcp capability？
数据来源是什么？
时间戳是什么？
缺失值和不确定性是什么？
```

后续可以做：

```text
LLMQUANT001 -> data-mcp study
```

重点看：

- MCP server 入口；
- tool registry；
- schema 设计；
- API client；
- data evidence contract；
- 如何被 agent 调用。

### 2. `skills`

`skills` 是 workflow 层。

它把金融任务拆成一类一类 skill：

```text
equities
options
macro
credit
rates / FX
commodities
crypto
events
portfolio
risk
strategies
market intelligence
investor lenses
```

这对我非常关键。

因为一个好的 finance agent 不是“随便回答金融问题”，而是要知道：

```text
这个任务属于哪个金融域？
需要哪些数据？
应该走什么分析步骤？
输出应该是什么格式？
哪些推断需要标注不确定性？
哪些结论不能直接当投资建议？
```

`skills` 给我的启发是：

```text
router SKILL.md
  -> workflow files
  -> data contract
  -> output discipline
```

这正是我自己的 Quant R&D Agent 应该学习的结构。

后续可以做：

```text
LLMQUANT002 -> skills study
```

重点看 workflow 怎么写、router 怎么组织、金融任务边界怎么定义。

### 3. `quant-mind`

`quant-mind` 是知识结构化层。

它的目标是把论文、新闻、博客、报告、filings 等非结构化材料，转成结构化金融知识。

我现在的理解是：

```text
QuantMind = research memory + financial knowledge extraction
```

它不是简单 summarization。

更重要的是把 source 转成可复用对象：

```text
paper
  -> structured paper object
  -> knowledge card
  -> factor / thesis / event candidate
  -> graph or retrieval memory
```

这个项目对于我的 Research OS 是核心中的核心。

因为我最终想做的是：

```text
paper / news / report
  -> factor hypothesis
  -> testable experiment
  -> diagnosis
  -> next research plan
```

如果没有 structured knowledge，后面的 hypothesis generation 很容易变成玄学。

后续可以做：

```text
LLMQUANT003 -> quant-mind study
```

重点看：

- fetch / parse / clean；
- knowledge model；
- paper flow；
- Factor / Thesis / GraphKnowledge 的设计方向；
- 如何接 RAG / graph memory。

### 4. `Magents`

`Magents` 是多 agent trading simulation 和 backtesting 框架。

它不是我的第一步，但它是重要的 execution reference。

我关心的不是“里面某个策略能不能赚钱”，而是它的系统结构：

```text
data pipeline
  -> strategy pods
  -> central risk manager
  -> backtesting engine
  -> reports
```

量化研究不能停留在想法。

最终必须进入：

- strategy specification；
- execution rule；
- position sizing；
- risk constraint；
- transaction cost；
- benchmark；
- out-of-sample diagnosis。

`Magents` 可以作为我学习多策略、多 agent、风控和回测组织方式的参考。

后续可以做：

```text
LLMQUANT004 -> Magents study
```

重点看 event-driven backtesting、pods、risk manager、strategy factory。

## 第二优先级项目

### `awesome-trading-agents`

这是生态雷达。

它收集 trading agents、MCPs、skills、benchmarks、resources。

对我来说，它有三个用途：

1. 找同行项目；
2. 找 benchmark 和学习对象；
3. 找真实 PR 机会。

但是原则必须清楚：

```text
不是为了 PR 而 PR
而是使用过程中真的发现 improvement possibility 再 PR
```

后续可以做：

```text
LLMQUANT005 -> awesome-trading-agents ecosystem map
```

### `docs`, `llmquant-book`, `quant-wiki.git`

这一组是知识资产层。

它们不是最先读源码的对象，但非常适合补金融 domain context。

作用是：

- 金融服务 workflow；
- AI + Quant 教材；
- Quant Wiki 概念库；
- 长期知识索引；
- 文章和研究叙事素材。

对我来说，它们更像：

```text
domain knowledge library
```

后续可以做：

```text
LLMQUANT006 -> finance knowledge layer study
```

## 我的本地扩展层

这一层是最贴近我自己的部分。

### `llmquantpengyinotes`

这是我的 LLMQuant 学习记忆层。

它负责把：

- 对话；
- 项目分析；
- 比较报告；
- 灵感；
- 研究路线；
- public-safe 文稿；

都沉淀成 Markdown、Word、PDF。

它的意义是：

```text
不要让学习停留在聊天记录里
要转成可调用的长期资产
```

### `llmquantpengyiproject`

这是我的个人项目集。

里面最关键的是：

| Project | Meaning |
|---|---|
| `pengyi_quant_rd_agent` | Quant R&D loop agent |
| `pengyi_quant_research_os_v0` | paper-to-factor-to-backtest orchestration |
| `pengyi_auto_paper_northpolestar` | paper evaluation and North Star alignment |
| `pengyiautocvspace` | CV / application material source-of-truth |

这其实已经形成我的 AI scientist / quant research transition 底层 OS。

核心 loop 是：

```text
Research Agent
  -> Developer Agent
  -> Runner
  -> Bias Diagnoser
  -> Loop Planner
  -> Human PM Reviewer
```

这就是我真正想做的 R&D Agent。

### `llmquantpengyistrategy`

这是策略实验室。

目前重点是 WorldQuant-style factor research scaffold。

它的意义不是马上做实盘，而是建立研究纪律：

```text
factor idea
  -> data validation
  -> factor implementation
  -> cross-sectional preprocessing
  -> portfolio construction
  -> backtest
  -> diagnostics
  -> research report
```

这会成为我的真实训练场。

## PENGYI_LLMQUANT_STUDYMAP 后续编号

我先定一个初始编号体系。

| Note | Topic | Deliverable |
|---|---|---|
| `LLMQUANT000` | project study map | 全局地图和学习顺序 |
| `LLMQUANT001` | `data-mcp` | data tool layer architecture |
| `LLMQUANT002` | `skills` | finance workflow / router / data contract |
| `LLMQUANT003` | `quant-mind` | structured financial knowledge and research memory |
| `LLMQUANT004` | `Magents` | multi-agent trading simulation and backtesting |
| `LLMQUANT005` | `awesome-trading-agents` | trading agent ecosystem radar |
| `LLMQUANT006` | `docs`, `llmquant-book`, `quant-wiki` | finance knowledge layer |
| `LLMQUANT007` | GitHub public repos | ecosystem coverage matrix and phase-1 closure |
| `LLMQUANT008` | `pengyi_quant_rd_agent` | R&D agent loop design |
| `LLMQUANT009` | `pengyi_quant_research_os_v0` | experiment ledger and research contracts |
| `LLMQUANT010` | `llmquantpengyistrategy` | WorldQuant-style factor research lab |

每一篇都回答三个问题：

```text
1. 这个项目解决什么问题？
2. 它是怎么实现的？
3. 它能怎么进入 Pengyi Quant Research OS？
```

## 最小闭环 Demo

我认为 LLMQuant 学习不能只停留在阅读。

最小闭环应该是：

```text
one public finance paper
  -> QuantMind-style structured knowledge card
  -> data-mcp evidence retrieval
  -> skills workflow routing
  -> Quant R&D Agent hypothesis
  -> Research OS experiment plan
  -> WorldQuant-style factor backtest protocol
  -> bias diagnosis note
  -> public-safe website write-up
```

这个 demo 不需要一开始完美。

但它必须做到：

- source-grounded；
- structured；
- reproducible；
- auditable；
- public-safe；
- human-reviewed。

这就是我区分“玩 agent”和“做研究系统”的边界。

## 对我自己的要求

我学习 LLMQuant，不应该只是激动。

要形成固定动作：

```text
read
  -> map
  -> extract
  -> implement
  -> test
  -> diagnose
  -> write
  -> publish
```

每个项目读完，至少产出一个东西：

- 一篇 study note；
- 一个 architecture map；
- 一个可运行 demo；
- 一个 workflow template；
- 一个 issue / PR；
- 一个 public-safe blog；
- 一个 Research OS component。

这才是真正的 compounding。

## Public-Safe Boundary

这个系列可以公开写：

- 公开 repo 的结构观察；
- 公开 README 和代码层面的学习；
- 高层架构图；
- 自己的 Research OS 抽象；
- sanitized demo；
- 不包含真实私密数据的因子实验。

不公开写：

- 私密因子库；
- 非公开数据源；
- 雇主内部材料；
- 私人沟通内容；
- 未经授权的业务数据；
- 未脱敏的申请/套磁策略。

私密规划继续放 private repo。

公开网站只放可以给任何人看的学习轨迹。

## 当前结论

LLMQuant 对我来说不是一个普通学习对象。

它是我目前最核心的 AI + Quant 训练场之一。

它给我一个完整参考：

```text
data layer
  -> knowledge layer
  -> workflow layer
  -> research loop
  -> execution and backtest
  -> diagnosis and review
  -> public artifact
```

我想做的 Pengyi Quant Research OS，本质上就是沿着这条链路继续往下打。

所以从这篇开始：

```text
PENGYI_LLMQUANT_STUDYMAP
  -> LLMQUANT000
  -> LLMQUANT001
  -> LLMQUANT002
  -> ...
```

先把地图立起来。

然后一个项目一个项目学透。

## References

- LLMQuant GitHub: <https://github.com/LLMQuant>
- LLMQuant Data MCP: <https://github.com/LLMQuant/data-mcp>
- LLMQuant Skills: <https://github.com/LLMQuant/skills>
- LLMQuant QuantMind: <https://github.com/LLMQuant/quant-mind>
- LLMQuant Magents: <https://github.com/LLMQuant/Magents>
- Awesome Trading Agents: <https://github.com/LLMQuant/awesome-trading-agents>
