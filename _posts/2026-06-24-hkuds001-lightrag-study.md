---
title: "HKUDS001: LightRAG 作为知识图谱 RAG 与 Research Memory 基建"
date: 2026-06-24 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds001, hkuds, lightrag, rag, knowledge-graph, research-memory, ai-infrastructure]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第二篇。

```text
HKUDS001 -> LightRAG
```

如果 `HKUDS000` 是地图，那么 `HKUDS001` 就是开始拆第一个核心系统。
这篇要细致讲清楚：

```text
LightRAG 到底解决什么问题
它和普通 RAG / GraphRAG 的关系是什么
它的代码结构如何组织
文档如何进入系统
知识图谱和向量库如何一起工作
查询模式如何选择
它怎么变成 Pengyi Research OS 的 research memory layer
```

我的一句话定位是：

```text
LightRAG = graph-based RAG infrastructure for source-grounded research memory.
```

也就是说，它不是一个普通的“把文档塞进向量库然后问答”的 demo。
它更像一个可以持续吸收文档、抽取实体关系、构建知识图谱、保留引用来源、支持多种检索模式的知识记忆系统。

对我们来说，它对应的是：

```text
Pengyi Research OS
  -> Research Memory Layer
  -> source-grounded knowledge base
  -> papers / notes / code / reports / CV / PS / RP 的统一记忆系统
```

## Local snapshot

我这次读的是本地 HKUDS 工作区里的 `LightRAG` 快照。

本地 git 状态：

| Item | Value |
|---|---|
| repo | `LightRAG` |
| branch | `main` |
| status | clean |
| local head | `4e1f952` |
| latest local commit title | `Merge pull request #3295 from HKUDS/refactor/docx-parser-no-chunking` |

本地规模：

| Metric | Count |
|---|---:|
| total files | 683 |
| Markdown files | 48 |
| Python files | 406 |
| TypeScript files | 100 |
| JSON files | 23 |

这已经是一个完整工程，而不是一个小脚本。

关键目录：

```text
LightRAG/
  lightrag/
    api/
    chunker/
    evaluation/
    kg/
    llm/
    parser/
    sidecar/
    tools/
    lightrag.py
    pipeline.py
    operate.py
    base.py
    utils.py
    prompt.py
  lightrag_webui/
  docs/
  examples/
  prompts/
```

核心代码文件可以先这样理解：

| File / directory | Role |
|---|---|
| `lightrag/lightrag.py` | `LightRAG` 主 facade，负责初始化、存储绑定、insert、query、delete、custom chunks / custom KG |
| `lightrag/pipeline.py` | 文档进入系统后的处理 pipeline：enqueue、parse、chunk、extract、status tracking |
| `lightrag/operate.py` | 实际的实体关系抽取、图检索、向量检索、上下文构造、query mode 逻辑 |
| `lightrag/base.py` | 抽象接口和数据结构：`QueryParam`、`BaseKVStorage`、`BaseVectorStorage`、`BaseGraphStorage`、`DocStatusStorage` |
| `lightrag/kg/` | 存储后端：JSON、NetworkX、NanoVectorDB、Postgres、Neo4j、Mongo、Redis、Qdrant、Milvus、OpenSearch 等 |
| `lightrag/llm/` | LLM / embedding provider 绑定 |
| `lightrag/parser/` | 文件解析、parser routing、文档格式转换 |
| `lightrag/chunker/` | chunking 策略 |
| `lightrag/api/` | FastAPI server、documents/query/graph routes、Ollama-compatible API |
| `lightrag_webui/` | 前端 WebUI |

## The problem

普通 RAG 的基本链路是：

```text
document -> chunks -> embeddings -> vector search -> LLM answer
```

这个架构简单，但是有几个硬问题：

| Problem | Why it matters |
|---|---|
| context 被切碎 | 一个知识点可能散落在多个 chunk 里，单个 chunk 召回后缺少上下文 |
| global reasoning 弱 | 只靠相似度召回，很难回答跨文档、跨主题、跨实体关系的问题 |
| relation 不显式 | 文档里的人、公司、方法、实验、数据集之间的关系没有结构化保存 |
| 可解释性弱 | 答案可能有 chunk 引用，但很难看到背后的概念图谱 |
| 增量维护复杂 | 文件更新、删除、重建、状态追踪都容易变成临时脚本 |

LightRAG 的核心思路是把普通向量 RAG 和知识图谱 RAG 结合：

```text
documents
  -> chunks
  -> entities
  -> relationships
  -> knowledge graph
  -> vector embeddings
  -> local / global / hybrid / naive / mix retrieval
  -> source-grounded answer
```

所以它真正的价值不只是“问文档”，而是：

```text
把非结构化知识变成可检索、可连接、可引用、可持续维护的 research memory.
```

## Architecture

LightRAG 可以拆成五层。

```text
Layer 1: document ingestion
Layer 2: parsing and chunking
Layer 3: entity / relation extraction
Layer 4: storage layer
Layer 5: query and generation
```

更具体一点：

```text
files / text / docs
  -> parser: legacy / native / mineru / docling
  -> LightRAG Document + sidecars
  -> chunker: F / R / V / P
  -> EXTRACT LLM extracts entities and relationships
  -> graph storage stores entity-relation graph
  -> vector storage stores chunks/entities/relationships embeddings
  -> KV storage stores full docs, chunks, caches, metadata
  -> KEYWORD LLM extracts high-level / low-level query keywords
  -> retrieval: local / global / hybrid / naive / mix
  -> rerank, token budget, context construction
  -> QUERY LLM produces grounded answer with references
```

这个架构的关键点是：

```text
KG is not an add-on.
KG is part of the retrieval substrate.
```

知识图谱不是最后画图给人看，而是直接参与召回和上下文构造。

## Main facade

`lightrag/lightrag.py` 里的核心类是：

```python
@dataclass
class LightRAG(_RoleLLMMixin, _StorageMigrationMixin, _PipelineMixin):
    """LightRAG: Simple and Fast Retrieval-Augmented Generation."""
```

这个类把三个方向组合起来：

| Mixin / class | Meaning |
|---|---|
| `LightRAG` | 主入口，暴露用户真正调用的 insert/query/delete 等方法 |
| `_PipelineMixin` | 文档处理 pipeline 能力 |
| `_RoleLLMMixin` | 不同阶段使用不同 LLM/VLM 的能力 |
| `_StorageMigrationMixin` | 存储迁移和兼容能力 |

初始化时，LightRAG 会绑定四类存储：

| Storage type | Default |
|---|---|
| KV storage | `JsonKVStorage` |
| Vector storage | `NanoVectorDBStorage` |
| Graph storage | `NetworkXStorage` |
| Doc status storage | `JsonDocStatusStorage` |

也就是说，默认模式可以本地轻量跑起来。
但如果要做生产级系统，可以换成 Postgres、Neo4j、Mongo、Redis、Milvus、Qdrant、OpenSearch 等。

`LightRAG` 初始化后会创建很多 namespace：

| Namespace object | What it stores |
|---|---|
| `llm_response_cache` | LLM response cache |
| `text_chunks` | chunk 文本 |
| `full_docs` | 原始完整文档 |
| `full_entities` | entity 完整信息 |
| `full_relations` | relation 完整信息 |
| `entity_chunks` | entity 到 chunk 的连接 |
| `relation_chunks` | relation 到 chunk 的连接 |
| `chunk_entity_relation_graph` | 知识图谱 |
| `entities_vdb` | entity 向量索引 |
| `relationships_vdb` | relationship 向量索引 |
| `chunks_vdb` | chunk 向量索引 |
| `doc_status` | 文档处理状态 |

这说明 LightRAG 的数据模型不是单一向量库，而是：

```text
KV + Vector + Graph + Status
```

这也是它能成为 research memory 基建的原因。

## Insert path

最简单的 SDK 用法是：

```python
from lightrag import LightRAG, QueryParam

rag = LightRAG(
    working_dir="./rag_storage",
    embedding_func=openai_embed,
    llm_model_func=gpt_4o_mini_complete,
)

await rag.initialize_storages()
await rag.ainsert(text)
answer = await rag.aquery("question", param=QueryParam(mode="hybrid"))
await rag.finalize_storages()
```

但从代码看，`ainsert` 不是整个 server pipeline 的全部。
它是 SDK convenience entry point，并且默认只走 fixed-token chunking。

代码注释里有一个很关键的点：

```text
ainsert always chunks with the fixed-token strategy.
The server / REST API ingests via apipeline_enqueue_documents
+ apipeline_process_enqueue_documents,
which is how F/R/V/P are chosen there.
```

这意味着：

| Path | Chunking flexibility |
|---|---|
| direct SDK `ainsert` | fixed-token strategy 为主 |
| server / REST pipeline | 可以通过 process options 选择 F/R/V/P |
| advanced SDK pipeline call | 也可以手动调用 enqueue + process 来选择策略 |

所以如果我们后面要做 `Pengyi Research Memory`，更适合直接用 server pipeline 或 pipeline-level SDK，而不是只用最简单的 `ainsert`。

## File processing

LightRAG v1.5 后的文件处理 pipeline 很完整。

文档解析有四种 engine：

| Engine | Meaning | Typical usage |
|---|---|---|
| `legacy` | 老的通用文本抽取 | 快速兼容各种普通文件 |
| `native` | LightRAG 内置结构化解析 | 本地解析 `docx`、`md`、`textpack` |
| `mineru` | 外部 MinerU 解析服务 | PDF、Office、图片、多模态文档 |
| `docling` | 外部 Docling 解析服务 | PDF、Office、HTML、图片等 |

文件处理不只是“读出文本”。
新 pipeline 引入了一个中间格式：

```text
LightRAG Document
```

它可以保存：

```text
text blocks
heading hierarchy
paragraph metadata
tables
images
equations
sidecar files
```

这件事非常重要。
因为 research memory 不应该只记住纯文本，它还要尽量保留文档结构。

比如一篇论文或者一份研究报告：

```text
title
abstract
section
subsection
table
figure
equation
references
```

这些结构本身就是信息。
如果全部压扁成 plain text，很多上下文会丢失。

## Chunking strategies

LightRAG 支持四类 chunking strategy：

| Strategy | Name | Core idea |
|---|---|---|
| `F` | Fix / fixed-token | 固定 token 长度切分 |
| `R` | Recursive | 按段落、换行、标点、空格等递归切分 |
| `V` | Vector / semantic vector | 用句子 embedding distance 找语义断点 |
| `P` | Paragraph Semantic | 利用 heading、paragraph、table row、sidecar 结构做语义 chunking |

这里面最值得关注的是 `P`。

`ParagraphSemanticChunking.md` 里讲得很清楚：

```text
P strategy targets documents with clear sectional structure.
Its goal is to align chunk boundaries with native semantic boundaries.
```

也就是说，P 策略不是机械切 token。
它会尽量沿着文档原生语义边界切：

```text
heading
paragraph
table row
parent heading path
section hierarchy
```

P 策略特别解决四类问题：

| Problem | P strategy answer |
|---|---|
| 大表格被切碎后失去上下文 | 尽量保持表格完整，必要时按 row 切，并保留 header |
| heading 层级没有被利用 | 使用 parent headings 和 section hierarchy |
| 细碎条款太短 | 用 hierarchy-aware merging 合并 |
| 长 block 被粗暴切断 | 用 anchor-driven splitting 优先在语义点切 |

这对金融研究、法律文本、合同、风控材料、论文非常有用。
因为这些文本往往不是普通文章，而是结构化程度很强的专业文档。

我们后面如果做：

```text
quant papers
WorldQuant-style factor notes
company reports
credit memos
contracts
research proposals
```

P strategy 比普通 fixed chunk 更接近真实需求。

## Entity and relation extraction

LightRAG 的核心不是 chunking，而是 chunking 后的实体关系抽取。

典型流程是：

```text
chunk text
  -> EXTRACT LLM
  -> entities
  -> relationships
  -> merge duplicate entities
  -> summarize long descriptions
  -> write graph storage
  -> write entity / relationship vector storage
```

这一步把文档从 plain text 变成 graph memory。

对 research 场景来说，实体可以是：

```text
paper
author
method
dataset
benchmark
model
company
factor
signal
portfolio
risk metric
market regime
```

关系可以是：

```text
paper proposes method
method improves benchmark
factor depends on data field
strategy uses signal
model is evaluated on dataset
company reports revenue segment
risk metric constrains portfolio
```

这样一来，RAG 的检索不再只是：

```text
which chunk is similar to this query?
```

而是还可以问：

```text
which entities are connected?
which relationships are relevant?
which chunks support these entities and relationships?
```

这就是 LightRAG 和普通向量库 RAG 的本质区别。

## Storage layer

`lightrag/kg/__init__.py` 里可以看到存储实现的注册表。

四种 storage type：

| Type | Examples |
|---|---|
| KV storage | JSON, Redis, Postgres, Mongo, OpenSearch |
| Graph storage | NetworkX, Neo4j, Postgres, Mongo, Memgraph, OpenSearch |
| Vector storage | NanoVectorDB, Milvus, Postgres, Faiss, Qdrant, Mongo, OpenSearch |
| Doc status storage | JSON, Redis, Postgres, Mongo, OpenSearch |

默认组合是：

```text
JsonKVStorage
NanoVectorDBStorage
NetworkXStorage
JsonDocStatusStorage
```

这很适合本地研究和小 demo。

生产或大规模工作区可以考虑：

| Scenario | Possible backend |
|---|---|
| graph-heavy exploration | Neo4j / Memgraph |
| unified infra | Postgres / OpenSearch |
| vector-heavy retrieval | Milvus / Qdrant / Faiss |
| document/status cache | Redis / Mongo / Postgres |

对我们来说，第一阶段不需要上复杂 infra。
最好的起点是：

```text
local JSON + NanoVectorDB + NetworkX
```

先把知识链路跑通，再考虑迁移。

## Query modes

LightRAG 最直观的用户功能是 query mode。

主要模式：

| Mode | Meaning | When to use |
|---|---|---|
| `naive` | 只做 chunk vector search，不用 KG | 对照实验，看看普通 RAG 的效果 |
| `local` | 以局部实体上下文为主 | 问某个具体实体、方法、项目、因子 |
| `global` | 以全局关系和主题为主 | 问跨文档主题、整体脉络、关系链 |
| `hybrid` | local + global | 既要细节，也要结构 |
| `mix` | KG retrieval + vector retrieval 综合 | 最适合复杂问答和默认研究查询 |
| `bypass` | 不做 RAG，直接问底层 LLM | 对照底层模型能力 |

在 API server 的 Ollama-compatible chat 接口里，还支持用前缀选择模式：

```text
/local
/global
/hybrid
/naive
/mix
/bypass
/context
```

这设计得很适合做交互式研究系统。
例如我们可以把 Open WebUI 或自己的 agent UI 接到 LightRAG，然后用：

```text
/mix 总结我最近 LLMQuant 和 HKUDS 学习笔记之间的共性
/local LightRAG 的 chunking strategy 有哪些
/global HKUDS 的项目体系对 AI Scientist 路线有什么启发
/context 给我只返回相关上下文，不生成答案
```

`/context` 或 `only_need_context=True` 特别重要。
它可以让我们检查 RAG 到底召回了什么，而不是盲相信最终答案。

## QueryParam

`QueryParam` 是查询控制中心。

常见参数包括：

| Param | Meaning |
|---|---|
| `mode` | `local/global/hybrid/naive/mix/bypass` |
| `only_need_context` | 只返回检索上下文，不生成最终答案 |
| `only_need_prompt` | 返回构造好的 prompt |
| `top_k` | entity / relationship 检索数量 |
| `chunk_top_k` | chunk 检索数量 |
| `max_entity_tokens` | entity context token budget |
| `max_relation_tokens` | relation context token budget |
| `max_total_tokens` | 总上下文 budget |
| `hl_keywords` | high-level keywords |
| `ll_keywords` | low-level keywords |
| `conversation_history` | 只给 LLM 生成阶段用，不参与 retrieval |
| `user_prompt` | 指导最终回答，不参与 retrieval |
| `enable_rerank` | 是否启用 reranker |
| `include_references` | 是否返回引用 |
| `include_chunk_content` | references 中是否包含 chunk 原文 |
| `stream` | 是否流式输出 |

这里有一个容易误解但很关键的点：

```text
conversation_history and user_prompt do not drive retrieval.
They guide the final LLM response after retrieval.
```

所以如果我们想影响召回，应该调整：

```text
query itself
mode
top_k
chunk_top_k
hl_keywords
ll_keywords
rerank
```

而不是只在 `user_prompt` 里说“请重点关注某某”。

## Retrieval internals

从 `operate.py` 看，LightRAG 的 query 可以理解成四阶段：

```text
1. Search
2. Truncate
3. Merge chunks
4. Build LLM context
```

也就是：

```text
query
  -> keyword extraction
  -> entity / relationship / chunk retrieval
  -> token budget truncation
  -> chunk merging and reranking
  -> context string construction
  -> QUERY LLM generation
```

它不仅返回最终文本，还可以通过 `query_data` / `aquery_data` 返回结构化检索数据：

```text
entities
relationships
chunks
references
metadata
```

这对 evaluation 非常重要。
因为我们可以把 retrieval result 当作可测试对象，而不是只看最终回答。

例如：

```text
Question: LightRAG 如何处理 parser 和 chunking？

Expected retrieval:
  docs/FileProcessingPipeline.md
  docs/ParagraphSemanticChunking.md
  lightrag/pipeline.py
  parser/routing logic
```

如果 final answer 很漂亮，但召回错了，那就是系统问题。
如果召回对了，但 answer 不好，那可能是 prompt / QUERY model 问题。

这就是 research engineering 的思维：

```text
Separate retrieval quality from generation quality.
```

## Role-specific LLM

LightRAG 很聪明的一点是 role-specific LLM/VLM。

它把不同任务分成四个角色：

| Role | Purpose |
|---|---|
| `EXTRACT` | 文档插入阶段，做实体关系抽取和摘要 |
| `KEYWORD` | 查询阶段，抽取 high-level / low-level keywords |
| `QUERY` | 查询阶段，根据召回内容生成最终答案 |
| `VLM` | 插入阶段，分析图片等多模态内容 |

这对成本和质量很关键。

因为不同阶段需要的模型能力不一样：

| Stage | Better model choice |
|---|---|
| extraction | 快、稳定、不要乱推理、上下文足够长 |
| keyword | 极快、低成本 |
| final answer | 更强推理、更好表达 |
| vision | 专门 VLM |

一个合理组合可能是：

```text
EXTRACT -> cheaper strong-enough model
KEYWORD -> ultra-fast small model
QUERY   -> strongest available model
VLM     -> vision-language model
```

这对我们未来做 Research OS 很有启发。
任何 agent system 都不应该只绑定一个模型。
更合理的是：

```text
task-specific model routing
```

把贵模型用在最值钱的地方，把便宜模型用在高频基础任务上。

## API server and WebUI

LightRAG 不只是 Python package，也提供 server 和 WebUI。

Server 主要能力：

```text
WebUI
document indexing
knowledge graph exploration
RAG query
Ollama-compatible chat interface
Swagger / ReDoc API docs
```

默认服务位置：

```text
http://localhost:9621/webui
http://localhost:9621/health
http://localhost:9621/docs
http://localhost:9621/redoc
```

API routes 可以分三类：

| Router | Examples |
|---|---|
| document routes | upload、scan、status、delete、reprocess、pipeline control |
| query routes | `/query`、stream query、`/query/data` |
| graph routes | graph labels、popular labels、subgraph、entity/relation edit/create/delete/merge |

`graph_routes.py` 里不仅能查图，还能编辑图：

```text
/graph/entity/edit
/graph/relation/edit
/graph/entity/create
/graph/relation/create
/graph/entities/merge
```

这说明 LightRAG 的 KG 不是只读产物。
它支持后期维护。

对 research memory 来说，这很关键。
因为自动抽取一定会出错，人工校正入口必须存在。

## Why it matters to us

LightRAG 对 Pengyi Research OS 的意义非常直接。

我们现在有很多材料：

```text
LLMQuant study map 000-007
HKUDS study map
Yuandong Tian study map
RA / PhD application materials
CV / PS / RP
quant interview notes
banking / organization notes
papers
project README
personal website posts
future WorldQuant factor notes
```

这些材料如果只是散落在 Markdown、PDF、Word、网页里，本质上还是静态文件。

LightRAG 可以把它们变成：

```text
source-grounded research memory
```

也就是：

```text
我写过什么
我学过什么
某个项目的核心模块是什么
某个导师/团队和我的方向有什么连接
某个 quant idea 从哪篇 paper / 哪个项目来
某个申请材料对应哪些真实 project evidence
```

这和普通个人笔记不同。
普通笔记靠人脑回忆和文件搜索。
Research memory 靠：

```text
structured retrieval
entity graph
references
query modes
evaluation
```

## LightRAG x LLMQuant

LightRAG 和 LLMQuant 可以非常自然地接起来。

| LLMQuant component | LightRAG role |
|---|---|
| `docs` / `llmquant-book` / `quant-wiki` | 作为 finance knowledge corpus |
| `data-mcp` reports | 作为 data access audit memory |
| `quant-mind` structured knowledge | 可以转成 graph-friendly knowledge |
| `Magents` backtest reports | 作为 strategy experiment memory |
| `awesome-trading-agents` | 作为 ecosystem radar corpus |
| future WorldQuant factor notes | 作为 factor research corpus |

我现在会这样分工：

```text
QuantMind = 将金融文本主动结构化成 quant knowledge object
LightRAG  = 将多源材料变成可检索、可引用、可问答的 graph RAG memory
```

二者不是替代关系。
更像上下游：

```text
documents / papers / notes
  -> LightRAG ingestion and retrieval
  -> QuantMind-style structured knowledge extraction
  -> R&D Agent uses both to propose and test ideas
```

或者反过来也可以：

```text
QuantMind generates structured knowledge
  -> LightRAG indexes it with provenance
  -> agent retrieves it during research planning
```

## First demo plan

我们不应该一上来就做很大的系统。
第一阶段 demo 应该小而完整。

我建议做：

```text
pengyi-research-memory-light
```

第一批 corpus：

| Corpus | Why |
|---|---|
| HKUDS000 | HKUDS map |
| HKUDS001 | LightRAG deep dive |
| LLMQUANT000-007 | quant research stack memory |
| Yuandong000 | AI scientist study map |
| sanitized CV / PS / RP | application narrative evidence |

第一批问题：

```text
1. Pengyi Research OS 的核心组件有哪些？
2. LightRAG 和 QuantMind 的差异是什么？
3. LLMQuant 哪些项目最适合接入 R&D Agent？
4. 我的 RA/PhD 叙事可以引用哪些项目 evidence？
5. HKUDS 和 LLMQuant 在 agent / RAG / finance research 上有什么交叉？
```

第一批评估：

| Evaluation | Method |
|---|---|
| retrieval correctness | `only_need_context=True` 检查召回内容 |
| mode comparison | naive/local/global/hybrid/mix 对同一问题比较 |
| citation quality | 检查 references 是否指向正确源文件 |
| hallucination control | 不在 corpus 里的问题是否会承认不足 |
| update behavior | 新增一篇 post 后是否能正确进入 memory |

这就是一个非常好的 Research OS 子系统 demo。

## Practical setup choices

第一阶段不要过度工程化。

建议：

```text
LLM provider: OpenAI-compatible or Ollama-compatible
Embedding: bge-m3 / text-embedding-3-large level
Reranker: optional, first run without, second run add rerank compare
Storage: default local JSON + NanoVectorDB + NetworkX
Parser: legacy-R first, selected docs try native-P
Workspace: pengyi_research_memory_v0
```

需要提前决定 embedding。
这是 LightRAG 文档里反复强调的点：

```text
embedding model / dimension / asymmetric config must be fixed before indexing.
```

如果中途换 embedding，一般要清掉 vector 相关数据并重新 index。

这对我们很重要。
因为 research memory 是长期资产，不要随便换 embedding 底座。

## Caveats

LightRAG 很强，但不是魔法。

要注意：

| Caveat | Meaning |
|---|---|
| embedding 必须稳定 | 换模型通常要重建向量库 |
| extraction 依赖 LLM 质量 | entity/relation 抽取错了，KG 就会脏 |
| reasoning model 不一定适合 indexing | 文档建议 indexing 阶段避免过强推理/思考模式 |
| rerank 提升质量但增加延迟 | 要做 latency/quality tradeoff |
| multimodal 依赖 parser/VLM | MinerU/Docling/VLM 要额外部署和配置 |
| external DB 要注意 workspace isolation | 多实例共用数据库时必须隔离 |
| source discipline 仍然必要 | RAG answer 不能替代原文核验 |

对金融和申请材料尤其要注意：

```text
do not index secrets into public or shared workspace.
```

如果后面把 WorldQuant 因子、银行材料、个人申请材料放进去，必须分 workspace：

```text
public_demo_workspace
private_research_workspace
confidential_workspace
```

开源版本只放脱敏样例。

## PR opportunities

如果我们想给 LightRAG 提 PR，可以从低风险、高价值的地方开始。

可能方向：

| PR idea | Why it is suitable |
|---|---|
| Windows setup notes | 我们本地就是 Windows，可以真实验证 |
| small Chinese/English research-memory demo | 对中文用户有帮助，也贴合我们的使用场景 |
| query mode comparison notebook | 帮用户理解 naive/local/global/hybrid/mix |
| docs for `ainsert` vs pipeline ingestion | 这个点容易误解，文档补充有价值 |
| evaluation template for retrieval quality | 帮助用户区分 retrieval 和 generation |

提 PR 的原则：

```text
Use first.
Find real friction.
Submit focused improvement.
```

不要为了 PR 而 PR。
真正使用中发现的问题，才是好的贡献入口。

## Personal synthesis

LightRAG 给我的启发是：

```text
AI scientist 不只是会问模型。
AI scientist 要搭自己的 knowledge substrate.
```

我们未来要冲顶会、做开源项目、做 Quant R&D Agent，就必须有一个持续吸收知识的系统。

这个系统应该能吃：

```text
papers
code repos
blog posts
PDF reports
research notes
experiment results
interview notes
application materials
```

然后输出：

```text
structured context
research memory
source-grounded answers
next research plans
project comparison
application narratives
PR opportunities
```

这就是 LightRAG 对我们的核心价值。

它不是终点。
它是底座。

```text
LightRAG -> Research Memory
Vibe-Trading -> Quant Research Workflow
nanobot -> Personal Agent Shell
LLMQuant -> Finance / Quant Research Stack
```

这四个拼起来，就开始接近我们想做的：

```text
Pengyi Quant Research OS v0
```

## Next

HKUDS 第一阶段路线继续：

```text
HKUDS000 -> Study Map
HKUDS001 -> LightRAG
HKUDS002 -> Vibe-Trading
HKUDS003 -> nanobot
HKUDS004 -> HKUDS x LLMQuant x Pengyi Research OS integration
```

下一篇做 `Vibe-Trading`。
重点看它是否可以作为：

```text
agentic quant research workflow
trading idea execution workspace
R&D Agent 的 finance-side application shell
```
