---
title: "FICC000: FICC 总地图 - Fixed Income / Currencies / Commodities 三大类"
date: 2026-07-02 00:00:00 +0800
categories: [Learning, Finance]
tags: [ficc000, ficc, fixed-income, currencies, commodities, rates, credit, fx, macro, quant-research, research-os]
---

这是 `PENGYI_FICC_MAP` 的第一篇：

```text
FICC000 -> FICC 总地图
```

这一篇先做总览。

目标不是直接写交易策略。

目标是把 FICC 的基本版图讲清楚：

```text
FICC = Fixed Income + Currencies + Commodities
```

中文通常可以理解为：

```text
固定收益 / 利率信用
外汇
商品
```

更准确地说，FICC 是金融市场里非常核心的一大块业务和研究场景。

它连接：

```text
宏观经济
央行政策
利率曲线
信用风险
汇率
商品供需
流动性
衍生品
客户风险管理
资产配置
做市与交易
```

对我们来说，FICC 不是孤立的金融知识。

它可以成为：

```text
AI Research OS 的真实金融场景
Quant Research OS 的宏观与跨资产方向
RAG / Agent / Forecasting / Research Automation 的业务落点
```

## 一句话总览

如果用一句话解释 FICC：

```text
FICC 是围绕利率、信用、汇率和商品价格进行定价、交易、风险管理、研究和客户服务的金融市场体系。
```

更工程化一点：

```text
FICC = macro variables + market instruments + risk factors + liquidity + derivatives + institutional workflow.
```

最核心的三个板块：

```text
FI:
  Fixed Income
  包括 rates、bonds、credit、repo、swaps、credit derivatives 等。

C:
  Currencies
  包括 FX spot、forwards、swaps、options、cross-currency basis、carry 等。

C:
  Commodities
  包括 energy、metals、agriculture、commodity futures、curve structure、inventory、geopolitics 等。
```

其中要注意：

```text
Fixed Income 不是只指“固定收益产品”。
在 markets 语境里，FI 往往同时覆盖利率、债券、信用、融资、衍生品和曲线风险。
```

## 为什么 FICC 重要

FICC 的重要性来自三点。

第一，它是宏观变量的交易场。

```text
GDP
inflation
employment
monetary policy
fiscal policy
liquidity
geopolitics
trade balance
energy supply
credit cycle
```

这些东西最后都会反映到：

```text
interest rates
yield curves
credit spreads
exchange rates
commodity prices
volatility
liquidity premium
```

第二，它是机构客户风险管理的主场。

企业、银行、资管、保险、基金、跨国公司都需要管理：

```text
利率风险
汇率风险
信用风险
商品价格风险
流动性风险
再融资风险
```

第三，它天然适合 AI / Quant / Research OS。

因为 FICC 研究每天都在处理：

```text
新闻
政策
宏观数据
央行讲话
收益率曲线
利差
汇率
商品库存
研报
客户问题
风险提示
交易想法
```

这正好适合：

```text
RAG
Graph RAG
forecast ledger
agent workflow
research brief generator
event tracking
cross-asset knowledge graph
human PM review
```

## 总体结构

FICC 可以先这样看：

| 大类 | 中文 | 核心对象 | 关键变量 | 典型问题 |
|---|---|---|---|---|
| Fixed Income | 固收 / 利率信用 | 债券、利率、信用、repo、swap | yield、curve、duration、spread、default | 利率往哪走？曲线怎么变？信用利差如何定价？ |
| Currencies | 外汇 | FX spot、forward、swap、option | interest differential、carry、vol、balance of payments | 汇率由什么驱动？央行分化如何影响 FX？ |
| Commodities | 商品 | 能源、金属、农产品、期货曲线 | supply-demand、inventory、term structure、geopolitics | 商品价格由供需、库存、地缘和金融条件如何驱动？ |

还有一层贯穿三者：

```text
Derivatives:
  futures
  options
  swaps
  forwards

Risk:
  rate risk
  credit risk
  FX risk
  commodity risk
  liquidity risk
  basis risk
```

FICC 的本质不是背产品名字。

真正要理解的是：

```text
市场如何把宏观预期、风险偏好、现金流和供需冲击折现到价格里。
```

## FI: Fixed Income

先讲 FI。

Fixed Income 字面是固定收益。

但在 FICC 里，FI 通常包含：

```text
Rates
Credit
Bonds
Money markets
Repo
Swaps
Credit derivatives
Structured credit
```

FI 的核心问题是：

```text
未来现金流如何被贴现？
利率如何影响债券价格？
信用风险如何通过 spread 被定价？
资金面和流动性如何影响曲线？
```

## FI 的核心资产

常见 FI 产品：

```text
Government bonds
  国债 / Treasury / sovereign bonds

Corporate bonds
  公司债 / investment grade / high yield

Municipal bonds
  市政债

Agency bonds
  政府机构债

Mortgage-backed securities
  MBS

Asset-backed securities
  ABS

Money market instruments
  bills, commercial paper, CDs

Repo
  repurchase agreements

Interest rate swaps
  fixed-for-floating swaps

Credit default swaps
  CDS

Bond futures and options
  rates derivatives
```

如果简化：

```text
债券 = 借钱给发行人，未来收到利息和本金。
```

但在市场里，债券不是静态持有到期这么简单。

债券每天都在被重新定价。

驱动因素包括：

```text
risk-free rate
yield curve
inflation expectation
central bank policy
credit spread
liquidity premium
default probability
recovery expectation
supply-demand
term premium
```

## Rates 和 Credit

FI 可以先拆成两条线：

```text
Rates:
  利率线

Credit:
  信用线
```

Rates 关注：

```text
央行政策
政策利率
收益率曲线
通胀预期
实际利率
期限溢价
短端资金面
长端增长和通胀预期
swap curve
repo market
```

Credit 关注：

```text
发行人偿债能力
行业景气
财务杠杆
现金流
评级变化
违约概率
回收率
信用利差
流动性溢价
credit cycle
```

Rates 更像：

```text
宏观 + 央行 + 曲线定价
```

Credit 更像：

```text
公司 / 行业 / 杠杆 / 风险补偿
```

当然两者会相互影响。

例如：

```text
央行加息 -> 无风险利率上升 -> 融资成本上升 -> 信用风险可能恶化 -> credit spreads 可能走阔
```

## FI 的核心变量

学习 Fixed Income，必须理解这些变量：

```text
Price
Yield
Coupon
Maturity
Duration
Convexity
DV01 / PV01
Yield curve
Spread
OAS
Credit rating
Default probability
Recovery rate
Liquidity premium
Repo rate
Swap rate
Basis
```

最基础关系：

```text
利率上升，固定利率债券价格通常下降。
利率下降，固定利率债券价格通常上升。
```

原因是：

```text
债券现金流固定。
贴现率上升时，同一组未来现金流的现值下降。
```

这个关系是 FI 的地基。

但真实交易里还要看：

```text
曲线哪一段动？
短端还是长端？
平行移动还是 steepening / flattening？
信用利差是否同步变化？
流动性是否变化？
债券久期是多少？
凸性如何？
```

## Yield Curve

收益率曲线是 FI 的核心图像。

它描述：

```text
不同期限债券的收益率。
```

常见期限：

```text
1M
3M
6M
1Y
2Y
5Y
10Y
30Y
```

常见曲线变化：

```text
parallel shift
  整条曲线平移

steepening
  长端相对短端上升，或短端相对长端下降

flattening
  长短利差收窄

inversion
  短端收益率高于长端
```

研究问题：

```text
央行加息主要影响短端还是长端？
通胀预期如何影响长端？
经济衰退预期如何压低长端？
资金面紧张如何影响短端？
财政发行供给如何影响长端 term premium？
```

## Duration

Duration 可以先理解成：

```text
债券价格对利率变化的敏感度。
```

久期越长，利率变动对价格影响越大。

简单直觉：

```text
短久期债券:
  现金流很快回来，对利率变化不太敏感。

长久期债券:
  现金流在很远未来，对贴现率更敏感。
```

这对风险管理非常关键。

组合层面会看：

```text
portfolio duration
DV01
key rate duration
curve exposure
hedge ratio
```

## Credit Spread

Credit spread 是信用市场的核心变量。

可以理解成：

```text
信用债收益率 - 无风险利率
```

它补偿投资者承担：

```text
default risk
liquidity risk
downgrade risk
sector risk
market stress risk
```

Spread 走阔通常意味着：

```text
信用风险补偿上升
市场风险偏好下降
流动性变差
发行人或行业风险恶化
```

Spread 收窄通常意味着：

```text
风险偏好改善
信用环境改善
流动性改善
对违约风险要求的补偿下降
```

Credit 研究不仅看宏观，也看微观。

```text
财务报表
现金流
杠杆率
偿债覆盖
行业周期
再融资压力
评级迁移
债务结构
担保和抵押
```

这和银行对公、授信、风控经验是有连接的。

## FI Research Workflow

一个 FI research workflow 可以是：

```text
1. Macro update
   央行、通胀、就业、财政、资金面

2. Curve move
   短端、中端、长端变化

3. Rates view
   policy path、term premium、real yield

4. Credit update
   spreads、rating、sector、issuer risk

5. Relative value
   curve segment、bond vs swap、cash vs derivative

6. Risk check
   duration、spread duration、liquidity、basis

7. Client implication
   financing cost、hedging、investment allocation
```

这很适合做 AI assistant。

因为每天都需要读材料、提取事件、更新曲线、写简报和保留观点。

## C: Currencies

第二个 C 是 Currencies，也就是外汇。

FX 的核心问题是：

```text
一种货币相对于另一种货币的价格如何变化？
```

最常见对象：

```text
EUR/USD
USD/JPY
GBP/USD
USD/CNY
USD/CNH
AUD/USD
USD/CHF
USD/CAD
```

外汇市场非常宏观。

驱动因素包括：

```text
interest rate differential
central bank policy divergence
inflation
growth
trade balance
capital flows
risk sentiment
commodity exposure
geopolitics
intervention risk
liquidity
positioning
```

## FX 的核心产品

常见 FX 产品：

```text
Spot
  即期外汇

Forward
  远期外汇

FX Swap
  外汇掉期

Currency Swap
  货币互换

FX Option
  外汇期权

NDF
  non-deliverable forward

Cross-currency basis swap
  跨货币基差互换
```

初学可以先抓住：

```text
spot:
  今天的汇率

forward:
  未来某一天约定汇率

swap:
  近端和远端交换现金流

option:
  在一定条件下买卖货币的权利
```

## FX 的核心变量

FX 研究需要看：

```text
spot rate
forward points
interest rate differential
carry
real yield
inflation differential
current account
capital flow
reserve policy
central bank reaction function
volatility
risk reversal
skew
positioning
```

最重要的直觉之一：

```text
汇率不是单一变量驱动。
汇率是两国宏观、利率、资本流动和风险偏好的相对价格。
```

例如：

```text
USD/CNY
```

不只是看中国。

还要看：

```text
US rates
China rates
growth differential
export cycle
capital flows
policy stance
risk sentiment
CNH liquidity
market expectation
```

## Carry

Carry 是 FX 里非常重要的概念。

简单理解：

```text
借低利率货币，买高利率货币，赚取利差。
```

但 carry 不是免费午餐。

风险包括：

```text
汇率反向波动
风险偏好崩塌
流动性冲击
央行政策突变
crowded positioning
volatility spike
```

所以 FX research 不能只看利差。

还要看：

```text
volatility
drawdown
tail risk
funding liquidity
positioning
policy risk
```

## FX 和央行

FX 和央行关系极强。

央行影响汇率的方式包括：

```text
policy rate
forward guidance
balance sheet
FX intervention
capital control
liquidity operations
communication
```

研究汇率，必须理解：

```text
央行 reaction function
```

也就是：

```text
在什么经济状态下，央行倾向于加息、降息、维持、干预、释放流动性或收紧流动性？
```

这和 rates 研究高度相连。

## FX Research Workflow

一个 FX research workflow 可以是：

```text
1. Identify currency pair
   例如 USD/CNH, EUR/USD, USD/JPY

2. Macro differential
   增长、通胀、政策、贸易、资本流动

3. Rates differential
   nominal rates、real rates、yield spread

4. Central bank path
   policy divergence / convergence

5. Technical market state
   volatility、positioning、liquidity

6. Event risk
   data release、election、geopolitics、intervention

7. Scenario
   base / upside / downside
```

这也很适合 AI Research OS。

因为 FX 每天有大量文本和数据：

```text
央行讲话
经济数据
市场评论
汇率走势
远期点
波动率
新闻事件
```

## C: Commodities

第三个 C 是 Commodities，也就是商品。

商品的核心问题是：

```text
实物供需、库存、运输、季节性、地缘政治和金融条件如何共同决定商品价格？
```

商品不是一个统一市场。

它可以分成：

```text
Energy
  crude oil, natural gas, refined products, coal

Metals
  gold, silver, copper, aluminum, nickel, iron ore

Agriculture
  corn, wheat, soybeans, coffee, sugar, cotton

Livestock
  cattle, hogs

Carbon / power / new energy linked products
  power, emissions, lithium-linked markets, etc.
```

商品研究非常具体。

每个品种都有自己的产业链、库存、运输和季节性。

## Commodities 的核心产品

常见商品金融产品：

```text
Spot physical commodity
  现货

Futures
  期货

Options
  期权

Swaps
  互换

Forwards
  远期

ETFs / ETCs
  交易型产品
```

FICC 桌通常更关注：

```text
futures curve
hedging demand
producer / consumer flows
inventory
macro linkage
commodity-linked FX
inflation implication
```

## Commodities 的核心变量

商品研究需要看：

```text
supply
demand
inventory
production
transportation
storage cost
seasonality
weather
geopolitics
OPEC / producer policy
industrial cycle
China demand
USD
real rates
futures curve
roll yield
convenience yield
```

商品和股票、债券、外汇不同的一点：

```text
商品背后有真实物理约束。
```

例如：

```text
原油有产能、运输、库存、炼厂需求。
天然气有管道、LNG、天气、储气。
铜有矿山供给、冶炼、工业需求。
农产品有天气、种植面积、收成、库存消费比。
黄金有实际利率、美元、避险需求、央行购金。
```

所以商品研究很适合知识图谱。

因为每个品种都可以建：

```text
commodity -> supply chain -> region -> producer -> inventory -> demand sector -> macro driver
```

## Futures Curve

商品期货曲线非常重要。

常见形态：

```text
Contango
  远月价格高于近月

Backwardation
  近月价格高于远月
```

Contango 可能反映：

```text
库存充足
持有成本
未来供需预期
融资和仓储成本
```

Backwardation 可能反映：

```text
现货紧张
库存偏低
即时需求强
convenience yield 高
```

这不是绝对规则。

不同商品和不同市场状态下要具体分析。

## Commodities Research Workflow

一个 commodities research workflow 可以是：

```text
1. Choose commodity
   oil / gas / copper / gold / soybeans etc.

2. Supply side
   production, disruptions, OPEC, mines, weather

3. Demand side
   industrial demand, transport, power, China demand, consumption

4. Inventory
   current level, seasonal norm, draw/build speed

5. Curve structure
   contango / backwardation / spread

6. Macro overlay
   USD, real rates, growth, inflation, risk appetite

7. Scenario
   base / bull / bear
```

商品研究特别适合：

```text
event tracking
knowledge graph
supply-chain RAG
forecast ledger
scenario generator
```

## FICC 三者如何联动

FICC 真正难的地方在跨资产联动。

不是分别看 FI、FX、Commodities。

而是看：

```text
利率如何影响汇率？
汇率如何影响商品？
商品如何影响通胀？
通胀如何影响央行？
央行如何影响利率曲线？
信用利差如何反映风险偏好？
风险偏好如何影响高收益债、EM FX 和商品？
```

一个典型链条：

```text
oil price up
  -> inflation pressure up
  -> central bank hawkish repricing
  -> rates up
  -> bond prices down
  -> USD may strengthen
  -> EM FX pressure
  -> credit spreads may widen if growth risk rises
```

另一个链条：

```text
growth slowdown
  -> commodity demand weaker
  -> inflation pressure lower
  -> rates rally
  -> yield curve bull steepening / flattening depending on policy
  -> credit spreads widen if recession risk rises
  -> safe-haven FX demand
```

这些联动很适合做 graph。

```text
macro event
  -> rates
  -> FX
  -> commodities
  -> credit
  -> risk sentiment
  -> portfolio impact
```

## FICC Research 的常见产物

FICC 研究团队常见产物：

```text
daily market brief
weekly macro note
rates strategy note
FX strategy note
credit market update
commodity market update
event preview
event aftermath
client presentation
risk scenario analysis
trade idea note
hedging proposal
curve monitor
spread monitor
forecast tracker
```

这些都可以转成 Research OS artifact：

```text
BriefCard
EventCard
CurveCard
SpreadCard
FXPairCard
CommodityCard
ScenarioCard
RiskCard
ClientQuestionCard
```

## FICC Analyst 每天看什么

一个简化 daily workflow：

```text
1. Overnight market move
   全球利率、FX、商品、股指、信用

2. Macro calendar
   CPI、PMI、就业、GDP、央行会议、财政事件

3. Rates curve
   yields, swaps, curve slope, funding stress

4. Credit
   spreads, issuance, rating, defaults, sector news

5. FX
   major pairs, CNH/CNY, DXY, carry, vol

6. Commodities
   oil, gold, copper, gas, inventories, geopolitics

7. News and policy
   central banks, ministries, regulators, geopolitical events

8. Client relevance
   hedging, financing, allocation, product demand

9. Risk view
   what changed, what matters, what to monitor
```

这正好可以做：

```text
Daily FICC Brief Generator
```

## FICC x AI 的自然结合点

FICC 场景很适合 AI，但不能乱用。

合理方向：

```text
1. RAG research memory
   让公开政策、研报、市场日报、产品说明可检索。

2. Daily brief automation
   把市场数据、新闻、事件和曲线变化整理成初稿。

3. Event tracker
   跟踪央行、宏观数据、地缘事件、商品供需事件。

4. Scenario generator
   生成 base / bull / bear 情景，但必须有人审。

5. Knowledge graph
   建立 macro -> rates -> FX -> commodities -> credit 的联动图。

6. Forecast ledger
   记录观点、时间戳、证据、市场基准和事后复盘。

7. Client question assistant
   帮研究员检索资料和生成结构化回答初稿。
```

不合理方向：

```text
让 LLM 直接下交易指令
让模型无审查生成实盘建议
把内部敏感数据直接放进外部模型
把未验证观点包装成确定性结论
忽视合规、权限、审计和 human review
```

FICC AI 的核心原则：

```text
AI can support research workflow.
AI should not bypass human accountability.
```

## FICC x RAG

FICC RAG 可以处理的材料：

```text
central bank statements
monetary policy reports
macro data releases
market daily notes
bond prospectuses
credit rating reports
commodity inventory reports
research PDFs
product documentation
internal public-safe training notes
```

一个 FICC RAG 问题：

```text
近期央行流动性操作如何影响短端利率？
```

系统应该返回：

```text
source-backed answer
relevant policy documents
historical operations
market rate response
uncertainty
what to monitor next
```

它不应该返回：

```text
无来源的方向性交易建议
```

这就是边界。

## FICC x Graph

FICC 很适合图结构。

一个 macro graph：

```text
Fed policy
  -> US rates
  -> USD
  -> EM FX
  -> commodity prices
  -> inflation expectation
  -> credit spreads
```

一个 credit graph：

```text
issuer
  -> sector
  -> parent company
  -> subsidiaries
  -> bond issues
  -> rating
  -> spread
  -> refinancing wall
```

一个 commodity graph：

```text
oil
  -> producer countries
  -> OPEC policy
  -> inventory
  -> refinery demand
  -> transportation
  -> inflation
  -> FX of commodity exporters
```

Graph RAG 的价值是：

```text
不只找相似文本。
还要找变量之间的关系。
```

## FICC x Quant

FICC quant 不等于 equity factor。

它更强调：

```text
time series
macro regimes
curve dynamics
relative value
volatility
carry
roll-down
basis
liquidity
scenario analysis
risk decomposition
```

常见研究问题：

```text
yield curve slope 如何预测宏观周期？
real rates 和 gold 的关系如何变化？
interest rate differential 如何影响 FX carry？
credit spreads 对 equity volatility 有什么反应？
oil inventory surprise 如何影响 futures curve？
```

公开 demo 可以做：

```text
toy curve monitor
public macro event tracker
public FX carry explainer
commodity curve visualizer
source-grounded FICC brief generator
```

但不能公开：

```text
具体 alpha 参数
实盘信号
未脱敏回测
内部数据
交易团队观点
客户信息
```

## FICC x Research OS

我们的 `Pengyi FICC Research OS v0` 可以这样设计：

```text
Data Layer:
  public rates, FX, commodity, macro data

Document Layer:
  central bank docs, public reports, market notes

RAG Layer:
  source-grounded retrieval

Graph Layer:
  macro -> rates -> FX -> commodity -> credit relations

Forecast Ledger:
  timestamped views and later review

Brief Generator:
  daily/weekly public-safe market summaries

Human Review:
  analyst / PM approval before any conclusion

Artifact Layer:
  website note, chart, memo, slide, demo
```

这可以和我们前面做的项目连接：

```text
LightRAG:
  FICC knowledge memory

RAG-Anything:
  PDF / table / chart ingestion

VideoRAG:
  central bank speech / seminar / interview memory

FutureShow:
  forecast ledger

Vibe-Trading:
  research workflow / backtest inspiration

X2Strategy:
  research idea -> structured hypothesis

QuantMind:
  financial knowledge structuring

MGP:
  memory governance
```

## FICC 对我们个人路线的意义

FICC 对我们不是随机方向。

它连接了三块：

```text
1. 银行经历
   信用、授信、合同、客户、资金、风险、组织约束。

2. Quant 背景
   市场、因子、信号、回测、风险、统计判断。

3. AI Research OS
   RAG、Agent、Forecasting、Knowledge Graph、Research Automation。
```

银行经历可以转译成：

```text
真实金融机构场景理解
信用和客户风险意识
合规与流程约束意识
金融业务材料处理经验
```

FICC 则把这些往市场研究方向推进：

```text
macro
rates
credit
FX
commodities
cross-asset
```

这就是为什么 FICC 能成为我们 A 面和 B 面之间的桥。

## 初学 FICC 的学习顺序

我建议这样学：

```text
Step 1:
  先懂利率和债券。

Step 2:
  再懂收益率曲线和 duration。

Step 3:
  再懂信用利差和信用周期。

Step 4:
  再懂 FX 的 spot / forward / carry。

Step 5:
  再懂商品的供需 / 库存 / 期货曲线。

Step 6:
  最后做跨资产联动。
```

具体路线：

```text
FICC001:
  Fixed Income / Rates / Credit

FICC002:
  Currencies / FX / Carry / Central Bank Divergence

FICC003:
  Commodities / Futures Curve / Supply-Demand

FICC004:
  FICC Daily Brief Workflow

FICC005:
  FICC x RAG / Agent / Quant Research OS
```

## 面试可用表达

如果被问：

```text
你怎么理解 FICC？
```

可以这样回答：

```text
我理解 FICC 是 Fixed Income, Currencies and Commodities。
它不是单一产品线，而是围绕利率、信用、汇率和商品价格进行定价、交易、风险管理和研究支持的金融市场体系。

Fixed Income 里我会先拆成 rates 和 credit：
rates 关注央行政策、收益率曲线、duration、swap curve 和资金面；
credit 关注发行人偿债能力、行业风险、信用利差、评级迁移和流动性。

Currencies 关注汇率、利差、carry、央行政策分化、资本流动和风险偏好。

Commodities 关注能源、金属、农产品等实物供需、库存、期货曲线、地缘政治和宏观联动。

我现在更感兴趣的是把 FICC 研究 workflow 和 AI infra 结合起来，例如 source-grounded RAG、daily brief generator、macro event tracker、forecast ledger 和 human-reviewed research assistant。
```

这段可以用于：

```text
FICC research support
AI for finance
quant research interview
bank internal rotation
RA / PhD AI-finance narrative
```

## Public-safe 边界

这篇是公开学习笔记。

所以必须明确边界：

```text
This is educational research infrastructure material.
It is not trading advice.
It does not contain proprietary data, internal views, client information, or actionable alpha.
```

中文：

```text
这是学习和研究系统搭建笔记，不是投资建议。
不包含内部数据、客户信息、未脱敏策略、实盘观点或可交易 alpha。
```

未来所有 FICC 文章都要遵守这个边界。

## 当前结论

FICC 可以压成一张图：

```text
Fixed Income:
  rates + bonds + credit + curve + spread + duration

Currencies:
  FX + interest differential + carry + central banks + flows

Commodities:
  energy + metals + agriculture + supply-demand + inventory + futures curve

Cross-asset:
  macro -> rates -> FX -> commodities -> credit -> risk sentiment

AI layer:
  RAG + Graph + Agent + Forecast Ledger + Daily Brief + Human Review
```

对我们来说，FICC 的意义是：

```text
它把真实金融市场、银行业务经验、quant thinking 和 AI Research OS 接在一起。
```

下一篇应该做：

```text
FICC001 -> Fixed Income / Rates / Credit 深入介绍
```

因为 Fixed Income 是 FICC 的地基。

不懂 rates、curve、duration、spread，就很难真正理解 FX 和 commodities 的宏观联动。

## References

- Societe Generale, FIC overview: <https://wholesale.banking.societegenerale.com/en/news-insights/glossary/fic-fixed-income-and-currencies/summary-page/172160-2/>
- Investor.gov, Bonds FAQ: <https://www.investor.gov/introduction-investing/investing-basics/investment-products/bonds-or-fixed-income-products/bonds>
- Investor.gov, interest-rate risk for fixed-rate bonds: <https://www.investor.gov/introduction-investing/general-resources/news-alerts/alerts-bulletins/investor-bulletins-86>
- Federal Reserve Bank of New York, foreign exchange operations: <https://www.newyorkfed.org/markets/international-market-operations/foreign-exchange-operations>
- CFTC overview of futures, options and swaps regulation: <https://www.whistleblower.gov/aboutcftc>
