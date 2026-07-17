---
title: "ICML2026001: Spotlight Paper Map - Agent / Quant / RAG / RL / Research OS 推荐阅读地图"
date: 2026-07-13 00:00:00 +0800
categories: [Learning, AI Research]
tags: [icml2026, icml2026001, spotlight-paper, ai-agent, reinforcement-learning, rag, quant-research, time-series, llm, research-os, paper-map]
---

这是 `PENGYI_ICML2026_STUDYMAP` 的第一篇。

```text
ICML2026001 -> Spotlight Paper Map
```

这一篇先不精读单篇论文，而是做 map：

```text
从 ICML 2026 Spotlight papers 里筛一批和我们最相关的论文，
按 Agent / Quant / RAG / RL / Research OS 的方向建立阅读优先级。
```

我们的目标不是“把 ICML 全部看完”。

我们的目标是：

```text
把顶会 paper 转化成我们自己的 Research OS、Quant OS、Agent OS、CV story 和项目灵感。
```

## Why ICML 2026

ICML 是机器学习三大顶会之一。

从研究品味上看，ICML 通常更重视：

```text
learning theory
optimization
reinforcement learning
statistical machine learning
representation learning
efficient training / inference
robust evaluation
```

而 2026 年的主题里，和我们最强相关的是：

```text
LLM Agents
Agentic RL
Multi-agent systems
Time-series modeling
Retrieval / embedding / contrastive learning
LLM post-training / knowledge update
Efficient LLM / quantization
Mechanistic interpretability
```

这和我们现在做的几条线正好接上：

```text
Pengyi AI / Quant Research OS
Rates Bond Quant
HKUDS OpenOPC / AgentSpace / OpenHarness
LightRAG / RAG-Anything
MLRL map
CV / interview delivery
```

## Reading Priority Map

先给结论。

如果只读三篇：

```text
1. MASPOB
2. T²PO
3. HELIX
```

如果读六篇：

```text
4. Updating Parametric Knowledge with Context Distillation
5. HOBIT
6. WaterSIC
```

如果继续扩展：

```text
7. Language Model Circuits Are Sparse in the Neuron Basis
8. Latent Spherical Flow Policy for RL with Combinatorial Actions
9. Towards Efficient LLMs Annealing with Principled Sample Selection
10. Rapid Poison
```

## Paper 1: MASPOB

```text
MASPOB: Bandit-Based Prompt Optimization for Multi-Agent Systems with Graph Neural Networks
```

### Why We Care

这篇非常适合我们现在的 OpenOPC / Agent Harness / multi-agent workflow。

它研究的问题是：

```text
Multi-Agent System 里面，prompt 不是独立变量。
一个 agent 的 prompt 会影响另一个 agent 的输出。
整个系统还有 topology coupling。
```

这和真实 agent organization 非常像：

```text
manager agent
researcher agent
coder agent
reviewer agent
deployment agent
public-safe reviewer
```

如果每个 agent 的 prompt 单独调，很容易局部最优。

MASPOB 的思路是：

```text
bandit -> 低成本探索 / exploitation
GNN -> 捕捉 multi-agent topology
coordinate ascent -> 把组合爆炸拆成可优化子问题
```

### Connect to Our OS

它可以直接进入：

```text
OpenOPC study
AgentSpace study
Pengyi Research OS
FICC project role prompt design
interview mock agent design
CV reviewer agent design
```

我们以后如果做 `Pengyi Agent Harness v2`，可以借鉴：

```text
prompt 不只是 prompt engineering
prompt 是 multi-agent system parameter
```

### Interview Hook

```text
I am interested in MASPOB because it treats prompt optimization in multi-agent systems as a structured optimization problem rather than isolated prompt tuning. This is relevant to agentic workflows where roles interact through a task graph.
```

## Paper 2: T²PO

```text
T²PO: Uncertainty-Guided Exploration Control for Stable Multi-Turn Agentic Reinforcement Learning
```

### Why We Care

这篇是 agentic RL。

它解决的问题很直接：

```text
LLM agent 在多轮任务里会 hesitation。
也就是反复生成低信息动作 / token，浪费 rollout budget，训练不稳定。
```

它的思路是：

```text
token-level uncertainty
turn-level uncertainty
exploration control
resample low-progress turns
stabilize multi-turn RL
```

这正好接我们的：

```text
MLRL003: RL 基础
MLRL004: RLHF / Agent Training
OpenOPC company mode
Vibe-Trading agent workflow
AI-Trader agent runtime
```

### Connect to Our OS

我们做 agent 系统时，经常遇到：

```text
agent 一直解释，不行动
agent 一直绕圈，不推进
agent 工具调用低效
agent 做了很多低信息步骤
```

T²PO 从训练层面处理这种问题。

### Interview Hook

```text
T²PO is interesting because it focuses on exploration control for multi-turn LLM agents. Instead of only optimizing final reward, it detects low-information token or turn behavior and uses uncertainty to improve rollout efficiency.
```

## Paper 3: HELIX

```text
HELIX: Hybrid Encoding with Learnable Identity and Cross-dimensional Synthesis for Time Series Imputation
```

### Why We Care

这篇适合 quant / FICC / futures / macro data。

金融时间序列很少是干净的：

```text
missing values
asynchronous observations
multi-frequency features
cross-asset dependencies
macro + market + fundamental mixed features
```

HELIX 的关键想法是：

```text
给每个 feature 一个 learnable identity。
让模型持续记住 feature 的语义属性。
再通过 temporal-feature attention 学 cross-feature dependency。
```

这对我们有两个启发：

```text
1. time-series imputation 不是简单 forward fill / mean fill。
2. feature identity 可以帮助模型稳定理解多变量时间序列结构。
```

### Connect to Our OS

可以接：

```text
Futures Quant Research Workflow
FICC yield curve / macro factor data
WorldQuant alpha feature engineering
Quant data quality pipeline
```

### Interview Hook

```text
HELIX is useful for quant research because financial time-series data is often incomplete and multi-dimensional. A learnable feature identity can help preserve stable cross-feature semantics instead of rediscovering relationships from scratch at every layer.
```

## Paper 4: Updating Parametric Knowledge with Context Distillation

```text
Updating Parametric Knowledge with Context Distillation Retains Post-Training Capabilities
```

### Why We Care

这是 LLM knowledge update / continual adaptation。

问题是：

```text
模型需要学新知识。
但直接 fine-tune 可能忘掉 instruction following、reasoning、factual capabilities。
```

它的方向是：

```text
context distillation
split contexts
learn new knowledge
retain post-training capabilities
```

### Connect to Our OS

这和 RAG / memory / fine-tuning 的边界有关：

```text
哪些知识放 RAG？
哪些知识进入 parametric memory？
如何避免更新知识时破坏已有能力？
```

对我们做 Research OS 很关键。

## Paper 5: HOBIT

```text
HOBIT: Hardness Optimized Batch Sampling for InfoNCE Training
```

### Why We Care

这篇和 retrieval / embedding / RAG 有关。

InfoNCE 训练里，in-batch negatives 很重要。
但如果 batch 里的 negative 太容易，模型很快饱和。

HOBIT 的想法是：

```text
通过优化 batch composition，提高 in-batch hard negative 质量。
```

这能启发：

```text
RAG retriever training
financial document retrieval
semantic search
research memory embedding
```

### Connect to Our OS

以后如果我们做：

```text
FICC document RAG
CV / interview material retrieval
research paper memory
banking workflow knowledge base
```

就会遇到 embedding / retrieval quality 问题。

HOBIT 是值得放进 RAG 训练地图的。

## Paper 6: WaterSIC

```text
WaterSIC: Information-Theoretically (Near) Optimal Linear Layer Quantization
```

### Why We Care

这篇是 LLM quantization。

它用信息论视角分析线性层低比特量化，并指出 GPTQ 这类方法可能离 information-theoretic limit 有 gap。

核心想法是：

```text
waterfilling
allocate quantization rate differently across input columns
1-4 bit quantization
```

### Connect to Our OS

它和我们有关的点：

```text
local LLM deployment
low-cost inference
agent runtime efficiency
edge / personal AI OS
```

如果以后我们要本地跑更多 agent，模型压缩和推理成本会很重要。

## Paper 7: Language Model Circuits Are Sparse in the Neuron Basis

```text
Language Model Circuits Are Sparse in the Neuron Basis
```

### Why We Care

这篇是 mechanistic interpretability。

它讨论一个很重要的问题：

```text
是不是所有可解释 feature 都必须依赖 SAE？
```

论文观点是：

```text
MLP neurons 本身也可以构成 sparse feature basis。
```

并且它做了 end-to-end gradient-based attribution pipeline 来找 circuit。

### Connect to Our OS

这对我们不是短期工程项目，但能提升 AI research taste。

尤其是以后讨论：

```text
LLM reasoning
agent failure diagnosis
model internal representation
mechanistic evaluation
```

这篇可以作为 interpretability 入口。

## Paper 8: Latent Spherical Flow Policy

```text
Latent Spherical Flow Policy for Reinforcement Learning with Combinatorial Actions
```

### Why We Care

金融里很多问题都是 combinatorial action：

```text
选哪些资产
如何分配仓位
选择哪些交易路径
选择哪些对冲工具
组合约束怎么满足
```

这篇解决的是：

```text
RL action space 很大，而且有复杂 feasibility constraints。
```

它的思路是：

```text
learn stochastic policy in continuous latent space
use solver to map latent sample to valid combinatorial action
train value network directly in latent space
```

### Connect to Our OS

适合接：

```text
portfolio construction
execution planning
combinatorial strategy selection
constrained decision making
```

## Paper 9: Towards Efficient LLMs Annealing

```text
Towards Efficient LLMs Annealing with Principled Sample Selection
```

### Why We Care

这篇讨论 LLM pretraining 后期 annealing 阶段的数据选择。

它从 loss landscape 的 spectral geometry 出发，用 Hessian eigen-directions 来指导 sample selection。

这对我们有两个启发：

```text
1. data curation 本身就是 optimization。
2. LLM training 后期不是简单多喂数据，而是要选对数据。
```

适合进入：

```text
LLM training map
data curation map
post-training / annealing map
```

## Paper 10: Rapid Poison

```text
Rapid Poison: Practical Poisoning Attacks Against the Rapid Response Framework
```

### Why We Care

这篇是安全方向。

它研究：

```text
动态防御系统生成 synthetic training data 时，可能被 prompt injection 污染。
```

这和 agent / RAG / tool system 很相关。

因为真实系统里经常会：

```text
自动抓取攻击样本
自动生成训练数据
自动更新防御分类器
```

如果这条 pipeline 被污染，防御系统反而会学错。

### Connect to Our OS

适合进入：

```text
AI safety
agent security
RAG poisoning
tool-use boundary
human approval checkpoint
```

## How to Read These Papers

不要一篇论文从头读到尾。

我们采用 Research OS reading protocol：

```text
1. Read title / abstract / contribution.
2. Identify the problem.
3. Identify the system or algorithm.
4. Map it to our own projects.
5. Extract one interview hook.
6. Extract one possible coding demo.
7. Write one public-safe learning note.
```

每篇论文输出一张 card：

```text
Paper:
Problem:
Core idea:
Why it matters:
Our connection:
Possible demo:
Interview hook:
```

## First 3 Deep Dives

后续如果继续做 ICML2026 系列，我建议：

```text
ICML2026002 -> MASPOB deep dive
ICML2026003 -> T²PO deep dive
ICML2026004 -> HELIX deep dive
```

这三篇分别对应：

```text
Agent OS
Agent RL
Quant Time-Series
```

正好是我们当前最核心的三条线。

## Final Takeaway

这篇 `ICML2026001` 的核心结论：

```text
ICML 2026 Spotlight 里，最值得我们跟进的不是单纯 SOTA。
而是那些能进入我们 Research OS 的方法：
multi-agent optimization、agentic RL、time-series representation、RAG retrieval training、LLM knowledge update、efficient deployment、interpretability、combinatorial RL。
```

换句话说：

```text
顶会论文不是拿来仰望的。
顶会论文要被我们拆成 project、demo、CV bullet、interview story 和下一轮 research loop。
```

## Sources

- ICML 2026 Conference OpenReview: <https://openreview.net/group?id=ICML.cc/2026/Conference>
- ICML 2026 Spotlight index: <https://papers.cool/venue/ICML.2026?group=Spotlight>
- T²PO GitHub: <https://github.com/WillDreamer/T2PO>
- Rethinking LLM Ensembling arXiv: <https://arxiv.org/abs/2605.00419>
