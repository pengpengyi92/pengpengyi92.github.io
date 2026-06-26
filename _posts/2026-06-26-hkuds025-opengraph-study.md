---
title: "HKUDS025: OpenGraph 作为 Open Graph Foundation Model 与 Zero-Shot Graph Generalization Layer"
date: 2026-06-26 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds025, hkuds, opengraph, graph-foundation-model, zero-shot-graph-learning, graph-transformer, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第二十六篇。

```text
HKUDS025 -> OpenGraph
```

上一篇 `HKUDS024` 看的是 `GraphAgent`：

```text
GraphAgent -> agentic graph language assistant
```

这一篇继续图智能主线，进入 `OpenGraph`：

```text
OpenGraph -> open graph foundation model
```

这两篇要连起来看。

`GraphAgent` 关注的是：

```text
user instruction
  -> task planning
  -> graph generation
  -> graph tokenizer
  -> graph-language model execution
```

`OpenGraph` 关注的是：

```text
graph itself
  -> unified graph tokenizer
  -> topology-aware projection
  -> scalable graph transformer
  -> zero-shot graph learning
```

也就是说，GraphAgent 更像把 graph 放进 agent 和 language model 工作流；OpenGraph 更像把 graph model 本身做成可以跨数据集泛化的 foundation model。

这对我们非常关键。因为未来的 Research OS / Quant OS 不是只有一个固定图，而是会不断遇到新图：

```text
paper graph
repo dependency graph
author-lab-project graph
factor-data-risk graph
asset-event-industry graph
client-company-credit graph
market-regime-transition graph
```

如果每换一个图都要重新训练一个 GNN，系统就不够 open。OpenGraph 想解决的正是这个问题：

```text
让图模型对 unseen graph data 也有 zero-shot generalization。
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `OpenGraph`。

| Item | Value |
|---|---|
| repo | `OpenGraph` |
| remote | `https://github.com/HKUDS/OpenGraph.git` |
| branch | `main` |
| local head | `152b8b6` |
| full commit | `152b8b6439a281e46bd569a92c69f0c59f98a902` |
| latest local commit date | `2024-10-11 12:10:39 +0800` |
| latest local commit | `Update README.md` |
| status | clean, synced with `origin/main` after fetch |
| paper | `OpenGraph: Towards Open Graph Foundation Models`, EMNLP 2024 |
| authors | Lianghao Xia, Ben Kao, Chao Huang |
| tracked files by `git ls-files` | 116 |
| Python files | 18 |
| markdown files | 1 |
| notebooks | 0 |
| main folders | `link_prediction`, `node_classification`, `graph_generation`, `datasets`, `Models`, `History`, `imgs` |
| dataset folders | `gen0`, `gen1`, `gen2`, `ml1m`, `ml10m`, `collab`, `amazon-book`, `ddi`, `cora`, `citeseer`, `pubmed` |
| environment in README | Python 3.10.13, torch 1.13.0, numpy 1.23.4, scipy 1.9.3 |
| package metadata | no `requirements.txt`, no `pyproject.toml`, no `setup.py` |
| pretrained model note | `Models/readme` points to a Google Drive folder |
| validation | `py -m compileall -q link_prediction node_classification graph_generation` passed |
| runtime check | not run locally because runtime needs CUDA, unzipped datasets, optional pretrained models, and undocumented deps such as `setproctitle` / `torch_geometric` |

一句话先行：

```text
OpenGraph 用 LLM 生成合成图数据，用统一 graph tokenizer 把不同图投影到共同 latent space，
再用 topology smoothing + anchor-sampled graph transformer 学到跨图可迁移的图表示。
```

它的核心不是“在某个图上做高分”，而是：

```text
在训练图之外的新图上保持 zero-shot graph learning 能力。
```

## 它解决什么问题

传统 GNN 有一个很大的限制：

```text
每个图的数据分布、节点数量、边结构、节点特征、任务标签空间都可能不同。
```

比如：

```text
MovieLens 是 user-item interaction graph
Amazon-Book 是 user-book interaction graph
OGBL-Collab 是作者合作图
DDI 是 drug-drug interaction graph
Cora / Citeseer / Pubmed 是 citation graph
```

如果一个模型在一个图上训练，它通常不能自然迁移到另一个图。原因包括：

```text
node id 不共享
feature space 不共享
graph scale 不同
degree distribution 不同
homophily / heterophily 不同
task label space 不同
```

OpenGraph 的目标是把图学习推向 foundation model：

```text
不为每个图从零训练一个专用模型，
而是训练一个能适配不同 unseen graph 的开放图基础模型。
```

README 里对目标的表达很直接：

```text
distilling zero-shot graph generalizability from LLMs
```

这里的关键是 `zero-shot` 和 `generalizability`。这和我们做 Research OS / Quant OS 很贴近，因为我们未来面对的图也会不断变：

```text
今天是 HKUDS repo graph
明天是 LLMQuant project graph
后天是 WorldQuant factor graph
再之后是 A-share asset-event graph
```

系统不能每次换图就重新开始。

## 总体结构

OpenGraph 可以拆成四层：

| Layer | Code | Role |
|---|---|---|
| LLM Data Generation | `graph_generation` | 用 LLM 生成合成图数据，缓解真实图数据稀缺 |
| Unified Graph Tokenizer | `InitialProjector` | 把不同图投影到统一 latent dimension |
| Topology Encoder | `TopoEncoder` | 用 adjacency smoothing 注入局部拓扑 |
| Scalable Graph Transformer | `GraphTransformer` / `GTLayer` | 用 anchor sampling 做全局依赖建模 |
| Task Heads / Evaluation | `link_prediction`, `node_classification` | 评估 link prediction / node classification 的跨图泛化 |

主链路可以写成：

```text
graph adjacency
  -> initial projection / tokenizer
  -> topology smoothing
  -> anchor-sampled graph transformer
  -> node embeddings
  -> link prediction or node classification
```

如果是预训练数据构造，则是：

```text
LLM prompts
  -> entity/category tree
  -> instance number estimation
  -> text embedding
  -> Gibbs-style human-item relation generation
  -> adjacency matrix
  -> generated graph dataset
```

所以它不是一个纯模型仓库，也不是一个纯数据生成仓库，而是两者合在一起：

```text
synthetic graph data generation + graph foundation model training/evaluation
```

## Component 1: LLM-Enhanced Graph Data Generation

目录：

```text
graph_generation
```

README 给出的生成流程是：

```text
python itemCollecting_dfsIterator.py
python instance_number_estimation_hierarchical.py
python embedding_generation.py
python human_item_generation_gibbsSampling_embedEstimation.py
python make_adjs.py
```

这条链路大致是：

```text
1. 用 LLM 从一个场景和根实体开始枚举子类别。
2. 构造层级 entity tree。
3. 估计每个类别应该生成多少实例。
4. 用 text embedding 表示每个 item。
5. 用 Gibbs sampling 风格方法生成 human-item interactions。
6. 生成 train / valid / test adjacency matrices。
```

默认示例是：

```text
entity   = products
scenario = e-commerce platform like Amazon
depth    = 3
```

这会生成电商商品类别树和交互数据，最后落到：

```text
gen_results/datasets/gen_data_ecommerce
```

这一步是 OpenGraph 的一个关键思想：

```text
用 LLM 的世界知识补足图数据稀缺问题。
```

对我们来说，这很有启发。因为量化研究里也会有数据稀缺和场景稀缺：

```text
某些市场 regime 样本少
某些风险事件样本少
某些新行业图关系没有足够历史
某些事件-资产传播路径很难直接标注
```

短期不能把 LLM 生成的数据直接当真实金融信号，但可以用它生成：

```text
scenario graph
event taxonomy
risk propagation template
factor failure mode taxonomy
company-supply-chain schema
```

这些可以成为 Research OS 的结构化先验。

## Component 2: Unified Graph Tokenizer

OpenGraph 的 graph tokenizer 主要体现在：

```text
InitialProjector
```

位置：

```text
link_prediction/model.py
node_classification/model.py
```

它要解决的问题是：

```text
不同 graph 没有共享 node vocabulary，也不一定有统一 feature space。
```

所以它先把图投影到固定维度：

```text
args.latdim = 1024
```

支持几种初始化方式：

```text
uniform
lowrank_uniform
svd
both
id
```

默认是：

```text
proj_method = svd
```

SVD 版本会对 adjacency 做低秩分解，再生成 node embedding：

```text
adj -> svd_lowrank -> U / S / V -> fixed-dimensional initial embeddings
```

这就是它的“统一 graph tokenizer”雏形：

```text
不管原图来自哪个数据集，
先把节点映射到统一 latent dimension，
再让后续 encoder 处理。
```

这和语言模型的 tokenizer 有一点类比：

```text
text tokenizer:
  arbitrary text -> token ids / embeddings

graph tokenizer:
  arbitrary graph -> initial node embeddings
```

但图更难，因为 graph 没有天然词表。OpenGraph 通过 topology-aware projection 来做适配。

## Component 3: Topology Encoder

`TopoEncoder` 做的是 adjacency smoothing。

代码逻辑很直接：

```text
embeds = LayerNorm(embeds)
for i in range(args.gnn_layer):
    embeds = sparse_adj @ embeds
    embeds_list.append(embeds)
embeds = sum(embeds_list)
```

默认：

```text
gnn_layer = 3
```

这意味着模型会把 1-hop、2-hop、3-hop 的结构信息平滑进 node embedding。

README 的 graph tokenizer study 也强调：

```text
Adjacency smoothing is important.
```

如果不做 smoothing，单纯的初始投影很难表达图拓扑。对跨图泛化来说，topology smoothing 是非常关键的 inductive bias：

```text
不是记住某个图的节点，
而是学习可迁移的拓扑模式。
```

对量化系统来说，这个思想很有用。比如资产图里：

```text
资产 A 受同产业链节点影响
资产 B 与同主题事件相邻
资产 C 与同资金流/风格因子相邻
```

邻接 smoothing 本质上是在说：

```text
一个节点的表示应该吸收邻居和多阶邻域的信息。
```

这非常适合行业链、供应链、新闻事件传播、风险扩散和 factor family graph。

## Component 4: Scalable Graph Transformer

`GraphTransformer` 由多个 `GTLayer` 组成：

```text
gt_layer = 4
head     = 4
anchor   = 256
```

标准 Transformer 如果对所有节点做全量 attention，复杂度会接近：

```text
O(N^2)
```

大图上会很贵。OpenGraph 的 `GTLayer` 做了 anchor sampling：

```text
1. 随机选 anchor nodes。
2. anchor nodes attend to all nodes。
3. all nodes attend back to anchor nodes。
4. 再过 feed-forward 和 layer norm。
```

代码里是：

```text
anchor_embeds = self._pick_anchors(embeds)
anchor_embeds = MultiheadAttention(anchor, embeds, embeds)
embeds        = MultiheadAttention(embeds, anchor, anchor)
```

这相当于用一组 sampled anchors 作为信息瓶颈，把全局上下文压缩后再广播回所有节点。

README 的 sampling study 也说明：

```text
sampling techniques help memory and time cost,
and token sequence sampling can even help performance.
```

这对我们也很重要。因为 Research OS / Quant OS 的图会越来越大：

```text
几百篇 paper
几千个 repo / functions / classes
几万个 factor result
几万条 news / event / asset links
```

如果每次都全图 attention，系统很快不可用。Anchor sampling 给了一个实用方向：

```text
用关键节点 / sampled anchors / representative nodes 做全局压缩。
```

## Link Prediction 主线

目录：

```text
link_prediction
```

这是 OpenGraph 的预训练和推荐/链接预测主线。

README 复现实验的命令包括：

```text
cd link_prediction/
python main.py --load pretrn_gen1 --epoch 0
python main.py --load pretrn_gen0 --tstdata amazon-book --epoch 0
python main.py --load pretrn_gen2 --tstdata ddi --epoch 0
```

预训练命令包括：

```text
python main.py --save pretrn_gen1
python main.py --trndata gen0 --tstdata amazon-book --save pretrn_gen0
python main.py --trndata gen2 --tstdata ddi --save pretrn_gen2
```

`main.py` 里默认：

```text
trn_datasets = ['gen1']
tst_datasets = ['ml1m', 'ml10m', 'collab']
```

这说明它希望用生成图 `gen1` 去迁移到真实图 `ml1m / ml10m / collab`。

训练逻辑：

```text
positive edge: (anchor, positive)
negative edge: (anchor, negative)
mask positive edge from adjacency
topology encode
graph transformer
dot product score
contrastive / softmax-like loss
```

评测指标：

```text
Recall@K
NDCG@K
```

这条线对量化有直接映射：

```text
link prediction = 预测两个节点之间未来是否应该连接。
```

在金融里可以类比：

```text
asset-event link
factor-asset link
company-supply-chain link
theme-stock link
risk-factor link
research-paper-method link
```

尤其是 factor research：

```text
一个新因子是否应该连接某类资产？
一个风险事件是否会影响某个行业？
一个策略是否和某类 market regime 相关？
```

这些都可以被抽象成 graph link prediction。

## Node Classification 主线

目录：

```text
node_classification
```

README 给出的测试命令：

```text
cd node_classification/
python main.py --load pretrn_gen1 --tstdata cora
python main.py --load pretrn_gen1 --tstdata citeseer
python main.py --load pretrn_gen1 --tstdata pubmed
```

节点分类分支复用了同一套核心模块：

```text
InitialProjector
TopoEncoder
GraphTransformer
Masker
OpenGraph
```

不同点在于任务输出。`pred_for_node_test` 会：

```text
1. 重新跑全图 embedding。
2. 取待分类 node embeddings。
3. 取最后 class_num 个 class embeddings。
4. 用 dot product 选择最匹配类别。
```

也就是说，node classification 被转成：

```text
node embedding -> nearest / highest-score class embedding
```

这和 link prediction 的形式很统一：

```text
link prediction:
  node A embedding dot node B embedding

node classification:
  node embedding dot class embedding
```

这种统一是 foundation model 思路里很重要的一点：

```text
尽量把不同 graph tasks 转换成统一的 embedding scoring 问题。
```

对 Research OS 来说，这可以映射成：

```text
给一个 paper/repo/project/factor 自动分类到某个 research theme。
```

对 Quant OS 来说，可以映射成：

```text
给一个 factor 自动分类到 value / momentum / quality / sentiment / event / risk / liquidity family。
```

## Evaluation Results

README 展示了几组结果：

```text
Overall Generalization Performance
Pre-training Dataset Study
Graph Tokenizer Study
Sampling Techniques Study
```

结论可以概括为：

```text
1. OpenGraph 在 0-shot setting 下表现强于一些 1-shot / 5-shot baselines。
2. 生成数据里的 Norm / Loc / Topo 技术对性能有正向作用。
3. 预训练数据的相关性很重要，例如 ML-10M 对 ML-1M / ML-10M 迁移更好。
4. adjacency smoothing 很重要。
5. topology-aware projection 比 one-hot / random / degree 替代方案更好。
6. anchor sampling 同时改善 memory / time cost，并可能改善 performance。
```

这里最值得我们吸收的是第三点：

```text
foundation model 不是说预训练数据随便来都行。
预训练图和目标图之间仍然需要 domain relevance。
```

对应到量化：

```text
用电商推荐图预训练，不一定能直接迁移到股票事件图。
用供应链/行业/主题/资产相关性图预训练，可能更适合金融图任务。
```

所以未来如果我们做 Quant Graph Foundation Layer，预训练图应该围绕：

```text
asset co-movement
industry hierarchy
supply chain
company-event links
factor-family links
news-topic-stock links
portfolio exposure graph
```

而不是随便拿一个通用图。

## 和 GraphAgent 的区别

GraphAgent 和 OpenGraph 都在图智能主线，但系统位置不同。

| Project | 更像什么 | 输入 | 核心输出 |
|---|---|---|---|
| GraphAgent | agentic graph-language assistant | user instruction + text / graph | graph-conditioned answer / prediction |
| OpenGraph | graph foundation model | graph adjacency / generated graph data | zero-shot transferable node embeddings |

GraphAgent 解决的是：

```text
如何让 agent 构图、理解任务、调用 graph-language model 完成预测或生成。
```

OpenGraph 解决的是：

```text
如何让 graph model 本身跨图泛化。
```

如果两者结合，合理结构是：

```text
GraphAgent:
  负责 task planning / graph generation / language interaction。

OpenGraph:
  负责 graph encoding / zero-shot topology representation。
```

放进我们的系统，可以是：

```text
Research OS / Quant OS
  -> GraphAgent-style planner decides what graph task to run
  -> OpenGraph-style encoder computes graph representations
  -> LLM writes explanation / diagnosis / next plan
```

这就是 `HKUDS024 -> HKUDS025` 的连续价值。

## 对 Pengyi Research OS 的意义

Research OS 未来会有很多图：

```text
project graph
paper citation graph
method dependency graph
repo architecture graph
person-lab-institution graph
learning-topic graph
PR opportunity graph
```

一开始我们可以用简单 JSON / NetworkX / SQLite 存储它们。但随着对象变多，问题会变成：

```text
如何对新 project graph 做分类？
如何预测两个 idea 是否应该连接？
如何判断某个 repo 属于哪条主线？
如何发现尚未连接但应该连接的 paper / method / project？
```

这就是 OpenGraph 可以启发的地方。

它告诉我们：

```text
Research OS 的 graph layer 不应该只是存图。
它最终应该能做 graph representation learning。
```

短期我们先做轻量版：

```text
node schema
edge schema
graph extraction
graph query
graph visualization
graph-to-prompt
```

中期可以做：

```text
node embedding
link prediction
theme classification
research opportunity recommendation
```

长期再考虑：

```text
OpenGraph-style graph foundation model for research objects
```

## 对 Pengyi Quant Research OS 的意义

Quant OS 的图结构更明显。

可以直接建成：

```text
asset graph:
  stock -> industry -> theme -> supply chain -> macro exposure

factor graph:
  factor -> data source -> universe -> horizon -> risk -> backtest result

event graph:
  event -> company -> industry -> asset -> price reaction

portfolio graph:
  holding -> exposure -> constraint -> risk source -> pnl attribution
```

OpenGraph 的 zero-shot graph generalization 对量化很关键，因为金融图一直在变：

```text
新行业出现
新主题出现
新政策事件出现
新公司上市
新因子被发现
旧关系失效
```

如果 graph model 只能在固定资产池里工作，就不够好。我们需要的是：

```text
new graph, still usable.
```

这可以变成我们自己的 Quant Graph Layer：

```text
worldquant factor formulas
  -> factor family graph
  -> factor-asset relation graph
  -> factor-risk relation graph
  -> factor-failure graph
  -> next factor recommendation
```

尤其是我们后面要整理 WorldQuant 因子库，不能只做列表。更好的方式是：

```text
每个 factor 都成为 graph node。
每个 formula component / operator / data field / decay / neutralization 都成为 relation。
每个 backtest result / failure mode / risk exposure 都成为 relation。
```

然后我们才能问：

```text
哪些 factor family 在成本后更容易失效？
哪些 operator 组合经常带来 look-ahead / liquidity bias？
哪些 data field 和哪些 horizon 最相关？
哪些失败案例可以合成新的 hypothesis？
```

OpenGraph 不是直接解决这些问题，但它给了底层路线：

```text
把 graph 变成可泛化的表征空间。
```

## 我们怎么吸收

不要一开始就复刻 OpenGraph 的完整训练。

我们的 v0 应该是：

```text
1. 先定义 Research OS / Quant OS 的 graph schema。
2. 把现有 HKUDS / LLMQuant 文章抽成 nodes and edges。
3. 建一个简单 graph store。
4. 做 graph query 和 graph visualization。
5. 做 simple link prediction / node classification demo。
```

一个实际 v0 可以是：

```text
nodes:
  Project
  Method
  Dataset
  Task
  Paper
  Factor
  Risk
  Backtest
  PR_Opportunity

edges:
  uses
  improves
  belongs_to
  depends_on
  evaluates_on
  connects_to_quant_os
  has_failure_mode
  suggests_next
```

然后做两个小任务：

```text
1. node classification:
   给一个新 project 自动分到 RAG / Agent / Graph / Quant / Research Engineering。

2. link prediction:
   预测某个 project 是否应该连接到某个 Quant OS module。
```

这会比一开始训练大模型更实际，也更贴近我们的公开 portfolio。

## 可以提 PR 的地方

OpenGraph 目前有一些具体、可落地的小改进点。

第一，仓库没有 `requirements.txt` / `environment.yml` / `pyproject.toml`，README 只列了几个核心版本：

```text
python==3.10.13
torch==1.13.0
numpy==1.23.4
scipy==1.9.3
```

但代码还用到：

```text
setproctitle
torch_geometric
sklearn
openai
tiktoken
networkx
```

可以补一个最小 `requirements.txt` 或 `environment.yml`。

第二，`graph_generation/Utils.py` 和 `graph_generation/itemCollecting_dfsIterator.py` 里有硬编码占位 API key：

```text
openai.api_key = "xx-xxxxxx"
openai.api_key = "xxxxxx"
```

更好的做法是从环境变量读取：

```text
OPENAI_API_KEY
```

第三，OpenAI Python SDK 写法是旧版：

```text
openai.ChatCompletion.create
openai.Embedding.create
```

如果用户装新版 `openai`，可能会出兼容问题。可以在 requirements 里 pin 旧版，或者迁移到新版 client。

第四，`link_prediction/model.py` 和 `node_classification/model.py` 里有重复模型实现，两个文件大段几乎一致。可以抽到共享模块，减少维护成本。

第五，`OpenGraph.forward` 里调用：

```text
self.topoEncoder(adj, initial_projector(), user_num)
```

但 `TopoEncoder.forward` 的签名是：

```text
forward(self, adj, embeds)
```

目前主训练和测试路径用的是 `cal_loss` / `pred_for_test` / `pred_for_node_test`，绕开了 `OpenGraph.forward`，所以 compileall 不会报错。但如果外部用户直接调用 `model(adj, projector, user_num)`，可能会触发 `TypeError`。这适合提一个小 PR。

第六，代码大量默认 CUDA：

```text
args.devices = ['cuda:0', 'cuda:0']
.cuda()
```

README 可以明确 GPU requirement，或者代码里增加 CPU fallback。

第七，README 有一些拼写和文档小问题：

```text
achives -> achieves
Suprisingly -> Surprisingly
gaphs -> graphs
wheter -> whether
inadverdently -> inadvertently
```

这类小 PR 很适合先建立 contributor 关系。

第八，`Models/readme` 只有 Google Drive 链接，没有说明具体文件名、放置路径、校验信息。可以补一个更明确的模型下载说明。

第九，部分数据需要手动 unzip。可以补一个小脚本：

```text
scripts/prepare_data.py
```

自动解压 `datasets` 下的 `.zip` 文件，并检查缺失文件。

这些都是比较合适的贡献入口，不需要改论文核心算法。

## 和我们的主线连接

GraphAgent 和 OpenGraph 这两篇合起来，给了我们一个清晰判断：

```text
Research OS / Quant OS 的图层不应该只停留在知识图谱存储。
它应该逐步走向 graph reasoning 和 graph representation learning。
```

可以分三阶段：

```text
Stage 1: Graph Memory
  notes / papers / repos / factors -> nodes and edges

Stage 2: Graph Retrieval / Reasoning
  graph query, path search, relation explanation, graph-to-prompt

Stage 3: Graph Foundation Layer
  node classification, link prediction, cross-graph transfer, zero-shot graph learning
```

OpenGraph 属于 Stage 3。

但它对 Stage 1 也有启发：

```text
一开始设计 graph schema 时，就要考虑以后能不能学习、泛化和评估。
```

不要只做漂亮的知识图谱可视化。要让每条边、每个节点都能服务后续任务：

```text
classification
recommendation
link prediction
failure diagnosis
next experiment planning
```

这就是 OpenGraph 对我们的长期价值。

## 下一步

HKUDS 图智能主线现在已经有：

```text
HKUDS024 GraphAgent
HKUDS025 OpenGraph
```

下一步继续看：

```text
HKUDS026 GraphGPT
HKUDS027 HiGPT
```

这四个项目合起来，会形成完整的 Graph / Knowledge Graph 学习组：

```text
GraphAgent -> agentic graph-language execution
OpenGraph  -> graph foundation model / zero-shot graph learning
GraphGPT   -> graph instruction tuning / graph-language alignment
HiGPT      -> heterogeneous graph intelligence
```

这一组对我们后面做 QuantMind、factor graph、Research OS memory layer 都很关键。
