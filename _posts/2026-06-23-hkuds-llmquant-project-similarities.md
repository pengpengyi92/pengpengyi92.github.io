---
title: "HKUDS and LLMQuant: Project Similarities for a Quant Research OS"
date: 2026-06-23 00:00:00 +0800
categories: [Learning, Research OS]
tags: [hkuds, llmquant, agents, rag, quant-research, research-os]
---

The more I compare HKUDS and LLMQuant, the more I see that they are not two unrelated project collections.

They are two versions of the same deeper idea:

```text
turn messy information into structured knowledge
turn structured knowledge into agent workflows
turn agent workflows into reviewable research artifacts
```

The difference is mainly the domain boundary.

```text
HKUDS    = general AI research infrastructure
LLMQuant = finance and quant research workflow system
```

This post focuses on their similarities, especially at the project level.

## 1. RAG and Knowledge Infrastructure

The clearest similarity is between HKUDS's RAG projects and LLMQuant's finance knowledge layer.

| HKUDS Projects | LLMQuant Projects | Shared Pattern |
|---|---|---|
| LightRAG | QuantMind | graph-aware knowledge retrieval |
| RAG-Anything | QuantMind + data-mcp | document ingestion and multimodal / multi-source research memory |
| MiniRAG / VideoRAG | finance reports, filings, papers, news workflows | source-grounded research over heterogeneous content |

HKUDS LightRAG shows how to build fast graph-based retrieval. RAG-Anything expands this into multimodal document processing across text, images, tables, charts, and equations.

LLMQuant QuantMind is the finance-domain version of the same pattern. It ingests papers, news, blogs, reports, and filings, then turns them into structured knowledge that can be searched, queried, and reused.

The shared idea:

```text
raw documents
  -> parsed units
  -> entities / relations / tags
  -> embeddings or graph
  -> retrieval
  -> research answer or artifact
```

For my own system, this is the memory layer of Pengyi Quant Research OS.

## 2. Agent Shells and Workflow Routing

HKUDS has several projects that make software and research workflows agent-native:

- nanobot;
- CLI-Anything;
- AutoAgent;
- OpenHarness;
- AI-Researcher.

LLMQuant has a similar concept in a finance-specific form:

- LLMQuant Skills;
- data-mcp;
- finance category workflows;
- market intelligence workflows;
- strategy and risk workflows.

The shared pattern:

```text
user intent
  -> route to the right skill / tool / workflow
  -> collect grounded context
  -> produce a structured output
  -> keep the result auditable
```

In HKUDS, the agent may operate over software tools, research papers, multimodal documents, or generic AI workflows.

In LLMQuant, the agent is routed into finance tasks:

```text
equities
options
macro
credit
rates / FX
commodities
crypto
portfolio
risk
strategies
```

The similarity is important: both ecosystems treat agents as workflow executors, not just chatbots.

## 3. Research Automation

HKUDS AI-Researcher is especially close to the R&D Agent direction I want to build.

At a high level, AI-Researcher is about turning research work into an agentic loop:

```text
literature review
  -> idea generation
  -> implementation
  -> validation
  -> result analysis
  -> paper or artifact drafting
```

LLMQuant points toward a finance-specific version:

```text
finance paper / market idea
  -> domain framing
  -> factor or strategy hypothesis
  -> implementation plan
  -> backtest or experiment
  -> risk diagnosis
  -> next research plan
```

This is exactly the bridge to a Quant R&D Agent:

```text
AI-Researcher
  + QuantMind
  + LLMQuant Skills
  + X2Strategy / Magents
  -> Quant Research OS
```

The common ambition is not summarization. It is research production.

## 4. Trading Agent and Strategy Execution

HKUDS has trading-related agent projects such as AI-Trader and Vibe-Trading. LLMQuant has Magents, llmquant-strategies, and awesome-trading-agents.

They all point to the same execution problem:

```text
natural-language idea
  -> structured strategy logic
  -> data access
  -> experiment or backtest
  -> diagnostics
  -> human review
```

The interesting part is not whether an agent can write trading code. The hard part is whether the loop is testable:

- What assumptions were made?
- What data was used?
- What baseline was compared?
- Was there look-ahead bias?
- How sensitive is the result to fees and slippage?
- Does the signal survive regime changes?

This is where LLMQuant can become more finance-rigorous, while HKUDS contributes stronger agent and infrastructure patterns.

## 5. Graph Learning, Recommender Systems, and Factor Discovery

HKUDS has a deep research base in graph learning and recommender systems:

- GraphGPT;
- OpenGraph;
- GraphAgent;
- RLMRec;
- LLMRec;
- XRec;
- RecLM.

LLMQuant's parallel direction is QuantMind's semantic knowledge graph and the future possibility of factor / paper / strategy recommendation.

The deeper similarity:

```text
recommendation = structured discovery
```

In consumer systems, recommender models discover products, content, or users.

In quant research, a similar structure can discover:

- related papers;
- related factors;
- reusable datasets;
- similar strategy mechanisms;
- unexplored asset-class transfer opportunities;
- risk patterns shared across strategies.

This suggests a strong research direction:

```text
graph learning / recommender systems
  -> factor idea recommendation
  -> paper-to-strategy matching
  -> research gap discovery
```

HKUDS gives the academic ML base. LLMQuant gives the financial objects to model.

## 6. Evaluation and Research Quality Control

HKUDS projects such as DeepResearch-Eval and OpenHarness point toward evaluation.

LLMQuant has finance-specific evaluation surfaces:

- research health check;
- risk workflows;
- backtesting engines;
- portfolio exposure maps;
- strategy diagnosis;
- evidence contracts through data-mcp.

Both ecosystems need the same discipline:

```text
generated answer
  -> evidence
  -> test
  -> diagnosis
  -> revision
```

This is the line between a demo and a research system.

For finance, evaluation is even stricter. A generated strategy is not useful until it survives:

- data validation;
- transaction cost assumptions;
- out-of-sample testing;
- bias checks;
- regime sensitivity;
- portfolio-level risk review.

## 7. The Project-to-Project Similarity Map

Here is my current working map:

| Research Function | HKUDS Side | LLMQuant Side | What I Can Reuse |
|---|---|---|---|
| project memory | LightRAG | QuantMind | source-grounded knowledge layer |
| multimodal document processing | RAG-Anything | QuantMind future reports / filings ingestion | PDF / report / table / chart understanding |
| personal agent shell | nanobot / AutoAgent | LLMQuant Skills | workflow routing design |
| tool-native execution | CLI-Anything | data-mcp | agent-accessible tool and data interfaces |
| research automation | AI-Researcher | Quant R&D Agent direction | paper-to-idea-to-experiment loop |
| trading agent | AI-Trader / Vibe-Trading | Magents / llmquant-strategies | idea-to-strategy-to-backtest loop |
| graph intelligence | GraphGPT / OpenGraph / GraphAgent | QuantMind semantic KG | financial knowledge graph |
| recommendation | RLMRec / LLMRec / RecLM | factor / paper / strategy recommendation | research discovery engine |
| evaluation | DeepResearch-Eval / OpenHarness | risk check / backtest / health check | validation and diagnosis loop |
| public artifact generation | Paper2Slides | research memo / strategy note / website post | communication layer |

## 8. Why This Matters for My Research OS

The practical direction is clear.

I should not simply collect HKUDS and LLMQuant repositories. I should extract reusable components from each ecosystem.

```text
HKUDS gives:
  agent design
  RAG architecture
  graph / recommender research
  evaluation infrastructure
  research automation patterns

LLMQuant gives:
  finance domain schema
  data access patterns
  market workflows
  quant research tasks
  strategy and risk templates
```

Combined, they suggest a concrete build path:

```text
source-grounded finance memory
  -> quant knowledge graph
  -> agent workflow router
  -> factor or strategy hypothesis generator
  -> implementation / experiment layer
  -> backtest and risk diagnosis
  -> human PM review
  -> reusable research artifact
```

That is the system I want to keep building.

## 9. First Pilot

A reasonable first pilot should be small:

```text
one public finance paper or market idea
  -> QuantMind-style structured knowledge card
  -> LightRAG-style retrieval memory
  -> LLMQuant-style domain workflow
  -> X2Strategy-style strategy spec
  -> Magents-style backtest plan
  -> diagnosis note
```

The deliverable is not a perfect trading system.

The deliverable is an auditable research artifact:

- what was read;
- what hypothesis was extracted;
- what strategy logic was proposed;
- what evidence supports it;
- what risks were diagnosed;
- what the next research step should be.

## Conclusion

HKUDS and LLMQuant are similar because both are moving from passive information consumption to active research production.

HKUDS approaches this from AI infrastructure.

LLMQuant approaches this from financial domain workflows.

Their intersection is exactly where a serious AI + Quant Research OS can be built:

```text
AI agent infrastructure
  + finance knowledge system
  + strategy execution loop
  + evaluation and human review
  -> research production OS
```

That is the project direction I want to compound.

## References

- HKUDS GitHub: <https://github.com/HKUDS>
- HKU Data Intelligence Lab: <https://sites.google.com/view/chaoh>
- HKUDS LightRAG: <https://github.com/HKUDS/LightRAG>
- HKUDS RAG-Anything: <https://github.com/HKUDS/RAG-Anything>
- LLMQuant GitHub: <https://github.com/LLMQuant>
- LLMQuant QuantMind: <https://github.com/LLMQuant/quant-mind>
- LLMQuant Skills: <https://github.com/LLMQuant/skills>
- LLMQuant Data MCP: <https://github.com/LLMQuant/data-mcp>
