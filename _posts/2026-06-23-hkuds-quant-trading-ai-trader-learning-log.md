---
title: "HKUDS Quant and Trading Projects: AI Trader Learning Log"
date: 2026-06-23 03:00:00 +0800
categories: [Learning, Quant Research]
tags: [hkuds, ai-trader, vibe-trading, futureshow, quant-research, trading-agents, rag]
---

I recently reviewed the HKUDS project ecosystem from one practical question:

```text
Which HKUDS projects are directly useful for building an AI trader?
```

My current conclusion is that HKUDS already has a strong public project cluster around agent-native trading, market research, forecasting, and retrieval-based research infrastructure.

The important point is framing. I do not want to describe this direction as a fully autonomous money-making trading bot. A stronger and more credible framing is:

```text
auditable AI trading research system
  -> market question
  -> source-grounded research
  -> strategy hypothesis
  -> backtest or simulation
  -> risk diagnosis
  -> human approval
```

This is much closer to a serious research and engineering direction.

## Core Quant And Trading Projects

### 1. Vibe-Trading

[Vibe-Trading](https://github.com/HKUDS/Vibe-Trading) is the most directly relevant HKUDS project for my current AI trader direction.

It looks like a personal trading agent and finance research system. The repo combines:

- natural-language finance research;
- market data access;
- backtesting;
- broker connector boundaries;
- MCP/API tools;
- research goal workflows;
- multi-agent trading teams;
- memory and session management;
- strategy generation and diagnosis;
- an Alpha Zoo with hundreds of formulaic alpha factors.

For my own learning map, Vibe-Trading is not just "a trading app." It is a possible reference design for an AI-native quant research loop:

```text
research goal
  -> agent planning
  -> data retrieval
  -> strategy generation
  -> backtest
  -> diagnostics
  -> report
  -> human review
```

This connects directly to my previous quant research experience: alpha factors, strategy evaluation, overfitting control, robustness checks, and public-safe research reports.

The parts I want to study first are:

- the agent loop and how a user research goal becomes tool calls;
- the backtest interface and artifact structure;
- the factor library and alpha benchmark logic;
- the safety boundary between research, paper trading, and live trading;
- the multi-agent presets for quant research workflows.

### 2. AI-Trader

[AI-Trader](https://github.com/HKUDS/AI-Trader) is more platform-oriented.

It is positioned around an agent-native trading marketplace where agents can publish strategies, operations, discussions, and trading signals. It also includes copy-trading style APIs, signal feeds, Polymarket-related workflows, and public API specifications.

For me, AI-Trader is useful because it shows what the product layer of an AI trader ecosystem may look like:

```text
agent identity
  -> signal publication
  -> market discussion
  -> follower / copy-trading workflow
  -> performance tracking
  -> API surface
```

This is different from Vibe-Trading.

```text
Vibe-Trading = research and strategy engine
AI-Trader    = platform, signal, and marketplace layer
```

I should be careful with this project from a research narrative perspective. The useful public framing is not "agents trade for humans automatically." The stronger framing is:

```text
agent-generated market signals with tracking, transparency, and evaluation
```

That makes it relevant to agent evaluation, market simulation, social signal systems, and human-supervised decision support.

### 3. FutureShow

[FutureShow](https://github.com/HKUDS/FutureShow) is a forecasting and prediction-market project.

It uses real-world event questions and market-style feedback to evaluate model predictions. The repo includes Polymarket-oriented tools, forecasting prompts, tracking scripts, web views, and evaluation logic.

This matters for AI trading because trading is not only about order execution. A serious AI trader needs:

- event understanding;
- probability estimation;
- market consensus comparison;
- evidence-based forecasting;
- calibration;
- decision logs;
- post-event evaluation.

FutureShow can become the forecasting and evaluation layer:

```text
event or macro question
  -> market and news context
  -> forecast
  -> probability / confidence
  -> benchmark against market price
  -> evaluate after resolution
```

For my own FICC direction, this is especially interesting. Rates, FX, commodities, inflation, central bank events, geopolitical events, and macro data releases all require probabilistic thinking before they become trade ideas.

## Supporting Infrastructure

The direct trading projects are not enough by themselves. A reliable AI trader also needs memory, retrieval, document parsing, and agent orchestration.

The HKUDS support layer I care about is:

| Project | Role in AI Trader OS |
|---|---|
| [LightRAG](https://github.com/HKUDS/LightRAG) | Fast retrieval and knowledge memory for research notes, reports, papers, and market documents |
| [RAG-Anything](https://github.com/HKUDS/RAG-Anything) | Document and multimodal RAG layer for PDFs, charts, tables, and complex research material |
| [nanobot](https://github.com/HKUDS/nanobot) | Lightweight personal agent framework for tools, memory, workflows, and provider routing |
| AI-Researcher | Research automation reference for paper-to-idea and experiment planning |
| DeepResearch-Eval | Evaluation reference for long-form research agents |
| AutoAgent / OpenHarness / OpenSpace | Agent runtime and orchestration references |

The system view becomes:

```text
Vibe-Trading     -> quant research and strategy loop
AI-Trader        -> signal and platform layer
FutureShow       -> forecasting and evaluation layer
LightRAG         -> memory and source grounding
RAG-Anything     -> complex document parsing
nanobot          -> lightweight personal agent shell
```

## Connection To My Own Research OS

This HKUDS map fits my current research direction very well.

My personal stack can be organized as:

```text
HKUDS      -> AI / agent / RAG / trading infrastructure
LLMQuant   -> finance-domain knowledge, data, and skills
X2Strategy -> paper-to-strategy and backtest workflow
FI-C-C OS  -> fixed income, currency, and commodity pilot
PM2.0      -> memory, project management, and human review
```

The most natural pilot is not a live trading system.

The most natural pilot is:

```text
FICC AI Trader Research OS
```

It should start with fixed income, currency, and commodity research tasks:

- central bank event tracking;
- rates and yield-curve narratives;
- FX macro drivers;
- commodity supply-demand stories;
- market regime classification;
- paper-to-strategy extraction;
- backtestable hypotheses;
- human approval before any trading action.

This gives the project a real domain while keeping the engineering scope controlled.

## Learning Plan

My near-term study plan is:

1. Read Vibe-Trading architecture and identify the research/backtest path.
2. Map the Alpha Zoo and strategy-generation skills to my previous WorldQuant-style alpha experience.
3. Study AI-Trader's signal and API design as a product-layer reference.
4. Study FutureShow as the forecasting and model-evaluation layer.
5. Connect LightRAG or RAG-Anything to finance documents, papers, and market notes.
6. Build a small public-safe FICC example:

```text
one macro or FICC question
  -> retrieve evidence
  -> write thesis
  -> generate strategy hypothesis
  -> define backtest plan
  -> diagnose risk and failure modes
  -> publish a sanitized learning note
```

## What I Want To Avoid

There are several failure modes I want to avoid:

- collecting too many repos without building anything;
- treating LLM output as a trading signal without verification;
- confusing backtest success with real market robustness;
- ignoring transaction costs, liquidity, regime shifts, and data leakage;
- overclaiming full automation before the research loop is auditable.

The safer principle is:

```text
automation drafts
human approves
experiments verify
memory records
public notes summarize
```

## Current Conclusion

HKUDS already gives me a strong base for AI trader research.

The direct quant/trading cluster is:

```text
Vibe-Trading + AI-Trader + FutureShow
```

The infrastructure cluster is:

```text
LightRAG + RAG-Anything + nanobot + AI-Researcher + DeepResearch-Eval
```

For my own path, the right direction is not a black-box trading robot. It is an auditable AI trading research OS:

```text
research grounding
  + strategy generation
  + backtest validation
  + risk diagnosis
  + human review
  -> reusable finance research artifacts
```

That is a credible bridge between AI agents, quantitative finance, research automation, and real financial decision workflows.
