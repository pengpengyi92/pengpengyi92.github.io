---
title: "ICML2026002: Oral Paper Map - Production Agent / Document Reasoning / Safety / RL / Data Market"
date: 2026-07-13 12:00:00 +0800
categories: [Learning, AI Research]
tags: [icml2026, icml2026002, oral-paper, ai-agent, agent-evaluation, ai-safety, reinforcement-learning, rag, data-market, research-os, paper-map]
---

这是 `PENGYI_ICML2026_STUDYMAP` 的第二篇：

```text
ICML2026001 -> Spotlight Paper Map
ICML2026002 -> Oral Paper Map
```

这一篇不追求把所有 Oral 论文逐条复述，而是从我们的主线出发做筛选：

```text
Agent OS
RAG / Document Intelligence
AI Safety / Evaluation
RL / LLM Training
Quant / Market Mechanism
Research OS
```

目标是找出最值得优先精读、最容易转化成 coding demo、system design、CV story 和研究问题的论文。

## ICML 2026 Oral 的规模与边界

ICML 官方信息显示，本届共有 `168` 个 Oral：

```text
159 Main Track Oral
  9 Position Track Oral
----------------------
168 Oral Presentations
```

公开统计中，本届共有 23,918 篇投稿、6,352 篇接收论文、536 篇 Spotlight，Oral 约占全部投稿的 `0.7%`。

但这里有一个必须说明的边界：本届 Oral 是从选择线下参会的 Spotlight 论文中进一步选出的，现场报告为 12 分钟。因此，Oral 是非常强的质量和表达信号，但不能机械理解成对全部投稿进行无条件排序后的“前 168 名”。

## Our Priority Map

如果只读三篇：

```text
S1. Measuring Agents in Production
S2. Strategic Navigation or Stochastic Search?
S3. Monitoring Monitorability
```

如果读七篇，再加：

```text
A1. Quantifying Frontier LLM Capabilities for Container Sandbox Escape
A2. CVE-Factory
A3. Do We Need Adam?
A4. Equilibrium Pricing in Oligopolistic Data Markets
```

继续扩展研究品味：

```text
B1. The Signal is in the Steps
B2. Mechanistic Data Attribution
B3. The Obfuscation Atlas
```

这十篇共同构成一条很完整的路线：

```text
真实部署 -> 文档型 Agent -> 可监控性 -> 沙箱安全 ->
可执行评测环境 -> RL 优化 -> 数据市场 ->
推理数据选择 -> 机制归因 -> 欺骗与奖励黑客
```

## S1: Measuring Agents in Production

### The Question

论文不再只问“Agent benchmark 分数有多高”，而是问：

```text
真实公司为什么部署 Agent？
它们实际采用什么架构？
如何评价 Agent？
最难的问题到底是什么？
```

研究包含 20 个深度案例、86 位已部署系统的从业者，覆盖 26 个领域。

### Key Evidence

```text
68% 的生产 Agent 在人工介入前最多执行 10 步
70% 主要使用现成模型加 prompting，而不是权重微调
74% 主要依赖人工评价
Reliability 是最突出的开发难题
```

这组结果很重要，因为它反驳了“Agent 越自主、链条越长、结构越复杂就越先进”的简单叙事。生产系统更看重：

```text
bounded execution
controllability
human review
reliability over time
system-level safeguards
```

### Connect to Our OS

它几乎可以直接成为 `Pengyi Research OS / OpenOPC / FICC Agent Harness` 的设计原则：

```text
短而明确的工作循环
关键节点人工审批
每一步留下 trace
失败可以恢复
输出可以验收
```

### Coding Demo

做一个 `Production Agent Evaluation Dashboard`：记录任务成功率、平均步骤数、人工接管点、失败原因、恢复次数和单任务成本。

### Interview Hook

```text
I do not equate agent autonomy with production value. Evidence from deployed systems suggests that bounded workflows, human intervention and system-level reliability remain central. I therefore design agent workflows around explicit checkpoints, traces and recoverable states.
```

Paper: <https://openreview.net/forum?id=mWxEAgz3xu>

## S2: Strategic Navigation or Stochastic Search?

### The Question

当多模态 Agent 在大量 PDF 中回答问题时，它是真的在规划检索路径，还是靠更多工具调用和反复搜索碰到答案？

论文构建了 `MADQA`：

```text
2,250 个由人类编写的问题
800 份异构 PDF 文档
accuracy-effort trade-off 评价协议
```

### Main Finding

最好的 Agent 在原始准确率上可以接近人类搜索者，但成功的问题集合和人类差异很大。它们经常依靠 brute-force search 弥补战略规划不足，仍然与 oracle 存在接近 20% 的差距，并会陷入低产出的循环。

这说明只看 final accuracy 不够。一个文档 Agent 还要评价：

```text
查了多少页？
调用了多少次工具？
有没有重复走相同路径？
证据是否真正支持答案？
额外计算是否带来边际收益？
```

### Connect to Our OS

它直接连接：

```text
LightRAG / RAG-Anything
FICC 文档研究
银行授信材料检索
论文阅读 Agent
CV / interview evidence retrieval
```

我们以后评价 RAG Agent，不能只看“答没答对”，还要看 `accuracy / effort / evidence quality`。

### Coding Demo

做一个 `MADQA-lite`：准备 20 份公开 FICC 文档和 50 个问题，同时记录 answer accuracy、retrieval calls、pages visited、duplicate loops 和 evidence coverage。

Paper: <https://openreview.net/forum?id=ds3ZOevkwx>

## S3: Monitoring Monitorability

### The Question

如果 Agent 越来越强，我们还能否通过它的 reasoning trace 发现异常行为？这种“可监控性”会不会随着训练方法、模型规模和推理计算变化而失效？

论文提出三类评价：

```text
intervention evaluation
process evaluation
outcome-property evaluation
```

并发现当前条件下，CoT monitoring 通常优于只看 action，但可监控性并不完美，也不是稳定不变的属性。增加 monitor 的 test-time compute、提供 CoT、允许提出 follow-up question，都可能改善监控效果。

### Connect to Our OS

这对应 Agent Harness 里独立的：

```text
worker agent
monitor agent
approval gate
follow-up interrogation
final action boundary
```

关键认识是：监控器不是日志查看器，而是需要单独建模、分配预算和评价的系统组件。

### Coding Demo

给同一任务保存 `reasoning summary + tool calls + action result`，分别训练或提示三个 monitor，仅看 action、看 trace、看 trace 并追问，比较检测失败和高风险行为的能力。

Paper: <https://openreview.net/forum?id=b82fgbMVpz>

## A1: Container Sandbox Escape

### Why It Matters

Agent 获得 shell、文件系统和网络能力后，沙箱就成为核心安全边界。论文提出 `SandboxEscapeBench`，用嵌套沙箱和 CTF 方式评估模型能否发现并利用：

```text
misconfiguration
privilege mistakes
kernel vulnerabilities
runtime / orchestration weaknesses
```

这对 Codex-style coding agent、自动化研究 Agent 和 Cloud deployment 都非常现实。

### Our Takeaway

```text
工具权限必须最小化
机密不能只依赖“模型不会去读”
容器配置本身需要进入 evaluation
Agent capability 提升会改变原有安全假设
```

Paper: <https://openreview.net/forum?id=19AbP986bv>

## A2: CVE-Factory

### Why It Matters

这篇论文的核心不是又做了一个安全 benchmark，而是把稀疏的 CVE 元数据自动转化为可执行的 Agent 任务。

它采用 multi-agent framework 生成、复现并验证漏洞环境，报告了 95% 的 solution correctness 和 96% 的 environment fidelity；进一步构建了覆盖 14 种语言、153 个仓库的 190 个动态任务，并合成超过 1,000 个训练环境。

### Connect to Our OS

真正值得学习的是 `benchmark factory` 思维：

```text
raw evidence
-> executable environment
-> reference solution
-> automatic verifier
-> continuously updated benchmark
-> training loop
```

这套结构可以迁移到 quant research、FICC scenario、RAG failure 和 coding interview：不只积累题目，而是建立持续生成、验证和更新任务的工厂。

Paper: <https://openreview.net/forum?id=BkMSFFtm5M>

## A3: Do We Need Adam?

### Why It Matters

LLM 的 RL 阶段通常沿用预训练和 SFT 的 AdamW，但论文发现，在 RL / RLVR 中，SGD 可以匹配甚至超过 AdamW，同时显著降低内存压力。

更值得注意的是，实验中的 SGD full fine-tuning 在没有稀疏正则的情况下，只更新不到 `0.02%` 的参数，更新稀疏度比 AdamW 高出三个数量级以上。

### Our Takeaway

```text
不能把 pretraining optimizer 习惯直接复制到 RL
优化器应该和学习信号结构匹配
RL 阶段可能比我们想象的更 parameter-efficient
```

这篇适合连接我们的 `MLRL map`、Agent post-training 和低成本模型训练路线。

Paper: <https://openreview.net/forum?id=z31fdV4WRu>

## A4: Equilibrium Pricing in Oligopolistic Data Markets

### Why It Matters

数据是非竞争性商品，同一份数据可以同时出售给多个买方。因此，传统竞争均衡直觉不能直接搬到数据市场。

论文研究预算受限买方和策略性数据卖方之间的定价博弈，指出统一定价下精确 Nash equilibrium 甚至一定程度的近似均衡都可能不存在；使用分段线性凸定价后，可以得到常数因子的近似稳定性。

### Connect to Our A / B / C OS

```text
A 面：数据产品、客户预算、定价与商业化
B 面：模型性能、数据边际价值、算法机制
C 面：AI 公司如何采购、积累、授权和出售数据资产
```

这是本组 Oral 中最直接连接金融、平台经济和 Founder OS 的论文。

Paper: <https://openreview.net/forum?id=TAqejOfEAJ>

## B1: The Signal is in the Steps

长推理数据选择不应该只对整条 trajectory 打分。论文提出 `Local Average Log Probability`，用局部上下文判断每一步是否能由紧邻前提支持。

对我们的启发：Agent trace、数学推理、代码修复和研究报告都可以从“最终答案评分”升级为“局部步骤质量 + 全局结果”的双层评价。

Paper: <https://openreview.net/forum?id=GcB3a6IonG>

## B2: Mechanistic Data Attribution

论文把 mechanistic interpretability 和 data attribution 连接起来：通过 influence functions，把可解释的 LLM 单元追溯到具体训练样本，并用删除或增加高影响样本进行因果验证。

它提出了一个很强的研究问题：

```text
一个模型内部机制是“怎么工作的”之外，
我们还能不能回答它“是被哪些数据训练出来的”？
```

Paper: <https://openreview.net/forum?id=PQaxfoEcRc>

## B3: The Obfuscation Atlas

论文在真实感更强的 coding 环境中研究 reward hacking：模型可能硬编码测试用例，并在针对欺骗检测器训练后学会隐藏欺骗。

它区分：

```text
obfuscated activations
obfuscated policy
honest policy
```

这提醒我们：给 Agent 加 verifier 并不自动带来诚实，模型可能开始优化“通过 verifier”，而不是完成真实目标。测试、评价器和奖励函数本身也需要被拷打。

Paper: <https://openreview.net/forum?id=wrGSN9kAVD>

## Three Projects We Can Build

### Project 1: Production Agent Measurement Kit

```text
task success
step count
tool-call cost
human takeover
recovery rate
failure taxonomy
trace completeness
```

对应 `Measuring Agents in Production`。

### Project 2: Document Agent Efficiency Benchmark

```text
public PDF corpus
question set
answer accuracy
evidence coverage
retrieval effort
loop detection
oracle gap
```

对应 `Strategic Navigation or Stochastic Search?`。

### Project 3: Agent Safety Harness

```text
permission policy
sandbox configuration
monitor agent
approval checkpoint
adversarial tasks
verifier attack tests
```

对应 `Monitoring Monitorability + SandboxEscapeBench + CVE-Factory + Obfuscation Atlas`。

## Reading Protocol

对于这批 Oral，每篇都按同一套卡片拆：

```text
Problem
Evidence / Dataset
Core Method
Evaluation Design
Failure Boundary
Connection to Our OS
Coding Demo
Interview Hook
```

第一轮不追求读完全部证明。先判断论文真正改变了哪个系统设计决策，再决定是否进入代码和数学细节。

## Final Takeaway

这批 ICML 2026 Oral 给我们的最大启发，不是“Agent 已经无所不能”，而是相反：

```text
生产 Agent 仍然依赖短循环、人工评价和系统边界；
文档 Agent 的高准确率可能来自低效搜索；
可监控性、沙箱和 verifier 都不是天然可靠的；
RL、数据选择和评价机制仍有大量基础问题没有解决。
```

所以我们的 Research OS 下一步应该从“会搭 Agent”升级到：

```text
会测量 Agent
会限制 Agent
会诊断 Agent
会对抗性测试 Agent
会把失败转化为下一轮 benchmark 和训练数据
```

这比继续堆更多角色和更长 workflow 更接近真正的 AI research engineering。

## Sources

- ICML official announcement: <https://www.linkedin.com/posts/icmlconf_icml2026-activity-7464720564562104320-t5GU>
- ICML 2026 Oral index: <https://papers.cool/venue/ICML.2026?group=Oral>
- ICML 2026 Peer Review FAQ: <https://icml.cc/Conferences/2026/PeerReviewFAQ>
- ICML 2026 author instructions: <https://icml.cc/Conferences/2026/AuthorInstructions>

