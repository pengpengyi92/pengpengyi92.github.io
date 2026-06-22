---
title: "HKUDS, LLMQuant, and X2Strategy: Toward a Personal Research and Quant Production OS"
date: 2026-06-23 02:00:00 +0800
categories: [Learning, Research OS]
tags: [hkuds, llmquant, x2strategy, research-os, quant-research, agents]
---

My recent work is converging around three systems:

- **HKUDS ecosystem**: AI agents, RAG, graph learning, recommender systems, research automation, and open-source research infrastructure.
- **LLMQuant**: a finance and quant-oriented project ecosystem with domain skills, market intelligence, strategy notes, and agent workflows.
- **X2Strategy**: a paper-to-strategy pipeline that turns research inputs into strategy specifications, code, backtests, and diagnosis reports.

I do not see these as separate learning tracks. I see them as three layers of a future personal research and quant production OS.

```text
HKUDS      -> AI / agent / RAG / research infrastructure
LLMQuant   -> quantitative finance domain system
X2Strategy -> paper-to-strategy execution loop
```

## Why These Three Fit Together

HKUDS is valuable because it shows how modern AI research systems are built: retrieval, agents, graph learning, recommender models, research automation, and evaluation.

LLMQuant is valuable because it brings the finance domain into the system: assets, markets, signals, events, risk, portfolio thinking, and analyst workflows.

X2Strategy is valuable because it focuses on execution. It tries to convert research material into a structured strategy spec, then into code, backtest artifacts, and diagnosis reports.

The fusion is natural:

```text
research input
  -> AI research infrastructure
  -> finance domain framing
  -> executable strategy specification
  -> experiment / backtest
  -> diagnosis
  -> human review
  -> reusable research artifact
```

## My Integration Thesis

A useful AI research system should not stop at summarization.

It should help produce artifacts that can be reviewed, tested, improved, and reused:

- structured paper notes;
- knowledge cards;
- strategy specifications;
- experiment configs;
- baseline and ablation plans;
- backtest reports;
- failure analysis;
- draftable technical notes.

This is the difference between a chatbot and a research operating system.

```text
chatbot = answer generation
research OS = artifact generation + validation loop + memory update
```

## A Target Workflow

The workflow I want to build toward looks like this:

```text
paper / market idea / research capture
  -> personal capture system
  -> HKUDS-style retrieval or agent analysis
  -> LLMQuant-style domain framing
  -> X2Strategy-style strategy specification
  -> implementation or experiment plan
  -> results and diagnosis
  -> human approval
  -> public-safe research artifact
```

This workflow keeps automation useful but bounded. The system can generate drafts, specs, experiments, and reports. Human review remains responsible for novelty, validity, risk, authorship, and final decisions.

## Three Near-Term Research Lines

### 1. Research Automation

```text
HKUDS AI-Researcher / DeepResearch-Eval
  + personal paper research workflow
  -> paper-to-idea-to-experiment pipeline
```

This line is about turning reading into research output: hypotheses, experiment plans, results, and eventually draftable academic artifacts.

### 2. Quant Strategy Production

```text
LLMQuant domain knowledge
  + X2Strategy strategy compiler
  -> reproducible quant strategy experiments
```

This line is about turning financial papers, reports, and market ideas into structured strategy specs and testable backtest plans.

### 3. FICC Intelligence Workflow

```text
HKUDS LightRAG / RAG-Anything
  + LLMQuant rates / FX / commodities framing
  -> source-backed FICC analyst workflow
```

This line is about building AI workflows for real financial research tasks: rates, FX, commodities, macro signals, events, and decision support.

## What I Want To Avoid

I do not want this to become a folder collection.

The goal is not:

```text
download more repos
```

The goal is:

```text
understand -> compare -> extract -> integrate -> verify
```

Every useful external project should be converted into a smaller artifact in my own system:

- a map;
- a note;
- a reusable schema;
- a workflow;
- a benchmark;
- a demo;
- a public-safe write-up.

## First Practical Deliverable

The first deliverable should be a comparative note:

```text
HKUDS AI-Researcher / DeepResearch-Eval
vs LLMQuant
vs X2Strategy
```

I want to compare them by:

- input format;
- output artifact;
- agent loop;
- evaluation method;
- strongest reusable component;
- how they connect to a paper research OS or a finance research workflow.

After that, the first pilot should be small:

```text
one finance paper or market idea
  -> domain-framed strategy spec
  -> minimum experiment or backtest plan
  -> diagnosis note
```

## Long-Term Direction

The long-term goal is a personal research and quant production OS:

```text
read papers
  -> extract ideas
  -> generate hypotheses
  -> map to finance domains
  -> produce strategy specs
  -> run or plan experiments
  -> evaluate results
  -> write reusable research artifacts
```

This is the public-safe story I want to keep building: not an autonomous trading system, and not an automatic paper factory, but an auditable research production system that connects AI infrastructure, finance domain knowledge, and executable strategy research.
