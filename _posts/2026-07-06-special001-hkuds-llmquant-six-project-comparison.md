---
title: "专题001: HKUDS x LLMQuant 六项目对比 - LightRAG / Vibe-Trading / AI-Trader / NanoBot / Skills / Data MCP"
date: 2026-07-06 00:00:00 +0800
categories: [Learning, AI Research OS]
tags: [learning-special001, hkuds, llmquant, lightrag, vibe-trading, ai-trader, nanobot, skills, data-mcp, active-mock, research-os, quant-os]
---

这是新的 `Learning Special` 系列第一篇。

目的不是再写一篇单项目学习笔记，而是把前面已经研究过的六个关键项目放到同一张系统图里：

```text
HKUDS LightRAG
HKUDS Vibe-Trading
HKUDS AI-Trader
HKUDS NanoBot
LLMQuant Skills
LLMQuant Data MCP
```

这六个项目对我现在的意义很直接：

```text
它们共同构成一个 AI Quant / Research OS 的六个关键层。
```

## 一句话总图

如果只用一句话概括：

```text
Data MCP 负责证据入口；
LightRAG 负责知识记忆；
Skills 负责金融任务路由；
NanoBot 负责 agent runtime；
Vibe-Trading 负责 quant research workflow；
AI-Trader 负责 trading platform / paper trading / agent society。
```

放成链路就是：

```text
external evidence
  -> data tool access
  -> knowledge memory
  -> workflow routing
  -> agent execution
  -> quant research loop
  -> trading / evaluation platform
```

这就是我现在想要的：

```text
Pengyi AI Quant Research OS v0
```

## 六个项目的定位

| Project | 我的一句话定位 | 对 Research OS 的位置 |
|---|---|---|
| HKUDS LightRAG | Knowledge / Graph RAG Runtime | 把论文、报告、新闻、笔记变成可检索、可关联、可复用的知识层 |
| HKUDS Vibe-Trading | Agentic Quant Research Workflow | 把 idea、hypothesis、data、backtest、diagnosis 变成研究工作流 |
| HKUDS AI-Trader | Agent-Native Trading Platform | 把 agent 行为、paper trading、challenge、copy trading、研究导出平台化 |
| HKUDS NanoBot | Personal Agent Shell / Agent Runtime | 把 LLM、tool、channel、provider、memory、workspace 接成常驻 agent |
| LLMQuant Skills | Finance Workflow Routing Layer | 把金融任务按 category / workflow / data contract / guardrail 路由 |
| LLMQuant Data MCP | Evidence Access Layer | 把 SEC、13F、macro、ETF、paper、wiki、market data 等变成 agent 可调用工具 |

我的当前判断：

```text
HKUDS 更像 AI infrastructure / agent product / RAG / platform stack。
LLMQuant 更像 finance-native workflow / data access / quant research stack。
```

所以它们不是竞争关系。

它们是互补关系。

## 第一层：Data MCP - 证据入口

LLMQuant Data MCP 的核心价值是：

```text
让 agent 不再凭记忆回答金融问题，而是先拿证据。
```

它在系统里解决的是：

```text
Where does evidence come from?
```

它对应的能力包括：

- filing / disclosure；
- 13F；
- macro snapshot；
- ETF / holding；
- market context；
- paper / wiki context；
- data freshness；
- tool-callable interface。

对我来说，Data MCP 的启发不是某个单独 API。

而是：

```text
所有金融研究任务都必须先定义 evidence contract。
```

也就是：

```text
Required data
Optional data
Freshness
Missing input
Fallback
Data used
```

没有这个层，后面的 RAG、agent、backtest 都会漂。

## 第二层：LightRAG - 知识记忆

LightRAG 的核心价值是：

```text
把 loose files 变成 graph-aware research memory。
```

普通 RAG 通常是：

```text
document -> chunk -> embedding -> vector retrieval
```

LightRAG 进一步强调：

```text
document
  -> chunk
  -> entity / relation extraction
  -> vector store
  -> graph store
  -> local / global / hybrid retrieval
```

这对 Research OS 非常关键。

因为研究不是只问一个文档。

研究经常需要跨：

- paper；
- news；
- company；
- factor；
- market event；
- method；
- author；
- experiment；
- failure case；
- follow-up plan。

所以 LightRAG 对我的启发是：

```text
研究记忆必须有关系结构。
```

在 AI Quant 场景里，它可以承载：

```text
paper -> factor idea
company -> event -> risk
macro variable -> asset class
strategy -> assumption -> backtest result
failure -> diagnosis -> next experiment
```

## 第三层：Skills - 金融工作流路由

LLMQuant Skills 的核心价值是：

```text
让 agent 进入正确金融工作流。
```

它不是单个 prompt。

它更像：

```text
category router
  -> workflow selection
  -> data contract
  -> output format
  -> guardrails
```

例如：

- equity research memo；
- 10-K risk review；
- IV rank；
- global macro dashboard；
- research health check；
- quant strategy workflow。

这个项目给我的核心启发是：

```text
好的 agent 不是更会聊天，而是更会进入正确 workflow。
```

如果 Data MCP 解决：

```text
数据从哪里来？
```

LightRAG 解决：

```text
知识如何记住和关联？
```

那么 Skills 解决：

```text
这个任务应该怎么做？
```

这就是 finance agent 的纪律。

## 第四层：NanoBot - Agent Runtime

NanoBot 的核心价值是：

```text
把 agent 做成常驻 runtime，而不是一次性 prompt。
```

它给我的启发是：

```text
agent core 要小，能力应该放到 adapters / tools / providers / channels / memory / workspace。
```

我把它理解成：

```text
User / Channel
  -> Agent Loop
  -> Context / Memory
  -> Tool Registry
  -> Provider
  -> Workspace
  -> Output / Action
```

这对我非常重要。

因为我不想把 LightRAG、Data MCP、Skills、Vibe-Trading 全部塞进一个大脚本。

更合理的方式是：

```text
NanoBot-style agent runtime
  + LLMQuant-style finance workflows
  + LightRAG-style knowledge memory
  + Vibe-Trading-style quant research tools
```

也就是说，NanoBot 是 shell。

其他项目是 capability。

## 第五层：Vibe-Trading - Quant Research Workflow

Vibe-Trading 的核心价值是：

```text
把 quant research 做成 agentic workflow。
```

我对它的定位是：

```text
idea -> hypothesis -> data -> backtest -> diagnosis -> next plan
```

它最吸引我的地方不是“能交易”。

而是它强调：

- hypothesis registry；
- research autopilot；
- data layer；
- backtest layer；
- goal and evidence ledger；
- alpha zoo；
- swarm team；
- live trading boundary。

这和我的 WorldQuant-style 投研流程高度共鸣。

我希望把自己的量化研究流程也拆成：

```text
factor idea
  -> factor hypothesis
  -> expression / signal definition
  -> data contract
  -> backtest protocol
  -> bias diagnosis
  -> PM review
  -> next research plan
```

Vibe-Trading 给我的启发是：

```text
quant research 不应该只是 notebook。
它应该是一个可保存、可复查、可继续的 workflow。
```

## 第六层：AI-Trader - Trading Platform

AI-Trader 的核心价值是：

```text
把 agent trading 从单个策略脚本推进到 platform。
```

它更像：

```text
agent-native trading platform
```

我关注它的几个点：

- agent onboarding；
- identity / registration；
- signal submission；
- paper trading；
- heartbeat；
- challenge system；
- experiment system；
- research export；
- copy trading；
- network edges；
- signal quality；
- market intelligence；
- platform UI。

对我来说，AI-Trader 提醒我：

```text
如果未来做 AI Quant Research OS，最终不能只停留在文件夹。
它要有平台层：用户、实验、信号、评价、复盘、导出、协作和审计。
```

但边界也很重要。

我现在不应该一上来做实盘。

更合理的是：

```text
paper trading
research ledger
human PM review
public-safe artifact
```

## 六项目横向对比

| Dimension | Data MCP | LightRAG | Skills | NanoBot | Vibe-Trading | AI-Trader |
|---|---|---|---|---|---|---|
| 核心问题 | 证据怎么拿 | 知识怎么记 | 任务怎么做 | agent 怎么跑 | research 怎么循环 | trading 怎么平台化 |
| 输入 | 外部数据源 | 文档 / 知识 | 用户任务 | 用户 / channel / tool | idea / data / strategy | agent / signal / market |
| 输出 | callable evidence | retrieval context | workflow result | agent action | backtest / diagnosis | paper trade / platform record |
| 最重要边界 | freshness / source | graph / retrieval quality | data contract / guardrail | tool / workspace safety | bias / backtest validity | live trading / risk boundary |
| 对我最有用 | data contract | research memory | finance router | agent shell | quant workflow | platform and evaluation |
| 面试表达 | evidence layer | GraphRAG layer | workflow layer | runtime layer | research loop layer | product layer |

这张表可以压成：

```text
Data MCP = evidence
LightRAG = memory
Skills = workflow
NanoBot = runtime
Vibe-Trading = research loop
AI-Trader = platform
```

## 组合成 Pengyi AI Quant Research OS

如果把六个项目合成一个系统，我会这样设计：

```text
1. Evidence Access Layer
   - LLMQuant Data MCP style
   - source / timestamp / freshness / missing input

2. Knowledge Memory Layer
   - LightRAG style
   - document store + vector store + graph store

3. Workflow Routing Layer
   - LLMQuant Skills style
   - category router + workflow contract + guardrails

4. Agent Runtime Layer
   - NanoBot style
   - channel + agent loop + tools + provider + workspace

5. Quant Research Layer
   - Vibe-Trading style
   - hypothesis + data + backtest + diagnosis + next plan

6. Trading / Evaluation Platform Layer
   - AI-Trader style
   - paper trading + challenge + experiment + export + human review
```

最小闭环：

```text
public paper / report / market note
  -> Data MCP style evidence fetch
  -> LightRAG knowledge memory
  -> Skills workflow router
  -> NanoBot agent runtime
  -> Vibe-Trading research loop
  -> AI-Trader-style paper trading / evaluation record
  -> public-safe report
```

## 主动 Mock 视角

这篇专题也服务 `Mock OS`。

我现在不只是“学习项目”，而是要主动 mock 这些项目，逼自己回答：

```text
这个项目解决什么问题？
核心架构是什么？
输入输出是什么？
它和其他项目边界在哪里？
我能复用什么？
我不能照搬什么？
如果面试官拷打，我怎么讲？
如果要提 issue / PR，我能提什么？
```

六个项目对应的 mock 问题：

| Project | 主动 Mock 问题 |
|---|---|
| LightRAG | 为什么 GraphRAG 比普通 vector RAG 更适合 research memory？ |
| Vibe-Trading | 一个 quant research workflow 如何避免变成 notebook 乱跑？ |
| AI-Trader | trading agent 为什么需要 platform，而不只是 strategy script？ |
| NanoBot | 为什么 agent core 要小？tool / provider / channel / memory 怎么分层？ |
| Skills | 为什么 finance agent 需要 router + workflow contract？ |
| Data MCP | 为什么金融 agent 必须记录 data used / freshness / missing input？ |

如果这些问题能讲清楚，它们就能进入我的面试素材。

## 面试表达版本

如果面试官问：

```text
你最近系统学习了哪些 AI / Quant project？
```

我可以这样回答：

```text
我最近把 HKUDS 和 LLMQuant 的几个项目放在一起研究。

我的理解是，LLMQuant Data MCP 对应 evidence access，LLMQuant Skills 对应 finance workflow routing，HKUDS LightRAG 对应 knowledge / graph RAG memory，NanoBot 对应 agent runtime，Vibe-Trading 对应 agentic quant research loop，AI-Trader 对应 agent-native trading platform。

这六个项目合起来给了我一个 AI Quant Research OS 的参考架构：先拿证据，再建知识记忆，再路由到金融工作流，再用 agent runtime 执行，再进入 backtest / diagnosis / paper trading / evaluation。这个思路也能连接我的 WorldQuant-style factor research workflow。
```

这段话比“我看了很多开源项目”强很多。

因为它讲清楚了：

```text
project -> system layer -> personal research direction
```

## 不该做什么

这六个项目很强，但我不能误用。

需要注意：

- 不把 Data MCP 当成万能金融数据库；
- 不把 LightRAG 当成自动正确的知识图谱；
- 不把 Skills 当成投资建议生成器；
- 不把 NanoBot 当成自动替我完成所有任务的代理；
- 不把 Vibe-Trading 当成“自动赚钱机器”；
- 不把 AI-Trader 当成现在就该实盘的信号。

正确路线应该是：

```text
public-safe
research-first
evidence-grounded
human-reviewed
paper-trading-before-live
```

## 立即行动

这个专题之后，下一步不是继续抽象。

我应该做三个动作。

### 1. 做一张六项目架构图

图里要有：

```text
Data MCP -> LightRAG -> Skills -> NanoBot -> Vibe-Trading -> AI-Trader
```

并标注：

```text
evidence / memory / workflow / runtime / research / platform
```

### 2. 做一个 public-safe demo

最小 demo：

```text
输入一篇公开金融 research note
-> 抽取 evidence
-> 存入 knowledge memory
-> 路由到 paper-to-factor workflow
-> 生成 factor hypothesis card
-> 写 backtest protocol
-> 输出 public-safe report
```

不需要实盘。

不需要暴露 alpha。

只展示 workflow。

### 3. 做一轮主动 mock

对每个项目问：

```text
如果我是面试官，我会怎么质疑这个项目？
如果我是开源维护者，我会希望 contributor 提什么 issue？
如果我是 PM，我会怎么把它产品化？
如果我是 quant researcher，我会怎么判断它是否可信？
```

这就是 `Learning Special001` 的下一步。

## 当前结论

这六个项目共同给我的启发是：

```text
AI Quant Research OS 不是一个单点模型。
它是 evidence、memory、workflow、runtime、research loop、platform 的组合系统。
```

所以我的后续路线也应该按层推进：

```text
先做证据和知识。
再做 workflow 和 agent runtime。
再做 quant research loop。
最后再考虑 platform 和 trading evaluation。
```

一句话：

```text
LLMQuant 给金融工作流纪律。
HKUDS 给 AI 系统工程结构。
我需要把两者合成自己的 Research OS 和 Mock OS。
```

## Related Notes

- [HKUDS001: LightRAG 作为知识图谱 RAG 与 Research Memory 基建](/posts/hkuds001-lightrag-study/)
- [HKUDS002: Vibe-Trading 作为 Agentic Quant Research Workflow](/posts/hkuds002-vibe-trading-study/)
- [HKUDS003: nanobot 作为 Personal Agent Shell 与 Always-On Research Workspace](/posts/hkuds003-nanobot-study/)
- [HKUDS005: AI-Trader 作为 Agent-Native Live Trading Platform Layer](/posts/hkuds005-ai-trader-study/)
- [LLMQUANT001: data-mcp study](/posts/llmquant001-data-mcp-study/)
- [LLMQUANT002: skills 作为金融 workflow 路由层](/posts/llmquant002-skills-study/)
- [HKUDS052: HKUDS Quant 系列专题总结](/posts/hkuds052-quant-series-summary/)
- [LLMQuant x WorldQuant: 从因子投研流程到 AI Quant Research OS](/posts/llmquant-worldquant-research-loop/)

