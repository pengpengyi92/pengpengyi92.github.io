---
title: "LightRAG Deepdown002: Python Core / File Pipeline / Storage / Retrieval / Why Light"
date: 2026-07-06 00:00:00 +0800
categories: [Learning, HKUDS]
tags: [lightrag-deepdown002, lightrag, python, file-pipeline, storage, retrieval, graph-rag, parser, unified-document, vector-storage, graph-storage, ai-infra]
---

这篇是 `LightRAG Deepdown002`。

Deepdown001 讲的是 coding language composition：Python、TypeScript、TSX、HTML、CSS、JS、TOML、YAML、JSON 各自在系统里做什么。

这一篇继续往里挖，回答几个更关键的问题：

```text
1. LightRAG 的 Python 部分到底做了什么？
2. PDF / DOCX / PPTX / HTML / code 等异构文件是怎么进入系统的？
3. storage 系统是数据库，还是文件格式，还是抽象层？
4. retrieval 是怎么做的？
5. 相比普通 RAG，LightRAG 的 "light" 到底轻在哪里？
```

我的核心判断：

```text
LightRAG 的 Python 八成不是“写算法”，而是在写一个完整 RAG 后端操作系统。
```

## 1. Python 部分为什么这么大

基于本地 `LightRAG-main.zip` 内 `.py` 文件统计，Python 部分大概分布如下：

| Area | Files | Lines | 作用 |
|---|---:|---:|---|
| `tests` | 119 | 57,084 | 测试体系，保护 parser、storage、API、pipeline 行为 |
| `lightrag/core` | 25 | 27,931 | 主类、pipeline、operate、utils、prompt、parser routing |
| `lightrag/kg` | 17 | 25,205 | KV / vector / graph / doc status storage backend |
| `lightrag/api` | 14 | 11,073 | FastAPI server、document/query/graph routes、auth、runtime validation |
| `lightrag/llm` | 18 | 5,992 | OpenAI、Gemini、Bedrock、Ollama、HF 等 provider binding |
| `lightrag/tools` | 9 | 5,175 | cache migration、password、visualizer、download cache 等工具 |
| `native_parser` | 11 | 4,919 | DOCX 等原生结构解析 |
| `external_parser` | 14 | 4,271 | MinerU / Docling 外部解析器 |
| `chunker` | 5 | 2,029 | chunking 策略 |

最大的几个 Python 文件非常有代表性：

```text
lightrag/kg/postgres_impl.py             7172 lines
lightrag/operate.py                      5827 lines
lightrag/pipeline.py                     4487 lines
lightrag/api/routers/document_routes.py  4197 lines
lightrag/lightrag.py                     4012 lines
lightrag/utils.py                        3901 lines
```

这说明它的 Python 重心不是一个小模型调用，而是：

```text
storage backend
retrieval logic
document ingest pipeline
API document management
system orchestration
```

## 2. Python 的九层系统职责

LightRAG 的 Python 可以拆成九层：

```text
1. Core orchestration
2. Data contracts
3. File processing / ingestion pipeline
4. Retrieval and query engine
5. Multi-storage backend
6. LLM and embedding provider binding
7. API server
8. Parser / chunker / sidecar
9. Tests and maintenance tools
```

这九层组成一个 RAG backend OS。

### 2.1 Core orchestration

核心入口是：

```text
lightrag/lightrag.py
```

主类是：

```python
class LightRAG(_RoleLLMMixin, _StorageMigrationMixin, _PipelineMixin)
```

这个类不是普通 wrapper。

它把三类能力合在一起：

```text
Role-specific LLM behavior
Storage migration behavior
Document ingestion pipeline behavior
```

它负责：

```text
initialize storages
finalize storages
insert documents
insert custom chunks
insert custom KG
query
query data
query LLM
delete by doc id
delete by entity
delete by relation
edit entity
edit relation
create entity
create relation
merge entities
export data
clear cache
```

所以 `LightRAG` 类是系统总控。

### 2.2 Data contracts

契约层在：

```text
lightrag/base.py
```

核心对象包括：

```text
QueryParam
BaseVectorStorage
BaseKVStorage
BaseGraphStorage
DocStatusStorage
DocProcessingStatus
QueryResult
QueryContextResult
```

这一层的作用是：

```text
先定义 storage / query / result 的抽象接口，再让不同后端实现它。
```

这就是为什么 LightRAG 可以本地轻量跑，也可以换成 production database。

## 3. 文件兼容性：不是靠后缀堆出来的

LightRAG 能处理很多文件类型。

大类包括：

```text
文本类：
txt, md, mdx, rtf, odt, tex, epub, html, htm, csv, json, xml, yaml, yml, log, conf, ini, properties, sql

代码类：
bat, sh, c, h, cpp, hpp, py, java, js, ts, swift, go, rb, php, css, scss, less

Office / 文档类：
pdf, docx, pptx, xlsx

图片 / 多模态类：
png, jpg, jpeg, jp2, webp, gif, bmp, tiff
```

但真正关键不是“支持很多后缀”。

关键是它用了一套文件处理架构：

```text
file type / filename hint / parser config
  -> parser routing
  -> content extraction
  -> unified intermediate representation
  -> chunking
  -> entity / relation extraction
  -> storage
```

这个设计把异构文件统一进同一条 RAG pipeline。

## 4. 统一中间格式：兼容性的核心

PDF、DOCX、PPTX、HTML、Markdown、代码文件差异非常大。

例如 PDF 里可能有：

```text
正文
标题
页眉页脚
表格
图片
公式
双栏排版
扫描图片
脚注
引用
跨页表格
```

如果后续 pipeline 直接面对原始 PDF，会非常难维护。

LightRAG 的做法是：

```text
复杂输入
  -> parser
  -> LightRAG Document / blocks / metadata / sidecar
  -> 通用 pipeline
```

也就是说：

```text
PDF / DOCX / PPTX / image / code
最终都要被转成 pipeline 可以理解的统一中间表示。
```

这就是兼容性的关键。

真正的系统原则是：

```text
input diversity is handled before the core pipeline.
core pipeline should operate on normalized representations.
```

## 5. PDF 是怎么处理的

以 PDF 为例。

PDF 不是直接塞给 LLM。

大概流程是：

```text
PDF
  -> upload / scan
  -> parser routing
  -> content extraction
  -> LightRAG Document / blocks / metadata
  -> chunking
  -> entity and relation extraction
  -> KV / vector / graph storage
  -> query retrieval
```

PDF 可以走不同 parser：

```text
legacy   -> 基础文本抽取
mineru   -> 更强的 PDF / OCR / layout / 多模态解析
docling  -> structured document parser
```

可以通过环境变量配置：

```bash
LIGHTRAG_PARSER=pdf:mineru-R,*:legacy-R
```

或者：

```bash
LIGHTRAG_PARSER=pdf:docling-P,*:legacy-R
```

也可以通过文件名 hint 单独指定：

```text
paper.[mineru-R].pdf
report.[docling-P].pdf
memo.[-!].pdf
```

这说明它不是硬编码 PDF 处理方式。

它提供的是：

```text
parser selection as configurable system behavior.
```

## 6. Parser routing 和 processing options

LightRAG 的 file pipeline 里有几个重要概念。

### Parser engine

```text
legacy
native
mineru
docling
```

### Processing options

```text
i = image
t = table
e = equation
P = paragraph semantic chunking
R = recursive chunking
! = disable KG construction
```

例如：

```bash
LIGHTRAG_PARSER=*:native-teP,*:legacy-R
```

可以理解为：

```text
优先用 native 处理 table/equation + paragraph chunking，
否则 fallback 到 legacy + recursive chunking。
```

这个设计很重要。

它不是把逻辑写死在代码里，而是把 parser / chunker / KG behavior 做成可配置选项。

## 7. LLM 不只负责最终回答

LightRAG 里 LLM 的角色很多，不只是 answer generation。

LLM 可能参与：

```text
entity extraction
relationship extraction
keyword extraction
description summarization
query understanding
answer generation
VLM image understanding
```

所以它有 role-specific LLM configuration。

这意味着不同任务可以使用不同模型或参数：

```text
extraction LLM
keyword LLM
summary LLM
query LLM
VLM
embedding model
reranker
```

这比普通 RAG 细很多。

普通 RAG 常常是：

```text
embed query
retrieve chunks
LLM answer
```

LightRAG 是：

```text
LLM participates in both indexing-time knowledge construction and query-time answer generation.
```

## 8. Storage 系统：四类抽象

LightRAG 的 storage 不是一个数据库。

它是四类 storage abstraction：

```text
KV Storage
Vector Storage
Graph Storage
DocStatus Storage
```

对应作用：

| Storage | 存什么 | 默认实现 |
|---|---|---|
| KV Storage | full docs、chunks、entity records、relation records、LLM cache | JSON file |
| Vector Storage | chunk/entity/relation embeddings | NanoVectorDB |
| Graph Storage | entity nodes、relation edges | NetworkX |
| DocStatus Storage | 文档处理状态、失败、进度 | JSON file |

默认本地模式可以是：

```text
JsonKVStorage
NanoVectorDBStorage
NetworkXStorage
JsonDocStatusStorage
```

这意味着本地 demo 不需要一开始就部署复杂数据库。

但生产可以换成：

```text
PostgreSQL / pgvector
Neo4j
MongoDB
Redis
Qdrant
Milvus
FAISS
OpenSearch
Memgraph
```

这就是 progressive complexity：

```text
先轻量启动，再按需求替换成 production backend。
```

## 9. Storage namespace：它到底存了什么

一个 `LightRAG` 对象会管理多个逻辑 namespace。

重要 namespace 包括：

```text
llm_response_cache
text_chunks
full_docs
full_entities
full_relations
entity_chunks
relation_chunks
chunk_entity_relation_graph
entities_vdb
relationships_vdb
chunks_vdb
doc_status
```

普通 RAG 通常只关心：

```text
chunks
chunk embeddings
```

LightRAG 多了：

```text
entities
relationships
graph
entity embeddings
relationship embeddings
doc status
LLM cache
```

所以它不是 chunk-only RAG。

它的知识单位是：

```text
chunk + entity + relationship + graph context
```

## 10. Retrieval modes

LightRAG 的 query modes 包括：

```text
naive
local
global
hybrid
mix
bypass
```

可以这样理解：

### naive

普通向量 RAG：

```text
query
  -> vector search chunks
  -> context
  -> answer
```

### local

偏 entity-centered retrieval：

```text
query
  -> extract low-level keywords
  -> find related entities
  -> pull entity descriptions
  -> find related chunks
  -> answer
```

适合问具体实体、概念、细节。

### global

偏 relationship / global retrieval：

```text
query
  -> extract high-level keywords
  -> find related relationships
  -> pull relation context
  -> answer
```

适合问主题、全局关系、跨文档联系。

### hybrid

local + global：

```text
entity context + relationship context
```

### mix

Graph retrieval + vector chunk retrieval：

```text
KG context + vector chunk context
```

### bypass

不检索，直接 LLM：

```text
query -> LLM
```

这个 query mode 设计体现了 LightRAG 的轻量化哲学：

```text
不是所有问题都走最重 pipeline。
按问题复杂度选择 retrieval path。
```

## 11. Context construction

Retrieval 后还要构造 context。

LightRAG 不只是把 top-k chunks 塞进 prompt。

它会构造：

```text
entities context
relationships context
chunks context
references
```

并处理：

```text
deduplication
chunk merge
rerank
token budget
entity token budget
relation token budget
total token budget
truncation
prompt construction
```

大概流程：

```text
query
  -> keyword extraction
  -> graph / vector search
  -> fetch node / edge data
  -> find related text chunks
  -> merge chunks
  -> apply token truncation
  -> build context string
  -> call LLM
```

这就是 `operate.py` 体量很大的原因。

## 12. Compared with ordinary RAG

普通 RAG：

```text
documents
  -> chunks
  -> embeddings
  -> vector search
  -> LLM answer
```

LightRAG：

```text
heterogeneous documents
  -> parser routing
  -> unified intermediate representation
  -> chunks
  -> entities
  -> relationships
  -> KV / vector / graph / doc status storage
  -> local / global / hybrid / mix retrieval
  -> role-specific LLM generation
```

普通 RAG 的知识单位是：

```text
chunk
```

LightRAG 的知识单位是：

```text
chunk + entity + relationship + graph
```

普通 RAG 擅长：

```text
找到相似段落
```

LightRAG 更适合：

```text
跨文档关系
实体网络
概念之间的联系
全局主题总结
知识图谱可视化
graph + vector hybrid retrieval
```

## 13. Why Light

LightRAG 的 `Light` 不是说功能少。

更准确地说：

```text
LightRAG 把 GraphRAG 拆成可选、可替换、可渐进部署的组件。
```

它轻在几个地方：

### 13.1 默认存储轻

不用一上来就部署 Neo4j + Milvus + Postgres。

默认可以：

```text
JSON + NanoVectorDB + NetworkX
```

### 13.2 使用入口轻

用户可以直接：

```python
rag = LightRAG(...)
rag.insert(text)
rag.query("...")
```

### 13.3 Query mode 轻

不同问题走不同模式：

```text
简单问题 -> naive
实体问题 -> local
全局问题 -> global
复杂问题 -> mix
```

### 13.4 Parser 可选

简单文本走 legacy。

复杂 PDF / 多模态文档再接：

```text
MinerU
Docling
VLM
```

### 13.5 后端可替换

本地轻量模式可以跑。

生产模式再换：

```text
Postgres
Neo4j
Milvus
Qdrant
Redis
OpenSearch
```

这就是：

```text
progressive complexity
```

## 14. 对我们 Research OS 的启发

LightRAG 对我们的启发非常直接。

如果做 `Pengyi Quant Research OS`，不能只做：

```text
PDF -> LLM summary
```

应该做：

```text
research reports / papers / filings / notes / data docs
  -> parser routing
  -> unified research document representation
  -> chunking
  -> entity / relation / claim / hypothesis extraction
  -> KV / vector / graph / experiment storage
  -> local / global / hybrid retrieval
  -> factor hypothesis / bias diagnosis / next plan generation
```

也就是说，Quant Research OS 也要有自己的中间格式：

```text
ResearchDocument
ResearchBlock
Claim
Evidence
Entity
Relation
FactorHypothesis
ExperimentRecord
Diagnosis
NextPlan
```

这才是可复用的系统。

## 15. 最终总结

LightRAG 的关键不只是 Graph RAG。

更准确地说，它做了五件事：

```text
1. 把异构文件转成统一中间格式。
2. 把普通 chunk-only RAG 扩展成 chunk + entity + relation + graph。
3. 把 storage 拆成 KV / vector / graph / doc status 四类抽象。
4. 把 retrieval 拆成 naive / local / global / hybrid / mix / bypass 多种路径。
5. 把 GraphRAG 做成可轻量启动、可逐步替换后端、可服务化、可产品化的 AI infra。
```

一句话：

```text
LightRAG 是把普通 RAG pipeline 拆细、模块化、图谱化、可配置化、可部署化之后的轻量 GraphRAG 系统。
```
