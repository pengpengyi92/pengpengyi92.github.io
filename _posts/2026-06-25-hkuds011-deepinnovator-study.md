---
title: "HKUDS011: DeepInnovator 作为 Scientific Idea Foundation Model 与 Research Innovation Training Layer"
date: 2026-06-25 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds011, hkuds, deepinnovator, scientific-idea-generation, research-innovation, grpo, verl, reward-model, ai-scientist, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第十二篇。

```text
HKUDS011 -> DeepInnovator
```

到目前为止，HKUDS 第一阶段我们已经看了：

```text
HKUDS000 -> study map
HKUDS001 -> LightRAG
HKUDS002 -> Vibe-Trading
HKUDS003 -> nanobot
HKUDS004 -> CLI-Anything
HKUDS005 -> AI-Trader
HKUDS006 -> AgentSpace
HKUDS007 -> RAG-Anything
HKUDS008 -> AutoAgent
HKUDS009 -> DeepCode
HKUDS010 -> AI-Researcher
```

现在来看 `DeepInnovator`。我对它的定位是：

```text
DeepInnovator = Scientific Idea Foundation Model + Research Innovation Training Layer
```

它和前面几个项目的关系很清楚：

```text
RAG-Anything  = multimodal document ingestion
LightRAG      = research memory
Vibe-Trading  = quant research workflow
nanobot       = personal agent shell
CLI-Anything  = software action layer
AI-Trader     = live trading platform layer
AgentSpace    = organizational agent workspace
AutoAgent     = self-developing agent factory
DeepCode      = research-to-code implementation layer
AI-Researcher = autonomous scientific discovery workflow
DeepInnovator = train a model to become better at research idea innovation
```

前面 AI-Researcher 更像一个完整科研 agent workflow。

DeepInnovator 则更底层一点：它不是只把一个 agent workflow 搭起来，而是尝试训练一个更擅长提出、改进、评价科研 idea 的模型。

也就是说：

```text
AI-Researcher = 用 agents 做科研流程
DeepInnovator = 训练 model 的科研创新能力
```

这对 Pengyi Research OS 很关键。因为我们最终不只是要会调用现成大模型，而是要思考：

```text
什么样的数据能训练科研创新能力？
什么样的 reward 能衡量 idea 质量？
什么样的 multi-turn loop 能让 idea 越改越好？
什么样的 benchmark 能推动 AI scientist 真的变强？
```

DeepInnovator 正是在回答这些问题。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `DeepInnovator`。

| Item | Value |
|---|---|
| repo | `DeepInnovator` |
| remote | `https://github.com/HKUDS/DeepInnovator.git` |
| branch | `main` |
| local head | `1144198` |
| latest local commit date | `2026-03-07` |
| latest local commit | `Update README.md` |
| status | clean, synced with `origin/main` after fetch |
| license | MIT at root; bundled `verl` code carries Apache-2.0 notices |
| paper in README | `arXiv:2602.18920` |
| model in README | DeepInnovator-14B |
| dataset/model links | Hugging Face links in README |

本地规模：

| Metric | Count |
|---|---:|
| total files tracked by `rg --files` | 511 |
| Python files | 402 |
| YAML/YML files | 59 |
| Markdown files | 14 |

核心目录：

```text
DeepInnovator/
  assets/
  recipe/
    DeepInnovator/
      data_preparation/
      config/
      metrics/
      preprocess.py
      reward_function.py
      DeepInnovator_interation.py
      DeepInnovator_agent_loop.py
      train_rl.sh
      utils.py
  verl/
  LICENSE
  README.md
  README_CN.md
  setup.py
```

它的结构说明了一个事实：

```text
DeepInnovator = data recipe + reward recipe + VERL RL training framework
```

它不是一个轻量级 app，也不是直接给用户点按钮的系统，而是一个科研 idea model 的训练工程。

## 一句话理解

DeepInnovator 最核心的价值是：

```text
把“科研 idea 的产生与改进”建模成一个可训练、可奖励、可迭代的任务。
```

普通 LLM 也会生成 idea，但它的问题是：

```text
容易空泛
容易 A+B+C 拼接
容易看起来高级但不可执行
容易缺少真实 implementation detail
容易忽略 practical challenges
容易 reward hacking
```

DeepInnovator 想解决的是：

```text
如何让模型提出更像真实科研工作的 idea？
如何让 idea 有更强的 novelty、feasibility、effectiveness、detailedness？
如何让模型通过多轮反馈不断 refine idea？
```

它把这个问题拆成三件事：

```text
1. 从论文中构造 idea training data
2. 设计 reward，鼓励 idea 逐轮变好
3. 用 VERL / GRPO 做 multi-turn RL training
```

这就是它和 AI-Researcher 的不同：

```text
AI-Researcher 主要是 workflow。
DeepInnovator 主要是 model training recipe。
```

## Key Features

README 里给出的能力包括：

```text
AI Research Idea Generator
Cross-Disciplinary Innovation Engine
Scientific Hypothesis & Question Formation
Research Gap & Trend Analysis
Innovation Methodology Framework
Creative Problem-Solving Assistant
```

我把它压成四个核心能力：

第一，生成 research idea。

```text
从已有论文和研究趋势中提出新的研究方向。
```

第二，识别 gap 和 trend。

```text
不是只总结论文，而是看哪些问题还没解决，哪些方向在加速。
```

第三，做 cross-disciplinary synthesis。

```text
从不同领域之间找到非显然的连接。
```

第四，训练 idea refinement 能力。

```text
让模型在批评和反馈下，把一个 idea 改得更具体、更可执行、更像真实研究。
```

这和我们想做的 AI scientist 能力高度一致。

## README Performance Claims

README 报告了 DeepInnovator-14B 的表现：

```text
DeepInnovator-14B outperforms Qwen-14B-Instruct across evaluation dimensions.
Win rates vs base model: 80.53% - 93.81%.
It is competitive with GPT-4o and Gemini-2.5-pro.
It reports stronger well-justified rationale score than GPT-4o: 82.3% vs 77.9%.
```

README 还展示了在 law、education、biotech 等未直接训练领域的 zero-shot transfer 结果。

这里要严谨一点：这些是项目 README 的报告结果，不是我本地独立复现的 benchmark。

所以更准确的表述是：

```text
README claims / project reports that DeepInnovator-14B improves research idea quality over Qwen-14B-IT and is competitive with stronger proprietary models on selected dimensions.
```

对我们来说，重点不是先相信所有数值，而是学习它的评价思路：

```text
Novelty
Feasibility
Effectiveness
Detailedness
Rationale quality
Cross-domain generalization
```

这些指标很适合迁移到 Quant Research OS。

## Architecture

README 把架构分成三个主要部分。

第一，`Intelligent Knowledge Synthesis Pipeline`：

```text
dense literature
  -> structured cognitive primitives
  -> Insight / Research Trending / Serendipity
```

第二，`Next Idea Prediction Training Paradigm`：

```text
idea generation
  -> evaluation
  -> refinement
  -> next idea
```

第三，`Decoupled Reward-Comment RL Architecture`：

```text
feedback / comment 与 reward score 分开
减少 creative task 中的 reward hacking
```

这三个点是 DeepInnovator 最值得学习的地方。

它不是单纯说“让模型生成 idea”，而是把 idea generation 拆成：

```text
knowledge synthesis
idea prediction
critic feedback
reward scoring
multi-turn RL
```

这就是从 prompt engineering 走向 model training 的分水岭。

## Data Preparation Pipeline

核心目录：

```text
recipe/DeepInnovator/data_preparation/
```

README 把数据准备分成三大步：

```text
1. Download Papers
2. Extract Target Paper Ideas
3. Generate Training Data
```

内部又分成四步：

```text
step1.py -> paper analysis and extraction
step2.py -> paper routing and grouping
step3.py -> connections, serendipity, trends
step4.py -> next idea synthesis
```

完整输出结构：

```text
data/{paper_id}/
  target_paper/
    raw_paper/
      paper_md/
      paper_idea.json
  raw_paper/
  layer0/
    paper_memory/
  layer1/
    inner_paper_memory.json
    inter_paper_group.json
  layer2/
    connections.json
    serendipity.json
    research_trending.json
  insights/
    idea_spark.json
```

这个结构非常重要。

它说明 DeepInnovator 不是直接拿论文标题训练，而是先构造多层 research memory。

## Step 0: Pull Papers

文件：

```text
recipe/DeepInnovator/data_preparation/data_prepare/pull_papers.py
```

它从 arXiv 下载论文，类别覆盖：

```text
cs.AI, cs.LG, cs.CL, cs.CV, cs.IR
stat.ML, stat.AP, stat.CO, stat.TH, stat.ME
q-fin.TR, q-fin.RM, q-fin.PM, q-fin.ST, q-fin.EC
math.OC, math.PR, math.ST, math.NA, math.AT
```

这里有一个对我们很有意思的点：

```text
q-fin 被纳入了 paper crawling category。
```

这意味着 DeepInnovator 的 pipeline 天然可以覆盖 quantitative finance paper。

对我们未来做 quant idea model，这是一个直接入口：

```text
q-fin papers
  -> factor idea memory
  -> research trend
  -> serendipity
  -> next alpha hypothesis
```

如果要做 Pengyi Quant DeepInnovator，第一步可以从 q-fin categories 开始。

## Step 1: Paper Memory

文件：

```text
recipe/DeepInnovator/data_preparation/data_prepare/step1.py
```

它把 raw paper markdown 转成结构化 paper memory。

输出字段包括：

```text
paper_id
paper_title
paper_summary
research_domain
key_findings
methodology
limitations
future_work
confidence
paper_path
```

这就是 layer0。

对 Research OS 来说，layer0 是最基础的 memory：

```text
每篇论文讲了什么？
它属于哪个 domain？
关键发现是什么？
方法是什么？
限制是什么？
未来工作是什么？
```

这一步决定后面 idea generation 的质量。

如果 layer0 抽取不准，后面所有 research idea 都会漂。

## Step 2: Paper Routing and Grouping

文件：

```text
recipe/DeepInnovator/data_preparation/data_prepare/step2.py
```

它把 paper memory 放进 paper profile，并让 agent 决定：

```text
update_existing
create_new
filtered_out
merge
```

输出：

```text
layer1/inner_paper_memory.json
layer1/inter_paper_group.json
```

这一步的意义是：

```text
把单篇论文记忆组织成研究主题结构。
```

一个 research idea 通常不是从一篇论文产生，而是从一组论文之间的张力产生。

比如：

```text
paper A 解决了建模问题
paper B 解决了评估问题
paper C 暴露了数据问题
paper D 提供了新工具
```

layer1 就是在构建这种“研究组块”。

## Step 3: Connections, Serendipity, Trends

文件：

```text
recipe/DeepInnovator/data_preparation/data_prepare/step3.py
```

它生成三类 layer2 产物：

```text
connections.json
serendipity.json
research_trending.json
```

对应三个 agent config：

```text
idea_paper_connections.yaml
idea_serendipity_engine.yaml
idea_research_trending.yaml
```

`Paper Connections` 找：

```text
complementary
contradictory
evolutionary
methodological
cross-domain
```

`Serendipity Engine` 找：

```text
tech_domain
tool_interest
problem_solution
skill_transfer
multi_domain
```

`Research Trend Radar` 找：

```text
trend_name
description
source_papers
projected_impact
maturity_stage
confidence
```

这就是 DeepInnovator 的核心 insight layer。

它不仅知道每篇论文，还知道：

```text
论文之间有什么连接？
哪些连接是非显然的？
哪些方向在形成趋势？
哪里可能有新 idea？
```

这对 AI scientist 非常重要。

真正的好 idea 往往不在单篇 paper summary 里，而在 paper 与 paper 之间。

## Step 4: Idea Spark

文件：

```text
recipe/DeepInnovator/data_preparation/data_prepare/step4.py
```

核心函数：

```text
step4_idea_spark()
```

它读取：

```text
layer1 paper groups
layer2 connections
layer2 serendipity
layer2 research_trending
```

然后生成：

```text
insights/idea_spark.json
```

它还有一个很有意思的机制：

```text
dropout_layer2()
```

这会随机删除一部分 connections 及相关 serendipity/trending item。

这个设计的意义是：

```text
让模型在不完整知识下也能进行 robust idea synthesis。
```

这很像训练 researcher 的“补全能力”：

```text
不是把所有答案喂给你，而是让你根据部分线索生成下一个合理 idea。
```

这就是 `Next Idea Prediction` 的味道。

## Idea Spark Schema

`paper_idea_spark.yaml` 定义了 idea 输出结构。

每个 idea 包含：

```text
current_limitations
idea_summary
technical_approach
supporting_insights
novelty_statement
source_paper_ids
confidence
```

这里最关键的是 `technical_approach`。

它要求：

```text
step-by-step workflow
data preparation
model / algorithm design
key technical components
implementation details
evaluation methodology
```

这很重要。因为很多 LLM idea 的问题是：

```text
idea_summary 很漂亮
technical_approach 很空
evaluation methodology 很虚
```

DeepInnovator 的 schema 强行把 idea 变成可实现的 research plan。

这点非常适合我们迁移到 quant。

Quant factor idea 也不能只写：

```text
利用市场情绪构建 alpha。
```

而要写：

```text
data source
universe
signal formula
rebalance frequency
neutralization
backtest period
transaction cost
risk diagnostics
expected failure modes
```

## Preprocess for RL

文件：

```text
recipe/DeepInnovator/preprocess.py
```

它把生成的数据处理成 RL training parquet：

```text
rl_train.parquet
rl_validation.parquet
```

它会读取：

```text
target_paper/raw_paper/paper_idea.json
insights/idea_spark.json
layer0 / layer1 / layer2 optional context
```

然后构造：

```text
prompt
ground_truth
raw_prompt
extra_info
reward_model
data_source = DeepInnovator
agent_name = DeepInnovator_agent
index
```

其中 `SYSTEM_PROMPT` 是 `Idea Quality Improver`。

它要求模型：

```text
improve a research idea
address weaknesses
enhance technical_approach
preserve core concept
output <Idea>{...}</Idea>
```

这说明 DeepInnovator 的训练任务不是从零生成，而是：

```text
给一个 first version idea，让模型不断 refine。
```

这和科研训练非常像。

真正的研究通常不是一次写对，而是：

```text
first idea
  -> criticism
  -> refinement
  -> more concrete technical approach
  -> better evaluation
  -> stronger novelty statement
```

DeepInnovator 把这个过程做成了 RL task。

## DeepInnovator Interaction

文件：

```text
recipe/DeepInnovator/DeepInnovator_interation.py
```

核心类：

```text
DeepInnovatorInteraction
```

它继承：

```text
verl.interactions.base.BaseInteraction
```

它做一件很关键的事：

```text
用 discriminator 判断 idea 是否像真实已发表研究。
```

prompt 名字是：

```text
Idea Authenticity Checker
```

它让 discriminator 判断：

```text
1 = real research work
0 = fictional research idea
```

并且明确要求不要看 citation、DOI、format，而是看内容质量：

```text
technical depth
problem clarity
limitations discussion
executable technical approach
implementation details
practical challenges
```

如果 discriminator 判断 idea 是 real：

```text
reward = 1.0
should_terminate_sequence = True
```

如果判断是 fictional：

```text
reward = 0.0
should_terminate_sequence = False
```

这个设计可以理解为：

```text
让模型学会生成“像真实研究”的 idea。
```

当然这里也有风险。我们后面会讲。

## DeepInnovator Agent Loop

文件：

```text
recipe/DeepInnovator/DeepInnovator_agent_loop.py
```

核心类：

```text
DeepInnovatorAgentLoop
```

它继承：

```text
verl.experimental.agent_loop.tool_agent_loop.ToolAgentLoop
```

它负责：

```text
raw_prompt messages
interaction config
pending / generating / interacting states
repeat rollouts
turn scores
response ids / masks
extra_fields
```

关键输出字段：

```text
turn_scores
messages
ground_truth
```

这说明 DeepInnovator 的 RL 不是单轮 completion，而是 multi-turn interaction。

训练脚本也配置了：

```text
max_user_turns = 5
max_assistant_turns = 6
num_repeat_rollouts = 3
```

所以它训练的是：

```text
模型在多轮反馈中改进 research idea 的能力。
```

这比单轮 SFT 更接近科研真实过程。

## Reward Function

文件：

```text
recipe/DeepInnovator/reward_function.py
```

核心函数：

```text
conversation_level_reward_func()
```

核心类：

```text
DeepInnovatorRewardManager
```

reward manager 注册名：

```text
DeepInnovator
```

它从配置读取：

```text
metric_weights
default_reward_kwargs
delta_reward_kwargs
```

默认 metric weights：

```yaml
delta_reward: 5
token_amount: 0.1
```

也就是说，它主要奖励 idea 的逐轮改进，少量奖励合适长度。

这很合理。科研 idea 不是越长越好，而是要：

```text
更具体
更接近真实 ground truth
更可执行
更有技术细节
```

## Metric 1: delta_reward

文件：

```text
recipe/DeepInnovator/metrics/delta_reward.py
```

它做的事：

```text
extract all assistant ideas from conversation
compare adjacent idea pairs
ask LLM judge which idea is closer to ground_truth
score = new idea score - old idea score
sum over all adjacent pairs
```

也就是说：

```text
如果后一轮 idea 比前一轮更接近 ground truth，就给正奖励。
```

这是很好的设计。

因为它奖励的是：

```text
improvement
```

不是单纯奖励最后一版。

这对科研训练很重要。真实 researcher 不是一开始就提出最优解，而是能不能在 feedback 下越改越好。

## Metric 2: token_amount

文件：

```text
recipe/DeepInnovator/metrics/token_amount.py
```

它奖励 idea 长度落在合适范围：

```text
3000 <= length <= 5000 -> reward 1.0
length < 3000 -> proportional reward
length > 5000 -> linear decay
```

这个 metric 看似简单，但很实用。

过短的 idea 通常缺少细节，过长的 idea 可能啰嗦、堆料、偏离重点。

它想把 idea 控制在：

```text
足够具体，但不过度膨胀。
```

## Metric 3: basic_reward

文件：

```text
recipe/DeepInnovator/metrics/basic_reward.py
```

它读取：

```text
turn_scores
```

并聚合成 reward。

这和 `DeepInnovatorInteraction` 中的 discriminator reward 对应。

如果 turn_scores 中包含 `-999`，则表示 invalid sample，不参与训练。

这说明 DeepInnovator 在处理：

```text
multi-turn interaction score
invalid sample filtering
conversation-level reward
```

## Training Script

文件：

```text
recipe/DeepInnovator/train_rl.sh
```

核心训练配置：

```text
algorithm.adv_estimator = grpo
actor_rollout_ref.model.path = ./qwen2.5-14b-it
train_batch_size = 16
ppo_mini_batch_size = 4
n_rollouts = 8
trainer.total_epochs = 3
rollout.name = vllm
rollout.mode = async
multi_turn.enable = true
max_user_turns = 5
max_assistant_turns = 6
num_repeat_rollouts = 3
n_gpus_per_node = 8
```

这说明它是重训练工程：

```text
14B base model
8 GPU training assumption
vLLM rollout
VERL PPO/GRPO infrastructure
WandB logging
custom reward manager
custom interaction
```

所以 DeepInnovator 不适合我们立刻本地完整训练。

但它非常适合我们学习：

```text
如何把科研 idea generation 变成 RL training recipe。
```

## Run-Readiness Caveats

本地代码里有几个运行前必须检查的点。

第一，`train_rl.sh` 里有：

```bash
cd ./verl-main
```

但本地目录是：

```text
verl/
```

第二，`train_rl.sh` 指向：

```text
recipe/DeepInnovator/config/DeepInnovator_interaction_config.yaml
```

但本地存在的是：

```text
recipe/DeepInnovator/config/ResearchGAN_interaction_config.yaml
```

第三，`recipe/DeepInnovator/config/agent.yaml` 里写的是：

```yaml
name: ResearchGAN_agent
_target_: recipe.ResearchGAN.ResearchGAN_agent_loop.ResearchGANAgentLoop
```

但当前源码里有：

```text
recipe.DeepInnovator.DeepInnovator_agent_loop.DeepInnovatorAgentLoop
```

第四，`delta_reward.py` 里导入：

```python
from recipe.ResearchGAN.utils import call_agent
from recipe.ResearchGAN.utils import clean_idea
```

但当前本地目录是：

```text
recipe/DeepInnovator/utils.py
```

这些可能是项目从 ResearchGAN 重命名为 DeepInnovator 后留下的路径残留。

也可能是作者训练环境里有额外 alias 或目录结构。

但从本地代码出发，严谨结论是：

```text
README 的 concept 和 recipe 清楚，但完整 training run 之前需要修正这些路径/config inconsistencies。
```

这也是一个潜在 PR / issue 方向。

## DeepInnovator 和 AI-Researcher 的区别

这两个项目都和 AI scientist 有关，但层级不同。

| Project | Position |
|---|---|
| AI-Researcher | agent workflow for autonomous scientific discovery |
| DeepInnovator | model training recipe for research idea innovation |

AI-Researcher 关注：

```text
给定 references / idea
  -> agent 做 survey
  -> agent 写 plan
  -> agent 实现
  -> agent 实验
  -> agent 分析
```

DeepInnovator 关注：

```text
如何训练一个更擅长提出和改进 research ideas 的 model
```

所以在 Pengyi Research OS 里：

```text
AI-Researcher = research workflow layer
DeepInnovator = research idea model layer
```

未来可以组合成：

```text
DeepInnovator proposes stronger ideas
AI-Researcher orchestrates full research workflow
DeepCode implements code
LightRAG stores memory
RAG-Anything ingests papers
```

## DeepInnovator 和 DeepCode 的区别

DeepCode 处理的是：

```text
paper / requirement -> code implementation
```

DeepInnovator 处理的是：

```text
paper memory / connections / trends -> research idea refinement model
```

DeepCode 更像工程师。

DeepInnovator 更像研究创意模型。

两者结合：

```text
DeepInnovator generates the hypothesis.
DeepCode implements the hypothesis.
AI-Researcher validates it in a research loop.
```

这就是 AI scientist 的关键组合。

## DeepInnovator 和 LightRAG / RAG-Anything 的关系

DeepInnovator 的数据构造其实需要两个能力：

```text
1. 把论文稳定解析成结构化内容
2. 把多篇论文组织成可检索、可关联、可生成 idea 的 memory
```

RAG-Anything 可以增强第一个能力：

```text
PDF / tables / formulas / figures / multimodal material -> structured content
```

LightRAG 可以增强第二个能力：

```text
paper memory graph
connection retrieval
trend retrieval
cross-paper knowledge organization
```

所以未来如果我们做自己的 version：

```text
RAG-Anything -> parse papers / reports
LightRAG -> store graph memory
DeepInnovator-style recipe -> train idea refinement model
```

## 对 Pengyi Quant Research OS 的启发

DeepInnovator 对我们最直接的启发是：

```text
Quant alpha idea generation 也可以做成训练任务。
```

我们可以把它迁移成：

```text
q-fin papers
WorldQuant factor examples
market microstructure notes
broker reports
internal sanitized experiment logs
```

然后构造多层 memory：

```text
layer0: single factor / paper memory
layer1: factor group / theme memory
layer2: factor connections / serendipity / trend signals
insights: next alpha idea spark
```

再做 RL task：

```text
first alpha idea
  -> critique
  -> refined alpha idea
  -> compare against ground-truth strong factor / human-written research memo
  -> reward improvement
```

这会形成：

```text
Pengyi Quant Innovator
```

也就是一个专门训练：

```text
factor hypothesis generation
signal refinement
backtest-aware research design
risk-aware alpha idea writing
```

的模型或 agent。

## Quant Version Schema

DeepInnovator 的 idea schema 可以迁移为 factor schema。

原 schema：

```text
current_limitations
idea_summary
technical_approach
supporting_insights
novelty_statement
source_paper_ids
confidence
```

Quant 版本可以是：

```text
current_market_or_factor_limitations
alpha_hypothesis_summary
signal_construction
data_requirements
universe_and_rebalance
backtest_design
risk_and_bias_checks
supporting_evidence
novelty_statement
expected_failure_modes
confidence
```

这样生成的 idea 才能进入后续工程链路：

```text
DeepInnovator-style model
  -> DeepCode implementation
  -> Vibe-Trading research workflow
  -> AI-Trader platform simulation
  -> human PM review
```

## Research OS 总架构位置

加入 DeepInnovator 之后，我们的 HKUDS Research OS 图可以这样理解：

```text
Input:
  RAG-Anything

Memory:
  LightRAG

Idea Model:
  DeepInnovator

Research Workflow:
  AI-Researcher

Implementation:
  DeepCode

Agent Factory:
  AutoAgent

Software Action:
  CLI-Anything

Personal Shell:
  nanobot

Organization:
  AgentSpace

Quant Workflow / Trading:
  Vibe-Trading + AI-Trader
```

DeepInnovator 填补的是：

```text
idea model layer
```

没有它，Research OS 可能只是会整理资料、执行任务、写代码。

有了它，Research OS 才开始有：

```text
系统性生成研究假设
系统性改进研究 idea
系统性训练创新能力
```

## 可以提 PR / issue 的方向

如果我们后续真的使用 DeepInnovator，比较适合的贡献方向包括：

| Direction | Why |
|---|---|
| Fix config path naming | `ResearchGAN` / `DeepInnovator` path inconsistency 会影响复现 |
| Fix `train_rl.sh` directory assumption | `cd ./verl-main` 与当前 `verl/` 不一致 |
| Add minimal dry-run docs | 训练太重，用户需要小规模验证 pipeline |
| Add data preparation toy sample | 方便先跑通 layer0/layer1/layer2/insights |
| Document reward metrics clearly | 解释 `delta_reward`、`token_amount`、`basic_reward` |
| Add q-fin example | 项目已经抓 q-fin categories，很适合做 quant research idea demo |
| Add run-readiness checklist | API keys、model config、WandB、GPU、dataset、paths |
| Add schema examples | 展示一条完整 idea_spark 输入输出 |

最适合我们的第一个小贡献，可能是：

```text
提一个文档 PR：Run-readiness checklist + config path notes。
```

这个 PR 不需要动核心训练逻辑，但能帮真实用户少踩坑。

## 风险和边界

DeepInnovator 的方向很强，但也有几个风险。

第一，训练成本很高。

```text
14B model + 8 GPU + vLLM + GRPO + multi-turn rollout
```

我们现阶段更适合学习 recipe，而不是马上全量训练。

第二，discriminator reward 可能被 hack。

如果目标是“让 idea 看起来像真实研究”，模型可能学会写得像真论文，但不一定真的正确。

所以必须加：

```text
implementation validation
experiment validation
human expert review
novelty search
```

第三，ground truth idea 不等于唯一正确答案。

科研不是只接近已发表论文。

如果 reward 过度贴近 ground truth，可能压制真正 novel 的方向。

第四，金融场景更敏感。

如果迁移到 quant：

```text
不能用未脱敏因子
不能泄露私有数据
不能把回测幻觉当真实 alpha
不能用 LLM judge 替代统计检验
```

第五，本地代码有路径一致性问题。

运行前需要先修配置，不适合直接无脑训练。

## 对我们路线的意义

DeepInnovator 对我们最大的意义是：

```text
AI scientist 不是只靠更强 prompt，而是可以训练“科研创新能力”。
```

它让我们看到一条更高层路线：

```text
收集高质量科研轨迹
结构化文献记忆
生成 idea candidates
设计 reward
训练 idea refinement model
接入 research workflow
接入 implementation engine
接入 evaluation / benchmark
```

这和我们想走的路线完全一致。

我们想成为能持续产出顶会、开源、有影响力 project 的 AI scientist。

那就不能只做：

```text
读论文
写总结
调 API
做 demo
```

还要做：

```text
构建自己的 research data flywheel
构建自己的 idea generation benchmark
构建自己的 implementation pipeline
构建自己的 evaluation loop
构建自己的 public research assets
```

DeepInnovator 给的是 idea model 这部分的参考答案。

## Study Map 更新

加入 DeepInnovator 之后，HKUDS 第一阶段地图变成：

| Index | Project | Role in Pengyi Research OS |
|---|---|---|
| HKUDS000 | Study Map | HKUDS project navigation |
| HKUDS001 | LightRAG | research memory and graph retrieval |
| HKUDS002 | Vibe-Trading | quant research workflow reference |
| HKUDS003 | nanobot | personal always-on agent shell |
| HKUDS004 | CLI-Anything | agent-native software action layer |
| HKUDS005 | AI-Trader | agent-native live trading platform layer |
| HKUDS006 | AgentSpace | organizational agent workspace |
| HKUDS007 | RAG-Anything | multimodal document ingestion layer |
| HKUDS008 | AutoAgent | self-developing agent factory |
| HKUDS009 | DeepCode | paper-to-code and research-to-code implementation layer |
| HKUDS010 | AI-Researcher | autonomous scientific discovery and research-agent benchmark layer |
| HKUDS011 | DeepInnovator | scientific idea foundation model and research innovation training layer |

`HKUDS011` 的一句话总结：

```text
DeepInnovator 把科研 idea generation 从 workflow 问题推进到 model training 问题。
```

这对我们很重要。

因为最终真正强的 Research OS 不应该只会调用别人的模型，而应该逐渐积累：

```text
自己的 research memory
自己的 idea data
自己的 reward signals
自己的 benchmark
自己的 research loop
自己的 open-source project
```

DeepInnovator 正好对应其中的：

```text
idea data + reward signals + model training recipe
```

这就是它在 Pengyi Research OS 里的位置。
