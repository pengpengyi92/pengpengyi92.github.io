---
title: "HKUDS050: Remaining Repo Map - 未完成项目总览、分类与后续学习路线"
date: 2026-07-01 01:00:00 +0800
categories: [Learning, HKUDS]
tags: [pengyi-hkuds-studymap, hkuds050, hkuds, remaining-map, study-map, recommendation, graph-learning, spatiotemporal, urban-ai, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS050`。

这一篇不深挖单个 repo。

这一篇做一件事：

```text
把 HKUDS 目前还没有系统写过的 repo 做一个剩余地图。
```

2026-07-02 update:

```text
HKUDS051 已重新分配给 RAG 系列专题总结。
HKUDS052 已重新分配给 Quant / Forecasting / Trading 系列专题总结。
OpenCity / Urban-ST continuation 顺延到后续 Urban-ST 专题节点。
```

我们已经做到了：

```text
HKUDS049 -> UrbanGPT
```

现在需要暂停一下，重新看全局：

```text
还有哪些没做？
哪些值得继续做？
哪些应该单独深挖？
哪些可以合并成一篇？
它们各自对 Pengyi Research OS / Quant OS 有什么意义？
```

## 数据来源

这次 map 使用两份信息交叉确认：

```text
1. 本地 HKUDS repo index:
   E:\2026\B面\香港大学\PENGYI笔记\PENGYI superCODEX PROJECT笔记！\hkuds

2. GitHub API 当前 HKUDS org repo list:
   checked on 2026-07-01
```

当前结论：

```text
HKUDS public repos checked: 89
已经写过深度笔记或阶段总结覆盖: 44
剩余未系统写过: 45
```

这 45 个不是都要逐个写成超长文章。

更合理的方式是：

```text
核心项目单独深挖。
同质论文代码合并成系列综述。
survey / meta repo 作为路线图材料。
```

## 已覆盖主线回顾

我们已经覆盖的核心能力非常多。

### Agent / Product / Harness

```text
nanobot
CLI-Anything
AgentSpace
AutoAgent
OpenHarness
AnyTool
OpenSpace
ClawTeam
ClawWork
FastAgent
Litewrite
OpenPhone
MoChat
VideoAgent
ViMax
CatchMe
MGP
```

这一块已经足够支撑：

```text
Agent Execution Harness
Tool Harness
Memory Harness
Product / Workspace Harness
```

### RAG / Knowledge / Multimodal Memory

```text
LightRAG
MiniRAG
RAG-Anything
VideoRAG
```

这一块已经支撑：

```text
Research OS knowledge memory
multimodal ingestion
long video knowledge
graph-enhanced RAG
```

### Research / AI Scientist

```text
DeepCode
AI-Researcher
DeepInnovator
Auto-Deep-Research
DeepResearch-Eval
DeepTutor
Paper2Slides
LightReasoner
SepLLM
```

这一块已经支撑：

```text
paper-to-code
research report producer
research report evaluator
scientific idea generation
personal tutoring
reasoning efficiency
long-context compression
```

### Quant / Forecasting / Trading

```text
Vibe-Trading
AI-Trader
FutureShow
UrbanGPT
```

这一块已经开始进入：

```text
trading agent
forecasting agent
spatio-temporal forecasting
Quant OS market prediction layer
```

### Graph / Recommendation 已覆盖核心入口

```text
GraphAgent
OpenGraph
GraphGPT
HiGPT
RecLM
XRec
AutoCF
KGRec
```

这一块已经给我们：

```text
graph-language alignment
graph foundation model
heterogeneous graph LLM
recommendation instruction tuning
explainable recommendation
collaborative filtering
KG-grounded recommendation
```

但是 graph / recommendation 还有大量基础论文仓库没做。

这就是 `HKUDS050` 要整理的重点。

## 剩余 45 个 Repo 总表

| 类别 | Repo | 一句话作用 | 是否建议单独做 |
|---|---|---|---|
| Urban / Spatio-temporal | `OpenCity` | 交通预测的开放时空 foundation model | 是 |
| Urban / Spatio-temporal | `EasyST` | 简洁时空预测框架 | 是 |
| Urban / Spatio-temporal | `AutoST` | 自动化时空图对比学习 | 是 |
| Urban / Spatio-temporal | `FlashST` | traffic prediction prompt tuning 框架 | 可单独 |
| Urban / Spatio-temporal | `GPT-ST` | 时空图神经网络 generative pre-training | 可单独 |
| Urban / Spatio-temporal | `GraphST` | spatial-temporal graph learning | 可合并 |
| Urban / Spatio-temporal | `CL4ST` | 时空 meta contrastive learning | 可合并 |
| Urban / Spatio-temporal | `STExplainer` | 可解释时空图神经网络 | 可合并 |
| Urban / Spatio-temporal | `Awesome-LLM4Urban-Papers` | LLM4Urban survey / paper list | 路线图材料 |
| Graph / LLM4Graph | `AnyGraph` | graph foundation model in the wild | 是 |
| Graph / LLM4Graph | `GraphEdit` | LLM for graph structure learning / editing | 是 |
| Graph / LLM4Graph | `DiffGraph` | heterogeneous graph diffusion model | 可单独 |
| Graph / LLM4Graph | `Awesome-LLM4Graph-Papers` | LLM4Graph survey / paper list | 路线图材料 |
| Recommendation / LLM Rec | `LLMRec` | LLM + graph augmentation for recommendation | 是 |
| Recommendation / LLM Rec | `RLMRec` | LLM representation learning for recommendation | 是 |
| Recommendation / LLM Rec | `RecGPT` | sequential recommendation foundation model | 是 |
| Recommendation / LLM Rec | `EasyRec` | simple language model for recommendation | 可单独 |
| Recommendation / Multimodal / Diffusion | `DiffKG` | KG diffusion for recommendation | 可单独 |
| Recommendation / Multimodal / Diffusion | `DiffMM` | multi-modal diffusion recommendation | 可单独 |
| Recommendation / Multimodal / Diffusion | `PromptMM` | multimodal prompt tuning / distillation | 可合并 |
| Recommendation / Multimodal / Diffusion | `RecDiff` | social recommendation diffusion model | 可合并 |
| Recommendation / Multimodal / Diffusion | `MMSSL` | multimodal self-supervised recommendation | 可合并 |
| Recommendation / SSL / CF | `SSLRec` | self-supervised recommendation framework | 是 |
| Recommendation / SSL / CF | `LightGCL` | graph contrastive learning recommendation | 是 |
| Recommendation / SSL / CF | `AdaGCL` | adaptive graph contrastive learning | 可合并 |
| Recommendation / SSL / CF | `DCCF` | disentangled contrastive collaborative filtering | 可合并 |
| Recommendation / SSL / CF | `DCRec` | debiased contrastive sequential recommendation | 可合并 |
| Recommendation / SSL / CF | `DGNN` | disentangled graph social recommendation | 可合并 |
| Recommendation / SSL / CF | `DSL` | denoised self-augmented social recommendation | 可合并 |
| Recommendation / SSL / CF | `GFormer` | graph transformer for recommendation | 可合并 |
| Recommendation / SSL / CF | `GraphAug` | graph augmentation for recommendation | 可合并 |
| Recommendation / SSL / CF | `GraphPro` | graph pre-training + prompt learning for recommendation | 可单独 |
| Recommendation / SSL / CF | `GTE` | GNN expressivity for recommendation | 可单独 |
| Recommendation / SSL / CF | `HGCL` | heterogeneous graph contrastive learning | 可合并 |
| Recommendation / SSL / CF | `LightGNN` | simple GNN for recommendation | 可合并 |
| Recommendation / SSL / CF | `MAERec` | graph masked autoencoder for sequential recommendation | 可合并 |
| Recommendation / SSL / CF | `MixRec` | heterogeneous graph collaborative filtering | 可合并 |
| Recommendation / SSL / CF | `RCL` | multi-relational contrastive learning | 可合并 |
| Recommendation / SSL / CF | `SelfGNN` | self-supervised GNN for sequential recommendation | 可合并 |
| Recommendation / SSL / CF | `SimRec` | graph-less collaborative filtering | 可合并 |
| Recommendation / Unlearning | `UnlearnRec` | recommendation unlearning / forget learning | 可单独观察 |
| Survey / Meta | `Awesome-SSLRec-Papers` | SSLRec survey / paper list | 路线图材料 |
| Survey / Meta | `AI-Researcher-Gallery` | AI-Researcher gallery / showcase | 补充材料 |
| Survey / Meta | `.github` | HKUDS org profile | 不必深挖 |
| Survey / Meta | `HKUDS` | HKUDS org homepage repo | 不必深挖 |

说明：`AutoCF` 已经在 `HKUDS030` 做过，后续只作为 SSL / CF 系列的已做 anchor，不计入剩余 45 个 repo。

## 四大剩余主线

剩余 repo 可以压缩成四大主线。

```text
1. Urban / Spatio-temporal Intelligence
2. Recommendation Foundation Stack
3. Graph Foundation / Graph Structure Learning
4. Survey / Meta / Lab Knowledge Map
```

这四条不是平均重要。

对我们当前最重要的是：

```text
Urban / Spatio-temporal -> Quant OS 的时序和预测层
Recommendation -> 用户行为、排序、金融信号抽象
Graph -> QuantMind / Research OS 的结构化知识层
Survey / Meta -> 快速建立领域路线图
```

## 1. Urban / Spatio-temporal Intelligence

这一条最直接接 `HKUDS049 UrbanGPT`。

`UrbanGPT` 已经把时空数据接到了 LLM：

```text
spatio-temporal encoder
  -> ST tokens
  -> LLM
  -> forecasting / classification head
```

剩下几个 repo 会把这条链路补齐。

### OpenCity

```text
OpenCity = open spatio-temporal foundation models for traffic prediction
```

它应该优先做。

原因：

```text
UrbanGPT 更像 ST LLM。
OpenCity 更像 ST foundation model / forecasting backbone。
```

对 Quant OS 的映射：

```text
traffic tensor -> city foundation model
market tensor -> market foundation model
```

如果我们想把市场理解成一个时空系统：

```text
asset x time x feature
sector x time x factor
region x time x macro
```

OpenCity 就非常值得看。

### EasyST

```text
EasyST = simple framework for spatio-temporal prediction
```

它的价值不一定在 SOTA。

价值在：

```text
简单、可复现、适合作为 baseline harness。
```

对我们来说，EasyST 可能比复杂模型更有工程意义：

```text
Quant Research OS 需要先有一个可跑通的 time-series baseline harness。
```

### AutoST

```text
AutoST = automated spatio-temporal graph contrastive learning
```

它连接两个关键词：

```text
spatio-temporal
automated contrastive learning
```

对我们有两个启发：

```text
1. 自动选择 / 学习时空增强方式
2. 用 self-supervised signal 增强预测模型
```

这对金融时间序列很关键。

因为金融标签稀疏、噪声大、 regime 变化快。

### FlashST / GPT-ST / GraphST / CL4ST / STExplainer

这一组可以组成一篇综合：

```text
Spatio-temporal Model Zoo
```

它们分别关注：

```text
FlashST:
  prompt tuning for traffic prediction

GPT-ST:
  generative pre-training for spatio-temporal GNN

GraphST:
  adversarial contrastive adaptation

CL4ST:
  meta contrastive learning

STExplainer:
  explainable spatio-temporal GNN
```

对 Quant OS 的作用：

```text
forecasting backbone
domain adaptation
self-supervised representation
explainability
```

这一组值得做，但不必每个都超长。

## 2. Recommendation Foundation Stack

推荐系统剩余 repo 最多。

这说明 HKUDS 的很大一部分研究积累来自：

```text
graph recommendation
self-supervised recommendation
LLM recommendation
multimodal recommendation
diffusion recommendation
```

为什么它对我们有用？

因为推荐系统的本质是：

```text
从稀疏行为中学习偏好、排序和决策。
```

这和金融有相通处：

```text
user-item interaction
  <-> investor-asset interaction

ranking items
  <-> ranking assets

behavior sequence
  <-> market event sequence

explainable recommendation
  <-> explainable alpha / trade rationale
```

### LLM Recommendation 子线

优先看：

```text
LLMRec
RLMRec
RecGPT
EasyRec
```

它们对应：

```text
LLMRec:
  LLM + graph augmentation for recommendation

RLMRec:
  representation learning with LLMs for recommendation

RecGPT:
  sequential recommendation foundation model

EasyRec:
  simple language model for recommendation
```

对我们来说，这一组很重要。

因为它告诉我们：

```text
如何把 user / item / sequence / graph 转成 LLM 可理解的形式。
```

映射到 Quant OS：

```text
asset / event / sector / signal / portfolio
  -> language or token representation
  -> ranking / explanation / decision
```

### SSL / Contrastive / CF 子线

优先 anchor：

```text
SSLRec
LightGCL
GTE
GraphPro
```

然后合并看：

```text
AdaGCL
DCCF
DCRec
DGNN
DSL
GFormer
GraphAug
HGCL
LightGNN
MAERec
MixRec
RCL
SelfGNN
SimRec
```

这一组很多，但都围绕一个核心问题：

```text
在稀疏、噪声、高维 interaction 里，如何学到更稳的 representation？
```

这对金融非常直接。

金融里的 alpha research 也经常面对：

```text
标签弱
噪声大
信号稀疏
非平稳
样本外衰减
```

所以推荐系统里的 SSL / contrastive learning 可以转化成：

```text
factor representation learning
asset relation learning
event sequence embedding
portfolio preference modeling
```

### 关于 DSL 和 GTE

之前问过：

```text
DDSL 做了吗？GTE 呢？
```

这里更准确地说：

```text
本地 repo 叫 DSL，不是 DDSL。
DSL 还没有做。
GTE 也还没有做。
```

它们的位置是：

```text
DSL:
  denoised self-augmented learning for social recommendation

GTE:
  graph neural network expressivity for recommendation
```

这两个都属于推荐基础线。

如果后面要补，建议：

```text
HKUDS059 -> GTE
HKUDS060 -> DSL + DCCF + DCRec + DGNN as social / contrastive recommendation cluster
```

### Multimodal / Diffusion Recommendation 子线

可以合并成一组：

```text
DiffKG
DiffMM
PromptMM
RecDiff
MMSSL
```

核心问题：

```text
如何用 diffusion / multimodal / prompt tuning 增强推荐？
```

对我们来说，重点不是复现推荐算法本身。

重点是学：

```text
多模态信号融合
KG signal 融合
噪声建模
解释性和生成式 rationale
```

映射到 Quant OS：

```text
news
filings
price
macro
sector graph
portfolio behavior
```

这些都是多源信号。

## 3. Graph Foundation / Graph Structure Learning

我们已经做过：

```text
GraphAgent
OpenGraph
GraphGPT
HiGPT
```

剩下的图主线还有：

```text
AnyGraph
GraphEdit
DiffGraph
Awesome-LLM4Graph-Papers
```

### AnyGraph

```text
AnyGraph = graph foundation model in the wild
```

这值得单独看。

因为它直接连接：

```text
graph foundation model
out-of-distribution graph generalization
real-world graph tasks
```

对 Research OS 的映射：

```text
paper graph
project graph
concept graph
author graph
tool graph
```

对 Quant OS 的映射：

```text
asset graph
sector graph
supply chain graph
news-event graph
fund-holding graph
```

### GraphEdit

```text
GraphEdit = LLM for graph structure learning / graph editing
```

这个很有意思。

因为现实中的 graph 往往不是天然干净的。

Research OS 里的知识图谱会有：

```text
missing edge
wrong edge
redundant node
conflicting relation
stale relation
```

GraphEdit 的启发是：

```text
LLM 不只是读 graph。
LLM 也可以参与修 graph。
```

这对 QuantMind / Research OS memory 很关键。

### DiffGraph

```text
DiffGraph = heterogeneous graph diffusion model
```

它可以和推荐 diffusion 系列合并理解。

重点是：

```text
graph generation
graph denoising
heterogeneous graph representation
```

## 4. Survey / Meta / Lab Knowledge Map

剩下还有几个不是传统代码仓库：

```text
Awesome-SSLRec-Papers
Awesome-LLM4Graph-Papers
Awesome-LLM4Urban-Papers
AI-Researcher-Gallery
.github
HKUDS
```

这些不需要像 `OpenHarness` 那样深挖源码。

它们更适合用作：

```text
领域路线图
论文入口
项目解释层
HKUDS lab knowledge map
```

其中三个 Awesome repo 很重要：

```text
Awesome-SSLRec-Papers
Awesome-LLM4Graph-Papers
Awesome-LLM4Urban-Papers
```

它们分别对应：

```text
self-supervised recommendation
LLM for graphs
LLM for urban computing
```

如果我们要做顶会路线图，这三个 survey repo 很适合转化成：

```text
research field map
paper reading queue
benchmark list
open problem list
```

## 优先级排序

我建议把剩余项目分成三档。

### P0: 直接服务 Research OS / Quant OS 的核心项目

```text
OpenCity
EasyST
AutoST
FlashST
GPT-ST
AnyGraph
GraphEdit
LLMRec
RLMRec
RecGPT
SSLRec
LightGCL
GTE
```

原因：

```text
它们能直接补齐时序预测、graph foundation、LLM recommendation、SSL representation learning。
```

这些对我们后续做：

```text
Quant Research OS
factor representation
market graph
research memory graph
decision ranking
```

都有直接价值。

### P1: 应该做，但可以合并成 cluster 的项目

```text
GraphST
CL4ST
STExplainer
DiffGraph
DiffKG
DiffMM
PromptMM
RecDiff
MMSSL
GraphPro
GFormer
GraphAug
HGCL
LightGNN
MAERec
SelfGNN
```

这些适合写成：

```text
cluster study
```

不是每个都写 1500 行。

### P2: 补全生态和论文背景的项目

```text
AdaGCL
DCCF
DCRec
DGNN
DSL
MixRec
RCL
SimRec
UnlearnRec
EasyRec
Awesome-SSLRec-Papers
Awesome-LLM4Graph-Papers
Awesome-LLM4Urban-Papers
AI-Researcher-Gallery
.github
HKUDS
```

这些可以作为：

```text
appendix
field map
paper queue
method comparison table
```

## 后续编号路线建议

`HKUDS051` 已插入为 RAG 系列专题总结。
`HKUDS052` 已插入为 Quant / Forecasting / Trading 系列专题总结。

所以原来的 Urban / ST 后续路线顺延两位。

我建议从 `HKUDS053` 开始这样排：

| 编号 | Repo / Topic | 系列 | 为什么看 |
|---|---|---|---|
| `HKUDS053` | `OpenCity` | Urban / ST Foundation | 接 UrbanGPT，理解时空 foundation model |
| `HKUDS054` | `EasyST` | Urban / ST Baseline | 建立可复现时空预测 baseline |
| `HKUDS055` | `AutoST` | Urban / ST SSL | 学自动化时空对比学习 |
| `HKUDS056` | `FlashST + GPT-ST` | Urban / ST Pretraining | 看 prompt tuning 与 generative pretraining |
| `HKUDS057` | `GraphST + CL4ST + STExplainer` | Urban / ST Model Zoo | 做时空图模型综合 |
| `HKUDS058` | `AnyGraph` | Graph Foundation | 补 graph foundation model |
| `HKUDS059` | `GraphEdit` | Graph Structure Learning | 学 LLM 如何修改和治理 graph |
| `HKUDS060` | `LLMRec + RLMRec` | LLM Recommendation | LLM 表征与推荐系统结合 |
| `HKUDS061` | `RecGPT + EasyRec` | Recommendation FM | sequential recommendation foundation model |
| `HKUDS062` | `SSLRec` | SSL Recommendation | self-supervised recommendation 总框架 |
| `HKUDS063` | `LightGCL + AdaGCL` | Graph Contrastive Rec | GCL 推荐系统核心方法 |
| `HKUDS064` | `GTE + GraphPro + GFormer` | Graph Rec Theory / Pretraining | GNN 表达力、预训练、transformer |
| `HKUDS065` | `DiffKG + DiffMM + RecDiff` | Diffusion Rec | diffusion 和推荐融合 |
| `HKUDS066` | `DSL + DCCF + DCRec + DGNN` | Social / Debiased Rec | social rec / debias / disentangle |
| `HKUDS067` | `Awesome-SSLRec / LLM4Graph / LLM4Urban` | Survey Map | 三条顶会阅读路线图 |
| `HKUDS068` | `Remaining Rec Cluster Summary` | Final Map | recommendation 剩余项目总复盘 |

这只是建议路线。

如果我们更想接 Quant OS，我建议优先：

```text
HKUDS052 Quant Series Summary
HKUDS053 OpenCity
HKUDS054 EasyST
HKUDS055 AutoST
HKUDS058 AnyGraph
HKUDS060 LLMRec + RLMRec
HKUDS062 SSLRec
```

因为这几篇对市场预测、资产图、信号表征和排序决策最有用。

## 对 Pengyi Research OS 的映射

剩余项目不是杂乱无章的。

它们可以进入我们自己的系统架构。

```text
Pengyi Research OS
  Knowledge Memory:
    Awesome-LLM4Graph / Awesome-LLM4Urban / Awesome-SSLRec

  Graph Memory:
    AnyGraph / GraphEdit / DiffGraph

  Research Reading Queue:
    Survey repos + paper code repos

  Representation Learning:
    SSLRec / LightGCL / GTE / GraphPro

  Decision Ranking:
    LLMRec / RLMRec / RecGPT / EasyRec

  Spatio-temporal Forecasting:
    OpenCity / EasyST / AutoST / FlashST / GPT-ST
```

## 对 Pengyi Quant OS 的映射

如果映射到 Quant OS：

```text
Market Tensor Layer:
  OpenCity / EasyST / AutoST / FlashST / GPT-ST

Asset Graph Layer:
  AnyGraph / GraphEdit / DiffGraph / GTE / GraphPro

Factor Representation Layer:
  SSLRec / LightGCL / AdaGCL / DCCF / MAERec

Ranking / Portfolio Preference Layer:
  LLMRec / RLMRec / RecGPT / EasyRec / XRec

Multi-source Signal Fusion:
  DiffKG / DiffMM / PromptMM / MMSSL / RecDiff

Explainability:
  STExplainer / XRec / KGRec / GraphEdit
```

这就是为什么推荐系统和时空预测不是偏题。

它们都在回答一个更底层的问题：

```text
如何从复杂关系、稀疏行为、时序信号和多源噪声中学到可用于决策的 representation？
```

这正是量化研究需要的。

## 当前最清晰的下一步

我建议下一篇不要继续随机挑。

直接做：

```text
HKUDS053 -> OpenCity
```

理由：

```text
1. 它自然接 HKUDS049 UrbanGPT。
2. 它是时空 foundation model，更靠近 Quant OS 的 market foundation model 想象。
3. 它能把 Urban / Spatio-temporal 系列正式展开。
```

然后：

```text
HKUDS054 -> EasyST
HKUDS055 -> AutoST
```

这三篇可以形成一个小闭环：

```text
UrbanGPT:
  ST data -> LLM

OpenCity:
  ST foundation model

EasyST:
  simple reproducible baseline

AutoST:
  automated ST contrastive learning
```

这条线最适合接到我们的 Quant OS。

## 一句话总结

```text
HKUDS 剩余项目主要不是 agent 产品，而是推荐系统、图学习、时空预测和 survey 资产。
```

我们前 49 篇已经把：

```text
agent / RAG / research / product / memory / graph core / rec core / UrbanGPT
```

搭起来了。

接下来的 HKUDS 学习应该从“看酷项目”进入“补系统底层能力”：

```text
spatio-temporal forecasting
graph foundation
self-supervised recommendation
LLM recommendation
survey-driven paper map
```

这就是 `HKUDS050` 的作用：

```text
给剩余 45 个 repo 一个分类地图，
并把后续学习路线从随机探索改成系统推进。
```
