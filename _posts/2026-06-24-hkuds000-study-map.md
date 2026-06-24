---
title: "HKUDS000: PENGYI_HKUDS_STUDYMAP"
date: 2026-06-24 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds000, hkuds, lightrag, vibe-trading, nanobot, agents, rag, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第一篇。

```text
HKUDS000 -> Study Map
```

这一篇不是单个项目详解，而是先给 HKUDS 项目宇宙建立第一张地图。

本地 HKUDS 工作区已经找到了：

```text
E:\2026\B面\香港大学\PENGYI笔记\PENGYI superCODEX PROJECT笔记！\hkuds
```

这个工作区里已经集中保存了 HKUDS / 黄超老师 Lab 的公开 GitHub 项目本地副本。

当前本地索引显示：

| Item | Count / Status |
|---|---:|
| HKUDS public repos indexed | 87 |
| standard shallow git clones | 84 |
| Windows repaired clone | 1 |
| Windows sparse clone | 1 |
| Windows source snapshot | 1 |

所以 HKUDS 不是一个小项目，而是一个完整的 AI research ecosystem。

## Why HKUDS

前面我们已经完成了 LLMQuant 第一阶段：

```text
LLMQuant = AI-native finance research stack
```

HKUDS 的性质不一样。

我现在会这样区分：

```text
LLMQuant = finance-domain research and workflow system
HKUDS    = AI research infrastructure and agent ecosystem
```

LLMQuant 更贴近金融任务：

```text
market data
finance workflows
quant knowledge
trading agents
backtest / strategy research
```

HKUDS 更像底层 AI 研究能力地图：

```text
agents
RAG
graph learning
recommendation
spatio-temporal intelligence
multimodal / video
AI research automation
LLM efficiency and reasoning
```

这对我们很重要。

因为我们要做的不是单纯量化项目，而是：

```text
AI Scientist
  + Quant Research OS
  + R&D Agent
  + personal agent workspace
  + source-grounded knowledge system
  + public research output
```

HKUDS 正好给了 AI research infrastructure 的训练场。

## Current focus

HKUDS 项目很多，不能一上来全拆。

第一阶段先聚焦三个：

```text
LightRAG
Vibe-Trading
nanobot
```

原因很直接：

| Repo | First role for us |
|---|---|
| `LightRAG` | knowledge / RAG / project memory infrastructure |
| `Vibe-Trading` | trading agent and quant research workspace |
| `nanobot` | lightweight personal agent shell |

这三个刚好对应我们自己的三条主线：

```text
Knowledge layer
Trading research layer
Personal agent layer
```

如果把它们和 LLMQuant 接起来，可以得到一个很清楚的系统方向：

```text
LightRAG        -> Research memory / source-grounded retrieval
Vibe-Trading    -> Trading research / backtest / agentic quant workflow
nanobot         -> Personal always-on agent shell
LLMQuant stack  -> finance data / skills / quant knowledge / ecosystem map
```

## Local snapshot

我本地看到的三个重点 repo 规模如下：

| Repo | Files | Markdown | Python | TypeScript | JSON |
|---|---:|---:|---:|---:|---:|
| `LightRAG` | 683 | 48 | 406 | 100 | 23 |
| `Vibe-Trading` | 1547 | 374 | 992 | 75 | 7 |
| `nanobot` | 642 | 62 | 415 | 118 | 16 |

`nanobot` 当前有本地未提交改动：

```text
M nanobot/agent/tools/long_task.py
M nanobot/providers/openai_compat_provider.py
```

所以后续读 `nanobot` 时要注意：只读不覆盖，不随便 pull，不改动这些未提交文件。

## HKUDS macro map

本地 `REPO_MEANINGS.md` 把 HKUDS 大致分成六条主线。

我会先采用这个分法：

| Line | Meaning | Representative repos |
|---|---|---|
| Agent and automation systems | personal agents, coding agents, coworker agents, research automation | `nanobot`, `AutoAgent`, `OpenHarness`, `ClawTeam`, `ClawWork`, `DeepCode`, `AI-Researcher` |
| RAG and knowledge systems | retrieval, KG, multimodal RAG, video RAG | `LightRAG`, `MiniRAG`, `RAG-Anything`, `VideoRAG` |
| Graph / LLM4Graph | graph foundation models and graph language models | `OpenGraph`, `GraphGPT`, `HiGPT`, `GraphAgent`, `AnyGraph` |
| Recommendation systems | SSL, GNN, diffusion, LLM recommendation, explainable recommendation | `SSLRec`, `LightGCL`, `RecLM`, `RecGPT`, `XRec`, `LLMRec` |
| Spatio-temporal / urban computing | traffic prediction, urban intelligence, ST graph learning | `UrbanGPT`, `OpenCity`, `GPT-ST`, `FlashST`, `EasyST` |
| LLM efficiency / reasoning / multimodal | reasoning transfer, compression, video agent, AI phone | `LightReasoner`, `SepLLM`, `VideoAgent`, `OpenPhone`, `ViMax` |

这个宏观地图告诉我们：HKUDS 不是只有 agent。

它的底盘其实是：

```text
agent + RAG + graph + recommendation + urban intelligence + multimodal
```

这对顶会研究非常有价值。

因为它不只是“产品 demo”，它背后有一组持续发表、持续开源、持续工程化的研究方向。

## LightRAG

`LightRAG` 的一句话定位：

```text
LightRAG = simple and fast graph-based RAG infrastructure
```

README 里讲得很明确：LightRAG 是一个 lightweight knowledge-graph RAG framework，也是 Microsoft GraphRAG 的高效替代方向。

它的核心不是普通向量检索，而是：

```text
documents
  -> chunks
  -> entities
  -> relationships
  -> knowledge graph
  -> vector embeddings
  -> local / global / hybrid retrieval
  -> grounded generation
```

本地顶层目录显示它已经是一个完整工程：

```text
lightrag/
lightrag_webui/
docs/
examples/
k8s-deploy/
prompts/
tests/
```

重点特性包括：

```text
API server
WebUI
KG / vector / storage backend
role-specific LLM configuration
multimodal document processing
reranker
citations
document deletion and KG regeneration
PostgreSQL / Neo4j / MongoDB / OpenSearch and other storage options
Docker / k8s deployment
```

对我们来说，LightRAG 应该先放在：

```text
Pengyi Research OS / Knowledge Memory Layer
```

它可以支撑：

```text
paper memory
project memory
CV / PS / RP material retrieval
quant research notes retrieval
source-grounded answer generation
website learning archive search
```

后续 `HKUDS001` 可以专门拆：

```text
LightRAG indexing pipeline
LightRAG query pipeline
storage abstraction
API server
how to connect it to Pengyi Research OS
```

## Vibe-Trading

`Vibe-Trading` 的一句话定位：

```text
Vibe-Trading = personal trading agent workspace
```

README 的定位是：

```text
Your Personal Trading Agent
One Command to Empower Your Agent with Comprehensive Trading Capabilities
```

它对我们非常直接。

因为它不是抽象 agent framework，而是把交易研究工作流做成了可运行系统：

```text
natural-language market research
market-data loaders
strategy generation
backtesting
reports
exports
persistent research memory
multi-agent research teams
broker / data connectors
MCP tools
Web UI
```

本地顶层目录：

```text
agent/
frontend/
wiki/
tools/
scripts/
```

README 里最值得我们关注的能力包括：

```text
self-improving trading agent
multi-agent trading teams
cross-market data and backtesting
shadow account
research autopilot
alpha zoo
run cards
MCP / API
persistent memory
provider routing
security hardening
```

这个项目和我们的 Quant R&D Agent 极度相关。

它已经接近我们一直说的闭环：

```text
research question
  -> hypothesis
  -> signal engine
  -> backtest
  -> attribution
  -> report
  -> next research
```

后续 `HKUDS002` 应该重点拆：

```text
agent runtime
tool registry
market data loader
backtest engine
Research Autopilot
Alpha Zoo
Shadow Account
MCP/API boundary
```

它也可以和 LLMQuant 的 `data-mcp`、`skills`、`Magents` 做强对比。

## nanobot

`nanobot` 的一句话定位：

```text
nanobot = ultra-light personal AI agent shell
```

README 里说得很直白：

```text
open-source, ultra-lightweight personal AI agent you can truly own
```

它不是专门做交易，也不是专门做 RAG。

它更像一个个人 agent operating layer：

```text
CLI
WebUI
chat channels
tools
memory
MCP
model routing
automation
deployment
project workspaces
long-running goals
```

本地顶层目录：

```text
nanobot/
webui/
docs/
bridge/
case/
scripts/
tests/
```

对我们来说，nanobot 最重要的是“轻”。

我们不是一上来就需要一个巨大的 agent platform。

我们需要的是：

```text
能长期陪跑
能接工具
能记忆
能接 Telegram / WebUI / CLI
能处理长任务
能被自己读懂和改造
```

这正好对应我们的个人生产力系统：

```text
Pengyi Personal AI Operating Loop
```

后续 `HKUDS003` 应该重点拆：

```text
AgentRunner
tool call loop
memory system
MCP server/client
provider routing
channel integrations
goal mode
project workspace
```

## First integration idea

如果只看三个项目本身，它们已经很强。

但真正有价值的是把它们合起来：

```text
nanobot = personal agent shell
LightRAG = project memory and source-grounded retrieval
Vibe-Trading = quant research and trading workspace
LLMQuant = finance data, skills, knowledge, and ecosystem
```

组合后可以形成：

```text
Pengyi AI Scientist Workbench
  -> nanobot runs the personal agent loop
  -> LightRAG stores papers, notes, project docs, CV/PS/RP materials
  -> Vibe-Trading provides trading research and backtest workflows
  -> LLMQuant supplies finance-specific data, skills, and knowledge structures
```

这就是 HKUDS 系列对我们最直接的价值。

不是“多看几个 repo”。

而是把它们变成自己的系统组件。

## Reading order

第一阶段建议这样走：

| Post | Project | Focus |
|---|---|---|
| `HKUDS000` | study map | overall map and first three priorities |
| `HKUDS001` | `LightRAG` | RAG / KG / storage / API server / WebUI |
| `HKUDS002` | `Vibe-Trading` | trading agent / Research Autopilot / backtest / MCP |
| `HKUDS003` | `nanobot` | personal agent loop / tools / memory / channels |
| `HKUDS004` | comparison | LightRAG + Vibe-Trading + nanobot + LLMQuant integration |

这条路线足够聚焦。

HKUDS 有 87 个公开 repo，我们现在不需要把 87 个全拆。

先把这三个拆透，就已经能支撑我们做自己的：

```text
AI Scientist Workbench
Quant R&D Agent
Personal Research OS
```

## My takeaway

HKUDS 第一眼看上去很大。

但如果从我们的目标出发，它可以先压缩成三块：

```text
LightRAG     -> knowledge memory
Vibe-Trading -> quant research workflow
nanobot      -> personal agent shell
```

这三块加上 LLMQuant 的 finance stack，正好组成我们下一阶段要做的系统骨架。

所以 `HKUDS000` 的结论是：

```text
HKUDS is the AI infrastructure side of our Research OS.
LLMQuant is the finance-domain side.
Our next task is to connect them through our own Pengyi Quant R&D Agent.
```
