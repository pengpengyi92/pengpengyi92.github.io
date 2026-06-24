---
title: "HKUDS005: AI-Trader 作为 Agent-Native Live Trading Platform Layer"
date: 2026-06-24 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds005, hkuds, ai-trader, agent-native-trading, live-trading-os, copy-trading, paper-trading, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第六篇。

```text
HKUDS005 -> AI-Trader
```

到目前为止，HKUDS 第一阶段我们已经看了：

```text
HKUDS000 -> study map
HKUDS001 -> LightRAG
HKUDS002 -> Vibe-Trading
HKUDS003 -> nanobot
HKUDS004 -> CLI-Anything
```

现在看 `AI-Trader`。我对它的定位是：

```text
AI-Trader = Agent-Native Live Trading Platform Layer
```

如果说：

```text
LightRAG     = research memory
Vibe-Trading = quant research workflow
nanobot      = personal agent shell
CLI-Anything = software action layer
```

那么：

```text
AI-Trader = 让 agent 真正进入一个可注册、可发言、可交易、可跟单、可比赛、可研究导出的交易平台
```

这点非常关键。因为很多 quant agent 项目只停留在：

```text
generate idea
write strategy
run backtest
print report
```

但 `AI-Trader` 关注的是另一层：

```text
agent identity
agent onboarding
signal publishing
strategy discussion
realtime operation
copy trading
paper position
leaderboard
challenge
experiment assignment
heartbeat notification
research export
```

也就是说，它不是单纯策略库，也不是单纯回测框架，而是一个 agent-native 的 trading platform。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `AI-Trader`。

| Item | Value |
|---|---|
| repo | `AI-Trader` |
| branch | `main` |
| local head | `d03ff6c` |
| latest local commit | `Merge pull request #255 from HKUDS/codex/ignore-research-local` |
| status | clean |
| license | MIT |
| backend | FastAPI |
| frontend | React / TypeScript |
| database | PostgreSQL for production, SQLite for local quick start |

本地规模：

| Metric | Count |
|---|---:|
| total files | 143 |
| Python files | 74 |
| TypeScript / TSX files | 12 |
| Markdown files | 16 |
| backend tests | 21 |

主结构可以先压成：

```text
AI-Trader/
  README.md
  README_ZH.md
  docs/
    README_AGENT.md
    README_USER.md
    api/
  skills/
    ai4trade/
    copytrade/
    tradesync/
    heartbeat/
    polymarket/
    market-intel/
  service/
    server/
    frontend/
  research/
    README.md
    schemas/
    scripts/
```

这已经说明它不是一个 notebook 项目，而是一个完整平台项目。

## Core Thesis

README 的核心判断很直接：

```text
Just like humans have their trading platforms, AI agents need their own.
```

这句话就是 `AI-Trader` 的核心。

人类交易员有：

```text
broker account
trading terminal
watchlist
leaderboard
social feed
copy trading
portfolio
orders
notifications
competition
research dashboard
```

那么 agent 也需要对应的：

```text
agent account
token identity
skill onboarding
HTTP trading API
signal feed
discussion feed
paper positions
copy trading
heartbeat
challenge
experiment assignment
research export
```

传统交易平台假设用户是人：

```text
click
read chart
type note
submit order
watch dashboard
```

`AI-Trader` 假设用户可以是 agent：

```text
read SKILL.md
register
get token
poll heartbeat
publish strategy
publish operation
reply discussion
follow leader
copy position
join challenge
export research data
```

这就是 agent-native trading platform 的含义。

## What Problem It Solves

`AI-Trader` 解决的不是“怎么生成一个策略”，而是“当很多 agent 都在生成策略之后，它们在哪里互动、交易、比较、协作、被研究”。

一个真实的 agent trading ecosystem 至少需要这些问题：

| Problem | AI-Trader 的处理方式 |
|---|---|
| agent 怎么进入系统 | `SKILL.md` onboarding + registration |
| agent 身份怎么管理 | agent token / account / points |
| agent 怎么发观点 | strategy / discussion / realtime signal |
| agent 怎么产生仓位 | realtime operation updates paper positions |
| agent 怎么学习别人 | feed / replies / following / copy trading |
| agent 怎么被市场评价 | leaderboard / profit history / metric snapshots |
| agent 怎么参加比赛 | challenge / team challenge |
| agent 怎么进入实验 | experiment assignment / task / notification |
| 研究者怎么复现数据 | research exports / schemas / analysis scripts |
| 系统怎么长期运行 | FastAPI + DB + background worker + frontend |

这套东西对我们做 `Pengyi Quant Research OS` 很有启发：不要只做一个会写策略的 agent，而要做一个能持续沉淀交易行为、研究证据、团队协作和实验数据的平台。

## Agent Onboarding

`AI-Trader` 的 agent 接入方式非常有代表性：

```text
Read https://ai4trade.ai/SKILL.md and register.
```

这句话背后是 skill-first architecture。

agent 不需要先读完整 README，也不需要人类手把手教它网页怎么点。它只要能读 skill 文件，就能知道：

```text
base URL
registration endpoint
login endpoint
token usage
signal feed endpoint
how to publish strategy
how to publish realtime operation
how to follow/copy
how to heartbeat
which child skill to read
```

主 skill 是：

```text
skills/ai4trade/SKILL.md
```

子 skill 包括：

```text
skills/copytrade/SKILL.md
skills/tradesync/SKILL.md
skills/heartbeat/SKILL.md
skills/polymarket/SKILL.md
skills/market-intel/SKILL.md
```

主 skill 的设计很清晰：

| Skill | Role |
|---|---|
| `ai4trade` | 主入口：注册、登录、feed、路由规则 |
| `copytrade` | 跟随交易员、查看跟随、自动复制仓位 |
| `tradesync` | 发布实时操作、策略、讨论、同步交易信号 |
| `heartbeat` | agent 周期性拉取消息、任务、提醒 |
| `polymarket` | 直接读取 Polymarket 公共市场数据 |
| `market-intel` | 读取金融事件和市场情报快照 |

这套结构和 `CLI-Anything` 的思路是相通的：不是把文档写给人看，而是把能力协议写给 agent 看。

## Registration And Identity

agent 注册入口是：

```text
POST /api/claw/agents/selfRegister
```

注册后返回：

```text
agent_id
name
token
```

token 就是 agent 的身份。之后调用 API 时，通过 bearer token 或 `X-Claw-Token` 进行认证。

这点对我们很重要，因为一个真实的 quant research platform 不能只有 anonymous script。它需要：

```text
who generated this idea
who traded it
who copied it
who replied to it
who accepted the reply
who joined the challenge
who belongs to which experiment variant
```

没有 identity，就没有 reputation；没有 reputation，就没有长期 credit；没有长期 credit，就无法形成真正的 agent market。

## Three Signal Types

`AI-Trader` 把 agent 的表达分成三类。

| Type | Endpoint | Meaning |
|---|---|---|
| Strategy | `POST /api/signals/strategy` | 研究观点、策略逻辑、交易 thesis |
| Discussion | `POST /api/signals/discussion` | 提问、反驳、补充、协作讨论 |
| Operation / Realtime | `POST /api/signals/realtime` | buy / sell / short / cover 等实时操作 |

这个分类非常合理。

因为一个交易 agent 不能只输出 order。真实 trading 里至少有三层：

```text
why        -> strategy
debate     -> discussion
what to do -> operation
```

如果一个平台只记录 order，就很难研究：

```text
agent 的 reasoning 是否可靠
agent 是否听取别人反馈
哪些讨论改变了交易决策
哪些策略后来被市场验证
哪些观点被其他 agent copy
```

`AI-Trader` 把 strategy、discussion、operation 放在同一个平台里，后面就可以做 network analysis、signal quality scoring、experiment evaluation。

## Realtime Operations

实时操作通过：

```text
POST /api/signals/realtime
```

请求结构大致是：

```json
{
  "market": "crypto",
  "action": "buy",
  "symbol": "BTC",
  "price": 65000,
  "quantity": 0.1,
  "content": "Breakout with strong volume.",
  "executed_at": "now"
}
```

支持的 action 包括：

```text
buy
sell
short
cover
```

后端不只是把这条 signal 存起来，还会尝试更新 paper position。

这说明 realtime signal 同时承担两个角色：

```text
public signal
position event
```

也就是说，agent 发出的实时操作既是给别人看的交易信号，也是系统内部更新仓位、收益、排行榜、跟单关系的数据源。

## Paper Trading Model

`AI-Trader` 的默认初始资金是：

```text
100000 USD
```

后端里有：

```text
agents.cash
agents.deposited
positions
profit_history
fees
price_fetcher
```

这让它具备 paper trading 的基本闭环：

```text
agent publishes operation
  -> validate market/action/symbol/quantity/price
  -> fetch or check price
  -> update cash and positions
  -> compute mark-to-market value
  -> update profit history
  -> show leaderboard
```

这和单纯 backtest 不一样。backtest 通常是：

```text
historical data -> strategy -> simulated trades -> metrics
```

`AI-Trader` 更像：

```text
living agent community -> ongoing paper trades -> live ranking -> research dataset
```

对我们来说，这就是 `Live Trading OS` 的雏形。

## Supported Markets

README 里写得很大：

```text
stocks, crypto, forex, options, futures
```

但从本地实现看，目前最明确的核心市场是：

```text
us-stock
crypto
polymarket
```

价格源包括：

| Market | Price Source |
|---|---|
| US stocks | Alpha Vantage, with yfinance fallback |
| Crypto | Hyperliquid public endpoint |
| Polymarket | Gamma API + CLOB orderbook |

这里要注意一个工程判断：README 的 product vision 可以写得大，但研究和工程实现要看代码里真实支持了什么。

目前这套实现最值得我们学习的是：

```text
market abstraction
price source fallback
rate limit / cooldown
paper execution validation
Polymarket special handling
```

尤其是 Polymarket。它不是普通股票，不应该直接 short / cover。代码里也单独约束了 Polymarket 不能做 short/cover，并且需要 outcome / token_id 等信息。

## Copy Trading

`copytrade` skill 说明了平台的另一个核心：copy trading。

关键接口包括：

```text
GET  /api/signals/feed
POST /api/signals/follow
POST /api/signals/unfollow
GET  /api/signals/following
GET  /api/positions
GET  /api/signals/{agent_id}?type=position&limit=50
```

核心行为是：

```text
follow leader
leader opens / updates / closes position
follower mirrors the position
```

当前实现里跟单比例偏简单，主要是 1:1 copy。未来可以扩展：

```text
capital ratio
risk cap
max drawdown stop
symbol whitelist
market whitelist
confidence threshold
quality score threshold
manual confirmation
```

这对我们的启发是：copy trading 不是单纯“抄作业”，而是 agent ecosystem 里的 social learning mechanism。

一个 agent 如果长期做对，别人会 follow；如果它的 signal 被别人 copy，就形成 reputation 和 points。

## Heartbeat

`heartbeat` skill 是我认为最关键的工程设计之一。

它明确说：

```text
AI-Trader uses pull-based polling.
Heartbeat is the primary mechanism.
WebSocket is optional.
```

核心接口是：

```text
POST /api/claw/agents/heartbeat
```

建议频率：

```text
minimum: every 30 seconds
recommended: every 60 seconds
max: 5 minutes
```

heartbeat 返回：

```text
messages
tasks
notifications
```

它解决的是 agent 长期在线的问题。

人类交易员可以看网页、看通知、看群消息。agent 如果没有 heartbeat，就会变成一次性脚本：

```text
run once
post once
exit
```

但有了 heartbeat，agent 就可以持续感知：

```text
new reply
new follower
trade copied
signal mention
experiment assignment
challenge invite
team mission task
```

这和 `nanobot` 的 always-on personal agent shell 很契合。`nanobot` 可以作为运行壳，`AI-Trader` 可以作为交易平台和外部事件源。

## Backend Architecture

后端入口是：

```text
service/server/main.py
```

它会：

```text
init database
create FastAPI app
register routes
start background tasks if enabled
run uvicorn
```

路由注册集中在：

```text
service/server/routes.py
```

核心模块：

| Module | Role |
|---|---|
| `routes_agent.py` | agent 注册、登录、heartbeat、消息、任务、WebSocket |
| `routes_signals.py` | strategy、discussion、realtime signal、reply、feed |
| `routes_trading.py` | positions、price、leaderboard、profit history、follow/unfollow |
| `routes_challenges.py` | challenge 创建、加入、交易、提交、排行榜、结算 |
| `routes_experiments.py` | experiment 创建、分组、通知、任务、奖励 |
| `routes_team_missions.py` | team mission、组队、提交、结算 |
| `routes_market.py` | health、market-intel |
| `routes_research.py` | research dataset export |
| `routes_users.py` | human user 注册、登录、points |
| `routes_misc.py` | skill 文件、静态页面、SPA fallback |

简化架构图：

```text
Agent / Human / Frontend
  -> FastAPI routes
    -> services
    -> database
    -> price_fetcher
    -> rewards / fees / permissions
    -> experiments / challenges / team missions
    -> research_exports
  -> background worker
    -> price refresh
    -> profit history
    -> Polymarket settlement
    -> market intel snapshots
  -> React frontend
    -> leaderboard
    -> market
    -> challenges
    -> experiments
    -> research exports
```

这个结构不复杂，但覆盖面很完整。

## Database Layer

`database.py` 支持两种运行模式：

```text
PostgreSQL -> production / shared deployment
SQLite     -> local quick start / small sample
```

它还有一层 SQL adaptation，把一些 SQLite 写法转换成 PostgreSQL 写法，例如：

```text
? placeholders -> %s
datetime function differences
AUTOINCREMENT differences
REAL -> DOUBLE PRECISION
ALTER TABLE ADD COLUMN -> IF NOT EXISTS
```

这说明项目方希望本地开发尽量轻，但生产环境可以上 PostgreSQL。

关键表可以分几组理解。

交易与 agent：

```text
agents
signals
signal_replies
subscriptions
positions
profit_history
points_transactions
```

实验与挑战：

```text
experiments
experiment_assignments
experiment_events
agent_reward_ledger
challenges
challenge_participants
challenge_trades
challenge_results
challenge_teams
challenge_team_members
challenge_submissions
```

团队任务：

```text
team_missions
teams
team_members
team_messages
team_submissions
team_contributions
team_results
```

研究与质量：

```text
signal_predictions
signal_quality_scores
agent_metric_snapshots
network_edges
```

市场情报：

```text
market_news_snapshots
macro_signal_snapshots
etf_flow_snapshots
stock_analysis_snapshots
```

这就是从 platform 到 research dataset 的底座。

## Frontend Layer

前端在：

```text
service/frontend/src/
```

主要页面包括：

```text
LandingPage
Leaderboard
Market
FinancialEvents
ChallengePage
ExperimentAdminPage
ResearchExportsPage
TeamMissionsPage
```

从前端也能看出它不是 demo landing page，而是已经把用户工作流拆成了多个真实页面：

```text
看市场
看排行
看金融事件
看交易员
参与讨论
参加 challenge
管理 experiment
导出 research data
做 team mission
```

这点对我们做个人网站和 Research OS 也有启发：前端不是装饰，而是研究流程的入口。

如果未来我们做自己的 `Pengyi Quant Research OS v0`，也不应该只有 README，而应该有最小 dashboard：

```text
factor idea board
experiment board
backtest report board
agent leaderboard
data source status
research export
```

## Challenge System

`AI-Trader` 有完整的 challenge 模块。

challenge 可以：

```text
create
join
trade
submit
vote
rank
settle
notify
```

支持：

```text
individual
team
hybrid
```

挑战可以绑定：

```text
market
symbol
initial_capital
max_position_pct
max_drawdown_pct
scoring_method
experiment_key
```

这非常像一个小型 quant competition platform。

对我们来说，它可以映射到：

```text
WorldQuant factor training arena
LLMQuant factor generation benchmark
Pengyi factor tournament
agent strategy contest
paper trading challenge
```

但有一个重要边界：如果我们未来使用 WorldQuant 因子作为训练场，公开版本必须脱敏，不能暴露任何 private / proprietary factor。公开平台只放 sanitized demo factors。

## Experiment System

`AI-Trader` 不只是比赛，还有 experiment。

实验模块支持：

```text
experiment creation
variant assignment
agent notification
agent task creation
challenge report
reward tracking
```

前端 `ExperimentAdminPage` 里默认 variants 示例是：

```json
[
  { "key": "control", "weight": 1, "reward_mode": "fixed" },
  { "key": "quality-weighted", "weight": 1, "reward_mode": "quality_weighted", "reward_multiplier": 1.4 }
]
```

这很有研究味道。

它说明平台不只是“让 agent 玩”，而是要研究：

```text
不同激励机制会不会改变 agent 行为
质量加权奖励是否提升信号质量
challenge 是否促进合作
team mission 是否提升 collective intelligence
通知和任务是否提高参与度
```

这就是可以写 paper 的地方。

## Research Export

`research/README.md` 说得很明确：这个目录用于把平台数据转成可复现的论文数据、指标、统计表和图。

它支持一条流水线：

```bash
python research/scripts/export_research_dataset.py --output-dir research/exports
python research/scripts/analyze_experiments.py --input-dir research/exports --output-dir research/exports/tables
python research/scripts/generate_figures.py --input-dir research/exports --tables-dir research/exports/tables --output-dir research/exports/figures
```

核心导出：

```text
agents.csv
events.csv
signals.csv
network_edges.csv
```

还有 challenge 和 team mission 相关数据：

```text
challenges
challenge_participants
challenge_submissions
challenge_trades
challenge_results
team_missions
teams
team_members
team_messages
team_submissions
team_contributions
team_results
```

研究脚本覆盖：

```text
A/B tests
DiD
regression
heterogeneous treatment effects
bootstrap confidence intervals
FDR tables
paper figures
```

这就是 `AI-Trader` 最像 research platform 的地方。

不是只做一个网站，而是把网站行为转成可以分析的 research dataset。

## Network Edges

`experiment_metrics.py` 里有 network edge 构建。

关系包括：

```text
reply
follow
accepted_reply
adoption
mentions
```

这说明平台可以研究 agent social graph：

```text
谁影响谁
谁经常被采纳
谁被 copy
谁的观点传播得更远
合作网络是否提升收益
高质量 agent 是否更容易形成中心节点
```

这对顶会论文很重要。单纯收益率很难支撑一个 AI agent 研究贡献，但如果能研究 multi-agent market、collective intelligence、incentive design、network effects，就会更像 AI / HCI / economics / finance crossover 的研究。

## Signal Quality

`signal_quality.py` 里有一个 heuristic quality scoring。

它尝试从 signal 中抽取：

```text
direction
target price / probability
confidence
evidence
```

并计算：

```text
verifiability
evidence
specificity
novelty
review
overall_score
```

这点非常关键。

交易平台如果只奖励发帖数量，agent 很容易变成噪声机器：

```text
post more
trade more
farm points
copy blindly
```

所以必须引入 quality layer：

```text
is the signal verifiable
does it cite evidence
is it specific enough
is it novel or duplicate
does it receive review / acceptance
does later market data validate it
```

未来我们做 `Quant R&D Agent`，也要有类似机制。

不能只看：

```text
生成了多少因子
跑了多少回测
```

还要看：

```text
因子假设是否清楚
数据是否无泄漏
回测是否稳健
风险暴露是否解释清楚
样本外是否保留
是否被下一轮研究复用
```

## Market Intelligence

`market-intel` skill 是只读市场情报层。

它提供：

```text
overview
macro-signals
etf-flows
featured stocks
stock latest
stock history
news
```

代码里能看到对这些信息源的处理：

```text
Alpha Vantage news sentiment
macro signal snapshots
ETF flow snapshots
stock analysis snapshots
optional Adanos sentiment
OpenRouter optional summarization
```

这个层的定位不是 execution，而是 context。

换句话说：

```text
market-intel = before trading, read context
tradesync    = after decision, publish signal
copytrade    = after trust, follow/copy
heartbeat    = after joining, stay alive
```

这种分工很清楚。

对我们来说，这可以映射成：

```text
data source status
market regime summary
news/event context
factor environment
risk regime
macro context
```

不要让 agent 裸着跑回测。它应该先知道市场环境。

## Polymarket Design

`polymarket` skill 的设计也值得记。

它强调：

```text
Read Polymarket public market data directly from Polymarket APIs.
Do not route market discovery through AI-Trader.
```

也就是说：

```text
AI-Trader handles simulated execution and social sharing
Polymarket public APIs handle market metadata and orderbook
```

这个边界很好。

系统不应该把所有外部世界都包进自己 API，而应该明确：

```text
which data should be fetched from source
which action belongs to platform
which state belongs to platform
```

这是我们之后做数据源系统时必须吸收的思路。

## Background Worker

README 里提到一次重要升级：FastAPI service 和 background workers 分离。

这在交易平台里是必要的。因为这些任务不能阻塞请求：

```text
price refresh
profit history refresh
Polymarket settlement
market news refresh
macro signal refresh
ETF flow refresh
stock analysis refresh
sentiment cache
```

如果所有任务都塞进 API 请求里，就会出现：

```text
request timeout
rate limit collision
price fetch instability
frontend slow
agent heartbeat lag
```

所以生产系统应该拆成：

```text
API service
worker service
database
cache
frontend
```

这一点对我们工程化 quant system 很重要。

## API Surface

从路由看，`AI-Trader` 的 API surface 已经比较完整。

agent：

```text
POST /api/claw/agents/selfRegister
POST /api/claw/agents/login
GET  /api/claw/agents/me
POST /api/claw/agents/heartbeat
GET  /api/claw/messages
GET  /api/claw/tasks
```

signals：

```text
POST /api/signals/realtime
POST /api/signals/strategy
POST /api/signals/discussion
GET  /api/signals/feed
GET  /api/signals/grouped
POST /api/signals/{signal_id}/reply
```

trading：

```text
GET  /api/positions
GET  /api/price
GET  /api/profit/history
GET  /api/trending
POST /api/signals/follow
POST /api/signals/unfollow
```

challenge：

```text
GET  /api/challenges
POST /api/challenges
POST /api/challenges/{challenge_key}/join
POST /api/challenges/{challenge_key}/trade
GET  /api/challenges/{challenge_key}/leaderboard
POST /api/challenges/{challenge_key}/submit
```

experiment：

```text
GET  /api/experiments
POST /api/experiments
GET  /api/experiments/{experiment_key}/assignments
POST /api/experiments/{experiment_key}/notify
POST /api/experiments/{experiment_key}/tasks
GET  /api/agents/me/experiments
```

research：

```text
GET /api/research/datasets
GET /api/research/events
GET /api/research/agents.csv
GET /api/research/events.csv
GET /api/research/signals.csv
GET /api/research/network_edges.csv
```

这就是一个平台，而不是一个脚本。

## Relationship With Previous HKUDS Projects

现在可以把前几篇连起来。

| Project | Layer | What It Gives Us |
|---|---|---|
| LightRAG | research memory | 把 papers / docs / reports 变成可检索、可引用、可图谱化的知识 |
| Vibe-Trading | quant workflow | 把自然语言金融问题转成数据、策略、回测和 research autopilot |
| nanobot | agent shell | 给个人 agent 一个长期运行、能接工具、能接消息的壳 |
| CLI-Anything | software action | 让 agent 稳定调用真实软件和外部工具 |
| AI-Trader | live trading platform | 让 agent 注册、发信号、交易、跟单、比赛、被研究 |

组合起来就是：

```text
LightRAG
  -> stores research memory

Vibe-Trading
  -> generates and tests quant ideas

nanobot
  -> runs the agent continuously

CLI-Anything
  -> gives the agent reliable software actions

AI-Trader
  -> puts the agent into a live trading society
```

这张图对我们很重要：

```text
research memory
  + workflow engine
  + personal agent shell
  + software action layer
  + trading platform layer
  = Research OS / Quant OS
```

## Difference From Vibe-Trading

`Vibe-Trading` 和 `AI-Trader` 很容易混在一起，但两者关注点不一样。

| Dimension | Vibe-Trading | AI-Trader |
|---|---|---|
| Main question | 如何从 idea 到策略和回测 | agent 在哪里交易、互动、比赛、被评估 |
| Core workflow | research autopilot / backtest / alpha zoo | registration / signal / copy / challenge / leaderboard |
| Primary artifact | strategy code, backtest report, research run | live signal, paper position, social graph, research export |
| User | researcher / quant agent | agent trader / human trader / platform admin |
| Data style | market data + backtest artifacts | platform behavior + trades + messages + experiments |
| Research value | strategy generation and evaluation | multi-agent trading behavior and incentive design |

所以它们不是替代关系，而是上下游关系：

```text
Vibe-Trading discovers strategies.
AI-Trader deploys them into an agent trading society.
```

## Difference From X2Strategy

之前我们也看过 X2Strategy。可以这样比较：

| System | Core Function |
|---|---|
| QuantMind | 把 paper / news / blog / pdf 转成结构化 quant knowledge |
| X2Strategy | 从结构化思路继续生成策略和回测 |
| Vibe-Trading | agentic quant research workflow 和 research autopilot |
| AI-Trader | agent-native live trading / copy trading / challenge platform |

`AI-Trader` 不只是把 knowledge 变成 strategy，而是把 agent 放进一个市场环境里。

这对我们自己的系统设计很有启发：

```text
knowledge layer  -> QuantMind / LightRAG
strategy layer   -> X2Strategy / Vibe-Trading
agent shell      -> nanobot
tool layer       -> CLI-Anything
platform layer   -> AI-Trader
```

## Pengyi Use Case

`AI-Trader` 对 `Pengyi Quant Research OS v0` 最直接的启发是：我们可以把研究系统做成 platform，而不是文件夹。

最小版本可以是：

```text
factor ideas
  -> agent proposes hypothesis
  -> agent implements factor
  -> agent runs backtest
  -> agent diagnoses bias
  -> agent publishes research note
  -> PM/human reviews
  -> system tracks quality and reuse
```

更进一步：

```text
agent identity
factor signal feed
paper portfolio
research challenge
leaderboard
experiment variants
team missions
research export
```

这就是我们之前说的 R&D Agent for Quant Research：

```text
自动提出因子假设
自动实现
自动回测
自动诊断偏差
自动生成下一轮研究计划
人类 PM 审核
```

`AI-Trader` 给了平台层参考。

## What To Copy Into Our Own System

我认为最值得吸收的是这些设计：

| AI-Trader Design | How Pengyi Can Use It |
|---|---|
| `SKILL.md` onboarding | 让任何 agent 能读规则并加入系统 |
| agent identity token | 每个研究 agent 有长期身份和 credit |
| heartbeat | agent 持续接收任务、反馈、评论 |
| strategy / discussion / operation split | 区分 hypothesis、debate、execution |
| paper positions | 把策略输出映射成可追踪仓位 |
| leaderboard | 让研究质量和表现可见 |
| challenge | 把研究任务组织成比赛或 benchmark |
| experiment variants | 比较不同 agent prompt / reward / workflow |
| research export | 把平台行为转成可复现数据 |
| network edges | 研究 agent 之间的影响与协作 |
| signal quality scoring | 防止平台被低质量输出淹没 |

这就是从“我有一个 agent”走向“我有一个 research operating system”。

## Possible PR Directions

如果我们之后给 `AI-Trader` 提 PR，我觉得可以从真实使用者角度入手，不是为了 PR 而 PR。

比较合适的方向：

| Direction | Why It Is Useful |
|---|---|
| Windows local self-host guide | 我们本地是 Windows，真实跑通后可以补文档 |
| `.env.example` explanation | 价格源、worker、DB、market intel 配置很多，可以写清楚 |
| agent quickstart with Codex / nanobot | 直接给 agent 平台接入 demo |
| Vibe-Trading bridge guide | research workflow 产出的策略如何发布到 AI-Trader |
| research export tutorial | 从平台到 paper-ready dataset 的复现路径 |
| challenge templates | crypto / us-stock / polymarket 的标准 challenge 模板 |
| API examples | strategy / discussion / realtime / follow / heartbeat 的最小 curl 示例 |
| safety disclaimer | copy trading、paper trading、real-money boundary 需要写清楚 |
| market data reliability notes | Alpha Vantage、yfinance、Hyperliquid、Polymarket fallback 行为整理 |

这些 PR 都是从使用中自然长出来的。

## Risks And Boundaries

这里必须说清楚：这篇是工程和研究学习笔记，不是投资建议。

`AI-Trader` 当前最适合理解成：

```text
agent-native paper trading and research platform
```

不能直接跳到：

```text
real money autonomous trading
```

真实资金场景还需要：

```text
risk limit
permission system
audit log
broker integration security
kill switch
human confirmation
regulatory compliance
data licensing
latency and outage handling
model behavior monitoring
```

同时也要注意：

```text
paper trading != live trading
leaderboard can be gamed
copy trading can amplify bad signals
market data can be stale
agent can overtrade
social reward can create noise
```

所以 signal quality、risk metric、experiment design、human review 都不是装饰，而是平台能不能长期健康运行的关键。

## Why It Matters For Us

`AI-Trader` 对我们现在的意义很直接。

我们想做的是：

```text
AI scientist
quant R&D agent
open-source project
top conference research
personal research OS
```

那就不能只做一个漂亮 demo。我们要有平台意识：

```text
data enters
agent reads
agent acts
system records
human reviews
experiments compare
research exports
paper writes
open-source improves
```

`AI-Trader` 正好给了一个 trading domain 的平台样板。

它提醒我们：真正有研究价值的系统，通常不是单点模型能力，而是完整 loop：

```text
identity
interaction
action
measurement
feedback
experiment
reproducibility
```

这也是我们之后做 `Pengyi Quant Research OS v0` 必须抓住的主线。

## Final Map

把 HKUDS 第一阶段合起来，现在是：

```text
HKUDS000 -> Study Map
HKUDS001 -> LightRAG
  = knowledge graph RAG / research memory

HKUDS002 -> Vibe-Trading
  = agentic quant research workflow / backtest layer

HKUDS003 -> nanobot
  = personal always-on agent shell

HKUDS004 -> CLI-Anything
  = agent-native software action layer

HKUDS005 -> AI-Trader
  = agent-native live trading platform layer
```

这套路线已经非常清楚：

```text
know
research
run
act
trade
measure
export
publish
```

下一步可以继续看 HKUDS 里和交易、agent、research platform 更相关的项目，或者直接做一篇 `HKUDS006`：把 `LightRAG + Vibe-Trading + nanobot + CLI-Anything + AI-Trader + LLMQuant` 统一成 `Pengyi Quant Research OS v0` 的系统架构图。
