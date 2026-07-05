---
title: "LLMQuant x WorldQuant: 从因子投研流程到 AI Quant Research OS"
date: 2026-07-05 00:00:00 +0800
categories: [Learning, Quant Research]
tags: [llmquant, worldquant, quant-research, ai-quant, research-os, factor-research, agent-workflow, public-safe]
---

这篇是一个应用型 learning。

前面我已经做过 `LLMQUANT000-008` 的项目学习地图，也做过 FICC、HKUDS、Harness、ML/RL、FICC x AI Research OS 等系列。

现在的问题变得更具体：

```text
LLMQuant 里面有哪些东西，可以和我的 WorldQuant-style 投研流程产生共鸣？
```

这里的重点不是泄露任何具体 alpha。

这篇只讨论公开安全的研究流程：

```text
idea
-> factor hypothesis
-> expression / feature construction
-> backtest / simulation
-> diagnosis
-> next research plan
-> research memory
```

## 公开边界

先写边界。

这篇不包含：

```text
真实私密因子
WorldQuant 平台敏感表达式
未脱敏策略
实盘观点
投资建议
交易建议
```

这篇只讨论：

```text
research workflow
system architecture
public-safe factor research abstraction
AI-assisted quant research process
```

## WorldQuant-Style 投研流程

我现在理解的 WorldQuant-style factor research，不应该被狭义理解成“写一个 alpha 表达式”。

它更像一个完整投研闭环：

```text
market observation
-> factor idea
-> formula / feature expression
-> simulation
-> metric check
-> bias diagnosis
-> robustness check
-> iteration
-> factor library / research memory
```

如果拆成研究系统，它至少需要七层。

| Layer | 问题 | 典型输出 |
|---|---|---|
| Idea Layer | 这个因子假设从哪里来？ | hypothesis card |
| Data / Tool Layer | 需要什么数据、如何取数？ | data access / tool call |
| Expression Layer | 如何把想法变成可计算表达？ | factor formula / feature spec |
| Experiment Layer | 如何测试？ | backtest / simulation |
| Diagnosis Layer | 为什么好或不好？ | bias / risk / robustness note |
| Memory Layer | 如何沉淀？ | factor library / research card |
| Iteration Layer | 下一轮做什么？ | next research plan |

只会写一个 expression 不够。

真正有用的是：

```text
能持续地产生、测试、诊断、复用和迭代。
```

## LLMQuant 的共鸣点

LLMQuant 对我最有启发的地方，是它天然不是一个单点项目，而是一组 finance-native research stack。

它里面和 WorldQuant-style 投研流程最共鸣的模块是：

```text
QuantMind
data-mcp
skills
Magents
awesome-trading-agents
llmquantpengyistrategy
pengyi_quant_rd_agent
pengyi_quant_research_os_v0
```

这些项目可以被映射到因子投研链路里。

## 共鸣矩阵

| WorldQuant 投研环节 | LLMQuant 对应模块 | 系统价值 |
|---|---|---|
| 因子想法来源 | `QuantMind` | 从 paper / news / report / blog 中抽取结构化金融知识，作为 hypothesis source |
| 数据与证据访问 | `data-mcp` | 把数据、文档、市场信息变成 agent 可调用工具 |
| 研究任务路由 | `skills` | 把 macro、events、risk、portfolio、strategies 等任务变成 workflow skill |
| 表达式 / 策略实验室 | `llmquantpengyistrategy` | 承接 WorldQuant-style factor research scaffold |
| 自动 R&D loop | `pengyi_quant_rd_agent` | 自动提出假设、实现、回测、诊断、生成下一轮计划 |
| Research OS 母体 | `pengyi_quant_research_os_v0` | 把 paper-to-factor-to-backtest 组织成完整研究系统 |
| 回测 / 仿真 / agent | `Magents` | 提供 strategy simulation、agent execution、risk / portfolio organization 的参考 |
| 生态雷达 | `awesome-trading-agents` | 看外部 trading agent / MCP / skill / paper 怎么设计 |

一句话：

```text
WorldQuant 给我 factor research 的训练场。
LLMQuant 给我 AI-native finance research stack 的系统图。
```

## QuantMind：因子假设的知识入口

`QuantMind` 最像 Research OS 的知识层。

它的关键意义不是直接生成可交易策略，而是把非结构化输入变成结构化知识对象：

```text
paper
news
report
blog
filing
market note
```

转成：

```text
entity
event
relationship
claim
mechanism
risk
hypothesis candidate
```

对 WorldQuant-style 投研来说，它可以回答：

```text
这个因子想法从哪里来？
它背后的经济机制是什么？
它和哪些变量、事件、行业、资产有关？
它可能在哪些 regime 失效？
```

所以 QuantMind 对应的是：

```text
factor hypothesis memory
+ research evidence graph
+ paper-to-factor grounding
```

## data-mcp：投研工具入口

`data-mcp` 的共鸣点在于工具化。

WorldQuant-style research 不只是想法，还需要不断调用数据和证据：

```text
market data
company data
macro data
paper data
filing data
factor metadata
experiment artifacts
```

如果这些工具可以被 agent 调用，投研流程就可以从手工检索变成半自动 research workflow。

它对应的是：

```text
EvidenceAccess
+ ToolUse
+ DataInterface
```

这里最重要的启发是：

```text
agent 不是凭空研究。
agent 必须接工具、接数据、接证据。
```

## skills：金融工作流模板

`skills` 像一组 finance workflow router。

它覆盖：

```text
macro
events
equities
options
portfolio
risk
strategies
rates / fx
crypto
commodities
```

对 WorldQuant-style research 来说，skills 可以变成不同类型因子的研究模板。

例如：

```text
event skill -> event-driven factor hypothesis
macro skill -> macro-to-factor workflow
risk skill -> factor risk diagnosis
portfolio skill -> factor combination / weighting view
strategy skill -> expression-to-strategy framing
```

这个模块的价值是把抽象的金融经验固化成可调用流程。

## llmquantpengyistrategy：WorldQuant-Style Factor Lab

本地 `llmquantpengyistrategy` 里面的 `01_worldquant_factor_research` 是最直接对应 WorldQuant-style 投研流程的地方。

它应该承担：

```text
factor idea registry
factor card
public-safe example
simulation protocol
diagnosis template
next experiment plan
```

在我的系统里，它不应该只是一个文件夹。

它应该成为：

```text
WorldQuant-style factor research lab
```

作用是把 factor research 做成可复用、可追踪、可审查的过程。

## pengyi_quant_rd_agent：自动 R&D 闭环

`pengyi_quant_rd_agent` 是最贴近我当前目标的模块。

它对应我们一直说的：

```text
自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ 人类 PM 审核
```

这个结构非常适合 WorldQuant-style research。

因为因子研究天然是循环型任务：

```text
idea
-> implementation
-> simulation
-> diagnosis
-> modification
-> re-test
-> archive
```

如果没有 R&D Agent，这个循环依赖人的状态和记忆。

如果有 R&D Agent，它可以变成一个可追踪的实验生产线。

## Magents：策略执行和仿真层

`Magents` 的价值在于提醒我：

```text
只生成 idea 不够。
必须进入 simulation / backtest / risk / portfolio / accounting。
```

WorldQuant-style research 最容易犯的错误是只盯着因子表达式。

但是一个真正的研究系统还要关心：

```text
rebalance
turnover
cost
risk exposure
portfolio construction
drawdown
robustness
regime sensitivity
```

所以 Magents 在系统里对应：

```text
ExperimentRuntime
+ SimulationLayer
+ StrategyExecutionReference
```

## 组合成 AI Quant Research OS

最终我想要的系统不是单个 repo。

它是一个 AI Quant Research OS。

可以这样表达：

```text
QuantMind
  -> factor hypothesis memory

data-mcp
  -> data and evidence access

skills
  -> finance workflow router

llmquantpengyistrategy
  -> WorldQuant-style factor lab

pengyi_quant_rd_agent
  -> R&D loop controller

Magents
  -> simulation and execution reference

Deliverable Log / Credit OS
  -> public-safe artifact and interview proof
```

完整 loop：

```text
public-safe input
  -> research evidence extraction
  -> factor hypothesis card
  -> expression / feature spec
  -> simulation protocol
  -> backtest / diagnostic artifact
  -> bias and robustness note
  -> next research plan
  -> factor memory / project story
```

## 为什么这对我重要

这条线连接了三个东西：

```text
WorldQuant training
+ LLMQuant ecosystem
+ Pengyi AI Quant Research OS
```

WorldQuant 给我真实因子研究训练。

LLMQuant 给我 AI-native finance workflow 的系统参照。

Pengyi Research OS 要把二者合成：

```text
一个可审计、可复用、可展示、可自进化的投研系统。
```

## 简历和面试表达

可以这样讲：

```text
I studied the LLMQuant ecosystem and mapped it to my WorldQuant-style factor research workflow.
The key insight is that AI for quant research should not stop at generating alpha ideas.
It needs a full research loop: evidence grounding, factor hypothesis, implementation, backtest, bias diagnosis, next research plan, memory, and human review.
```

中文表达：

```text
我把 LLMQuant 看成 finance-native AI research stack，把 WorldQuant 看成 factor research 训练场。
二者结合后，可以形成一个 AI Quant Research OS：从公开安全的研究材料出发，生成因子假设，组织实验，做回测与偏差诊断，再沉淀为研究记忆和下一轮计划。
```

## 下一步

接下来不再只做总结。

要进入 delivery：

```text
1. 选一个 public-safe factor hypothesis 示例。
2. 写成 FactorHypothesisCard。
3. 接一个 toy data / public data / synthetic data test。
4. 输出 backtest protocol，不暴露私密 alpha。
5. 写 diagnosis note。
6. 形成一份可展示的 AI Quant Research OS mini demo。
```

最终目标：

```text
不是展示一个神秘 alpha。
而是展示我能搭建一个严谨的、可控的、可迭代的 AI-assisted quant research workflow。
```

