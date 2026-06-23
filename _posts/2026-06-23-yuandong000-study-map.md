---
title: "yuandong000: Study Map for Yuandong Tian's Projects"
date: 2026-06-23 00:00:00 +0800
categories: [Learning, Study Map]
tags: [pengyistudymap_yuandong, yuandong-tian, research-os, ai-scientist, reinforcement-learning, research-engineering]
---

This is the opening map for a new learning series:

```text
pengyistudymap_yuandong
```

The goal is to study Yuandong Tian's public projects and related research systems as a living example of how a strong AI researcher turns ideas into code, code into experiments, and experiments into reusable research assets.

This first note is `yuandong000`: the study map.

Later notes can go deeper:

```text
yuandong001 -> arXiv_recbot
yuandong002 -> llm_utils and personal research tools
yuandong003 -> ELF, OpenGo, DarkForest, and game research systems
yuandong004 -> understanding, theory, and representation learning
yuandong005 -> optimization and solver-related repos
yuandong006 -> Obsidian, TODO tools, and personal knowledge workflows
```

## Why Study This

I am interested in a specific pattern:

```text
research question
  -> small tool
  -> experiment system
  -> reproducible artifact
  -> paper / project / public output
```

Yuandong Tian's public work is useful because it spans several layers that matter for an AI scientist:

- reinforcement learning and planning;
- game AI systems;
- optimization and theory;
- LLM reasoning and efficiency;
- paper recommendation and research workflow tools;
- personal infrastructure for learning and output.

For my own path, the purpose is not imitation. The purpose is extraction:

```text
What engineering patterns can become part of Pengyi Research OS?
What research taste can guide AI + quant research?
What small tools can compound into serious research output?
```

## Local Study Snapshot

I created a local shallow-clone workspace for study.

Current snapshot:

| Item | Count / Size |
|---|---:|
| Public repos from `yuandong-tian` | 33 |
| Related organization repos added | 3 |
| Total local repos | 36 |
| Clone mode | `git clone --depth 1` |
| Local size | about 245 MiB |

The three extra related organization repos are:

- `facebookresearch/ELF`;
- `pytorch/ELF`;
- `facebookresearch/darkforestGo`.

These are important because they connect directly to the OpenGo / AlphaZero-style research line.

## The High-Level Map

I currently divide the repos into five study zones.

| Zone | Core Repos | What To Learn |
|---|---|---|
| Research workflow tools | `arXiv_recbot`, `llm_utils`, `tools2`, `tools3`, `todo-tools`, `obsidian-plugin` | how research intake, paper recommendation, notes, and small tools support deep work |
| RL and game systems | `facebookresearch/ELF`, `pytorch/ELF`, `darkforestGo`, `ELF-1`, `ELF-examples`, `MiniRTS-pretrain-models` | how complex research systems are structured around environments, agents, training loops, and evaluation |
| Theory and representation learning | `understanding`, `ICML17_ReLU`, `DataDrivenDescent` | how theory questions are converted into experiments and notebooks |
| Optimization and solvers | `PySCIPOpt`, `glucose`, `minisat` | how search, solver, and optimization tools connect to AI decision-making |
| Background forks and references | `graph_nets`, `pytorch_geometric`, `luckmatters`, `seq2seq-keyphrase-pytorch`, `Auto-GPT` | what external systems were useful enough to fork or inspect |

This map gives a controlled reading order. The point is not to read everything equally.

The first pass should prioritize repos that directly inform my own Research OS.

## Reading Order

### 1. `arXiv_recbot`

This is the first repo to study.

Why:

- It is directly connected to paper discovery.
- It suggests a concrete way to build a research intake loop.
- It is small enough to understand quickly.
- It can inspire a private research assistant that ranks papers, tracks preferences, and pushes next reading actions.

For Pengyi Research OS, the natural adaptation is:

```text
paper feed
  -> preference signal
  -> recommendation model / ranking heuristic
  -> reading queue
  -> summary card
  -> research task
```

For quant research, this can become:

```text
finance paper / market paper
  -> factor idea
  -> data requirement
  -> experiment plan
  -> backtest task
```

### 2. `llm_utils`, `tools2`, `tools3`, `todo-tools`

These repos are likely more useful than they look at first glance.

Strong researchers often build many small tools around their own workflow. The interesting question is:

```text
What repeated friction did this tool remove?
```

For my own work, this is important because a Research OS should not begin as a huge platform. It should begin as small scripts that remove repeated friction:

- reading;
- summarizing;
- scheduling;
- running experiments;
- checking outputs;
- generating reports;
- creating next actions.

If a small tool is used every day, it becomes infrastructure.

### 3. ELF, OpenGo, and DarkForest

This is the system-design layer.

The core repos:

- `facebookresearch/ELF`;
- `pytorch/ELF`;
- `facebookresearch/darkforestGo`;
- `ELF-1`;
- `ELF-examples`.

What I want to learn:

- how an RL platform separates game environment, agent, model, training, and evaluation;
- how self-play systems organize data generation and policy improvement;
- how a research system becomes reproducible enough for others to inspect;
- how engineering constraints shape research output.

This matters for quant because serious quant research also needs environment design:

```text
market data
  -> research environment
  -> strategy agent / factor model
  -> simulation
  -> evaluation
  -> diagnosis
```

The market is not a game board, but the system pattern is still useful.

### 4. `understanding` and Theory Repos

The `understanding` repo is the largest personal repo in this study snapshot.

This group should be read after the practical workflow and system repos, because it may require more context.

The target is to understand how theory-facing questions become code:

```text
claim
  -> minimal experiment
  -> visualization / notebook
  -> repeated test
  -> paper-level insight
```

For my own AI scientist path, this is one of the most important muscles: not only using tools, but making claims testable.

### 5. Optimization and Solver Repos

The solver-related repos are useful background:

- `PySCIPOpt`;
- `glucose`;
- `minisat`.

They connect to a broader research line:

```text
search
planning
combinatorial optimization
decision-making
```

This is relevant to LLM reasoning, agent planning, and quant portfolio construction.

For quant research, the connection may appear in:

- portfolio optimization;
- execution constraints;
- feature selection;
- strategy search;
- risk budgeting;
- combinatorial allocation.

## What To Extract For Pengyi Research OS

I want to extract four reusable layers.

### Layer 1: Research Intake

This is the paper and information layer.

Potential components:

- paper recommendation;
- reading queue;
- source metadata;
- summary cards;
- preference feedback;
- topic graph.

Relevant repos:

- `arXiv_recbot`;
- `llm_utils`;
- `obsidian-plugin`.

### Layer 2: Research Execution

This is the experiment layer.

Potential components:

- experiment config;
- reproducible run command;
- dataset boundary;
- model / strategy implementation;
- metrics;
- failure diagnosis.

Relevant repos:

- `ELF`;
- `ELF-examples`;
- `understanding`;
- `DataDrivenDescent`.

### Layer 3: Planning and Decision Systems

This is the agent / RL / search layer.

Potential components:

- state;
- action;
- reward;
- policy;
- planning loop;
- evaluation loop.

Relevant repos:

- `facebookresearch/ELF`;
- `pytorch/ELF`;
- `darkforestGo`;
- solver-related repos.

### Layer 4: Personal Productivity Infrastructure

This is the layer that makes output compound.

Potential components:

- TODO system;
- schedule system;
- research notes;
- personal website;
- writing pipeline;
- public/private knowledge boundary.

Relevant repos:

- `todo-tools`;
- `scheduler`;
- `scheduler2`;
- `scheduler_new`;
- `obsidian-plugin`;
- `yuandong-tian.github.io`.

## The Quant Research Translation

My own target is not only general AI research. It is AI + quant research engineering.

The translation looks like this:

| General AI Research Pattern | Quant Research Translation |
|---|---|
| paper recommender | finance paper / factor idea recommender |
| RL environment | market simulation and backtest environment |
| self-play / simulation | strategy simulation across regimes |
| model evaluation | out-of-sample performance, turnover, drawdown, risk exposure |
| theory experiment | factor robustness and bias diagnosis |
| personal research tools | private quant research operating system |

This creates a concrete bridge:

```text
Yuandong-style research workflow
  + LLMQuant / QuantMind knowledge layer
  + X2Strategy-style strategy compiler
  + our own backtest and diagnosis loop
  -> Pengyi Quant Research OS
```

## The First Three Study Outputs

The next three notes should be concrete:

| Note | Topic | Deliverable |
|---|---|---|
| `yuandong001` | `arXiv_recbot` | paper recommendation workflow map |
| `yuandong002` | `llm_utils` and small tools | reusable research utility patterns |
| `yuandong003` | ELF / DarkForest | RL system architecture map |

Each note should answer the same three questions:

```text
1. What does this project do?
2. How is it implemented?
3. What can be reused in Pengyi Research OS?
```

## Public-Safe Boundary

This series should stay public-safe.

Publish:

- public GitHub observations;
- public paper/project links;
- high-level architecture maps;
- personal learning notes;
- reusable engineering patterns.

Do not publish:

- private communications;
- confidential job/application strategy;
- non-public datasets;
- private factor libraries;
- anything tied to employer internal work.

Private planning stays in private repos. The public website should only show the cleaned learning trail.

## Conclusion

This series is a study map for becoming stronger at research engineering.

The core question is:

```text
How does a serious AI researcher build systems around research itself?
```

Yuandong Tian's public projects give a useful reference point across paper reading, tools, RL systems, optimization, theory, and public output.

For my own path, the target is to convert this learning into a practical operating system:

```text
read better
think better
build faster
evaluate harder
publish cleaner
```

That is the purpose of `pengyistudymap_yuandong`.

## References

- Yuandong Tian GitHub: <https://github.com/yuandong-tian>
- Yuandong Tian homepage: <https://yuandong-tian.com/>
- `facebookresearch/ELF`: <https://github.com/facebookresearch/ELF>
- `pytorch/ELF`: <https://github.com/pytorch/ELF>
- `facebookresearch/darkforestGo`: <https://github.com/facebookresearch/darkforestGo>
- `arXiv_recbot`: <https://github.com/yuandong-tian/arXiv_recbot>
