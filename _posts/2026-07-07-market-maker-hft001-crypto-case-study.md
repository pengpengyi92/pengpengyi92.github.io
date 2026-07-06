---
title: "MarketMakerHFT001: Crypto Case Study for High-Frequency Market Making"
date: 2026-07-07 00:00:00 +0800
categories: [Learning, Quant Research]
tags: [market-maker-hft001, market-making, high-frequency-trading, crypto, order-book, microstructure, asq, avellaneda-stoikov, nautilus-trader, public-safe]
---

这是 `MarketMakerHFT001`。

这篇不是公开任何原始数据，也不是发布私有学习材料。

这篇做的是一件更重要的事：

```text
把一次 crypto 高频做市学习，抽象成 public-safe 的高频交易 / 做市商研究框架。
```

核心边界先说清楚：

```text
1. 这是学习笔记，不是实盘做市系统。
2. 这是以 crypto market 为案例，不是宣称我已经精通 crypto 做市。
3. 高频做市理论可以迁移到股票、期货、ETF、期权、外汇、数字货币等市场。
4. 公开版只保留通用方法、工程结构、代码片段和研究问题，不暴露原始数据、路径、私有来源名称或私有材料标识。
```

更准确的项目定位是：

```text
Crypto tick-level market making research and learning prototype.
```

英文面试表达：

```text
I studied a crypto HFT market making workflow as a learning prototype. 
The project covers tick data normalization, order book features, short-horizon price modeling, 
AS/ASQ-style quoting, inventory-aware market making, fee/rebate-aware backtesting, 
and the key gap between research backtests and production-grade HFT systems.
```

中文表达：

```text
我系统学习了一套以 crypto tick 数据为案例的高频做市研究流程，覆盖 tick 数据标准化、盘口特征工程、短期价格模型、
AS/ASQ 做市报价、库存约束、手续费/返佣回测，以及研究原型与生产级高频系统之间的差距。
```

## 1. 做市商到底在做什么

普通交易者常常想的是：

```text
买低卖高
预测方向
抓趋势
```

做市商想的是：

```text
同时挂 bid 和 ask
用 spread / rebate / order flow edge 获利
持续控制库存和逆向选择风险
```

一个最简盘口：

```text
best bid: 2999.9, size 10
best ask: 3000.1, size 12

mid price = (2999.9 + 3000.1) / 2 = 3000.0
spread    = 3000.1 - 2999.9 = 0.2
```

做市商可能同时挂：

```text
bid quote = 2999.9
ask quote = 3000.1
```

如果两边都能成交，并且价格没有明显单边冲击，就可能赚到 spread。

但现实更难：

```text
1. 买入后价格继续跌，库存亏损。
2. 卖出后价格继续涨，库存亏损。
3. 你的订单不一定排在队列前面。
4. 你的报价可能被 informed trader 打掉。
5. 高频环境里 latency、撤单、限速、交易所撮合规则都影响结果。
```

所以做市的核心不是“挂两个单就赚钱”，而是：

```text
quote placement + inventory control + adverse selection control + execution realism.
```

## 2. 为什么用 Crypto 作为第一仗

Crypto 市场适合作为高频做市学习案例，原因不是它更简单，而是它的数据和市场结构很适合练习：

```text
1. 7x24 连续交易。
2. tick 数据丰富。
3. 盘口和成交行为频繁。
4. maker / taker fee structure 清晰。
5. 永续合约有 funding、杠杆、保证金、强平等独特机制。
6. 很多交易所 API 和数据格式相对容易接触。
```

但 crypto 也有自己的复杂性：

```text
1. 极端波动更强。
2. 交易所稳定性差异大。
3. fee / rebate / funding 对收益影响很大。
4. 真实撮合、撤单限制、风控、断线恢复非常重要。
5. 市场参与者结构和传统股票/期货不同。
```

所以这篇的正确理解是：

```text
Crypto is a case study for learning market making, not the full universe of market making.
```

## 3. 高频做市系统的最小架构

一个研究型高频做市系统可以拆成九层：

```text
Raw Market Data
  -> Data Parser
  -> Standard Tick Object
  -> Local Data Catalog
  -> Feature Engineering
  -> Short-Horizon Price Model
  -> Quoting Model
  -> Backtest / Simulation
  -> Diagnostics
```

更工程化一点：

```text
1. Data Ingestion
2. Data Normalization
3. Instrument Metadata
4. Order Book State
5. Microstructure Features
6. Quote Engine
7. Inventory Engine
8. Execution Simulator
9. Risk / Metrics / Report
```

这和中低频量化最大区别是：

```text
中低频策略更关注 bar data、signal、position、portfolio。
高频做市更关注 tick、order book、queue、fill、latency、inventory、fees。
```

## 4. 数据层：从 Raw Tick 到 Standard Tick

高频做市的第一步不是模型，而是数据标准化。

一个 raw `bookTicker` 类数据通常包含：

```text
event_time
best_bid_price
best_bid_qty
best_ask_price
best_ask_qty
```

我们可以抽象成统一对象：

```python
from dataclasses import dataclass

@dataclass
class QuoteTick:
    ts_event: int
    bid_price: float
    ask_price: float
    bid_size: float
    ask_size: float

    @property
    def mid_price(self) -> float:
        return (self.bid_price + self.ask_price) / 2

    @property
    def spread(self) -> float:
        return self.ask_price - self.bid_price
```

数据 parser 的核心任务：

```python
def parse_book_ticker(row) -> QuoteTick:
    return QuoteTick(
        ts_event=int(row["event_time"]),
        bid_price=float(row["best_bid_price"]),
        ask_price=float(row["best_ask_price"]),
        bid_size=float(row["best_bid_qty"]),
        ask_size=float(row["best_ask_qty"]),
    )
```

真实工程里还要处理：

```text
1. price precision
2. quantity precision
3. timestamp timezone
4. duplicate ticks
5. missing values
6. out-of-order events
7. exchange-specific schema
8. instrument metadata
```

这一步非常关键。因为高频系统里，错误数据会直接污染回测结论。

## 5. Local Catalog：为什么要存成标准格式

直接用 CSV 也能研究，但不够工程化。

更稳的做法是：

```text
raw csv/json
  -> normalized tick objects
  -> columnar local catalog
  -> reusable backtest data source
```

常见选择：

```text
Parquet
Arrow
HDF5
DuckDB
ClickHouse
specialized backtest catalog
```

为什么偏好 columnar / catalog：

```text
1. tick 数据量大。
2. 查询特定时间范围更快。
3. schema 更稳定。
4. 便于复现实验。
5. 便于按 instrument / date / data type 管理。
```

一个简化 catalog 思路：

```python
def write_ticks_to_catalog(ticks, root, symbol, date):
    path = f"{root}/{symbol}/quote_ticks/{date}.parquet"
    frame = ticks_to_dataframe(ticks)
    frame.to_parquet(path, index=False)
```

这个步骤背后的思想是：

```text
不要把一次性 notebook 变成最终系统。
要把数据变成可复用、可追踪、可复验的研究资产。
```

## 6. 价格模型：Mid Price 不够

最基础价格：

```text
mid_price = (best_bid + best_ask) / 2
```

但 mid price 默认假设 bid/ask 两边对称，这在真实盘口里经常不成立。

例如：

```text
best bid size = 100
best ask size = 10
```

这表示买盘更厚，短期价格可能更容易向上。

于是可以构造 order book imbalance：

```python
def order_book_imbalance(bid_size, ask_size):
    denom = bid_size + ask_size
    if denom == 0:
        return 0.0
    return (bid_size - ask_size) / denom
```

再构造 adjusted mid price：

```python
def adjusted_mid_price(bid, ask, bid_size, ask_size):
    mid = (bid + ask) / 2
    spread = ask - bid
    imbalance = order_book_imbalance(bid_size, ask_size)
    return mid + spread * imbalance / 2
```

直觉：

```text
买盘更厚 -> adjusted_mid_price 高于 mid_price
卖盘更厚 -> adjusted_mid_price 低于 mid_price
```

这就是 microstructure price model 的起点。

## 7. 高频特征工程

高频特征大致分两类。

第一类：Order Book Features

```text
mid price
spread
order book imbalance
volume imbalance
VWAP around top levels
book slope
advanced OBI
price/volume changes at best bid/ask
short-horizon volatility
momentum
```

第二类：Trade Features

```text
taker buy volume
taker sell volume
gross buy ratio
net buy ratio
buy/sell concentration
trade volume rolling sum
average buy/sell trade price
trade interval distribution
```

一个简化特征函数：

```python
def build_features(df):
    df = df.copy()

    df["mid_price"] = (df["best_bid_price"] + df["best_ask_price"]) / 2
    df["spread"] = df["best_ask_price"] - df["best_bid_price"]
    df["obi"] = (
        (df["best_bid_qty"] - df["best_ask_qty"]) /
        (df["best_bid_qty"] + df["best_ask_qty"])
    )
    df["adj_mid"] = df["mid_price"] + df["spread"] * df["obi"] / 2
    df["ret_1s"] = df["mid_price"].shift(-1) / df["mid_price"] - 1
    df["vol_10s"] = df["ret_1s"].rolling(10).std()

    return df
```

真正的高频特征要更小心：

```text
1. 不能用未来信息。
2. trade 和 book tick 对齐要严格。
3. 不同交易所时间戳含义不同。
4. rolling window 不能跨越不可见未来。
5. 特征必须能在决策时刻实时获得。
```

## 8. AS / ASQ 做市模型

高频做市里一个经典框架是 Avellaneda-Stoikov market making。

它想解决的问题是：

```text
给定当前价格、波动率、市场单到达速度、风险厌恶和库存，
应该把 bid / ask 挂在哪里？
```

一个简化表达：

```text
bid_quote = reference_price - bid_spread
ask_quote = reference_price + ask_spread
```

这里的 `reference_price` 可以是：

```text
mid price
micro price
adjusted mid price
model-predicted mid price
```

关键参数：

```text
sigma: short-term volatility
A: market order arrival intensity scale
k: market order arrival decay with quote distance
gamma: risk aversion
q: current inventory
Q: inventory limit
```

简化代码：

```python
import math

def bid_spread(sigma, A, k, gamma, q):
    base = (1 / gamma) * math.log(1 + gamma / k)
    inventory_term = (2 * q + 1) / 2 * math.sqrt(
        sigma ** 2 * gamma / (2 * k * A) * (1 + gamma / k) ** (1 + k / gamma)
    )
    return base + inventory_term

def ask_spread(sigma, A, k, gamma, q):
    base = (1 / gamma) * math.log(1 + gamma / k)
    inventory_term = (2 * q - 1) / 2 * math.sqrt(
        sigma ** 2 * gamma / (2 * k * A) * (1 + gamma / k) ** (1 + k / gamma)
    )
    return base - inventory_term
```

库存的直觉：

```text
q > 0: 已经偏多，需要更积极卖出、更谨慎买入。
q < 0: 已经偏空，需要更积极买入、更谨慎卖出。
```

这就是 inventory-aware quoting。

## 9. Market Order Arrival：A 和 k 怎么估

AS/ASQ 里 `A` 和 `k` 描述的是：

```text
报价离 mid 越远，被 market order 打到的概率越低。
```

常见形式：

```text
lambda(delta) = A * exp(-k * delta)
```

其中：

```text
delta = quote distance from reference price
lambda = order arrival intensity
```

一个研究型估计方法：

```python
import numpy as np
from scipy.optimize import curve_fit

def exp_decay(x, A, k):
    return A * np.exp(-k * x)

def fit_arrival_intensity(distances, arrival_rates):
    params, _ = curve_fit(exp_decay, distances, arrival_rates)
    A, k = params
    return A, k
```

实际估计时需要：

```text
1. 按 tick size 划分 quote distance。
2. 统计不同距离上的 hit frequency。
3. 用历史窗口估计 arrival intensity。
4. 注意 regime shift 和交易时间差异。
```

这一步把理论模型接到了真实盘口数据。

## 10. ML Price Model + Market Making

传统 AS/ASQ 用当前 mid price 做报价中心。

增强版本可以用短期价格预测：

```text
features -> predicted short-horizon return -> predicted reference price
```

例如：

```python
def reference_price_with_prediction(mid_price, predicted_return):
    return mid_price * (1 + predicted_return)
```

然后做市报价：

```python
ref = reference_price_with_prediction(mid_price, pred_ret)
bid = ref - bid_spread(sigma, A, k, gamma, q)
ask = ref + ask_spread(sigma, A, k, gamma, q)
```

这条线的核心思想：

```text
如果短期预测向上，可以整体上移 bid/ask quote。
如果短期预测向下，可以整体下移 bid/ask quote。
```

但这里风险很大：

```text
1. 短期预测很容易过拟合。
2. correlation 不等于可交易收益。
3. 一点点 latency 就可能毁掉信号。
4. 预测越激进，越容易变成方向交易而不是做市。
```

所以更稳的表达是：

```text
ML is used as a reference-price adjustment layer, not as a guarantee of alpha.
```

## 11. 回测逻辑：从 Quote 到 PnL

一个简化做市回测：

```python
def simulate_market_maker(rows, params):
    q = 0       # inventory
    cash = 0.0
    pnl = []
    fees = 0.0

    for i in range(len(rows) - 1):
        row = rows.iloc[i]
        nxt = rows.iloc[i + 1]

        ref = row["reference_price"]
        bid = ref - bid_spread(params.sigma, params.A, params.k, params.gamma, q)
        ask = ref + ask_spread(params.sigma, params.A, params.k, params.gamma, q)

        # keep quotes on maker side
        bid = min(bid, row["best_bid_price"])
        ask = max(ask, row["best_ask_price"])

        buy_filled = q <= params.max_inventory and nxt["best_bid_price_min"] < bid
        sell_filled = q >= -params.max_inventory and nxt["best_ask_price_max"] > ask

        if buy_filled:
            q += 1
            cash -= bid
            fees += bid * params.maker_rebate

        if sell_filled:
            q -= 1
            cash += ask
            fees += ask * params.maker_rebate

        mark_to_market = cash + q * nxt["mid_price"]
        pnl.append(mark_to_market + fees)

    return pnl
```

这个回测框架里最值得拷打的点是：

```text
fill assumption.
```

因为真实市场里：

```text
价格触碰你的 quote，不代表你的订单一定成交。
你排在队列后面，前面可能有很多单。
你撤单可能来不及。
市场可能先打掉你，再反向移动。
```

所以研究型回测必须主动写出限制：

```text
This simplified backtest does not fully model queue position, latency, partial fills, cancel/replace behavior, or exchange matching rules.
```

## 12. Maker Rebate 和 Fee 是核心变量

做市收益不只来自 spread。

很多市场里，费用结构是：

```text
maker fee / rebate
taker fee
funding
borrow cost
exchange tier
```

Crypto 做市尤其要关注：

```text
1. maker rebate 能否覆盖 adverse selection。
2. taker fee 会不会吞掉 hedge / unwind 成本。
3. funding 对永续合约持仓的影响。
4. 交易所等级和成交量门槛。
```

有些做市原型会出现：

```text
交易本身亏钱，但加上 maker rebate 后赚钱。
```

这不是坏事，但必须谨慎：

```text
1. rebate policy 会变。
2. 成交量刷出来不等于真实收益。
3. adverse selection 可能在极端行情里吞掉 rebate。
4. 真实延迟和排队位置会影响成交质量。
```

## 13. 高频做市和普通 RAG/Agent/Quant OS 的连接

这个项目对 Research OS 的启发很强。

因为高频做市天然要求：

```text
1. 数据 lineage 清楚。
2. 回测假设清楚。
3. 参数估计清楚。
4. 每个信号是否可实时获得必须清楚。
5. 每个成交假设都必须被审计。
6. 每个实验必须能复现。
```

可以形成一个 `Market Making Research OS`：

```text
Data Card
Feature Card
Model Card
Quote Policy Card
Execution Assumption Card
Risk Card
Backtest Report
Grilling Q&A
```

这也解释了为什么高频交易训练很适合磨炼工程思维：

```text
它不允许模糊。
时间戳错一点，结论就错。
fill 假设错一点，收益就虚。
fee 忽略一点，策略就失真。
```

## 14. 跨市场迁移：为什么不止 Crypto

高频做市理论不是 crypto 专属。

可以迁移到：

```text
Equities
Futures
Options
ETF
FX
Crypto spot
Crypto perpetual futures
```

通用问题是：

```text
1. 当前 fair price 是多少？
2. bid / ask 应该挂多远？
3. 当前库存偏不偏？
4. 市场单到达概率如何？
5. 被 adverse selected 的概率多大？
6. fee / rebate / tick size 是否支持做市？
7. 回测成交假设是否接近真实？
```

不同市场的差别：

```text
股票：交易时间、撮合规则、监管、借券、tick size、队列更严格。
期货：合约乘数、保证金、主力切换、夜盘、交割。
期权：希腊字母、波动率面、hedging、宽盘口。
FX：分散流动性、dealer market、last look。
Crypto：7x24、交易所差异、funding、链上/交易所账户风险。
```

所以我们可以把 crypto 当作第一仗，但最终目标是理解通用 market making。

## 15. 面试拷打问题

必须能答：

```text
Q1: 做市和方向交易有什么区别？
Q2: 做市商为什么能赚钱？
Q3: spread、rebate、inventory、adverse selection 分别是什么？
Q4: 什么是 order book imbalance？为什么它可能预测短期价格？
Q5: mid price、micro price、adjusted mid price 有什么区别？
Q6: AS/ASQ 模型里的 sigma、A、k、gamma、q、Q 分别是什么？
Q7: market order arrival intensity 怎么估？
Q8: 为什么高频回测的 fill assumption 很危险？
Q9: queue position 为什么重要？
Q10: 如果你的回测没有 latency，会有什么问题？
Q11: maker rebate 策略为什么可能脆弱？
Q12: 你这个项目离生产级系统还差什么？
```

一个稳的回答框架：

```text
This is a research prototype. 
The goal is to understand the market making workflow: tick data normalization, microstructure feature engineering, 
ASQ-style inventory-aware quoting, and fee-aware backtesting.
The biggest limitation is execution realism: queue position, latency, partial fills, cancel/replace logic, 
and exchange-specific matching rules are not fully modeled yet.
```

## 16. 简历表达版本

一行版：

```text
Studied a crypto HFT market making prototype covering tick data normalization, order-book features, ASQ-style quoting, inventory constraints, and fee-aware backtesting.
```

三行版：

```text
Built a research prototype for crypto tick-level market making.
Normalized best bid/ask tick data, engineered microstructure features, estimated ASQ parameters, and tested inventory-aware bid/ask quote rules.
Analyzed key backtest limitations including queue position, latency, partial fills, maker rebate dependence, and adverse selection.
```

中文版：

```text
学习并复现以数字货币 tick 数据为案例的高频做市研究原型，覆盖盘口数据标准化、微观结构特征、ASQ 做市报价、库存约束和手续费/返佣回测，并重点分析高频回测中的队列位置、延迟、成交假设和逆向选择风险。
```

## 17. 公开安全边界

这篇公开文章保留：

```text
通用市场微观结构概念
通用高频做市架构
可复用伪代码
通用 AS/ASQ 参数解释
公开可讨论的工程风险
面试可讲的项目边界
```

这篇公开文章不包含：

```text
原始数据
私有文件路径
私有来源名称
内部材料标识
未脱敏结果
具体可交易 alpha
生产级交易参数
真实账户或交易记录
```

这是正确的公开方式：

```text
show capability, not leak edge.
```

## 18. 下一步

这个方向后续可以继续做三篇：

```text
MarketMakerHFT002: Order Book Imbalance and Micro Price
MarketMakerHFT003: ASQ Model and Inventory-Aware Quoting
MarketMakerHFT004: Backtest Realism - Queue Position, Latency, Fill Assumption
```

更工程化的下一步：

```text
1. 用公开样例数据构造一个极小 tick dataset。
2. 写一个 minimal market making simulator。
3. 加入 queue position / latency / fee 参数。
4. 输出 public-safe experiment report。
5. 把它接入 Pengyi Quant Research OS 的 experiment ledger。
```

这一仗的意义不是证明我们已经是高频做市专家，而是：

```text
从中低频期货因子研究，推进到 tick-level market microstructure。
从策略信号，推进到报价、库存、成交、费用和真实交易系统约束。
```

这就是 `MarketMakerHFT001` 的位置。
