---
title: "FICC002: Currencies / FX - 汇率、远期、掉期、利差、套息与跨境资金流"
date: 2026-07-03 00:30:00 +0800
categories: [Learning, Finance]
tags: [ficc002, ficc, currencies, fx, foreign-exchange, spot, forward, swap, options, carry, cross-currency-basis, macro, research-os]
---

这是 `PENGYI_FICC_MAP` 的 `FICC002`。

```text
FICC000 -> FICC 总地图
FICC001 -> Fixed Income / Rates / Credit
FICC002 -> Currencies / FX
FICC003 -> Commodities
```

这一篇补齐 FICC 中间这一块：

```text
Currencies = FX = Foreign Exchange = 外汇
```

它不是简单的“美元兑人民币涨跌”。
在 FICC 语境里，FX 连接的是：

```text
interest rate differential
central bank policy divergence
inflation and real rate
balance of payments
capital flow
trade flow
commodity terms of trade
reserve currency system
funding liquidity
geopolitical risk
cross-border risk transfer
```

公开边界：

```text
This is educational material and research infrastructure thinking.
It is not investment advice, trading advice, or an actionable alpha note.
```

中文边界：

```text
这是公开学习笔记，不是投资建议。
不包含内部数据、客户信息、未脱敏策略、实盘观点或可交易 alpha。
```

## 一句话总览

FX 的核心问题是：

```text
不同国家货币之间的相对价格，如何由利率、通胀、增长、资本流动、贸易流动、政策可信度和全球风险偏好共同决定。
```

工程化一点：

```text
FX = relative macro + relative rates + cross-border flow + funding liquidity + policy credibility + market positioning.
```

如果把 FX 压成一张链路图：

```text
macro data / inflation / employment / growth
  -> central bank reaction function
  -> rate differential and real rate differential
  -> FX spot / forward / carry

trade balance / current account / capital account
  -> cross-border flow
  -> currency demand and supply

risk sentiment / dollar liquidity / geopolitics
  -> safe haven demand / funding stress
  -> FX volatility and basis
```

## 为什么 FX 属于 FICC

FICC 是：

```text
Fixed Income
Currencies
Commodities
```

FX 放在 FICC 里，是因为它天然和 rates、credit、commodities 连在一起。

例如：

```text
Fed 更鹰派
  -> USD rates 上行
  -> US rate differential 扩大
  -> USD carry 变强
  -> non-USD funding pressure 上升
  -> EM FX 可能承压
```

再例如：

```text
oil price 上行
  -> commodity exporter terms of trade 改善
  -> CAD / NOK 等商品货币可能受益
  -> oil importer 的通胀和贸易账压力上升
  -> local rates / FX 同时重定价
```

所以 FX 不是孤立资产。
它是宏观变量在跨国资金价格上的投影。

```text
Rates tells us the price of money over time.
FX tells us the relative price of money across countries.
```

中文：

```text
利率回答：钱在不同期限上的价格是多少？
外汇回答：不同国家的钱彼此之间值多少？
```

## FX 市场的基本对象

最基础的 FX 产品包括：

```text
FX spot
FX forward
FX swap
FX option
NDF
cross-currency swap
```

它们分别对应不同问题：

| 产品 | 中文理解 | 回答的问题 |
|---|---|---|
| FX spot | 即期外汇 | 今天两种货币的交换价格是多少 |
| FX forward | 外汇远期 | 未来某天锁定什么汇率交换 |
| FX swap | 外汇掉期 | 近端换入、远端换出，管理短期资金和套保 |
| FX option | 外汇期权 | 用期权管理方向、波动率和尾部风险 |
| NDF | 无本金交割远期 | 管制货币或离岸市场的远期表达 |
| Cross-currency swap | 交叉货币掉期 | 长期限跨币种融资和现金流交换 |

第一阶段先抓住四个：

```text
spot
forward
swap
option
```

这四个能覆盖大部分 FX research 的基础语言。

## 货币对怎么报价

FX 不是“某个资产价格”，而是一个货币对。

常见写法：

```text
EUR/USD
USD/JPY
GBP/USD
USD/CHF
AUD/USD
USD/CAD
USD/CNH
```

一个货币对里：

```text
base currency / quote currency
```

例如：

```text
EUR/USD = 1.10
```

表示：

```text
1 EUR = 1.10 USD
```

如果 `EUR/USD` 上升，表示：

```text
EUR stronger against USD
USD weaker against EUR
```

如果 `USD/JPY` 上升，表示：

```text
USD stronger against JPY
JPY weaker against USD
```

FX 初学者容易混乱的地方在于：

```text
有些货币对是 USD 在后面，有些是 USD 在前面。
```

所以看图之前一定先问：

```text
base currency 是谁？
quote currency 是谁？
这条线涨，具体是谁升值？
```

## Spot: 即期汇率

`Spot` 是最直观的一层：

```text
今天市场愿意用多少 quote currency 去换 1 单位 base currency。
```

Spot 的短期变动通常受这些因素影响：

```text
macro surprise
central bank communication
risk sentiment
equity / credit market stress
commodity price shock
positioning unwind
intervention risk
liquidity condition
geopolitics
```

但 spot 不是全部。

在机构 FX 里，很多业务真正关心的是：

```text
未来现金流怎么套保？
跨境融资成本是多少？
不同货币之间的利率差如何反映到远期点？
```

所以必须进入 forward 和 swap。

## Forward: 远期汇率

`Forward` 是未来某一天交割的汇率。

最基础直觉：

```text
FX forward = FX spot + forward points
```

其中 `forward points` 很大程度上来自两种货币的利率差。

理论上，远期汇率和即期汇率之间有一个核心关系：

```text
covered interest rate parity
```

直觉是：

```text
如果两个货币的无风险收益不同，那么远期汇率必须调整，否则会出现低风险套利空间。
```

非常简化的表达：

```text
Forward / Spot ≈ (1 + domestic rate) / (1 + foreign rate)
```

这里不要急着背公式。
真正要抓住的是：

```text
forward price is deeply linked to rate differential.
```

中文：

```text
远期汇率本质上把两种货币的利率差写进了未来换汇价格。
```

例如：

```text
如果 USD 利率显著高于 JPY 利率，
那么 USD/JPY 的远期点会反映美元融资和日元融资之间的成本差。
```

这就是 FX 和 Fixed Income 的连接点。

## FX Swap: 外汇掉期

`FX swap` 不是利率 swap，也不是 credit swap。

它通常是：

```text
near leg: 现在交换两种货币
far leg: 未来再反向交换回来
```

它常用于：

```text
short-term funding
liquidity management
hedging foreign currency cash flow
rolling forward exposure
bank balance sheet management
```

直觉上：

```text
FX swap market 是跨币种短期资金市场的一部分。
```

当美元流动性紧张时，很多非美机构需要通过 FX swap 获得美元资金。
这会影响：

```text
forward points
cross-currency basis
USD funding cost
global liquidity stress
```

所以 FX swap 不是“衍生品细节”。
它是理解全球美元流动性的关键入口之一。

## Cross-Currency Basis

`Cross-currency basis` 可以先这样理解：

```text
理论利率平价和真实跨币种融资成本之间的偏离。
```

更直白：

```text
同样想借美元，不同市场、不同货币体系下的真实成本可能不同。
```

当 basis 明显偏离时，说明：

```text
balance sheet constraint
USD funding pressure
regulatory cost
collateral scarcity
market segmentation
liquidity stress
```

可能在起作用。

这对 FICC 很重要，因为它连接：

```text
FX swap market
money market
bank balance sheet
repo / funding
global dollar liquidity
```

对于 Research OS 来说，cross-currency basis 是一个非常好的“压力传感器”。

可以做成：

```text
USD funding stress monitor
cross-currency basis dashboard
funding liquidity event ledger
```

## FX Options: 方向、波动率与尾部风险

FX option 让我们不只看方向，还看波动率和尾部风险。

基础对象包括：

```text
call
put
strike
expiry
implied volatility
delta
gamma
vega
risk reversal
butterfly
volatility smile
```

FX options 常见用途：

```text
hedging
event protection
tail risk management
structured products
volatility trading
expression of asymmetric views
```

比如重大央行会议、选举、地缘冲突、汇率干预风险来临时，spot 未必已经大幅动。
但 options market 可能先反映：

```text
implied volatility higher
risk reversal skew changed
tail protection demand increased
```

所以 FX research 不能只看 spot。
还要看：

```text
vol surface
skew
term structure of implied vol
event premium
```

## G10 FX 与 EM FX

FX 可以先分为两大块：

```text
G10 FX
EM FX
```

G10 常见货币：

```text
USD
EUR
JPY
GBP
CHF
CAD
AUD
NZD
NOK
SEK
```

G10 FX 更关注：

```text
central bank divergence
rate differential
inflation path
growth differential
risk sentiment
safe haven demand
commodity linkage
positioning
```

EM FX 更关注：

```text
external balance
current account
foreign reserve
inflation credibility
real yield
political risk
capital control
commodity exposure
USD funding condition
local market liquidity
```

G10 和 EM 的区别不是“发达”和“新兴”这么简单。

更重要的是：

```text
EM FX 对全球美元流动性、本国政策可信度、外储、经常账户和风险偏好通常更敏感。
```

## USD: 外汇系统的中心变量

FX 里最重要的货币通常是 USD。

原因包括：

```text
reserve currency
global trade invoicing
commodity pricing
USD funding market
Treasury market depth
safe haven demand
Fed policy influence
global risk sentiment
```

很多 FX 研究问题本质上是：

```text
USD 是在走强还是走弱？
这是 rate-driven、risk-driven、liquidity-driven，还是 growth-driven？
```

一个很常见的拆解：

```text
USD up because Fed is more hawkish
  -> rate differential story

USD up because global risk sentiment is weak
  -> safe haven / deleveraging story

USD up because dollar funding is tight
  -> liquidity stress story

USD down because global growth recovers outside US
  -> growth convergence story
```

同样是 USD 走强，背后的机制可能完全不同。
这就是 FX research 的难点。

## Interest Rate Differential

FX 最核心的驱动之一是：

```text
interest rate differential
```

也就是：

```text
两种货币对应国家的利率差。
```

更进一步是：

```text
nominal rate differential
real rate differential
expected policy path differential
```

例如：

```text
US rates up relative to Eurozone rates
  -> USD assets become more attractive on carry
  -> EUR/USD may face pressure
```

但这里不能机械理解。
因为 FX 还要看：

```text
这个利差变化是否已经 priced in？
这个利差背后是 strong growth 还是 sticky inflation？
市场是 risk-on 还是 risk-off？
央行是否 credible？
```

所以要把利差放进宏观叙事里。

## Carry Trade

`Carry` 是 FX 里非常核心的概念。

直觉：

```text
借低利率货币，买高利率货币，赚取利差。
```

最简化版本：

```text
funding currency: low rate currency
investment currency: high rate currency
```

Carry trade 通常在这些环境更舒服：

```text
global risk sentiment stable
volatility low
funding liquidity abundant
central bank path predictable
high-yielding currency not sharply depreciating
```

Carry trade 害怕：

```text
risk-off shock
volatility spike
funding squeeze
policy surprise
devaluation pressure
crowded positioning unwind
```

一句话：

```text
Carry earns slowly, but can unwind violently.
```

中文：

```text
套息平时赚得慢，风险来时可能撤得很快。
```

所以 carry 不是免费午餐。
它是：

```text
interest differential compensation + volatility risk + crash risk + liquidity risk.
```

## Balance of Payments: 国际收支

FX 不只看利率。
还要看真实跨境资金流。

国际收支可以先拆成：

```text
current account
capital account / financial account
```

Current account 关注：

```text
trade balance
services balance
income balance
transfers
```

Financial account 关注：

```text
portfolio flow
FDI
bank flow
reserve flow
external debt
```

直觉：

```text
经常账户强，说明一国通过贸易和收入项目获得外币的能力较强。
资本流入强，说明外部资金愿意配置该国资产。
```

但二者也会冲突。

例如：

```text
一个国家经常账户赤字，但资本流入很强，货币也可能很强。
如果资本流入反转，赤字国家的 FX 压力可能迅速上升。
```

所以 FX research 要同时看：

```text
flow level
flow direction
flow stability
flow reversibility
policy response
```

## Central Bank Divergence

FX 很多时候交易的是央行分化：

```text
Fed vs ECB
Fed vs BoJ
Fed vs PBoC
BoE vs ECB
RBA vs RBNZ
```

核心不是只问：

```text
谁加息？
谁降息？
```

而是问：

```text
谁比市场预期更鹰？
谁比市场预期更鸽？
谁的通胀压力更顽固？
谁的增长下行更明显？
谁的政策可信度更强？
谁的政策空间更大？
```

FX 对“相对变化”很敏感。

```text
absolute macro level matters.
relative macro surprise matters more.
```

中文：

```text
绝对宏观状态重要，但相对预期差更容易驱动汇率重定价。
```

## Risk Sentiment 与 Safe Haven

FX 还有一个重要维度：

```text
risk-on / risk-off
```

Risk-on 时，市场更愿意承担风险：

```text
high beta FX may benefit
carry currencies may perform
commodity FX may strengthen
safe haven demand may fade
```

Risk-off 时，市场去杠杆、买避险：

```text
USD may strengthen
JPY / CHF may attract safe-haven demand
EM FX may weaken
carry positions may unwind
FX volatility may rise
```

但这也不是机械规则。
比如 JPY 在不同阶段可能受到：

```text
safe haven demand
rate differential pressure
BoJ policy shift
carry unwind
capital repatriation
```

共同影响。

所以研究 FX 要避免单因子解释。

## Commodity FX

一些货币和商品高度相关：

```text
CAD - oil
NOK - oil and gas
AUD - iron ore / China growth / metals
NZD - dairy / risk sentiment
CLP - copper
BRL - commodities and local rates
```

Commodity FX 的核心逻辑是：

```text
commodity price changes
  -> terms of trade
  -> export revenue
  -> current account
  -> inflation / rates
  -> FX
```

这也是为什么 FICC 三块必须一起看：

```text
Commodities move inflation.
Inflation moves rates.
Rates and terms of trade move FX.
FX moves import prices and financial conditions.
```

## FX Risk: 企业和机构为什么关心

FX 对企业非常现实。

跨国企业会面临：

```text
transaction exposure
translation exposure
economic exposure
```

简单说：

```text
未来外币收入或成本怎么锁定？
海外资产负债表折算怎么影响报表？
汇率长期变化怎么影响竞争力？
```

银行和机构则关心：

```text
client hedging demand
market making inventory
VaR / stress test
liquidity risk
counterparty risk
collateral and funding
```

所以 FX 不是只服务投机交易。
它也是实体企业和金融机构的风险管理基础设施。

## FX Research 每天看什么

一个 FX analyst / strategist 每天可能看：

```text
spot moves
forward points
FX swap points
implied vol
risk reversal
rates differential
yield curve change
central bank speeches
macro data surprise
positioning
capital flow
commodity price
equity / credit sentiment
geopolitical event
intervention headline
```

可以做成日常 checklist：

```text
1. Overnight FX move
2. What drove the move
3. Rates differential update
4. Central bank repricing
5. Risk sentiment and cross-asset confirmation
6. Commodity linkage
7. Flow / positioning signal
8. Vol and options market
9. Key events ahead
10. Research view and uncertainty
```

这非常适合用 RAG / Agent 来辅助。

## FX 与 AI Research OS

FX 是很适合 AI Research OS 的场景。

因为 FX research 同时需要处理：

```text
market data
macro data
central bank speeches
policy statements
news
research reports
flow commentary
event calendar
historical regimes
```

可以拆成几个模块：

```text
FX Pair Monitor
Central Bank Divergence Tracker
Rate Differential Dashboard
Forward Points / Basis Monitor
FX Vol Surface Summary
Macro Surprise Ledger
Event-to-FX Knowledge Graph
Forecast Ledger
Daily FX Brief Generator
Human PM Review Layer
```

核心不是让 agent 直接喊单。
核心是：

```text
把信息整理、因果链条、历史类比、风险点、反例和下一步验证系统化。
```

## FX RAG 怎么做

一个公开安全的 FX RAG 可以这样搭：

```text
Inputs:
  central bank statements
  macro calendar
  public economic data
  public market summaries
  public research notes
  official data releases

Processing:
  chunk documents by event / country / currency pair
  extract entities
  extract policy stance
  extract macro surprise
  extract market reaction
  link to currency pair and rate differential

Outputs:
  daily brief
  pair-specific summary
  event impact note
  forecast ledger update
  uncertainty and counterargument list
```

Graph schema 可以先这样：

```text
Country
Currency
CentralBank
PolicyRate
Inflation
Growth
TradeBalance
CommodityExposure
CurrencyPair
MarketEvent
FXMove
VolatilityMove
ResearchClaim
Evidence
```

关系包括：

```text
CentralBank -> sets -> PolicyRate
Country -> uses -> Currency
Currency -> forms_pair -> CurrencyPair
MacroEvent -> affects -> Currency
CommodityShock -> affects -> TermsOfTrade
TermsOfTrade -> affects -> Currency
RiskOffEvent -> affects -> SafeHavenCurrency
ResearchClaim -> supported_by -> Evidence
```

这就是 FX 版本的 knowledge layer。

## FX Quant 可以怎么入手

公开学习阶段，先不要追求“实盘 alpha”。
先做研究基础设施：

```text
pair dashboard
macro event study
central bank text classifier
rate differential monitor
carry basket backtest skeleton
vol regime classifier
forecast ledger
research note generator
```

可以做几个安全练习：

```text
1. 用公开数据看 rate differential 与主要货币对的历史相关性。
2. 做 FOMC / ECB / BoJ 会议前后的 FX event study。
3. 对央行声明做 hawkish / dovish 文本分类。
4. 做 G10 carry basket 的教育版回测框架。
5. 做 FX volatility regime summary。
```

注意边界：

```text
public data
educational backtest
no live signal
no proprietary factor
no client data
no actionable recommendation
```

## Interview 里怎么讲 FX

如果面试被问：

```text
你怎么理解 FX？
```

可以回答：

```text
我会把 FX 理解为 relative macro asset。
它不是只看一个国家，而是看两个货币经济体之间的相对利率、相对通胀、相对增长、央行政策分化、国际收支和资本流动。
在 FICC 框架里，FX 和 rates、credit、commodities 高度联动；例如央行路径影响利差，商品价格影响贸易条件，美元流动性影响 EM FX 和 cross-currency basis。
```

如果被问：

```text
FX 和 Fixed Income 有什么关系？
```

可以回答：

```text
Fixed Income 主要定价 money over time，FX 定价 money across countries。
两者通过利率平价、forward points、FX swap、cross-currency basis、央行政策路径和全球 funding liquidity 连接。
所以做 FX 不能只看 spot，必须看 rate differential、forward curve、swap points 和 funding condition。
```

如果被问：

```text
你怎么把 AI 用在 FX research？
```

可以回答：

```text
我会先做 research infrastructure，而不是直接做自动交易。
具体包括央行文本解析、宏观事件抽取、pair-level RAG、rate differential monitor、vol surface summary、forecast ledger 和 human review。
目标是把 FX research 的信息流、证据链、历史类比、反例和下一步验证系统化。
```

## Pengyi FX Research OS v0

可以把这篇落到一个小系统：

```text
Pengyi FX Research OS v0
```

模块设计：

```text
1. Currency Pair Registry
   管理 G10 / EM currency pairs

2. Macro Calendar Layer
   记录 CPI, payroll, GDP, PMI, central bank meetings

3. Central Bank Text Layer
   抽取 hawkish / dovish stance and policy path

4. Rate Differential Layer
   跟踪 nominal / real rate differential

5. Forward / Swap Layer
   跟踪 forward points, FX swap, basis

6. Flow and Positioning Layer
   记录公开 flow / positioning / CFTC 等信息

7. Volatility Layer
   总结 implied vol, skew, event premium

8. Event Graph Layer
   建立 macro event -> market reaction -> research claim 的图谱

9. Forecast Ledger
   记录预测、证据、反例、结果和复盘

10. Human Review
   PM / researcher 审核，避免 agent 直接输出未经验证的结论
```

这个 OS 的目标不是“直接交易”。
目标是：

```text
把 FX research 从碎片化新闻阅读，升级成可追踪、可复盘、可审计的研究流程。
```

## 和前后两篇的连接

和 `FICC001` 的连接：

```text
rates
yield curve
real yield
central bank path
repo / funding
swap curve
cross-currency basis
```

和 `FICC003` 的连接：

```text
commodity price
terms of trade
inflation
commodity exporter FX
shipping and energy shock
geopolitical risk
```

和 Quant / AI 的连接：

```text
event study
RAG
Graph RAG
forecast ledger
agent research workflow
daily brief automation
human review
```

所以完整 FICC 学习路径变成：

```text
FICC000: total map
FICC001: money over time
FICC002: money across countries
FICC003: physical supply-demand and futures curve
```

## 最终总结

FX 的核心不是背货币对名称。
真正要抓住的是：

```text
1. FX 是相对宏观资产。
2. Spot 只是第一层，forward / swap / option 才能看到更完整的资金价格。
3. 利差、央行分化、国际收支、资本流动、风险偏好和美元流动性共同驱动汇率。
4. FX 和 rates、commodities、credit 深度联动。
5. AI 在 FX 里最先应该做 research workflow、RAG、event graph、forecast ledger 和 human review，而不是直接喊单。
```

一句话收束：

```text
FX is the cross-border pricing layer of macro, rates, liquidity, trade, and risk sentiment.
```

中文：

```text
外汇是宏观、利率、流动性、贸易和风险偏好在跨境货币价格上的集中表达。
```
