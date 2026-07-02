---
title: "FICC001: Fixed Income / Rates / Credit - 债券、利率曲线、久期与信用利差"
date: 2026-07-02 00:00:00 +0800
categories: [Learning, Finance]
tags: [ficc001, ficc, fixed-income, rates, credit, bonds, yield-curve, duration, credit-spread, repo, swaps, research-os]
---

这是 `PENGYI_FICC_MAP` 的第二篇：

```text
FICC001 -> Fixed Income / Rates / Credit
```

上一篇 `FICC000` 做了总地图：

```text
FICC = Fixed Income + Currencies + Commodities
```

这一篇专门讲第一个部分：

```text
FI = Fixed Income
```

但这里要先纠正一个直觉：

```text
Fixed Income 不只是“固定收益产品”。
在 FICC / markets 语境里，它通常覆盖 rates、bonds、credit、repo、swaps、curve risk、funding 和 credit risk。
```

也就是：

```text
Fixed Income = cash flow discounting + interest rates + yield curve + credit spread + liquidity + derivatives + risk management.
```

这一篇目标是把 FI 的地基打牢：

```text
债券是什么？
收益率是什么？
价格和利率为什么反向？
收益率曲线怎么看？
duration / convexity 是什么？
rates 和 credit 如何区分？
credit spread 表示什么？
repo / swap 在 FI 里起什么作用？
FI research workflow 怎么做？
它如何接入 FICC Research OS？
```

公开边界：

```text
This is educational material and research infrastructure thinking.
It is not investment advice, trading advice, or an actionable alpha note.
```

## 一句话总览

Fixed Income 的核心是：

```text
未来现金流如何被贴现，以及利率、信用、流动性和期限结构如何改变这些现金流的价格。
```

最短框架：

```text
Bond price
  = present value of future cash flows
  = coupon payments + principal repayment discounted by appropriate rates
```

FI 研究最重要的几个变量：

```text
yield
yield curve
duration
convexity
spread
credit risk
liquidity
repo / funding
swap rate
basis
central bank policy
inflation expectation
```

如果压成一张图：

```text
macro data / central bank / inflation / growth
  -> policy rate expectation
  -> yield curve
  -> bond prices
  -> duration risk

issuer fundamentals / sector / leverage / cash flow
  -> default probability / recovery expectation
  -> credit spread
  -> corporate bond price

funding / repo / liquidity / dealer balance sheet
  -> market liquidity and basis
  -> relative value and execution cost
```

## Fixed Income 的两条主线

FI 可以先拆成两条线：

```text
Rates
Credit
```

Rates 关注：

```text
无风险利率
央行政策
收益率曲线
期限溢价
通胀预期
实际利率
swap curve
repo / funding
```

Credit 关注：

```text
发行人偿债能力
违约概率
回收率
信用评级
信用利差
行业风险
流动性风险
融资环境
```

更直观：

```text
Rates answers:
  money over time is worth what?

Credit answers:
  this borrower is risky by how much?
```

中文：

```text
Rates 问的是“钱在不同期限上的价格”。
Credit 问的是“借款人风险应该补偿多少”。
```

二者不是分离的。

例如：

```text
央行加息
  -> risk-free curve up
  -> financing cost higher
  -> weak issuers face more refinancing pressure
  -> credit spreads may widen
```

或者：

```text
衰退风险上升
  -> long-end rates may fall
  -> credit spreads may widen
  -> government bonds rally while risky credit sells off
```

所以 FI 的难点是：

```text
rates view 和 credit view 经常同时变化，而且方向可能不同。
```

## 债券是什么

最基础定义：

```text
Bond = debt security.
```

投资者买债券，本质是借钱给发行人。

发行人承诺：

```text
1. 在债券存续期支付利息。
2. 到期偿还本金。
```

典型债券现金流：

```text
t1: coupon
t2: coupon
t3: coupon
...
T: coupon + principal
```

所以债券价格是：

```text
future cash flows 的 present value
```

最简单模型：

```text
Bond Price = Coupon_1 / (1+y)^1
           + Coupon_2 / (1+y)^2
           + ...
           + (Coupon_T + Principal) / (1+y)^T
```

这不是完整专业定价公式，只是地基直觉。

关键是：

```text
贴现率 y 越高，现值越低。
贴现率 y 越低，现值越高。
```

所以固定利率债券常见关系是：

```text
yield up -> price down
yield down -> price up
```

## 债券类型

常见债券类型：

```text
Government bonds
  国债 / sovereign bonds

Treasury bills
  短期国库券

Treasury notes
  中期国债

Treasury bonds
  长期国债

TIPS
  inflation-linked bonds

Municipal bonds
  市政债

Corporate bonds
  公司债

Investment grade bonds
  投资级债券

High yield bonds
  高收益债 / junk bonds

Agency bonds
  政府机构债

MBS / ABS
  mortgage-backed / asset-backed securities
```

从 FICC 研究角度，先不用急着背完所有产品。

先抓四类：

```text
1. Government bonds
   rates benchmark

2. Corporate bonds
   credit spread and issuer risk

3. Money market instruments
   short-term funding and liquidity

4. Swaps / derivatives
   curve, hedging, relative value
```

## 债券基本字段

每只债券至少要看：

```text
issuer
currency
coupon
maturity
face value / par value
price
yield
rating
seniority
callability
issue size
liquidity
```

中文：

```text
谁发行？
用什么货币？
票息多少？
什么时候到期？
面值是多少？
当前价格是多少？
收益率是多少？
评级如何？
债务优先级如何？
是否可赎回？
规模多大？
流动性如何？
```

研究债券不是只看收益率高低。

还要问：

```text
为什么它收益率高？
是因为期限长？
是因为信用差？
是因为流动性差？
是因为结构复杂？
是因为市场暂时错价？
```

## Price, Coupon, Yield

三个基础概念：

```text
Price:
  当前市场价格。

Coupon:
  债券承诺支付的票息。

Yield:
  按当前价格买入并持有时对应的收益率度量。
```

Coupon 是合同条款。

Yield 是市场价格反推出来的。

例子：

```text
一只面值 100、票息 5% 的债券，每年付 5。
```

如果市场价格是 100：

```text
收益率接近 5%。
```

如果市场价格跌到 95：

```text
同样未来现金流更便宜了，收益率上升。
```

如果市场价格涨到 105：

```text
同样未来现金流更贵了，收益率下降。
```

所以：

```text
price and yield move inversely.
```

## Yield 的几种口径

收益率不是单一概念。

常见口径：

```text
Current yield
  annual coupon / current price

Yield to maturity
  YTM, 持有到期、所有现金流贴现到当前价格的内部收益率

Yield to call
  可赎回债券被提前赎回情景下的收益率

Yield to worst
  多种赎回/到期情景下最保守收益率

Real yield
  nominal yield adjusted for inflation
```

初学先记住：

```text
YTM 是最常用的债券收益率直觉。
但真实交易里还要看 callability、option-adjusted spread、liquidity 和 scenario。
```

## 利率为什么影响债券价格

直觉：

```text
旧债券票息固定。
如果市场利率上升，新债券能给更高票息。
为了让旧债有吸引力，旧债价格需要下降。
```

反过来：

```text
如果市场利率下降，旧债的固定高票息变得更有吸引力，价格会上升。
```

这一点对面试很重要。

可以这样讲：

```text
Fixed-rate bond cash flows are fixed in nominal terms.
When discount rates rise, the present value of those cash flows falls.
That is why bond prices usually move inversely with yields.
```

中文：

```text
固定利率债券的未来现金流是固定的。
当贴现率上升时，同一组未来现金流的现值下降。
所以收益率上升时，债券价格通常下降。
```

## Yield Curve

收益率曲线是 Fixed Income 的核心。

它描述：

```text
同一类信用质量资产在不同期限上的收益率。
```

最常见是政府债收益率曲线：

```text
1M
3M
6M
1Y
2Y
3Y
5Y
7Y
10Y
20Y
30Y
```

曲线的作用：

```text
1. 反映市场对未来利率路径的预期。
2. 反映通胀、增长、期限溢价和供需。
3. 作为其他资产定价基准。
4. 作为宏观周期信号。
```

## 曲线形态

常见形态：

```text
Upward sloping:
  长端收益率高于短端。

Flat:
  长短端收益率接近。

Inverted:
  短端收益率高于长端。
```

常见变化：

```text
Parallel shift:
  整条曲线平移。

Steepening:
  长短利差扩大。

Flattening:
  长短利差收窄。

Bull move:
  yields down, prices up.

Bear move:
  yields up, prices down.
```

组合表达：

```text
Bull steepening:
  yields down, curve steepens.

Bear steepening:
  yields up, curve steepens.

Bull flattening:
  yields down, curve flattens.

Bear flattening:
  yields up, curve flattens.
```

这些词在 rates research 里非常常见。

## 短端、中端、长端

收益率曲线可以分段：

```text
Front end:
  overnight to 2Y

Belly:
  3Y to 7Y / 10Y

Long end:
  10Y to 30Y
```

不同段受不同因素影响。

Front end 更受：

```text
central bank policy rate
near-term inflation
money market liquidity
funding pressure
```

Belly 更受：

```text
expected policy path
growth outlook
cycle repricing
```

Long end 更受：

```text
long-term inflation expectation
real growth expectation
term premium
fiscal supply
pension / insurance demand
global savings
```

研究曲线时不能只说：

```text
利率上升。
```

要说：

```text
哪一段上升？
为什么上升？
是 policy path，inflation，term premium，还是 supply-demand？
```

## 央行与 Rates

Rates research 离不开央行。

央行影响曲线的方式：

```text
policy rate
reserve requirement
open market operations
balance sheet policy
forward guidance
quantitative easing / tightening
liquidity facilities
communication
```

核心问题：

```text
央行 reaction function 是什么？
```

也就是：

```text
央行如何在 inflation, growth, employment, financial stability 之间权衡？
```

Rates analyst 每天会问：

```text
市场隐含的降息/加息路径是什么？
央行讲话是否改变路径？
宏观数据是否支持这个路径？
短端是否已经 priced in？
长端变化是 growth view 还是 term premium？
```

## Nominal Yield, Real Yield, Breakeven

Rates 里有三个重要概念：

```text
Nominal yield:
  名义收益率。

Real yield:
  实际收益率，大致理解为剔除通胀后的收益率。

Breakeven inflation:
  市场隐含通胀预期的一种度量。
```

简化关系：

```text
nominal yield ≈ real yield + inflation expectation
```

实际中更复杂，但这个直觉很重要。

例如黄金研究常看：

```text
real rates
USD
risk sentiment
central bank demand
```

因为黄金不支付固定现金流，实际利率上升通常会提高持有黄金的机会成本。

这就是 Fixed Income 和 Commodities 的联动。

## Duration

Duration 是 Fixed Income 的核心风险度量。

简单理解：

```text
Duration measures bond price sensitivity to interest rate changes.
```

中文：

```text
久期衡量债券价格对利率变化的敏感度。
```

直觉：

```text
久期越长，利率变化对价格影响越大。
久期越短，利率变化对价格影响越小。
```

近似关系：

```text
Price change ≈ - Duration x Yield change
```

例如：

```text
duration = 5
yield rises by 1%
price change ≈ -5%
```

这是简化近似，不是精确定价。

但非常有用。

## 为什么长久期更敏感

因为现金流越远，越依赖贴现率。

```text
短期债券:
  本金很快回来。

长期债券:
  很多价值来自很远未来的现金流。
```

贴现率小幅变化，对远期现金流现值影响更大。

所以：

```text
long duration assets are more sensitive to rates.
```

这也是为什么：

```text
long bonds
growth stocks
long-dated cash-flow assets
```

在利率上行时都可能承压。

当然具体表现还取决于增长、风险偏好和盈利。

## Modified Duration, Macaulay Duration, DV01

实际市场会区分：

```text
Macaulay duration
  加权平均现金流期限。

Modified duration
  对收益率变化的价格敏感度。

DV01 / PV01
  yield 变化 1bp 时价格或价值变化多少。
```

对交易和风险管理来说，`DV01` 很常用。

```text
DV01 = dollar value of one basis point.
```

也就是：

```text
收益率变动 1bp，头寸价值变化多少。
```

Portfolio 里不会只看一只债。

会看：

```text
portfolio DV01
key rate DV01
curve exposure
hedge ratio
```

## Convexity

Duration 是一阶近似。

Convexity 是二阶修正。

为什么需要 convexity？

因为债券价格和收益率的关系不是直线，而是曲线。

```text
yield up -> price down
yield down -> price up
```

但下降和上升的幅度不完全对称。

Convexity 衡量：

```text
duration itself changes with yield.
```

简化理解：

```text
Positive convexity:
  rates fall, price gains accelerate;
  rates rise, price losses decelerate.
```

很多普通债券有正凸性。

但带 embedded option 的债券可能有复杂 convexity。

例如：

```text
callable bonds
MBS
structured products
```

当利率下降时，发行人或借款人可能提前赎回/提前还款，限制价格上涨空间。

这会带来：

```text
negative convexity
```

这也是 MBS 和 callable bonds 复杂的原因。

## Repo

Repo 是 Fixed Income 里非常重要的 funding market。

Repo 全称：

```text
repurchase agreement
```

简单理解：

```text
一方卖出证券并约定未来买回，本质上像以证券作抵押的短期融资。
```

Repo 的重要性：

```text
1. 它影响短端融资成本。
2. 它影响债券持仓成本。
3. 它影响市场流动性。
4. 它是 dealer balance sheet 和 collateral market 的核心。
```

Repo market 出问题时，可能影响：

```text
Treasury market liquidity
funding pressure
money market rates
central bank operations
```

对 AI Research OS 来说，repo 是一个典型的：

```text
market plumbing variable
```

表面上它不是“交易观点”，但它会影响市场能不能顺畅运转。

## Swaps

Interest rate swap 是 FI 衍生品核心。

最常见：

```text
fixed-for-floating interest rate swap
```

一方支付固定利率，收浮动利率。

另一方收固定利率，付浮动利率。

用途：

```text
hedging
duration management
curve exposure
relative value
asset-liability management
```

为什么 swap 重要？

因为很多机构不是只交易现金债。

他们用 swap 来管理利率风险。

常见概念：

```text
swap rate
swap curve
swap spread
OIS
SOFR
basis
```

如果政府债曲线是 cash bond benchmark，

swap curve 就是 derivative market 的重要 reference。

## Rates Relative Value

Rates 里经常看 relative value。

例子：

```text
cash bond vs futures
Treasury yield vs swap rate
on-the-run vs off-the-run bonds
2s10s curve
5s30s curve
cross-market rates spread
real yield vs nominal yield
```

注意：

```text
relative value 不是简单“哪个收益率高买哪个”。
```

要考虑：

```text
duration
liquidity
funding
repo specialness
convexity
tax
balance sheet
hedging cost
basis risk
```

公开文章里我们只讲框架。

具体 relative value 交易参数不公开。

## Credit

Credit 是 FI 的另一半。

Rates 问：

```text
risk-free or near-risk-free discount curve 怎么变？
```

Credit 问：

```text
这个发行人比无风险资产多要多少补偿？
```

最核心变量：

```text
credit spread
```

简单定义：

```text
credit spread = corporate bond yield - comparable risk-free yield
```

例如：

```text
某公司债 yield = 6%
同期限政府债 yield = 4%
credit spread ≈ 200 bps
```

这 200 bps 补偿的不是单一风险。

它包含：

```text
default risk
recovery uncertainty
liquidity risk
downgrade risk
sector risk
market risk premium
technical supply-demand
```

## Investment Grade vs High Yield

Corporate credit 常先分：

```text
Investment Grade
High Yield
```

Investment Grade:

```text
更高评级
违约风险较低
spread 较低
更接近利率风险 + 较低信用风险组合
```

High Yield:

```text
较低评级
违约风险较高
spread 较高
更接近 credit beta / equity-like risk
```

但不能简单理解成：

```text
IG 安全，HY 危险。
```

更准确：

```text
不同评级段有不同风险补偿、流动性和周期敏感性。
```

## Credit Spread 为什么变化

Spread 走阔可能因为：

```text
issuer fundamentals deteriorate
sector outlook worsens
default probability rises
recovery expectation falls
liquidity dries up
risk appetite falls
new issuance supply pressures market
rating downgrade risk rises
macro recession risk increases
```

Spread 收窄可能因为：

```text
fundamentals improve
earnings / cash flow improve
leverage declines
liquidity improves
risk appetite improves
central bank liquidity supports market
technicals are favorable
```

Credit 研究的问题是：

```text
spread move 是 rates-driven，issuer-driven，sector-driven，还是 market liquidity-driven？
```

## Credit Analysis

信用分析看两类东西：

```text
Fundamental analysis
Market analysis
```

Fundamental 看：

```text
revenue
EBITDA
free cash flow
debt maturity
leverage
interest coverage
liquidity
collateral
covenants
business model
sector cycle
management
```

Market 看：

```text
spread level
spread history
spread vs peers
CDS
bond liquidity
new issue concession
rating outlook
equity price
volatility
macro regime
```

这和银行对公 / 授信经验有连接。

银行授信关注：

```text
借款人能不能还钱？
现金流够不够？
担保抵押如何？
行业和经营风险怎样？
合同和法律风险如何？
```

Credit market 也关注这些，但会额外问：

```text
这些风险在价格里补偿够不够？
```

## Default Probability and Recovery

Credit spread 的背后是：

```text
default probability
loss given default
recovery rate
risk premium
liquidity premium
```

非常简化的直觉：

```text
Expected credit loss ≈ PD x LGD
```

其中：

```text
PD:
  probability of default

LGD:
  loss given default = 1 - recovery rate
```

但市场 spread 不等于 expected loss。

还包括：

```text
risk premium
liquidity premium
uncertainty premium
technical factors
```

所以 credit 不是纯会计分析。

它是：

```text
fundamental risk + market pricing + liquidity + cycle + risk appetite
```

## Ratings

评级是 credit market 的重要 reference。

常见评级机构：

```text
S&P
Moody's
Fitch
```

评级影响：

```text
investor mandate
index inclusion
funding cost
spread level
fallen angel risk
```

但评级不是万能。

研究员不能只看评级。

要问：

```text
市场 spread 是否已经比评级更悲观？
评级是否滞后？
是否有 downgrade / upgrade catalyst？
公司基本面是否和评级一致？
```

## CDS

CDS 是 credit default swap。

简单理解：

```text
买 CDS protection 类似买违约保护。
卖 CDS protection 类似承担信用风险并收取 premium。
```

CDS spread 是信用风险的市场价格之一。

它常用于：

```text
hedging credit exposure
expressing credit view
relative value vs cash bonds
market-implied credit risk monitoring
```

但 CDS 和 cash bond spread 不一定完全一致。

因为有：

```text
basis
liquidity
contract terms
deliverability
funding
technical demand
```

## OAS

OAS 是 option-adjusted spread。

常用于：

```text
bonds with embedded options
MBS
callable bonds
structured fixed income products
```

它试图在考虑 embedded option 后衡量 spread。

初学可以先记：

```text
如果债券有 call / prepayment / embedded option，简单 spread 可能误导。
OAS 是更调整后的 spread 度量。
```

## FI Risk Map

Fixed Income 的风险可以列成：

```text
Interest rate risk
Credit risk
Inflation risk
Liquidity risk
Call / prepayment risk
Reinvestment risk
Currency risk
Basis risk
Funding risk
Model risk
```

每个 risk 都有对应问题：

```text
Interest rate risk:
  yields move against you?

Credit risk:
  issuer default or spread widens?

Inflation risk:
  fixed cash flows lose real value?

Liquidity risk:
  cannot trade at fair price?

Call risk:
  issuer redeems when it is bad for investor?

Reinvestment risk:
  coupons/principal reinvested at lower yield?

Basis risk:
  hedge does not move with exposure?
```

FI 的专业性就在于：

```text
它把每一种风险都定量化、拆分化、交易化、对冲化。
```

## FI Daily Research Workflow

一个 Fixed Income daily workflow 可以这样设计：

```text
1. Overnight rates move
   Treasury / government yield changes

2. Curve shape
   2Y, 5Y, 10Y, 30Y; 2s10s, 5s30s

3. Macro drivers
   CPI, jobs, PMI, GDP, fiscal, geopolitics

4. Central bank repricing
   policy path, meeting probabilities, speeches

5. Real yield and inflation expectation
   nominal / real / breakeven

6. Funding and repo
   money market stress, repo rates, liquidity

7. Credit spread update
   IG, HY, sector, issuer, CDS

8. New issue and technicals
   supply, demand, fund flows

9. Risk scenario
   what can break the view?

10. Client / portfolio implication
   duration, curve, credit exposure, hedge need
```

这个 workflow 非常适合我们做：

```text
FICC Daily Brief Generator
```

## Rates Brief Template

一个 rates brief 可以这样写：

```text
Date:

Market Move:
  2Y:
  5Y:
  10Y:
  30Y:
  2s10s:

Key Driver:
  macro data / central bank / supply / risk sentiment

Policy Path:
  market-implied path changed how?

Curve Interpretation:
  parallel / steepening / flattening / inversion

Risk:
  upcoming data, central bank speech, auction, liquidity event

What to Monitor:
  3-5 bullet points
```

这不是交易建议。

这是市场研究结构化。

## Credit Brief Template

一个 credit brief 可以这样写：

```text
Date:

Market Move:
  IG spreads:
  HY spreads:
  CDS indices:
  sector moves:

Issuer / Sector News:
  event:
  affected names:
  likely spread impact:

Fundamental Check:
  leverage:
  cash flow:
  refinancing:
  rating outlook:

Technical Check:
  new issuance:
  fund flows:
  liquidity:

Risk:
  downgrade, default, refinancing, macro shock

What to Monitor:
  3-5 bullet points
```

这可以直接接入 Research OS。

## FI x RAG

Fixed Income 特别适合 RAG。

可检索材料：

```text
central bank statements
FOMC minutes
monetary policy reports
Treasury auction announcements
bond prospectuses
rating reports
issuer filings
market daily notes
macro data releases
credit research reports
```

RAG 问题：

```text
最近央行措辞如何改变市场对短端利率的预期？

某个发行人的债务结构和再融资压力在哪里？

某次 CPI 数据为什么影响 2Y yield 多于 10Y yield？

某行业 credit spread 走阔是基本面还是流动性？
```

输出必须包含：

```text
source
timestamp
evidence
uncertainty
what to monitor
human review required
```

## FI x Graph

FI 也适合 graph。

Rates graph：

```text
inflation
  -> central bank reaction function
  -> policy rate expectation
  -> front-end yields
  -> curve slope
  -> mortgage / credit / FX impact
```

Credit graph：

```text
issuer
  -> parent company
  -> subsidiaries
  -> bonds
  -> maturities
  -> ratings
  -> covenants
  -> sector
  -> macro sensitivity
```

Portfolio graph：

```text
portfolio
  -> bond positions
  -> issuer exposures
  -> sector exposures
  -> rating exposures
  -> duration exposures
  -> spread duration
  -> liquidity buckets
```

Graph RAG 的价值：

```text
不仅检索文本，还能理解债券、发行人、期限、评级、行业、曲线和风险之间的关系。
```

## FI x Quant

Fixed Income quant 方向很多。

公开安全地说，可以包括：

```text
yield curve modeling
term structure modeling
duration / convexity risk
spread decomposition
default probability estimation
liquidity risk measurement
macro factor sensitivity
scenario analysis
relative value screening
forecast evaluation
```

但不能公开：

```text
具体交易信号
真实参数
未脱敏回测
内部报价
客户流
交易员观点
可复制 alpha
```

我们的 public demo 可以做：

```text
toy yield curve monitor
public Treasury curve visualizer
duration calculator
credit spread explainer
RAG-based central bank statement summarizer
source-grounded FI daily brief demo
```

这些展示的是：

```text
research engineering ability
not proprietary trading edge
```

## FI x Research OS

`Pengyi Fixed Income Research OS v0` 可以这样拆：

```text
Data Layer:
  public yields, macro data, credit indices, issuer public data

Document Layer:
  central bank docs, filings, bond descriptions, public research notes

RAG Layer:
  source-grounded retrieval and answer

Curve Layer:
  yield curve snapshot, changes, slope, scenarios

Credit Layer:
  issuer / sector / spread / rating / maturity profile

Risk Layer:
  duration, DV01, spread duration, liquidity buckets

Forecast Ledger:
  timestamped rates and credit views

Brief Layer:
  daily rates brief, credit brief, event preview

Human Review:
  PM / analyst approval

Artifact Layer:
  website note, chart, memo, slide
```

这和我们已经学过的模块可以连接：

```text
LightRAG:
  central bank and credit document memory

RAG-Anything:
  prospectus / PDF / tables / charts ingestion

GraphAgent:
  issuer-sector-bond-risk graph

FutureShow:
  forecast ledger and outcome review

Vibe-Trading:
  research workflow and artifact generation

MGP:
  memory governance and audit
```

## FI 对我们银行经历的连接

我们在银行看到的东西，和 FI/credit 有真实连接。

银行对公 / 授信关注：

```text
客户是谁？
现金流如何？
行业风险如何？
担保抵押如何？
还款来源是什么？
合同结构如何？
授信额度如何控制？
风险审批如何做？
```

Credit market 关注：

```text
发行人是谁？
现金流如何？
杠杆和覆盖率如何？
行业风险如何？
债务期限结构如何？
违约概率如何？
spread 是否补偿风险？
```

这不是完全一样。

但底层问题相通：

```text
borrower risk + cash flow + legal claim + market pricing + risk control
```

所以银行经历不是废的。

它是 credit intuition 的现实场景来源。

## 面试可用表达

如果被问：

```text
你怎么理解 Fixed Income？
```

可以回答：

```text
我理解 Fixed Income 的核心是未来现金流的贴现，以及利率、信用、流动性和期限结构如何影响这些现金流的价格。

在 FICC 语境里，Fixed Income 不只是固定收益产品，而是包括 rates、bonds、credit、repo、swaps 和 curve risk。

Rates 更关注央行政策、收益率曲线、通胀预期、实际利率和期限溢价。
Credit 更关注发行人偿债能力、违约概率、回收率、信用评级和信用利差。

债券价格和收益率通常反向变动，因为固定现金流在更高贴现率下现值下降。
Duration 衡量债券价格对利率变化的敏感度，convexity 是对这种非线性关系的二阶修正。

我现在更关注的是如何把 Fixed Income research workflow 和 AI infrastructure 结合起来，比如 source-grounded RAG、yield curve monitor、credit spread brief、forecast ledger 和 human-reviewed research assistant。
```

这段可以用于：

```text
FICC research support
quant interview
AI for finance
bank internal rotation
RA / PhD narrative
```

## 常见误区

误区一：

```text
收益率高就是好。
```

更准确：

```text
收益率高可能是风险补偿高，也可能是市场要求更高 credit / liquidity / duration premium。
```

误区二：

```text
债券就是低风险。
```

更准确：

```text
债券有 interest rate risk, credit risk, liquidity risk, inflation risk, call risk 等。
```

误区三：

```text
看 10Y yield 就等于看 FI。
```

更准确：

```text
FI 要看整条曲线、短端资金面、长端 term premium、credit spread、repo、swap、basis 和 liquidity。
```

误区四：

```text
Credit spread 只反映违约概率。
```

更准确：

```text
Credit spread 同时包含 expected loss、risk premium、liquidity premium、technical factors 和 uncertainty。
```

## 学习顺序

Fixed Income 初学顺序：

```text
1. Bond cash flows
2. Price-yield inverse relationship
3. Yield curve
4. Duration and convexity
5. Rates and central banks
6. Repo and funding
7. Corporate credit
8. Credit spread
9. Swaps and derivatives
10. FI research workflow
```

不要一开始就钻复杂模型。

先把：

```text
cash flow
discounting
curve
risk sensitivity
spread
liquidity
```

这些地基打牢。

## 下一篇

下一篇建议：

```text
FICC002 -> Currencies / FX / Carry / Central Bank Divergence
```

因为 Fixed Income 学完后，FX 就自然了。

FX 的很多核心驱动都来自 rates：

```text
interest rate differential
real yield differential
central bank divergence
carry
funding currency
risk sentiment
```

也就是说：

```text
不懂 rates，就很难真正理解 FX。
```

## 当前结论

Fixed Income 可以压成：

```text
Bond:
  future cash flows

Rates:
  discount curve, central bank, yield curve, duration

Credit:
  issuer risk, spread, default, recovery, liquidity

Funding:
  repo, money market, collateral

Derivatives:
  swaps, futures, options, CDS

Research OS:
  curve monitor + credit brief + RAG + graph + forecast ledger + human review
```

这就是 FICC001 的核心。

我们不是为了背术语。

我们是为了把 Fixed Income 变成：

```text
可理解的金融市场框架
可拆解的 research workflow
可接入 AI / RAG / Agent 的真实场景
```

## References

- Investor.gov, Bonds FAQ: <https://www.investor.gov/introduction-investing/investing-basics/investment-products/bonds-or-fixed-income-products/bonds>
- Investor.gov, Fixed Income Investments and Interest Rate Risk: <https://www.investor.gov/introduction-investing/general-resources/news-alerts/alerts-bulletins/investor-bulletins-86>
- U.S. Treasury, Daily Treasury Rates: <https://home.treasury.gov/resource-center/data-chart-center/interest-rates/TextView?type=daily_treasury_yield_curve>
- Federal Reserve Bank of New York, Treasury Repo Reference Rates information: <https://www.newyorkfed.org/markets/reference-rates/additional-information-about-reference-rates>
- FINRA, Bond basics: <https://www.finra.org/investors/investing/investment-products/bonds>
