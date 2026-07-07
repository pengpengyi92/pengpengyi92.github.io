---
title: "Futures x Crypto HFT: 期货投研项目与高频做市项目对比"
date: 2026-07-08 00:00:00 +0800
categories: [Learning, Quant Research]
tags: [futures, crypto-hft, market-making, quant-research, cta, factor-research, microstructure, asq, backtest, public-safe]
---

这是 `Futures x Crypto HFT`。

前面已经分别写过两篇：

- [Futures001: 从期货 CTA 回测到 ML / GP 因子研究](/posts/futures001-cta-ml-gp-research-project/)
- [MarketMakerHFT001: Crypto Case Study for High-Frequency Market Making](/posts/market-maker-hft001-crypto-case-study/)

这篇做一件更重要的事：

```text
把两个项目放在同一个量化研究坐标系里比较。
```

它们不是同一种项目。

期货项目更像：

```text
中低频投研 workflow
+ CTA / factor / ML / GP
+ 研究信号、仓位、回测、稳健性
```

Crypto HFT 项目更像：

```text
tick-level microstructure workflow
+ order book / market making / ASQ
+ 研究报价、成交、库存、返佣、微观结构风险
```

两者合起来，给我们的量化叙事非常完整：

```text
从中低频方向性/截面投研
到高频盘口/做市机制
再到 AI + Quant Research OS 的统一研究工程框架。
```

## 1. 一句话对比

| 维度 | Futures Quant Project | Crypto HFT Market Making Project |
|---|---|---|
| 核心问题 | 如何从期货数据里构造可评估的交易信号 | 如何在 tick-level 盘口里报价并控制库存 |
| 交易范式 | CTA、趋势、截面多因子、ML/GP 因子 | 做市、spread、rebate、order flow、inventory |
| 数据频率 | 日频 / 分钟级为主 | tick-level / order book / trade tick |
| 研究对象 | signal -> position -> PnL | quote -> fill -> inventory -> PnL |
| 主要风险 | 过拟合、未来函数、换月、交易成本、样本外失效 | queue position、latency、partial fill、adverse selection |
| 能力训练 | 量化研究流程、因子评价、回测诊断 | 市场微观结构、做市机制、高频回测真实性 |

最短表达：

```text
Futures project trains signal research.
Crypto HFT project trains microstructure and execution-aware thinking.
```

中文：

```text
期货项目训练的是“信号研究能力”。
Crypto HFT 项目训练的是“微观结构与成交机制理解”。
```

## 2. 研究目标不同

### 2.1 Futures：寻找信号和仓位规则

期货项目的核心链条是：

```text
data
-> indicator / factor
-> signal
-> position
-> backtest
-> metrics
-> diagnosis
```

你关心的是：

- 这个信号有没有预测性？
- 这个策略的 Sharpe / Calmar / drawdown 怎么样？
- 是否对参数敏感？
- 是否有 look-ahead bias？
- 是否能在 out-of-sample 里保持稳定？
- 交易成本之后还剩多少收益？

这类研究很接近传统量化投研流程。

### 2.2 Crypto HFT：管理报价、成交和库存

Crypto HFT 做市项目的核心链条是：

```text
tick data
-> order book / trade feature
-> short-horizon price model
-> bid / ask quote
-> fill assumption
-> inventory update
-> cash / PnL / fee / rebate
-> fill realism diagnosis
```

你关心的是：

- bid / ask 应该挂在哪里？
- spread 是否足够覆盖风险？
- 当前库存 q 是否过高？
- maker rebate 是否能覆盖 adverse selection？
- 回测里的 fill assumption 是否过于乐观？
- queue position 和 latency 没模拟会带来多大偏差？

这类研究更接近 market microstructure 和 execution-aware trading。

## 3. 数据形态不同

### 3.1 Futures：OHLCV / open interest / contract metadata

期货项目的数据重点是：

```text
open
high
low
close
volume
open interest
turnover
contract multiplier
main contract / continuous contract
```

期货和股票不一样。

期货有：

- 合约到期和换月。
- 主力合约切换。
- 合约乘数。
- 保证金。
- 夜盘。
- 不同品种不同交易时间。
- open interest 对资金行为有解释力。

所以期货研究不能只把数据当成普通股票 OHLCV。

### 3.2 Crypto HFT：best bid / ask / size / trade tick

Crypto HFT 项目的数据重点是：

```text
best bid price
best ask price
best bid size
best ask size
trade price
trade size
trade direction proxy
event time
```

核心特征包括：

```text
mid price = (best_bid + best_ask) / 2
spread = best_ask - best_bid
order book imbalance = (bid_size - ask_size) / (bid_size + ask_size)
adjusted mid price = mid + spread * imbalance / 2
```

这里的研究对象不是“今天涨跌”，而是：

```text
下一小段时间订单流会不会打到我的报价？
当前盘口是否暗示短期价格压力？
我挂在 bid / ask 上被成交后是否会被逆向选择？
```

## 4. 模型不同

### 4.1 Futures：CTA / Factor / ML / GP

期货项目可以拆成几条研究线：

```text
CTA: ATR / DMA / RSI / trend following / breakout
cross-sectional factor: ranking / long-short / term structure / momentum
ML: feature matrix -> label -> train/test split -> model -> prediction
GP: symbolic expression search -> fitness -> complexity control -> OOS validation
```

它的模型目标是：

```text
把历史市场状态映射成未来收益或仓位倾向。
```

典型问题：

- signal 怎么定义？
- position 是否滞后一格，避免未来函数？
- label 是未来收益还是方向？
- train/test split 是否按时间切？
- GP 发现的表达式是否只是数据挖掘噪声？

### 4.2 Crypto HFT：Microstructure / ASQ / Inventory

Crypto HFT 项目可以拆成几条研究线：

```text
microstructure features: spread / imbalance / trade flow / adjusted mid
short-horizon price model: predict next micro price or short return
ASQ quoting: bid spread / ask spread / risk aversion / order arrival
inventory control: q / Q / quote skew
fee-aware PnL: maker fee / rebate / adverse selection
```

它的模型目标不是单纯预测方向，而是：

```text
在控制库存风险和成交风险的前提下，生成 bid / ask quote。
```

ASQ / Avellaneda-Stoikov 类模型里，关键变量是：

```text
sigma: 短期波动率
A: 市价单到达强度规模
k: 到达强度随报价距离衰减速度
gamma: 风险厌恶
q: 当前库存
Q: 最大库存边界
```

典型问题：

- 报价中心用 mid price 还是 predicted mid price？
- 库存为正时，是否要降低 ask、提高卖出概率？
- 库存为负时，是否要提高 bid、加快补库存？
- maker rebate 是否能抵消 adverse selection？

## 5. 回测假设不同

### 5.1 Futures：仓位回测

期货项目的回测核心是：

```text
position[t]
return[t+1]
transaction cost
net value
Sharpe / drawdown / Calmar
```

最大风险通常是：

- `signal-position alignment` 错位。
- 使用了未来价格。
- 参数调太多导致过拟合。
- 手续费和滑点估计太低。
- 合约换月处理不干净。
- 样本外验证不严格。

所以 futures 项目最重要的防守点是：

```text
我知道一个漂亮回测不等于真实 alpha。
我会检查未来函数、交易成本、样本外和参数稳健性。
```

### 5.2 Crypto HFT：成交回测

Crypto HFT 做市回测的核心是：

```text
quote[t]
next interval market movement
fill assumption
cash update
inventory update
fees / rebate
PnL
```

最大风险通常是：

- queue position 没有模拟。
- latency 没有模拟。
- partial fill 没有模拟。
- cancel / replace 成本没模拟。
- 只用 best bid / ask 判断成交太乐观。
- 没有真实交易所撮合规则。
- adverse selection 被低估。

所以 crypto HFT 项目最重要的防守点是：

```text
我知道研究型 HFT backtest 和生产级 HFT 系统差距很大。
最关键的差距是 fill realism、latency、queue position 和 exchange mechanics。
```

## 6. 工程形态不同

### 6.1 Futures：Research Notebook / pandas Workflow

期货项目更适合：

```text
Python
pandas
NumPy
Jupyter
factor script
backtest utility
metric table
plot
research report
```

它的工程目标是：

```text
快速验证一个因子或策略假设。
```

重点是 research iteration。

### 6.2 Crypto HFT：Event / Tick / Trading Engine Workflow

Crypto HFT 项目更接近：

```text
tick parser
QuoteTick
instrument definition
Parquet data catalog
event-driven backtest
strategy class
quote update
inventory state
fee model
```

它的工程目标是：

```text
把盘口数据转成交易系统能理解的事件流。
```

重点是 market simulation。

这也是为什么 HFT 工程复杂度明显更高。

## 7. 面试里怎么讲

### 7.1 Futures 项目 30 秒版本

```text
我做过一个期货量化研究流程复现项目，主要覆盖 OHLCV 数据处理、CTA 信号构建、时序回测、截面期货因子测试、ML 信号建模和 GP 因子搜索。这个项目最核心的价值不是某个单一策略，而是让我完整理解了从数据、信号、仓位、回测到 bias diagnosis 的量化研究闭环。
```

英文：

```text
I studied and reproduced a futures quant research workflow covering OHLCV processing, CTA signal construction, time-series backtesting, cross-sectional futures factor testing, ML-based signal modeling, and GP-based factor discovery. The key value was not a single strategy, but understanding the full research loop from data and signal to position, backtest, and bias diagnosis.
```

### 7.2 Crypto HFT 项目 30 秒版本

```text
我还学习过一个以 crypto tick 数据为案例的高频做市研究项目，覆盖盘口数据标准化、order book imbalance、adjusted mid price、短期价格模型、ASQ 做市报价、库存约束和手续费/返佣回测。这个项目让我理解了做市不是单纯预测方向，而是在 spread、成交概率、库存风险和逆向选择之间做权衡。
```

英文：

```text
I also studied a crypto tick-level market making prototype covering order book data normalization, order book imbalance, adjusted mid price, short-horizon price modeling, ASQ-style quoting, inventory constraints, and fee/rebate-aware backtesting. It helped me understand that market making is not only about directional prediction, but about balancing spread capture, fill probability, inventory risk, and adverse selection.
```

### 7.3 两者合起来的版本

```text
这两个项目给我的量化训练是互补的：期货项目让我理解中低频投研里的信号、因子、仓位和回测诊断；crypto HFT 项目让我理解高频交易里的盘口、报价、成交、库存和回测真实性问题。一个偏 alpha research，一个偏 microstructure / execution-aware research。两者合起来，让我能从不同频率和不同市场机制理解量化研究。
```

英文：

```text
These two projects are complementary. The futures project trained my understanding of signal research, factor evaluation, position construction, and backtest diagnostics in medium-to-low-frequency quant research. The crypto HFT project trained my understanding of order book microstructure, quoting, fills, inventory control, and execution realism. One is closer to alpha research, while the other is closer to microstructure and execution-aware research.
```

## 8. 简历上怎么放

如果只能放一个项目，不建议把两个拆开写太多。

更稳的方式是放进：

```text
Pengyi AI / Quant Research OS
```

然后在 bullet 里写：

```text
Integrated futures CTA / ML / GP factor research, crypto HFT market making study, RAG / Agent / Harness learning, and open-source quant project analysis into a reusable AI + Quant Research OS.
```

中文：

```text
将期货 CTA / ML / GP 因子研究、crypto 高频做市学习、RAG / Agent / Harness 和开源量化项目拆解整合到统一 AI + Quant Research OS 中。
```

如果面试官追问，再展开：

```text
Futures side: signal research / factor / backtest / bias.
Crypto HFT side: microstructure / quote / fill / inventory / execution realism.
```

这比简历上塞很多项目更稳。

## 9. 两个项目各自的边界

### Futures 边界

可以说：

```text
I studied and reproduced a futures quant research workflow.
```

不要说：

```text
I built a production futures trading system.
```

可以说：

```text
I focused on signal construction, backtest mechanics, ML/GP extensions, and bias-aware evaluation.
```

不要说：

```text
The strategy can be directly deployed with real capital.
```

### Crypto HFT 边界

可以说：

```text
I studied a crypto tick-level market making research prototype.
```

不要说：

```text
I built a production HFT market maker.
```

可以说：

```text
The most important limitation is fill realism, especially queue position, latency, partial fills, and exchange-specific matching rules.
```

不要说：

```text
The backtest result proves live profitability.
```

## 10. 统一到 Research OS

这两个项目最终都应该进入同一个 Research OS。

统一结构是：

```text
research question
-> data schema
-> feature / signal design
-> model / rule
-> backtest
-> diagnostics
-> public-safe artifact
-> interview story
```

Futures 项目把我们训练成：

```text
factor researcher
```

Crypto HFT 项目把我们训练成：

```text
microstructure-aware researcher
```

Research OS 把它们整合成：

```text
AI + Quant research engineer
```

这是最关键的叙事。

## 11. 最终总结

这两个项目的共同价值不是“我已经拥有一个可实盘赚钱系统”。

共同价值是：

```text
我能读懂不同频率的量化研究问题，
能拆解数据、信号、模型、回测和偏差，
能主动承认研究原型和生产系统的边界，
能把学习项目转化成可解释、可复用、可面试的 Research OS。
```

这就是它们对我们 CV、面试、Research OS 和未来开源项目的核心贡献。

