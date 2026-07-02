---
title: "AI_CONF000: AI 顶会地图 - ML / NLP / CV / Agent / RAG / Data Mining / Robotics / Systems"
date: 2026-07-02 00:00:00 +0800
categories: [Learning, AI Research]
tags: [ai-conf000, ai-conference, top-conference, ml, nlp, cv, agent, rag, data-mining, robotics, systems, ai-scientist, research-os]
---

这是 `PENGYI_AI_CONF_MAP` 的第一篇：

```text
AI_CONF000 -> AI 顶会地图
```

我们要成为 AI scientist，不能只知道“顶会很重要”。

我们需要知道：

```text
哪些是 AI / ML / NLP / CV / Agent / RAG / Data Mining / Robotics / Systems 的主战场？
每个会到底看什么？
什么类型的工作适合投哪里？
我们的 Research OS / Quant OS / Agent Harness 应该瞄准哪些 venue？
```

这篇就是一张总地图。

## 一句话总览

AI 顶会不是一个单点。

它更像一个研究生态：

```text
Core ML:
  NeurIPS / ICML / ICLR

General AI:
  AAAI / IJCAI

NLP / LLM:
  ACL / EMNLP / NAACL / COLING

Computer Vision / Multimodal:
  CVPR / ICCV / ECCV / ACM MM

Data Mining / Web / IR / Rec:
  KDD / TheWebConf / WSDM / SIGIR / CIKM / RecSys

Agents / Multi-agent / Robotics:
  AAMAS / CoRL / RSS / ICRA / IROS

Theory / Statistics / Uncertainty:
  AISTATS / UAI / COLT

Systems / AI Infrastructure:
  MLSys / OSDI / SOSP / NSDI / EuroSys / SIGMOD / VLDB / ICDE

Human-AI Interaction:
  CHI / UIST / CSCW

Responsible AI:
  FAccT / AIES
```

对我们来说，最关键的是：

```text
不是“哪个会最顶”。
而是“我们的 work 属于哪个 research contribution type”。
```

一个 AI agent / RAG / Quant Research OS 项目，可能投：

```text
NeurIPS / ICML / ICLR:
  如果它有新方法、新训练、新 evaluation、新 benchmark

ACL / EMNLP / NAACL:
  如果核心是语言、RAG、LLM reasoning、tool use、agent dialogue

KDD / SIGIR / TheWebConf / RecSys:
  如果核心是 retrieval、recommendation、knowledge discovery、finance / web-scale data

AAMAS:
  如果核心是 autonomous agents / multi-agent systems

MLSys / Systems venues:
  如果核心是 inference system、agent runtime、serving、benchmark infra

Workshops:
  如果是早期 idea、domain-specific demo、AI for finance / AI scientist / agent evaluation
```

## 顶会不是奖项

先区分两个概念。

```text
顶会:
  研究成果发表场域。

奖项:
  对长期贡献或特定成果的认可。
```

CS / AI 领域更接近“诺贝尔”的奖项包括：

```text
ACM A.M. Turing Award
ACM Prize in Computing
IJCAI Computers and Thought Award
AAAI Fellows / AAAI Classic Paper / Test of Time style awards
NeurIPS / ICML / ICLR / ACL / CVPR best paper and test-of-time awards
```

但我们现阶段最实际的路线不是直接想奖项。

更实际的是：

```text
先进入 workshop / benchmark / open-source contribution
再进入 main conference paper
再形成长期 research agenda
```

顶会是训练场。

奖项是长期结果。

## Core ML 三巨头

## NeurIPS

定位：

```text
Machine learning / AI / statistics / neuroscience-inspired learning / optimization / large-scale AI
```

它是 AI 研究最核心的综合 venue 之一。

适合的工作：

```text
new ML method
representation learning
optimization
reinforcement learning
foundation models
evaluation and benchmark
AI safety / alignment
agent learning
scientific ML
AI for domain science
```

我们能学什么：

```text
如何把一个 idea 做成严肃 method paper
如何设计 benchmark
如何做 ablation
如何写 limitation
如何把系统结果压成 scientific contribution
```

适合我们的方向：

```text
coding agent evaluation
research agent benchmark
RAG memory evaluation
Quant Research OS benchmark
agent harness reliability
LLM tool-use failure diagnosis
```

如果要投 NeurIPS，不能只是“我搭了个系统”。

需要回答：

```text
新问题是什么？
新方法是什么？
为什么现有方法不够？
评估是否严格？
是否有可复现 benchmark？
是否有对比和 ablation？
```

## ICML

定位：

```text
Machine learning core methods and theory-practice bridge.
```

ICML 很适合：

```text
learning algorithms
optimization
generalization
RL
probabilistic learning
representation learning
efficient training
agent training
foundation model methods
```

对我们来说，ICML 更偏：

```text
能不能把 agent / RAG / quant research 问题转成 ML problem？
```

例如：

```text
agent trajectory learning
preference optimization for research agent
reward model for coding agent
failure-to-skill learning
tool-use policy learning
quant hypothesis ranking model
```

如果我们的 work 只是“系统整合”，ICML 不一定合适。

如果能提出：

```text
新的训练目标
新的 learning framework
新的 evaluation metric
新的 theoretical or empirical insight
```

ICML 就可能合适。

## ICLR

定位：

```text
Representation learning / deep learning / foundation models.
```

ICLR 的气质非常适合：

```text
LLM
representation
multimodal alignment
graph-language model
reasoning
RAG representation
agent representation
model architecture
training recipe
```

我们最近看的很多 HKUDS 项目，都和 ICLR 气质接近：

```text
LightRAG:
  knowledge representation and graph memory

GraphGPT / HiGPT:
  graph-language representation

UrbanGPT:
  spatio-temporal tensor -> LLM representation

LightReasoner:
  reasoning distillation

SepLLM:
  long-context representation and memory compression
```

如果我们做：

```text
Research OS memory representation
QuantGPT market tensor representation
Graph RAG representation evaluation
Agent skill representation
```

ICLR 是非常自然的目标。

## General AI

## AAAI

定位：

```text
Broad artificial intelligence.
```

AAAI 覆盖范围很大：

```text
planning
reasoning
search
knowledge representation
multi-agent systems
machine learning
NLP
vision
AI applications
AI ethics
```

AAAI 对我们有一个好处：

```text
它不像 NeurIPS / ICML / ICLR 那样只看 ML core。
它更容易容纳 AI system + reasoning + application + agent workflow 的综合工作。
```

适合我们的方向：

```text
Research agent planning
R&D agent for quant research
human PM review agent
agent workflow verification
AI scientist task decomposition
knowledge-grounded decision agent
```

如果我们要做一个比较完整的 `Research OS` paper，AAAI 可能比纯 ML venue 更自然。

## IJCAI

定位：

```text
International Joint Conference on Artificial Intelligence.
```

IJCAI 也是广义 AI 顶会。

适合：

```text
reasoning
planning
multi-agent
knowledge representation
decision making
AI applications
```

如果我们的 work 是：

```text
RAG + planning + tools + evaluation + human review
```

IJCAI/AAAI 都值得关注。

## NLP / LLM

## ACL

定位：

```text
Computational linguistics / NLP / LLM.
```

ACL 是 NLP 主会。

适合：

```text
language model reasoning
RAG
retrieval-augmented generation
tool use
agent dialogue
long-context
evaluation
dataset / benchmark
alignment
multilingual NLP
```

对我们来说，ACL 很重要。

因为我们做的很多东西都从语言出发：

```text
paper -> strategy
research question -> plan
docs -> knowledge graph
chat -> agent action
RAG -> grounded answer
```

如果核心贡献是：

```text
LLM 如何读材料
LLM 如何检索证据
LLM 如何生成研究计划
LLM 如何用工具
LLM 如何被评估
```

ACL 是很自然的 venue。

## EMNLP

定位：

```text
Empirical NLP and language technology.
```

EMNLP 通常很重 empirical evaluation。

适合：

```text
LLM evaluation
RAG benchmark
prompting / decoding / tool-use behavior
information extraction
question answering
document understanding
agent text environments
```

我们的 RAG / Research OS work，如果有扎实数据和实验，EMNLP 很合适。

例如：

```text
RAG for research paper understanding
financial document RAG benchmark
agent report hallucination diagnosis
tool-use trajectory evaluation
research memo generation evaluation
```

## NAACL / COLING

定位：

```text
NLP family venues.
```

NAACL 是 ACL family 的区域性强会，COLING 是计算语言学老牌会议。

适合：

```text
NLP methods
LLM evaluation
information extraction
dialogue
retrieval
language resources
```

如果我们后续做 NLP/RAG 方向，ACL/EMNLP/NAACL/COLING 是一组一起看的 venue。

## Computer Vision / Multimodal

## CVPR

定位：

```text
Computer vision and pattern recognition.
```

CVPR 是 CV 顶会。

适合：

```text
image understanding
video understanding
3D vision
vision-language models
multimodal generation
visual reasoning
segmentation / detection / tracking
```

对我们来说，CVPR 不是第一主线，但很有用。

尤其是：

```text
VideoRAG
VideoAgent
ViMax
multimodal RAG
paper figures / charts understanding
document layout understanding
```

这些都和视觉、多模态有关。

## ICCV / ECCV

定位：

```text
ICCV:
  International CV flagship, odd years.

ECCV:
  European CV flagship, even years.
```

它们和 CVPR 一起构成 CV 三大主会。

如果我们以后做：

```text
video-based research memory
multimodal document agent
chart / table / figure understanding
visual research assistant
```

CVPR / ICCV / ECCV 都可以看。

## ACM MM

定位：

```text
Multimedia.
```

ACM MM 适合：

```text
video
audio
image-text
multimodal retrieval
multimodal generation
media understanding
```

对我们的 VideoRAG / ViMax / multimodal Research OS 很相关。

## Data Mining / Web / IR / Recommendation

## KDD

定位：

```text
Knowledge discovery and data mining.
```

KDD 对我们非常重要。

因为 quant / finance / recommendation / graph / large-scale behavior data 都和 KDD 很接近。

适合：

```text
data mining
graph mining
recommendation
forecasting
time series
knowledge discovery
applied ML
AI for business / finance / web-scale systems
datasets and benchmarks
```

如果我们做：

```text
Quant Research OS benchmark
financial RAG evaluation
factor discovery workflow
research idea recommendation
agent-generated hypothesis ranking
market event forecast ledger
```

KDD 是非常值得关注的。

KDD 的 applied data science track 也很适合系统和应用型贡献。

## TheWebConf / WWW

定位：

```text
Web, social systems, web mining, web-scale AI.
```

适合：

```text
web agents
web search
web-scale retrieval
recommendation
knowledge graphs
online platforms
social networks
LLM for web
```

我们的：

```text
AI-Trader platform
Research OS website
agent browsing / web research
public artifact network
```

都可以从 TheWebConf 学方法。

## SIGIR

定位：

```text
Information retrieval.
```

SIGIR 对 RAG 极其重要。

因为 RAG 的一半其实是 retrieval。

适合：

```text
retrieval
ranking
search
query understanding
document retrieval
recommendation-adjacent ranking
evaluation metrics
interactive search
```

如果我们的贡献在：

```text
RAG retrieval strategy
Graph RAG retrieval
research paper retrieval
tool retrieval
memory retrieval
financial evidence retrieval
```

SIGIR 很值得看。

## CIKM / WSDM / RecSys

这一组也非常实用。

```text
CIKM:
  information and knowledge management

WSDM:
  web search and data mining

RecSys:
  recommender systems
```

对我们来说：

```text
research idea recommendation
paper recommendation
factor recommendation
strategy ranking
tool recommendation
agent memory retrieval
```

都可以从这些 venue 学。

## Agents / Multi-agent / Robotics

## AAMAS

定位：

```text
Autonomous agents and multi-agent systems.
```

这是 agent / multi-agent 传统强会。

适合：

```text
agent decision making
multi-agent coordination
agent communication
agent societies
market mechanisms
game theory
autonomous planning
human-agent interaction
```

对我们特别相关：

```text
AI-Trader:
  agent trading society

MoChat:
  agent communication layer

OpenHarness / FastAgent:
  agent execution framework

Quant Research OS:
  ResearcherAgent / BacktestAgent / PMReviewAgent 协作
```

如果我们做 multi-agent quant research 或 agent platform，AAMAS 是很自然的目标。

## CoRL

定位：

```text
Robot learning: robotics + machine learning.
```

它看的是 embodied intelligence / robot learning。

我们不是直接做机器人，但 CoRL 对 agent 研究有启发：

```text
policy learning
environment interaction
trajectory
planning
evaluation
offline-to-online
sim2real
```

coding agent / quant agent 不是 physical robot。

但它们也有：

```text
state
action
tool
environment
trajectory
reward
failure recovery
```

所以 CoRL 的 thinking 可以迁移到 agent harness。

## RSS / ICRA / IROS

定位：

```text
Robotics core venues.
```

如果以后我们关心：

```text
embodied agents
computer use agents
mobile agents
GUI agents
robotic task planning
```

这些 venue 可以作为方法来源。

## Theory / Statistics / Uncertainty

## AISTATS

定位：

```text
Artificial intelligence and statistics.
```

适合：

```text
statistical learning
probabilistic models
Bayesian methods
causal inference
optimization
theoretical and empirical ML
```

对 quant 很重要。

因为 quant 不能只有 demo。

Quant 需要：

```text
statistical validity
uncertainty
estimation
sampling
time-series robustness
overfitting control
```

如果我们以后做 factor evaluation / forecast uncertainty，AISTATS 值得看。

## UAI

定位：

```text
Uncertainty in AI.
```

适合：

```text
probabilistic reasoning
uncertainty estimation
causal inference
Bayesian learning
decision making under uncertainty
```

这对 quant / agent 都非常关键。

因为两者都不是确定性系统。

```text
agent 需要知道自己不知道什么。
quant 需要知道预测的不确定性和风险边界。
```

## COLT

定位：

```text
Learning theory.
```

COLT 更理论。

我们短期不一定投，但要知道它是 ML 理论高地。

如果以后我们做：

```text
generalization
online learning
bandits
regret bounds
theoretical RL
```

COLT 是核心 venue。

## Systems / AI Infrastructure

## MLSys

定位：

```text
Machine learning systems.
```

适合：

```text
training systems
inference systems
serving
distributed ML
hardware-aware optimization
benchmarking
ML infrastructure
```

对我们很相关：

```text
agent harness runtime
RAG system efficiency
LLM serving
tool orchestration
research automation pipeline
evaluation infrastructure
```

如果我们做 DeepSeek coding agent harness / Research OS runtime，MLSys 是值得关注的。

## OSDI / SOSP / NSDI / EuroSys

定位：

```text
Computer systems top venues.
```

这些不是传统 AI 顶会，但 AI infrastructure 很多时候会走 systems venue。

适合：

```text
distributed systems
operating systems
networking
storage
serving infrastructure
fault tolerance
resource scheduling
```

如果我们把 agent harness 做成：

```text
reliable execution system
sandboxed tool runtime
multi-agent workflow scheduler
memory and artifact system
```

就要学习 systems paper 的写法。

## SIGMOD / VLDB / ICDE

定位：

```text
Database and data management.
```

对 RAG / memory / knowledge base 很重要。

适合：

```text
vector database
graph database
query engine
data lineage
data lake
structured memory
retrieval system
benchmarking
```

我们的 MGP / RAG / Research OS memory，长期可能和 data management venue 相关。

## Human-AI Interaction

## CHI

定位：

```text
Human-computer interaction.
```

AI 产品最终要被人用。

CHI 适合：

```text
human-AI collaboration
AI tools
workflow studies
interface design
usability
trust and transparency
```

对我们很重要：

```text
Human PM Review
Research OS workspace
AI scientist workflow
agent product design
credit OS presentation
```

如果我们做的是：

```text
一个 human-in-the-loop research agent workspace
```

CHI / CSCW / UIST 就值得看。

## UIST / CSCW

```text
UIST:
  interactive systems and tools

CSCW:
  computer-supported cooperative work
```

如果我们的方向是：

```text
agent workspace
collaborative research environment
AI-assisted writing / coding / reviewing
team workflow automation
```

这两个 venue 很有启发。

## Responsible AI

## FAccT

定位：

```text
Fairness, Accountability, and Transparency.
```

适合：

```text
fairness
accountability
transparency
bias
governance
social impact
```

Agent / Quant / RAG 都有 governance 问题：

```text
agent 是否胡说？
RAG 是否引用错误？
金融 agent 是否越权？
自动化系统如何审计？
人类审核边界在哪里？
```

FAccT 对这些问题很重要。

## AIES

定位：

```text
AI, Ethics, and Society.
```

如果我们的 work 涉及：

```text
AI agent governance
financial AI risk
autonomous decision boundary
human oversight
compliance
```

AIES 可以作为参考。

## 我们自己的冲会地图

现在把所有 venue 映射到我们的方向。

## Agent Harness / Coding Agent

适合 venue：

```text
NeurIPS
ICML
ICLR
AAAI
AAMAS
MLSys
CHI / UIST
```

可能题目：

```text
agent failure recovery benchmark
coding agent harness reliability
tool-use trajectory evaluation
agent skill acquisition from failed sessions
multi-agent research workflow
human-in-the-loop coding agent system
```

我们已有基础：

```text
PENGYI_HARNESS_MAP000
PENGYI_HARNESS001
HKUDS OpenHarness / AnyTool / FastAgent / AutoAgent
MLRL004 RLHF / Agent Training
NEETCODE as coding agent benchmark
```

## RAG / Graph RAG / Memory

适合 venue：

```text
ACL
EMNLP
SIGIR
KDD
TheWebConf
CIKM
ICLR
NeurIPS
```

可能题目：

```text
research paper RAG benchmark
Graph RAG evidence grounding
multimodal RAG for scientific documents
memory governance for AI agents
long-context retrieval compression
RAG failure diagnosis
```

我们已有基础：

```text
HKUDS051 RAG summary
LightRAG
RAG-Anything
MiniRAG
VideoRAG
MGP
CatchMe
SepLLM
```

## Quant Research OS / AI for Finance

适合 venue：

```text
KDD
TheWebConf
SIGIR
RecSys
AAAI
IJCAI
NeurIPS / ICML / ICLR workshops
ACM ICAIF
```

注意：

```text
ACM ICAIF 是 AI in Finance 的专门会议，不一定和 NeurIPS/ICML 同层级，但对 AI + finance 方向非常相关。
```

可能题目：

```text
AI-native quant research workflow
forecast ledger for financial agents
financial RAG benchmark
factor hypothesis generation and evaluation
human PM review for quant agents
research-to-backtest compiler
```

我们已有基础：

```text
HKUDS052 Quant summary
LLMQuant000-008
X2Strategy000
QuantMind
Vibe-Trading
AI-Trader
FutureShow
UrbanGPT
```

## AI Scientist / Research OS

适合 venue：

```text
NeurIPS
ICML
ICLR
AAAI
ACL / EMNLP
KDD
CHI / UIST
MLSys
workshops on AI scientist / agents / automated research
```

可能题目：

```text
AI research workflow benchmark
automatic hypothesis-to-experiment loop
research report evaluator
evidence-grounded paper writing agent
human PM review protocol
scientific artifact lineage system
```

我们已有基础：

```text
HKUDS AI-Researcher
Auto-Deep-Research
DeepResearch-Eval
DeepInnovator
Litewrite
Paper2Slides
Research OS notes
Credit OS / Transition OS
```

## Graph / Recommendation / Structured Knowledge

适合 venue：

```text
KDD
WWW / TheWebConf
SIGIR
RecSys
CIKM
ICLR
NeurIPS
```

可能题目：

```text
graph-language model for research memory
recommendation system for research ideas
strategy recommendation
factor graph reasoning
asset-event graph construction
LLM-enhanced knowledge graph retrieval
```

我们已有基础：

```text
GraphAgent
OpenGraph
GraphGPT
HiGPT
RecLM
XRec
AutoCF
KGRec
```

## 顶会文章的贡献类型

一篇顶会文章通常不是“我做了一个东西”。

它需要落在一种贡献类型里。

常见类型：

```text
1. Method paper
   新方法、新模型、新训练目标。

2. Benchmark paper
   新任务、新数据、新评估协议、新 leaderboard。

3. System paper
   新系统、新架构、新效率、新可靠性。

4. Analysis paper
   解释现象、失败模式、scaling law、behavior study。

5. Dataset paper
   高质量数据集、标注协议、任务定义。

6. Application paper
   在重要真实场景里证明 AI 方法有效。
```

我们的项目更容易从：

```text
benchmark paper
system paper
analysis paper
application paper
```

切入。

短期不要硬装成纯 method paper。

我们的优势在：

```text
系统集成
真实 workflow
agent harness
domain grounding
public artifact
research engineering
```

## 从 workshop 到 main conference

现实路线应该是：

```text
1. 公开学习和复现
2. 做小 demo
3. 做 benchmark / dataset / evaluation
4. 投 workshop
5. 收 feedback
6. 扩大实验
7. 投 main conference
```

Workshop 不是失败。

Workshop 是进入社区的入口。

尤其对我们这种正在从工程 / 金融场景切入 AI research 的路线，workshop 非常重要。

适合我们的 workshop 类型：

```text
LLM agents
AI for finance
AI scientist
RAG / long-context
tool use
evaluation
AI safety
ML for systems
AI for science
data-centric AI
```

## 如何读顶会

不要泛泛读。

我们应该按 Research OS 方式读：

```text
1. 先看 accepted paper list
2. 标记 oral / spotlight / best paper
3. 找和自己方向相关的 workshop
4. 读 20 篇 title + abstract
5. 精读 3-5 篇
6. 提取 task / method / data / eval / limitation
7. 写成 website note
8. 找 repo 复现或提 PR
9. 反向生成自己的 project idea
```

每篇 paper 的卡片：

```text
Paper:
Problem:
Why important:
Method:
Data:
Metric:
Baseline:
Main result:
Limitation:
Repo:
Can we reproduce:
Can it enter Pengyi Research OS:
Possible PR:
Possible follow-up idea:
```

这就是顶会阅读的正确方式。

## 我们的优先级

结合我们当前路线，我建议优先关注：

```text
P0:
  NeurIPS / ICML / ICLR
  ACL / EMNLP
  KDD / SIGIR

P1:
  AAAI / IJCAI
  AAMAS
  RecSys / TheWebConf / CIKM
  MLSys

P2:
  CVPR / ICCV / ECCV / ACM MM
  CHI / UIST / CSCW
  AISTATS / UAI / COLT
  FAccT / AIES
```

为什么：

```text
我们现在最强主线是 agent + RAG + quant research automation + research engineering。
```

所以最自然的 venue 是：

```text
LLM / agent / RAG:
  ACL / EMNLP / ICLR / NeurIPS

Quant / data mining / graph / recommendation:
  KDD / SIGIR / TheWebConf / RecSys

Agent system / harness:
  AAAI / AAMAS / MLSys / CHI
```

## 我们可以立刻做的事

第一步：

```text
建立 AI Conference Reading Board。
```

目录可以是：

```text
ai-conference-board/
  neurips/
  icml/
  iclr/
  acl/
  emnlp/
  kdd/
  sigir/
  aamas/
  mlsys/
  workshops/
```

第二步：

```text
每周精读 3 篇 paper。
```

第三步：

```text
每篇 paper 写一张 research card。
```

第四步：

```text
每个月产出一篇 synthesis note。
```

第五步：

```text
把 synthesis note 反向生成 project idea / benchmark idea / PR idea。
```

## 对我们个人叙事的意义

顶会地图不是装饰。

它直接服务我们的长期叙事：

```text
I am building toward an AI scientist profile:
  research taste
  engineering execution
  domain grounding
  public artifact production
  open-source contribution
  conference-level problem selection
```

我们不只是刷项目。

我们要逐渐训练：

```text
看到一个 repo -> 识别它属于哪个 research community
看到一个 problem -> 判断它该投哪个 venue
看到一个 demo -> 判断它缺什么 evaluation
看到一个 paper -> 反向抽象成自己的 Research OS module
```

这就是 research taste。

## 当前结论

AI 顶会地图可以压成这样：

```text
NeurIPS / ICML / ICLR:
  AI / ML core

ACL / EMNLP:
  LLM / NLP / RAG

CVPR / ICCV / ECCV:
  vision / multimodal

KDD / SIGIR / TheWebConf / RecSys:
  data mining / retrieval / recommendation / graph / web / finance-adjacent

AAAI / IJCAI / AAMAS:
  general AI / agents / reasoning / planning

MLSys / OSDI / SOSP / SIGMOD / VLDB:
  AI infrastructure / systems / data management

CHI / UIST / CSCW:
  human-AI workflow and productized research systems

AISTATS / UAI / COLT:
  statistics / uncertainty / theory

FAccT / AIES:
  responsible AI and governance
```

对我们来说，第一阶段不需要全追。

重点是：

```text
NeurIPS / ICML / ICLR
ACL / EMNLP
KDD / SIGIR
AAAI / AAMAS
MLSys
```

因为这些最贴近：

```text
AI agents
RAG
Research OS
Quant Research OS
coding agent harness
AI scientist workflow
```

下一步可以做：

```text
AI_CONF001 -> NeurIPS / ICML / ICLR 精读路线
AI_CONF002 -> ACL / EMNLP / RAG / LLM Agent 路线
AI_CONF003 -> KDD / SIGIR / Quant + Retrieval + Recommendation 路线
AI_CONF004 -> AAMAS / Agent Systems 路线
AI_CONF005 -> 如何把我们的项目变成 workshop / main conference paper
```

## References

- NeurIPS official site: <https://neurips.cc/>
- ICML official site: <https://icml.cc/>
- ICLR official site: <https://iclr.cc/>
- AAAI Conference official site: <https://aaai.org/conference/aaai/>
- CVPR official site: <https://cvpr.thecvf.com/>
- ACL official site: <https://2026.aclweb.org/>
- SIGKDD official site: <https://www.kdd.org/>
- SIGIR sponsored conferences: <https://sigir.org/conferences/sponsored-conferences/>
- AISTATS official site: <https://aistats.org/>
- UAI official site: <https://www.auai.org/>
- CoRL official site: <https://www.corl.org/>
- AAMAS 2026 official site: <https://cyprusconferences.org/aamas2026/>
