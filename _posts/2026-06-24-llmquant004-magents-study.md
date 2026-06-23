---
title: "LLMQUANT004: Magents 作为多策略回测与仿真层"
date: 2026-06-24 00:00:00 +0800
categories: [Learning, Quant Research]
tags: [pengyi-llmquant-studymap, llmquant004, magents, multi-agent-trading, backtesting, research-os]
---

这是 `PENGYI_LLMQUANT_STUDYMAP` 的第五篇。

```text
LLMQUANT004 -> Magents
```

前面几篇已经逐渐把 LLMQuant 的层次拆出来：

```text
LLMQUANT000 = project study map
LLMQUANT001 = data-mcp as evidence access layer
LLMQUANT002 = skills as finance workflow routing layer
LLMQUANT003 = quant-mind as financial knowledge structuring layer
```

这一篇看 `Magents`。

我的一句话结论：

```text
Magents = strategy execution and simulation layer
```

更贴近量化研究系统的说法：

```text
Magents 负责把策略想法放进一个多 pod、多 agent、带订单、组合、风控、绩效指标的回测环境里跑。
```

也就是说：

```text
quant-mind
  -> turns paper/news/report into structured knowledge

R&D Agent
  -> turns structured knowledge into factor or strategy hypothesis

Magents
  -> turns strategy hypothesis into simulated trading behavior and performance evidence
```

这正好接到我们之前一直说的 R&D loop：

```text
自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ 人类 PM 审核
```

`Magents` 对应的是中间非常关键的部分：

```text
implementation -> simulation -> metrics -> risk feedback
```

## Project snapshot

我本地看的项目是 `LLMQUANT/Magents`。

当前项目关键信息：

| Item | Value |
|---|---|
| package | `magents` |
| version | `0.1.0` |
| slogan | Multi-Agent Generative Trading System |
| Python | `>=3.10,<3.13` |
| main package | `src` |
| source Python files | 64 |
| test Python files | 6 |
| core dependencies | numpy, pandas, matplotlib, pyyaml |
| reporting dependency | jinja2 |
| dev dependency | pytest, pytest-cov, ruff |
| CLI entry | `magents = src.main:main` |

README 对它的定义是：

```text
open-source Python framework for multi-strategy hedge fund simulation and backtesting
```

它的核心想法是：

```text
independent strategy pods
  + shared data feed
  + shared event-driven engine
  + central risk management
  + portfolio-level accounting
  + performance reporting
```

这不是一个单纯的 factor notebook。
它更像一个轻量版 multi-strategy hedge fund simulator。

## Why Magents matters

做量化研究，很多时候最容易犯的错误是：

```text
只做 signal，不做 execution
只做 return curve，不做 order lifecycle
只看 Sharpe，不看 position/risk/exposure
只做单策略，不做组合层和多策略冲突
```

`Magents` 的价值在于，它把策略研究从：

```text
signal column -> backtest curve
```

推进到：

```text
market data event
  -> strategy pod
  -> signal agent
  -> execution agent
  -> order event
  -> order book
  -> fill event
  -> portfolio update
  -> risk validation
  -> performance metrics
```

这对我们做 `Pengyi Quant Research OS` 很重要。

因为真正的 Research OS 不能只记录：

```text
这个因子 IC 很高。
```

它还要记录：

```text
这个因子如何变成交易信号？
如何下单？
仓位怎么定？
风控如何限制？
交易成本如何扣？
多策略是否共享资金？
max drawdown 怎么处理？
```

`Magents` 正是在补这个执行和仿真层。

## Main architecture

当前 `Magents` 的核心结构可以分为七层：

```text
data/
  -> DataFeed, CSVDataFeed, InMemoryDataFeed, DataManager

core/
  -> Event, EventQueue, Order, OrderBook, Portfolio, BacktestingEngine

pods/
  -> BasePod, MultiAgentPod, SignalAgent, ExecutionAgent, RiskAgent

pods/strategies/
  -> moving average, value, sentiment, congressional trading

risk/
  -> RiskManager, PositionLimit, ExposureLimit, DrawdownLimit, LeverageLimit

utils/
  -> config, visualization, reporting

agents/ and graph/
  -> LLM analyst style pipeline, partly separate from the event-driven core
```

我会把它画成：

```text
DataManager
  -> MarketDataEvent
  -> BacktestingEngine
  -> Strategy Pods
  -> SignalAgent
  -> SignalEvent
  -> ExecutionAgent
  -> OrderEvent
  -> RiskManager.validate_order
  -> OrderBook
  -> FillEvent
  -> PortfolioManager
  -> Metrics and Reports
```

这个链路比普通 notebook 更接近真实交易系统。

## Event-driven core

`core/event.py` 定义了系统事件。

当前事件类型包括：

```text
MARKET_DATA
ORDER
FILL
SIGNAL
RISK
SYSTEM
```

对应的 dataclass 包括：

| Event | Meaning |
|---|---|
| `MarketDataEvent` | new price/volume/data point |
| `OrderEvent` | pod requests an order |
| `FillEvent` | order fill confirmation |
| `SignalEvent` | strategy signal |
| `RiskEvent` | risk alert or breach |
| `EventQueue` | FIFO event queue |

这套设计的意义是：

```text
策略不直接改组合
策略不直接成交
策略发信号和订单
engine 统一处理事件
```

这非常重要。

如果策略直接改 portfolio，就很难统一加：

```text
commission
slippage
risk validation
order status
fill delay
multi-strategy fund accounting
```

事件驱动架构把这些都放在统一入口。

## Backtesting engine

`core/engine.py` 是核心。

主循环大概是：

```text
while current_time <= end_date:
  market_data = data_manager.get_data_for_timestamp(current_time)
  for instrument, data in market_data:
    event_queue.put(MarketDataEvent(...))
  process all events
  record portfolio values
  current_time += one day
```

事件处理逻辑是：

```text
MARKET_DATA -> update order book, update prices, notify pods
ORDER       -> build Order, validate risk, submit to order book
FILL        -> update portfolio, notify pod
SIGNAL      -> forward signal to pod
RISK        -> notify pod and enforce actions
```

这里有几个关键点。

第一，engine 统一注册 pod：

```text
engine.register_pod(pod_id, pod)
```

每个 pod 会分到自己的 portfolio：

```text
initial_capital / number_of_pods
```

第二，engine 统一生成成交：

```text
OrderBook.update_market_data(...)
  -> fills
  -> FillEvent
```

第三，engine 会扣交易成本和滑点：

```text
commission = price * quantity * transaction_cost
slippage = price * slippage_rate
```

第四，engine 会记录 equity curve：

```text
portfolio_history
  -> per pod value
  -> COMBINED fund value
```

第五，engine 会输出常见绩效指标：

```text
total_return
annualized_return
volatility
sharpe_ratio
max_drawdown
final_value
initial_value
```

这已经足够支撑一个基础多策略回测框架。

## Order lifecycle

`core/order.py` 定义订单状态：

```text
CREATED
SUBMITTED
PARTIAL
FILLED
CANCELLED
REJECTED
```

订单类型：

```text
MARKET
LIMIT
STOP
STOP_LIMIT
```

订单方向：

```text
BUY
SELL
```

`OrderBook` 是一个模拟订单簿。

当前它做的是简化撮合：

```text
market order -> fill at current price
buy limit -> fill if current price <= limit price
sell limit -> fill if current price >= limit price
stop -> trigger when stop price reached
stop-limit -> trigger then apply limit condition
```

这不是交易所级别的 order book。
但对研究 MVP 来说已经很有用。

它让我们能从：

```text
target position
```

升级到：

```text
orders
fills
order status
commission
slippage
```

这对 future execution research 很关键。

## Portfolio layer

`core/portfolio.py` 定义了：

```text
Position
Portfolio
PortfolioManager
```

`Position` 负责单个 instrument：

```text
quantity
cost_basis
realized_pnl
unrealized_pnl
current_price
trades
```

它支持：

```text
long position
short position
partial close
full close
position reversal
market value
average price
```

`Portfolio` 负责一个 pod：

```text
cash
positions
transactions
total_value
total_pnl
exposure
positions summary
```

`PortfolioManager` 负责多个 pod 和 fund-level 组合：

```text
pod portfolios
combined portfolio
process_fill
update_market_prices
pod allocations
exposure by instrument
performance summary
```

这个设计对多策略非常重要。

因为多策略系统不是只有一个账户。
它经常是：

```text
pod A: equity long/short
pod B: event-driven
pod C: value
pod D: sentiment
combined book: total exposure and fund value
```

`Magents` 至少把这个层次抽出来了。

## Risk layer

`risk/` 是 `Magents` 很值得学的一层。

它有一个抽象类：

```text
RiskLimit
```

每个 limit 都要实现：

```text
validate_order(order, portfolio)
validate_portfolio(portfolio)
breach_details(portfolio)
```

当前实现了四类：

| RiskLimit | Meaning |
|---|---|
| `PositionLimit` | maximum position size |
| `ExposureLimit` | maximum market value exposure |
| `DrawdownLimit` | maximum drawdown |
| `LeverageLimit` | gross exposure / equity cap |

`RiskManager` 有两种 limit：

```text
pod-specific limits
global limits
```

它在 order 进入 order book 前做：

```text
risk_manager.validate_order(order, portfolio)
```

如果违反：

```text
order.status = REJECTED
pod.on_order_status(order)
```

它也能监控 portfolio，生成 `RiskEvent`：

```text
monitor_portfolio
monitor_all_portfolios
```

如果是 critical risk event，engine 里有动作：

```text
drawdown breach -> close all positions
position limit breach -> reduce position by 50%
```

这说明 `Magents` 的风控不是事后报表。
它已经被放进交易事件链路里。

## Data layer

`data/management.py` 和 `data/feeds/base.py` 构成数据层。

当前 feed 抽象是：

```text
DataFeed
  -> load_data(start_date, end_date, instruments)
  -> get_available_instruments()
  -> preprocess_data()
```

内置两个 feed：

```text
CSVDataFeed
InMemoryDataFeed
```

`DataManager` 负责：

```text
register_feed
load_data
get_data_for_timestamp
get_historical_data
get_latest_data
calculate_indicator
merge_feeds
resample_data
```

这里有一个设计点很适合我们：

```text
backtesting engine should not care where data comes from
```

engine 只需要：

```text
timestamp -> {instrument -> data}
```

未来如果我们接：

```text
data-mcp
WorldQuant-like data
CSV
database
live API
factor store
```

都应该先变成同一种 data event。

## Pod and agent design

`pods/base.py` 把 trading team 抽象成 pod。

```text
BasePod
  -> initialize
  -> on_market_data
  -> on_order_status
  -> on_order_fill
  -> on_signal
  -> on_risk_event
  -> send_order
  -> send_signal
```

`MultiAgentPod` 里面可以挂多个 agent：

```text
signal agent
execution agent
risk agent
```

`pods/agents/base_agent.py` 又定义了：

```text
BaseAgent
SignalAgent
ExecutionAgent
RiskAgent
```

我认为这是 `Magents` 最值得借鉴的地方之一。

因为真正的策略不是一个函数。
一个策略至少包括：

```text
signal generation
position sizing
execution rule
risk response
state tracking
```

`Magents` 把它们拆成 agent role。

这让我们可以把一个 factor hypothesis 落成：

```text
FactorSignalAgent
FactorExecutionAgent
FactorRiskAgent
FactorPod
```

这个结构对我们后续实现自动回测很实用。

## Strategy factory

`pods/strategies/factory.py` 是策略注册层。

它定义的策略类型：

```text
equity_long_short
long_biased
event_driven
macro
quant
multi_strategy
```

当前 factory lazy-load 的内置策略是：

| Key | Module | Type |
|---|---|---|
| `ma` | moving average crossover | equity_long_short |
| `congress` | congressional trading | event_driven |
| `value` | value investing | long_biased |
| `sentiment` | sentiment trading | quant |

这里要注意一个现实状态：

README 的策略表列了更多策略：

```text
buffett
graham
munger
wood
ackman
druckenmiller
fundamentals
```

这些文件在 repo 里也能看到。
但当前 factory 默认注册的是上面四个。

所以文章里不能简单说“所有 README 策略都已经接入统一 factory”。
更准确的判断是：

```text
strategy examples exist broadly
factory-integrated event-driven pods are currently narrower
```

这也是一个 PR opportunity。

## Concrete strategy pattern

看四个已接入 factory 的策略，会发现模式很一致。

### Moving average

`MovingAveragePod` 由两个 agent 组成：

```text
MovingAverageSignalAgent
MovingAverageExecutionAgent
```

signal agent 做：

```text
store historical prices
calculate fast MA and slow MA
detect bullish/bearish crossover
send LONG or SHORT signal
```

execution agent 做：

```text
on LONG:
  close short if needed
  open long

on SHORT:
  close long if needed
  open short
```

这是一套非常清晰的 demo strategy。

### Value investing

`ValueInvestingPod` 内部是 Graham 风格：

```text
GrahamSignalAgent
GrahamExecutionAgent
```

signal 逻辑看：

```text
EPS
current ratio
debt-to-equity
book value
Graham Number
margin of safety
```

execution 逻辑偏 long-biased：

```text
LONG -> open long
SHORT -> close long but generally avoid shorting
EXIT -> close position
```

### Sentiment trading

`SentimentTradingPod` 看：

```text
insider trades
news sentiment
social sentiment
```

如果没有真实 sentiment data，会使用基于近期价格变化的 demo 逻辑。

execution 里有：

```text
max positions
pyramiding
position sizing by signal strength
entry price tracking
```

### Congressional trading

`CongressionalTradingPod` 是 event-driven demo。

signal 端当前是 mock policy/congressional analysis。

它体现的是一种事件驱动框架：

```text
policy signal
government spending
regulatory change
congressional insider behavior
```

这类策略如果接入真实数据，会很适合作为 event-driven pod。

## LLM analyst layer

除了 `pods/` 和 `core/`，repo 里还有一套 `src/agents`、`src/graph`、`src/llm`、`src/tools`。

这里更像是一个 LLM analyst hedge fund pipeline。

能看到这些 agent：

```text
ben_graham
bill_ackman
cathie_wood
charlie_munger
warren_buffett
stanley_druckenmiller
fundamentals
sentiment
technicals
valuation
risk_manager
portfolio_manager
```

这些 agent 使用：

```text
AgentState
langchain_core.messages
ChatPromptTemplate
call_llm
Pydantic output
financialdatasets.ai API
```

`portfolio_manager.py` 会把 analyst signals 变成：

```text
buy
sell
short
cover
hold
```

并输出：

```text
quantity
confidence
reasoning
```

这个方向本身很有价值：

```text
LLM analyst team -> portfolio manager -> trading decisions
```

但从工程状态看，它和事件驱动 `core/engine.py` 还不是完全统一的一套系统。

我看到的现象是：

```text
event-driven pod framework
  -> src/core, src/pods, src/risk, src/data

LLM analyst framework
  -> src/agents, src/graph, src/llm, src/tools, src/backtester.py
```

两者有重叠，但还需要收敛。

这对我们也有启发：

```text
LLM analyst should become signal generator
event-driven engine should remain execution simulator
```

也就是说，不应该让 LLM agent 直接绕过 engine 去改组合。
更好的架构是：

```text
LLM analyst output
  -> SignalEvent
  -> ExecutionAgent
  -> OrderEvent
  -> RiskManager
  -> OrderBook
  -> PortfolioManager
```

## Backtester.py as legacy path

`src/backtester.py` 是另一条回测路径。

它按交易日循环：

```text
prefetch data
for each business day:
  get current prices
  call agent
  parse decisions
  execute buy/sell/short/cover
  update portfolio value
  print table
  update metrics
```

它支持：

```text
long
short
cover
cash
margin_used
realized gains
Sharpe
Sortino
max drawdown
win rate
win/loss ratio
consecutive wins/losses
```

这条路线也有价值。
它更像一个 LLM hedge fund backtester。

但和 `core/engine.py` 相比，它的问题是：

```text
agent directly returns decisions
trade execution is embedded in Backtester
event/order/risk abstraction is weaker
dependency surface is wider
```

所以我会把它理解为：

```text
useful legacy/prototype path
```

不是未来最干净的主架构。

未来更好的收敛方式是：

```text
Backtester agent decision loop
  -> refactor into SignalAgent / PortfolioManagerAgent
  -> emit SignalEvent
  -> use core BacktestingEngine for execution
```

## Dependency watch point

这里有一个非常实际的问题。

`pyproject.toml` 声明的运行依赖主要是：

```text
numpy
pandas
matplotlib
pyyaml
```

reporting 依赖：

```text
jinja2
```

但代码里实际还有：

```text
langchain_core
langchain_openai
langchain_anthropic
langchain_deepseek
langchain_google_genai
langchain_groq
langgraph
questionary
colorama
requests
python-dateutil
financialdatasets.ai API
```

这说明当前 repo 存在：

```text
declared dependencies and imported dependencies are not fully aligned
```

如果我们要使用它，必须分清：

```text
core backtesting path
  -> lighter dependency set

LLM analyst path
  -> wider dependency set and API keys
```

这不是否定项目。
这是工程使用时必须知道的边界。

## Testing status

本地测试文件主要覆盖：

```text
event queue
order book
portfolio and position accounting
risk limits
config manager
```

测试模块包括：

```text
test_event.py
test_order.py
test_portfolio.py
test_risk.py
test_config.py
```

我在本机尝试运行：

```text
py -m pytest tests
```

当前环境返回：

```text
No module named pytest
```

所以我没有在本机跑通 Magents test suite。

但从测试文件可以看出，项目对 core layer 的最小行为有覆盖：

```text
FIFO event queue
market and limit order fills
position PnL
short PnL
commission deduction
position limits
drawdown limits
pod status halt/reset
YAML config loading
```

如果未来我们要提 PR，可以优先补：

```text
engine integration tests
strategy factory tests
DataManager timestamp tests
RiskManager monitor_portfolio tests
LLM analyst dependency tests
README strategy registry consistency tests
```

## Relationship with previous layers

现在把 001 到 004 串起来：

```text
data-mcp
  = evidence access layer

skills
  = finance workflow routing layer

quant-mind
  = financial knowledge structuring layer

Magents
  = strategy execution and simulation layer
```

这条链路可以变成：

```text
paper/news/report
  -> data-mcp retrieves evidence
  -> quant-mind structures knowledge
  -> R&D Agent derives factor hypothesis
  -> skills routes to backtest workflow
  -> Magents runs strategy simulation
  -> Research OS records metrics and diagnosis
  -> human PM reviews
```

这个组合已经很接近我们想做的 AI quant research loop。

## How to connect QuantMind and Magents

`quant-mind` 输出的是 knowledge artifact。

比如：

```text
PaperKnowledgeCard
Factor
Thesis
News
```

`Magents` 需要的是可执行策略组件：

```text
SignalAgent
ExecutionAgent
RiskAgent
Pod
Config
DataFeed
```

所以中间需要一个 translation layer。

我会叫它：

```text
StrategySpec
```

它应该包含：

```text
strategy_name
asset_class
universe
required_data_fields
signal_formula
signal_frequency
rebalance_frequency
position_sizing
execution_rule
risk_limits
transaction_cost_model
slippage_model
evaluation_metrics
citations
source_knowledge_ids
```

然后生成：

```text
StrategySpec
  -> SignalAgent implementation
  -> ExecutionAgent implementation
  -> RiskLimit config
  -> Backtest config
  -> Research OS experiment record
```

这就是从 research knowledge 到 executable simulation 的桥。

## Where Magents fits in Pengyi Research OS

我会把 `Magents` 放在 Research OS 的执行仿真层。

```text
Research Material
  -> QuantMind Knowledge Objects
  -> Factor / Strategy Hypothesis
  -> StrategySpec
  -> Magents Pod
  -> Backtest Engine
  -> Metrics / Risk / Drawdown
  -> Bias Diagnosis
  -> Next Research Plan
```

其中 Magents 负责：

```text
market event generation
signal execution
order lifecycle
risk limits
portfolio accounting
equity curve
performance metrics
visualization
```

Research OS 负责：

```text
experiment id
data version
hypothesis source
config snapshot
run logs
metrics summary
diagnosis
PM decision
```

这两个系统边界要分清。

Magents 不应该承担全部 research memory。
Research OS 也不应该自己重写撮合和 portfolio accounting。

## Strengths

我认为 `Magents` 当前最值得学习的点有六个。

第一，事件驱动核心是对的。

```text
MARKET_DATA -> SIGNAL -> ORDER -> FILL -> PORTFOLIO -> RISK
```

第二，pod abstraction 很适合多策略。

```text
each strategy is an independent pod
each pod can have multiple agents
```

第三，signal 和 execution 分离。

这让策略思想和交易执行可以独立迭代。

第四，风控在下单前介入。

```text
validate_order before order book
```

这比事后 risk report 更接近真实系统。

第五，组合层支持 pod-level 和 combined-level。

这让它可以自然表达 multi-strategy fund。

第六，example 能自动生成 mock data。

这对教学和快速实验很实用。

## Watch points

当前也有一些需要谨慎的地方。

第一，README 和 factory 的策略数量不完全一致。

README 展示很多策略，但 factory 当前默认注册 4 个。

第二，事件驱动 core 和 LLM analyst layer 还没有完全统一。

`src/backtester.py` 和 `src/core/engine.py` 是两条不同路线。

第三，依赖声明不完整。

很多 LLM/langchain/questionary/colorama/requests 依赖没有出现在 `pyproject.toml` 的主依赖中。

第四，策略逻辑不少是 demo/mock。

比如 congressional trading 和 sentiment 部分需要真实数据接入才能变成可严肃评估的策略。

第五，engine 时间推进目前是 daily step。

```text
current_time + timedelta(days=1)
```

这适合日频 baseline，不适合直接拿来做高频或 intraday。

第六，订单簿是简化撮合。

它适合研究 MVP，不等于真实交易执行模拟。

第七，风险监控目前需要进一步接进主循环。

`RiskManager` 有 `monitor_portfolio`，但主 engine 的核心循环里更明显的是 order validation。后续可以加强 periodic risk monitoring。

## PR opportunities

如果我们未来要给 `Magents` 提 PR，我会优先考虑这些方向。

### 1. Dependency cleanup

把 LLM analyst 依赖拆成 optional extra：

```text
magents[core]
magents[llm]
magents[reporting]
```

这样用户可以只装回测核心。

### 2. Factory and README consistency

让 README 策略表和 factory registry 对齐。

如果某些策略还不是 `MultiAgentPod`，就标注：

```text
available as analyst function
not yet integrated as event-driven pod
```

### 3. Engine integration test

新增一个最小端到端测试：

```text
InMemoryDataFeed
MovingAveragePod
RiskManager
BacktestingEngine
assert equity curve exists
assert orders/fills happen
```

这会显著提高项目可信度。

### 4. LLM analyst to SignalEvent bridge

把 `src/agents` 的输出统一转成 `SignalEvent`。

```text
analyst signal
  -> SignalEvent
  -> ExecutionAgent
```

这样两个系统就能合并。

### 5. StrategySpec schema

加一个 schema，把研究假设转成策略定义。

这对和 `quant-mind` 对接非常关键。

```text
FactorKnowledge
  -> StrategySpec
  -> Magents Pod
```

## Pengyi implementation plan

基于 `Magents`，我自己的 Research OS 可以先做一个很小的 MVP。

### Phase 1: Minimal factor pod

实现：

```text
FactorSignalAgent
FactorExecutionAgent
FactorPod
```

输入：

```text
factor score by date and instrument
price data
universe
```

输出：

```text
LONG top quantile
SHORT bottom quantile
EXIT neutral
```

### Phase 2: Connect StrategySpec

定义一个 `StrategySpec`：

```text
required data
signal rule
rebalance rule
position sizing rule
risk limits
metrics
```

然后自动生成 Magents-compatible pod。

### Phase 3: Research OS record

每次 backtest 保存：

```text
experiment_id
source_knowledge_id
strategy_spec
data_range
universe
config
equity_curve
metrics
risk events
diagnosis
next plan
PM decision
```

### Phase 4: Bias diagnosis

回测后自动检查：

```text
look-ahead bias
survivorship bias
transaction cost sensitivity
turnover
concentration
exposure drift
regime sensitivity
sample length
data quality
```

这样 `Magents` 就不只是回测器。
它会成为 R&D Agent 的实验执行环境。

## One useful mental model

如果说 `quant-mind` 像 compiler front-end：

```text
raw research material -> structured AST
```

那 `Magents` 就像 simulation runtime：

```text
strategy AST
  -> executable pod
  -> event loop
  -> state transitions
  -> metrics
```

更完整地看：

```text
data-mcp        = evidence IO
skills          = workflow router
quant-mind      = research knowledge compiler
Magents         = trading simulation runtime
Research OS     = experiment ledger and governance layer
R&D Agent       = planner and iterative optimizer
Human PM        = final reviewer
```

这个模型非常清楚。

我们不是只要一个能聊天的 agent。
我们要的是一个能把 research idea 一路推进到 simulation evidence 的系统。

## LLMQUANT004 conclusion

`Magents` 对我的启发是：

```text
量化 agent 系统不能停在 idea generation。
它必须进入 event-driven simulation, order lifecycle, portfolio accounting, and risk control.
```

它当前还不是完全成熟的 production trading stack。
但它已经给出了一个值得学习的骨架：

```text
Strategy Pod
  -> SignalAgent
  -> ExecutionAgent
  -> OrderEvent
  -> RiskManager
  -> OrderBook
  -> FillEvent
  -> PortfolioManager
  -> Metrics
```

这正好是我们 `Pengyi Quant Research OS` 缺的执行仿真层。

下一篇：

```text
LLMQUANT005 -> awesome-trading-agents
```

重点看 trading agent ecosystem map，整理外部项目如何做 data、agent、strategy、backtest、portfolio、research automation。
