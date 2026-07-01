---
title: "MLRL003: Reinforcement Learning 基础"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, AI Foundations]
tags: [pengyi-mlrl-map, mlrl003, reinforcement-learning, mdp, policy, value-function, q-learning, actor-critic, ppo, agent]
---

这是 `PENGYI_ML_RL_MAP` 的第四篇：

```text
MLRL003 -> Reinforcement Learning 基础
```

`MLRL001` 讲 PyTorch。
`MLRL002` 讲 Transformer。
这一篇讲 RL，也就是 reinforcement learning。

我现在对 RL 的判断是：

```text
RL 是研究 agent 如何在环境反馈中学习决策策略的框架。
```

它不是只属于游戏 AI。
LLM agent、coding agent、tool-use agent、trading agent、robotics agent，都绕不开 RL 的语言：

```text
state
action
reward
policy
value
environment
trajectory
exploration
credit assignment
```

## 一句话定义

```text
Reinforcement Learning = learning to act through reward-driven interaction with an environment.
```

中文：

```text
强化学习 = 一个 agent 在环境中采取行动，根据 reward 学习更好的 policy。
```

最小闭环：

```text
state_t
  -> agent chooses action_t
  -> environment returns reward_t and state_{t+1}
  -> agent updates policy
```

这个闭环和监督学习很不一样。

监督学习是：

```text
input -> label -> loss
```

强化学习是：

```text
state -> action -> reward -> future state -> long-term return
```

核心难点是：

```text
当前 action 的好坏，可能要很久之后才知道。
```

## RL 的基本对象

基础概念先打牢。

```text
Agent
    做决策的主体。

Environment
    agent 交互的外部系统。

State
    当前环境信息。

Observation
    agent 实际能看到的信息，可能不等于完整 state。

Action
    agent 可以选择的行为。

Reward
    环境给出的反馈信号。

Policy
    state/observation -> action 的决策规则。

Trajectory
    一段 state, action, reward 序列。

Return
    从某个时间点开始的累计奖励。
```

最重要的是 policy：

```text
policy = agent 的行为规则。
```

RL 的目标就是找到一个好 policy。

## MDP: Markov Decision Process

很多 RL 问题会形式化成 MDP。

MDP 包含：

```text
S: state space
A: action space
P: transition dynamics
R: reward function
gamma: discount factor
```

直觉：

```text
agent 在 state S 里选择 action A，
环境根据 P 转移到下一个 state，
同时给出 reward R。
```

Markov 性质的意思是：

```text
未来只依赖当前 state 和 action，而不依赖更早的历史。
```

真实世界经常不完全满足 Markov 假设。
所以很多时候我们会用 observation history、memory、RNN、Transformer context 来补足状态。

这对 LLM agent 很重要：

```text
当前 prompt/context 就是 agent 对 state 的近似表示。
```

## Reward 和 Return

Reward 是即时反馈。
Return 是累计反馈。

常见 discounted return：

```text
G_t = r_t + gamma r_{t+1} + gamma^2 r_{t+2} + ...
```

其中 `gamma` 是 discount factor。

直觉：

```text
gamma 越接近 1，越重视长期回报。
gamma 越接近 0，越重视短期回报。
```

RL 的目标通常是最大化 expected return：

```text
maximize E[G_t]
```

这就是 RL 和普通分类任务的根本区别。
它关心一串行动带来的长期结果。

## Policy

Policy 有两种常见形式：

```text
Deterministic policy
    a = pi(s)

Stochastic policy
    a ~ pi(a | s)
```

在很多 RL 任务里，stochastic policy 很重要。
因为它天然包含探索。

例如：

```text
同一个 state 下，不总是选同一个 action。
而是按照概率分布采样 action。
```

LLM 本身也可以看成一种 stochastic policy：

```text
context -> next token distribution
```

如果把 tool call 也看成 action，那么 LLM agent 就可以被放进 RL 语言里：

```text
context/state -> action/tool call/message -> observation/reward
```

## Value Function

Value function 评估一个 state 好不好。

```text
V(s) = expected return starting from state s
```

直觉：

```text
如果我现在处在这个 state，未来大概能拿多少 reward？
```

Q function 评估一个 state-action pair 好不好：

```text
Q(s, a) = expected return after taking action a in state s
```

直觉：

```text
如果我在这个 state 采取这个 action，未来大概能拿多少 reward？
```

Value 和 Q 的区别：

```text
V 看 state。
Q 看 state + action。
```

很多 RL 算法本质上是在学习 V 或 Q。

## Exploration vs Exploitation

RL 的经典矛盾：

```text
Exploration
    尝试新 action，获取信息。

Exploitation
    利用当前已知最优 action，获取 reward。
```

如果只 exploit，可能陷入局部最优。
如果只 explore，收益不稳定。

量化里也有类似问题：

```text
继续使用已知有效策略
vs
探索新因子、新模型、新市场状态
```

Agent product 里也一样：

```text
严格按已有流程走
vs
尝试新的 tool-use plan
```

RL 的价值在于，它给我们一套讨论探索和利用的语言。

## Credit Assignment

Credit assignment 是 RL 的核心难点。

问题是：

```text
一段长 trajectory 最后成功了，到底是哪几个 action 贡献最大？
一段任务失败了，到底是哪一步造成了失败？
```

例如 coding agent：

```text
读需求
搜索代码
修改文件
运行测试
修复错误
提交总结
```

最后测试通过了。
这是不是每一步都好？
不一定。

如果最后失败了。
是不是最后一步错？
也不一定。

这就是 credit assignment。

RLHF、agent training、trajectory evaluation 都会遇到这个问题。

## Value-based Methods

Value-based 方法学习 value 或 Q function。

典型代表：

```text
Q-learning
DQN
```

Q-learning 的核心思想：

```text
学习 Q(s, a)，然后选择 Q 值最高的 action。
```

直觉更新：

```text
Q(s, a) <- current estimate + correction from reward and next state
```

DQN 则是用神经网络近似 Q function：

```text
Q_network(state) -> Q values for actions
```

适合离散 action space 的很多任务。

局限：

```text
连续 action 更复杂
policy 本身不是直接优化对象
训练稳定性需要很多技巧
```

## Policy Gradient

Policy gradient 直接优化 policy。

它不一定先学 Q table。
而是让 policy 产生 action，根据 return 调整 action 概率。

直觉：

```text
如果某个 action 带来高 return，就提高它在类似 state 下的概率。
如果某个 action 带来低 return，就降低它的概率。
```

适合：

```text
stochastic policy
large action space
continuous action
language model token generation
```

LLM 的输出是 token probability distribution。
这就是为什么 RLHF 这条线通常会和 policy optimization 接在一起。

## Actor-Critic

Actor-critic 把 policy 和 value 分开：

```text
Actor
    负责选择 action，也就是 policy。

Critic
    负责评价 state 或 action 的价值。
```

直觉：

```text
actor 做事，critic 打分。
```

这在 agent training 里非常自然：

```text
agent proposes action
evaluator judges quality
agent improves behavior
```

Actor-critic 的好处：

```text
policy 可以直接优化
critic 可以降低梯度估计方差
训练效率通常更好
```

## PPO

PPO 是很多 RLHF 讨论里会出现的算法。

PPO 的核心目标是：

```text
让 policy 变好，但每次更新不要离旧 policy 太远。
```

为什么？

因为 policy 更新太猛会导致训练崩掉。

PPO 用一种 clipped objective 限制更新幅度。
直觉是：

```text
improve policy conservatively
```

对 LLM 来说，PPO 还经常和 KL penalty 一起出现。
目的也是防止新模型偏离原模型太多。

面试里不一定要推公式。
但要讲得出：

```text
PPO is a stable policy optimization method that constrains policy updates,
which is why it has been used in RLHF-style training.
```

## Model-free 和 Model-based

另一种分类：

```text
Model-free RL
    不显式学习环境转移模型，直接学 policy 或 value。

Model-based RL
    学一个环境模型，然后用它规划或生成经验。
```

LLM agent 里有一个有意思的对应：

```text
world model / simulator / tool environment / planning model
```

如果 agent 能预测行动后果，它就有了某种 model-based planning 能力。

Research agent 里也类似：

```text
如果我读这篇 paper，可能得到什么信息？
如果我运行这个实验，可能验证什么假设？
如果我改这个模块，可能影响哪些测试？
```

这都是 model-based 思维。

## Offline RL

Offline RL 从历史数据学习策略，不直接在线探索。

这对金融很重要。
因为真实交易里不能随便探索。

金融场景常见限制：

```text
探索成本高
错误 action 可能亏钱
市场环境不可重放
数据分布会变化
历史数据有偏
```

所以 trading agent 不能简单照搬游戏 RL。
必须考虑：

```text
off-policy evaluation
backtest realism
transaction cost
risk constraint
distribution shift
capacity
```

这就是为什么 RL 在量化中很有吸引力，但落地难度很高。

## RL 和 LLM Agent

LLM agent 可以用 RL 语言描述：

```text
State / Observation
    prompt, memory, tool outputs, repo state, task state

Action
    text response, tool call, code edit, search query, plan update

Reward
    task success, test pass, user approval, evaluator score, cost penalty

Policy
    LLM behavior under current context

Trajectory
    multi-turn interaction and tool-use sequence
```

这也是为什么 agent harness 很关键。
没有 harness，就没有可靠的 environment 和 evaluation。

```text
harness defines the environment.
evaluator defines reward or score.
logs define trajectory.
training or prompting defines policy improvement.
```

## RL 和 Quant

Trading 可以自然写成 RL 问题：

```text
state
    market features, positions, risk exposure, signals

action
    buy, sell, hold, rebalance, adjust leverage

reward
    return, risk-adjusted return, PnL after cost

policy
    trading strategy
```

但金融 RL 很难：

```text
market is non-stationary
reward is noisy
actions affect future state through position and cost
backtest can overfit
online exploration is expensive
tail risk matters
```

所以我们要谨慎表达：

```text
RL gives a useful decision framework for quant,
but production trading requires strong data discipline, realistic simulation, risk constraints, and evaluation harness.
```

## RL 的学习路线

这条线建议这样学：

```text
1. MDP
2. policy / value / Q function
3. Bellman equation intuition
4. Q-learning / DQN
5. policy gradient
6. actor-critic
7. PPO
8. offline RL
9. RLHF / preference optimization
10. agent evaluation harness
```

不用一开始陷入公式。
先建立行为优化框架，再逐步补数学。

## 面试可用表达

如果被问“强化学习是什么”，可以这样说：

```text
Reinforcement learning studies how an agent learns a policy through interaction with an environment.
At each step, the agent observes a state, takes an action, receives a reward,
and transitions to the next state.
The objective is to maximize expected cumulative return, not just predict a static label.

The key concepts are policy, value function, Q function, exploration versus exploitation,
and credit assignment over trajectories.
Value-based methods learn how good state-action pairs are.
Policy gradient methods directly optimize the action distribution.
Actor-critic methods combine a policy actor with a value critic.
PPO is a stable policy optimization method that constrains updates.
```

再接 agent：

```text
For LLM agents, prompts and tool observations can be treated as states,
tool calls or messages as actions, and task success or evaluator scores as rewards.
This is why evaluation harnesses and trajectory logging are essential for agent training.
```

## 当前结论

`MLRL003` 的核心结论：

```text
RL = state -> action -> reward -> next state 的长期决策学习框架。
```

它给我们解释 agent、tool use、trading policy、RLHF、coding agent training 的共同语言。

接下来 `MLRL004` 就可以进入：

```text
RLHF / Agent Training
```

也就是把 RL 的语言接到 LLM 后训练和 agent 行为优化上。
