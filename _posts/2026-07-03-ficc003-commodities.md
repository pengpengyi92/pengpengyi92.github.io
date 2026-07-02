---
title: "FICC003: Commodities - 能源、金属、农产品、期货曲线、库存与供需"
date: 2026-07-03 00:00:00 +0800
categories: [Learning, Finance]
tags: [ficc003, ficc, commodities, energy, metals, agriculture, futures, contango, backwardation, inventory, supply-demand, macro, research-os]
---

这是 `PENGYI_FICC_MAP` 的 `FICC003`。

按编号我们先做：

```text
FICC000 -> FICC 总地图
FICC001 -> Fixed Income / Rates / Credit
FICC002 -> Currencies / FX
FICC003 -> Commodities
```

到这里，FICC 三大主线已经完整：

```text
Fixed Income:
  money over time

Currencies / FX:
  money across countries

Commodities:
  physical supply-demand + futures curve + macro linkage
```

这一篇把第三块打透：

```text
Commodities = 商品
```

公开边界：

```text
This is educational material and research infrastructure thinking.
It is not investment advice, trading advice, or an actionable alpha note.
```

中文：

```text
这是公开学习笔记，不是投资建议。
不包含内部数据、客户信息、未脱敏策略、实盘观点或可交易 alpha。
```

## 一句话总览

Commodities 的核心是：

```text
实物供需、库存、运输、季节性、地缘政治、金融条件和期货曲线如何共同决定商品价格。
```

和股票、债券、外汇相比，商品最特殊的地方是：

```text
它背后有真实物理约束。
```

例如：

```text
原油要开采、运输、储存、炼化。
天然气要管道、LNG、储气库和天气需求。
铜要矿山、冶炼、库存、工业需求。
粮食要种植面积、天气、收成、库存消费比。
黄金要实际利率、美元、央行购金和避险需求。
```

所以商品不是简单的“价格时间序列”。

它是：

```text
physical market + futures market + macro market + geopolitical market.
```

## 为什么 Commodities 属于 FICC

FICC 是：

```text
Fixed Income
Currencies
Commodities
```

Commodities 属于 FICC，是因为它和宏观、利率、外汇、通胀、地缘政治、企业套保和机构风险管理高度相连。

商品影响：

```text
inflation
trade balance
terms of trade
commodity exporter FX
producer margins
consumer cost
central bank reaction function
credit risk
portfolio allocation
```

例如：

```text
oil price up
  -> headline inflation pressure up
  -> central bank may become more hawkish
  -> rates repricing
  -> USD / EM FX pressure
  -> credit conditions may change
```

或者：

```text
copper demand weakens
  -> industrial cycle concern
  -> China/global growth concern
  -> risk sentiment weakens
  -> credit spreads may widen
  -> commodity exporter FX may weaken
```

商品是宏观周期的高频传感器。

## 商品三大类

Commodities 可以先分三大类：

```text
Energy
Metals
Agriculture
```

进一步拆：

| 大类 | 子类 | 代表品种 | 核心驱动 |
|---|---|---|---|
| Energy | 能源 | crude oil, natural gas, gasoline, diesel, coal, power | 供需、库存、地缘、OPEC、天气、炼厂、运输 |
| Metals | 金属 | gold, silver, copper, aluminum, nickel, iron ore | 实际利率、美元、工业周期、矿山供给、库存 |
| Agriculture | 农产品 | wheat, corn, soybeans, cotton, sugar, coffee | 天气、种植面积、单产、库存消费比、出口、政策 |

还有一些交叉品类：

```text
livestock
carbon / emissions
power
freight
lithium / battery metals
rare earths
```

但第一阶段先抓住：

```text
oil
natural gas
gold
copper
wheat / corn / soybeans
```

这些足够建立商品框架。

## 商品和金融资产的区别

股票看：

```text
earnings
growth
valuation
cash flow
discount rate
```

债券看：

```text
cash flow
yield
duration
credit spread
default risk
```

外汇看：

```text
relative rates
growth differential
capital flows
policy divergence
```

商品看：

```text
physical supply-demand
inventory
storage
transport
weather
geopolitics
futures curve
producer / consumer hedging
macro overlay
```

商品难在：

```text
它同时是实物资产、金融合约、宏观变量和地缘资产。
```

这也是为什么商品研究很适合 RAG + Graph。

因为很多信息不是结构化价格数据，而是：

```text
OPEC statement
EIA inventory report
USDA WASDE report
mine disruption news
weather forecast
shipping bottleneck
sanction / conflict / export ban
refinery outage
policy announcement
```

## Energy

Energy 是商品里最宏观、最地缘、最通胀相关的一块。

代表品种：

```text
crude oil
natural gas
gasoline
diesel
heating oil
jet fuel
coal
power
LNG
```

能源研究最常见问题：

```text
全球原油供需是否平衡？
OPEC+ 政策如何影响供给？
美国页岩油产量如何变化？
库存是在 draw 还是 build？
炼厂开工率如何？
成品油需求如何？
天然气天气需求如何？
LNG 贸易流如何变化？
地缘风险是否影响供应？
```

## Crude Oil

原油是最核心的 energy commodity。

常见基准：

```text
WTI
Brent
Dubai / Oman
```

研究原油要看：

```text
global supply
global demand
OPEC+ production policy
U.S. shale production
inventories
refinery runs
exports / imports
spare capacity
geopolitical risk
shipping / sanctions
futures curve
time spreads
```

原油价格不是只由“经济好坏”决定。

它是多因素：

```text
supply shock
demand shock
inventory cycle
financial positioning
USD
real rates
risk appetite
geopolitical premium
```

## Oil Inventory

库存是油市的核心变量。

如果库存持续下降：

```text
market may be tighter
physical demand may be stronger than supply
near-term scarcity premium may rise
curve may move toward backwardation
```

如果库存持续上升：

```text
market may be oversupplied
physical demand may be weaker than supply
storage pressure may increase
curve may move toward contango
```

但不能机械理解。

要看：

```text
crude inventory
gasoline inventory
distillate inventory
Cushing inventory
SPR changes
refinery utilization
imports
exports
production
seasonal average
market expectation
```

EIA 的 Weekly Petroleum Status Report 是公开能源研究的重要来源。

它提供：

```text
crude oil stocks
petroleum product stocks
gasoline stocks
distillate stocks
refinery utilization
imports / exports
production
prices
```

对 Research OS 来说，这种报告非常适合做：

```text
OilInventoryCard
EnergyBrief
CurveImpactNote
ForecastLedger
```

## Natural Gas

天然气和原油不同。

它更受：

```text
weather
heating demand
cooling demand
storage
pipeline constraints
LNG exports/imports
regional infrastructure
power generation
```

影响。

天然气更“区域化”。

因为运输和储存约束更强。

常见研究问题：

```text
winter heating demand 是否超预期？
summer cooling demand 是否推高发电用气？
storage injection / withdrawal 是否偏离季节性？
LNG export capacity 是否改变本地平衡？
pipeline outage 是否造成区域价格冲击？
```

天然气是典型的：

```text
weather-sensitive commodity
```

所以需要：

```text
seasonality
weather forecast
storage report
infrastructure map
regional basis
```

## Refined Products

原油不是终端消费品。

它要炼化成：

```text
gasoline
diesel
jet fuel
heating oil
petrochemical feedstock
```

所以研究 oil 还要看 refinery。

关键变量：

```text
refinery utilization
crack spread
gasoline demand
distillate demand
jet fuel demand
maintenance season
product inventory
```

`crack spread` 可以理解为：

```text
refined product price - crude oil input cost
```

它反映炼厂利润和成品油市场紧张程度。

## Metals

Metals 可以分为：

```text
Precious metals
Industrial metals
Ferrous metals
Battery / energy transition metals
```

代表：

```text
Precious:
  gold, silver, platinum, palladium

Industrial:
  copper, aluminum, zinc, nickel, lead, tin

Ferrous:
  iron ore, steel

Battery:
  lithium, cobalt, nickel, graphite
```

金属研究的核心是：

```text
real rates
USD
industrial cycle
China demand
mine supply
inventory
energy cost
geopolitics
green transition
```

## Gold

Gold 很特殊。

它不是普通工业品。

它的驱动包括：

```text
real yields
USD
inflation concern
safe-haven demand
central bank buying
geopolitical risk
jewelry demand
ETF flows
positioning
```

黄金没有现金流。

所以它经常和：

```text
real rates
USD
risk sentiment
```

联系紧密。

简化直觉：

```text
real rates up
  -> opportunity cost of holding gold may rise
  -> gold may face pressure

real rates down or geopolitical risk up
  -> gold may be supported
```

但黄金不能用单变量解释。

例如央行购金、地缘风险、美元信用、避险需求都可能改变关系。

## Copper

Copper 常被称为宏观周期敏感品种。

因为铜广泛用于：

```text
construction
power grids
manufacturing
electronics
EVs
renewables
infrastructure
```

研究铜要看：

```text
China demand
global manufacturing cycle
PMI
mine supply
smelter capacity
TC/RC
exchange inventories
scrap supply
energy transition demand
USD
risk sentiment
```

铜是典型的：

```text
growth-sensitive commodity
```

所以铜价常被市场用来观察：

```text
global industrial cycle
China stimulus expectation
manufacturing recovery
```

但也要小心。

铜价既有宏观需求，也有供给扰动。

不能简单说：

```text
copper up = growth good
```

需要拆：

```text
demand-driven move
supply-driven move
inventory-driven move
positioning-driven move
```

## Iron Ore and Steel

铁矿和钢铁更接近：

```text
China property / infrastructure / steel production cycle
```

关键变量：

```text
China steel output
property construction
infrastructure demand
port inventories
mine shipments
steel margins
policy stimulus
environmental production cuts
```

这类商品非常受区域需求和政策影响。

## Agriculture

Agriculture 是商品里最受天气和季节影响的部分。

代表：

```text
wheat
corn
soybeans
rice
cotton
sugar
coffee
cocoa
```

农产品研究核心变量：

```text
planted area
yield
weather
crop condition
harvest progress
exports
ending stocks
stocks-to-use ratio
biofuel demand
trade policy
currency
fertilizer cost
```

农业商品非常季节性。

例如：

```text
planting season
growing season
harvest season
export season
```

不同月份市场关注点不同。

## WASDE

USDA 的 WASDE 是农业商品研究的重要公开报告。

WASDE 全称：

```text
World Agricultural Supply and Demand Estimates
```

它每月发布，提供美国和全球主要农产品的年度供需预测。

覆盖：

```text
wheat
rice
coarse grains
oilseeds
cotton
sugar
meat
poultry
eggs
milk
```

对 Research OS 来说，WASDE 可以转成：

```text
CropSupplyDemandCard
StocksToUseCard
WASDEChangeLog
AgricultureBrief
ForecastLedger
```

典型问题：

```text
本月 WASDE 调整了哪个作物的 ending stocks？
产量调整来自 yield 还是 area？
出口需求是否被上调？
库存消费比是否偏紧？
市场预期和报告结果差在哪里？
```

## Weather

天气是 agriculture 和 natural gas 的核心变量。

农业看：

```text
drought
flood
heat wave
frost
rainfall
soil moisture
El Nino / La Nina
```

天然气看：

```text
heating degree days
cooling degree days
winter storm
summer heat
```

天气不是单纯新闻。

它会进入：

```text
supply forecast
demand forecast
inventory path
price volatility
option implied volatility
```

## Futures

商品市场很大一部分通过期货交易。

Futures contract 是：

```text
standardized contract to buy or sell an asset at a future date under specified terms.
```

商品期货重要，因为：

```text
producers hedge future selling price
consumers hedge future purchase cost
traders express supply-demand views
investors access commodity exposure
markets reveal forward curve
```

期货不是简单“赌涨跌”。

它是：

```text
risk transfer + price discovery + hedging + financing/storage signal.
```

## Spot, Forward, Futures

三个基础价格：

```text
Spot:
  当前现货价格。

Forward:
  双方约定未来交割价格，通常 OTC。

Futures:
  标准化交易所合约，每日盯市，有保证金。
```

商品研究经常看：

```text
spot price
front-month futures
next-month futures
futures curve
calendar spreads
```

## Futures Curve

期货曲线是商品研究的核心图像。

它描述：

```text
不同到期月份期货合约的价格结构。
```

例如：

```text
Jan contract
Feb contract
Mar contract
...
Dec contract
```

看曲线，不只是看绝对价格。

还要看：

```text
near vs far
front spread
calendar spread
curve slope
curve shape
roll yield
```

## Contango

Contango 通常指：

```text
远月期货价格高于近月 / 现货价格。
```

简化图：

```text
near price < far price
```

可能反映：

```text
storage cost
financing cost
insurance cost
ample inventory
future supply-demand expectation
```

在 contango 中，如果持有多头并不断 roll 到更远月，可能面对负 roll yield。

但具体要看：

```text
curve shape
roll mechanics
spot move
contract selection
transaction cost
```

不能机械说 contango 一定不好。

## Backwardation

Backwardation 通常指：

```text
近月期货价格高于远月。
```

简化图：

```text
near price > far price
```

可能反映：

```text
near-term physical tightness
low inventory
high convenience yield
strong immediate demand
supply disruption
```

Backwardation 常被视为现货紧张信号之一。

但也要谨慎：

```text
不同商品、不同市场状态、不同合约设计下含义不同。
```

## Cost of Carry

商品期货曲线和持有成本相关。

持有商品可能需要：

```text
storage cost
insurance
financing
transportation
quality maintenance
```

这就是 cost of carry。

简化理解：

```text
如果持有实物很贵，远期价格可能需要补偿这些成本。
```

但实际还要考虑：

```text
convenience yield
inventory scarcity
physical demand
market expectations
```

## Convenience Yield

Convenience yield 可以理解为：

```text
持有实物商品带来的非现金便利收益。
```

例如：

```text
炼厂持有原油，可以避免供应中断。
电厂持有燃料，可以保障发电。
生产商持有库存，可以稳定生产。
```

当现货紧张时，convenience yield 可能很高。

这会推动：

```text
near-term prices higher
curve backwardation
```

## Inventory

库存是商品研究的中心变量。

库存连接：

```text
supply
demand
storage
curve
volatility
physical tightness
```

库存高：

```text
市场有缓冲
供需冲击影响可能较小
contango 可能更常见
```

库存低：

```text
市场缺少缓冲
小冲击也可能导致价格大幅波动
backwardation / volatility 可能上升
```

但库存要看：

```text
absolute level
seasonal norm
days of demand cover
location
quality
accessibility
commercial vs strategic inventory
```

不是所有库存都一样。

## Calendar Spread

Calendar spread 是商品期货研究常见对象。

例如：

```text
front month price - second month price
```

或：

```text
Dec contract - Mar contract
```

Spread 反映：

```text
near-term tightness
storage economics
seasonality
roll pressure
physical market state
```

很多商品研究员比起 outright price，更重视 spreads。

因为 spreads 更直接反映：

```text
physical supply-demand balance
```

## Roll Yield

如果投资者通过期货持有商品 exposure，到期前需要换仓。

这叫：

```text
roll
```

Roll yield 来自：

```text
合约换仓过程中的曲线结构影响。
```

在 contango 中：

```text
卖近月低价，买远月高价
roll yield 可能为负
```

在 backwardation 中：

```text
卖近月高价，买远月低价
roll yield 可能为正
```

这对 commodity index 和 ETF 非常重要。

但实际表现仍取决于：

```text
spot move
curve move
contract selection
roll schedule
cost
```

## Commodity Basis

Basis 是：

```text
local cash price - futures price
```

Basis 反映：

```text
location
transportation
quality
storage
local supply-demand
delivery constraint
```

商品是实物市场，所以 location 很重要。

例如：

```text
同样是天然气，不同区域价格可能差很多。
同样是原油，不同质量和交割地点会有差价。
同样是农产品，港口、产区、运输瓶颈都会影响 basis。
```

## Supply-Demand Balance

商品研究最终经常落到供需平衡表。

基础结构：

```text
Beginning inventory
+ Production
+ Imports
- Domestic consumption
- Exports
= Ending inventory
```

或者：

```text
Supply:
  production + imports + beginning stocks

Demand:
  consumption + exports + ending stocks
```

关键不是公式复杂。

关键是判断：

```text
哪个变量在变？
变化是否超预期？
市场是否已经 priced in？
库存路径是否偏紧？
```

## Seasonality

商品有强季节性。

能源：

```text
summer driving season
winter heating demand
hurricane season
refinery maintenance
```

天然气：

```text
winter withdrawals
summer injections
heating degree days
cooling degree days
```

农产品：

```text
planting
growing
pollination
harvest
export season
```

金属：

```text
industrial production cycle
construction season
Chinese New Year effects
```

研究商品，不能忽略季节性。

否则会把正常季节变化误读成结构变化。

## Geopolitics

商品和地缘政治高度相关。

典型事件：

```text
war
sanctions
shipping disruption
export ban
pipeline sabotage
OPEC policy
trade restrictions
tariffs
strategic reserve release
port strike
```

地缘事件影响：

```text
supply availability
transport routes
insurance cost
risk premium
inventory hoarding
price volatility
```

能源最明显，但金属和农产品也会受到影响。

## USD and Rates

很多商品以美元计价。

因此美元和利率很重要。

```text
USD stronger
  -> commodities may become more expensive for non-USD buyers
  -> commodity prices may face pressure

real rates higher
  -> opportunity cost for non-yielding assets like gold may rise
```

但不能机械。

如果商品出现强供给冲击，美元和利率影响可能被压过。

所以要区分：

```text
macro-financial driver
physical supply-demand driver
geopolitical driver
positioning driver
```

## Commodities and Inflation

商品是通胀的重要来源。

尤其：

```text
energy
food
industrial inputs
```

商品价格上升可能推高：

```text
headline CPI
PPI
transportation cost
input cost
household energy bills
food prices
```

但商品到通胀的传导要看：

```text
weight in index
pass-through
substitution
tax / subsidy
currency
corporate margin absorption
policy response
```

这连接回 Fixed Income：

```text
commodity shock
  -> inflation expectation
  -> central bank path
  -> yield curve
```

## Commodity-Linked FX

商品和货币也强相关。

部分货币被称为 commodity-linked currencies：

```text
AUD
CAD
NZD
NOK
BRL
CLP
ZAR
```

例如：

```text
oil and CAD / NOK
iron ore and AUD
copper and CLP
agriculture and BRL
gold/platinum and ZAR
```

但这种关系也不是固定。

还要看：

```text
rates differential
risk sentiment
domestic policy
capital flows
terms of trade
external balance
```

这就是为什么 FICC 三块不能割裂。

## Commodity Research Workflow

一个 commodity research workflow 可以这样做：

```text
1. Define commodity
   oil / gas / copper / gold / wheat / corn / soybeans

2. Identify market structure
   spot, futures, physical market, key benchmarks

3. Supply side
   production, outages, policy, capacity, imports

4. Demand side
   consumption, industrial cycle, weather, exports

5. Inventory
   level, seasonal norm, location, draw/build

6. Futures curve
   contango / backwardation, calendar spreads, roll yield

7. Macro overlay
   USD, rates, growth, inflation, risk sentiment

8. Event risk
   OPEC, EIA, WASDE, weather, geopolitics, policy

9. Scenario
   base / bull / bear

10. What to monitor
   3-5 key indicators
```

这很适合做 Research OS。

## Commodity Daily Brief Template

可以设计一个公开安全的商品日报模板：

```text
Date:

Commodity:
  oil / gas / gold / copper / soybeans

Market Move:
  spot:
  front-month futures:
  key spread:
  curve shape:

Supply Update:
  production / outage / OPEC / mine / crop / exports

Demand Update:
  refinery / power / industrial / China / weather / biofuel

Inventory:
  latest level:
  vs seasonal norm:
  draw/build:

Macro Overlay:
  USD:
  rates:
  risk sentiment:
  inflation:

Event Risk:
  EIA / WASDE / OPEC / weather / geopolitics

Interpretation:
  what changed?
  what matters?
  what to monitor?

Public-safe conclusion:
  no trading advice
```

这能成为：

```text
FICC Daily Brief Generator
```

的一部分。

## Energy Brief Template

能源专用：

```text
Crude:
  WTI:
  Brent:
  Brent-WTI:

Curve:
  front spread:
  6-month spread:
  contango / backwardation:

Inventory:
  crude:
  gasoline:
  distillate:
  Cushing:

Supply:
  OPEC:
  U.S. production:
  exports/imports:
  outages:

Demand:
  refinery utilization:
  gasoline demand:
  distillate demand:
  jet fuel:

Risk:
  geopolitics:
  sanctions:
  weather:
  policy:
```

## Metals Brief Template

金属专用：

```text
Metal:
  gold / copper / aluminum / iron ore

Price move:
  spot:
  futures:
  spread:

Macro:
  USD:
  real rates:
  China growth:
  PMI:

Supply:
  mine output:
  smelter:
  disruptions:
  energy cost:

Demand:
  construction:
  manufacturing:
  grid / EV / renewables:

Inventory:
  exchange stocks:
  bonded stocks:
  warehouse trend:

Risk:
  policy:
  trade:
  sanctions:
```

## Agriculture Brief Template

农产品专用：

```text
Crop:
  corn / wheat / soybeans / cotton

Report:
  WASDE / crop progress / export sales

Supply:
  planted area:
  yield:
  production:
  weather:

Demand:
  feed:
  food:
  biofuel:
  exports:

Inventory:
  ending stocks:
  stocks-to-use:
  vs expectation:

Seasonality:
  planting / growing / harvest / export

Risk:
  drought:
  flood:
  policy:
  currency:
```

## Commodities x RAG

商品 RAG 可以处理：

```text
EIA petroleum reports
USDA WASDE reports
OPEC statements
mine production updates
weather reports
company production guidance
port / shipping news
government policy documents
commodity exchange notices
research PDFs
```

典型问题：

```text
本周 EIA 原油库存变化主要来自 supply 还是 demand？
WASDE 本月对玉米 ending stocks 的调整来自哪里？
铜库存下降是需求强还是供应扰动？
黄金近期变化更像 real rates 还是 safe-haven demand？
天然气库存路径是否偏离季节性？
```

RAG 输出必须包含：

```text
source
timestamp
evidence
uncertainty
what to monitor
human review required
```

不能让模型凭空生成商品观点。

## Commodities x Graph

商品非常适合 graph。

Energy graph：

```text
crude oil
  -> OPEC policy
  -> U.S. shale
  -> inventories
  -> refinery runs
  -> gasoline / diesel
  -> inflation
  -> rates
```

Copper graph：

```text
copper
  -> mine supply
  -> smelter capacity
  -> China demand
  -> construction
  -> power grid
  -> EV / renewables
  -> inventories
  -> USD / risk sentiment
```

Agriculture graph：

```text
corn
  -> planted area
  -> weather
  -> yield
  -> production
  -> feed demand
  -> ethanol demand
  -> exports
  -> ending stocks
```

Graph RAG 的价值：

```text
把事件、地区、库存、供需、价格、宏观变量连起来。
```

## Commodities x Quant

公开安全地说，商品 quant 可以研究：

```text
term structure signals
inventory surprise
seasonality
carry / roll yield
momentum
volatility
curve shape
cross-commodity relationships
macro factor sensitivity
event studies
forecast evaluation
```

但不能公开：

```text
具体参数
实盘信号
未脱敏回测
交易团队观点
客户 flow
内部库存/报价
可复制 alpha
```

公开 demo 可以做：

```text
commodity curve visualizer
EIA inventory brief generator
WASDE change summarizer
contango/backwardation explainer
gold-real-yield educational dashboard
copper macro graph
```

这展示的是：

```text
research engineering ability
not proprietary trading edge
```

## Commodities x Forecast Ledger

商品观点很适合做 forecast ledger。

每条观点记录：

```text
timestamp
commodity
event
base view
bull scenario
bear scenario
evidence
market baseline
what to monitor
review date
outcome
post-mortem
```

例如：

```text
event:
  EIA inventory draw larger than expected

view:
  near-term physical tightness increased

monitor:
  refinery utilization, exports, Cushing stocks, front spread

review:
  one week later
```

这不是交易建议。

这是研究判断的可审计化。

## Pengyi Commodities Research OS v0

我们可以设计：

```text
Pengyi Commodities Research OS v0
```

模块：

```text
Data Layer:
  public futures prices, curves, inventory data, macro data

Document Layer:
  EIA, USDA, OPEC, exchange notices, public reports

RAG Layer:
  source-grounded commodity document retrieval

Graph Layer:
  commodity -> supply -> demand -> inventory -> macro -> risk graph

Curve Layer:
  contango / backwardation / spreads / roll

Inventory Layer:
  draw/build, seasonal norm, location

Event Layer:
  weather, policy, geopolitics, production disruptions

Forecast Ledger:
  timestamped views and reviews

Brief Layer:
  energy brief, metals brief, agriculture brief

Human Review:
  PM / analyst approval before any conclusion

Artifact Layer:
  website note, chart, memo, slide
```

这可以和前面项目连接：

```text
LightRAG:
  commodity report memory

RAG-Anything:
  PDF/table/chart ingestion

GraphAgent:
  supply-demand relationship graph

FutureShow:
  forecast ledger and outcome review

Vibe-Trading:
  research workflow and artifact generation

MGP:
  memory governance and audit
```

## 和 Fixed Income 的连接

Commodities 和 Fixed Income 的连接主要通过：

```text
inflation
real rates
central bank policy
growth expectation
risk sentiment
```

例如：

```text
oil up
  -> inflation pressure
  -> rates repricing
  -> curve changes
  -> credit conditions
```

```text
gold up
  -> real rates down / safe-haven demand up / USD down
  -> rates and FX context needed
```

```text
copper down
  -> growth concern
  -> long-end rates may fall
  -> credit spreads may widen
```

所以不懂 FI，很难完整理解 commodities 的宏观含义。

## 和 FX 的连接

Commodities 和 FX 的连接主要通过：

```text
terms of trade
export revenue
import cost
inflation
rate differential
risk sentiment
```

例子：

```text
oil exporters:
  oil price up may support external balance

commodity importers:
  energy price up may worsen trade balance

AUD:
  linked to iron ore, China demand, risk sentiment

CAD / NOK:
  linked to oil, rates, global risk

CLP:
  linked to copper
```

这也解释了为什么 `FICC002` 的 FX 和这一篇必须连起来看。

商品和外汇是强连接。

## 面试可用表达

如果被问：

```text
你怎么理解 Commodities？
```

可以回答：

```text
我理解 Commodities 是 FICC 中最具有实物约束的一块。
它不只是价格时间序列，而是 physical market, futures market, macro market and geopolitical market 的结合。

我会先分成 Energy, Metals 和 Agriculture。
Energy 关注原油、天然气、成品油、库存、OPEC、炼厂、地缘和天气。
Metals 关注黄金、铜、铝、铁矿等，其中黄金更受 real rates、USD 和避险需求影响，铜更受工业周期、中国需求、矿山供给和库存影响。
Agriculture 关注天气、种植面积、单产、库存消费比、出口和 WASDE 等供需报告。

商品研究里我会重点看 futures curve、contango/backwardation、inventory、calendar spread、roll yield、supply-demand balance 和 seasonality。

我现在更感兴趣的是把商品研究 workflow 和 AI Research OS 结合起来，例如 EIA/WASDE RAG、commodity knowledge graph、curve monitor、inventory brief generator、forecast ledger 和 human-reviewed commodity research assistant。
```

这段可以用于：

```text
FICC research support
commodities research
AI for finance
quant research interview
bank internal rotation
RA / PhD narrative
```

## 常见误区

误区一：

```text
商品只看供需。
```

更准确：

```text
商品看供需、库存、曲线、地缘、天气、仓储、运输、金融条件和持仓。
```

误区二：

```text
库存下降价格一定涨。
```

更准确：

```text
要看库存下降是否超预期、是否季节性、发生在哪里、质量如何、曲线是否确认 tightness。
```

误区三：

```text
Contango 一定 bearish，backwardation 一定 bullish。
```

更准确：

```text
它们反映 futures curve structure，常与库存、持有成本和现货紧张程度相关，但不能脱离具体商品和市场状态解释。
```

误区四：

```text
黄金只看通胀。
```

更准确：

```text
黄金要看 real rates、USD、央行购金、避险需求、ETF flows、地缘风险和市场结构。
```

误区五：

```text
铜涨就一定说明经济好。
```

更准确：

```text
铜价可能由需求、供给、库存、政策预期、美元、风险偏好和持仓共同驱动。
```

## 学习顺序

Commodities 初学顺序：

```text
1. Spot / futures / forward
2. Futures curve
3. Contango / backwardation
4. Inventory
5. Supply-demand balance
6. Energy: crude oil / natural gas
7. Metals: gold / copper
8. Agriculture: WASDE / weather / stocks-to-use
9. Seasonality and event risk
10. Cross-asset link: rates / FX / inflation / risk sentiment
```

不要一开始就做复杂策略。

先把：

```text
physical market
inventory
curve
supply-demand
macro overlay
```

这些地基打牢。

## 下一篇

现在 FICC 三大基础块已经齐了：

```text
FICC001 -> Fixed Income / Rates / Credit
FICC002 -> Currencies / FX
FICC003 -> Commodities
```

下一篇最自然进入产品化和工程化：

```text
FICC004 -> Daily FICC Brief Generator
```

也就是把 FI、FX、Commodities 三条线合成一个每日研究工作流：

```text
macro calendar
rates / curve monitor
FX pair monitor
commodity inventory and curve monitor
news and event extraction
cross-asset causal chain
forecast ledger
human review
```

## 当前结论

Commodities 可以压成：

```text
Energy:
  oil, gas, refined products, inventory, OPEC, weather, refinery

Metals:
  gold, copper, industrial cycle, real rates, USD, China demand, mine supply

Agriculture:
  crops, weather, WASDE, ending stocks, stocks-to-use, exports

Market structure:
  spot, futures, forwards, curve, contango, backwardation, spreads, roll yield

Physical structure:
  supply, demand, inventory, storage, transportation, seasonality

Macro link:
  inflation, rates, FX, risk sentiment, geopolitics

Research OS:
  RAG + graph + curve monitor + inventory brief + forecast ledger + human review
```

这就是 FICC003 的核心。

商品不是简单的“价格涨跌”。

它是：

```text
physical constraints + financial contracts + macro forces + geopolitical events.
```

这正好适合我们用 AI Research OS 去结构化、检索、追踪和复盘。

## References

- CFTC Futures Glossary: <https://www.cftc.gov/LearnAndProtect/AdvisoriesAndArticles/CFTCGlossary/index.htm>
- CFTC official site: <https://www.cftc.gov/>
- U.S. EIA Weekly Petroleum Status Report: <https://www.eia.gov/petroleum/supply/weekly/>
- U.S. EIA Petroleum & Other Liquids: <https://www.eia.gov/petroleum/>
- USDA WASDE Report: <https://www.usda.gov/about-usda/general-information/staff-offices/office-chief-economist/commodity-markets/wasde-report>
- USDA WASDE publication page: <https://esmis.nal.usda.gov/publication/world-agricultural-supply-and-demand-estimates>
- CME Group, Contango and Backwardation: <https://www.cmegroup.com/education/courses/introduction-to-ferrous-metals/what-is-contango-and-backwardation>
