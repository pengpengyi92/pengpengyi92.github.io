---
title: "MLRL005: Quant / Agent / LLM 里的 ML-RL 统一视角"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, AI Foundations]
tags: [pengyi-mlrl-map, mlrl005, quant, agent, llm, ml-rl, research-os, harness, ai-scientist]
---

这是 `PENGYI_ML_RL_MAP` 的第六篇：

```text
MLRL005 -> Quant / Agent / LLM 里的 ML-RL 统一视角
```

前面几篇分别讲：

```text
MLRL000: Machine Learning / Reinforcement Learning 总地图
MLRL001: PyTorch 架构与训练循环
MLRL002: Transformer 架构
MLRL003: Reinforcement Learning 基础
MLRL004: RLHF / Agent Training
```

这一篇做总融合。

我现在的判断是：

```text
Quant、Agent、LLM 不是三条完全分离的线。
它们都可以放进同一套 ML-RL-Harness 框架里理解。
```

这对我们很重要。
因为我们后面的核心方向就是：

```text
AI Scientist
Quant Research OS
Coding Agent Harness
Research Agent
Credit OS
open-source project portfolio
```

必须有一套统一语言，不能每个项目都从零解释。

## 一句话统一视角

```text
ML learns patterns from data.
RL learns behavior from feedback.
LLM provides a general policy and representation engine.
Harness defines the executable environment and evaluation loop.
Quant and Agent systems are domain-specific instantiations of this loop.
```

中文：

```text
ML 负责从数据里学规律。
RL 负责从反馈里学行为。
LLM 负责提供通用语言、代码、推理、工具调用能力。
Harness 负责定义环境、动作、约束、评估和迭代。
Quant 和 Agent 是这套系统在不同领域里的落地形态。
```

这就是统一视角。

## 一个总公式

可以把很多系统写成同一条链：

```text
Input / State
  -> Representation
  -> Model / Policy
  -> Action / Prediction
  -> Environment / Market / Tool
  -> Feedback / Reward / Metric
  -> Diagnosis
  -> Update / Next Plan
```

这条链在不同领域有不同名字。

在 ML 里：

```text
data -> model -> prediction -> loss -> optimization -> validation
```

在 RL 里：

```text
state -> policy -> action -> environment -> reward -> policy update
```

在 LLM agent 里：

```text
context -> LLM -> message/tool call -> tool/environment -> observation/eval -> next action
```

在 Quant Research OS 里：

```text
idea/data -> factor/model -> signal/strategy -> backtest/market simulator -> metrics/diagnosis -> next research plan
```

它们其实是同一个闭环的不同投影。

## ML 视角: 预测和泛化

ML 的核心问题：

```text
在训练数据上学到的规律，能否泛化到未见数据？
```

量化里的对应：

```text
训练期有效的因子，能否在未来市场继续有效？
```

RAG 里的对应：

```text
检索和生成系统在新问题上是否仍然准确？
```

coding agent 里的对应：

```text
在 benchmark 上通过的 agent，能否解决真实 repo 中的新任务？
```

所以无论哪个方向，都要问：

```text
train distribution 是什么？
test distribution 是什么？
metric 是什么？
failure mode 是什么？
是否存在 leakage？
是否过拟合 eval？
```

这是 ML 给我们的基本纪律。

## RL 视角: 行为和反馈

RL 的核心问题：

```text
一个 agent 如何在环境反馈中学习长期更好的行为？
```

LLM agent 对应：

```text
prompt/context -> action/tool call -> observation -> next action
```

Quant 对应：

```text
market state -> trade/rebalance -> PnL/risk -> next position
```

Research OS 对应：

```text
research state -> experiment action -> result/diagnosis -> next plan
```

RL 强调：

```text
sequential decision
long-term return
credit assignment
exploration
policy improvement
```

这比普通监督学习更接近 agent。

## Harness 视角: 环境和评估

Harness 是我们最近反复讲的关键概念。

在统一视角里：

```text
Harness = environment + tools + constraints + logs + evaluators + replay + reporting.
```

它把模型从“会输出”变成“能执行”。

没有 harness：

```text
LLM 只是文本生成器。
quant model 只是 notebook 里的预测器。
research idea 只是想法。
```

有 harness：

```text
agent 可以行动。
策略可以回测。
实验可以复现。
结果可以审计。
失败可以诊断。
下一轮可以规划。
```

这就是我们做 Research OS 的底层理由。

## Quant 的统一建模

Quant 可以拆成两种任务：

```text
Prediction
    预测收益、风险、波动、成交量、事件影响。

Decision
    选股、调仓、交易执行、组合优化、风险控制。
```

Prediction 更像 ML：

```text
features -> model -> expected return / risk / label
```

Decision 更像 RL / control：

```text
market state + portfolio state -> action -> reward/risk outcome
```

所以量化系统要同时考虑：

```text
supervised learning
time-series validation
ranking objective
portfolio construction
transaction cost
risk constraints
backtest realism
online/offline gap
```

一个完整 quant harness：

```text
data layer
feature layer
model layer
signal layer
portfolio layer
backtest layer
risk layer
diagnosis layer
report layer
next-plan layer
```

这就是 `Pengyi Quant Research OS` 的结构来源。

## LLM 的统一建模

LLM 可以从三个层面看：

```text
Model
    Transformer next-token predictor。

Policy
    context -> next token / tool call / response。

Agent
    在 harness 中进行多步行动的 policy。
```

预训练阶段：

```text
ML objective
    next-token prediction
```

后训练阶段：

```text
preference / RL objective
    human-preferred behavior
    safer behavior
    more helpful behavior
```

Agent 阶段：

```text
environment objective
    task completion
    tool-use correctness
    long-horizon reliability
```

所以 LLM 不只是模型。
进入工具环境后，它就是一种 policy。

## Agent 的统一建模

Agent 系统可以统一写成：

```text
state/context
  -> policy/model
  -> action
  -> tool/environment
  -> observation
  -> evaluator
  -> memory/update
```

不同 agent 的 action 不一样：

```text
coding agent
    read file, search, edit, test, commit, explain

research agent
    search paper, extract idea, design experiment, run eval, write report

quant agent
    generate factor, implement, backtest, diagnose, propose next factor

office agent
    read email, draft reply, update calendar, summarize document
```

但底层都一样：

```text
policy under constraints
```

所以 agent 产品能力的核心不是“模型很聪明”。
而是：

```text
state representation
action space design
tool reliability
evaluation signal
memory quality
recovery from failure
safety boundary
```

这就是 ML-RL-Harness 统一视角能带来的判断力。

## Research OS 的统一建模

Research OS 本质上也是 agent harness。

一个研究任务：

```text
problem
  -> hypothesis
  -> implementation
  -> experiment
  -> result
  -> diagnosis
  -> next hypothesis
```

这条链可以 ML 化：

```text
每次实验都是一个 data point。
每次结果都是 feedback。
每次 diagnosis 都是 representation update。
每次 next plan 都是 policy improvement。
```

对 AI Scientist 来说，最关键的是：

```text
把研究过程变成可记录、可复现、可评估、可迭代的系统。
```

这就是：

```text
Research OS = scientific process harness.
```

## RAG / Graph RAG 的位置

RAG 可以放在统一视角里的 representation layer。

```text
query / task
  -> retrieve relevant knowledge
  -> construct context
  -> LLM policy acts with better state information
```

Graph RAG 则是把知识组织成图：

```text
entity
relation
community
path
subgraph
```

它增强的是：

```text
state representation quality
```

对 agent 来说，state 表示越好，action 才越可能好。

所以 RAG 不是孤立技术。
它是 agent / LLM / research system 的状态构造模块。

## Evaluation 的统一问题

所有系统最后都会卡在 evaluation。

ML eval：

```text
accuracy
F1
AUC
MSE
ranking metric
out-of-sample performance
```

Quant eval：

```text
IC
Sharpe
drawdown
turnover
cost
capacity
stability
factor decay
```

LLM eval：

```text
helpfulness
harmlessness
truthfulness
instruction following
reasoning
tool-use success
```

Coding agent eval：

```text
test pass rate
patch correctness
minimal diff
repo understanding
failure recovery
latency
cost
```

Research agent eval：

```text
hypothesis quality
implementation correctness
experiment validity
bias diagnosis
novelty
report usefulness
```

评价指标不同，但共同问题一样：

```text
metric 是否真的代表目标？
eval 是否会被过拟合？
有没有 hidden failure mode？
能不能复现？
有没有人工审查入口？
```

这就是 harness 设计的核心。

## 三种系统的对照表

```text
Dimension          Quant System              LLM Agent                  Research OS
--------------------------------------------------------------------------------------------
State              market/data/position       context/tool observations  problem/history/results
Action             trade/rebalance/factor     message/tool call/edit     hypothesis/experiment
Model              predictor/strategy         Transformer policy         planner/research agent
Reward             PnL/risk metric            task success/eval score    valid result/insight
Environment        market/backtest            tools/repo/browser         experiment platform
Memory             factor library/logs        conversation/vector store  research notes/artifacts
Evaluator          backtest/risk report        tests/judges/human         review/metrics/report
Failure            leakage/overfit/cost        hallucination/bad edit     invalid experiment
Harness            Quant Research OS          Agent Harness              Research OS
```

这张表就是我们后面做项目时的统一索引。

## 对我们项目路线的含义

这条统一视角可以直接指导我们的项目组织。

### Pengyi Quant Research OS

核心能力：

```text
factor hypothesis
factor implementation
backtest
bias diagnosis
risk report
next research plan
```

对应 ML-RL-Harness：

```text
ML: factor prediction
RL: strategy decision and feedback
Harness: experiment and backtest loop
```

### DeepSeek Coding Agent Harness

核心能力：

```text
task intake
repo inspection
planning
file edit
test execution
failure recovery
final report
```

对应 ML-RL-Harness：

```text
ML: model behavior and evaluator learning
RL: action sequence optimization
Harness: repo/tool/test environment
```

### AI Scientist

核心能力：

```text
read literature
generate hypothesis
implement experiment
evaluate result
diagnose weakness
write paper-style report
plan next step
```

对应 ML-RL-Harness：

```text
ML: evidence and pattern learning
RL: research action policy
Harness: scientific workflow environment
```

## 我们该怎么学习

后续学习不能散。
建议按四条线同步推进：

```text
1. Foundation
    PyTorch, Transformer, RL, RLHF, evaluation

2. Engineering
    repo reading, training loop, data pipeline, benchmark, deployment

3. Domain
    quant research, backtest, factor, risk, financial data

4. Presentation
    GitHub, website, reports, demos, interview narrative
```

这四条线共同构成 credit。

只学算法不够。
只做网站不够。
只写 demo 不够。

要把它们接成：

```text
credible technical artifact
```

也就是全球可验证的能力证明。

## 面试可用表达

如果被问“你怎么把 ML、RL、LLM、Agent、Quant 放在一起理解”，可以这样说：

```text
I use a unified ML-RL-Harness view.
Machine learning learns predictive structure from data.
Reinforcement learning learns behavior from feedback through interaction.
LLMs can be seen as Transformer-based policies that map context to tokens or tool actions.
A harness defines the executable environment, action space, constraints, logs, and evaluation signals.

In quant research, this becomes data, factors, strategies, backtests, risk metrics, and next research plans.
In coding agents, it becomes repository state, file edits, tests, tool calls, and task success.
In research automation, it becomes hypothesis generation, experiment execution, result diagnosis, and paper-style reporting.
```

再补一句：

```text
The hard part is not just model intelligence.
The hard part is building the loop: state representation, action design, evaluation, memory, safety, and iteration.
```

这就是我们现在所有项目的共同叙事。

## 当前结论

`MLRL005` 的核心结论：

```text
Quant、Agent、LLM 可以统一放进 ML-RL-Harness 框架。
```

这套框架让我们能同时解释：

```text
为什么要学 PyTorch。
为什么要懂 Transformer。
为什么 RL 对 agent 重要。
为什么 RLHF 是 LLM 后训练核心。
为什么 harness 是 AI product 和 research system 的关键。
为什么 Quant Research OS 可以成为我们的核心项目。
```

后续我们不是零散学习。
我们是在搭建一套：

```text
AI Scientist + Quant Research + Agent Harness 的统一能力系统。
```
