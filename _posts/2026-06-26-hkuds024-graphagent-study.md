---
title: "HKUDS024: GraphAgent 作为 Agentic Graph Language Assistant 与 Graph Reasoning Layer"
date: 2026-06-26 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds024, hkuds, graphagent, graph-llm, knowledge-graph, graph-reasoning, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第二十五篇。

```text
HKUDS024 -> GraphAgent
```

前面四篇形成了这一阶段的主线：

```text
HKUDS020 FutureShow -> forecast / judgment ledger
HKUDS021 VideoRAG   -> video memory / multimodal knowledge ingestion
HKUDS022 FastCode   -> repo-level code intelligence / coding acceleration
HKUDS023 OpenSpace  -> self-evolving agent workspace
```

这一篇进入 `GraphAgent`：

```text
GraphAgent -> graph language assistant / graph reasoning layer
```

它的位置很关键。因为我们后面不只是要把资料放进 RAG，也不只是要让 agent 调工具，而是要把研究对象之间的关系组织起来：

```text
paper
repo
author
method
dataset
factor hypothesis
asset
event
risk
backtest result
```

这些东西天然不是一条线，而是图。GraphAgent 的价值就是把文本和结构化关系都放进同一个 graph-language pipeline 里，让模型可以基于图结构做 predictive task 和 generative task。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `GraphAgent`。

| Item | Value |
|---|---|
| repo | `GraphAgent` |
| remote | `https://github.com/HKUDS/GraphAgent.git` |
| branch | `main` |
| local head | `27adee7` |
| full commit | `27adee75789890d316e87c5febe43fc8f0c2373f` |
| latest local commit date | `2025-02-08 16:58:54 +0800` |
| latest local commit | `Update README.md` |
| status | clean, synced with `origin/main` after fetch |
| paper | `GraphAgent: Agentic Graph Language Assistant`, arXiv `2412.17029` |
| main models | `GraphAgent/GraphAgent-8B`, `GraphAgent/GraphTokenizer`, `sentence-transformers/all-mpnet-base-v2` |
| tracked files by `git ls-files` | 68 |
| Python files | 45 |
| notebooks | 10 |
| shell scripts | 4 |
| main folders | `GraphAgent-inference`, `GraphAGent-training`, `assets` |
| inference entrypoints | `GraphAgent-inference/run.sh`, `GraphAgent-inference/serve_graph_agent.py` |
| training entrypoints | `GraphAGent-training/pl_train.py`, `pl_train_stage1.sh`, `pl_train_stage2.sh` |
| validation | `py -m compileall -q GraphAgent-inference GraphAGent-training` passed |
| runtime check | not run locally because full inference needs API key, HF checkpoints, large graph model, and CUDA setup |

一句话先行：

```text
GraphAgent 把 user instruction 先拆成任务，再把文本知识变成异构图，
再用 graph tokenizer 把图变成连续 graph tokens，
最后把这些 graph tokens 注入 Llama-style language model 做预测或生成。
```

所以它不是普通 GraphRAG，也不是普通 LLM agent。它更像：

```text
agentic graph construction + graph neural tokenizer + graph-language model execution
```

## 它解决什么问题

普通 RAG 通常是：

```text
documents -> chunks -> embeddings -> retrieve -> answer
```

这个结构适合做局部文本检索，但它不擅长表达复杂关系。

真实研究对象往往是：

```text
一个 paper 属于多个 topic
一个 method 依赖多个 dataset
一个 author 连接多个 institution 和 project
一个 factor 暴露多个 risk
一个资产受到多个 macro / event / liquidity channel 影响
一个 backtest result 受 universe、cost、rebalance、horizon、neutralization 共同决定
```

如果这些关系只作为文本 chunk 被检索出来，模型容易漏掉结构。GraphAgent 的判断是：

```text
真实世界同时有 explicit graph dependency 和 implicit semantic dependency。
```

所以它做两件事：

```text
1. 把已有图或文本中的隐式关系组织成 graph。
2. 让 language model 真正接收 graph token，而不是只在 prompt 里读一段图描述。
```

这就是它和普通 RAG 最大的区别。

## 总体链路

GraphAgent inference 的主链路在：

```text
GraphAgent-inference/serve_graph_agent.py
```

核心流程可以写成：

```text
user instruction / file path
  -> Task Planning Agent
  -> Graph Generation Agent
  -> PyG HeteroData
  -> Graph Tokenizer
  -> Graph Action Agent
  -> final answer
```

更展开一点：

```text
raw user input
  -> parse task:
       knowledge_text
       task_type: predictive / generative
       user_annotation
  -> extract scaffold nodes
  -> enrich scaffold-node text
  -> extract keywords
  -> build heterogeneous graph
  -> encode node text and node/edge types
  -> MetaHGT graph tokenizer
  -> insert graph embeddings into LLM input
  -> generate answer
```

这条链路非常适合我们未来的 Research OS：

```text
notes / papers / repos / datasets
  -> structured research graph
  -> graph-aware query / diagnosis / hypothesis generation
```

也非常适合 Quant OS：

```text
market data / news / filings / factors / assets / backtests
  -> factor-event-asset-risk graph
  -> graph-aware alpha research assistant
```

## Component 1: Task Planning Agent

目录：

```text
GraphAgent-inference/task_planning_agent
```

它的任务不是直接回答，而是先把用户输入整理成结构化任务。

输出 schema 大致是：

```text
knowledge_text
task_type
user_annotation
```

其中 `task_type` 只有两类：

```text
predictive
generative
```

示例：

```text
paper classification              -> predictive
paper acceptance prediction       -> predictive
long report summarization         -> generative
related work generation           -> generative
relationship / story explanation  -> generative
```

这一步很重要。因为如果没有 task planning，后面的 graph execution 就不知道自己是在做分类、判断、摘要还是生成。

对我们来说，这给了一个很直接的启发：

```text
Quant Research OS 也应该先把任务类型显式化。
```

比如：

```text
factor hypothesis generation  -> generative
factor implementation         -> code generation / development
factor validation             -> predictive / diagnostic
backtest bias diagnosis       -> diagnostic
next experiment planning      -> planning
portfolio risk explanation    -> generative + graph reasoning
```

不要让所有任务都混成一句 prompt。任务类型一旦显式，系统就可以选择不同工具、不同验证方式和不同输出格式。

## Component 2: Graph Generation Agent

目录：

```text
GraphAgent-inference/graph_generation_agent
```

Graph Generation Agent 做的是从文本中构图。

它不是简单 NER，而是三段式：

```text
1. scaffold node extraction
2. scaffold text parsing
3. keyword extraction
```

然后再落到 PyTorch Geometric 的 `HeteroData`：

```text
scaffold nodes
  -> node types
  -> node descriptions
  -> keyword nodes
  -> has_keyword edges
  -> optional has_property edges
  -> HeteroData
```

这个设计很值得吸收。因为它不是直接抽一堆实体，而是先抽高层 scaffold node。

举例：

```text
paper
research_background
research_question
methodology
key_results
keyword
```

或者：

```text
movie
author
paper
subject
term
review
```

对研究系统来说，scaffold node 就是知识对象的骨架。它解决的是：

```text
这段材料到底应该被组织成哪些可复用知识节点？
```

如果把它迁移到我们的量化系统，可以变成：

```text
factor
data_source
universe
horizon
risk_exposure
market_regime
transaction_cost
backtest_metric
failure_mode
next_experiment
```

这样一篇研究笔记、一个 backtest 报告、一段访谈、一个交易策略，都可以被结构化成图。

## Component 3: Graph Tokenizer

目录：

```text
GraphAgent-inference/graph_tokenizer
```

这是 GraphAgent 最核心的地方之一。

普通做法可能是：

```text
graph -> textual description -> put into prompt
```

GraphAgent 更进一步：

```text
graph -> node text embeddings + type embeddings + edge type embeddings
      -> MetaHGT graph encoder
      -> continuous graph tokens
      -> language model input
```

它用到几个特殊 token：

```text
<graph>
<g_patch>
<g_start>
<g_end>
```

`build_input.py` 会把每一种 node type 对应成 prompt 里的 graph placeholder：

```text
"paper" nodes: <graph>
"keyword" nodes: <graph>
...
```

然后真正送进模型时，这些 token 位置会被 graph embeddings 替换。

这个思路类似多模态模型：

```text
image patches -> visual tokens -> LLM
graph patches -> graph tokens  -> LLM
```

它的意义是：

```text
图结构不是只作为文字提示，而是作为连续向量进入语言模型上下文。
```

这也是 GraphAgent 比普通 graph prompt 更强的地方。

## Component 4: Graph Action Agent

目录：

```text
GraphAgent-inference/graph_action_agent
```

Graph Action Agent 负责真正执行任务。它加载：

```text
HeteroGraphLLMForCausalLM
```

这个模型本质上是一个 Llama-style causal LM，但它多了 graph 输入通道：

```text
input_ids
attention_mask
graph_data
hetero_key_order
```

核心机制在 `HeteroGraphLLMModel.forward`：

```text
1. 正常 token 先变成 text embedding。
2. graph_data 里的各类 node embedding 经过 graph_projector。
3. 找到 <g_start> ... <g_end> 或 <g_patch> 位置。
4. 把 graph embedding 拼进原始 text embedding。
5. 再走 Llama decoder 和 LM head。
```

这就把 graph reasoning 和 language generation 接起来了。

所以 GraphAgent 的执行不是：

```text
LLM read graph text and answer
```

而是：

```text
LLM receives graph embeddings and text prompt together.
```

这对复杂关系任务有价值，尤其是：

```text
node classification
paper judgement prediction
related work generation
long report summarization
```

## Training Side

训练代码在：

```text
GraphAGent-training
```

注意目录名是 `GraphAGent-training`，大小写有点不一致。

训练入口：

```text
pl_train.py
pl_train_stage1.sh
pl_train_stage2.sh
```

训练框架是：

```text
Lightning + Transformers + PyTorch Geometric
```

数据配置在：

```text
GraphAGent-training/config/data_config.yaml
```

里面配置了很多任务和图数据：

```text
BBH_movie
NLP_related_works
DBLP
ACM
ICLR_peer_review
Arxiv2023
IMDB_fewshot_train
ACM_test_1000
```

训练可以理解成两段：

```text
stage 1: graph-language alignment
stage 2: downstream task adaptation / few-shot task training
```

stage 1 脚本使用：

```text
stage_1_mix_with_higpt
```

stage 2 脚本示例使用：

```text
stage_2_dual_graph_imdb_few_shot_40
```

模型训练时会控制哪些参数可训练：

```text
tune_graph_mlp_adapter
tune_embed_tokens
full_finetune
tune_gnn
freeze_backbone
```

这说明它不是只写一个 demo pipeline，而是有完整的图语言模型训练思路：

```text
graph tokenizer / graph projector / special graph tokens / LLM backbone
```

这一点对我们很重要。因为未来如果我们真的做 Quant Research OS 的 graph layer，短期可以先不训练大模型，但长期路径会是：

```text
domain graph construction
  -> graph-text alignment
  -> task-specific graph-language fine-tuning
```

## Dataset 与 Benchmark

README 里的任务覆盖两类：

```text
Predictive tasks:
  - IMDB node classification
  - ACM node classification
  - Arxiv paper classification
  - ICLR paper judgement prediction

Generative tasks:
  - related work generation
  - GovReport summarization
```

这组任务设计很聪明，因为它同时验证：

```text
图上预测能力
文本生成能力
图结构与长文本语义融合能力
```

README 里的 benchmark 重点有三个：

```text
1. ACM-1000 zero-shot classification
2. Arxiv-Papers / ICLR-Peer Reviews complex predictive tasks
3. Related work generation perplexity evaluation
```

其中 ACM-1000 的结果显示，GraphAgent 在多个设定下超过 SAGE、GAT、HAN、HGT、HetGNN、HiGPT。比如 IMDB-40 迁移到 ACM-1000 的 Micro-F1，README 表里 GraphAgent 是 74.98，高于 HiGPT 的 50.50。

这说明它不是只做文本 agent，而是在图任务上确实有实验支撑。

## 和 LightRAG / VideoRAG / QuantMind 的关系

我们前面看过很多 RAG 和知识系统。GraphAgent 可以这样定位：

| System | 更像什么 | 核心能力 |
|---|---|---|
| LightRAG / MiniRAG | graph-enhanced retrieval | 从知识图谱和文本 chunk 中检索证据 |
| VideoRAG | multimodal video knowledge ingestion | 把视频切成可检索、可定位、可问答的知识对象 |
| QuantMind | structured quant knowledge base | 把 paper/news/blog/PDF 转成可复用 quant knowledge |
| GraphAgent | graph-language execution model | 把任务、图结构、语言模型执行连成一个 agentic pipeline |

最关键的区别是：

```text
RAG 系统重点是 retrieve。
GraphAgent 重点是 graph-conditioned task execution。
```

换句话说：

```text
LightRAG 问：我应该取哪些知识？
GraphAgent 问：我能不能基于图结构完成预测或生成任务？
```

这对我们的系统设计很有启发。Research OS 需要两层：

```text
retrieval layer:
  找到相关材料、证据、原文、代码、实验。

graph reasoning layer:
  把材料之间的关系显式组织起来，再做判断、生成、诊断、规划。
```

GraphAgent 属于第二层。

## 对 Pengyi Research OS 的意义

Research OS 未来会积累大量对象：

```text
papers
repos
authors
labs
methods
datasets
benchmarks
blog notes
interview notes
applications
PR opportunities
research questions
```

如果只放 markdown，会越来越散。如果只放向量库，会能搜但不好组织。如果用图结构，就可以问：

```text
哪些 HKUDS 项目都在做 graph reasoning？
哪些 repo 可以连接到 Quant Research OS？
哪些 paper / project 适合写成顶会 work？
哪些导师/实验室/项目和我们的路线最相近？
某个 idea 需要补哪些代码、数据、benchmark？
```

GraphAgent 给我们的启发是：

```text
每篇笔记都应该被转换成 research graph object。
```

比如这篇 HKUDS024 本身可以抽成：

```text
project: GraphAgent
method: task planning
method: graph generation
method: graph tokenizer
method: graph-language model
application: predictive task
application: generative task
application: Research OS
application: Quant OS
pr_opportunity: README/model mismatch
pr_opportunity: hardcoded CUDA
```

这就不是一篇孤立文章，而是我们知识图里的一个节点。

## 对 Pengyi Quant Research OS 的意义

Quant Research OS 里最适合图化的对象包括：

```text
factor
data source
asset universe
industry
macro event
news event
filing
management signal
risk exposure
transaction cost
backtest
portfolio
failure mode
next experiment
```

一条因子研究链路可以被图化成：

```text
factor hypothesis
  -> required data
  -> implementation
  -> universe
  -> neutralization
  -> cost assumption
  -> backtest result
  -> risk exposure
  -> failure diagnosis
  -> next hypothesis
```

这和我们一直说的 R&D Agent 非常接近：

```text
自动提出因子假设
自动实现
自动回测
自动诊断偏差
自动生成下一轮研究计划
人类 PM 审核
```

GraphAgent 可以启发其中的 memory 和 reasoning layer：

```text
factor research history should be a graph, not a folder of disconnected notebooks.
```

这样未来才能问：

```text
哪些失败因子共享同一种风险暴露？
哪些数据源反复产生看似有效但成本后失效的信号？
哪些假设在 small-cap 上有效但在 liquidity filter 后消失？
哪些 WorldQuant-style formula 可以迁移成更稳健的 feature family？
```

这些问题靠普通文本检索会很吃力，靠图结构会更自然。

## 我们怎么吸收

短期不要直接复刻 GraphAgent-8B。这个成本太高，也不是我们现在最需要的。

我们的 v0 应该更轻：

```text
markdown notes / PDFs / repo summaries
  -> extract entities and relations
  -> save as JSON / SQLite / NetworkX
  -> build searchable graph
  -> use LLM to query / update / diagnose
```

第一版可以不训练 graph-language model，而是做：

```text
1. schema design
2. graph extraction
3. graph storage
4. graph query
5. graph-to-prompt rendering
6. research task templates
```

等对象和任务足够多，再考虑：

```text
graph embedding
graph retrieval
graph neural encoder
graph-language fine-tuning
```

这条路线更实际。

对我们当前网站和笔记系统，最先可以落地的是：

```text
每篇学习地图文章末尾自动生成 structured metadata：
  project
  tasks
  methods
  components
  relation_to_research_os
  relation_to_quant_os
  pr_opportunities
```

然后把这些 metadata 汇总成一个 `Pengyi Knowledge Graph`。

## 可以提 PR 的地方

GraphAgent 目前有几个很具体、适合贡献的小问题。

第一，`serve_graph_agent.py` 顶部有一个疑似无效 import：

```text
from graph_agent import GraphActionAgent
```

本地文件树里没有对应的 `graph_agent.py`。后面又有正确导入：

```text
from graph_action_agent.agent import GraphActionAgent
```

这可能会导致直接运行时 import 失败。可以提一个小 PR 删除无效 import 和重复 import。

第二，`serve_graph_agent.py` 里规划阶段已经得到 `task_type`，但最后调用：

```text
graph_action_agent.invoke(user_instruction, grounded_graph_with_emb_gnn, "generative")
```

这里硬编码成了 `"generative"`。如果用户任务是 paper classification 或 paper acceptance prediction，这个 task type 可能没有被正确传下去。更合理的是把 Task Planning Agent 的 `task_type` 传入 action agent。

第三，README 顶部 Hugging Face badge 指向：

```text
GraphAgent/GraphAgent-7B
```

但 README 模型列表和 `run.sh` 使用：

```text
GraphAgent/GraphAgent-8B
```

这需要统一。

第四，`graph_tokenizer.py` 里有硬编码：

```text
device = 'cuda:0'
```

并且 SentenceTransformer 在模块 import 时就根据环境变量加载。这样对 CPU/MPS/多卡环境都不友好，也会让 import 失败更早发生。可以改成从参数或环境变量读取 device，并在缺少环境变量时给出清晰错误。

第五，README 说模型可以自动下载，但 `load_graph_tokenizer_pretrained` 里对 `pretrain_model_path` 使用了本地路径断言。如果用户传的是 Hugging Face repo id，可能和 README 预期不一致。可以补 `snapshot_download`，或者在 README 里明确需要先下载到本地目录。

第六，`requirements.txt` 里有：

```text
torch==2.2.1+cu118
```

这个通常需要 PyTorch CUDA wheel index，不是普通 `pip install -r requirements.txt` 就一定能装成功。README 可以补 PyTorch 安装命令。

第七，README 里 `Training GraphAgent with Your Own Data` 仍写着 coming soon，但仓库已经包含 training code 和 stage scripts。文档状态可以更新。

这些都不是大改，但很适合作为我们对 HKUDS 项目的真实贡献入口。

## 和我们主线的连接

GraphAgent 给我们的最大启发是：

```text
Research OS 不能只有文件夹。
Quant OS 不能只有 notebook。
真正的长期研究系统，需要一个 graph memory。
```

我们后面要做的不是把所有知识堆进一个巨大的向量库，而是要逐渐形成：

```text
Artifact Layer:
  markdown / PDF / code / backtest / website post

Retrieval Layer:
  text chunks / embeddings / BM25 / source citations

Graph Layer:
  project / paper / factor / dataset / result / risk / person / organization

Agent Layer:
  planning / implementation / execution / diagnosis / next research plan

Human PM Layer:
  review / approve / reject / redirect / prioritize
```

GraphAgent 在这里对应第三层和第四层之间的桥：

```text
graph memory -> graph-aware agent execution
```

这就是 HKUDS024 对我们的价值。

## 下一步

HKUDS024 打开的是 graph / knowledge graph 主线。后面可以继续看：

```text
OpenGraph
GraphGPT
HiGPT
GraphAgent adjacent graph reasoning projects
```

但对我们自己的系统来说，最应该先做的不是训练大模型，而是：

```text
把我们已经写过的 HKUDS / LLMQuant 学习文章抽成一个 Pengyi Research Knowledge Graph。
```

先有结构化资产，再谈更强的 agent。
