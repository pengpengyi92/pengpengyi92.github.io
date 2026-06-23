---
title: "LLMQUANT003: QuantMind 作为金融知识结构化层"
date: 2026-06-24 00:00:00 +0800
categories: [Learning, Quant Research]
tags: [pengyi-llmquant-studymap, llmquant003, quantmind, knowledge-graph, rag, research-os]
---

这是 `PENGYI_LLMQUANT_STUDYMAP` 的第四篇。

```text
LLMQUANT003 -> quant-mind
```

前面三篇的定位是：

```text
LLMQUANT000 = LLMQuant project map
LLMQUANT001 = data-mcp as evidence access layer
LLMQUANT002 = skills as finance workflow routing layer
```

这一篇研究 `quant-mind`。

我的一句话结论：

```text
quant-mind = financial knowledge structuring layer
```

更直接地说：

```text
data-mcp 解决 evidence 从哪里来
skills 解决 task 应该怎么做
quant-mind 解决 evidence 如何变成可复用的 research memory
```

这对我的 `Pengyi Quant Research OS` 非常关键。

量化研究不是只把 PDF、新闻、博客、研报丢给 LLM 总结。
真正有价值的是把这些非结构化材料转成稳定的、可引用的、可检索的、可组合的知识对象。

```text
paper / news / blog / report / filing
  -> structured knowledge
  -> retrieval
  -> factor hypothesis
  -> implementation plan
  -> backtest protocol
  -> bias diagnosis
  -> next research plan
```

`quant-mind` 就是在补这个中间层。

## Project snapshot

我本地看的项目是 `LLMQUANT/quant-mind`。

当前项目关键信息：

| Item | Value |
|---|---|
| package | `quantmind` |
| version | `0.2.0` |
| Python | `>=3.10` |
| core runtime | OpenAI Agents SDK |
| schema system | Pydantic v2 |
| PDF parser | PyMuPDF |
| HTML parser | trafilatura |
| fetch layer | httpx, arxiv |
| local test files | 33 |
| current status | migrating into an OpenAI Agents SDK based domain library |

这里最重要的不是某个单点功能，而是项目方向：

```text
QuantMind is not rebuilding a custom agent framework.
QuantMind is becoming a finance-domain knowledge library on top of OpenAI Agents SDK.
```

这点很关键。

很多 agent 项目容易越写越重：

```text
custom agent runtime
custom tools protocol
custom memory
custom storage
custom orchestration
custom plugin registry
```

`quant-mind` 现在的路线更清晰：

```text
use OpenAI Agents SDK for agent execution
keep quantmind focused on domain schemas, preprocessing, flows, and knowledge objects
```

这对工程很重要。
因为一个研究系统要长期演进，不能把基础设施和领域逻辑全部搅在一起。

## Two-stage architecture

从 README 的定位看，`quant-mind` 有两个阶段：

```text
Stage 1: Knowledge Extraction
Stage 2: Intelligent Retrieval
```

我把它翻译成 Research OS 里的语言：

```text
Stage 1:
raw material -> structured knowledge artifact

Stage 2:
structured knowledge artifact -> retrieval / memory / RAG / hypothesis generation
```

更工程化地看：

```text
source APIs / local files / web pages / papers
  -> fetch
  -> format
  -> clean
  -> flow
  -> OpenAI Agents SDK extractor
  -> Pydantic knowledge object
  -> future memory / store / retrieval
```

当前项目重点已经把第一阶段的主干打出来了。
第二阶段的 `mind`、`memory`、store、retrieval 还在后续 PR 的路线里。

这意味着现在看 `quant-mind`，要分清：

```text
已经落地的：
- knowledge schema
- preprocessing
- paper flow
- batch runner
- magic input resolver
- architecture contracts

正在形成的：
- memory layer
- store layer
- retrieval layer
- graph knowledge final shape
```

## Package structure

当前目标结构大致是：

```text
quantmind/
  configs/
  flows/
  knowledge/
  preprocess/
  mind/
  magic.py
  utils/
```

每一层的职责很清楚：

| Layer | Responsibility |
|---|---|
| `preprocess` | fetch, parse, format, clean raw material |
| `knowledge` | define durable financial knowledge schemas |
| `configs` | define flow input and runtime configuration |
| `flows` | compose end-to-end extraction pipelines |
| `magic.py` | turn natural language into typed input and config |
| `mind` | future cognitive and memory layer |
| `utils` | leaf utilities |

我最喜欢的是它开始用 import-linter 维护架构边界。

这说明项目不是只在堆代码。
它在定义层级关系：

```text
knowledge should be a leaf layer
utils should be a leaf layer
configs can depend on knowledge
preprocess can depend on utils
flows and magic live at the apex
deleted transitional packages should not come back
```

这对我们后面做自己的 Research OS 也有启发。

系统不是写出来就结束。
系统要有边界。

## Core idea: knowledge object

`quant-mind` 最核心的资产是 `knowledge/`。

因为它决定了：

```text
什么东西可以被保存
什么东西可以被检索
什么东西可以被引用
什么东西可以进入下一轮研究
```

这里的根类是 `BaseKnowledge`。

它不是一个普通 data class。
它定义了金融知识对象的最小合约。

| Field / Method | Meaning |
|---|---|
| `id` | unique artifact id |
| `item_type` | knowledge type |
| `as_of` | the time this knowledge is valid for |
| `created_at` | creation time |
| `source` | where the evidence came from |
| `extraction` | how the artifact was extracted |
| `confidence` | confidence level |
| `citations` | evidence anchors |
| `tags` | retrieval and organization labels |
| `disclaimers` | caveats |
| `embedding_text()` | canonical text used for embedding |

我认为这里最重要的是三个字段：

```text
as_of
source
citations
```

金融知识和普通知识不一样。

普通知识可以说：

```text
this company has product X
```

金融知识必须问：

```text
as of when?
from which source?
can we cite the sentence, page, node, or offset?
```

这是 `BaseKnowledge` 的价值。

它把金融知识天然变成：

```text
time-aware
source-aware
citation-aware
retrieval-ready
```

## Provenance design

`SourceRef` 记录证据来源。

它支持：

```text
arxiv
http
doi
local
rss
transcript
manual
```

关键字段包括：

```text
kind
uri
fetched_at
content_hash
```

`ExtractionRef` 记录提取过程：

```text
flow
model
run_id
extracted_at
```

`Citation` 记录引用锚点：

```text
source_id
page
char_offset
quote
tree_id
node_id
```

这一套 provenance 设计，对于量化研究非常重要。

因为我们未来的 R&D Agent 不能只是说：

```text
我觉得这个因子有 alpha。
```

它必须能说：

```text
这个 hypothesis 来自哪篇 paper
哪一段描述了 methodology
哪一段描述了 limitation
它适用于哪个 asset class
as_of 是什么时候
提取模型是什么
后续 backtest 应该验证哪个 claim
```

这才是可以进入研究流水线的 artifact。

## Three knowledge shapes

`quant-mind` 当前把 knowledge 分成三种形状：

```text
FlattenKnowledge
TreeKnowledge
GraphKnowledge
```

这三个形状非常关键。

### FlattenKnowledge

`FlattenKnowledge` 是原子卡片。

适合这种对象：

```text
News
Earnings
Factor
Thesis
PaperKnowledgeCard
```

特点是：

```text
one item
one compact embedding text
easy to retrieve
easy to rank
easy to put into dashboard
```

比如一个新闻事件：

```text
headline
event_type
timestamp
entities
sentiment
materiality
```

比如一个 factor hypothesis：

```text
factor_name
universe
source
as_of
citations
```

这适合做检索卡片，也适合进入 PM review。

### TreeKnowledge

`TreeKnowledge` 是层级文档。

适合完整 paper、filing、transcript、long report。

它的核心是 `TreeNode`：

```text
node_id
title
summary
content
citations
children_ids
```

一篇 paper 可以被组织成：

```text
Paper
  -> Abstract
  -> Introduction
  -> Methodology
  -> Data
  -> Experiments
  -> Results
  -> Limitations
  -> Trading implications
```

这比单纯 summary 强很多。

因为对研究来说，很多时候我们不是只要摘要。
我们要定位：

```text
methodology 在哪里
data assumption 在哪里
样本期在哪里
limitation 在哪里
是否存在 look-ahead bias
是否可以转成 factor
```

`TreeKnowledge` 就是为了保留这种结构。

### GraphKnowledge

`GraphKnowledge` 目前还是 placeholder。

项目里甚至明确测试了：

```text
the class exists for type hints
subclassing is currently blocked
```

这说明作者暂时没有急着把 graph schema 定死。

我认为这是对的。

金融 graph 一旦定错，后面很难改。
未来可能出现的 graph 有：

```text
paper citation graph
factor lineage graph
news-entity-event graph
company-supply-chain graph
macro-variable-causal graph
strategy-dependency graph
```

这些 graph 的边类型和节点类型差异很大。
先把 `FlattenKnowledge` 和 `TreeKnowledge` 打稳，再定 graph，是更稳的路线。

## Concrete schemas

当前已经能看到这些具体 schema：

| Schema | Shape | Use |
|---|---|---|
| `Paper` | TreeKnowledge | full paper structure |
| `PaperKnowledgeCard` | FlattenKnowledge | compact paper insight card |
| `News` | FlattenKnowledge | event and market news |
| `Earnings` | FlattenKnowledge | earnings related artifact |
| `Factor` | FlattenKnowledge | factor hypothesis artifact |
| `Thesis` | FlattenKnowledge | research claim |

这里 `Paper` 和 `PaperKnowledgeCard` 的组合很重要。

我理解它们是两层：

```text
Paper = full structured document
PaperKnowledgeCard = extracted compact research card
```

这对应我们的实际工作流：

```text
先完整理解 paper
再抽取可复用 research insight
再转成 factor hypothesis
再进入 backtest
```

如果只有 `PaperKnowledgeCard`，信息可能太薄。
如果只有 `Paper`，检索和筛选可能太重。

两者并存是合理的。

## Paper flow

`flows/paper.py` 是当前最核心的 end-to-end pipeline。

它的输入是 `PaperInput` union。

支持：

```text
ArxivIdentifier
HttpUrl
LocalFilePath
RawText
DoiIdentifier
```

当前 DOI 分支还没有完整实现。
测试里也明确期望 DOI raise `NotImplementedError`。

这不是问题。
这是项目边界清晰。

主流程可以概括为：

```text
PaperInput
  -> _fetch_and_format
  -> markdown text + metadata
  -> _format_input
  -> Agent(
       name="paper_extractor",
       instructions=...,
       model=cfg.model,
       tools=...,
       output_type=Paper
     )
  -> run_with_observability
  -> Paper
```

这里关键点有三个。

第一，输入是 typed。

不是传一个随意字符串。

```text
ArxivIdentifier(id="2604.12345")
HttpUrl(url="...")
LocalFilePath(path=...)
RawText(text="...")
```

第二，输出是 typed。

`output_type=Paper` 意味着 agent 的最终产物不是自由文本，而是 Pydantic knowledge object。

第三，flow 暴露了扩展点。

```text
extra_tools
extra_instructions
output_type
memory
extra_run_hooks
input_guardrails
output_guardrails
```

这说明它不是死 pipeline。
它是一个可被研究系统组合的 extraction primitive。

## Preprocess layer

`preprocess/` 负责把原始材料变成 agent 能处理的文本。

当前主要分三层：

```text
fetch
format
clean
```

`fetch` 层负责获取原始 bytes 和 metadata：

```text
fetch_arxiv
fetch_url
read_local_file
fetch_doi
```

`format` 层负责把不同内容类型转成 markdown 或 plain text：

```text
pdf_to_text
pdf_to_markdown
html_to_markdown
```

`clean` 层负责基础清理：

```text
normalize_unicode
collapse_whitespace
dedupe_lines
```

这里的设计也有一个很值得学的点：

```text
Pydantic at boundaries
frozen dataclass internally
```

也就是说，面向外部的输入输出用 Pydantic 保证 schema。
内部中间态用 frozen dataclass 保证轻量、不可变、易测试。

这比全部用 dict 稳定很多。

## Config layer

`BaseFlowCfg` 是所有 flow 的基础配置。

它覆盖了这些维度：

```text
model
model_settings
max_turns
timeout_seconds
output_dir
overwrite
memory_dir
workflow_name
trace_id
trace_metadata
archive_trajectory
max_input_tokens
max_output_tokens
max_cost_usd
enable_default_guardrails
```

这其实就是一个研究 pipeline 应该有的 runtime contract。

很多 demo agent 没有这些字段，所以看起来能跑，但很难进入真实研究生产。

真实研究需要：

```text
traceability
cost control
timeout control
memory location
output location
guardrails
```

`PaperFlowCfg` 又在基础配置上增加 paper extraction 的选项：

```text
extract_methodology
extract_limitations
asset_class_hint
```

这说明 flow config 不只是技术配置。
它也包含 domain extraction intent。

## Batch runner

`flows/batch.py` 提供 `batch_run`。

这是非常实际的功能。

研究中很少只看一篇 paper。
我们经常会做：

```text
20 papers about momentum
50 papers about analyst revision
100 news articles about supply chain shocks
all filings for a specific sector
```

`batch_run` 支持：

```text
concurrency
on_error = skip / raise
on_progress
cfg forwarding
extra kwargs forwarding
success and failure summary
tokens_total
cost_estimate_usd
```

我特别注意到一点：

```text
batch_run rejects memory= in MVP
```

原因是避免 batch 并发时产生 cross-run memory race。

这是工程判断。

很多系统会先把功能做满，然后留下隐性状态污染。
这里选择先拒绝，是更好的默认。

## Observability runner

`flows/_runner.py` 包了一层 `run_with_observability`。

它做的是：

```text
compose hooks
pass RunConfig
attach tracing metadata
respect max_turns
prepare artifact archiving path
```

当前 `_archive_run_artifacts` 还是 no-op。
但位置已经留出来。

这对 Research OS 很重要。

未来每一次 extraction 都应该能留下：

```text
input
output
model
trace
tokens
cost
citations
warnings
run artifacts
```

没有 observability，就没有可复盘研究。

## Magic input

`magic.py` 是一个很有意思的层。

它的目标是：

```text
natural language -> typed input + typed config
```

比如人说：

```text
fetch arxiv 2604.12345 about momentum, extract methodology and limitations
```

`resolve_magic_input` 会根据目标 flow 的签名，解析出：

```text
input_obj = ArxivIdentifier(id="2604.12345")
cfg_obj = PaperFlowCfg(...)
```

它做了几件事：

```text
introspect target flow signature
render input schema
render config schema
call lightweight resolver agent
return typed object pair
```

这个层的意义不是炫技。

它是在连接：

```text
human research intent
  -> typed executable flow
```

对我们自己的系统来说，这很有用。

我们可以想象：

```text
"帮我看 20 篇最近的 cross-sectional momentum paper，抽出可测试因子假设"
  -> PaperInput batch
  -> PaperFlowCfg
  -> Paper objects
  -> PaperKnowledgeCards
  -> Factor hypotheses
```

这就是自然语言调度研究系统。

## Architecture discipline

我觉得 `quant-mind` 最值得学习的，不只是功能，而是工程约束。

它的开发指导很明确：

```text
do not rebuild an agent runtime
do not resurrect deleted transitional packages
prefer functions over unnecessary classes
use Pydantic at boundaries
use frozen dataclass internally
implement tests for new features
comments in English Google style
no meaningless wrappers
```

这对我们非常有启发。

如果我们做 `Pengyi Quant Research OS`，也应该明确：

```text
哪些东西是 domain layer
哪些东西是 orchestration layer
哪些东西是 storage layer
哪些东西是 UI layer
哪些东西不能互相依赖
```

否则系统很快会变成一团。

## Relationship with data-mcp and skills

现在把 001、002、003 串起来。

```text
data-mcp
  = evidence access layer

skills
  = finance workflow routing layer

quant-mind
  = financial knowledge structuring layer
```

三者组合以后，是一条完整链路：

```text
research intent
  -> skills router
  -> data-mcp tools
  -> raw evidence
  -> quant-mind preprocessing
  -> quant-mind flow
  -> structured knowledge
  -> retrieval / memory
  -> next research action
```

这就是 agentic quant research 的骨架。

也就是说：

```text
skills 决定做什么
data-mcp 负责拿证据
quant-mind 负责把证据沉淀为知识
Research OS 负责把知识变成项目资产
R&D Agent 负责不断提出下一轮研究
```

## Relationship with X2Strategy

之前我把 `quant-mind` 和 `X2Strategy` 做过对比。

现在看得更清楚：

```text
quant-mind is knowledge-first
X2Strategy is strategy-generation-first
```

`quant-mind` 更像：

```text
paper / news / report
  -> structured knowledge
  -> reusable memory
```

`X2Strategy` 更像：

```text
paper / idea
  -> trading strategy
  -> implementation / backtest direction
```

两者不是冲突关系。

它们可以串起来：

```text
quant-mind extracts and stores research knowledge
X2Strategy-style module turns selected knowledge into strategy candidates
```

对我们来说，更合理的路线是：

```text
first build knowledge quality
then build strategy generation
then build backtest and diagnosis
```

否则 strategy generation 会缺少稳定证据地基。

## Where QuantMind fits in Pengyi Research OS

我会把 `quant-mind` 放在 Research OS 的中间层。

```text
                 Human PM
                    |
                    v
Research Intent -> Workflow Router -> Evidence Access -> Knowledge Structuring
                                                            |
                                                            v
                                                     Research Memory
                                                            |
                                                            v
Factor Hypothesis -> Implementation -> Backtest -> Bias Diagnosis -> Next Plan
```

具体到 paper-to-factor：

```text
1. choose paper
2. fetch paper
3. parse paper into markdown
4. extract Paper TreeKnowledge
5. derive PaperKnowledgeCard
6. derive Factor hypothesis
7. define universe and data requirement
8. implement factor
9. run backtest
10. diagnose bias
11. generate next research plan
12. human PM review
```

这和我们之前说的 R&D Agent 是一致的：

```text
自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ 人类 PM 审核
```

`quant-mind` 负责的是前两步之间最关键的桥：

```text
raw research material -> auditable knowledge artifact -> factor hypothesis source
```

## Strengths

我认为 `quant-mind` 当前最强的地方有七个。

第一，知识对象有时间和来源。

```text
as_of + source + citations
```

这对金融很核心。

第二，knowledge shape 分层合理。

```text
FlattenKnowledge for cards
TreeKnowledge for long documents
GraphKnowledge reserved for future graph semantics
```

第三，flow 输出是 Pydantic 对象。

这比纯文本 summary 更适合工程系统。

第四，它没有继续造自定义 agent framework。

使用 OpenAI Agents SDK，把精力放在金融领域层。

第五，preprocess 层拆得清楚。

```text
fetch / format / clean
```

第六，batch runner 考虑了真实研究负载。

```text
concurrency
error handling
progress
cost placeholder
```

第七，架构边界有 import-linter 约束。

这说明项目有长期维护意识。

## Watch points

当前也有一些需要注意的边界。

第一，retrieval/store/memory 还没有完全落地。

所以现在的 `quant-mind` 更像 extraction and schema layer，不是完整知识库产品。

第二，`GraphKnowledge` 还只是 placeholder。

这很正常，但如果我们想做 factor lineage 或 causal graph，需要自己继续设计。

第三，`Factor` 和 `Thesis` 还是较轻的 stub。

如果我们要做真正的因子研究，需要扩展：

```text
formula
universe
rebalance frequency
data fields
neutralization
expected direction
risk notes
backtest protocol
bias checks
```

第四，PDF parsing 目前偏基础。

PyMuPDF 足够做 baseline，但复杂论文的 table、formula、figure、appendix 可能需要更强 parser。

第五，DOI resolver 还没完全实现。

这影响 paper sourcing 的覆盖。

第六，LLM extraction quality 需要评估集。

有 schema 不等于提取一定正确。
需要 fixtures、golden samples、manual review。

## What we should learn

对我自己的系统，`quant-mind` 给了几个直接启发。

### 1. 先定 artifact contract

不要一开始就追求复杂 agent。

先问：

```text
最终要留下什么 artifact?
artifact 必须有什么字段?
artifact 如何被引用?
artifact 如何被检索?
artifact 如何进入下一轮研究?
```

这比先写 prompt 更重要。

### 2. Research memory must be typed

我们的研究记忆不能只是 markdown note。

应该是：

```text
Markdown for human reading
Pydantic object for machine reuse
JSONL / database row for persistence
embedding text for retrieval
citations for audit
```

### 3. Paper extraction should become a primitive

未来我的 Research OS 里，`paper_flow` 应该是基础能力。

```text
read paper
structure paper
extract methodology
extract data assumption
extract limitation
extract factor hypothesis
generate implementation TODO
```

### 4. Batch paper reading matters

单篇论文总结不够。

真正有价值的是：

```text
batch read 20 papers
cluster ideas
find repeated factor families
compare data assumptions
rank implementability
select top hypotheses
```

`batch_run` 就是这个方向的开端。

### 5. Human PM review is still necessary

`quant-mind` 可以把材料结构化。
但是否值得做、是否有交易价值、是否适合当前数据条件，仍然需要 PM 判断。

所以我自己的系统应该保留：

```text
AI proposal
human PM review
decision log
next action
```

这也是我们一直说的：

```text
human-in-the-loop is not weakness
it is research governance
```

## Pengyi implementation plan

基于 `quant-mind`，我下一步会给自己的 Research OS 定一个 paper-to-factor MVP。

```text
Input:
  arxiv id / pdf / URL / raw text

Process:
  fetch
  format
  clean
  extract Paper
  derive PaperKnowledgeCard
  derive FactorHypothesis
  write markdown report
  save JSON artifact

Output:
  human-readable study note
  machine-readable knowledge object
  next research checklist
```

`FactorHypothesis` 我会考虑包含：

```text
name
claim
economic intuition
asset class
universe
required data
feature formula
expected direction
holding period
rebalance frequency
risk controls
known limitations
backtest checklist
citations
```

这样它才能进入下一层：

```text
implementation agent
backtest agent
bias diagnosis agent
next-plan agent
```

## One useful mental model

可以把 `quant-mind` 理解为金融研究里的 compiler front-end。

```text
raw paper/news/report = source code
preprocess = lexer/parser
knowledge schema = AST
retrieval/memory = index
R&D agent = optimizer
strategy implementation = codegen
backtest = runtime
PM review = human governance
```

这个类比很有用。

如果 AST 质量差，后面的 optimizer 和 codegen 都会不稳定。

所以 `quant-mind` 这种 knowledge structuring layer，不是边缘组件。
它是 AI quant research system 的核心地基之一。

## LLMQUANT003 conclusion

`quant-mind` 对我的启发是：

```text
AI quant research 的核心不是让 LLM 多说几句总结。
核心是把研究材料转成可审计、可检索、可组合、可继续执行的知识对象。
```

它和前面两个项目组合起来，形成了很清晰的路线：

```text
data-mcp -> get evidence
skills -> choose workflow
quant-mind -> structure knowledge
Research OS -> persist and govern research artifacts
R&D Agent -> iterate hypotheses and experiments
```

这就是我现在要学透 LLMQuant 的原因。

我们不是只学习某个 repo。
我们是在学习怎样把 AI-native finance research system 一层一层搭出来。

下一篇：

```text
LLMQUANT004 -> Magents
```

重点会看 multi-agent trading simulation、strategy/backtest orchestration，以及它和 `quant-mind` 的衔接方式。
