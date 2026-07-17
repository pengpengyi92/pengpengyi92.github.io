---
title: "ICML2026003: Quant Paper Map - Data / Signal / Risk / Portfolio / Market / Financial Agent"
date: 2026-07-13 13:00:00 +0800
categories: [Learning, AI Research, Quant Research]
tags: [icml2026, icml2026003, quant-paper, time-series, alpha-research, portfolio-optimization, risk-management, market-simulation, financial-agent, research-os, paper-map]
---

这是 `PENGYI_ICML2026_STUDYMAP` 的第三篇：

```text
ICML2026001 -> Spotlight Paper Map
ICML2026002 -> Oral Paper Map
ICML2026003 -> Quant Paper Map
```

这一次不按 Oral、Spotlight、Poster 的展示等级组织，而是从 Quant Research 的完整工作流出发，重新筛选 ICML 2026 全部论文。

```text
Data
-> Representation
-> Signal / Forecast
-> Risk & Uncertainty
-> Portfolio Decision
-> Market Simulation
-> Financial Agent
```

筛选标准是：

```text
Quant relevance
x Method transferability
x Coding demo feasibility
x Interview explainability
```

## What Quant Means Here

Quant 不等于“预测明天股票涨跌”。

一套可交付的量化研究系统至少包括：

```text
数据是否同步、完整和可交易？
标签是否与持有期和交易目标一致？
模型学到的是方向、风险还是市场状态？
预测误差如何传递到仓位和 P&L？
组合是否考虑稀疏性、换手和交易成本？
分布变化时决策是否仍然稳健？
回测是否尊重市场机制和执行边界？
Agent 是否会正确调用金融工具并留下证据？
```

因此，这张 map 同时保留两类论文：

```text
Direct Quant Papers
    标题、数据或实验直接针对金融市场。

Transferable ML Papers
    方法原本不只为金融设计，但能解决 quant pipeline 的关键问题。
```

我们不会把后者包装成金融成果，但会明确它能迁移到哪一个研究环节。

## Priority Map

如果只看六篇：

```text
S1. The Label Horizon Paradox
S2. Joint-Embedding Predictive Learning of Latent Market States
S3. Decision-focused Sparse Tangent Portfolio Optimization
S4. Loss-aware Distributionally Robust Optimization
S5. MarketSim
S6. Towards Professional-Grade Financial Agents
```

这六篇正好覆盖：

```text
label
-> representation
-> portfolio
-> risk
-> market
-> agent
```

第二阅读层：

```text
A1. ConFlux
A2. HELIX
A3. SDEVI for Irregular Time Series
A4. Online Conformal Prediction via Universal Portfolio Algorithms
A5. Sparse Portfolio Optimization with Transaction Cost
A6. Error Propagation in Dynamic Programming and American Option Pricing
```

研究扩展层：

```text
B1. Robust Causal Discovery in Real-World Time Series with Power-Laws
B2. Universal Redundancies in Time Series Foundation Models
B3. Latent Spherical Flow Policy for Combinatorial Actions
B4. Equilibrium Pricing in Oligopolistic Data Markets
B5. BizFinBench.v2
B6. Evolving Quantitative Reasoning through Self-Play
```

## S1: The Label Horizon Paradox

### Core Question

传统金融预测通常默认：

```text
如果最终要预测未来 20 天收益，
训练标签就应该直接使用未来 20 天收益。
```

这篇论文挑战了这个默认设定。它提出 `Label Horizon Paradox`：最优训练标签的 horizon 可能并不等于最终推断目标，而是在中间期限发生偏移。

原因来自动态的 signal-noise trade-off：

```text
horizon 太短 -> 信号尚未充分实现
horizon 太长 -> 噪声持续累积
```

论文使用 bi-level optimization，在一次训练中自动选择更合适的 proxy label。

### Why We Care

这和 WorldQuant Alpha research、期货 CTA、持有期设计直接相关：

```text
feature window
label horizon
rebalance frequency
position holding period
turnover
signal decay
```

这些变量不能彼此独立决定。

### Coding Demo

选择同一组特征，分别使用 `1D / 5D / 10D / 20D` forward return 训练，再统一评价目标持有期下的：

```text
IC
Rank IC
turnover
Sharpe
drawdown
cost-adjusted return
```

重点不是找出一个最漂亮的数字，而是画出 `training horizon -> downstream performance` 曲线。

### Interview Hook

```text
I treat label construction as part of the investment hypothesis rather than a fixed preprocessing choice. The optimal supervision horizon may differ from the final holding horizon because signal realization and noise accumulation operate at different rates.
```

Paper: <https://openreview.net/forum?id=G43CIfmmxh>

## S2: Joint-Embedding Predictive Learning of Latent Market States

### Core Design

这篇论文尝试用 JEPA 学习美股市场的 latent state：

```text
unordered per-asset daily features
-> permutation-invariant tokenizer
-> learned factor tokens
-> temporal masked prediction
-> compact market-state embedding
```

它不是直接预测涨跌，而是先学习“市场现在处于什么状态”。

### Main Findings

论文发现 embedding 与以下二阶市场结构关联较强：

```text
realized volatility
correlation concentration
effective factor dimensionality
```

但与市场方向关联较弱。它更容易预测逐渐恢复的动态，而不是突然发生的 stress onset；无文本监督的 latent regimes 还与新闻主题变化表现出显著对应。

### Why We Care

这是一个非常真实的结论：representation 不一定产生 directional alpha，但可能成为：

```text
risk regime feature
portfolio exposure controller
volatility state
correlation state
factor crowding indicator
```

### Coding Demo

用公开指数、行业 ETF、波动率和相关性数据做一个 `Market State Lab`：

```text
daily cross-sectional features
-> low-dimensional state embedding
-> clustering / state transition
-> volatility and correlation attribution
-> downstream allocation test
```

Paper: <https://openreview.net/forum?id=BZfkxSasd3>

## S3: Decision-focused Sparse Tangent Portfolio Optimization

### The Problem

普通 quant pipeline 是：

```text
predict expected returns
-> evaluate forecast accuracy
-> send predictions into optimizer
```

但预测误差小，不等于最终组合好。某些预测在 MSE 上更准，却可能产生更差的资产选择、风险暴露和 Sharpe。

论文采用 decision-focused learning，把预测、资产选择和组合再优化放进同一条可微分链路：

```text
predictive model
-> smooth top-k selection
-> DPP-compliant convex layer
-> tangent portfolio
-> downstream portfolio objective
```

其中 smooth top-k 用来保持精确的 sum-to-k 稀疏预算。

### Why We Care

它对应一个核心 quant 原则：

```text
不要只优化 prediction metric，
要优化最终 decision quality。
```

### Coding Demo

比较两套流程：

```text
Model A: minimize forecast MSE
Model B: optimize differentiable portfolio objective
```

统一比较 OOS Sharpe、turnover、最大回撤、持仓数量和稳定性。

Paper: <https://openreview.net/forum?id=KV7XHF0IbK>

## S4: Loss-aware Distributionally Robust Optimization

### The Problem

传统 optimal-transport DRO 通常分成两步：

```text
先定义 ambiguity set
再在该集合上做 robust optimization
```

如果 uncertainty set 与最终任务无关，决策可能过度保守。

这篇论文让 loss function 参与 ambiguity set 的学习，通过 bilevel optimization 和 hypergradient，端到端学习 decision-focused uncertainty set。

### Quant Connection

金融里的风险不只是历史波动率。真正困难的是：

```text
训练分布和未来分布不同
相关结构会变化
尾部样本有限
估计误差进入 optimizer 后会被放大
```

论文包含 portfolio optimization 实验，因此既是可迁移方法，也有直接 quant evidence。

### Coding Demo

比较：

```text
mean-variance portfolio
fixed-radius OT-DRO
loss-aware OT-DRO
```

并在 regime shift、volatility shock 和 correlation shock 下评价收益与保守程度。

Paper: <https://openreview.net/forum?id=K1EPPO9t2c>

## S5: MarketSim

### Core Design

MarketSim 是一个大规模生成式 Agent 股票市场模拟框架：

```text
15,000+ heterogeneous participants
NASDAQ-style continuous double auction
nanosecond-resolution execution layer
12,000+ news / policy / earnings documents
hierarchical reasoning and execution
```

它把高层策略推理与高频执行拆开，使 LLM Agent 不需要直接承担每一个微观撮合动作。

评价覆盖 8 个 GICS 行业、3 类真实场景、5 个市场 stylized facts 和 5 个价格统计指标。

### Why We Care

它连接我们的两条既有项目线：

```text
Futures Quant Project
Crypto HFT Market Making Project
```

前者关注 signal、factor 和 portfolio，后者关注 order book、inventory、latency 和 fill realism。MarketSim 位于两者之间：研究大量异构参与者如何共同生成市场状态和价格路径。

### Research Boundary

生成式市场能用于机制实验和压力测试，但不应该因为复现了若干 stylized facts 就被视为真实市场替代品。需要继续拷打：

```text
calibration
agent homogeneity
information leakage
matching-engine realism
market impact
counterfactual validity
```

Paper: <https://openreview.net/forum?id=EzpJxPDqXB>

## S6: Towards Professional-Grade Financial Agents

### Core Components

论文提出：

```text
ProFinR       -> 528 个专家设计金融任务
Tool Universe -> 53 个领域工具，13 个类别
ProFinAgents  -> DAG workflow + Case-Based Memory
```

DAG 用于并行组织工具执行，Case-Based Memory 用历史案例减少重复推理失败。

### Why We Care

它直接连接：

```text
LLMQuant
FICC Research OS
Agent Harness
financial tool registry
case memory
evaluation rubric
```

但这里最重要的不是复制一个“金融 Agent”，而是研究：

```text
任务如何定义？
工具如何注册和验证？
DAG 如何生成和执行？
case memory 何时帮助、何时污染？
最终答案如何由证据和计算共同支持？
```

### Coding Demo

构建一个小型 `FICC Professional Agent`：

```text
yield curve calculator
bond price / YTM
duration / convexity
scenario P&L
evidence retrieval
structured report
human approval
```

Paper: <https://openreview.net/forum?id=x6nNQBumUQ>

## Data and Forecast Layer

### ConFlux

ConFlux 面向动态、异步、相互作用的多变量时间序列。它先重排变量以降低 cross-variable entanglement，再将相邻变量聚合为 compact patches，用 ViT-style architecture 统一处理。

Quant 连接：跨资产、跨期限和跨宏观变量预测，但必须验证“变量重排”是否尊重金融语义，而不只是 benchmark 最优。

Paper: <https://openreview.net/forum?id=qugRvYjaFx>

### HELIX

HELIX 为每个变量分配持久的 learnable feature identity，再通过 temporal-feature attention 学习跨变量关系。

Quant 连接：金融数据经常缺失且异步，feature identity 可以避免模型在每一层重新发现变量语义。

Paper: <https://openreview.net/forum?id=FN20iuPnEU>

### SDEVI

SDEVI 针对 sparse、asynchronous 的 irregular time series，使用 SDE-induced continuous-discrete variational inference，在离散观测和连续过程之间建立一致性。

Quant 连接：不同资产、宏观指标、基本面和事件数据不在同一时钟上，连续时间建模可能比粗暴 forward fill 更自然。

Paper: <https://openreview.net/forum?id=IXdjkxDXI0>

### Robust Causal Discovery with Power-Laws

论文从真实时间序列常见的 power-law frequency spectra 出发，提取频谱特征以增强真实因果信号、降低噪声导致的伪因果。

Quant 连接：宏观、行业和资产之间的 lead-lag 结构；但 causal discovery 结果不能直接当作可交易因果关系，仍需经济机制和干预假设。

Paper: <https://openreview.net/forum?id=7i8d203tky>

### Universal Redundancies in Time Series Foundation Models

论文用 ablation 和 direct logit attribution 分析 TSFM，发现多个领先模型对整层消融具有鲁棒性，并定位 motif parroting、seasonality bias 等退化行为相关的 heads。

Quant 连接：foundation model 参数更多不等于有效信息更多；需要检查模型是在理解 regime，还是重复上下文模式和季节性。

Paper: <https://openreview.net/forum?id=DyA4KHj1wy>

## Risk and Decision Layer

### Online Conformal Prediction via Universal Portfolio Algorithms

论文把 online conformal prediction 转化为 two-asset portfolio selection 问题，提出 parameter-free 的 UP-OCP，为任意、甚至 adversarial data stream 提供长期 coverage 控制。

Quant 连接：在线风险区间、volatility regime 和动态模型置信度。它并不是直接的交易策略，但能训练我们从 point forecast 转向 calibrated interval。

Paper: <https://openreview.net/forum?id=EKmiOtjZjI>

### Sparse Portfolio Optimization with Transaction Cost

论文同时考虑 K-sparse portfolio 和 transaction cost，将 NP-hard 稀疏问题改写为 nonsmooth difference-of-convex optimization，并结合 proximal subgradient 与 ADMM。

Quant 连接非常直接：

```text
不是只找理论最优权重，
而是控制持仓数量、换手和真实交易成本。
```

Paper: <https://openreview.net/forum?id=yZAo4TPqhE>

### Error Propagation and American Option Pricing

论文研究离散时间 stochastic optimal control，使用 RKHS / KRR 与 Monte Carlo 估计 continuation value，并分析误差如何从到期日向初始时点反向传播，最后应用于 American option pricing。

它代表传统 mathematical quant 主线：dynamic programming、conditional expectation、Monte Carlo 和 pricing error，而不是机器学习预测 Alpha。

Paper: <https://openreview.net/forum?id=FB5LYFzu33>

### Latent Spherical Flow Policy

LSFlow 在连续 latent space 中学习随机策略，再由 combinatorial solver 映射为满足约束的合法动作；value network 直接在 latent space 训练，以减少反复调用 solver。

Quant 连接：带约束的资产选择、交易篮子、对冲工具组合和执行路径。但需要额外加入成本、市场冲击和风险预算才可能成为金融系统。

Paper: <https://openreview.net/forum?id=07wwDFdi3k>

## Market and Financial Agent Layer

### Equilibrium Pricing in Oligopolistic Data Markets

数据是 non-rival good，同一数据可以同时卖给多个买方。论文指出统一定价下精确 Nash equilibrium 可能不存在，并研究分段线性凸定价带来的近似稳定性。

它连接 A / B / C 三面：数据商业化、模型性能和 AI 公司资产定价。

Paper: <https://openreview.net/forum?id=TAqejOfEAJ>

### BizFinBench.v2

BizFinBench.v2 使用中美股票市场真实用户 query-response 数据，包含 28,860 个问题、8 个离线任务和 2 个在线任务。论文强调现有模型与真实业务要求之间仍有明显差距。

它最值得学习的不是模型排行榜，而是：如何从真实用户任务构建 bilingual offline / online financial evaluation。

Paper: <https://openreview.net/forum?id=8cQGYH64R2>

### Evolving Quantitative Reasoning through Self-Play

这篇论文把 semantic planning 和 numerical computation 解耦：LLM 负责规划、分析和解释，结构化工具负责计算和统计推断；Agent 在 digital twin market 中根据反馈持续修改工具选择和构造。

它和我们的自进化 Research OS 很接近，但需要重点验证：

```text
self-evolution 是否真正提高 OOS reliability？
工具是否被正确构造和调用？
feedback 是否会放大 simulator bias？
```

Paper: <https://openreview.net/forum?id=j4SxlIyvRg>

## One Unified Quant OS

把这些论文放在一起，可以得到一套完整系统设计：

```text
Raw Market Data
    |
    v
Data Quality / Irregular Sampling
    HELIX / SDEVI
    |
    v
Representation & Regime
    JEPA Market State / ConFlux / TSFM Diagnosis
    |
    v
Label & Signal
    Label Horizon Paradox / Causal Discovery
    |
    v
Uncertainty & Robustness
    Online Conformal / Loss-aware OT-DRO
    |
    v
Portfolio & Decision
    Decision-focused Portfolio / Sparse TCO / LSFlow
    |
    v
Market & Pricing
    MarketSim / American Option / Data Market Equilibrium
    |
    v
Financial Agent
    ProFinAgents / BizFinBench / Quant Self-Play
    |
    v
Human Review + Evidence + Deployment Boundary
```

## Three Projects Worth Building

### Project 1: Label-to-Portfolio Research Lab

```text
multiple label horizons
same features and universe
forecast-focused baseline
decision-focused model
transaction-cost-aware portfolio
walk-forward evaluation
```

连接 `Label Horizon + Decision-focused Portfolio + Sparse TCO`。

### Project 2: Latent Market State and Risk Lab

```text
cross-sectional factor tokens
market-state embedding
volatility / correlation attribution
online prediction interval
regime-conditioned allocation
stress transition analysis
```

连接 `JEPA Market State + ConFlux + Online Conformal + OT-DRO`。

### Project 3: FICC Professional Agent Harness

```text
financial tool registry
DAG task planning
case memory
calculation verification
evidence citation
scenario report
human approval checkpoint
failure benchmark
```

连接 `ProFinAgents + BizFinBench + Quant Self-Play`，并落到我们现有的 Rates Bond Quant 项目。

## CV and Interview Boundary

在真正复现之前，简历可以诚实地表达为：

```text
Studied ICML 2026 methods across financial forecasting,
decision-focused portfolio optimization, distributionally robust risk,
market simulation and tool-using financial agents,
and mapped them into a reproducible quant research workflow.
```

完成 coding demo 后，才升级为：

```text
Implemented and compared...
Built a reproducible pipeline...
Evaluated under walk-forward and transaction-cost settings...
```

没有真实复现和评价，就不使用 `implemented`、`outperformed`、`production-grade` 或 `deployed`。

## Recommended Deep-Dive Sequence

```text
ICML2026004 -> Label Horizon Paradox
ICML2026005 -> Decision-focused Portfolio Optimization
ICML2026006 -> Latent Market State / Regime Representation
ICML2026007 -> MarketSim and Agent-Based Market
ICML2026008 -> Professional Financial Agent
```

这个顺序从最容易做出小型实证的 label engineering 开始，逐步进入 portfolio、representation、market simulation 和 agent system。

## Final Takeaway

ICML 2026 对 Quant 最有价值的内容，不是一篇可以直接拿去交易的“神奇模型”，而是一组能重构研究流程的方法：

```text
标签不再是固定答案；
representation 不只服务涨跌预测；
forecast metric 不等于 portfolio quality；
风险集合需要与决策目标对齐；
market simulator 必须接受机制和真实性检验；
financial agent 必须有工具、证据、评价和人工边界。
```

真正的 Quant Map 不是 Paper List。

它应该把论文转换成：

```text
research question
-> executable experiment
-> robust evaluation
-> explainable result
-> reusable research component
```

## Sources

- ICML 2026 official conference: <https://icml.cc/Conferences/2026>
- ICML 2026 Call for Papers: <https://icml.cc/Conferences/2026/CallForPapers>
- ICML 2026 official paper page: <https://icml.cc/virtual/2026/papers.html>
- ICML 2026 OpenReview venue: <https://openreview.net/group?id=ICML.cc/2026/Conference>
- ICML 2026 Paper Explorer based on official virtual data: <https://gisbi-kim.github.io/icml2026-explorer/output/icml2026_explorer.html>
