---
title: "FICC009: FICC Rates Bond Quant - 我们自己的开源利率债量化项目"
date: 2026-07-15 00:00:00 +0800
categories: [Learning, Finance, Open Source]
tags: [ficc009, ficc, rates, fixed-income, bond-quant, duration, convexity, pnl, python, fastapi, nextjs, typescript, cloudflare-pages, open-source, public-safe]
---

这是 `PENGYI_FICC_MAP` 的 `FICC009`，也是这个系列第一次从“研究地图”真正走到“自己写出来并公开运行的软件”。

```text
FICC000-FICC003 -> 认识 Fixed Income / FX / Commodities
FICC004-FICC005 -> 从 daily brief 走向 event-to-signal workflow
FICC006-FICC008 -> AI Research OS 与 Agent Harness
FICC009         -> 自己实现、测试、部署并维护一个 Rates Bond Quant 项目
```

## Project Links

- **Live Website:** [ficc-rates-bond-quant.pages.dev](https://ficc-rates-bond-quant.pages.dev/)
- **GitHub Repository:** [pengpengyi92/ficc-rates-bond-quant](https://github.com/pengpengyi92/ficc-rates-bond-quant)
- **Current Milestone:** `v0.7.0`
- **License:** MIT

![FICC Rates Bond Quant project icon](https://raw.githubusercontent.com/pengpengyi92/ficc-rates-bond-quant/master/assets/ficc-rates-bond-quant-icon.png)

## 一句话介绍

`FICC Rates Bond Quant` 是一个 public-safe 的利率债量化教学项目：它把固定利率债券现金流、定价、Macaulay Duration、Modified Duration、Convexity、平行利率冲击、持仓 P&L 和期限比较，组织成一个有 Python 核心、API 层、Web Dashboard、测试、案例、文档和版本记录的开源仓库。

我可以准确地说：

```text
I built an educational FICC rates bond quant project that implements bond
pricing, duration, convexity, rate-shock scenarios, P&L estimation, and
multi-maturity comparison in Python, with a Next.js dashboard and a public demo.
```

但不能把它包装成：

```text
production rates trading system
bank internal pricing engine
live portfolio optimizer
verified real-money strategy
actionable trading signal
```

这正是我们的 CV 原则：**功能、部署状态、数据来源和项目边界，每一句都能被代码、测试或文档解释。**

## 为什么做这个项目

前面的 FICC 学习已经覆盖了宏观事件、收益率曲线、跨资产关系、研究 workflow 和 AI Agent Harness，但仅有文章还不够。

真正的软件项目必须回答：

```text
输入是什么？
公式在哪里？
风险敏感度怎么算？
同一冲击对不同期限有什么差异？
结果如何测试？
用户如何操作？
系统如何部署？
哪些结论可以公开？
```

因此，FICC009 的意义不是再写一篇“债券概念介绍”，而是把概念变成可执行接口：

```text
Bond Assumptions
-> Cash Flows
-> Price
-> Duration / Convexity
-> Yield Shock
-> Price Impact / P&L
-> Maturity Comparison
-> Dashboard / Report
-> Human Interpretation
```

## System Design

项目目前可以拆成六层：

```text
1. Quant Core
   Python: cash flow, price, duration, convexity, scenario and P&L

2. Portfolio Layer
   BondPosition, single-position analysis, multi-maturity comparison

3. API Scaffold
   FastAPI + Pydantic request/response schemas

4. Product Layer
   Next.js + TypeScript + React + Recharts dashboard

5. Verification Layer
   Python unit tests, sample report, case-study assumptions and limits

6. Delivery Layer
   GitHub repository + MIT License + version log + Cloudflare Pages
```

仓库不是只有一个 Notebook。它有标准的开源项目结构：

```text
src/            reusable Python package
tests/          quantitative invariants and regression checks
backend/        FastAPI API scaffold
frontend/       Next.js interactive dashboard
demos/          runnable report generation
case_studies/   public-safe market simulations
docs/           system design, boundary and deployment notes
assets/         project identity and teaching images
outputs/        reproducible sample report
pyproject.toml  package metadata and dependencies
VERSION_LOG.md  visible capability milestones
```

### 一个必须讲清楚的部署事实

仓库中已经有 FastAPI API：

```text
POST /api/calculate
GET  /api/scenarios
```

但当前 Cloudflare Pages 线上版本是 **Next.js static export**。页面在浏览器中调用 TypeScript 版 `bondMath.ts` 直接计算，并没有调用 FastAPI 服务。

因此准确架构是：

```text
Current public deployment:
Browser -> Next.js static UI -> TypeScript bond math -> charts / table

Available repository scaffold:
Client -> FastAPI -> Python quant core
```

这不是缺点掩饰，而是版本状态。静态部署让 Demo 无需数据库和服务器即可打开；后续再把 API 部署与前端接通，就能消除 Python/TypeScript 双实现。

## Quant Core

### 1. Cash Flow and Bond Pricing

固定利率债券价格来自未来现金流贴现：

```text
P = sum(CF_t / (1 + y / m)^t)
```

其中：

```text
P  = bond price
CF = coupon or coupon + principal cash flow
y  = annual yield to maturity
m  = coupon frequency
t  = coupon period index
```

Python 核心先生成逐期 `Cashflow`，再统一用于价格、久期和凸性计算。这比在每个函数里重复构造现金流更容易检查和复用。

### 2. Macaulay and Modified Duration

Macaulay Duration 是现金流现值时间的加权平均：

```text
D_mac = sum(time_t * PV(CF_t)) / P
```

Modified Duration 把它转化成对收益率变化的一阶价格敏感度：

```text
D_mod = D_mac / (1 + y / m)
dP / P ~= -D_mod * dy
```

直觉是：

```text
duration 越长
-> 相同利率变化下价格越敏感
-> 长久期持仓的 P&L 波动越大
```

### 3. Convexity

Duration 是价格-收益率曲线的切线近似，Convexity 则修正曲率：

```text
dP / P ~= -D_mod * dy + 0.5 * Convexity * dy^2
```

项目中的 P&L 核心就是这一行：

```python
relative_change = (
    -modified_duration_value * yield_change
    + 0.5 * convexity_value * yield_change**2
)
```

对于较小的 `dy`，一阶久期项占主导；冲击增大、期限拉长以后，凸性项变得更重要。

### 4. Position P&L

项目把面值规模转换为债券单位数：

```text
units = notional_face_value / face_value
PnL   = units * estimated_price_change
```

这样可以比较同一名义本金在 `1Y / 3Y / 5Y / 10Y / 30Y` 上的利率风险差异。

## Scenarios and Trading Interpretation

项目支持以 basis point 定义平行利率冲击：

```text
-50bp / -25bp / -10bp -> rate cut
+10bp / +25bp / +50bp -> rate hike
```

在普通正久期债券假设下：

```text
yield down -> price up -> long bond P&L positive
yield up   -> price down -> long bond P&L negative
```

这已经涉及 rates trading 最基本的一层：

```text
direction view
x duration exposure
x convexity adjustment
x position notional
= scenario P&L
```

但它还不是完整交易系统。真正的 rates trade 还需要：

```text
carry and roll-down
repo funding
bid-ask and transaction cost
curve steepener / flattener / butterfly exposure
DV01 and key-rate duration
treasury futures or swap hedge
liquidity and execution constraints
mark-to-market conventions
portfolio limits and stress testing
```

所以 FICC009 的定位是 **rates risk and scenario analytics foundation**，而不是宣称已经完成 production trading。

## Web Product Layer

Dashboard 将量化函数变成了可操作产品：

```text
Inputs:
- position notional
- yield change in bps
- coupon rate
- initial YTM

Outputs:
- price
- Macaulay duration
- modified duration
- convexity
- estimated price change
- estimated P&L
```

React 负责状态和组件组合，TypeScript 约束输入输出结构，Recharts 展示：

```text
P&L by maturity
duration by maturity
convexity by maturity
scenario result table
```

前端不是装饰层。它让用户可以立刻改变冲击和本金，观察期限风险如何重新分布，也把“久期更长、价格更敏感”从一句话变成可交互结果。

## FastAPI Layer

API 层使用 Pydantic 定义明确的数据契约：

```text
CalculateRequest
-> notional
-> yield_change_bps
-> maturities
-> coupon_rate
-> yield_to_maturity

CalculateResponse
-> scenario
-> notional
-> yield_change_bps
-> results[]
```

它的价值是把 Python quant core 暴露成标准 HTTP 接口，使未来的 Web、Notebook、Agent 或其他客户端都能复用同一份计算逻辑。

当前需要继续做的是 API 集成测试、服务部署，以及让前端切换到后端计算。

## Tests: 我们到底验证了什么

当前 Python 测试覆盖七个关键不变量：

```text
1. zero-coupon price equals discounted principal
2. zero-coupon Macaulay duration equals maturity
3. modified duration formula is internally consistent
4. convexity is positive in the tested vanilla-bond case
5. rate cut increases long-bond price and P&L
6. rate hike decreases long-bond price and P&L
7. longer maturity has higher duration under common assumptions
```

本次发布前重新执行：

```text
Ran 7 tests
OK
```

这些测试不是证明模型适合实盘，而是保护最基本的定价方向、公式关系和期限比较不被后续修改破坏。

下一步测试应该增加：

```text
Python vs TypeScript parity tests
API schema and endpoint tests
exact repricing vs duration-convexity approximation error
invalid maturity / frequency / yield boundary tests
curve-shift and portfolio aggregation tests
frontend component and end-to-end tests
```

## Public-Safe Real-Market Cases

项目已经加入 2026 China government bond 教学案例。案例使用近似公开市场变动和明确假设，将公式连接到真实语境，但不使用：

```text
employer internal data
client information
real portfolio positions
private trading records
proprietary market views
internal risk limits
```

一个案例比较 `2Y / 5Y / 10Y` 在不同收益率变化和久期假设下的 P&L；另一个案例用长久期债券说明，即使收益率变动相近，duration-adjusted exposure 也会显著改变结果。

核心结论是：

```text
Yield move size matters, but duration-adjusted exposure matters more.
```

这些是 educational simulations，不是实盘业绩记录。

## Open-Source Engineering

项目当前公开能力不是只有公式：

```text
v0.1 -> public-safe structure, docs, demo, license, icon
v0.2 -> Python quant engine and tests
v0.3 -> FastAPI backend scaffold
v0.4 -> TypeScript frontend
v0.5 -> interactive dashboard
v0.6 -> public-safe market case studies
v0.7 -> Cloudflare Pages static deployment
```

版本记录只追踪可见能力里程碑，小修改留在 Git commit history。这让 README 不会退化成每次改字都更新的流水账。

MIT License、README、测试、部署文档、public/private boundary 和公开 issue/PR 入口，则让它从个人脚本走向可阅读、可运行、可讨论的开源项目。

## What I Would Explain in an Interview

### 30-second version

```text
I built a public open-source rates bond quant project. The Python core prices
fixed-coupon bonds and computes Macaulay duration, modified duration, convexity,
parallel rate-shock impact, and position P&L across maturities. I added tests,
a FastAPI interface scaffold, and an interactive Next.js dashboard deployed on
Cloudflare Pages. I also document the model limitations and the gap from a
production rates trading system.
```

### 深挖时可以展开

```text
为什么 yield 和 price 反向？
Macaulay 与 modified duration 有何区别？
为什么需要 convexity？
duration-convexity approximation 什么时候误差变大？
为什么 30Y 对 10bp 更敏感？
notional、face value、market value 与 P&L 如何连接？
为什么当前前端在浏览器计算，而不是调用 FastAPI？
如何避免 Python 与 TypeScript 两套公式漂移？
真实 rates desk 还缺哪些数据、风险和执行模块？
哪些案例可以公开，哪些必须留在 private workspace？
```

这正是项目的价值：不是让简历多一个名字，而是提供一组能从数学、代码、架构、部署、测试、交易解释和风险边界持续展开的问题。

## Roadmap to v1.0

最有价值的下一步不是继续堆页面，而是补全利率风险系统：

```text
1. DV01 / PVBP
2. key-rate duration
3. real yield-curve input schema
4. parallel / steepener / flattener / butterfly scenarios
5. exact repricing vs approximation diagnostics
6. multi-position portfolio aggregation
7. carry and roll-down decomposition
8. treasury futures hedge example
9. frontend -> FastAPI integration
10. Python / TypeScript parity and API tests
```

完成这些以后，项目会从单债场景计算器继续向一个小型、可解释、可测试的 Rates Risk Lab 演化。

## FICC009 的结论

FICC009 是一个明确的分界点：

```text
before FICC009: we studied and mapped the system
from FICC009:   we implement, test, deploy, explain, and maintain the system
```

它不是终点，也不需要假装成熟。当前版本已经真实拥有：

```text
quant formulas
reusable Python modules
scenario and P&L workflow
unit tests
API scaffold
interactive web product
public deployment
case-study boundary
open-source documentation
versioned roadmap
```

接下来的标准仍然不变：每增加一个模块，都同时增加实现、测试、文档、可视化、边界说明和可讲解的 interview story。

## References

- [FICC Rates Bond Quant GitHub Repository](https://github.com/pengpengyi92/ficc-rates-bond-quant)
- [FICC Rates Bond Quant Live Website](https://ficc-rates-bond-quant.pages.dev/)
- [Project README](https://github.com/pengpengyi92/ficc-rates-bond-quant/blob/master/README.md)
- [System Design](https://github.com/pengpengyi92/ficc-rates-bond-quant/blob/master/docs/system_design.md)
- [Version Log](https://github.com/pengpengyi92/ficc-rates-bond-quant/blob/master/VERSION_LOG.md)
- [Public / Private Boundary](https://github.com/pengpengyi92/ficc-rates-bond-quant/blob/master/docs/public_private_boundary.md)
- [Python Quant Core](https://github.com/pengpengyi92/ficc-rates-bond-quant/tree/master/src/ficc_rates_bond_quant)
- [Python Tests](https://github.com/pengpengyi92/ficc-rates-bond-quant/tree/master/tests)
- [Next.js Frontend](https://github.com/pengpengyi92/ficc-rates-bond-quant/tree/master/frontend)
