---
title: "LLMQUANT002: skills 作为金融 workflow 路由层"
date: 2026-06-24 00:00:00 +0800
categories: [Learning, Quant Research]
tags: [pengyi-llmquant-studymap, llmquant002, llmquant-skills, agent-skills, quant-research, research-os]
---

这是 `PENGYI_LLMQUANT_STUDYMAP` 的第三篇：

```text
LLMQUANT002 -> skills
```

前一篇 `LLMQUANT001` 研究了 `data-mcp`。

我的结论是：

```text
data-mcp = evidence access layer
```

它解决的是 agent 如何拿到金融数据、论文、wiki、SEC filing、13F、ETF、macro 等外部证据。

这一篇研究 `skills`。

一句话理解：

```text
skills = finance workflow routing layer
```

如果说 `data-mcp` 负责回答“数据从哪里来”，那么 `skills` 负责回答：

```text
这个金融任务应该怎么做？
```

## 为什么 skills 重要

金融 agent 最大的问题不是“会不会说金融术语”。

真正的问题是：

```text
它是否知道不同金融任务的工作流边界？
```

股票研究、期权分析、宏观简报、信用风险、ETF exposure、portfolio what-if、hedge advisor、quant strategy backtest，不应该共用同一套泛泛 prompt。

每类任务都需要：

- 不同的数据；
- 不同的步骤；
- 不同的 freshness 要求；
- 不同的 fallback；
- 不同的输出格式；
- 不同的 guardrails。

`skills` 的价值就在这里。

它不是单个 `SKILL.md`，而是一个 category-level workflow catalog。

它把金融任务组织成：

```text
category router
  -> selected workflow
  -> data contract
  -> repeatable procedure
  -> structured output
  -> guardrails
```

这对我的 Quant Research OS 很关键。

我不想做一个“想到什么答什么”的 agent。

我想做的是：

```text
task intent
  -> route to workflow
  -> retrieve evidence
  -> produce auditable artifact
  -> record data used
  -> diagnose gaps
```

## 项目快照

本地 `LLMQUANT/skills` 当前结构显示：

| Item | Value |
|---|---:|
| category skills | 18 |
| workflow files | 79 |
| install unit | `skills/llmquant-*` category folder |
| router file | `SKILL.md` |
| workflow folder | `workflows/*.md` |
| data source | LLMQuant Data |
| plugin manifests | `.codex-plugin`, `.claude-plugin`, `.cursor-plugin` |
| templates | `templates/SKILL_TEMPLATE.md`, `templates/WORKFLOW_TEMPLATE.md` |
| license | MIT |

README 的定义很清楚：

```text
This repository is a skill catalog.
```

也就是说，`skills` 不是一个运行时数据服务。

它是金融 agent 的流程层。

## 总架构

我现在把 LLMQuant 的前两层理解成：

```text
data-mcp
  -> gives tools and evidence

skills
  -> tells the agent which workflow to run
```

组合起来：

```text
user intent
  -> category router SKILL.md
  -> selected workflow
  -> LLMQuant Data capability requirement
  -> data-mcp / compatible data tools
  -> evidence table
  -> structured answer
  -> risks / caveats / data used
```

这是一个非常好的 agent system pattern。

因为它把 agent 分成两个边界：

```text
tool boundary:
  how to get data

workflow boundary:
  how to reason over that data
```

很多 agent 项目失败，是因为这两个边界混在一起。

`skills` 把它们分清楚了。

## Category Router 结构

每个 category folder 都有一个 `SKILL.md`。

例如：

```text
skills/llmquant-equities/SKILL.md
skills/llmquant-options/SKILL.md
skills/llmquant-risk/SKILL.md
skills/llmquant-strategies/SKILL.md
```

`SKILL.md` 的 frontmatter 通常包括：

```yaml
name: llmquant-equities
description: Router skill for LLMQuant equities workflows...
input_data_source: LLMQuant Data
category: equities
```

然后正文有几个固定模块：

```text
Routing Rules
Workflow Index
LLMQuant Data Contract
Fallback
```

Router 的任务不是执行全部内容，而是：

```text
识别用户任务
选择最接近的 workflow
只打开相关 workflow
使用 LLMQuant Data 作为外部事实来源
报告日期、coverage、stale notice、missing input
```

这很重要。

一个好的 router 不应该把所有知识都塞进上下文。

它应该做：

```text
intent classification
  -> workflow selection
  -> minimal context loading
```

这和我前面从 `data-mcp` 学到的 progressive disclosure 是同一类工程思想。

## Workflow Contract

每个 workflow 文件通常包含：

```text
Purpose / Use When
Input Data Source
LLMQuant Data Contract / Data Needed
Workflow
Output Format
Guardrails
```

`templates/WORKFLOW_TEMPLATE.md` 里给出的标准结构是：

```text
1. Confirm identifiers, horizon, and output target.
2. Pull required LLMQuant Data.
3. Check coverage, dates, and missing fields.
4. Separate evidence from interpretation.
5. Produce the output format below.
```

这几句话非常关键。

它定义了一个 finance agent 应该有的纪律：

```text
先确认任务
再拉数据
检查数据边界
区分证据和解释
最后输出结构化结果
```

这就是 Research OS 需要的 workflow discipline。

## 18 个 Category

当前 catalog 有 18 个 category：

| Category | Workflows | Role |
|---|---:|---|
| `llmquant-data` | 4 | SEC filing、13F、macro snapshot、macro brief 等数据 primitive workflows |
| `llmquant-equities` | 5 | 股票分析、比较、research memo、merger arb、take-profit |
| `llmquant-etfs` | 1 | ETF overlap、持仓、集中度和 exposure |
| `llmquant-options` | 10 | IV rank、Greeks、P&L、vol surface、unusual activity、option backtest |
| `llmquant-equity-derivatives` | 2 | 单股衍生品、convertible / warrant |
| `llmquant-commodities` | 2 | 商品市场 lens、futures curve |
| `llmquant-crypto` | 3 | crypto regime、token research、perp funding |
| `llmquant-prediction-markets` | 3 | event probability、prediction market arb、options pricing 对比 |
| `llmquant-macro` | 3 | macro dashboard、Fed preview、macro-to-portfolio |
| `llmquant-credit` | 3 | issuer credit、spread regime、high-yield stress |
| `llmquant-rates-fx` | 3 | yield curve、central bank divergence、FX carry |
| `llmquant-events` | 3 | earnings、M&A、regulatory risk |
| `llmquant-portfolio` | 5 | company profile、thesis tracker、theme research、watchlist、alert |
| `llmquant-portfolio-lab` | 2 | exposure map、what-if simulator |
| `llmquant-risk` | 4 | fear score、VIX status、hedge advisor、research health check |
| `llmquant-strategies` | 6 | equity L/S、long-biased、event-driven、macro、quant、multi-strategy |
| `llmquant-market-intelligence` | 3 | macro view、market sentiment、event probability signals |
| `llmquant-investor-lenses` | 17 | Buffett、Graham、Munger、Lynch、Damodaran、Burry 等投资人视角 |

这个表说明一件事：

```text
skills 已经覆盖从数据查询到 PM 决策的多层金融任务。
```

它不是单一“股票分析 prompt”。

它更像一个金融工作流目录。

## 代表性 Workflow 观察

### 1. `equity-research-memo`

这个 workflow 用来生成 public company 的 equity research memo。

它要求的数据包括：

- SEC filing discovery；
- SEC filing section retrieval；
- equity price history；
- optional 13F holder data；
- optional wiki / paper context。

它的工作流是：

```text
clarify ticker and horizon
  -> read latest filing sections
  -> pull price history
  -> pull 13F holders when needed
  -> use wiki/paper for context
  -> build memo from evidence first
```

输出格式也很明确：

```text
Rating / View
Thesis Summary
Business Quality
Financial / Filing Evidence
Market Context
Ownership / Crowding
Key Risks
Variant Perception
Data Used
```

这比一般“帮我分析一只股票”的 prompt 强很多。

它强在：

```text
把股票研究变成一个可重复 memo protocol。
```

### 2. `10k-risk-review`

这个 workflow 是数据 primitive 层的好例子。

它处理的是：

```text
从最新 10-K 里做 evidence-first risk review
```

它要求读取：

- Item 1: business context；
- Item 1A: risk factors；
- Item 7: MD&A。

它的 guardrails 很明确：

- 不要把 10-Q 说成 10-K；
- 不要长篇引用 filing；
- 不要把 boilerplate risk factor 直接当高风险；
- 不要加入外部新闻，除非用户提供或 LLMQuant Data 获取。

这类 workflow 对我的 Research OS 很有价值。

因为它教我如何做 primary-source company read。

### 3. `iv-rank`

`llmquant-options` 的 `iv-rank` workflow 处理 volatility environment。

它要求：

- current IV snapshot；
- 1-year IV / HV history；
- equity price history；
- earnings / event calendar。

它不是简单说“IV rank 高就卖 premium”。

它明确写了 guardrail：

```text
Do not recommend selling premium solely because IV rank is high; check event risk.
```

这是非常好的金融约束。

因为期权策略不能只看单个指标。

### 4. `global-macro-dashboard`

这个 workflow 把 macro 任务拆成：

```text
growth
inflation
labor
liquidity
financial conditions
cross-asset confirmation
next releases
```

它还要求报告：

- observation date；
- market price date；
- release calendar date；
- stale-data notice。

宏观数据频率不同，非常容易误用。

所以这个 guardrail 很关键：

```text
不要混用 monthly / weekly / daily 数据而不说明频率差异。
```

### 5. `research-health-check`

这是我最喜欢的 workflow 之一。

它不是做市场分析，而是审计研究工作区：

- stale profiles；
- thesis drift；
- orphan themes；
- missing alerts；
- outdated evidence。

这和我的 Research OS 非常接近。

我未来也需要类似功能：

```text
哪些 research notes 过期了？
哪些 factor hypothesis 没有 backtest？
哪些 thesis 已经被新数据破坏？
哪些项目没有 next action？
哪些公开文章需要更新？
```

这说明 LLMQuant Skills 不只是“分析市场”，也在做 research operations。

### 6. `quant`

`llmquant-strategies/workflows/quant.md` 是 systematic / quant PM 的策略手册。

它强调：

- hypothesis before data；
- train / validation / test isolation；
- walk-forward analysis；
- overfitting control；
- capacity and slippage；
- factor vs alpha；
- paper trading；
- post-deployment monitoring。

这和我的 R&D Agent 完全对齐。

对我来说，最关键的一句是：

```text
Every quant strategy has four layers:
idea -> signal -> portfolio -> execution
```

我的 Research OS 也应该强制拆成：

```text
hypothesis
  -> signal definition
  -> portfolio construction
  -> execution / backtest
  -> diagnosis
```

## Data Contract 是核心

`skills` 最重要的思想不是“有很多 workflow”。

而是所有 workflow 都围绕一个 data contract：

```text
Use LLMQuant Data as external evidence.
State which data capabilities were used.
Cite returned dates or periods.
Do not invent data.
Name missing inputs.
Separate facts from interpretation.
```

这个 contract 是金融 agent 的安全边界。

没有它，agent 容易做三件危险的事：

1. 把记忆当数据；
2. 把推断当事实；
3. 把缺失数据补成幻觉。

所以我以后写自己的 workflow，也必须固定写：

```text
Required data
Optional data
Freshness
Fallback
Output
Guardrails
```

## Freshness 和 Fallback

金融数据很容易过期。

所以 `skills` 反复要求：

- filing dates；
- report periods；
- observation dates；
- price ranges；
- holdings as-of dates；
- stale notices；
- unsupported coverage。

这点非常重要。

例如：

```text
13F 不是实时持仓
ETF N-PORT 不是实时发行商持仓
10-K 反映的是 filing date 之前的披露
macro series 可能有 release lag 和 revision
options data 需要 timestamp
```

如果不写 freshness，金融分析就很容易变成假精确。

Fallback 也一样重要。

一个 workflow 不应该因为缺一个数据就胡编。

它应该说：

```text
缺了什么
还能基于什么继续
哪些结论因此不能下
```

这就是审计性。

## Guardrails

几乎每个 workflow 都有 guardrails。

一些典型 guardrails：

```text
不要编造缺失数据
不要把 model output 当 data
不要把 opinion 当 fact
不要给个性化投资建议
不要用 stale data 装作 current
不要把 10-Q 当 10-K
不要只因为 IV rank 高就卖期权
不要混用不同频率宏观数据而不说明
不要伪装成真实 Buffett / Munger 持仓或观点
```

这些 guardrails 给我的启发是：

```text
好的 agent workflow 不只是告诉 agent 做什么，
也要告诉 agent 不能做什么。
```

尤其金融领域，这一点非常重要。

## Investor Lenses 的价值和风险

`llmquant-investor-lenses` 有 17 个 workflow。

它覆盖 Buffett、Graham、Munger、Pabrai、Lynch、Fisher、Cathie Wood、Druckenmiller、Ackman、Burry、Taleb、Damodaran、Duan Yongping、Howard Marks 等 lens。

这类 workflow 很有趣，但也最容易误用。

它的 router 写得很清楚：

```text
The named workflows are analytical lenses,
not claims of endorsement or replication.
```

也就是说：

```text
这是分析框架，不是真人附体。
```

这个边界很好。

我觉得这类 lens 可以用于：

- 多视角分析；
- thesis stress test；
- valuation discipline；
- risk framing；
- communication style。

但不能用于：

- 虚构名人观点；
- 虚构名人持仓；
- 伪装成授权建议；
- 用 persona 替代数据。

这对我的网站写作也有启发：

```text
可以学习思想框架，
但必须公开标注它只是 lens。
```

## 和 data-mcp 的关系

现在我可以把 `data-mcp` 和 `skills` 放在一起理解：

```text
data-mcp:
  provides callable data tools

skills:
  provides repeatable finance workflows
```

一个完整任务应该是：

```text
user asks:
  analyze AAPL 10-K risk

skills:
  route to llmquant-data / 10k-risk-review

workflow:
  says read latest 10-K Item 1, 1A, 7

data-mcp:
  sec_filing_browse
  sec_filing_read
  equity_historical_prices

output:
  risk evidence table
  MD&A readthrough
  data used
```

这就是 agentic finance 的正确结构。

不是：

```text
LLM 直接凭印象回答
```

而是：

```text
workflow chooses evidence
evidence supports answer
answer records data boundary
```

## 对 Pengyi Quant Research OS 的启发

我自己的 Research OS 应该学习 `skills` 的 router + workflow 结构。

可以设计成：

```text
pengyi-research-skills/
  factor-research/
    SKILL.md
    workflows/
      paper-to-factor.md
      factor-backtest-diagnosis.md
      factor-robustness-review.md

  paper-research/
    SKILL.md
    workflows/
      paper-to-knowledge-card.md
      paper-to-experiment-plan.md

  quant-rd/
    SKILL.md
    workflows/
      hypothesis-generation.md
      developer-implementation-plan.md
      bias-diagnosis.md
      pm-review.md
```

每个 workflow 都要固定包含：

```text
Use When
Data Needed
Freshness
Fallback
Workflow
Output Format
Guardrails
```

这会让我的 R&D Agent 从“想法驱动”变成“流程驱动”。

## 对 Quant R&D Agent 的直接迁移

我现在的 R&D Agent loop 是：

```text
Research Agent
  -> Developer Agent
  -> Runner
  -> Bias Diagnoser
  -> Loop Planner
  -> Human PM Reviewer
```

`skills` 给我的启发是：每个 agent role 都可以有自己的 router 和 workflow。

例如：

```text
Research Agent:
  paper-to-factor
  news-to-hypothesis
  factor-family-expansion

Developer Agent:
  factor-spec-to-code-plan
  data-pipeline-checklist
  test-plan-generation

Bias Diagnoser:
  lookahead-check
  survivorship-check
  data-snooping-check
  transaction-cost-check
  regime-robustness-check

PM Reviewer:
  approve-reject-iterate
  evidence-quality-score
  capital-readiness-gate
```

这比一个大 prompt 更稳定。

## 我想复用的 Patterns

| Pattern | 复用方式 |
|---|---|
| category folder as install unit | 每个 research domain 一个 folder |
| router `SKILL.md` | 先做 intent routing，不直接塞全部上下文 |
| workflow index | 所有 workflow 可审计、可导航 |
| data capabilities in natural language | 不把 workflow 绑死在某个 tool 名 |
| freshness rules | 所有实验和研究报告必须记录日期 |
| fallback rules | 缺数据时明确降级，而不是补幻觉 |
| structured output | 每个任务产出固定 report skeleton |
| guardrails | 明确 agent 不能做什么 |
| templates | 新 workflow 标准化创建 |
| plugin manifests | 同一套 skills 可在多个 agent host 使用 |

最重要的 takeaway：

```text
好的 agent 不是更会聊天，
而是更会进入正确工作流。
```

## 和 LLMQuant 后续项目的关系

`skills` 会连接后面的多个项目：

```text
QuantMind:
  turns papers/news/reports into structured knowledge

data-mcp:
  provides source-grounded data access

skills:
  routes finance task workflows

Magents:
  executes or simulates strategy / portfolio workflows

Research OS:
  records experiment contracts, artifacts, and diagnosis
```

因此它在系统里的位置是：

```text
knowledge + data
  -> workflow
  -> experiment
  -> report
```

这就是为什么我把 `skills` 放在 `LLMQUANT002`。

它是 `data-mcp` 之后最自然的一层。

## LLMQUANT002 后续实践

读完这个项目，我应该做三个动作。

### 1. 建立 Pengyi Workflow Template

基于 `templates/WORKFLOW_TEMPLATE.md`，做我自己的版本：

```text
Use When
Required Evidence
Optional Evidence
Freshness
Fallback
Workflow
Output Format
Bias / Risk Guardrails
Research OS Artifacts
```

这可以用于后续所有 factor research、paper research、quant memo。

### 2. 做一个 `paper-to-factor` workflow

这是我自己的核心任务：

```text
public finance paper
  -> structured knowledge
  -> factor hypothesis
  -> data requirement
  -> backtest protocol
  -> bias checklist
  -> PM review note
```

这个 workflow 应该成为 `Pengyi Quant R&D Agent` 的第一条主线。

### 3. 做一个 public-safe demo

可以选择一个公开 ticker，跑一个安全 workflow：

```text
10-K risk review
  -> equity research memo
  -> risk health check
```

它不涉及私密因子，也不需要非公开数据。

这适合放到网站上展示：

```text
source-grounded finance research workflow
```

## 当前结论

`skills` 是 LLMQuant 生态里非常核心的一层。

它解决的是：

```text
finance agent 如何从泛泛回答进入可重复工作流。
```

我的当前判断：

```text
data-mcp = evidence access layer
skills   = workflow routing layer
```

下一篇应该读：

```text
LLMQUANT003 -> quant-mind
```

因为有了数据访问和 workflow 路由之后，下一步就是：

```text
如何把论文、新闻、报告转成可复用的金融知识？
```

这就是 QuantMind 的位置。

## References

- LLMQuant Skills: <https://github.com/LLMQuant/skills>
- LLMQuant Data MCP: <https://github.com/LLMQuant/data-mcp>
- LLMQuant GitHub: <https://github.com/LLMQuant>
- Model Context Protocol: <https://modelcontextprotocol.io>
