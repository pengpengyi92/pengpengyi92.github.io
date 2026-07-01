---
title: "MLRL000: Machine Learning / Reinforcement Learning / PyTorch / Transformer 总地图"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, AI Foundations]
tags: [pengyi-mlrl-map, mlrl000, machine-learning, reinforcement-learning, pytorch, transformer, deep-learning, ai-foundations, research-os]
---

这是一个新的基础能力系列：

```text
PENGYI_ML_RL_MAP
```

这一篇是：

```text
MLRL000 -> Machine Learning / Reinforcement Learning / PyTorch / Transformer 总地图
```

这个系列的目标不是把每一个公式都讲完。
目标是先建立一张可以持续复用的底层地图：

```text
Machine Learning 给我们学习问题的语言。
PyTorch 给我们实现模型和训练循环的工程底座。
Transformer 给我们理解 LLM 的核心结构。
Reinforcement Learning 给我们理解 agent、反馈、奖励和行为优化的语言。
```

如果我们后面要做：

```text
AI Scientist
Coding Agent Harness
Quant Research OS
RAG / Graph RAG
LLM Agent
Research Automation
RLHF / Agent Training
```

那 `ML / DL / PyTorch / Transformer / RL` 就是必须打透的底层能力。

## 一句话总览

我现在对这条主线的理解是：

```text
ML = 从数据中学习规律
Deep Learning = 用神经网络学习复杂函数
PyTorch = 把神经网络训练过程工程化
Transformer = 当前 LLM 的核心神经网络结构
RL = 在环境反馈中学习决策策略
```

把它们合起来看：

```text
Data / Environment
    -> Model / Policy
    -> Loss / Reward
    -> Optimization
    -> Evaluation
    -> Deployment / Harness
```

这也是我们做 `Research OS` 和 `Harness` 时反复遇到的底层闭环。

## 总地图

这一条线可以拆成五层。

```text
Layer 1: Machine Learning
    supervised learning
    unsupervised learning
    self-supervised learning
    reinforcement learning

Layer 2: Deep Learning
    tensor
    neural network
    loss function
    backpropagation
    optimizer
    training loop

Layer 3: PyTorch
    torch.Tensor
    autograd
    nn.Module
    Dataset / DataLoader
    optimizer
    checkpoint
    evaluation

Layer 4: Transformer
    tokenization
    embedding
    positional information
    Q / K / V attention
    multi-head attention
    feed-forward network
    residual connection
    layer normalization
    logits

Layer 5: Reinforcement Learning
    agent
    environment
    state
    action
    reward
    policy
    value
    Q function
    actor-critic
    PPO / preference optimization
```

这五层不是并列的知识点。
它们更像一条工程链：

```text
ML defines the learning problem.
PyTorch implements the learning system.
Transformer is a dominant model architecture.
RL defines behavior optimization under feedback.
Harness makes the whole loop executable, observable, and evaluable.
```

## Machine Learning 是什么

Machine Learning 的核心问题是：

```text
给定数据和目标，学习一个可以泛化的函数。
```

最常见的几类：

```text
Supervised Learning
    input -> label
    example: image -> class, factor features -> return label

Unsupervised Learning
    input -> structure
    example: clustering, representation learning, anomaly detection

Self-supervised Learning
    input itself creates the target
    example: predict masked token, predict next token

Reinforcement Learning
    state -> action -> reward -> next state
    example: game agent, trading agent, tool-use agent
```

如果讲得面试化一点：

```text
Supervised learning learns from explicit labels.
Self-supervised learning creates labels from data itself.
Reinforcement learning learns from delayed feedback through interaction.
```

对我们来说，最关键的是不要把 ML 当成一堆算法名字。
应该把它当成一套问题建模语言：

```text
What is the input?
What is the target?
What is the loss?
What is the evaluation metric?
What does generalization mean?
What failure mode should be diagnosed?
```

这几个问题，后面无论做 quant factor、RAG eval、coding agent eval，都会重复出现。

## Deep Learning 是什么

Deep Learning 本质上是：

```text
用多层可微分函数来逼近复杂映射。
```

一条最小训练链路是：

```text
input x
  -> model f_theta(x)
  -> prediction y_hat
  -> loss(y_hat, y)
  -> backward
  -> optimizer.step()
  -> updated theta
```

关键组件：

```text
Tensor
    数据容器，也是计算图里的基本对象。

Parameter
    需要被训练更新的张量。

Forward
    模型从输入到输出的计算过程。

Loss
    预测和目标之间的差距。

Backward
    根据 loss 计算梯度。

Optimizer
    根据梯度更新参数。
```

这就是所有大模型训练、微调、embedding model、reward model 的共同骨架。

## PyTorch 的架构

PyTorch 可以理解成一个深度学习工程系统。
它把训练神经网络需要的核心能力封装起来：

```text
torch.Tensor
    多维数组 + GPU 计算 + 自动求导入口。

autograd
    自动记录计算图，并根据 loss 反向传播梯度。

nn.Module
    模型结构的标准组织方式。

Dataset / DataLoader
    数据读取、batch、shuffle、并行加载。

loss function
    训练目标。

optimizer
    参数更新规则，例如 SGD / Adam / AdamW。

training loop
    把 forward / loss / backward / update 串成可重复流程。

checkpoint
    保存模型权重和训练状态。

eval mode
    固定 dropout / batchnorm 等行为，进入评估阶段。
```

最小 PyTorch 训练循环大概是这样：

```python
for batch in dataloader:
    x, y = batch

    optimizer.zero_grad()

    y_hat = model(x)
    loss = criterion(y_hat, y)

    loss.backward()
    optimizer.step()
```

这个循环看起来很短，但里面包含了深度学习工程的核心：

```text
data -> model -> loss -> gradient -> parameter update
```

如果以后要讲清楚“我会 PyTorch”，不能只说会写 `model.fit`。
应该能讲清楚：

```text
1. 数据怎么变成 batch
2. 模型结构怎么组织成 nn.Module
3. loss 怎么定义
4. gradient 怎么产生
5. optimizer 怎么更新参数
6. evaluation 怎么和 training 分开
7. checkpoint / reproducibility / device placement 怎么处理
```

这就是工程可信度。

## Transformer 的架构

Transformer 是当前 LLM 的核心结构。
最常见的 decoder-only LLM 可以粗略写成：

```text
text
  -> tokenizer
  -> token ids
  -> token embedding
  -> positional information
  -> transformer blocks
  -> logits
  -> next token distribution
```

一个 Transformer block 通常是：

```text
x
  -> LayerNorm
  -> Multi-Head Self-Attention
  -> Residual Add
  -> LayerNorm
  -> Feed-Forward Network
  -> Residual Add
```

Attention 的核心思想是：

```text
每一个 token 都根据当前上下文，决定应该关注哪些 token。
```

Q / K / V 可以这样理解：

```text
Q = query, 当前 token 想找什么信息
K = key, 每个 token 能提供什么索引信号
V = value, 每个 token 真正携带的内容
```

Attention 的计算直觉：

```text
Q 和 K 做匹配 -> 得到注意力权重 -> 对 V 加权求和 -> 得到上下文表示
```

Multi-head attention 则是：

```text
用多个 attention head 从不同子空间看同一段上下文。
```

Transformer 的三个常见形态：

```text
Encoder-only
    代表：BERT-style models
    擅长：理解、分类、检索、embedding

Decoder-only
    代表：GPT-style LLMs
    擅长：next-token generation、chat、tool use、agent

Encoder-decoder
    代表：T5-style models
    擅长：sequence-to-sequence transformation
```

如果我们要解释 LLM，本质上是在解释：

```text
一个基于 Transformer 的 next-token prediction system，
通过大规模预训练、指令微调、偏好优化和工具环境，变成可以交互执行任务的 agent。
```

## Reinforcement Learning 是什么

Reinforcement Learning 研究的是：

```text
agent 如何在 environment 中通过 action 获得 reward，并学习更好的 policy。
```

基础元素：

```text
Agent
    做决策的主体。

Environment
    agent 所处的外部系统。

State
    当前环境信息。

Action
    agent 可以采取的动作。

Reward
    环境给出的反馈信号。

Policy
    state -> action 的决策规则。

Value
    某个 state 未来累计 reward 的期望。

Q function
    某个 state-action pair 未来累计 reward 的期望。
```

最小交互循环：

```text
state_t
  -> agent chooses action_t
  -> environment returns reward_t and state_{t+1}
  -> agent updates policy
```

RL 和 supervised learning 最大的区别：

```text
Supervised learning: 每个样本有明确 label。
Reinforcement learning: action 的好坏通常通过延迟 reward 才能看出来。
```

典型方法族：

```text
Value-based
    学 Q function，例如 DQN。

Policy gradient
    直接优化 policy。

Actor-critic
    actor 负责行动，critic 负责评价。

Model-based RL
    学环境模型，再基于模型规划。

Offline RL
    从历史数据学习策略，不直接在线探索。
```

LLM 时代常见的 RL / preference optimization 关系：

```text
pretraining
  -> supervised fine-tuning
  -> reward / preference modeling
  -> RLHF or preference optimization
  -> aligned assistant behavior
```

这里的重点不是死记 PPO。
重点是理解：

```text
RL 提供了一种“根据反馈优化行为”的框架。
```

这对 coding agent、tool-use agent、quant strategy agent 都非常关键。

## PyTorch、Transformer、RL 怎么连起来

可以这样合并：

```text
PyTorch = implementation substrate
Transformer = model architecture
ML = learning objective
RL = feedback-driven behavior optimization
Harness = controlled execution and evaluation environment
```

举一个 LLM agent 的例子：

```text
Transformer model
  -> generates action / tool call
  -> harness executes tool call
  -> environment returns observation
  -> evaluator gives score / reward / diagnosis
  -> training or prompting loop improves behavior
```

这就是为什么我们最近反复讲 `harness`。
没有 harness，模型只是在输出文本。
有了 harness，模型才进入：

```text
task
  -> action
  -> observation
  -> evaluation
  -> memory
  -> iteration
```

## 对 Quant Research OS 的意义

在量化研究里，这条线也可以对应起来：

```text
ML
    学因子、预测收益、分类市场状态、检测异常。

PyTorch
    实现深度时序模型、cross-sectional model、representation model。

Transformer
    处理长序列、文本新闻、财报、研报、行情序列、multi-modal signal。

RL
    研究交易执行、组合调仓、策略选择、agent decision loop。

Harness
    把策略生成、回测、诊断、风险约束、报告生成串成可重复系统。
```

我们后面要做的 `Pengyi Quant Research OS`，不是只写一个 notebook。
更应该是：

```text
idea -> data -> model/spec -> experiment -> backtest -> diagnosis -> report -> next plan
```

这正好需要 ML/RL 底层能力。

## 对 Coding Agent Harness 的意义

如果我们要站在 DeepSeek PM / coding agent harness 的视角，ML/RL 也很重要。

一个 coding agent harness 不是只看模型能不能写代码。
它要看：

```text
Can the agent understand the task?
Can it inspect the repo?
Can it plan?
Can it edit safely?
Can it run tests?
Can it recover from failure?
Can it explain the final change?
Can it improve over repeated tasks?
```

这里面每一项都可以被看成 learning / evaluation / feedback 问题：

```text
task success rate
test pass rate
diff quality
tool-use efficiency
rollback safety
instruction following
long-horizon consistency
```

所以 ML/RL 不是孤立课程。
它们会进入我们对 agent product、harness product、AI scientist system 的判断。

## 之后的学习路线

这一篇只是 000。
后面建议按这个顺序拆：

```text
MLRL001: PyTorch 架构与训练循环
    tensor / autograd / nn.Module / DataLoader / optimizer / checkpoint / eval

MLRL002: Transformer 架构
    tokenization / embedding / QKV attention / MHA / FFN / residual / layer norm / decoder-only LLM

MLRL003: Reinforcement Learning 基础
    MDP / policy / value / Q-learning / policy gradient / actor-critic / PPO

MLRL004: RLHF / Preference Optimization / Agent Training
    SFT / reward model / RLHF / DPO / eval harness / agent feedback loop

MLRL005: Quant / Agent / LLM 的统一视角
    factor model / trading agent / coding agent / research agent / evaluation harness
```

这条路线不是为了“补课”。
它是为了把我们的输出能力接到更坚实的底座上：

```text
能读 paper。
能看 repo。
能实现 demo。
能解释架构。
能设计 eval。
能提出改进。
能把它写成公开可验证的 credit。
```

## 面试可用的精简表达

如果需要在面试里快速解释，可以这样说：

```text
I view machine learning as the general language for defining learning problems:
data, objective, optimization, evaluation, and generalization.

PyTorch is the engineering substrate that lets us implement differentiable models
with tensors, autograd, modules, dataloaders, losses, optimizers, and training loops.

Transformer is the dominant architecture behind LLMs, where token embeddings pass
through repeated attention and feed-forward blocks to produce next-token logits.

Reinforcement learning extends the setup from static labels to interactive feedback,
where an agent learns a policy through rewards from an environment.

For AI agents and quant research systems, the key is to connect these foundations
with a harness: executable tasks, observations, evaluation, memory, and iteration.
```

这段可以直接变成我们之后 AI / Quant / Harness 面试里的底层叙事。

## 当前结论

`MLRL000` 的核心结论：

```text
Machine Learning 是学习问题语言。
PyTorch 是工程实现底座。
Transformer 是 LLM 结构核心。
Reinforcement Learning 是交互决策和反馈优化语言。
Harness 是把这些能力变成可执行、可评估、可迭代系统的外壳。
```

这条线后面要持续深挖。
因为它是我们从“会用 AI”走向“能设计 AI scientist / agent / quant system”的基础。
