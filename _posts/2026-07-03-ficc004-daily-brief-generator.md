---
title: "FICC004: Daily FICC Brief Generator - 把 FI / FX / Commodities 合成每日研究工作流"
date: 2026-07-03 01:00:00 +0800
categories: [Learning, Finance]
tags: [ficc004, ficc, daily-brief, research-os, fixed-income, fx, commodities, rag, forecast-ledger, agent-workflow, human-review]
---

这是 `PENGYI_FICC_MAP` 的 `FICC004`。

前面四篇已经把 FICC 的地基铺出来：

```text
FICC000 -> FICC 总地图
FICC001 -> Fixed Income / Rates / Credit
FICC002 -> Currencies / FX
FICC003 -> Commodities
FICC004 -> Daily FICC Brief Generator
```

这一篇开始从“知识学习”进入“系统搭建”。

目标不是直接预测市场，也不是直接生成交易信号。
目标是设计一个每日 FICC 研究简报生成器：

```text
Daily FICC Brief Generator
```

它把三条线合在一起：

```text
Fixed Income:
  rates, yield curve, credit spread, funding, duration

Currencies / FX:
  spot, forward, swap, rate differential, carry, basis, central bank divergence

Commodities:
  energy, metals, agriculture, inventory, futures curve, geopolitics
```

然后每天输出一份结构化 brief：

```text
what happened
why it happened
which asset moved
which macro variable changed
which narrative is stronger
what evidence supports it
what counter-evidence exists
what to watch next
how yesterday's forecast performed
```

公开边界：

```text
This is educational material and research infrastructure thinking.
It is not investment advice, trading advice, or an actionable alpha note.
```

中文边界：

```text
这是公开学习笔记和研究基础设施设计，不是投资建议。
不包含内部数据、客户信息、未脱敏策略、实盘观点或可交易 alpha。
```

## 一句话总览

Daily FICC Brief Generator 的本质是：

```text
把每天分散在 rates、FX、commodities、macro data、central bank、news 和 market commentary 里的信息，转化成一份可追踪、可复盘、可审计的跨资产研究简报。
```

更工程化一点：

```text
Daily FICC Brief = data ingestion + event extraction + cross-asset mapping + evidence retrieval + narrative ranking + forecast ledger + human review.
```

它不是一个“自动喊方向”的 agent。
它更像一个研究副驾驶：

```text
research copilot
market memory
event ledger
cross-asset reasoning assistant
daily review system
```

## 为什么需要 Daily Brief

FICC 市场每天的信息密度非常高。

每天会有：

```text
macro data release
central bank speech
yield curve movement
credit spread widening / tightening
FX spot move
forward / basis change
oil inventory report
commodity curve move
geopolitical headline
risk sentiment shift
client / market commentary
```

如果不结构化，这些信息会变成碎片。

人脑会出现几个问题：

```text
1. 只记住最新 headline，忘掉之前的上下文。
2. 对某个叙事过度自信，忽略反例。
3. 看到了价格变化，但没拆清楚驱动来源。
4. 每天都在读新东西，但没有形成可复盘的研究资产。
5. 昨天的判断没有被系统记录，今天就无法真实校准。
```

Daily Brief Generator 要解决的就是这些问题。

```text
not more information
but better structure
```

中文：

```text
不是读更多信息，而是把信息整理成可复用的研究结构。
```

## Daily FICC Brief 的核心输出

一份好的 Daily FICC Brief 不应该只是新闻摘要。

它应该包括：

```text
1. Market Snapshot
2. Macro Calendar
3. Rates and Credit
4. FX and Dollar Liquidity
5. Commodities
6. Cross-Asset Causal Chain
7. Key Narrative
8. Evidence and Counter-Evidence
9. Forecast Ledger Update
10. Watchlist and Human Review
```

也就是：

```text
市场发生了什么
宏观变量怎么变
资产价格怎么反应
跨资产链条是否一致
当前主叙事是什么
这个叙事有哪些证据
有哪些反例
之前判断是否被验证
明天重点看什么
哪些地方必须人工审核
```

这才是 research workflow。

## 输入层

Daily Brief 的输入可以分成六类：

```text
market data
macro data
policy text
news and events
research notes
forecast ledger
```

更具体：

| 输入 | 示例 | 作用 |
|---|---|---|
| Market data | yields, curves, FX spot, commodity futures, spreads | 记录价格和风险因子变化 |
| Macro data | CPI, payroll, GDP, PMI, trade balance, inventory | 识别宏观 surprise |
| Policy text | FOMC, ECB, BoJ, PBoC, speeches | 提取央行 stance |
| News and events | geopolitics, sanctions, supply disruption, election | 抽取事件冲击 |
| Research notes | public commentary, internal-safe notes, public reports | 提供解释和上下文 |
| Forecast ledger | yesterday's claims and watchlist | 做复盘和校准 |

公开版本的数据边界应该清楚：

```text
public data
public documents
sanitized notes
no client information
no proprietary signal
no trading instruction
```

## 输出层

Daily Brief 的输出可以固定成一个模板：

```text
# Daily FICC Brief

Date:
Market Regime:
Main Narrative:
Confidence:
Human Review Required:

## 1. Executive Summary
## 2. Market Snapshot
## 3. Macro and Policy Calendar
## 4. Fixed Income: Rates / Credit / Funding
## 5. FX: USD / G10 / EM / Basis
## 6. Commodities: Energy / Metals / Agriculture
## 7. Cross-Asset Causal Chain
## 8. Evidence
## 9. Counter-Evidence
## 10. Forecast Ledger Review
## 11. Watchlist
## 12. Open Questions
```

它要同时服务三类读者：

```text
human researcher
portfolio manager
future agent memory
```

所以 brief 不能只写给人看。
也要适合机器读取。

可以同时输出：

```text
markdown report
json report
event table
claim table
forecast ledger update
```

## 系统架构

可以先设计成七层：

```text
Layer 1: Data Ingestion
Layer 2: Normalization
Layer 3: Event Extraction
Layer 4: Retrieval and Memory
Layer 5: Cross-Asset Reasoning
Layer 6: Brief Generation
Layer 7: Human Review and Ledger Update
```

完整链路：

```text
raw inputs
  -> normalized observations
  -> extracted events
  -> linked entities and assets
  -> retrieved historical context
  -> generated narratives
  -> evidence / counter-evidence check
  -> forecast ledger update
  -> human reviewed brief
```

核心原则：

```text
每一个结论都要能追溯到 observation / event / evidence。
```

也就是：

```text
no unsupported market narrative
```

## Layer 1: Data Ingestion

输入层负责把每天的信息收进来。

典型输入：

```text
rates:
  treasury yields
  swap rates
  yield curve slope
  breakeven inflation
  credit spreads
  repo / funding indicators

FX:
  major FX pairs
  DXY
  forward points
  cross-currency basis
  implied volatility
  risk reversal

commodities:
  oil futures
  gas futures
  gold
  copper
  grains
  futures curve
  inventory report

macro:
  CPI
  payroll
  GDP
  PMI
  retail sales
  trade balance
  central bank meetings

text:
  central bank statements
  speeches
  official reports
  public news
  public market summaries
```

第一版不需要接复杂数据源。
可以先用：

```text
CSV
manual markdown notes
public official links
small JSON fixtures
```

关键不是数据源豪华。
关键是结构要对。

## Layer 2: Normalization

Normalization 把不同来源变成统一格式。

统一观察对象：

```text
Observation:
  id
  date
  asset_class
  asset
  metric
  value
  change
  unit
  source
  timestamp
```

例如：

```json
{
  "date": "2026-07-03",
  "asset_class": "rates",
  "asset": "US10Y",
  "metric": "yield",
  "value": 4.32,
  "change": 0.06,
  "unit": "percent",
  "source": "public_market_data"
}
```

统一事件对象：

```text
Event:
  id
  date
  event_type
  region
  entities
  summary
  affected_assets
  expected_direction
  actual_market_reaction
  source
```

统一 claim 对象：

```text
Claim:
  id
  date
  claim_text
  claim_type
  assets
  evidence_ids
  counter_evidence_ids
  confidence
  horizon
  status
```

这个很重要。
因为后面的 forecast ledger 和 human review 都依赖它。

## Layer 3: Event Extraction

Event extraction 要从文本和数据里抽取“今天真正发生了什么”。

事件类型可以先定义成：

```text
macro_surprise
central_bank_signal
rate_move
curve_move
credit_spread_move
fx_move
commodity_supply_event
commodity_inventory_event
geopolitical_event
risk_sentiment_event
funding_stress_event
```

例如：

```text
US CPI higher than expected
Fed speech more hawkish than previous
2s10s curve steepened
USD broadly strengthened
Brent front spread moved into stronger backwardation
Gold rallied despite USD strength
```

好的事件抽取不是只写 headline。
它要写清：

```text
event
surprise
affected variable
market reaction
uncertainty
```

模板：

```text
Event:
  What happened?
  Was it expected?
  Which variable changed?
  Which asset reacted?
  Was reaction consistent with theory?
  What else could explain it?
```

## Layer 4: Retrieval and Memory

Daily Brief 必须有记忆。

否则每天都像第一次看市场。

记忆层包括：

```text
previous daily briefs
forecast ledger
event history
asset-specific notes
central bank stance history
macro regime notes
cross-asset causal templates
```

RAG 检索的问题可以是：

```text
上一次 CPI surprise 之后 US10Y 怎么走？
过去三次 Fed hawkish surprise 对 DXY 的影响是什么？
oil inventory draw 对 Brent curve 的历史解释是什么？
gold 在 real yield 上升时为什么还会涨？
carry trade unwind 的历史模式有哪些？
```

这一步让 brief 从“新闻摘要”升级为“研究记忆”。

```text
retrieval turns today's headline into historical context.
```

中文：

```text
检索把今天的 headline 放回历史上下文。
```

## Layer 5: Cross-Asset Reasoning

FICC 的价值在 cross-asset。

Daily Brief 最核心的推理是：

```text
macro event -> rates -> FX -> commodities -> credit -> risk sentiment
```

常见链条：

```text
inflation upside surprise
  -> front-end yields up
  -> real rates up
  -> USD stronger
  -> gold pressure
  -> credit risk depends on growth implication
```

```text
oil supply shock
  -> oil price up
  -> headline inflation pressure
  -> commodity exporter FX support
  -> importer terms-of-trade pressure
  -> central bank reaction may become more hawkish
```

```text
risk-off shock
  -> equities down
  -> credit spreads wider
  -> safe haven demand
  -> USD / JPY / CHF reaction
  -> yields may fall if growth fear dominates
```

但系统必须允许“不一致”。

例如：

```text
USD up and gold up at the same time
```

这不是错误。
可能说明：

```text
safe haven demand dominates real-rate pressure
```

又例如：

```text
yields up but credit spreads also widen
```

可能说明：

```text
market is pricing inflation and financial condition tightening together
```

所以 cross-asset reasoning 不能只套公式。
它要列出多个机制，并给证据排序。

## Narrative Ranking

每天市场都会有多个叙事。

例如：

```text
hawkish Fed
growth slowdown
oil supply risk
China demand weakness
USD funding stress
carry unwind
risk-on rebound
```

Daily Brief 要做的是 narrative ranking：

```text
哪一个叙事最能解释今天的资产联动？
哪一个叙事只是局部解释？
哪一个叙事和市场价格冲突？
哪一个叙事需要明天继续验证？
```

评分可以先简单：

```text
Narrative Score =
  price confirmation
  + cross-asset consistency
  + source reliability
  + historical similarity
  - counter-evidence
```

输出：

```text
Main Narrative:
  Fed repricing dominated today's FICC market.

Supporting Evidence:
  front-end yields rose
  USD strengthened
  gold weakened
  rate-sensitive equities underperformed

Counter-Evidence:
  credit spreads did not widen materially
  oil move was driven by inventory headline

Watch:
  next CPI and Fed speakers
```

这就是研究组织能力。

## Evidence and Counter-Evidence

每个 claim 都必须绑定证据。

Claim 示例：

```text
Today's USD strength was mainly driven by rate differential rather than broad risk-off.
```

Evidence：

```text
US front-end yields rose more than peers.
EUR/USD and USD/JPY moved in direction consistent with rate spread widening.
Equity drawdown was limited, and credit spreads were stable.
```

Counter-evidence：

```text
Some safe-haven assets also rallied.
FX vol rose modestly.
Commodity FX weakened more than rates alone would suggest.
```

系统输出必须保持这种结构：

```text
claim
evidence
counter-evidence
confidence
what would change my mind
```

这也是面试和研究训练里最重要的能力。

## Forecast Ledger

Daily Brief 必须带 forecast ledger。

否则每天写得再漂亮也不能进化。

Ledger 记录：

```text
date
claim
asset
horizon
expected direction
confidence
evidence
counter-evidence
actual outcome
review
lesson
```

示例：

```text
Claim:
  If Fed speakers continue hawkish tone, front-end yields may remain supported this week.

Horizon:
  1 week

Evidence:
  stronger inflation surprise, repricing in front-end curve

Counter-evidence:
  risk-off growth concern may pull yields lower

Review:
  Pending
```

这里要注意：

```text
forecast 不等于 trading signal。
```

它可以只是研究假设：

```text
research hypothesis
scenario expectation
risk monitor
```

Forecast Ledger 的意义是：

```text
让研究判断可复盘，让模型和人都能校准。
```

## Human Review

FICC brief 不能完全交给 agent 自动发布。

至少要有人审这几个点：

```text
1. 是否把相关性误写成因果？
2. 是否漏掉关键反例？
3. 是否输出了不该公开的 alpha 或内部信息？
4. 是否把教育材料写成了交易建议？
5. 是否对市场结论过度自信？
6. 是否存在数据源不可靠或时间戳错误？
7. 是否对事件理解错了国家、货币、合约或期限？
```

Human review checklist：

```text
Source checked?
Timestamp checked?
Asset direction checked?
Unit checked?
Claim supported?
Counter-evidence included?
No confidential data?
No trading instruction?
Confidence calibrated?
Next watch item clear?
```

这就是我们之前一直讲的：

```text
human PM review
```

它不是形式主义。
它是 Research OS 的安全阀和质量阀。

## Brief 模板

下面是一份可复用模板。

````markdown
# Daily FICC Brief

Date:
Prepared by:
Review status:

## 1. Executive Summary

- Main narrative:
- Key market move:
- Cross-asset consistency:
- Biggest uncertainty:
- Human review notes:

## 2. Market Snapshot

| Asset | Latest | Change | Interpretation |
|---|---:|---:|---|
| US 2Y |  |  |  |
| US 10Y |  |  |  |
| 2s10s |  |  |  |
| DXY |  |  |  |
| EUR/USD |  |  |  |
| USD/JPY |  |  |  |
| Brent |  |  |  |
| Gold |  |  |  |
| Copper |  |  |  |
| IG spread |  |  |  |
| HY spread |  |  |  |

## 3. Macro and Policy Calendar

- Today's releases:
- Surprise vs expectation:
- Central bank comments:
- Next key events:

## 4. Fixed Income

- Rates:
- Curve:
- Inflation expectation:
- Credit:
- Funding:
- Main FI interpretation:

## 5. FX

- USD:
- G10:
- EM:
- Forward / basis:
- Vol:
- Main FX interpretation:

## 6. Commodities

- Energy:
- Metals:
- Agriculture:
- Curve / inventory:
- Main commodities interpretation:

## 7. Cross-Asset Causal Chain

```text
event
  -> macro variable
  -> rates / FX / commodities reaction
  -> risk sentiment
  -> open question
```

## 8. Evidence

- Evidence 1:
- Evidence 2:
- Evidence 3:

## 9. Counter-Evidence

- Counter-evidence 1:
- Counter-evidence 2:
- Counter-evidence 3:

## 10. Forecast Ledger Review

| Previous Claim | Horizon | Outcome | Lesson |
|---|---|---|---|
|  |  |  |  |

## 11. New Research Hypotheses

| Claim | Horizon | Evidence | Counter-Evidence | Confidence |
|---|---|---|---|---|
|  |  |  |  |  |

## 12. Watchlist

- Watch item 1:
- Watch item 2:
- Watch item 3:
````

## JSON Schema

为了让机器能读，brief 还应该有 JSON 版。

第一版 schema 可以很简单：

```json
{
  "date": "YYYY-MM-DD",
  "main_narrative": "",
  "market_regime": "",
  "confidence": "low|medium|high",
  "sections": {
    "fixed_income": {
      "summary": "",
      "key_moves": [],
      "claims": []
    },
    "fx": {
      "summary": "",
      "key_moves": [],
      "claims": []
    },
    "commodities": {
      "summary": "",
      "key_moves": [],
      "claims": []
    }
  },
  "cross_asset_chains": [],
  "evidence": [],
  "counter_evidence": [],
  "forecast_ledger_updates": [],
  "watchlist": [],
  "human_review": {
    "required": true,
    "status": "pending",
    "notes": []
  }
}
```

后续可以升级成：

```text
Pydantic models
SQLite storage
DuckDB analytics
vector database
graph database
static markdown publishing
```

但第一版不需要过度工程化。

## Agent Workflow

可以把 Daily Brief Generator 拆成多个 agent。

```text
Data Agent:
  收集 market data / macro data / text inputs

Event Agent:
  抽取今天发生的事件

Rates Agent:
  分析 rates / curve / credit / funding

FX Agent:
  分析 spot / forward / basis / USD / carry

Commodities Agent:
  分析 energy / metals / agriculture / inventory / curve

Cross-Asset Agent:
  生成跨资产因果链

Skeptic Agent:
  找反例、找不一致、降低过度自信

Writer Agent:
  生成 markdown / json brief

Reviewer:
  人类审核并批准发布
```

注意这里的 agent 不应该互相“聊天到失控”。
应该由 workflow 控制：

```text
fixed inputs
fixed outputs
schema validation
evidence required
counter-evidence required
human review required
```

这就是 harness 的意义。

## Harness 设计

Daily FICC Brief 的 harness 可以定义为：

```text
task
input schema
tool permissions
retrieval scope
output schema
validation rules
review rules
logging rules
```

例如：

```text
Task:
  Generate daily FICC brief from public-safe inputs.

Allowed tools:
  local markdown notes
  approved public data
  approved RAG index
  forecast ledger

Not allowed:
  client data
  confidential documents
  live trading orders
  private alpha database

Validation:
  every claim needs evidence
  every narrative needs counter-evidence
  every forecast needs horizon
  all trading advice language removed
  human review before publish
```

这直接连接我们的 `PENGYI_HARNESS_MAP`。

## RAG 设计

RAG 的 corpus 可以分几类：

```text
FICC knowledge notes:
  FICC000 - FICC003

official documents:
  central bank statements
  EIA reports
  WASDE reports
  macro releases

market memory:
  previous daily briefs
  forecast ledger
  event history

public research:
  sanitized public notes
  educational materials
```

检索策略：

```text
query by asset
query by event
query by macro variable
query by regime
query by historical analogy
```

例子：

```text
Query:
  oil price up and USD up, historical interpretation

Retrieve:
  commodity inflation notes
  USD liquidity notes
  previous oil shock briefs
  FICC003 oil / inflation section
  FICC002 USD / commodity FX section
```

RAG 的目标不是替代判断。
目标是给判断提供上下文和证据。

## Graph 设计

Graph 可以让 cross-asset 更清晰。

核心节点：

```text
Asset
Currency
Country
CentralBank
MacroVariable
Commodity
Curve
Event
Claim
Evidence
Forecast
```

核心关系：

```text
CentralBank -> sets -> PolicyRate
PolicyRate -> affects -> YieldCurve
YieldCurve -> affects -> FXForward
Commodity -> affects -> Inflation
Inflation -> affects -> CentralBankReaction
USD -> affects -> CommodityPrice
CommodityShock -> affects -> TermsOfTrade
TermsOfTrade -> affects -> Currency
Event -> supports -> Claim
Evidence -> supports -> Claim
CounterEvidence -> challenges -> Claim
Forecast -> reviews -> Claim
```

这样 brief 可以回答：

```text
今天这个事件影响了哪些节点？
哪些资产反应一致？
哪些资产反应冲突？
哪些旧 claim 被支持？
哪些旧 claim 被挑战？
```

这就是 Graph RAG 在 FICC 里的实际落点。

## Evaluation

Daily Brief Generator 也需要评估。

不能只看文字漂不漂亮。

可以评估：

```text
coverage
factuality
source grounding
cross-asset consistency
counter-evidence quality
forecast calibration
human edit distance
time saved
review pass rate
```

具体指标：

```text
Coverage:
  是否覆盖 FI / FX / Commodities 三块

Grounding:
  关键 claim 是否有来源或 observation 支持

Consistency:
  跨资产因果链是否自洽

Skepticism:
  是否主动列出反例

Calibration:
  预测 confidence 是否和后续结果匹配

Human Review:
  人类需要改多少
```

第一版可以做人工评分：

```text
1 - poor
2 - acceptable
3 - good
4 - very good
5 - publishable
```

每天评一次。
这就是持续进化。

## MVP 怎么做

第一版 MVP 不要复杂。

目标：

```text
用本地 markdown + CSV + JSON 生成一份 daily brief。
```

文件结构：

```text
ficc_daily_brief/
  data/
    market_snapshot.csv
    macro_calendar.csv
    events.md
  ledger/
    forecast_ledger.csv
  notes/
    ficc000.md
    ficc001.md
    ficc002.md
    ficc003.md
  outputs/
    daily_ficc_brief_YYYY-MM-DD.md
    daily_ficc_brief_YYYY-MM-DD.json
  src/
    ingest.py
    extract_events.py
    generate_brief.py
    validate_brief.py
```

MVP 流程：

```text
1. 人工填一份 market_snapshot.csv。
2. 人工写当天 events.md。
3. 系统读取 FICC 笔记和 ledger。
4. 生成 markdown brief。
5. 生成 json brief。
6. validate 检查 claim / evidence / counter-evidence。
7. 人类审核。
8. 写入 forecast ledger。
```

这个版本已经足够展示能力：

```text
domain understanding
research workflow
agent harness
structured output
human review
forecast ledger
public-safe finance AI
```

## Pseudocode

第一版伪代码：

```python
def run_daily_ficc_brief(date: str):
    market = load_market_snapshot(date)
    macro = load_macro_calendar(date)
    events = load_event_notes(date)
    ledger = load_forecast_ledger()
    memory = load_ficc_notes()

    observations = normalize_market_data(market)
    extracted_events = extract_events(events, macro)

    rates_view = analyze_rates(observations, extracted_events, memory)
    fx_view = analyze_fx(observations, extracted_events, memory)
    commodities_view = analyze_commodities(observations, extracted_events, memory)

    chains = build_cross_asset_chains(
        rates_view=rates_view,
        fx_view=fx_view,
        commodities_view=commodities_view,
        events=extracted_events,
    )

    claims = rank_narratives(chains)
    claims = attach_evidence_and_counter_evidence(claims, memory, observations)

    brief = render_markdown_brief(
        date=date,
        observations=observations,
        events=extracted_events,
        rates_view=rates_view,
        fx_view=fx_view,
        commodities_view=commodities_view,
        claims=claims,
        ledger=ledger,
    )

    validate_brief(brief)
    save_outputs(brief)
    return brief
```

这里真正重要的是接口。

每个模块都应该输出结构化对象，而不是自由文本乱飞。

## 面试怎么讲

如果被问：

```text
你说你做 FICC AI Research OS，具体是什么？
```

可以回答：

```text
我会先做 Daily FICC Brief Generator。
它不是自动交易系统，而是研究工作流系统。输入包括公开 market data、macro calendar、central bank text、official reports、news event notes 和 forecast ledger；系统把这些信息标准化成 observations、events、claims 和 evidence，然后分别生成 rates、FX、commodities 三条线的分析，再通过 cross-asset reasoning 合成主叙事、反例和下一步 watchlist。最后必须经过 human review，并把新研究假设写入 forecast ledger，方便后续复盘和校准。
```

如果被问：

```text
这个和普通新闻摘要有什么区别？
```

可以回答：

```text
普通摘要只是压缩文本。Daily FICC Brief Generator 要做的是研究结构化：每个 market narrative 都要绑定 evidence 和 counter-evidence；每个观点要放进 FI、FX、Commodities 的跨资产链条里；每个 forecast 都有 horizon、confidence 和后续 review。这使它可以被审计、复盘和持续改进。
```

如果被问：

```text
为什么要 human review？
```

可以回答：

```text
金融研究里错误的因果解释和过度自信很危险。Human review 用来检查数据源、时间戳、因果链、反例、保密边界和语言边界，确保输出是 public-safe research note，而不是未经验证的交易建议。
```

## 和 Quant Research OS 的连接

Daily FICC Brief 是 Quant Research OS 的上游。

它输出：

```text
events
narratives
hypotheses
watchlist
forecast outcomes
```

这些可以进入：

```text
event study
factor hypothesis generation
regime classification
risk dashboard
macro feature library
strategy research queue
```

例如：

```text
brief detects:
  oil inventory shock + curve backwardation strengthens + commodity FX reacts

research queue creates:
  event study on inventory surprise and oil curve
  commodity FX sensitivity analysis
  inflation expectation follow-through study
```

这就是：

```text
research brief -> structured hypothesis -> quant experiment -> forecast review
```

我们最终要做的是闭环。

## 和 Credit OS 的连接

这套系统也能服务个人 credit。

因为它可以公开展示：

```text
金融市场理解
AI agent workflow
RAG / Graph / Harness 思维
工程化能力
研究复盘能力
public-safe writing
```

对外呈现时，不要展示内部 alpha。
展示的是：

```text
how I think
how I structure research
how I build systems
how I control risk
how I review evidence
```

这就是 public credit。

## 下一步

`FICC004` 完成后，后面可以继续：

```text
FICC005:
  FICC Event-to-Signal Workflow

FICC006:
  FICC RAG / Graph RAG System

FICC007:
  FICC Forecast Ledger and Evaluation

FICC008:
  FICC Interview Playbook
```

如果要进入工程落地，最建议下一步是：

```text
FICC005 -> Event-to-Signal Workflow
```

不是直接实盘。
而是把事件、假设、验证和复盘流程打通。

## 当前结论

Daily FICC Brief Generator 是 FICC 系列从知识到系统的第一步。

它把：

```text
Fixed Income
FX
Commodities
Macro
News
RAG
Graph
Forecast Ledger
Human Review
```

合成一个每日研究工作流。

核心不是“让 AI 预测市场”。
核心是：

```text
让研究过程结构化、可追踪、可复盘、可审计、可持续进化。
```

一句话收束：

```text
Daily FICC Brief Generator turns market noise into structured research memory.
```

中文：

```text
Daily FICC Brief Generator 把市场噪音变成结构化研究记忆。
```
