---
title: "HKUDS030: AutoCF 作为 Automated Self-Supervised Collaborative Filtering 与 Recommendation Backbone Layer"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds030, hkuds, autocf, recommendation, collaborative-filtering, self-supervised-learning, graph-augmentation, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS030`。

```text
HKUDS030 -> AutoCF
```

前两篇我们已经进入 Recommendation / Finance-adjacent 主线：

```text
HKUDS028 RecLM -> profile generation + profile-augmented ranking
HKUDS029 XRec  -> collaborative-signal-grounded recommendation explanation
```

现在做 `AutoCF`。

它的位置非常明确：

```text
AutoCF -> recommendation representation backbone
```

也就是说，`AutoCF` 不是一个 LLM profile generator，也不是一个 explanation generator。

它解决的是更底层的问题：

```text
在稀疏、有噪声、长尾的 user-item interaction graph 上，
如何自动构造 self-supervised augmentation，
学习更鲁棒的 collaborative filtering representation。
```

这对我们做 Quant Research OS 很重要。

因为量化研究里的数据也经常是：

```text
sparse
noisy
long-tail
highly non-stationary
limited labeled signal
```

尤其是 factor / strategy / paper / repo / PM preference 这种 research object，它们之间的交互记录不会像互联网推荐系统那样密集。

所以我们需要一个底层 representation learner：

```text
Research interaction graph
-> self-supervised representation learning
-> candidate ranking
-> grounded explanation
-> PM review
```

AutoCF 正好补上 RecLM 和 XRec 下面的这一层。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `AutoCF`。

| Item | Value |
|---|---|
| repo | `AutoCF` |
| remote | `https://github.com/HKUDS/AutoCF.git` |
| branch | `main` |
| local head | `62ac626` |
| full commit | `62ac626b18c5327ce93676762138ae1b7b4a0d0c` |
| latest local commit date | `2024-02-28 20:49:37 +0800` |
| latest local commit | `Delete Models directory` |
| paper | `Automated Self-Supervised Learning for Recommendation`, arXiv `2303.07797` |
| venue | WWW 2023 |
| authors | Lianghao Xia, Chao Huang, Chunzhen Huang, Kangyi Lin, Tao Yu, Ben Kao |
| tracked files by `rg --files` | 19 |
| Python files | 6 |
| Markdown files | 1 |
| JSON files | 0 |

项目结构很小：

```text
Datasets/              sparse Yelp / Gowalla / Amazon pickle matrices
figs/                  intro and framework figures
methods/AutoCF/        core model, training, data handler, params
datapreprocessing.zip  raw-data preprocessing scripts
README.md             paper, usage, dataset description
```

核心代码集中在：

```text
methods/AutoCF/
  DataHandler.py
  Main.py
  Model.py
  Params.py
  Utils/
```

这个 repo 很适合作为 recommender backbone 的最小可读样例。

## 一句话定位

AutoCF 是一个：

```text
automated self-supervised collaborative filtering framework
```

更具体地说：

```text
AutoCF = learnable graph augmentation
       + masked graph autoencoder
       + GCN encoder
       + graph transformer decoder
       + recommendation objective
       + contrastive regularization
```

它想解决的问题是：

```text
过去 graph contrastive recommendation 依赖人工设计 augmentation view。
这些 heuristic view 不一定适合不同 dataset、噪声比例、长尾分布和下游任务。
```

AutoCF 的方案是：

```text
让系统自动选择重要子图和 masking seed，
自动构造 augmentation graph，
再通过 masked graph reconstruction / self-supervised signal 学出更好的 CF representation。
```

这和我们做 Quant OS 的痛点非常像。

我们未来不会只有一种固定的 factor augmentation：

```text
按行业分组？
按市值分组？
按波动率 regime？
按 macro event？
按 PM preference？
按历史 failure mode？
```

这些 view 手工设计很难泛化。

AutoCF 给我们的启发是：

```text
augmentation 本身也应该被学习和自动化。
```

## 它解决什么问题

推荐系统里的 self-supervised learning 很常见。

典型思路是：

```text
user-item graph
-> 构造两个 augmented views
-> 学习节点 representation invariance
-> 用于 downstream recommendation
```

但问题是 augmentation 经常是手工设计的：

```text
edge dropout
node dropout
random walk
feature masking
subgraph sampling
noise perturbation
```

这些策略在一个数据集上有效，不代表在另一个数据集上有效。

尤其当数据非常稀疏、长尾严重、噪声很多时，随便 dropout 可能会把本来就稀缺的有效信号删掉。

AutoCF 的核心判断是：

```text
self-supervised recommendation 的关键不只是 contrastive objective，
更是如何自动找到有用的 augmentation signal。
```

所以它提出：

```text
automated data augmentation for collaborative filtering
```

代码里对应的实现就是：

```text
LocalGraph -> 自动选择重要 seed nodes
RandomMaskSubgraphs -> 基于 seeds mask 子图并构造 encoder / decoder graph
Model -> 用 GCN 和 graph transformer 学 user/item embeddings
Coach -> 训练 recommendation + contrastive + local-global losses
```

## 数据层

README 里列出三个稀疏数据集：

| Dataset | Users | Items | Interactions | Density |
|---|---:|---:|---:|---:|
| Yelp | 42,712 | 26,822 | 182,357 | 1.6e-4 |
| Gowalla | 25,557 | 19,747 | 294,983 | 5.9e-4 |
| Amazon | 76,469 | 83,761 | 966,680 | 1.5e-4 |

本地 `Datasets/` 里已经带了处理后的 pickle：

```text
Datasets/sparse_yelp/
  trnMat.pkl
  valMat.pkl
  tstMat.pkl

Datasets/sparse_gowalla/
  trnMat.pkl
  valMat.pkl
  tstMat.pkl

Datasets/sparse_amazon/
  trnMat.pkl
  valMat.pkl
  tstMat.pkl
```

这些矩阵对应 implicit feedback recommendation：

```text
user x item sparse matrix
```

README 说明训练/验证/测试划分是：

```text
70 : 5 : 25
```

`DataHandler.py` 的职责很直接：

```text
load trnMat.pkl and tstMat.pkl
convert scipy sparse matrix to torch sparse adjacency
build bipartite user-item graph
normalize adjacency
create training loader and test loader
```

它会把 user-item matrix 扩成 bipartite adjacency：

```text
[ 0   R ]
[ R^T 0 ]
```

然后加 self-loop 并做对称归一化。

这就是后面 GCN / graph transformer 使用的图。

## Model: GCN Encoder + Graph Transformer Decoder

核心模型在：

```text
methods/AutoCF/Model.py
```

`Model` 初始化两组 embedding：

```text
uEmbeds: user embeddings
iEmbeds: item embeddings
```

然后有两类 layer：

```text
GCNLayer
GTLayer
```

`GCNLayer` 很轻：

```text
return sparse_adj @ embeds
```

它负责在 encoder graph 上做 collaborative propagation。

`GTLayer` 是 graph transformer layer。

它在 sparse adjacency 的 edge 上计算：

```text
query
key
value
attention
```

然后按 row 聚合 value。

所以整体 forward 是：

```text
ego user/item embeddings
-> GCN layers on encoderAdj
-> optional GT layers on decoderAdj
-> sum all layer embeddings
-> split back into user embeddings and item embeddings
```

这里有一个重要细节：

```text
encoderAdj 和 decoderAdj 可以不同。
```

这就是 masked graph autoencoder 思路的入口。

训练时：

```text
encoderAdj = 被 mask 后的 graph
decoderAdj = 加入 sampled / reconstructed structure 的 graph
```

测试时：

```text
self.model(self.handler.torchBiAdj, self.handler.torchBiAdj)
```

也就是直接用完整 graph。

## LocalGraph: 自动选择 Masking Seeds

AutoCF 的自动化 augmentation 从 `LocalGraph` 开始。

它的输入是：

```text
allOneAdj
ego embeddings
```

`allOneAdj` 是把 normalized adjacency 的 value 全部变成 1 的 sparse graph。

`LocalGraph` 会计算每个节点的 local subgraph representation。

代码里大概做了：

```text
first-order neighbor embeddings
second-order neighbor embeddings
subgraph embedding = first-order + second-order aggregation
```

然后把：

```text
subgraph embedding
ego node embedding
```

做 normalize，再算相似度分数。

之后加入 Gumbel-style noise：

```text
noise = -log(-log(rand))
score = log(score) + noise
```

最后：

```text
topk(scores, seedNum)
```

选出 masking seeds。

这一步很关键。

它不是随机选节点，也不是纯 heuristic。

它根据当前 embedding 和 local subgraph consistency，自动选一些更值得被 mask / reconstruct 的节点。

这就是 AutoCF 里的：

```text
learnable / automated augmentation seed selection
```

对 Quant OS 的映射是：

```text
不要随机 mask factor / strategy / paper / repo。
应该根据当前 research graph 的 local-global signal，自动选择最有信息量的研究对象做 augmentation。
```

## RandomMaskSubgraphs: 构造 Encoder / Decoder Graph

选出 seeds 后，`RandomMaskSubgraphs` 负责构造两个 graph：

```text
encoderAdj
decoderAdj
```

它的核心流程是：

```text
1. 从 seed nodes 出发，按 maskDepth 扩展邻居。
2. 从原始 graph 中移除这些 seed-neighborhood edges。
3. 额外随机采样一批节点，比例由 keepRate 控制。
4. 合并 maskNodes。
5. 用剩余 edges 构造 encoderAdj。
6. 用 maskNodes 随机生成重构边，加上 self-loop 和剩余 edges，构造 decoderAdj。
```

这其实是一个 masked graph autoencoder 风格的 augmentation：

```text
encoder sees incomplete graph
decoder tries to operate on augmented / reconstructed graph
```

代码里第一次运行还会打印：

```text
LENGTH CHANGE
Original SAMPLED NODES
AUGMENTED SAMPLED NODES
```

这能帮助观察 mask 后边数和节点覆盖率的变化。

对我们自己的系统来说，类似操作可以变成：

```text
mask part of factor-regime links
mask part of paper-method links
mask part of repo-capability links
mask part of PM-accepted-idea links
```

再让模型学会从剩余结构中恢复或增强 representation。

## Training: Recommendation + Contrastive + Local-Global

训练主循环在：

```text
methods/AutoCF/Main.py
```

核心对象是 `Coach`。

训练开始时：

```text
self.model = Model().cuda()
self.masker = RandomMaskSubgraphs()
self.sampler = LocalGraph()
```

每个 epoch 里先做 negative sampling：

```text
trnLoader.dataset.negSampling()
```

然后每隔 `fixSteps` 步重新采样一次 augmentation graph：

```text
sampScores, seeds = sampler(allOneAdj, egoEmbeds)
encoderAdj, decoderAdj = masker(torchBiAdj, seeds)
```

这意味着：

```text
一个 sampled graph 会被连续使用 fixSteps 个 batch，
避免每个 batch 都重新构图造成过高开销。
```

每个 batch 会计算：

```text
usrEmbeds, itmEmbeds = model(encoderAdj, decoderAdj)
ancEmbeds = usrEmbeds[ancs]
posEmbeds = itmEmbeds[poss]
```

loss 由几部分组成：

```text
recommendation loss
regularization loss
contrastive regularization
local-global seed reward
```

代码里 recommendation loss 是：

```text
(-sum(ancEmbeds * posEmbeds)).mean()
```

也就是最大化 observed user-item positive pair 的 dot product。

`TrnData` 里虽然准备了 negative item，但当前 `Main.py` 解包后没有使用 negative item。

所以从代码角度看，这里更准确地说是：

```text
positive-pair score maximization
```

而不是标准完整 BPR pairwise loss。

contrastive regularization 通过 `Utils.contrast()` 计算：

```text
contrast(ancs, usrEmbeds)
contrast(poss, itmEmbeds)
contrast(ancs, usrEmbeds, itmEmbeds)
```

local-global loss 是：

```text
localGlobalLoss = -sampScores.mean()
```

它鼓励 LocalGraph 选出的 seed nodes 有更高 local-global consistency score。

最终训练目标大概是：

```text
loss = recommendation_loss
     + reg_loss
     + contrast_loss
     + local_global_loss
```

这就是 AutoCF 的核心训练闭环。

## Evaluation

测试在 `testEpoch()`。

流程是：

```text
use full graph
compute user/item embeddings
score = userEmbeds @ itemEmbeds.T
mask training interactions
topK recommendation
evaluate Recall and NDCG
```

默认：

```text
topk = 20
```

所以它的评估对象不是 explanation quality，也不是 profile quality。

它评估的是 recommender backbone 的 ranking quality：

```text
Recall@20
NDCG@20
```

这也说明 AutoCF 和 RecLM / XRec 的分工：

```text
AutoCF -> 学 representation，提高 ranking
RecLM  -> 用 LLM profile 增强 ranking
XRec   -> 给 recommendation 生成 grounded explanation
```

三者不是替代关系。

它们更像一条完整链路里的不同层。

## 和 RecLM / XRec 的关系

把最近三篇放在一起看：

| Project | Layer | Core Output |
|---|---|---|
| AutoCF | CF representation backbone | user/item embeddings and ranking scores |
| RecLM | profile and ranking augmentation | semantic profiles as recommendation features |
| XRec | explanation layer | CF-grounded natural language explanation |

如果用系统图表示：

```text
AutoCF
  -> learn robust user/item representations from sparse graph

RecLM
  -> generate semantic user/item profiles and enrich ranking

XRec
  -> inject collaborative embeddings into LLM and explain recommendation
```

这三者可以合成：

```text
sparse interaction graph
-> self-supervised CF representation
-> semantic profile generation
-> recommendation ranking
-> grounded explanation
-> human review
```

放到 Quant Research OS：

```text
research interaction graph
-> factor / strategy / paper / repo embeddings
-> profile generation
-> recommendation / prioritization
-> grounded explanation
-> PM review
```

这条线非常完整。

## 对 Quant Research OS 的启发

AutoCF 对我们最重要的启发有五点。

### 1. 先有 Representation Backbone，再谈 Agent

很多 agent 项目容易直接跳到：

```text
LLM decides what to research next
```

但如果底层没有可靠 representation，LLM 的推荐会很飘。

AutoCF 提醒我们：

```text
research recommendation system 需要一个 interaction representation backbone。
```

也就是：

```text
谁研究了什么？
什么 idea 被接受？
什么 factor 被回测？
什么策略在什么 regime 失败？
哪些 repo / paper 被复用？
哪些 PM 偏好哪些风险收益结构？
```

这些行为图要先被建起来，再让 agent 在上面 reasoning。

### 2. 量化研究天然稀疏，不能只依赖 supervised label

真实 quant research 里，高质量 label 很少：

```text
真正 out-of-sample 有效的因子很少
真正能实盘存活的策略很少
真正被 PM 采用的 idea 很少
```

但弱信号很多：

```text
读过的 paper
看过的 repo
写过的 notebook
失败的 backtest
PM 的评论
市场 regime 的变化
```

所以 self-supervised learning 很适合：

```text
从大量 weak interaction 里学 representation。
```

### 3. Augmentation 不能永远手写

我们未来一定会遇到这个问题：

```text
factor graph 应该怎么增强？
strategy graph 应该怎么 mask？
paper-repo graph 应该怎么 dropout？
PM preference graph 应该怎么采样？
```

如果全部手写 heuristic，会很难泛化。

AutoCF 的思路是：

```text
让 augmentation 选择本身变成模型的一部分。
```

这很适合我们的 R&D Agent。

### 4. Long-tail 是核心问题

AutoCF 的 README 明确强调 sparse data 和 long-tail distribution。

量化里也一样。

热门资产、热门策略、热门因子有很多记录。

但真正有 alpha 的东西，常常在长尾：

```text
小众资产
小众事件
细分行业
特殊 regime
被忽视的数据源
非主流 signal construction
```

所以底层 representation learner 必须能处理 long-tail。

这比简单把所有东西塞进 embedding database 更重要。

### 5. Recommendation Backbone 可以服务多个上层任务

AutoCF 学到的 embedding 不只服务 item ranking。

在我们的系统里，类似 backbone 可以服务：

```text
factor recommendation
strategy recommendation
paper recommendation
repo recommendation
dataset recommendation
experiment next-step recommendation
PM review prioritization
```

然后上层再接：

```text
RecLM-style profile generation
XRec-style grounded explanation
QuantMind-style structured knowledge
X2Strategy-style paper-to-strategy execution
```

这就是“融会贯通”的位置。

## 可以怎么改造成 Quant AutoCF

原始 AutoCF：

```text
users
items
user-item interactions
automated graph augmentation
self-supervised CF embeddings
Recall / NDCG recommendation evaluation
```

Quant AutoCF 可以是：

```text
researcher / PM / project / regime
factor / strategy / paper / repo / dataset
research interactions
automated research graph augmentation
self-supervised research embeddings
next-research recommendation evaluation
```

可以先做一个最小版本：

```text
nodes:
  factors
  strategies
  papers
  repos
  datasets
  experiments
  market regimes
  PM review decisions

edges:
  factor tested in experiment
  strategy uses factor
  paper inspires strategy
  repo implements method
  dataset supports factor
  PM accepts / rejects idea
  regime makes strategy work / fail
```

然后训练一个 AutoCF-like backbone：

```text
mask part of research graph
learn embeddings
rank candidate next actions
evaluate against historical accepted / useful actions
```

上层输出可以是：

```text
Top candidate factors
Top candidate papers
Top candidate repos
Top next experiments
Top risk diagnoses
```

再接 XRec-style explanation：

```text
为什么推荐这个 factor？
为什么这个 repo 适合当前 Research OS？
为什么这个 paper 值得复现？
为什么这个策略在当前 regime 下更值得测？
```

## 工程上值得注意的 PR 点

AutoCF 很适合学习，但也有一些现实的 improvement opportunity。

第一，README 的命令使用了：

```bash
python Main.py --data yelp --reg 1e-4 --seed 500
```

但 `Params.py` 里没有 `--seed` 参数，只有：

```text
--seedNum
```

这里大概率是 README 参数写错。

可以做一个很小但有价值的 PR：

```text
把 --seed 改成 --seedNum
或者在 Params.py 里补一个真正的 random seed 参数
```

第二，README 要求手动创建：

```text
History/
Models/
```

但代码不会自动创建目录。

`saveHistory()` 直接写：

```text
../../History/{save_path}.his
../../Models/{save_path}.mod
```

更好的工程做法是：

```text
os.makedirs('../../History', exist_ok=True)
os.makedirs('../../Models', exist_ok=True)
```

第三，代码强依赖 CUDA：

```text
.cuda()
CUDA_VISIBLE_DEVICES
```

没有 CPU fallback，也没有统一 device abstraction。

如果要提高复现体验，可以改成：

```text
device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
```

第四，repo 没有 `requirements.txt`。

README 只列了：

```text
python=3.10.4
torch=1.11.0
numpy=1.22.3
scipy=1.7.3
```

但代码还 import 了：

```text
setproctitle
```

这会让新用户第一次运行时容易缺依赖。

第五，`TrnData` 生成了 negative samples，但当前训练 loss 没有使用 negative item。

代码里：

```text
ancs, poss, _ = tem
```

这可能是有意简化，也可能是实现遗留。

至少 README 可以说明当前 objective 不是标准 pairwise BPR，或者代码可以恢复 `negEmbeds` 参与 loss。

第六，项目的 `.gitignore` 只有：

```text
.DS_Store
```

如果用户运行训练，很容易产生：

```text
__pycache__/
History/
Models/
*.his
*.mod
```

这些应该进入 `.gitignore`。

这些都是很实际的 PR 点。

## 我们怎么学它

学习 AutoCF，不要只看它的 recommender 指标。

我们要重点学三个东西。

第一，学它怎么把 sparse interaction matrix 变成 bipartite graph：

```text
user-item matrix -> normalized graph adjacency -> GCN propagation
```

第二，学它怎么让 augmentation 自动化：

```text
LocalGraph seed selection
RandomMaskSubgraphs masking
encoder / decoder adjacency split
```

第三，学它怎么把 backbone 和上层系统接起来：

```text
embedding backbone -> ranking -> profile / explanation / PM review
```

对我们来说，AutoCF 最值得抽象的是：

```text
Automated Self-Supervised Research Graph Representation Learning
```

这可以成为 Pengyi Quant Research OS 的底层能力之一。

## 最终总结

AutoCF 的核心价值不是“又一个推荐模型”。

它真正重要的是：

```text
把 self-supervised recommendation 里的 graph augmentation 自动化。
```

它从稀疏 user-item graph 出发，通过：

```text
LocalGraph seed selection
RandomMaskSubgraphs
GCN encoder
Graph Transformer decoder
recommendation + contrastive training
Recall / NDCG evaluation
```

形成一个 CF representation backbone。

放进我们这条主线：

```text
AutoCF -> 学 interaction representation
RecLM  -> 生成 semantic profile 并增强 ranking
XRec   -> 生成 collaborative-grounded explanation
```

放进 Quant Research OS：

```text
AutoCF-like backbone -> factor / strategy / paper / repo embeddings
RecLM-like layer     -> profile and ranking augmentation
XRec-like layer      -> grounded explanation for PM review
```

所以 `HKUDS030 AutoCF` 是 Recommendation / Finance-adjacent 主线里非常关键的一块底座。

它让我们看到：

```text
AI scientist system 不只是让 LLM 更会说。
它还要让 research object 的结构表示更扎实。
```
