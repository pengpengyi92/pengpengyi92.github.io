---
title: "LLMQUANT005: awesome-trading-agents 作为交易 Agent 生态雷达"
date: 2026-06-24 00:00:00 +0800
categories: [Learning, Quant Research]
tags: [pengyi-llmquant-studymap, llmquant005, awesome-trading-agents, trading-agents, mcp, skills, research-os]
---

这是 `PENGYI_LLMQUANT_STUDYMAP` 的第六篇。

```text
LLMQUANT005 -> awesome-trading-agents
```

前面几篇已经拆了 LLMQuant 里的几个系统层：

```text
LLMQUANT001 = data-mcp as evidence access layer
LLMQUANT002 = skills as finance workflow routing layer
LLMQUANT003 = quant-mind as financial knowledge structuring layer
LLMQUANT004 = Magents as strategy execution and simulation layer
```

这一篇看 `awesome-trading-agents`。

我的一句话结论：

```text
awesome-trading-agents = trading agent ecosystem radar
```

它不是一个执行框架。
它不是一个回测引擎。
它也不是一个 MCP server。

它是一张地图：

```text
Agents
MCPs
Skills
Resources
```

更准确地说，它是在回答：

```text
现在 AI-native trading agent 生态里，到底有哪些项目？
它们分别解决哪一层问题？
我们应该先学谁？
我们可以跟谁对标？
我们可以给谁提 PR？
我们自己的 Research OS 缺哪一块？
```

这对我非常重要。

因为我们不是只想做一个孤立项目。
我们要进入一个生态。
进入生态意味着：

```text
知道主流项目
知道分类边界
知道质量标准
知道贡献规则
知道下一步应该跟谁对齐
```

## Project snapshot

我本地看的项目是：

```text
LLMQUANT/awesome-trading-agents
```

项目结构非常轻：

```text
README.md
README.zh-CN.md
CONTRIBUTING.md
CONTRIBUTING.zh-CN.md
LICENSE
assets/
.github/
```

它没有源码目录。

这说明它的核心资产不是 code runtime，而是：

```text
curated knowledge
taxonomy
entry descriptions
quality bar
contribution workflow
bilingual presentation
```

本地 README 统计：

| Item | Count |
|---|---:|
| sub-categories | 19 |
| listed entries | 114 |
| GitHub links | 120 |
| arXiv links | 6 |

这是一份相当大的 LLM trading agent 生态索引。

## Scope

README 说得很清楚：

```text
Awesome Trading Agents collects open-source projects where LLMs help research markets,
make trading decisions, or connect agents to market data and execution tools.
```

也就是说，它收录的是 post-LLM 时代的 agentic trading stack。

它主动不覆盖：

```text
classic quant libraries
time-series models
reinforcement-learning trading bots
generic finance AI lists
```

这点很重要。

因为很多 "awesome finance" 清单会混在一起：

```text
传统量化库
深度学习预测
RL trading
金融 NLP
agent trading
券商 API
```

`awesome-trading-agents` 把边界收窄到：

```text
LLM-driven agents
MCP servers
Agent Skills
directly relevant papers and learning resources
```

这个边界对我们有启发。

做一个好系统，先要知道自己不做什么。

## Three pillars

它的三大支柱是：

```text
Agents
MCPs
Skills
```

我会这样理解：

| Pillar | Role |
|---|---|
| Agents | 决策主体，负责研究、推理、组合决策、交易行为 |
| MCPs | 工具接口，负责数据、券商、交易所、研究工具、回测平台 |
| Skills | 工作流说明，负责把任务变成可复用 procedure |

这和我们前面几篇高度对应：

```text
Agents -> R&D Agent / Trading Agent / PM Agent
MCPs -> data-mcp and broker/exchange connectors
Skills -> workflow routing and repeatable task contracts
```

也就是说：

```text
awesome-trading-agents 是生态地图
data-mcp / skills / quant-mind / Magents 是其中某些系统层的具体实现
```

## Category distribution

本地 README 里，各子类条目数量如下：

| Category | Entries |
|---|---:|
| Agents > Multi-agent trading systems | 26 |
| Agents > Single-agent end-to-end traders | 8 |
| Agents > Research / equity-research copilots | 6 |
| Agents > Real-money / competition experiments | 5 |
| Agents > Prediction-market specialists | 3 |
| Agents > Benchmarks & evaluations | 4 |
| Agents > Strategy coding / self-improving agents | 2 |
| MCPs > Market data / data providers | 16 |
| MCPs > Brokerage / exchange trading | 9 |
| MCPs > Research tools / analysis | 5 |
| MCPs > TradingView bridge | 1 |
| MCPs > Prediction markets | 2 |
| MCPs > Strategy / backtesting platforms | 1 |
| Skills > Equity research | 5 |
| Skills > Crypto / DeFi / on-chain | 2 |
| Skills > Strategy coding & backtesting | 1 |
| Skills > Brokerage execution & portfolio | 2 |
| Resources > Papers | 4 |
| Resources > Learn | 2 |

这个分布很有意思。

最大的类是：

```text
Agents > Multi-agent trading systems
```

这说明当前 trading agent 生态的主流方向，还是在探索：

```text
analyst team
debate
trader
risk manager
portfolio manager
multi-agent decision process
```

第二大块是：

```text
MCPs > Market data / data providers
```

这也很合理。

因为交易 agent 最大的现实瓶颈之一就是：

```text
data access
data freshness
data scope
tool calling
broker/exchange integration
```

没有数据和工具，agent 只是聊天。

## First-read picks

README 里直接给了 "If you only read three"。

它推荐三组：

```text
Agents:
  TauricResearch/TradingAgents
  virattt/ai-hedge-fund
  HKUDS/AI-Trader

MCPs:
  alpacahq/alpaca-mcp-server
  krakenfx/kraken-cli
  financial-datasets/mcp-server

Skills:
  tradermonty/claude-trading-skills
  himself65/finance-skills
  RKiding/Awesome-finance-skills
```

这个推荐很实用。

它不是随机挑项目。

这九个项目覆盖了三条主线：

```text
1. trading decision system
2. data / execution tool interface
3. repeatable trading workflow
```

对我来说，最值得优先看的顺序是：

```text
TradingAgents
AI-Trader
ai-hedge-fund
alpacahq/alpaca-mcp-server
financial-datasets/mcp-server
claude-trading-skills
```

原因是：

```text
TradingAgents -> multi-agent decision architecture
AI-Trader -> agent-native live trading platform idea
ai-hedge-fund -> LLM analyst personas and PM decision loop
Alpaca MCP -> broker + paper/live trading interface
Financial Datasets MCP -> financial data interface
Claude trading skills -> repeatable task procedures
```

这和我们自己的 Research OS 直接相关。

## Agents pillar

Agents 是最核心的一栏。

README 对 Agents 的定义是：

```text
projects where an LLM is part of the actual research or trading decision
```

这句话很关键。

它排除了只把 LLM 当作：

```text
post-hoc explainer
UI assistant
generic chatbot
```

真正收录的是：

```text
LLM participates in market research
LLM participates in trading decision
LLM participates in strategy generation
LLM participates in evaluation
```

Agents 下面又分成七类。

### Multi-agent trading systems

这一类最大。

代表项目包括：

```text
TradingAgents
TradingAgents-CN
TradingAgents-AShare
AI-Trader
Vibe-Trading
FinRobot
QuantDinger
AutoHedge
LangAlpha
CryptoTradingAgents
AlpacaTradingAgent
oracle3
AutoGen financial analysis
```

这一类的共同问题是：

```text
一个 trading decision 是否应该由多个 agent 分工完成？
```

典型结构是：

```text
fundamental analyst
technical analyst
news analyst
sentiment analyst
bull researcher
bear researcher
trader
risk manager
portfolio manager
```

这和我们前面看 `Magents` 的 pod/agent 结构相似。

区别在于：

```text
TradingAgents-like projects 更偏 decision debate
Magents 更偏 execution simulation
```

两者可以合并：

```text
multi-agent research team
  -> SignalEvent
  -> Magents execution pod
  -> backtest / portfolio / risk
```

### Single-agent end-to-end traders

这一类包括：

```text
virattt/ai-hedge-fund
OpenAlice
atlas-gic
Hyperliquid AI trading agents
Gemini crypto trading agent
CloddsBot
minimal TypeScript trading agent demos
```

它们的核心问题是：

```text
一个 agent 能不能从 research 到 entry/hold/exit 走完全流程？
```

这类项目的优点：

```text
系统简单
决策链短
容易 demo
容易连接实盘 API
```

缺点：

```text
容易缺少专业分工
难以审计每个子观点
风险和组合层容易混在一个 prompt 里
```

对我来说，single-agent 项目适合学习：

```text
end-to-end UX
broker/exchange integration
minimal viable trading loop
```

但我们的 Research OS 长期更可能走：

```text
multi-agent research
typed workflow
human PM review
event-driven execution
```

### Research copilots

这一类包括：

```text
daily_stock_analysis
alpha-arena
PanWatch
WyckoffTradingAgent
DeepEar
finance-agent
```

它们不一定直接交易。

更多是：

```text
monitor
screen
research
Q&A
report
signal tracking
```

这对我也很重要。

因为我们不应该一开始就强行实盘。

一个现实路径是：

```text
research copilot
  -> paper trading
  -> backtest lab
  -> PM review
  -> limited execution
```

先做高质量 research copilot，反而更稳。

### Real-money / competition experiments

这一类很关键。

包括：

```text
LLM-Trading-Lab
nof1.ai
alpha-arena-okx
OpenNof1
LLM-trader-test
```

它们的价值在于：

```text
forward-only
live or quasi-live
真实市场条件
真实风险约束
可评估 agent 行为
```

这比历史回测更接近未来。

对我们而言，它提醒了一个问题：

```text
backtest evidence is not enough
```

最终要有：

```text
paper trading
live benchmark
out-of-sample protocol
decision log
audit trail
```

### Prediction-market specialists

这一类包括：

```text
Kalshi
Polymarket
multi-venue prediction-market arbitrage
```

这类项目特别适合 agent。

原因是 prediction market 的任务天然包含：

```text
event research
probability estimation
order book comparison
Kelly sizing
position management
resolution risk
```

LLM 擅长事件研究。
但概率校准和风控必须严肃处理。

这对我们未来做非股票资产研究也有启发。

### Benchmarks and evaluations

代表项目：

```text
live-trade-bench
AgenticTrading
finance-agent benchmark
DeepFund
```

这类项目很重要。

因为 agent trading 不能只靠 demo。

需要：

```text
benchmark
leaderboard
standard task
live evaluation
reproducible protocol
```

如果我们未来冲顶会，这一类很值得研究。

顶会不只是做一个交易 agent。
更强的方向可能是：

```text
new evaluation protocol for agentic financial research
live/forward benchmark
agent decision audit dataset
research-to-backtest reproducibility benchmark
```

### Strategy coding / self-improving agents

这一类目前条目少，但对我最接近 R&D Agent。

代表项目：

```text
pwb-alphaevolve
Miasyster/QuantGPT
```

这里的核心问题是：

```text
LLM 能不能自动写策略、改策略、跑回测、迭代？
```

这和我们的目标完全一致：

```text
自动提出因子假设
自动实现
自动回测
自动诊断偏差
自动生成下一轮研究计划
```

所以虽然这一类只有 2 个条目，但它是我最应该长期关注的一类。

## MCPs pillar

MCPs 是工具层。

README 的定义是：

```text
servers that let an agent call external tools through the Model Context Protocol
```

它覆盖：

```text
market data
brokerage
exchange trading
research tools
TradingView bridge
prediction markets
backtesting platforms
```

这正好对应我们前面看 `data-mcp` 的意义：

```text
agent needs tools, not just text context
```

### Market data / data providers

这一类有 16 个条目，是 MCP 里最大的一组。

代表项目：

```text
LLMQuant/data-mcp
financial-datasets/mcp-server
opennews-mcp
FinanceMCP
akshare MCPs
Yahoo Finance MCPs
SEC EDGAR MCP
FMP MCP
Octagon MCP
Equibles
```

这类 MCP 解决：

```text
prices
fundamentals
news
filings
macro
13F
crypto
regional markets
```

对我们来说，数据 MCP 是 Research OS 的地基。

没有稳定数据工具，就没有：

```text
evidence retrieval
factor implementation
backtest
live monitoring
```

### Brokerage / exchange trading

代表项目：

```text
Alpaca MCP
Kraken CLI
Korea Investment Open Trading API
OKX agent trade kit
MetaTrader MCP
IBKR MCP
QuantConnect MCP
```

这一层解决：

```text
paper trading
live trading
exchange data
broker actions
order placement
portfolio access
```

这是从 research 到 execution 的门。

但这也是最需要风控和审批的层。

我自己的原则是：

```text
先 research and paper trading
再 constrained execution
最后才 live trading
```

### Research tools / analysis

代表项目：

```text
maverick-mcp
tradememory-protocol
TradingAgents-MCPmode
AI-Kline
stock-scanner-mcp
```

这类特别像 Research OS 的插件。

其中 `tradememory-protocol` 这种项目很值得关注。

因为它在做：

```text
decision rationale
outcomes
review evidence
trade memory
```

这和我们自己的 experiment ledger 非常接近。

### Strategy / backtesting platforms

这里目前只有一个代表：

```text
whchien/ai-trader
```

但这个类对我们很重要。

未来我觉得这里会扩展。

因为 trading agent 生态必然需要：

```text
backtest as a tool
strategy evaluation as a tool
simulation as a tool
portfolio analysis as a tool
```

这也说明 `Magents` 如果未来暴露 MCP，会非常自然。

## Skills pillar

Skills 是第三根支柱。

README 里说：

```text
Skills are reusable instructions and workflows for Claude Code or other agent systems.
```

它们的作用是：

```text
让 agent 稳定重复完成一个金融任务
```

比如：

```text
research a stock
check options
backtest a strategy
manage a portfolio
execute with broker tools
```

这一栏和 `LLMQUANT002` 对应。

`skills` 不是普通 prompt。
它是 workflow contract。

### Equity research skills

代表项目：

```text
claude-trading-skills
finance-skills
finance_skills
claude-equity-research
Awesome-finance-skills
```

它们覆盖：

```text
market analysis
breadth
regime
screening
options
valuation
earnings review
ETF checks
liquidity
geopolitical risk
buy/sell/hold report
```

这对我们公开网站的学习内容也很有帮助。

我们可以把自己的研究流程逐渐沉淀为：

```text
Pengyi factor research skill
Pengyi paper-to-factor skill
Pengyi quant interview prep skill
Pengyi PM review skill
```

### Strategy coding and backtesting skills

目前代表是：

```text
vectorbt-backtesting-skills
```

这和我们 R&D Agent 特别贴近。

因为我们最终要把：

```text
factor idea
```

变成：

```text
implementation
backtest
optimization
quick stats
strategy comparison
```

Skill 的意义是让这个过程可重复。

### Brokerage execution and portfolio skills

代表项目：

```text
trading_skills
finlab-ai
```

它们连接：

```text
options
market data
portfolio work
IBKR
Alpaca
FinLab data
Taiwan equity strategy discovery
```

这一类适合学习“从研究到实际组合管理”的工作流边界。

## Resources

Resources 目前分为：

```text
Papers
Learn
```

Papers 里收录的是直接解释项目的论文，而不是泛泛的金融 LLM 论文。

包括：

```text
TradingAgents paper
LLM-Trading-Lab paper/repo
FinRobot paper
DeepFund paper
```

Learn 目前很短：

```text
Tauric Research GitHub Org
AI4Finance Foundation GitHub Org
```

这很克制。

它没有把泛教程全部塞进去，而是保留和 trading agent stack 直接相关的学习入口。

这也是 curated list 应该有的纪律。

## Curation quality bar

贡献指南里定义了质量标准。

新条目一般要满足：

```text
open source or public technical artifact
demonstrably LLM-driven
active in last 12 months or canonical
clear scope and minimal documentation
distinct contribution
public credibility signal, normally >=100 GitHub stars
```

这个质量标准非常值得我们学习。

因为它不是“看到一个项目就收录”。

它要求：

```text
技术上属于这个生态
有公开可信度
近期可用
和已有条目不重复
读者能快速判断它做什么
```

这对我以后提 PR 也很关键。

不能为了提 PR 而提 PR。

正确路径是：

```text
真实使用项目
发现缺失或错误
确认 fit and quality bar
提交 issue 或 PR
中英文同步
解释分类和区别
```

## Contribution workflow

这个 repo 的贡献流程很成熟。

新增条目需要：

```text
Repo URL
Pillar + sub-category
Proposed entry bullet
GitHub stars
Last commit or activity date
Quality-bar self-evaluation
Pairing annotation
Curation notes
```

PR 模板还要求：

```text
README.md and README.zh-CN.md structural sync
awesome-lint advisory check
Conventional Commit PR title
```

CI 包括：

```text
link-check with lychee
PR title semantic check
```

这个设计对我们有两个启发。

第一，awesome list 也可以是工程项目。

它需要：

```text
taxonomy
quality bar
templates
CI
bilingual sync
link rot maintenance
```

第二，如果我们以后做自己的 curated research list，也应该有类似规则。

比如：

```text
Pengyi Quant Research OS papers
Pengyi AI scientist tools
Pengyi factor research resources
Pengyi top conference target map
```

这些都不应该是随便堆链接。
它们应该是可维护的知识资产。

## Relationship with previous LLMQuant projects

现在把 001 到 005 串起来。

```text
data-mcp
  = evidence access layer

skills
  = finance workflow routing layer

quant-mind
  = financial knowledge structuring layer

Magents
  = strategy execution and simulation layer

awesome-trading-agents
  = ecosystem radar
```

它们的关系是：

```text
awesome-trading-agents tells us what exists
data-mcp gives us evidence access
skills gives us workflow contracts
quant-mind gives us structured research memory
Magents gives us simulation and execution architecture
```

组合起来就是：

```text
ecosystem map
  -> choose project / pattern
  -> retrieve evidence
  -> structure knowledge
  -> route workflow
  -> run strategy simulation
  -> record experiment
  -> contribute back to ecosystem
```

这对我们非常关键。

因为我们不是闭门造系统。
我们是在对齐生态主线。

## What this means for Pengyi Research OS

`awesome-trading-agents` 给我的最大启发是：

```text
我们的 Research OS 必须有生态视角。
```

不能只看自己的代码。

我们要知道：

```text
谁在做 multi-agent trading
谁在做 live benchmark
谁在做 MCP data
谁在做 broker execution
谁在做 skill workflow
谁在做 strategy coding agent
谁在做 trading memory
谁在做 evaluation benchmark
```

然后把这些映射到自己的系统层：

| Ecosystem layer | Pengyi layer |
|---|---|
| Multi-agent trading systems | R&D Agent + PM review |
| Research copilots | Research OS UI and notes |
| Real-money experiments | paper trading / live audit |
| Benchmarks | top-conference evaluation protocol |
| Strategy coding agents | factor implementation loop |
| Market data MCPs | evidence and data access |
| Broker MCPs | future constrained execution |
| Skills | repeatable research workflows |
| Trading memory | experiment ledger |

这张映射图会帮助我们避免走偏。

## How we should use the list

我不会把它当成收藏夹。

我会按四种方式使用。

### 1. 学习路线

先按系统层学习：

```text
TradingAgents -> multi-agent decision
AI-Trader -> agent-native platform
ai-hedge-fund -> analyst persona + portfolio manager
data-mcp / financial-datasets -> market data MCP
Alpaca / Kraken / OKX -> execution MCP
claude-trading-skills -> repeatable workflow
live-trade-bench / DeepFund -> evaluation
QuantGPT / pwb-alphaevolve -> strategy coding loop
```

### 2. Project comparison

每看一个项目，就问：

```text
它是 agent, MCP, skill, benchmark, or platform?
它解决 research, data, execution, risk, or evaluation?
它有没有可复用架构？
它和我们的 Research OS 哪一层对应？
```

### 3. PR opportunity map

我们以后可以提 PR 的方向：

```text
add missing high-quality entries
fix stale links
improve bilingual descriptions
add pairing notes
move misclassified entries
add new benchmark/resource entries
clarify project lineage
```

但前提是：

```text
真实使用
真实发现
小而准确
符合贡献规则
```

### 4. Research opportunity map

这份清单也能启发论文方向。

可能的 research questions：

```text
How should trading agents be evaluated live?
How do multi-agent debates improve or hurt trading decisions?
How can agent decisions be audited across data, reasoning, and execution?
How can paper-to-strategy pipelines be made reproducible?
How can MCP tool access be made safe for trading agents?
How should human PM review be formalized in AI trading systems?
```

这些都是可以冲 workshop、顶会、开源项目的方向。

## Gaps I notice

这份 list 已经很强，但从我的 Research OS 视角，仍然可以观察几个生态缺口。

### 1. Research memory is still underdeveloped

很多项目有 agent，有工具，有执行。

但真正严肃的：

```text
decision memory
experiment ledger
evidence citations
post-trade review
research lineage
```

还不多。

这正好是我们可以做的。

### 2. Evaluation is still early

Benchmark 类项目数量不多。

未来 trading agent 如果要进学术主流，一定需要更强的：

```text
task suite
live benchmark
audit protocol
data leakage control
reproducible evaluation
```

这也适合我们冲顶会。

### 3. Strategy coding loop is still small

`Strategy coding / self-improving agents` 目前只有少数条目。

但这是最接近 AI scientist / quant R&D agent 的方向。

这个生态很可能还会快速增长。

### 4. Human PM governance is not yet standard

很多 trading agent 强调 autonomous。

但我认为现实里必须有：

```text
human PM approval
risk sign-off
audit log
capital allocation decision
```

这可能是我们系统的差异化。

### 5. Cross-pillar integration is still fragmented

Agents、MCPs、Skills 经常是分散项目。

未来强系统应该是：

```text
agent decision
  + MCP data and execution
  + Skill workflow
  + memory
  + backtest
  + live evaluation
  + PM review
```

这正是我们可以融会贯通的地方。

## PR ideas for us

如果未来给 `awesome-trading-agents` 提 PR，我会优先考虑这些小而稳的方向。

### 1. Bilingual polishing

如果发现中英文 README 某些 entry 不完全对齐，可以提：

```text
style(zh-CN): align wording for <entry>
```

### 2. Add missing canonical project

前提是项目满足：

```text
LLM-driven
public code/artifact
active
distinct
>=100 stars or first-party/canonical exception
```

PR title 例子：

```text
docs(agents): add org/repo to benchmarks
```

### 3. Add pairing notes

如果一个 Agent 明确使用某个 MCP，或者某个 Skill 配套某个 broker MCP，可以补：

```text
*(→ pairs with: <project>.)*
```

这类 PR 很小，但对读者很有价值。

### 4. Add Research OS adjacent project

如果我们未来自己的开源项目成熟了，可能可以作为：

```text
Skills
Research tools / analysis MCP
Strategy coding / self-improving agents
Benchmarks & evaluations
```

但必须达到质量标准。

不能提前硬塞。

## Pengyi action plan

基于这份清单，我给自己定一个实际行动顺序。

### Phase 1: Read the anchor projects

优先看：

```text
TradingAgents
AI-Trader
ai-hedge-fund
claude-trading-skills
Alpaca MCP
financial-datasets MCP
live-trade-bench
DeepFund
QuantGPT
pwb-alphaevolve
```

### Phase 2: Build comparison notes

每个项目都按同一模板记录：

```text
project purpose
architecture
data sources
agent roles
tool interfaces
execution/backtest support
risk controls
memory/evaluation design
what Pengyi can learn
possible PR
```

### Phase 3: Extract reusable architecture

把项目中的共同结构抽出来：

```text
Research Agent
Data Tool
Signal Generator
Execution Agent
Risk Manager
Portfolio Manager
Memory Ledger
Evaluation Harness
PM Review
```

### Phase 4: Map to our Research OS

形成自己的系统模块：

```text
Pengyi Data Layer
Pengyi Knowledge Layer
Pengyi Workflow Skills
Pengyi StrategySpec
Pengyi Magents Runner
Pengyi Experiment Ledger
Pengyi PM Review Console
```

### Phase 5: Contribute back

真实使用以后：

```text
open issue
fix docs
submit PR
improve entry
add missing pairing
```

这就是从使用者到 contributor 的路径。

## One useful mental model

可以把 `awesome-trading-agents` 看成 AI trading stack 的目录树。

```text
Agents
  -> who thinks and decides

MCPs
  -> what tools they can call

Skills
  -> how they repeat tasks reliably

Resources
  -> what papers and orgs define the field
```

而我们的 Research OS 是：

```text
take this ecosystem map
  -> choose components
  -> learn architecture
  -> build our own integrated system
  -> produce public artifacts
  -> contribute back
```

这就是生态视角。

## LLMQUANT005 conclusion

`awesome-trading-agents` 对我的启发是：

```text
AI quant research 不是单点技术竞赛。
它是 agent, tools, skills, memory, backtest, execution, evaluation, governance 的系统工程。
```

它让我们看到：

```text
现在谁在做什么
每类项目的边界在哪里
我们应该先学什么
我们可以在哪里贡献
我们自己的 Research OS 还缺什么
```

这篇之后，LLMQuant 的前五个学习点已经串起来了：

```text
000 = map the LLMQuant universe
001 = data access
002 = workflow routing
003 = knowledge structuring
004 = simulation runtime
005 = ecosystem radar
```

下一篇：

```text
LLMQUANT006 -> finance knowledge layer
```

重点看 `docs`、`llmquant-book`、`quant-wiki` 这些金融知识层，以及它们如何成为 Research OS 的基础教材和知识库。
