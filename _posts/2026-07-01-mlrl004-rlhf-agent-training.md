---
title: "MLRL004: RLHF / Agent Training"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, AI Foundations]
tags: [pengyi-mlrl-map, mlrl004, rlhf, agent-training, preference-optimization, llm, reward-model, evaluator, harness]
---

这是 `PENGYI_ML_RL_MAP` 的第五篇：

```text
MLRL004 -> RLHF / Agent Training
```

前面三篇分别打了基础：

```text
MLRL001: PyTorch 架构与训练循环
MLRL002: Transformer 架构
MLRL003: Reinforcement Learning 基础
```

这一篇把它们接起来：

```text
LLM 如何从“会预测下一个 token”变成“更符合人类偏好、更能完成任务、更像 agent”。
```

我现在的判断是：

```text
RLHF / Agent Training 的本质，是把人类偏好、任务成功、工具反馈、评估器分数，转化成可优化的行为反馈。
```

## 一句话定义

```text
RLHF = using human feedback to improve a language model's behavior beyond next-token pretraining.
```

中文：

```text
RLHF = 用人类反馈训练模型，让模型输出更符合人类偏好和任务目标。
```

Agent Training 更宽一点：

```text
Agent Training = using trajectories, tool feedback, environment outcomes, and evaluators to improve an agent's behavior.
```

也就是说：

```text
RLHF 更偏 assistant alignment。
Agent Training 更偏任务执行能力、工具使用能力、长程行为能力。
```

## LLM 训练的四层

现代 LLM 可以粗略拆成四层训练：

```text
1. Pretraining
    大规模文本上的 next-token prediction。

2. Supervised Fine-Tuning
    用高质量指令数据训练 assistant 行为。

3. Preference / Reward Training
    学习人类或评估器偏好。

4. Policy Optimization / Preference Optimization
    用反馈进一步优化模型行为。
```

这四层可以写成：

```text
internet-scale language modeling
  -> instruction following
  -> preference modeling
  -> aligned / task-oriented behavior
```

每一层解决的问题不同。

## Pretraining: 学语言和世界模式

预训练目标通常是：

```text
predict next token
```

输入：

```text
token_1, token_2, ..., token_t
```

目标：

```text
predict token_{t+1}
```

这看起来简单，但在大规模数据和模型下，它会学习：

```text
syntax
semantics
facts
reasoning patterns
code patterns
domain language
latent world structure
```

但预训练模型不一定会变成好助手。
因为它只是学会续写分布。

它可能：

```text
不知道如何遵循指令
不知道如何拒绝危险请求
不知道如何输出结构化答案
不知道如何在多轮对话中保持角色
不知道如何调用工具
```

所以需要后训练。

## SFT: Supervised Fine-Tuning

SFT 用高质量指令数据训练模型。

数据形态：

```text
instruction -> ideal response
```

或者多轮：

```text
conversation history -> assistant response
```

目标：

```text
让模型模仿高质量示范答案。
```

SFT 的作用：

```text
instruction following
chat format adaptation
domain response style
tool-use demonstration
reasoning format
safe behavior examples
```

SFT 可以让模型像一个 assistant。
但它也有限制：

```text
它只学习示范，不直接学习偏好排序。
它不知道两个答案哪个更好，除非数据里显式体现。
它容易模仿数据风格，但不一定优化最终任务成功。
```

所以需要 preference。

## Preference Data

偏好数据通常不是单个标准答案。
而是比较：

```text
prompt
response A
response B
human preference: A better than B
```

或者：

```text
trajectory A
trajectory B
which one completed the task better?
```

这比 SFT 更接近真实产品判断。

例如两个答案都能回答问题，但一个更好：

```text
more accurate
more concise
more helpful
safer
better structured
less hallucinated
better follows instruction
```

偏好数据把“质量判断”显式放进训练。

## Reward Model

RLHF 经典流程里，会训练一个 reward model。

输入：

```text
prompt + response
```

输出：

```text
scalar reward
```

目标：

```text
让 reward model 给人类更喜欢的 response 更高分。
```

直觉：

```text
reward model = 学出来的人类偏好打分器。
```

然后可以用它给模型输出打分，再用 RL 优化模型。

这对应 RL 语言：

```text
policy
    language model

action
    generated tokens / response

reward
    reward model score
```

## PPO-style RLHF

经典 RLHF 流程可以写成：

```text
pretrained model
  -> SFT model
  -> reward model
  -> PPO policy optimization
  -> aligned assistant
```

PPO 优化时，通常还会加 KL 约束。
原因是：

```text
不能让优化后的 policy 离原始 SFT model 太远。
```

否则模型可能为了 reward model 分数做出奇怪行为。

这叫 reward hacking 风险：

```text
模型优化了 reward model，却没有真正优化人类想要的行为。
```

所以 RLHF 不是简单“让 reward 越高越好”。
必须控制：

```text
reward quality
policy drift
safety
diversity
task success
human evaluation
```

## DPO 和 Preference Optimization

后来的 preference optimization 方法，不一定显式训练 reward model 再跑 PPO。

例如 DPO 类方法的直觉是：

```text
直接用 preference pairs 优化模型，让 preferred response 的概率相对更高。
```

它的工程吸引力：

```text
流程更简单
训练更稳定
不需要完整 RL rollout
更容易复现
```

但无论 PPO 还是 DPO，本质都在做一件事：

```text
把偏好信号变成模型行为优化。
```

所以面试里可以不要把某个算法说成唯一答案。
应该讲清楚训练信号：

```text
demonstration data
preference data
reward / evaluator score
policy update constraint
```

## Agent Training 和 RLHF 的区别

RLHF 多数时候关注：

```text
prompt -> response quality
```

Agent Training 关注：

```text
task -> multi-step trajectory -> environment outcome
```

例如 coding agent：

```text
read task
inspect repo
form plan
edit files
run tests
debug failures
summarize changes
```

它不是单轮回答问题。
它是一段 trajectory。

训练和评估时要看：

```text
Did it solve the task?
Did tests pass?
Did it make minimal correct edits?
Did it avoid unsafe operations?
Did it recover from errors?
Did it follow instructions?
Did it explain the result?
```

这就进入 agent harness。

## Agent Harness 的角色

Agent training 不能只靠模型。
必须有 harness。

Harness 定义：

```text
environment
tools
allowed actions
state observation
logging
evaluation
rollback / safety boundary
```

对 coding agent：

```text
repo filesystem
shell tools
test runner
git diff
lint
build system
instruction hierarchy
permission model
```

对 quant agent：

```text
data loader
factor library
backtest engine
risk constraints
diagnosis report
experiment tracking
```

如果没有 harness，就没有稳定的 training environment。

## Trajectory Data

Agent training 的核心数据不是普通 prompt-response。
而是 trajectory：

```text
observation_1
thought / plan_1
action_1
tool_result_1
observation_2
action_2
tool_result_2
...
final answer
score
```

trajectory 可以被用于：

```text
behavior cloning
error analysis
preference comparison
reward modeling
process supervision
tool-use policy training
eval benchmark construction
```

这也是为什么日志很重要。
没有高质量 logs，就没有高质量 agent training data。

## Outcome Reward 和 Process Reward

Agent 训练里有两种反馈：

```text
Outcome reward
    最终结果好不好。

Process reward
    中间过程每一步好不好。
```

例如数学题：

```text
outcome: final answer correct or not
process: reasoning steps correct or not
```

coding agent：

```text
outcome: tests pass or not
process: file search、edit scope、debug steps 是否合理
```

quant research agent：

```text
outcome: strategy backtest metric
process: data handling、bias check、risk report 是否合规
```

Process reward 更难标注，但对长程 agent 很关键。

## Evaluator

Agent training 经常需要 evaluator。

Evaluator 可以是：

```text
unit tests
compiler
static analyzer
backtest engine
human reviewer
LLM judge
rule-based checker
benchmark scorer
```

不同 evaluator 的可信度不同。

```text
unit tests
    对代码功能很硬，但覆盖不完整。

human reviewer
    质量高，但成本高。

LLM judge
    便宜灵活，但会有偏差。

backtest engine
    对策略结果直接，但容易被过拟合利用。
```

一个好的 harness 通常会组合多个 evaluator。

## Reward Hacking

只要有 reward，就有 reward hacking。

例子：

```text
模型学会讨好 LLM judge，但答案不真实。
策略过拟合 backtest，但实盘无效。
coding agent 修改测试而不是修 bug。
agent 输出很长解释，掩盖没有完成任务。
```

所以 reward 设计必须配合：

```text
hard constraints
manual review
adversarial tests
out-of-distribution evaluation
process logging
regression benchmark
```

这是做 agent product 的硬问题。

## Agent Training 的方法谱系

可以按训练信号分：

```text
Behavior Cloning
    模仿高质量 trajectory。

SFT for Tool Use
    学习工具调用格式和流程。

Preference Optimization
    从好坏 trajectory pair 学偏好。

RL with Environment Reward
    在环境中 rollout，用任务结果优化。

Process Supervision
    对中间步骤给反馈。

Self-Improvement
    agent 生成、反思、筛选、再训练。

Curriculum Learning
    从简单任务逐步到复杂任务。
```

对我们来说，最重要的是理解：

```text
agent training is not just fine-tuning text.
It is training behavior under environment constraints.
```

## 对 Coding Agent 的启发

Coding agent training 可以定义成：

```text
state
    task prompt, repo files, current diff, test output

action
    search, read file, edit, run test, explain, ask clarification

reward
    tests pass, minimal diff, style compliance, no unsafe edits, user satisfaction

trajectory
    full sequence from task intake to final answer
```

DeepSeek / Codex / Claude Code 这类 coding agent harness 的关键能力：

```text
repo understanding
tool-use reliability
edit safety
test-driven repair
context management
instruction following
failure recovery
diff quality
final communication
```

如果我们做 DeepSeek coding agent harness 方案，就要围绕这些指标建立评估和训练闭环。

## 对 Quant Agent 的启发

Quant research agent training 可以定义成：

```text
state
    research question, data schema, factor library, previous experiments

action
    propose hypothesis, implement factor, run backtest, diagnose bias, write report

reward
    valid experiment, clean implementation, robust out-of-sample result, low bias risk

trajectory
    research loop from idea to next plan
```

但 quant reward 不能只看收益。
还要看：

```text
risk
turnover
cost
drawdown
capacity
stability
leakage
economic rationale
```

这就是我们 `Pengyi Quant Research OS` 需要的训练与评估语言。

## 面试可用表达

如果被问“RLHF 是什么”，可以这样说：

```text
RLHF is a post-training framework that uses human feedback to improve a language model's behavior.
The typical pipeline is pretraining, supervised fine-tuning, preference data collection,
reward modeling, and policy optimization with constraints such as KL regularization.

The key idea is that pretraining teaches next-token prediction,
SFT teaches instruction-following behavior,
and preference optimization teaches the model to prefer outputs that humans or evaluators judge as better.
```

如果被问“Agent Training 和 RLHF 的关系”，可以这样说：

```text
Agent training generalizes the idea from single-turn response preference to multi-step trajectories.
For an agent, the environment, tools, observations, actions, logs, and evaluators form a harness.
The training signal can come from final task success, process-level feedback, human preference,
unit tests, backtests, or LLM judges.
The core challenge is reliable credit assignment and avoiding reward hacking.
```

## 当前结论

`MLRL004` 的核心结论：

```text
RLHF = 用人类偏好优化 LLM 行为。
Agent Training = 用 trajectory、工具反馈、环境结果和评估器优化 agent 行为。
```

它们的共同底层是：

```text
policy
feedback
reward / preference
evaluation
constrained optimization
harness
```

这直接连接我们的 DeepSeek coding agent harness、Quant Research OS、AI Scientist 系统。
