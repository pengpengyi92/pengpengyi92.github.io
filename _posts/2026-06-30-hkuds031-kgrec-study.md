---
title: "HKUDS031: KGRec 作为 Knowledge Graph Self-Supervised Rationalization 与 KG-Grounded Recommendation Layer"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds031, hkuds, kgrec, knowledge-graph, recommendation, self-supervised-learning, rationalization, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS031`。

```text
HKUDS031 -> KGRec
```

这篇继续 Recommendation / Finance-adjacent 主线。

前面三篇是：

```text
HKUDS028 RecLM  -> semantic profile + profile-augmented ranking
HKUDS029 XRec   -> collaborative-signal-grounded explanation
HKUDS030 AutoCF -> automated self-supervised CF backbone
```

现在做 `KGRec`。

它补上的能力是：

```text
knowledge graph grounded recommendation
```

更准确地说：

```text
KGRec = knowledge graph
      + user-item interaction graph
      + attentive heterogeneous graph encoder
      + KG self-supervised rationalization
      + masked edge reconstruction
      + UI-KG contrastive learning
```

AutoCF 解决的是：

```text
在 sparse user-item graph 上自动做 SSL augmentation。
```

KGRec 进一步问：

```text
如果 recommendation 不只有 user-item interaction，
还有 item/entity/relation knowledge graph，
我们如何知道哪些 KG edges 是推荐决策里的关键 rationale？
```

这对 Quant Research OS 很关键。

我们的研究对象不会只有：

```text
researcher -> factor
PM -> strategy
paper -> repo
```

还会有大量知识图谱关系：

```text
factor -> asset
asset -> industry
company -> event
event -> macro regime
strategy -> risk factor
paper -> method
repo -> capability
dataset -> signal type
```

KGRec 的启发是：

```text
推荐系统不能只看 interaction graph。
它还要知道 recommendation 背后的 knowledge path 和 rationale edge。
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `KGRec`。

| Item | Value |
|---|---|
| repo | `KGRec` |
| remote | `https://github.com/HKUDS/KGRec.git` |
| branch | `main` |
| local head | `07431ac` |
| full commit | `07431ac6021705930e7b208240b1e41793066c6f` |
| latest local commit date | `2024-06-10 14:41:11 +0800` |
| latest local commit | `Add files via upload` |
| paper | `Knowledge Graph Self-Supervised Rationalization for Recommendation` |
| venue | KDD 2023 |
| authors | Yuhao Yang, Chao Huang, Lianghao Xia, Chunzhen Huang |
| tracked files by `rg --files` | 43 |
| Python files | 18 |
| Markdown files | 1 |
| JSON files | 0 |
| dataset files | 21 files, about 76 MB |

项目结构：

```text
data/
  alibaba-fashion/
  last-fm/
  mind-f/

modules/
  KGRec.py
  AttnHGCN.py
  contrast.py
  KGCL/KGCL.py
  KGIN/KGIN.py

utils/
  data_loader.py
  evaluate_kgsr.py
  parser.py
  sampler.py
  metrics.py
  helper.py
  ext/sampling.cpp

run_kgrec.py
run_kgcl.py
run_kgin.py
requirements.txt
README.md
```

README 给出的主模型运行方式是：

```bash
python run_kgrec.py --dataset [dataset_name]
```

并且 repo 里还实现了两个 baseline：

```text
KGCL
KGIN
```

这让 KGRec 不是单模型孤岛，而是在同一个代码库里和 KG recommendation baseline 对齐。

## 一句话定位

KGRec 是一个：

```text
knowledge graph self-supervised rationalization recommender
```

它的核心不是简单把 KG embedding 拼到 recommender 里。

它真正重要的是：

```text
用 attention score 找到 KG 中更像 recommendation rationale 的边，
再用这些 rationale edge 指导 masked edge reconstruction 和 contrastive learning。
```

可以简化成：

```text
KG edge attention
-> rationale score
-> adaptive KG masking
-> masked edge reconstruction
-> adaptive UI/KG contrastive views
-> better recommendation embedding
```

所以 KGRec 解决的是一个非常关键的问题：

```text
KG 里不是所有边都同等重要。
推荐系统需要识别哪些 knowledge edges 真正支撑 recommendation。
```

这和 Quant OS 完全一致。

金融知识图谱里边会非常多：

```text
company has event
company belongs to industry
asset exposed to macro factor
factor correlated with signal
strategy fails in regime
paper proposes method
repo implements method
```

但不是每条边都能解释一个 factor 或 strategy 为什么值得研究。

我们需要的是：

```text
rationale-aware knowledge graph recommendation
```

KGRec 就是这类思想的 recommender 版本。

## 数据层

KGRec 的 `data/` 里有三个数据集：

```text
alibaba-fashion
last-fm
mind-f
```

每个数据集都有相同格式：

```text
train.txt
test.txt
kg_final.txt
user_list.txt
item_list.txt
entity_list.txt
relation_list.txt
```

`train.txt` 和 `test.txt` 是 user-item interaction。

每一行类似：

```text
user_id item_id item_id item_id ...
```

`kg_final.txt` 是 knowledge graph triples：

```text
head relation tail
```

`utils/data_loader.py` 做了几件事。

第一，读取 CF 数据：

```text
read_cf(train.txt)
read_cf(test.txt)
```

第二，读取 KG triples：

```text
read_triplets(kg_final.txt)
```

如果 `inverse_r=True`，它会加入 inverse relation：

```text
h, r, t
t, r_inverse, h
```

同时把 KG relation id 整体加 1，把 relation 0 留给 user-item interaction。

这很重要，因为后面：

```text
relation 0 = interaction
relation >= 1 = KG relation
```

第三，构造两个图对象：

```text
1. networkx MultiDiGraph for KG triples
2. sparse matrix for user-item interaction
```

也就是说，KGRec 明确区分：

```text
UI graph: user-item interaction
KG graph: entity-relation-entity knowledge graph
```

这对 Quant OS 也应该保留。

我们不能把所有关系都粗暴混成一种边。

应该至少区分：

```text
interaction edge:
  PM accepted factor
  researcher tested strategy
  project used repo

knowledge edge:
  factor belongs to style
  asset belongs to sector
  event affects company
  paper proposes method
```

## Runner: run_kgrec.py

`run_kgrec.py` 是训练入口。

主流程是：

```text
set random seed
parse args
load data
build KGRec model
negative sampling
train batch by batch
periodically evaluate
early stopping by recall@20
optional save checkpoint
```

它会优先尝试加载 C++ negative sampler：

```text
utils/ext/sampling.cpp
```

如果失败，就 fallback 到 Python 的 `UniformSampler`。

每个 batch 输入：

```text
users
pos_items
neg_items
batch_start
```

评价指标来自 `evaluate_kgsr.py`：

```text
precision
recall
ndcg
hit_ratio
auc
```

默认 `Ks` 是：

```text
[20]
```

所以它核心仍然是 top-K recommendation。

## AttnHGCN: Heterogeneous KG Encoder

KGRec 的图编码器是：

```text
modules/AttnHGCN.py
```

类名是：

```text
AttnHGCN
```

它维护 relation embedding：

```text
relation_emb: [n_relations - 1, channel]
```

为什么是 `n_relations - 1`？

因为 relation 0 是 interaction，不进入 KG relation embedding。

KG 聚合时，对每条 KG edge：

```text
head --relation--> tail
```

会计算：

```text
query = entity_emb[head] @ W_Q
key   = entity_emb[tail] @ W_Q
key   = key * relation_emb[edge_type - 1]
attention = query * key
```

然后按 head node 做 softmax，再聚合 value。

这就是 relation-aware attention。

它的作用是：

```text
不同 relation 对 item/entity representation 的贡献不同。
```

同时它还聚合 user-item interaction：

```text
item/entity embedding -> user aggregation through interaction edges
```

所以 AttnHGCN 同时处理：

```text
KG aggregation
UI aggregation
```

这比纯 CF backbone 更强，因为 item 不再只是 interaction graph 里的点。

item 还被 KG 里的 entity/relation neighborhood 解释。

## Rationale Score: KG Edge Attention

KGRec 最核心的组件之一是：

```text
norm_attn_computer()
```

它会根据当前 entity embedding 和 KG edge type，计算每条 KG edge 的 attention score。

输出：

```text
edge_attn_score
edge_attn_logits
```

这个分数可以理解成：

```text
这条 KG edge 对当前 representation / recommendation 是否重要。
```

这就是 KGRec 里的 rationalization。

不是所有 KG edges 都是 rationale。

模型要从 KG 中自动识别：

```text
哪些 relation edge 更可能解释 recommendation。
```

对 Quant OS 来说，这非常关键。

如果系统推荐一个 factor，它不能只说：

```text
因为它和很多东西相关。
```

它应该能指出更具体的 rationale edge：

```text
factor -> exposed_to -> macro regime
strategy -> failed_in -> high volatility period
paper -> proposes -> contrastive graph learning
repo -> implements -> RAG memory layer
asset -> belongs_to -> export-sensitive industry
```

KGRec 给我们的启发是：

```text
recommendation explanation 的证据边，可以由模型 attention/rationale score 自动筛选。
```

## KGRec Forward: 三个训练任务

`modules/KGRec.py` 的 `forward()` 是核心。

它做三件事：

```text
1. recommendation task
2. masked KG edge reconstruction task
3. UI-KG contrastive learning task
```

### 1. Recommendation Task

首先根据 KG edge attention 做 graph sparsification：

```text
_relation_aware_edge_sampling()
```

然后计算 KG edge attention：

```text
edge_attn_score, edge_attn_logits = gcn.norm_attn_computer(...)
```

再对 user-item interaction 做 sparse dropout：

```text
_sparse_dropout(inter_edge, inter_edge_w, node_dropout_rate)
```

然后调用 AttnHGCN：

```text
entity_gcn_emb, user_gcn_emb = gcn(
  user_emb,
  item_emb,
  enc_edge_index,
  enc_edge_type,
  inter_edge,
  inter_edge_w
)
```

得到 user embedding 和 entity/item embedding 后，用标准 BPR：

```text
pos_scores = user * pos_item
neg_scores = user * neg_item
loss = -log sigmoid(pos_scores - neg_scores)
```

这部分是 recommendation ranking backbone。

### 2. Masked KG Edge Reconstruction

KGRec 会选择要 mask 的 KG edges。

选择方式不是纯随机。

它会先对 `edge_attn_score` 加 Gumbel noise：

```text
edge_attn_score = edge_attn_score + noise
```

再取 top-k：

```text
topk_attn_edge_id = topk(edge_attn_score, mae_msize)
```

然后 `_mae_edge_mask_adapt_mixed()` 做混合 mask：

```text
top-attention edges
+ random edges
```

输出：

```text
enc_edge_index
enc_edge_type
masked_edge_index
masked_edge_type
```

也就是：

```text
encoder graph sees remaining KG edges
MAE task reconstructs masked KG edges
```

重构 loss 是 dot-product decoder：

```text
head_emb
tail_emb * relation_emb
sigmoid dot product
```

这相当于让模型学习：

```text
哪些 KG relation structure 是推荐表示里应该被保留和恢复的。
```

### 3. UI-KG Contrastive Learning

第三个任务是 contrastive learning。

KGRec 构造两个 view：

```text
KG view
UI view
```

KG view 来自：

```text
_adaptive_kg_drop_cl()
```

它根据 `edge_attn_score` 自适应保留 KG edges。

UI view 来自：

```text
_adaptive_ui_drop_cl()
```

它根据 item 的 KG attention mean，来决定 user-item interaction edge 的采样概率。

这个设计非常妙。

它说明：

```text
KG rationale 不只影响 KG graph 本身。
它还会反过来影响 UI interaction graph 的 contrastive sampling。
```

然后：

```text
item_agg_ui = gcn.forward_ui(...)
item_agg_kg = gcn.forward_kg(...)
cl_loss = Contrast(item_agg_ui, item_agg_kg)
```

也就是说，模型要让：

```text
item from UI view
item from KG view
```

在 representation space 中对齐。

这就是 KGRec 的自监督核心：

```text
align collaborative interaction signal and knowledge graph signal.
```

## 总 Loss

KGRec 的总 loss 是：

```text
loss = BPR recommendation loss
     + MAE KG reconstruction loss
     + contrastive UI-KG alignment loss
```

代码里返回的 loss_dict 包括：

```text
rec_loss
mae_loss
cl_loss
```

这三个 loss 对应三个目标：

| Loss | Meaning |
|---|---|
| `rec_loss` | 用户是否更偏好 positive item 而不是 negative item |
| `mae_loss` | masked KG relation edge 是否能被重构 |
| `cl_loss` | UI view 和 KG view 的 item representation 是否对齐 |

这比 AutoCF 更进一步。

AutoCF 主要围绕 user-item graph 做 augmentation。

KGRec 则把：

```text
interaction graph
knowledge graph
rationale score
masked reconstruction
contrastive alignment
```

全部连起来。

## 和 AutoCF / RecLM / XRec 的关系

把 HKUDS028-031 放在一起：

| Project | Main Layer | Core Question |
|---|---|---|
| AutoCF | CF backbone | 如何在稀疏 interaction graph 上自动学习 representation？ |
| KGRec | KG-grounded backbone | 如何让 KG rationale 支撑 recommendation representation？ |
| RecLM | Semantic profile layer | 如何用 LLM 生成 profile 并增强 ranking？ |
| XRec | Explanation layer | 如何生成被 CF signal 约束的 recommendation explanation？ |

更完整的系统图是：

```text
AutoCF:
  user-item interaction representation

KGRec:
  knowledge graph rationalization and UI-KG alignment

RecLM:
  semantic profile generation and ranking augmentation

XRec:
  grounded explanation generation
```

放进 Quant Research OS：

```text
Research interaction graph
  -> AutoCF-like sparse graph representation

Financial / research knowledge graph
  -> KGRec-like rationale-aware KG representation

Semantic notes / profiles
  -> RecLM-like profile generation

PM-facing explanation
  -> XRec-like grounded explanation
```

这条线现在基本闭环了。

## 对 Quant Research OS 的启发

KGRec 对我们有五个关键启发。

### 1. Recommendation 需要 Knowledge Graph

如果系统推荐一个 factor，只看历史 interaction 是不够的。

我们还需要：

```text
factor belongs to which style
factor depends on which data source
factor works in which market regime
factor fails under which bias
factor relates to which paper / repo / method
```

也就是：

```text
recommendation candidate has knowledge context.
```

KGRec 是把这个 context 接进 recommender backbone 的范式。

### 2. KG 里的 Rationale Edge 要被识别

金融知识图谱会很大。

如果每条边都当证据，系统会变得很吵。

KGRec 的 attention/rationale score 告诉我们：

```text
每个 recommendation 应该有一组被模型认为关键的 knowledge edges。
```

这对 PM review 非常重要。

PM 不只是要看：

```text
推荐了什么。
```

还要看：

```text
为什么推荐。
关键证据边是什么。
这些证据边是否可信。
有没有遗漏风险边。
```

### 3. Interaction Graph 和 Knowledge Graph 要对齐

KGRec 的 CL task 很有启发：

```text
UI view 和 KG view 的 item representation 要对齐。
```

Quant 里对应的是：

```text
research behavior view
knowledge graph view
```

例如：

```text
behavior view:
  我们读了哪些 paper
  做了哪些 repo
  回测了哪些 strategy
  PM 接受了哪些 idea

knowledge view:
  paper 提出什么方法
  repo 实现什么 capability
  strategy 依赖什么 factor
  factor 暴露什么 risk
```

好的系统应该让这两个 view 对齐。

否则就会出现：

```text
行为上推荐了 A，但知识上解释不了 A。
```

### 4. MAE 可以用来训练 Knowledge Reconstruction

KGRec 的 masked edge reconstruction 可以迁移到 Research OS。

例如：

```text
mask paper-method edge
mask factor-regime edge
mask repo-capability edge
mask strategy-risk edge
mask PM-preference edge
```

然后让模型学习恢复这些关系。

这会逼 representation 学到结构知识，而不是只记住文本相似度。

### 5. Rationalization 是 Explanation 的前置层

XRec 负责生成自然语言 explanation。

但 KGRec 告诉我们，自然语言解释之前应该先有：

```text
rationale edge selection
```

也就是：

```text
先选证据边，再生成解释。
```

对 Quant OS 来说，这可以变成：

```text
KGRec-like rationale selector
-> XRec-like explanation generator
-> PM review interface
```

这会比单纯让 LLM 自己写解释更稳。

## 可以怎么改造成 Quant KGRec

原始 KGRec：

```text
users
items
user-item interactions
item/entity KG
KG relation attention
masked KG reconstruction
UI-KG contrastive learning
top-K recommendation
```

Quant KGRec：

```text
researchers / PMs / projects
factors / strategies / papers / repos / datasets
research interactions
financial / research knowledge graph
relation attention
masked research KG reconstruction
behavior-knowledge contrastive learning
next research recommendation
```

一个最小版本可以这样设计。

节点：

```text
factor
strategy
paper
repo
dataset
asset
industry
event
market regime
PM review decision
experiment
```

边：

```text
strategy uses factor
factor exposed to regime
asset belongs to industry
event affects asset
paper proposes method
repo implements method
dataset supports signal
experiment tests strategy
PM accepts idea
PM rejects idea
```

训练任务：

```text
1. recommendation:
   recommend next factor / paper / repo / experiment

2. masked KG reconstruction:
   recover hidden relation edges

3. contrastive alignment:
   align research behavior view with knowledge graph view
```

输出给 PM：

```text
candidate: factor A
score: high
rationale edges:
  factor A -> exposed_to -> low volatility regime
  strategy B -> uses -> factor A
  paper C -> supports -> construction method
  backtest D -> failed_due_to -> transaction cost
next action:
  run cost-sensitive out-of-sample test
```

这就是 KGRec 对我们最直接的启发。

## 工程上值得注意的 PR 点

KGRec 这个 repo 很有学习价值，也有一些可以改进的工程点。

第一，README 里的 dataset name 和代码里的 dataset name 不完全一致。

README baseline 里写了：

```text
alibaba-ifashion
mind
```

但本地数据目录和代码判断是：

```text
alibaba-fashion
mind-f
last-fm
```

`parser.py` 的 help 里还写着：

```text
[last-fm,amazon-book,alibaba]
```

这容易让新用户跑错命令。

第二，README 说 hyperparameters fixed in `KGRec.py`，但更准确的位置是：

```text
modules/KGRec.py
```

可以补一个超参数表：

```text
last-fm:
  mae_coef=0.1
  mae_msize=256
  cl_coef=0.01
  tau=1.0
  cl_drop=0.5

mind-f:
  mae_coef=0.1
  mae_msize=256
  cl_coef=0.001
  tau=0.1
  cl_drop=0.6

alibaba-fashion:
  mae_coef=0.1
  mae_msize=256
  cl_coef=0.001
  tau=0.2
  cl_drop=0.5
```

第三，`args.cuda` 在不同 parser 里类型不一致。

KGRec / KGCL 里：

```text
--cuda type=int default=1
```

KGIN 里：

```text
--cuda type=bool default=True
```

命令行 bool 在 argparse 里经常容易踩坑。

第四，`run_kgrec.py` 在 fix seed 时直接调用：

```text
torch.cuda.manual_seed_all(seed)
```

即使后面可以用 CPU device，这里也最好加：

```text
if torch.cuda.is_available():
```

第五，保存 checkpoint 时：

```text
save_path = args.out_dir + log_fn + '.ckpt'
```

如果 `--save` 开了但没有设置 `--log_fn`，`log_fn` 可能是 `None`。

更稳妥的做法是给默认 run id，并确保：

```text
os.makedirs(args.out_dir, exist_ok=True)
```

第六，README 可以加一个最小复现实验命令。

例如：

```bash
python run_kgrec.py --dataset last-fm --epoch 2 --cuda 0
```

这样用户可以先验证数据加载和 pipeline，而不是一上来跑 1000 epoch。

这些都是真实改善复现体验的小 PR。

## 我们怎么学它

KGRec 值得重点学三件事。

第一，学习它如何区分两类图：

```text
UI interaction graph
KG relation graph
```

第二，学习它如何用 attention score 做 rationale：

```text
edge attention -> rationale edge -> mask / reconstruction / sampling
```

第三，学习它如何对齐 behavior view 和 knowledge view：

```text
UI aggregation
KG aggregation
contrastive alignment
```

这三件事都可以迁移到我们的 Research OS。

尤其是：

```text
rationale edge selection before explanation generation
```

这个思想非常重要。

## 最终总结

KGRec 的核心价值不是“推荐系统加知识图谱”这么简单。

它真正重要的是：

```text
用 KG attention / rationale score 来指导 recommendation self-supervised learning。
```

它把：

```text
user-item interaction
knowledge graph relation
attention rationale
masked edge reconstruction
UI-KG contrastive learning
BPR ranking
```

连成了一条完整链路。

放进 HKUDS Recommendation / Finance-adjacent 主线：

```text
AutoCF -> sparse interaction representation
KGRec  -> KG rationale and UI-KG alignment
RecLM  -> semantic profile and ranking augmentation
XRec   -> collaborative-grounded natural language explanation
```

放进 Pengyi Quant Research OS：

```text
interaction graph
+ financial / research knowledge graph
+ rationale edge selector
+ profile generator
+ recommendation ranker
+ grounded explanation generator
+ PM review
```

这就是 `HKUDS031 KGRec` 的位置。

它把我们的推荐系统主线从：

```text
who interacted with what
```

推进到：

```text
what knowledge relations rationalize the recommendation
```

这是 Quant Research OS 需要的能力。
