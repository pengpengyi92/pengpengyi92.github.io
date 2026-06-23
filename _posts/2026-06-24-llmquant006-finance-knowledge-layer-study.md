---
title: "LLMQUANT006: finance knowledge layer 作为金融知识底座"
date: 2026-06-24 00:00:00 +0800
categories: [Learning, Quant Research]
tags: [pengyi-llmquant-studymap, llmquant006, finance-context, llmquant-book, quant-wiki, finance-knowledge, research-os]
---

这是 `PENGYI_LLMQUANT_STUDYMAP` 的第七篇。

```text
LLMQUANT006 -> docs + llmquant-book + quant-wiki.git
```

前面几篇已经把 LLMQuant 的几个系统层拆开了：

```text
LLMQUANT001 = data-mcp as evidence access layer
LLMQUANT002 = skills as finance workflow routing layer
LLMQUANT003 = quant-mind as financial knowledge structuring layer
LLMQUANT004 = Magents as strategy execution and simulation layer
LLMQUANT005 = awesome-trading-agents as ecosystem radar
```

这一篇看的是更底层、更长期的东西：

```text
finance knowledge layer = domain knowledge foundation for AI-native quant research
```

如果说 `data-mcp` 解决的是“agent 去哪里拿数据”，`skills` 解决的是“金融任务怎么执行”，`quant-mind` 解决的是“论文、新闻、报告怎么变成结构化知识”，那么 `docs`、`llmquant-book`、`quant-wiki.git` 解决的是：

```text
agent 和 human researcher 到底懂不懂金融语境？
概念、工作流、术语、案例、风险、行业实践有没有被沉淀下来？
每一次研究能不能站在一个稳定的知识底座上，而不是每次重新解释一遍？
```

这就是知识层的价值。

## Project snapshot

我本地看的三个项目是：

```text
LLMQUANT/docs
LLMQUANT/llmquant-book
LLMQUANT/quant-wiki.git
```

它们不是同一种资产。

| Asset | Local form | Main role | Scale I observed |
|---|---|---|---:|
| `docs` | Mintlify MDX docs | Finance Context, workflow encyclopedia, bilingual AI finance context | 230 MDX pages |
| `llmquant-book` | Markdown book + notebooks | AI + Quant textbook and learning path | 12 Markdown files, 2 notebooks |
| `quant-wiki.git` | bare Git mirror | Chinese quant wiki and long-term concept base | 754 files |

`docs` 也就是 Finance Context。它是一个开源金融知识与 AI 工作流百科，目标不是单纯解释名词，而是把 Wall Street 工作流、Finance 101、AI plugin / skill 使用方式放在一起。

`llmquant-book` 是一本线性教材，标题是《一本书读懂：人工智能时代的量化交易》。它更像学习路线，从 AI 在量化金融的趋势讲到数据、情绪分析、机器学习、实战不顺、极端事件、LLM agents。

`quant-wiki.git` 是 Quant Wiki 的 bare repo 镜像。它更像长期可检索的中文量化百科，覆盖基础金融、概率统计、量化概念、AI、论文、资源和进阶主题。

我的一句话总结是：

```text
Finance Context = workflow encyclopedia
llmquant-book = curriculum
Quant Wiki = searchable knowledge base
```

## Finance Context

Finance Context 的核心不是“文档站”，而是“金融工作流上下文”。

我本地统计到：

| Metric | Count |
|---|---:|
| total MDX pages | 230 |
| English pages | 115 |
| Chinese pages | 115 |
| command pages | 96 |
| skill pages | 110 |

它的结构非常规整：英文和中文页面数量完全对称，说明它不是随便堆内容，而是有意识地做 bilingual finance context。

核心模块包括：

| Module | What it covers |
|---|---|
| Financial Analysis | DCF, comps, LBO, three-statement model, competitive analysis |
| Equity Research | earnings review, initiating coverage, idea generation, catalyst tracking |
| Investment Banking | CIM, merger model, buyer list, pitch deck, deal tracking |
| Private Equity | deal screening, due diligence, IC memo, returns analysis, value creation |
| Wealth Management | client review, financial planning, rebalancing, tax-loss harvesting |
| LSEG Partner | bond RV, FX carry, swap curve, option volatility, macro rates |
| S&P Global Partner | tear sheets, funding digest, earnings preview |

这里最关键的是 `command` 和 `skill` 的区别。

```text
command = quick invocation
skill = detailed procedure and educational workflow
```

比如 DCF skill 不是只告诉你“DCF 是贴现现金流”，它会继续落到：

```text
revenue projection
free cash flow schedule
WACC
terminal value
sensitivity analysis
Excel formulas
source comments
validation checklist
common mistakes
how to add to local context
```

这对 Research OS 很重要。

因为一个真实的 R&D Agent 不应该只输出：

```text
I think this stock is undervalued.
```

它应该能输出：

```text
valuation method
input assumptions
formula lineage
source comments
risk checklist
scenario table
next research task
human PM review points
```

Finance Context 正好把这些专业工作流写成了可复用上下文。

## llmquant-book

`llmquant-book` 的角色不同。

它不是工具层，也不是工作流索引，而是课程层。

README 里给出的主线是：

```text
AI in quantitative finance
data as the blood of quant trading + AI
sentiment analysis as factor
machine learning in quant trading
why quant practice often fails
robustness under extreme events
LLM agents in quant finance
```

这对我很有启发，因为它不是只讲 agent，也不是只讲 backtest，而是把 AI + Quant 放在一条学习曲线上：

```text
data
feature
sentiment / event
model
backtest
risk
robustness
agent
deployment imagination
```

第二章的重点是数据：结构化数据、非结构化数据、另类数据、多模态数据融合。

第七章的重点是 LLM agents：单智能体、多智能体、工具调用、记忆、规划、风险、伦理、系统性金融风险、因果推理、多智能体协同。

这正好和我们要做的 R&D Agent 对上：

```text
research idea
  -> data source
  -> feature engineering
  -> hypothesis
  -> implementation
  -> backtest
  -> robustness
  -> PM review
  -> next iteration
```

`llmquant-book` 的价值不在于“可以直接拿来跑”，而在于它给 Research OS 提供了学习路线和概念顺序。

我会把它放在：

```text
Pengyi Quant Research OS / onboarding curriculum
```

也就是每一个子系统都应该能回答：

```text
这个模块对应书里哪一章？
它解决的是数据、特征、模型、回测、风控、agent 还是部署问题？
它的失败模式在书里有没有被讲到？
```

## Quant Wiki

`quant-wiki.git` 是一个 bare Git repo，不是普通 working tree。

这意味着本地不能直接像普通项目一样编辑文件夹，但可以用 Git plumbing 读取内容：

```text
git --git-dir=quant-wiki.git show HEAD:README.md
git --git-dir=quant-wiki.git ls-tree -r HEAD
```

我观察到的规模是：

| Metric | Count |
|---|---:|
| total files | 754 |
| Markdown files | 130 |
| PNG assets | 218 |
| Python files | 60 |
| YAML files | 7 |

主目录包括：

```text
advanced
ai
basic
industry
job
library
llmquant_resources
paper
repo
start
```

这说明 Quant Wiki 的定位比 Finance Context 更宽。

它覆盖：

```text
basic finance concepts
probability and statistics
quant concepts
AI for quant
paper tracking
library resources
career and job materials
advanced research topics
```

README 里讲得很清楚：

```text
We are committed to the open-sourcing and localization of quantitative knowledge,
aiming to bridge the information gap between the domestic and international quantitative finance industries.
```

我对 Quant Wiki 的理解是：

```text
Quant Wiki = Chinese quant knowledge memory
```

它的价值不是某一个页面，而是长期可维护、可检索、可贡献的知识底座。

对 Research OS 来说，它可以变成：

```text
glossary source
concept retrieval source
paper pointer source
basic finance / probability grounding source
RAG corpus
PM review reference
```

## Three forms of knowledge

这三个项目给我的最大启发是：金融知识不是一种形态，而是至少三种形态。

| Form | Project | Best use |
|---|---|---|
| Workflow knowledge | Finance Context | Tell an agent how to perform a real finance task |
| Curriculum knowledge | llmquant-book | Teach a human or agent the ordered learning path |
| Reference knowledge | Quant Wiki | Provide searchable concepts, terms, papers, and background |

如果只做 workflow，没有 curriculum，系统会变成能做任务但不懂大图。

如果只做 curriculum，没有 workflow，系统会变成懂理论但落不到具体动作。

如果只做 reference，没有结构化调用，系统会变成百科堆料，不能进入研究闭环。

所以它们要合起来看：

```text
Quant Wiki gives concepts.
llmquant-book gives learning path.
Finance Context gives professional workflows.
QuantMind structures external documents.
data-mcp retrieves evidence.
skills routes tasks.
Magents simulates strategies.
awesome-trading-agents tells us who else is building.
```

这才像一个真正的 AI-native Quant Research OS。

## Into Pengyi Research OS

我会把 `LLMQUANT006` 抽象成一个模块：

```text
Pengyi Finance Knowledge Layer
```

它的目标不是“收藏资料”，而是让资料可以进入系统。

最小 schema 可以是：

```yaml
KnowledgeSource:
  source: finance-context | llmquant-book | quant-wiki
  path: string
  language: en | zh
  domain: equity_research | financial_analysis | quant | ai | risk | macro | other
  artifact_type: command | skill | chapter | wiki_page | paper_note
  summary: string
  key_terms: string[]
  workflow_steps: string[]
  research_use: string
  license: string
```

进入系统后的用途是：

```text
factor hypothesis grounding
workflow template retrieval
domain term disambiguation
PM review checklist generation
backtest report explanation
paper-to-strategy bridge
public-safe blog writing
RA / PhD research narrative support
```

一个具体例子：

```text
user asks: generate equity long-short idea from AI infrastructure theme

Research OS should retrieve:
  Finance Context -> idea generation skill
  Finance Context -> DCF or comp model skill
  llmquant-book -> structured / unstructured / alternative data chapter
  Quant Wiki -> valuation, factor model, market efficiency, risk concepts
  QuantMind -> recent papers / reports / news converted to structured knowledge
  data-mcp -> market and fundamental evidence
  Magents -> backtest or simulation
```

这才是“知识进入生产力”。

## PR opportunities

这三个项目也有一些可以贡献的方向。

| Project | Possible contribution |
|---|---|
| Finance Context | add missing examples, improve bilingual consistency, add China-market cases, refine command / skill cross-links |
| llmquant-book | fix README URL inconsistency if confirmed, improve chapter navigation, add notebooks tied to chapters |
| Quant Wiki | add concept cards, improve topic taxonomy, add ingestion-friendly metadata, build RAG export script |

我观察到一个小细节：`llmquant-book` README 里出现了两个在线阅读地址写法，分别是 `https://llmquant.github.io/llmquant-book/` 和 `https://llmquant.github.io/Book/`。如果上游确认其中一个是旧链接，这就是一个很小但合适的 PR 点。

另一个细节是 Finance Context README 提到 Mintlify 配置时使用了 `mint.json` 的说法，但本地实际配置文件是 `docs.json`。这也可能是文档 stale 的地方，适合后续确认后再提 issue 或 PR。

## Watch points

这些资料不能无脑搬进自己的项目。

第一，license 要看清楚。Finance Context 和 Quant Wiki 都使用 CC BY-NC-SA 4.0，这意味着非商业、署名、相同方式共享等条件需要遵守。做公开学习笔记可以引用和总结，但如果未来做商业化系统，需要重新审视授权边界。

第二，知识不是实时数据。百科、教材和 workflow 文档适合做 grounding，不适合直接替代最新市场数据、法规、财报和价格。

第三，agent 需要 source discipline。金融场景里最危险的是“听起来对”。知识层必须和数据层、证据层、时间戳绑定，否则会让 agent 变得更会编故事。

第四，Quant Wiki 是 bare repo 镜像，路径中有中文文件名和 Git 转义，后续做 ingestion 时要用 Git 命令稳定读取，不要用脆弱的路径字符串处理。

## My takeaway

`LLMQUANT006` 让我更确定一件事：

```text
AI-native quant research 不是只堆 agent。
真正有用的 agent 背后，需要金融知识层、证据层、工作流层、实验层、诊断层和人类 PM 审核层。
```

Finance Context、llmquant-book、Quant Wiki 共同提供了这个知识层。

对我来说，这一层的意义非常直接：

```text
让自己学得更快。
让 agent 少胡说。
让研究流程更像真实金融机构。
让公开输出更有体系。
让未来的 Quant R&D Agent 有可检索、可复用、可审计的金融知识底座。
```

下一篇先做第一阶段总复盘，把 GitHub 上的 LLMQuant public repos 和我们已经写过的 000-006 做 coverage matrix：

```text
LLMQUANT007 -> LLMQuant ecosystem coverage matrix
```
