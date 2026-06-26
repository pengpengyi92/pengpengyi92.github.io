---
title: "HKUDS018: MiniRAG 作为 Lightweight Graph RAG 与 On-Device Knowledge Layer"
date: 2026-06-26 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds018, hkuds, minirag, graph-rag, lightweight-rag, small-language-models, on-device-rag, lihua-world, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第十九篇。

```text
HKUDS018 -> MiniRAG
```

上一篇 `HKUDS017` 看的是 `AnyTool`：

```text
AnyTool = Universal Tool-Use Layer + Capability Routing Layer
```

`AnyTool` 解决的是 agent 怎么找到工具、调用工具、记录工具质量。

这一篇 `MiniRAG` 回到知识层：

```text
MiniRAG = Lightweight Graph RAG + On-Device Knowledge Layer
```

如果说 `LightRAG` 是完整的 graph-based RAG 框架，那么 `MiniRAG` 更像是它的轻量化、端侧化、小模型友好版本。

```text
LightRAG 关注强 graph RAG 能力。
RAG-Anything 关注多模态复杂文档入口。
MiniRAG 关注小模型、低存储、低复杂度、端侧 RAG。
```

这对我们的 `Pengyi Research OS` 很关键。因为不是所有知识任务都应该调用最重的模型、最复杂的图数据库、最长的上下文。

真正可持续的系统需要分层：

```text
heavy research memory
lightweight local memory
task-specific temporary memory
agent working memory
```

`MiniRAG` 就是 lightweight local memory 的很强参考。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `MiniRAG`。

| Item | Value |
|---|---|
| repo | `MiniRAG` |
| remote | `https://github.com/HKUDS/MiniRAG.git` |
| branch | `main` |
| local head | `e204d23` |
| full commit | `e204d239421f45004852953679927fdf6733f236` |
| latest local commit date | `2025-10-16 15:43:16 +0800` |
| latest local commit | `Update README.md` |
| status | clean, synced with `origin/main` after fetch |
| local tags | none |
| license | `MIT` |
| package name | `minirag-hku` |
| package version | `0.0.2` |
| API version | `1.0.3` |
| Python requirement | `>=3.9` |
| tracked files by `git ls-files` | 77 |
| Python files | 45 |
| KG storage impl files | 14 |
| LLM provider files | 12 |
| default KV storage | `JsonKVStorage` |
| default vector storage | `NanoVectorDBStorage` |
| default graph storage | `NetworkXStorage` |
| default doc status storage | `JsonDocStatusStorage` |
| syntax check | `py -m compileall -q minirag main.py reproduce tests` passed |
| metadata check | `pyproject.toml` parsed successfully with `tomllib` |
| import smoke test | blocked in this environment because dependencies are not installed: missing `python-dotenv` |

一句话先行：

```text
MiniRAG 把 RAG 从“大模型理解长文本”转成“小模型借助异构图和轻量拓扑检索完成知识发现”。
它用 text chunks + named entities + relationships 构成异构图，再用 answer type、query entities、2-hop graph neighborhood、edge voting 和 chunk scoring 找到少量高价值上下文，减少对大模型语义能力和长上下文的依赖。
```

## 它解决什么问题

MiniRAG 的目标场景不是云端大模型 RAG，而是：

```text
Small Language Models
on-device RAG
resource-constrained deployment
low storage
simple retrieval
```

README 里强调一个现实问题：

```text
现有 RAG 框架放到 SLM 上会明显退化。
```

原因是小模型有三个短板：

| SLM Limitation | 对 RAG 的影响 |
|---|---|
| semantic understanding weaker | 直接向量召回容易漏掉复杂关系 |
| text processing weaker | 长 context 容易压垮生成质量 |
| instruction following weaker | 复杂 graph context 不一定会被正确利用 |

MiniRAG 的回答是：

```text
不要把理解压力全部放在小模型上。
把更多结构信息提前放进 index 和 retrieval。
```

所以它提出两点：

| Innovation | Meaning |
|---|---|
| semantic-aware heterogeneous graph indexing | 把 text chunks 和 named entities 放进统一结构 |
| lightweight topology-enhanced retrieval | 用图拓扑做知识发现，减少对高级语义能力的依赖 |

这就是 `MiniRAG` 的核心：

```text
less semantic burden on the model
more structure in the retrieval layer
```

## 和 LightRAG 的关系

MiniRAG 明确基于 `LightRAG` 和 `nano-graphrag`。

但它不是简单复制 LightRAG，而是做了一个更轻的版本：

| Repo | System Position |
|---|---|
| `LightRAG` | graph-based RAG baseline，完整知识图谱检索框架 |
| `RAG-Anything` | multimodal document ingestion layer |
| `MiniRAG` | SLM-friendly lightweight graph RAG layer |
| `VideoRAG` | long-context video understanding RAG |

对我们来说可以这样理解：

```text
LightRAG = Research OS 的主知识图谱层
RAG-Anything = 复杂文档入口层
MiniRAG = 本地轻量知识层 / 小模型知识层
```

`MiniRAG` 的意义不是替代 LightRAG，而是补一个更轻、更适合端侧或低成本场景的层。

## Benchmark 结果

README 给出的核心实验表里，MiniRAG 在小模型上非常突出。

LiHua-World 上：

| Model | NaiveRAG acc | LightRAG acc | MiniRAG acc |
|---|---:|---:|---:|
| Phi-3.5-mini-instruct | 41.22% | 39.81% | **53.29%** |
| GLM-Edge-1.5B-Chat | 42.79% | 35.74% | **52.51%** |
| Qwen2.5-3B-Instruct | 43.73% | 39.18% | **48.75%** |
| MiniCPM3-4B | 43.42% | 35.42% | **51.25%** |
| gpt-4o-mini | 46.55% | **56.90%** | 54.08% |

MultiHop-RAG 上：

| Model | NaiveRAG acc | LightRAG acc | MiniRAG acc |
|---|---:|---:|---:|
| Phi-3.5-mini-instruct | 42.72% | 27.03% | **49.96%** |
| GLM-Edge-1.5B-Chat | 44.44% | / | **51.41%** |
| Qwen2.5-3B-Instruct | 39.48% | 21.91% | **48.55%** |
| MiniCPM3-4B | 39.24% | 19.48% | **47.77%** |
| gpt-4o-mini | 53.60% | 64.91% | **68.43%** |

这说明一个重要结论：

```text
MiniRAG 对小模型不是“缩水版”，而是专门为小模型重构 retrieval burden 的版本。
```

它不追求给模型塞最多上下文，而是尝试把上下文压成更结构化、更少、更有用。

## LiHua-World

MiniRAG 还带了一个 benchmark：

```text
dataset/LiHua-World
```

本地文件包括：

| File | Size / Role |
|---|---|
| `data/LiHuaWorld.zip` | 548,314 bytes，一年聊天记录 |
| `qa/query_set.csv` | 84,305 bytes，问题、标准答案、证据 |
| `qa/query_set.json` | 154,099 bytes，JSON 格式问题集 |

LiHua-World 是一个本地 RAG 场景数据集：

```text
one year of chat records from a virtual user named LiHua
```

问题类型：

| Type | Meaning |
|---|---|
| Single-hop | 单跳事实问题 |
| Multi-hop | 多时间点、多证据问题 |
| Summary | 汇总型问题 |

一个样例问题是：

```text
Did Adam Smith send a message to Li Hua about the upcoming building maintenance schedule before the administrators announced a temporary change in the construction schedule due to weather conditions?
```

这类问题对 RAG 很真实。因为它不是单纯语义搜索，而是：

```text
找到两个事件
比较时间顺序
确认人物和动作
给出 yes/no
```

所以 MiniRAG 的 graph topology 有意义。它能帮助系统在碎片化聊天记录里连接人物、事件、时间和证据。

## 代码结构

核心目录：

```text
minirag/
├── minirag.py
├── operate.py
├── base.py
├── prompt.py
├── utils.py
├── kg/
├── llm/
└── api/
```

主要文件：

| File | Role |
|---|---|
| `minirag/minirag.py` | `MiniRAG` 主类，负责初始化 storage、insert、query |
| `minirag/operate.py` | chunking、entity extraction、query modes、MiniRAG retrieval |
| `minirag/base.py` | `QueryParam`、storage abstract classes、doc status |
| `minirag/prompt.py` | entity extraction、keyword extraction、RAG response prompts |
| `minirag/utils.py` | hashing、tiktoken、context combine、path scoring、metrics |
| `minirag/kg/*_impl.py` | JSON、NetworkX、NanoVectorDB、Neo4j、Postgres、Oracle、Mongo、Redis、Weaviate 等存储 |
| `minirag/llm/*.py` | OpenAI、Azure、Ollama、HF、Bedrock、Jina、Zhipu 等 LLM/embedding wrappers |
| `minirag/api/minirag_server.py` | FastAPI server、document endpoints、Ollama-compatible API |
| `reproduce/Step_0_index.py` | 索引 LiHua-World |
| `reproduce/Step_1_QA.py` | 跑 QA 实验 |

## MiniRAG 主类

入口是：

```python
from minirag import MiniRAG, QueryParam
```

`MiniRAG` 默认存储：

```python
kv_storage = "JsonKVStorage"
vector_storage = "NanoVectorDBStorage"
graph_storage = "NetworkXStorage"
doc_status_storage = "JsonDocStatusStorage"
```

它初始化了几个核心 storage：

| Storage | Meaning |
|---|---|
| `full_docs` | 原始文档 |
| `text_chunks` | chunk KV store |
| `chunk_entity_relation_graph` | entity-relation graph |
| `entities_vdb` | entity + description vector DB |
| `entity_name_vdb` | entity name vector DB |
| `relationships_vdb` | relationship vector DB |
| `chunks_vdb` | chunk vector DB |
| `llm_response_cache` | LLM response cache |
| `doc_status` | pending / processing / processed / failed |

这说明 MiniRAG 的知识层其实不是单一向量库，而是：

```text
KV storage + vector storage + graph storage + doc status + LLM cache
```

只是默认实现都很轻：

```text
JSON files
NetworkX graphml
NanoVectorDB json
```

这和它的 lightweight 目标一致。

## Insert Pipeline

`rag.insert()` 最终调用 `ainsert()`。

流程是：

```text
input documents
-> apipeline_enqueue_documents
-> apipeline_process_enqueue_documents
-> chunking
-> upsert chunks/full_docs/text_chunks
-> mark doc as processed
-> extract_entities
-> upsert graph nodes/edges
-> upsert entity/name/relationship vectors
-> index_done callbacks
```

`apipeline_enqueue_documents()` 做：

| Step | Meaning |
|---|---|
| validate ids | 如果用户传 ids，检查长度和唯一性 |
| clean and hash docs | 默认用 md5 生成 `doc-` id |
| deduplicate content | 文档内容去重 |
| create status | 初始状态 `PENDING` |
| filter processed docs | 已处理文档不重复进队 |
| upsert doc_status | 写入状态存储 |

`apipeline_process_enqueue_documents()` 做：

```text
PENDING / FAILED / PROCESSING docs
-> batches by max_parallel_insert
-> chunking_by_token_size
-> upsert chunks_vdb
-> upsert full_docs
-> upsert text_chunks
-> mark PROCESSED
```

之后 `extract_entities()` 才会从 processed docs 里重新取 chunk，抽取 entity / relationship。

这就是 MiniRAG 的 indexing 核心：

```text
document -> chunks -> entities -> relationships -> graph + vector stores
```

## Entity Extraction

`extract_entities()` 在 `operate.py`。

它用 LLM 从每个 chunk 抽：

```text
entity
relationship
content_keywords
```

默认 entity types：

```python
["organization", "person", "location", "event"]
```

每个 entity 包含：

```text
entity_name
entity_type
entity_description
source_id
```

每条 relationship 包含：

```text
src_id
tgt_id
description
keywords
weight
source_id
```

然后它会：

```text
merge duplicate nodes
merge duplicate edges
upsert graph nodes
upsert graph edges
upsert entity vectors
upsert entity name vectors
upsert relationship vectors
```

这里有一个很 MiniRAG 的点：

```text
entity_name_vdb
```

它不是用实体描述做 retrieval，而是单独存实体名字，用来把 query 里的实体 phrase 对齐到图里的 entity nodes。

这对小模型很重要。小模型可能不擅长复杂语义判断，但名字匹配、局部拓扑、类型池可以降低难度。

## QueryParam

`QueryParam` 支持三个 mode：

```python
mode: Literal["light", "naive", "mini"] = "mini"
```

主要参数：

| Param | Default | Meaning |
|---|---:|---|
| `mode` | `mini` | 查询模式 |
| `top_k` | env `TOP_K` or 60 | top-k retrieval |
| `max_token_for_text_unit` | 4000 | chunk context token budget |
| `max_token_for_global_context` | 4000 | relationship context budget |
| `max_token_for_local_context` | 4000 | entity context budget |
| `max_token_for_node_context` | 500 | mini mode node context budget |
| `only_need_context` | false | 只返回检索上下文 |
| `only_need_prompt` | false | 保留字段，但当前主路径里没看到核心使用 |
| `conversation_history` | [] | conversation history support |
| `history_turns` | 3 | 历史轮数 |

`max_token_for_node_context = 500` 非常关键。

代码注释写得很直接：

```text
For Mini, if too long, SLM may be fail to generate any response
```

这就是 MiniRAG 的小模型约束意识：

```text
context 不是越多越好。
对 SLM 来说，少而准更重要。
```

## 三种 Query Mode

MiniRAG 有三个查询模式：

| Mode | Function | Meaning |
|---|---|---|
| `naive` | `naive_query` | 直接 chunk vector retrieval |
| `light` | `hybrid_query` | LightRAG 风格 high/low keyword graph retrieval |
| `mini` | `minirag_query` | MiniRAG 的 lightweight topology retrieval |

### naive

`naive_query()` 很简单：

```text
query chunks_vdb
-> get chunk ids
-> load text chunks
-> truncate by token budget
-> RAG response
```

它是 baseline。

### light

`hybrid_query()` 更接近 LightRAG：

```text
LLM extracts high-level and low-level keywords
-> low-level keywords retrieve entities
-> high-level keywords retrieve relationships
-> build local context
-> build global context
-> combine contexts
-> RAG response
```

也就是：

```text
local entity context + global relationship context
```

### mini

`minirag_query()` 是重点。

它不是抽 high/low keywords，而是抽：

```text
answer_type_keywords
entities_from_query
```

prompt 叫：

```text
minirag_query2kwd
```

它会先从图里取 type pool：

```python
TYPE_POOL, TYPE_POOL_w_CASE = await knowledge_graph_inst.get_types()
```

然后让模型在这个 type pool 里选择答案类型，并抽 query 里的具体实体。

这一步非常关键：

```text
query -> answer type + query entities
```

这比单纯关键词更适合小模型。因为很多问题问的是“答案应该属于哪类东西”。

比如：

```text
When was ...
-> DATE AND TIME

Where is ...
-> LOCATION

Who ...
-> PERSON
```

MiniRAG 把这个 answer type 当成 retrieval signal。

## Mini Retrieval Path

`_build_mini_query_context()` 是 MiniRAG 的核心。

简化后的流程：

```text
entities_from_query
-> query entity_name_vdb
-> candidate entity nodes
-> get 2-hop neighbors from graph
-> get nodes matching answer type
-> score paths toward answer-type candidates
-> query relationship vectors with original query
-> keep edges touching important entities
-> edge voting over paths
-> path2chunk
-> direct chunk vector retrieval
-> merge/scored chunk ids
-> build compact context
```

这里有几个关键技巧。

### 1. 用 entity_name_vdb 做实体对齐

```text
query entity phrase -> graph entity name
```

这一步避免小模型直接在长 chunk 里理解所有细节。

### 2. 用 2-hop graph neighborhood 找候选路径

代码里：

```python
get_neighbors_within_k_hops(key, 2)
```

这对 multi-hop question 很重要。很多答案不是一个 chunk 里直接出现，而是几个实体/事件之间的路径。

### 3. 用 answer type 限制目标候选

```python
get_node_from_types(type_keywords)
```

比如问题问时间，就优先找时间类节点；问地点，就优先找地点类节点。

这让 retrieval 更像：

```text
从 query entities 出发，沿图找可能回答类型的节点。
```

### 4. 用 edge voting 修正路径

`edge_vote_path()` 和 `path2chunk()` 会把 relationship retrieval 的结果投票到路径和 chunk 上。

也就是说：

```text
graph path signal + relationship vector signal + chunk vector signal
```

最后一起决定哪些 chunk 进上下文。

### 5. 输出非常短的 context

mini context 只包含：

```text
Entities
Sources
```

不像 LightRAG hybrid 那样输出 entities + relationships + sources 三段完整表。

这是为 SLM 降低负担。

## Storage Layer

MiniRAG 默认是轻量本地存储：

```text
JsonKVStorage
JsonDocStatusStorage
NetworkXStorage
NanoVectorDBStorage
```

但 `STORAGES` 也映射了更多后端：

```text
Neo4J
Oracle
Milvus
Mongo
Redis
Chroma
Postgres
AGE
Gremlin
Weaviate
```

README 新闻里说已经支持 10+ heterogeneous graph databases。

从 Research OS 角度看，这个抽象很有价值：

```text
开发期：JSON + NetworkX + NanoVectorDB
本地生产：Chroma / Redis / Mongo
团队部署：Postgres / Neo4j / Weaviate
企业场景：Oracle / AGE / Gremlin
```

也就是说，MiniRAG 的轻量不是只能本地玩具化，而是可以从简单存储逐步迁移到更重的后端。

## API Server

MiniRAG 提供 FastAPI server：

```text
minirag/api/minirag_server.py
```

安装后 entry point：

```text
minirag-server
```

支持的主要 endpoint：

| Endpoint | Meaning |
|---|---|
| `POST /query` | RAG query |
| `POST /query/stream` | streaming RAG query |
| `POST /documents/text` | 插入文本 |
| `POST /documents/file` | 上传单文件 |
| `POST /documents/batch` | 批量上传文件 |
| `POST /documents/scan` | 扫描 input dir |
| `DELETE /documents` | 清空文档 |
| `GET /documents` | 查看 indexed files |
| `GET /health` | 查看服务状态和配置 |
| `GET /api/version` | Ollama-compatible endpoint |
| `GET /api/tags` | Ollama-compatible model list |
| `POST /api/chat` | Ollama-compatible chat |
| `POST /api/generate` | Ollama-compatible generate |

它支持多种 LLM / embedding binding：

```text
ollama
lollms
openai
azure_openai
```

这个 API 层的意义是：

```text
MiniRAG 不只是 Python library，也可以作为本地 RAG 服务挂到 Open WebUI / Ollama-compatible frontend。
```

对我们未来的 Research OS 来说，这意味着 MiniRAG 可以作为一个本地 knowledge microservice。

## Docker

仓库有：

```text
Dockerfile
docker-compose.yml
```

Dockerfile 做两阶段构建：

```text
builder installs requirements
final copies minirag and setup.py
pip install .
entrypoint python -m minirag.api.minirag_server
```

默认暴露端口：

```text
9721
```

`docker-compose.yml` 里仍然有很多 LightRAG 命名遗留：

```text
service: lightrag
network: lightrag_net
```

这也是后面可以提 PR 的小点。

## Reproduce Flow

复现实验主要在：

```text
reproduce/Step_0_index.py
reproduce/Step_1_QA.py
```

`Step_0_index.py`：

```text
load model config
load embedding model
initialize MiniRAG
find txt files under dataset path
rag.insert(file content)
```

`Step_1_QA.py`：

```text
load query_set.csv
resume from existing output csv
for each question:
    rag.query(question, QueryParam(mode="mini"))
    write question / gold answer / minirag answer
```

默认小模型选项：

| Flag | Model |
|---|---|
| `PHI` | `microsoft/Phi-3.5-mini-instruct` |
| `GLM` | `THUDM/glm-edge-1.5b-chat` |
| `MiniCPM` | `openbmb/MiniCPM3-4B` |
| `qwen` | `Qwen/Qwen2.5-3B-Instruct` |

embedding model：

```text
sentence-transformers/all-MiniLM-L6-v2
```

这进一步说明 MiniRAG 的目标就是小模型可用。

## 对 Pengyi Research OS 的启发

MiniRAG 对我们的 Research OS 有一个很具体的启发：

```text
不是所有知识记忆都要进大而全的 RAG。
我们需要一个轻量本地知识层。
```

它适合存：

| Knowledge Type | Why MiniRAG Fits |
|---|---|
| personal notes | 规模不大，但经常查询 |
| meeting records | 人物、时间、事件关系强 |
| repo study notes | project、component、concept 之间有图关系 |
| RA/PhD application material | 人、组、topic、deadline、材料之间有关联 |
| daily research logs | 多时间点、多证据、多跳问题 |
| paper reading snippets | concept/entity/claim 之间有关系 |

Research OS 可以这样分层：

```text
LightRAG
-> long-term source-grounded research memory

RAG-Anything
-> multimodal document ingestion

MiniRAG
-> local lightweight memory for notes/logs/chat/repo summaries

AnyTool
-> capability routing and tool execution

UpSkill
-> skill distillation from traces
```

这形成一条很清楚的链：

```text
documents -> RAG-Anything
knowledge graph -> LightRAG
lightweight local memory -> MiniRAG
actions -> AnyTool
skills -> UpSkill
```

## 对 Pengyi Quant Research OS 的启发

Quant Research OS 也需要轻量知识层。

很多量化知识不是大文档，而是碎片：

```text
某个因子为什么有效
某个 backtest 失败在哪里
某个数据字段的含义
某个策略的参数变更
某个 PM 的 review comment
某次 meeting 的结论
某个市场 regime 的观察
```

这类信息天然像 LiHua-World：

```text
时间序列化
碎片化
多人物/多事件/多证据
需要 multi-hop reasoning
```

所以 MiniRAG 可以成为：

```text
Quant R&D Memory Lite
```

一个可能的设计：

| Quant Memory Object | MiniRAG Entity / Relation |
|---|---|
| factor | entity |
| dataset | entity |
| market regime | entity / event |
| backtest run | event |
| metric | entity |
| PM feedback | event / interaction |
| bias diagnosis | relationship |
| code artifact | source chunk |

典型问题：

```text
Which factors failed after the liquidity filter changed?
Which datasets were used before the last turnover anomaly?
Did the signal decay issue appear before or after the universe change?
Which PM feedback mentioned look-ahead bias?
```

这类问题很适合 graph + lightweight topology retrieval。

## 和前面 HKUDS 项目的组合

MiniRAG 可以接到我们已经学过的几个 repo：

| Repo | 和 MiniRAG 的关系 |
|---|---|
| `LightRAG` | 主图谱 RAG 层，MiniRAG 是轻量版 |
| `RAG-Anything` | 负责把复杂文档解析成可进入 RAG 的内容 |
| `Vibe-Trading` | 可以把策略研究日志写入 MiniRAG |
| `AI-Researcher` | 研究过程产物可以进入 MiniRAG |
| `DeepResearch-Eval` | report factuality / quality 结果可以进入 MiniRAG |
| `AnyTool` | 可以把 MiniRAG 暴露成一个 knowledge tool |
| `UpSkill` | 可以把 MiniRAG 查询失败的 trace 变成 better retrieval skill |

一个很清楚的 Research OS flow：

```text
read paper
-> RAG-Anything parse
-> LightRAG / MiniRAG index
-> AI-Researcher generates idea
-> AnyTool executes experiments
-> DeepResearch-Eval judges report
-> UpSkill distills workflow skill
-> MiniRAG stores distilled notes and trace summaries
```

MiniRAG 是这里的轻量长期记忆层。

## 可以提的 PR

这次读代码发现几个清楚的 PR opportunity。

### PR 1: Fix README package name inconsistency

README news 里写：

```text
pip install minirag-hku
```

但安装段写：

```text
pip install lightrag-hku
```

API README 里也多处写：

```text
lightrag-hku
lightrag-server
lightrag-server.service
```

这会让新用户困惑。可以统一成 `minirag-hku` / `minirag-server`，必要时加一句说明：

```text
MiniRAG is based on LightRAG, but the package published by this repo is minirag-hku.
```

### PR 2: Fix `SearchMode.hybrid` bug

API server 里：

```python
class SearchMode(str, Enum):
    light = "light"
    naive = "naive"
    mini = "mini"
```

但 `parse_query_mode()` 默认返回：

```python
return query, SearchMode.hybrid
```

`SearchMode.hybrid` 不存在。

这会影响 Ollama-compatible chat/generate 里没有 `/light`、`/naive`、`/mini` prefix 的默认路径。

合理修复：

```text
default to SearchMode.mini
```

或者显式加 `hybrid` 并映射到 `light`，但当前 `QueryParam` 只支持 `light/naive/mini`，所以直接默认 `mini` 更一致。

### PR 3: Fix API README query examples

API README 里示例写：

```json
{"query": "Your question here", "mode": "hybrid"}
```

但当前 `SearchMode` 不支持 `hybrid`。

应该改成：

```json
{"query": "Your question here", "mode": "mini"}
```

或者说明可选值：

```text
mini / light / naive
```

### PR 4: Remove or implement missing graph endpoints

API 里有：

```python
@app.get("/graph/label/list")
async def get_graph_labels():
    return await rag.get_graph_labels()

@app.get("/graphs")
async def get_graphs(label: str):
    return await rag.get_graps(nodel_label=label, max_depth=100)
```

但 `MiniRAG` 主类里没有看到 `get_graph_labels()` 或 `get_graps()`。

这类 endpoint 要么补实现，要么暂时移除/标记 unsupported。

### PR 5: Check missing `tidb_impl.py`

`STORAGES` 里映射了：

```python
"TiDBKVStorage": ".kg.tidb_impl"
"TiDBVectorDBStorage": ".kg.tidb_impl"
"TiDBGraphStorage": ".kg.tidb_impl"
```

但本地 `minirag/kg` 里没有 `tidb_impl.py`。

如果 README 宣称支持 TiDB，这里需要补文件；如果暂时不支持，就应该从 mapping / docs 里移除，避免运行时 import error。

### PR 6: Rename Docker Compose LightRAG leftovers

`docker-compose.yml` 里：

```text
service: lightrag
network: lightrag_net
```

这不影响核心功能，但影响项目一致性。可以做一个很小的 docs/devops cleanup PR。

## 我们怎么吸收

MiniRAG 对我们的直接行动建议：

```text
为 Pengyi Research OS 做一个 lightweight memory tier。
```

不要一上来把所有东西都塞进最重的 graph RAG。

可以分三层：

| Layer | Tool |
|---|---|
| raw files | Markdown / PDF / HTML / CSV |
| heavy knowledge graph | LightRAG |
| lightweight local memory | MiniRAG |

MiniRAG 可以优先吃：

```text
我们的 repo study notes
HKUDS / LLMQuant study map
RA/PhD contact notes
quant research log
factor experiment notes
weekly planning notes
```

它的查询重点不是“长文档问答”，而是：

```text
在我们的长期碎片记录里找到人、项目、时间、事件、结论之间的关系。
```

这正好适合我们现在的工作方式。

## Next

下一篇继续 HKUDS 主线：

```text
HKUDS019 -> Paper2Slides
```

`MiniRAG` 是轻量知识层，`Paper2Slides` 会进入科研表达和产出层：

```text
MiniRAG helps remember.
Paper2Slides helps communicate.
```

Research OS 最后一定要把知识和实验变成可展示的 artifact。
