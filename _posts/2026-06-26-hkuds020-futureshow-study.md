---
title: "HKUDS020: FutureShow 作为 Forecasting Agent Benchmark 与 Quant Judgment Layer"
date: 2026-06-26 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds020, hkuds, futureshow, forecasting, prediction-market, polymarket, quant-os, agent-benchmark, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第二十一篇。
```text
HKUDS020 -> FutureShow
```

上一张地图 `HKUDS00000 StudyMap3` 已经把 `HKUDS020` 之后的路线定下来：

```text
Forecasting -> VideoRAG -> FastCode -> OpenSpace -> Graph -> Recommendation -> Spatiotemporal -> Agent Product
```

所以 `FutureShow` 是第三阶段的第一站。
它不是普通的 trading demo，也不是单纯的 leaderboard。
它真正重要的地方在于：

```text
FutureShow = Forecasting Agent Benchmark + Prediction Market Arena + Quant Judgment Layer
```

也就是说，它把 AI agent 的判断能力放到真实未来事件上，用 prediction market 的 crowd price 作为基准，然后持续记录：

```text
agent 什么时候预测
预测了什么
当时市场价格是多少
最后结果是什么
agent 是否比市场共识更有价值
```

这对我们的 `Pengyi Quant Research OS` 非常关键。
因为量化研究的本质不是“写了一个因子”，而是：

```text
在某个时间点提出一个可检验的判断，
用未来结果验证它，
并长期积累可审计的 track record。
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `FutureShow`。

| Item | Value |
|---|---|
| repo | `FutureShow` |
| remote | `https://github.com/HKUDS/FutureShow.git` |
| branch | `main` |
| local head | `d86041f` |
| full commit | `d86041f04ec729d8a0923ad8653e78319b12de2a` |
| latest local commit date | `2026-01-26 21:07:06 +0800` |
| latest local commit | `Fix typos in README CN` |
| status | clean, synced with `origin/main` after fetch |
| package name | `futureshow` |
| package version | `0.1.0` |
| Python requirement | `>=3.10` |
| license | `MIT` |
| tracked files by `git ls-files` | 81 |
| Python files | 44 |
| tests tracked files | 13 |
| syntax check | `py -m compileall -q ...` passed |
| metadata check | `pyproject.toml` parsed by `tomllib` |
| import smoke | `import futureshow` passed |

核心依赖很清楚：

```text
openai-agents[litellm]
fastmcp
python-dotenv
requests
rich
```

也就是说，它的 agent 运行栈主要是：

```text
OpenAI Agents SDK
+ LiteLLM model routing
+ MCP-style tools
+ Polymarket data / CLOB
+ local JSONL state
+ frontend dashboard
```

## 它解决什么问题

`FutureShow` 的 README 里把问题问得很直接：

```text
Can AI Predict the Future?
```

但工程上它不是在问一个玄学问题。
它把这个问题拆成了可运行、可记录、可比较的系统：

```text
AI model agents
  -> read market/event context
  -> gather web/news/social evidence
  -> make YES/NO/ABSTAIN predictions
  -> store timestamped forecasts
  -> compare with market consensus
  -> evaluate when outcomes resolve
```

这里最重要的是 prediction market。
Prediction market 的价格可以被理解为 crowd consensus：

```text
YES price ~= 市场认为 YES 发生的概率
NO price ~= 市场认为 NO 发生的概率
```

如果 agent 只是预测正确，不一定有价值。
因为市场可能本来就认为这件事 95% 会发生。
真正有价值的是：

```text
agent 是否在市场低估时预测正确
agent 是否在市场高估时避开错误
agent 是否长期产生超过 crowd baseline 的判断质量
```

这就是 `FutureShow` 对 quant 的直接启发。
在量化里，我们不应该只问：

```text
策略有没有赚钱？
```

还要问：

```text
它有没有超过合理 benchmark？
它有没有在不确定性里提供真实增量信息？
它的判断是否可复现、可审计、可长期累计？
```

## 系统总架构

`FutureShow` 可以拆成七层：

| Layer | Role |
|---|---|
| Market Universe | 从 Polymarket 拉取 event / market / price / order book |
| Watchlist | 维护当前要预测的一组事件 |
| Forecast Agent | 每个模型独立读取事件并输出结构化预测 |
| Evidence Tools | web search, news search, URL text, Reddit, Twitter |
| Forecast Ledger | 把预测写入 `forecasts.jsonl` |
| Tracker | 持续记录市场状态、最终结果和 ground truth |
| Dashboard | 展示 accuracy、human baseline、prediction value、history |

整体链路可以这样看：

```text
Polymarket events
  -> watchlist
  -> model-specific forecast agents
  -> tool-augmented evidence gathering
  -> <PREDICTION>YES/NO/ABSTAIN</PREDICTION>
  -> forecasts.jsonl
  -> tracking.jsonl / result.json
  -> dashboard metrics
```

这已经很接近一个 `judgment OS`。
每个判断都不再是聊天记录里的自然语言，而是一个可评分对象。

## 两种 Agent 模式

这个 repo 里有两个核心 agent。

| Agent | File | Role |
|---|---|---|
| `PolymarketForecastAgent` | `futureshow/agent/polymarket/polymarket_forecast_agent.py` | 只做预测，不交易 |
| `PolymarketAgent` | `futureshow/agent/polymarket/polymarket_agent.py` | 做模拟交易、持仓、PnL |

这两个模式的分离非常重要。

### Forecast-only Agent

`PolymarketForecastAgent` 的目标是：

```text
give one explicit forecast
do not trade
record reasoning and prediction
```

它会读取 watchlist，每个 event 构造上下文：

```text
event slug
event title
close time
description
markets
outcomes
current prices
recent forecasts
```

然后调用工具：

```text
google_web_search
google_news_search
google_url2text
search_tweets
reddit_search
reddit_post_details
```

最后输出结构化标签：

```text
<PREDICTION>market-slug|YES</PREDICTION>
<PREDICTION>YES</PREDICTION>
<PREDICTION>ABSTAIN</PREDICTION>
```

这个设计非常好。
因为它把 LLM 的自然语言输出压缩成一个可解析的决策对象。

### Trading Agent

`PolymarketAgent` 更接近交易系统。
它有更多工具：

```text
list_events
get_polymarket_info_by_slug
get_market_prices
get_market_history
buy
sell
google_url2text
google_web_search
google_news_search
search_tweets
reddit_search
reddit_post_details
```

它不仅要判断事件，还要考虑：

```text
capital allocation
position
liquidity
slippage
PnL
risk
no-trade decision
```

这对应到 quant 系统里，就是：

```text
research signal
  -> portfolio decision
  -> execution simulation
  -> position tracking
  -> PnL attribution
```

所以 `FutureShow` 的一个重要启发是：

```text
judgment layer 和 execution layer 应该分开。
```

先有 forecast ledger，再有 trading ledger。
否则系统很容易把“判断质量”和“交易执行效果”混在一起。

## Forecast Ledger

预测结果会写到：

```text
data/forecasts/<signature>/<event_slug>/forecasts.jsonl
```

单条记录大致包含：

```text
timestamp
signature
event_slug
event_title
forecast
predictions
market_prob
```

这里的 `signature` 通常对应一个模型或 agent 身份。
例如：

```text
gpt-5
deepseek
gemini
claude
```

这意味着每个模型都有自己的独立预测历史。
这点对 benchmark 很重要：

```text
同一个 event
不同模型
同一个时间点附近
不同 forecast
最后统一结算
```

在我们的 Quant Research OS 里，可以直接迁移这个设计：

```text
data/research_forecasts/<agent>/<hypothesis_id>/forecasts.jsonl
```

每条 factor hypothesis 都应该被记录为：

```text
timestamp
universe
signal definition
expected direction
expected horizon
baseline
confidence
evidence
future realized outcome
```

这样研究才有 track record。

## Watchlist 与 Tracker

`FutureShow` 有一个非常实用的 watchlist 层。
默认路径来自：

```text
futureshow/utils/polymarket_watchlist.py
```

它会维护当前要预测的 event 集合，并支持：

```text
fetch active events
filter closed events
category balancing
manual slugs
month/year filtering
remove resolved events
```

然后 tracker 会持续记录市场状态：

```text
tracking.jsonl
result.json
```

这对 quant 的启发也很直接：

```text
watchlist = research universe
tracker = outcome labeling engine
```

很多量化研究失败，不是因为没有模型，而是因为没有稳定地管理：

```text
当前研究 universe 是什么
每个样本什么时候进入观察
什么时候退出
最终标签如何确定
期间价格和状态如何变化
```

`FutureShow` 这里的 watchlist / tracker 思路可以迁移成：

```text
FactorUniverse
SignalWatchlist
EventOutcomeTracker
BacktestLabelStore
```

## Metrics

`FutureShow` 最值得学的是评价指标。

它不只看模型预测对不对，还看模型相对市场有没有 value。

### Accuracy

最基础指标：

```text
accuracy = correct / total
```

这个指标简单，但不够。
因为如果一个事件市场价格已经是 98%，预测 YES 并不代表很强。

### Human Acc

`FutureShow` 用市场价格构造 human baseline：

```text
if market probability > 50%:
    human prediction = YES
else:
    human prediction = NO
```

然后比较 agent 与 market consensus 在相同预测点的表现。

```text
vs Human = model accuracy - human accuracy
```

这一步非常重要。
它告诉我们：

```text
agent 不是和空气比，而是和市场共识比。
```

这在 quant 里对应：

```text
strategy 不是和 0 比，而是和 benchmark / risk model / market-implied expectation 比。
```

### Prediction Value

更关键的是 `Pred Value`。
代码里使用 log-return 风格的 scoring：

```text
if correct:
    value = -log(p)
else:
    value = log(p)
```

其中 `p` 是 agent 所预测 outcome 在预测时的市场概率。

这意味着：

```text
低概率事件预测正确 -> 高正 value
高概率事件预测正确 -> 小正 value
高概率事件预测错误 -> 大负 value
低概率事件预测错误 -> 小负 value
```

这比 accuracy 更接近真实市场判断。
因为市场里真正有价值的不是“预测大众已经知道的事”，而是：

```text
在价格没有充分反映时，发现 edge。
```

对我们自己的 Quant OS 来说，这个指标可以迁移成：

```text
signal information value
market-implied baseline adjusted score
forecast edge score
```

一个 factor 不是只看命中率，而要看：

```text
它在哪些市场状态下提供了额外信息？
它有没有超过已有 baseline？
它的错误是否集中在高置信、高风险位置？
```

## Dashboard

预测 dashboard 由：

```text
web_server_pred.py
frontend/
```

驱动。

它展示：

```text
model leaderboard
accuracy
human accuracy
vs market
prediction value
event list
event detail
forecast history
ground truth
```

这说明 `FutureShow` 不只是 benchmark script。
它已经把 forecast 变成了可浏览、可解释、可对外展示的 artifact。

对我们来说，这点很重要。
未来 Quant Research OS 不能只在命令行里跑出一个 backtest。
它应该有一个 dashboard：

```text
factor hypothesis ledger
forecast history
backtest result
bias diagnostics
model comparison
PM review status
next experiment plan
```

`FutureShow` 的 dashboard 是一个很好的参考。

## Quickstart 工作流

README 里的核心命令可以分成三组。

### Forecast Benchmark

```bash
python run_forecast_loop.py --once
python run_forecast_loop.py --refresh --interval 21600
python run_forecast_loop.py --limit 4 --month 1 --year 2025 --refresh
```

### Forecast Tracker / Dashboard

```bash
python run_forecast_trackers.py --interval 1800
python web_server_pred.py
```

### Trading Mode

```bash
python main.py configs/default_config.json
python run_agents_loop.py --interval 2400 --overrun-pause 900 --config configs/default_config.json
python run_pnl_trackers.py --interval 10 --config configs/default_config.json
python web_server.py
```

这三个入口刚好对应：

```text
forecast generation
forecast evaluation
trading simulation
```

系统边界比较清楚。

## MCP Tools

它的工具层也很值得学。

Market data:

```text
list_events
list_markets
get_polymarket_info_by_slug
get_market_prices
get_market_history
```

Evidence gathering:

```text
google_web_search
google_news_search
google_url2text
reddit_search
reddit_post_details
search_tweets
```

Trading:

```text
buy
sell
settle
```

Utility:

```text
math_tool
```

这套工具结构说明一个现实问题：

```text
forecast agent 不能只靠模型内部知识。
它必须能主动获取最新证据。
```

对 quant 来说也是一样。
Research agent 如果要做真实判断，必须接入：

```text
price data
fundamental data
news
filings
research reports
factor library
backtest engine
portfolio/risk engine
```

否则它只能做 paper discussion，不能做 production-grade research。

## 和 Quant Research OS 的关系

`FutureShow` 和我们的 Quant OS 是高度同构的。

| FutureShow | Quant Research OS |
|---|---|
| event | stock / asset / macro event / factor sample |
| market probability | market-implied expectation / benchmark |
| forecast agent | research agent / signal agent |
| prediction | factor hypothesis / directional call |
| watchlist | universe |
| result.json | realized label |
| forecasts.jsonl | research decision ledger |
| accuracy | hit rate / classification score |
| prediction value | information value / alpha edge |
| dashboard | research review panel |

这篇最大的启发是：

```text
把 research hypothesis 当成 forecast object 管理。
```

例如一个因子研究可以这样记录：

```text
hypothesis_id: momentum_reversal_001
timestamp: 2026-06-26
universe: CSI 800
horizon: 20 trading days
claim: high short-term reversal score predicts next-period excess return
baseline: sector-neutral equal-weight benchmark
expected outcome: top quantile outperforms bottom quantile
confidence: 0.63
evidence: prior IC, decay, turnover, economic logic
status: pending
```

未来结果出来后，再补：

```text
realized_ic
return_spread
drawdown
turnover
cost-adjusted return
bias diagnostics
final score
```

这就是 `FutureShow` 给我们的系统思想。

## 和 R&D Agent 的关系

我们之前一直在说：

```text
R&D Agent for Quant Research
= 自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ 人类 PM 审核
```

`FutureShow` 正好补上其中一个底层结构：

```text
forecast ledger
```

R&D Agent 不应该只是不断生成想法。
它应该长期维护：

```text
idea ledger
hypothesis ledger
experiment ledger
forecast ledger
decision ledger
review ledger
```

否则 agent 生成再多内容，也会变成不可追踪的噪声。

`FutureShow` 告诉我们：

```text
每一次判断都要 timestamped。
每一次判断都要有 baseline。
每一次判断都要等待未来结算。
每一次判断都要进入长期评分。
```

这是从 chat agent 走向 research organization 的关键一步。

## 和前面 HKUDS 项目的连接

`FutureShow` 和前面很多项目可以串起来。

| Previous Repo | Connection |
|---|---|
| `LightRAG` / `MiniRAG` | 为 forecast agent 提供长期知识记忆 |
| `RAG-Anything` | 把 PDF、报告、图表转成可检索证据 |
| `VideoRAG` | 后续可把访谈、课程、讲座进入 forecast evidence |
| `Vibe-Trading` | 把 forecast 转成 strategy research workflow |
| `AI-Trader` | 把 forecast / edge 接到 live trading platform |
| `AnyTool` | 扩展 forecast agent 的工具调用能力 |
| `OpenHarness` | 管理多 agent benchmark 和运行环境 |
| `Paper2Slides` | 把 forecast benchmark 结果生成 presentation artifact |

所以 `FutureShow` 不是孤立项目。
它是 Quant / Forecasting 主线上的关键一块：

```text
knowledge
  -> evidence
  -> judgment
  -> forecast ledger
  -> evaluation
  -> trading / research decision
  -> presentation
```

## 对个人网站的启发

我们的网站现在记录了很多学习笔记。
`FutureShow` 提醒我们，网站也可以记录一种更强的内容：

```text
public research forecast
```

不是随便预测，而是结构化预测：

```text
我认为某个技术方向会如何发展
某个 project 的长期价值在哪里
某个 research stack 是否会成为主流
某个 quant agent 方向是否值得投入
```

每条预测都应该有：

```text
date
claim
confidence
evidence
baseline
expected evaluation date
future review
```

这样网站就不只是 portfolio，而会逐渐变成：

```text
public thinking ledger
```

这对 AI scientist path 很有价值。
因为真正有分量的人不是只会总结别人做过什么，而是能长期留下自己的判断记录。

## PR Opportunities

这次读下来，有几个比较清楚的 issue / PR 方向。

### 1. Prediction dashboard 端口文档不一致

README 里 prediction dashboard 写的是：

```text
http://localhost:10086
```

但 `web_server_pred.py` 里默认端口是：

```python
port = int(os.environ.get("PRED_PORT", "10032"))
```

可以提一个很小的 PR：

```text
要么把 README 改成 10032
要么把 web_server_pred.py 默认端口改成 10086
要么明确用 PRED_PORT 控制
```

这是低风险、边界清晰的贡献点。

### 2. `compute_model_accuracy_from_history` 返回 tuple 后的调用疑似不一致

`compute_model_accuracy_from_history()` 返回：

```text
(model_stats, human_stats_per_model)
```

在 `build_local_events()` 里有正确用法：

```python
stats, human_stats_per_model = compute_model_accuracy_from_history(...)
```

但 `get_event_details()` 附近还有一处：

```python
stats = compute_model_accuracy_from_history(full_forecasts, result_winners)
for m, s in stats.items():
    ...
```

如果这段路径实际触发，`stats` 会是 tuple，`items()` 会失败。
代码外层有 `try/except`，所以可能不会让 server 直接崩，但会导致 per-event model stats 缺失或异常被吞掉。

这个 PR 也很适合：

```text
统一返回值解包
补一个最小测试
确认 event detail API 有 model stats
```

### 3. interval help text 和默认值不一致

`run_forecast_loop.py`：

```python
default=3600 * 6
help="循环间隔秒（默认 4 小时，最小 300s）"
```

实际默认是 6 小时，help 写 4 小时。

`run_forecast_trackers.py`：

```python
default=1800
help="记录间隔（秒），默认 600"
```

实际默认是 1800 秒，help 写 600。

这是很典型的 docs / CLI consistency PR。

### 4. watchlist 默认路径不一致

forecast agent 默认 watchlist 来自：

```text
polymarket_watchlist_2026_01.json
```

但 `web_server_pred.py` 里读取的是：

```text
futureshow/utils/polymarket_watchlist.json
```

这可能导致：

```text
forecast loop 用一个 watchlist
dashboard 读另一个 watchlist
```

可以考虑统一默认路径，或者让 dashboard 也读取 `DEFAULT_WATCHLIST_PATH`。

### 5. 部分 tests 仍引用旧包名 `agent_tools`

本地搜索看到一些测试还在引用：

```text
agent_tools.tool_polymarket_data
agent_tools.tool_polymarket_trade
```

但主代码现在在：

```text
futureshow/tool/
```

这说明测试可能有一部分是旧路径。
可以做一个 test maintenance PR：

```text
更新 import path
确认 pytest collection
删除或迁移旧的 compatibility references
```

### 6. `main.py` 对 `PolymarketForecastAgent` 的 registry 入口可能不适配

`main.py` 的 `AGENT_REGISTRY` 里包含：

```text
PolymarketAgent
PolymarketForecastAgent
```

但 `main.py` 的实例化路径主要传入：

```text
stock_symbols
log_path
initial_cash
init_datetime
```

这些参数更适合 `PolymarketAgent`，不适合 `PolymarketForecastAgent`。
同时后续还会调用：

```text
agent.initialize()
agent.register_agent()
```

而 forecast agent 的主要运行入口其实是：

```text
run_forecast_loop.py
run_forecast_trackers.py
```

所以这里可以提 issue：

```text
main.py 是否应该只负责 trading agent？
forecast agent 是否应该从 registry 中移出，或拆分 agent factory 参数？
```

这个 PR 稍微大一点，但也很有价值。

## 我们可以怎么吸收

不要一上来就复刻 Polymarket。
对我们来说，第一步应该是抽象它的结构：

```text
ForecastObject
ForecastLedger
Baseline
OutcomeTracker
ValueScore
Dashboard
```

然后在 Quant Research OS 里先做一个小版本：

```text
factor hypothesis forecast ledger
```

最小实现可以是：

```text
1. 每个研究想法写成 structured markdown/json
2. 每个想法有 expected outcome 和 horizon
3. 每次 backtest 结果进入 ledger
4. 每次 human PM review 进入 ledger
5. 每月回看哪些判断真实有效
```

这比直接做复杂交易系统更稳。

第二步才是接：

```text
market data
backtest engine
agent-generated next plan
dashboard
```

`FutureShow` 的核心不是 Polymarket，而是：

```text
judgment should be auditable.
```

## 对 Quant Interview / 子健哥沟通的问题启发

这篇也能直接反哺我们之后问 senior quant 的问题。

可以问：

```text
真实 quant team 如何记录 research hypothesis？
一个 PM 如何判断 researcher 的 idea quality？
除了 backtest PnL，会不会记录 hypothesis-level track record？
prediction / judgment / investment thesis 在团队里如何被审计？
数据源不完整时，junior researcher 应该如何构建可验证 demo？
```

这些问题比单纯问“怎么找量化工作”更有质量。
因为它们直接切到：

```text
quant research organization 如何管理判断。
```

这正是 `FutureShow` 给我们的启发。

## 系统位置

现在 HKUDS 的 Quant / Research OS 主线可以这样放：

| Repo | System Position |
|---|---|
| `Vibe-Trading` | agentic quant research workflow |
| `AI-Trader` | agent-native live trading platform |
| `FutureShow` | forecast benchmark and judgment ledger |
| `LightRAG` / `MiniRAG` | research memory |
| `RAG-Anything` | multimodal evidence ingestion |
| `AnyTool` / `OpenHarness` | tool execution and runtime |
| `Paper2Slides` | research presentation artifact |

如果画成链路：

```text
Evidence
  -> Knowledge
  -> Forecast
  -> Evaluation
  -> Strategy
  -> Trading
  -> Report / Presentation
```

`FutureShow` 就在最关键的中间：

```text
Knowledge -> Forecast -> Evaluation
```

没有这一层，Research OS 会变成材料整理系统。
有了这一层，Research OS 才开始有判断能力。

## 一句话总结

`FutureShow` 的价值不是“AI 预测未来”这个口号。
更准确地说：

```text
FutureShow 把 AI agent 的判断变成了 timestamped、baseline-adjusted、outcome-verifiable 的 forecast object。
```

这对我们的 `Pengyi Quant Research OS` 非常重要。
因为量化研究最终要证明的不是我们读了多少 paper，也不是我们写了多少代码，而是：

```text
我们能否持续提出比 baseline 更有信息量的判断。
```

所以这篇的核心启发是：

```text
build the research engine,
but also build the judgment ledger.
```

## Next

下一篇进入：

```text
HKUDS021 -> VideoRAG
```

如果说：

```text
FutureShow gives us forecasting.
```

那么下一步就是：

```text
VideoRAG gives us video memory.
```

也就是把访谈、课程、讲座、视频资料纳入 Research OS 的知识入口。
