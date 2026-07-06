---
title: "Futures001: 从期货 CTA 回测到 ML / GP 因子研究"
date: 2026-07-07 00:00:00 +0800
categories: [Learning, Quant Research]
tags: [futures001, futures, cta, quant-research, backtest, atr, dma, machine-learning, xgboost, genetic-programming, factor-research, public-safe]
---

这是 `Futures001`。

这篇不是发布原始数据，也不是公开私有课程材料。

这篇做的是一件更重要的事：

```text
把一个期货投研训练项目，抽象成 public-safe 的量化研究框架。
```

核心目标：

```text
1. 讲清楚期货数据是什么。
2. 讲清楚 CTA/time-series backtest 怎么跑。
3. 讲清楚 cross-sectional futures factor 怎么理解。
4. 讲清楚 ML / GP 为什么能扩展因子研究。
5. 讲清楚哪里容易出 bias、leakage、overfit。
6. 把它变成可以面试、可以交付、可以继续工程化的项目。
```

## 1. 项目定位

这个项目最安全、最准确的定位是：

```text
Futures quant research reproduction and study project.
```

不要把它说成：

```text
production trading system
live trading engine
institutional execution platform
guaranteed alpha library
```

更好的说法是：

```text
I studied and reproduced a futures quant research workflow covering futures OHLCV data processing, CTA signal construction, time-series backtesting, cross-sectional futures factor testing, ML-based signal modeling, genetic-programming factor search, and bias-aware performance diagnostics.
```

翻译成中文：

```text
我系统学习和复现了一套期货量化研究流程，覆盖期货 OHLCV 数据处理、CTA 信号构建、时序回测、截面期货因子测试、机器学习信号建模、遗传规划因子搜索，以及偏差诊断和表现评估。
```

这句话很稳。

它不夸张，但足够体现完整研究链路。

## 2. 两类核心数据包：Dataset1 和 Dataset2

这个期货项目可以拆成两类材料。

为了公开安全，这里只讲结构和方法，不暴露原始数据。

### 2.1 Dataset1：CTA / Time-Series Backtest 基础盘

Dataset1 更像期货 CTA 和回测基本功包。

它支撑这条线：

```text
期货分钟数据
  -> ATR / DMA / RSI 等技术指标
  -> 信号生成
  -> 仓位生成
  -> 期货回测
  -> 手续费、净值、回撤、夏普、Calmar、胜率
  -> 参数敏感性分析
```

常见数据字段：

```text
date_time
open
high
low
close
volume
turnover
oi
tradingDay
```

这里的 `oi` 是 open interest，持仓量。

期货里 open interest 很重要，因为它和流动性、主力合约、资金参与程度有关。

Dataset1 的核心价值：

```text
它把期货回测从“看价格涨跌”推进到“信号 -> 仓位 -> 成交 -> PnL -> 风险指标”的完整闭环。
```

### 2.2 Dataset2：ML / GP / 因子扩展包

Dataset2 更像进阶建模包。

它支撑这条线：

```text
指数 / 股指期货 / 衍生特征数据
  -> 特征生成
  -> 机器学习建模
  -> XGBoost demo
  -> 聚类 / 分布检验
  -> 遗传规划 GP 因子挖掘
  -> 单因子评价
  -> 模型过拟合诊断
```

Dataset2 的核心价值：

```text
它把传统 CTA 回测往 AI / ML futures research 推进。
```

但是这里要非常谨慎：

```text
ML / GP 不是自动印钞机。
```

更准确地说：

```text
ML / GP 扩大了因子搜索和非线性建模空间，但同时也极大放大了过拟合、数据泄露、样本切分错误和解释不稳定的风险。
```

## 3. Dataset1 和 Dataset2 的区别

| 维度 | Dataset1 | Dataset2 |
|---|---|---|
| 核心角色 | CTA / 期货回测基础 | ML / GP / 因子发现进阶 |
| 重点 | ATR、DMA、RSI、参数回测、PnL | XGBoost、聚类、分布检验、遗传规划 |
| 数据形态 | 商品/股指期货 OHLCVM，分钟/日频 | 指数、股指期货、持仓、价差、基差等 |
| 简历支撑 | 期货回测框架、CTA 策略复现 | ML 信号建模、GP 因子挖掘 |
| 面试拷打 | PnL、手续费、合约乘数、回撤、信号滞后 | 特征、标签、时间切分、泄露、过拟合 |

一句话：

```text
Dataset1 帮我打通期货 CTA 回测基本功；
Dataset2 帮我把研究延伸到 ML 建模、因子生成和 GP 搜索。
```

## 4. 期货数据和股票数据不一样

期货不是股票。

期货研究里必须额外理解：

```text
contract multiplier
margin
contract roll
main contract
continuous contract
open interest
night session
trading calendar
liquidity difference
transaction cost
slippage
```

最容易被面试拷打的是：

```text
你是不是把期货当股票回测了？
```

好的回答：

```text
不会。期货回测要考虑合约乘数、换月、主力合约连续性、保证金、交易时段、持仓量和手续费。PnL 也不是简单价格收益率，而要乘以合约乘数，并且每次开平仓都会产生交易成本。
```

## 5. CTA 回测主线

CTA 可以先粗略理解为：

```text
用趋势、动量、突破、波动率等信号，在期货品种上做 time-series long / short / flat 决策。
```

一个典型流程：

```text
OHLCVM data
  -> indicator
  -> signal
  -> target position
  -> trade
  -> daily PnL
  -> net value
  -> metrics
```

这条链路里，最重要的区分是：

```text
signal is not position.
```

信号只是研究判断。

仓位才是交易动作。

中间还要经过：

```text
开仓规则
平仓规则
交易时段限制
仓位大小
手续费
滑点
合约乘数
```

## 6. ATR 突破信号

ATR 是 Average True Range。

它衡量的是价格波动范围。

常见计算：

```text
TR = max(
  high - low,
  abs(high - previous_close),
  abs(low - previous_close)
)

ATR = rolling_mean(TR, N)
```

然后构造上下轨：

```text
Upline = open + k1 * ATR
Downline = open - k2 * ATR
```

信号：

```text
if close > Upline:
    signal = 1
elif close < Downline:
    signal = -1
else:
    signal = 0
```

解释：

```text
如果价格突破上轨，认为上涨趋势/突破成立；
如果价格跌破下轨，认为下跌趋势/突破成立；
否则不交易或维持原仓位。
```

这是一个非常经典的 CTA / breakout 思路。

## 7. 自写伪代码：ATR Signal

公开文章里不放原始源码，但可以写一个 public-safe 版本：

```python
def atr_signal(df, n=14, k1=1.0, k2=1.0):
    ohlc = df[["open", "high", "low", "close"]].copy()

    tr1 = ohlc["high"] - ohlc["low"]
    tr2 = (ohlc["high"] - ohlc["close"].shift(1)).abs()
    tr3 = (ohlc["low"] - ohlc["close"].shift(1)).abs()

    tr = pd.concat([tr1, tr2, tr3], axis=1).max(axis=1)
    atr = tr.rolling(n).mean()

    upper = ohlc["open"] + k1 * atr
    lower = ohlc["open"] - k2 * atr

    signal = pd.Series(0, index=df.index)
    signal[ohlc["close"] > upper] = 1
    signal[ohlc["close"] < lower] = -1

    return signal
```

这段代码体现的是方法，不是私有 alpha。

核心是：

```text
volatility-adjusted breakout.
```

## 8. Position Function：从信号到仓位

信号不能直接等于仓位。

一个保守仓位函数可以是：

```python
def position_rule(signal, current_position, is_near_close):
    target = current_position

    if current_position == 0:
        if is_near_close:
            return 0
        if signal != 0:
            target = signal

    else:
        if current_position * signal < 0:
            target = 0
        if is_near_close:
            target = 0

    return target
```

这里表达几个规则：

```text
1. 空仓时，出现信号才开仓。
2. 接近收盘不新开仓。
3. 持仓时，出现反向信号先平仓。
4. 接近收盘主动平仓。
```

面试中要讲清楚：

```text
signal -> target position -> trade execution
```

这三者不是一回事。

## 9. PnL 和合约乘数

期货 PnL 要考虑合约乘数。

一个简化表达：

```text
holding_pnl = start_position * (close - previous_close) * contract_multiplier
trading_pnl = position_change * (close - trade_price) * contract_multiplier
turnover = trade_price * volume * contract_multiplier
commission = turnover * commission_rate
net_pnl = holding_pnl + trading_pnl - commission
```

这就是为什么 `contract_multiplier` 重要。

如果忽略它，PnL 就会错。

如果忽略手续费和滑点，策略表现也会被高估。

## 10. 参数网格和过拟合

ATR 策略通常会调参数：

```text
N
k1
k2
```

比如：

```text
k1 = 0.1, 0.2, ..., 3.0
k2 = 3.0, 2.9, ..., 0.1
```

然后看：

```text
annualized return
volatility
Sharpe
max drawdown
Calmar
win rate
trade count
return-risk ratio
```

但是这里有一个很大的风险：

```text
parameter sweep can overfit.
```

参数热力图很好看，不代表未来有效。

必须做：

```text
in-sample / out-of-sample split
walk-forward validation
multi-instrument validation
cost sensitivity
regime robustness
```

## 11. Cross-Sectional Futures Factor

CTA 是 time-series。

Cross-sectional futures factor 是另一种逻辑。

区别：

```text
CTA:
  对单个品种，判断它自己应该做多、做空还是空仓。

Cross-sectional factor:
  在同一个日期，对多个期货品种排序，做多高分组，做空低分组。
```

一个简单框架：

```text
for each date:
    compute factor value for each instrument
    rank instruments
    long top group
    short bottom group
    aggregate portfolio return
```

常见因子方向：

```text
momentum
term structure
roll return
basis
open interest change
volume / liquidity
volatility
technical morphology
```

这和股票多因子类似，但期货里有自己的特殊性：

```text
contract roll
term structure
carry
seasonality
inventory
macro sensitivity
margin and leverage
```

## 12. ML Signal Modeling

机器学习进入期货研究，常见流程是：

```text
features X
label y
train / validation / test split
model fit
prediction
signal conversion
backtest
diagnosis
```

常见模型：

```text
linear model
decision tree
random forest
XGBoost
SVM
RNN / LSTM
deep sequence model
ensemble model
```

但是金融时间序列不能随便 random split。

一个危险做法：

```text
randomly split all samples into train and test.
```

为什么危险？

因为会破坏时间结构，容易让未来信息通过数据分布、标准化、标签构造、重复样本等方式泄露到训练集。

更好的做法：

```text
rolling split
walk-forward validation
time-based OOS
```

## 13. XGBoost 在这里的角色

XGBoost 不是魔法。

它的作用是：

```text
用树模型拟合非线性关系和特征交互。
```

但它不能自动解决：

```text
低信噪比
市场 regime change
样本外退化
交易成本
label instability
feature leakage
```

面试表达：

```text
I treat XGBoost as a nonlinear modeling tool, not as an alpha guarantee. The key is feature-label definition, rolling validation, baseline comparison, and OOS stability.
```

## 14. Genetic Programming：因子公式搜索

GP 可以理解为：

```text
自动搜索 symbolic factor formulas.
```

它需要定义：

```text
terminal set
function set
fitness function
population
mutation
crossover
selection
complexity penalty
```

一个 GP 因子可能长得像：

```text
rank(ts_mean(volume, 20) / ts_std(close, 10))
```

或者：

```text
ts_corr(return, open_interest_change, 20)
```

注意，这里只是示意。

GP 最大的问题：

```text
huge search space + noisy financial data = severe overfitting risk.
```

所以 GP 的价值不是：

```text
自动找到永久 alpha。
```

更合理的定位是：

```text
factor candidate generation engine.
```

后面必须接：

```text
OOS validation
turnover check
cost check
factor decay
cross-instrument robustness
economic interpretation
complexity control
```

## 15. 这套项目最重要的不是某一个结果

这套期货项目最重要的不是：

```text
某个参数组合收益最高。
```

也不是：

```text
某个 ML 模型跑出来很漂亮。
```

最重要的是研究能力：

```text
1. 能拆数据。
2. 能写信号。
3. 能转仓位。
4. 能做回测。
5. 能算指标。
6. 能识别 bias。
7. 能解释为什么结果可能不可靠。
8. 能提出下一轮验证计划。
```

这是 quant research 的基本功。

## 16. Bias Checklist

期货项目必须拷打这些：

```text
look-ahead bias
signal / position misalignment
label leakage
feature standardization leakage
contract roll error
survivorship bias in universe
ignored transaction cost
ignored slippage
parameter overfitting
single-period overfitting
random split on time-series
liquidity blind spot
turnover explosion
regime dependency
```

如果这些讲不清楚，项目很容易被认为只是“跑过代码”。

如果这些能讲清楚，就能体现真正研究训练。

## 17. 面试 30 秒版本

```text
I worked through a futures quant research project covering CTA backtesting, futures multi-factor research, and ML / GP factor discovery. The core was to understand the full research loop: futures OHLCV data, ATR / DMA style signal construction, position generation, transaction-cost-aware backtesting, performance metrics, and bias diagnosis. I do not frame it as a production trading system; I frame it as a research reproduction and training project that taught me the structure and failure modes of futures quant research.
```

中文版本：

```text
我做过一个期货量化研究训练项目，覆盖 CTA 回测、期货多因子研究，以及 ML / GP 因子发现。核心不是宣称做出了生产级交易系统，而是打通完整研究链路：期货 OHLCV 数据、ATR / DMA 信号构建、仓位生成、含手续费的回测、表现指标，以及 look-ahead、过拟合、时间切分等偏差诊断。
```

## 18. 面试 2 分钟版本

```text
这个项目可以分三层。

第一层是期货数据和回测。我学习了分钟/日频 OHLCVM、持仓量、成交额、合约乘数等字段，并理解为什么期货回测不能直接套股票回测，因为期货涉及合约换月、乘数、保证金、交易时段和成本。

第二层是 CTA 和多因子。CTA 部分主要是 ATR、DMA、RSI 这类技术指标，把价格和波动率转成 signal，再通过 position function 转成仓位，并计算手续费后的净值、回撤、夏普、Calmar 等指标。多因子部分则从单品种 time-series 拓展到多个期货品种的 cross-sectional ranking 和 long-short portfolio。

第三层是 ML 和 GP。ML 部分关注 feature、label、rolling split、XGBoost 等模型；GP 部分关注 symbolic factor search。我的核心理解是，ML/GP 能扩大搜索空间，但金融数据低信噪比，必须特别注意未来函数、随机切分、参数过拟合、成本和样本外稳定性。
```

## 19. 和 Research OS 的连接

这套期货项目可以接进 `Pengyi Quant Research OS`。

系统对象可以是：

```text
FuturesDataCard
SignalSpec
PositionRule
BacktestProtocol
MetricReport
BiasDiagnosis
ModelExperiment
FactorCandidate
NextResearchPlan
```

对应 pipeline：

```text
data
  -> signal / factor
  -> position
  -> backtest
  -> metrics
  -> diagnosis
  -> next experiment
  -> human review
```

这和我们之前做的 R&D Agent 很一致：

```text
自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ 人类 PM 审核
```

期货项目给了这个 Research OS 一个真实训练场。

## 20. Public-Safe 边界

这篇文章遵守几个边界：

```text
不公开原始数据。
不公开课程文件。
不公开完整私有源码。
不公开具体可交易 alpha 细节。
不把研究训练项目包装成生产交易系统。
```

可以公开的是：

```text
研究框架
方法论
指标解释
伪代码
bias checklist
面试表达
系统设计抽象
```

这才是长期能沉淀信用的方式。

## 21. 最终总结

`Futures001` 的核心结论：

```text
Dataset1 是期货 CTA / time-series backtest 基础盘。
Dataset2 是 ML / GP / factor discovery 进阶包。
两者合起来，构成了从传统期货 CTA 到 AI/ML futures research 的完整训练场。
```

但是更重要的是：

```text
我们真正要展示的不是某个神奇收益曲线，而是完整、可解释、可验证、能诊断偏差的 quant research workflow。
```

一句话：

```text
This futures project is a public-safe bridge from futures backtesting basics to AI-assisted quant research OS.
```

