---
title: "HKUDS028: RecLM 作为 Recommendation Instruction Tuning 与 Profile-Augmented Ranking Layer"
date: 2026-06-28 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds028, hkuds, reclm, recommendation, instruction-tuning, ranking, profile-augmentation, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第二十九篇。

```text
HKUDS028 -> RecLM
```

上一篇 `HKUDS027` 完成了图智能主线：

```text
HKUDS024 GraphAgent -> agentic graph language assistant
HKUDS025 OpenGraph  -> graph foundation model
HKUDS026 GraphGPT   -> graph instruction tuning
HKUDS027 HiGPT      -> heterogeneous graph language model
```

现在进入第三张学习地图里的下一条主线：

```text
Recommendation / Finance-adjacent
```

第一篇就是：

```text
RecLM: Recommendation Instruction Tuning
```

这条线对我们做 Quant Research OS 很关键。

因为量化研究里有很多问题本质上不是生成文本，而是：

```text
排序
推荐
选择
匹配
解释
```

例如：

```text
推荐下一个值得研究的因子。
推荐适合当前 regime 的策略。
推荐和当前 idea 相似的历史实验。
推荐需要 PM 重点审核的风险点。
推荐该读的 paper / repo / dataset。
```

这些都和 recommendation system 的思想高度一致。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `RecLM`。

| Item | Value |
|---|---|
| repo | `RecLM` |
| remote | `https://github.com/HKUDS/RecLM.git` |
| branch | `main` |
| local head | `2c34ae2` |
| full commit | `2c34ae2c5050abe74c1cfaea64bab6ac1928614a` |
| latest local commit date | `2025-06-02 17:45:53 +0800` |
| latest local commit | `Update README.md` |
| status | clean, synced with `origin/main` after fetch |
| paper | `RecLM: Recommendation Instruction Tuning`, arXiv `2412.19302` |
| venue | ACL 2025 main conference |
| authors | Yangqin Jiang, Yuhao Yang, Lianghao Xia, Da Luo, Kangyi Lin, Chao Huang |
| tracked files by `rg --files` | 112 |
| Python files | 44 |
| Markdown files | 3 |
| JSON files | 12 |
| Python syntax check | `py -m compileall -q TextEncoder.py llm base_models` passed |

项目结构很清楚：

```text
base_models/   traditional / neural recommenders
data/          MIND and Netflix interaction/profile artifacts
llm/lora/      Llama-2 LoRA SFT, collaborative tuning, inference, RLHF
sft_data/      instruction tuning and RLHF datasets
TextEncoder.py profile text -> embedding
```

`base_models/` 里有五类推荐模型：

```text
BiasMF
LightGCN
NCF
SGL
SimGCL
```

这说明 RecLM 不是只做一个 isolated LLM demo。

它的目标是：

```text
把 LLM 生成的 profile 作为 plug-and-play component 接入已有推荐系统。
```

## 一句话定位

RecLM 是一个：

```text
recommendation instruction tuning framework
```

更具体地说，它是：

```text
LLM profile generator
+ collaborative instruction tuning
+ RLHF profile enhancement
+ user/item profile embeddings
+ base recommender augmentation
```

它不是让 LLM 直接端到端输出推荐列表。

它更现实：

```text
LLM 负责生成和增强 user / item profile。
推荐模型负责 ranking。
```

这点非常重要。

因为在工业推荐、量化排序、研究任务推荐里，LLM 最强的地方不一定是直接算分，而是：

```text
从稀疏、噪声、文本、历史行为中抽象出更好的 profile。
```

然后把 profile 变成 feature，交给 ranking model 使用。

## RecLM 想解决什么问题

README 里讲了三个关键问题。

第一，推荐系统在 low-data / cold-start 场景泛化差。

用户或 item 的历史交互少时，传统 CF 模型容易没有足够信号。

第二，用户 profile 质量低。

LLM 可以根据历史行为和 item 文本生成自然语言 profile，但 naive profile 容易：

```text
噪声大
泛化差
过度平滑
和真实 interaction objective 不一致
```

第三，LLM 和 CF 需要结合。

RecLM 的思路不是让 LLM 凭空推荐，而是让 LLM 学会 collaborative filtering 的任务形式：

```text
user history
neighbor users
item metadata
target item
interaction prediction
```

也就是说，LLM 要学会：

```text
用户画像不是纯文本摘要，而是服务于未来交互预测的中间表示。
```

这对 Quant OS 很有启发。

我们未来生成 factor / strategy profile，也不能只是好看的总结。

它必须服务于：

```text
future return prediction
risk diagnosis
strategy selection
PM review
research prioritization
```

## 总体架构

RecLM 可以拆成五层。

```text
Layer 1: interaction data and item metadata
Layer 2: instruction data construction
Layer 3: LLM profile generation / refinement
Layer 4: profile embedding
Layer 5: profile-augmented recommender
```

### Layer 1: Interaction Data

项目使用 MIND、Netflix 和 Industrial data。

README 给出的数据统计包括：

| Dataset | Users | Train Items | Train Interactions | Train Sparsity |
|---|---:|---:|---:|---:|
| MIND | 57,128 | 2,386 | 89,734 | 99.934% |
| Netflix | 16,835 | 6,532 | 1,655,395 | 98.495% |
| Industrial | 117,433 | 152,069 | 858,087 | 99.995% |

这三个数据集都高度稀疏。

所以 RecLM 的核心背景就是：

```text
sparse interaction + rich text/profile signal
```

量化里也是一样。

真正可验证的好策略样本很少，但旁边有大量文本、研究记录、回测日志、市场事件、宏观背景。

### Layer 2: Instruction Data

`sft_data/` 里有三类关键数据。

```text
netflix_hf / mind_hf
cf_instruction_data.csv
item_side_instruction_data.csv
rlhf/train.csv, rl.csv, eval.csv
```

`cf_instruction_data.csv` 的一条样本包含：

```text
UID
Input1
Response1
Input2
Response2
```

这代表两阶段任务。

第一阶段：

```text
Input1:
  给 target user 的 interaction list。
  给 neighbor users 的 interaction list。
  给 item metadata。

Response1:
  生成 target user 和 neighbor users 的 profile。
```

第二阶段：

```text
Input2:
  基于 user preferences 和 item information，
  用 collaborative filtering 判断 target user 是否会和 target item interaction。

Response2:
  Yes / No
```

这很关键。

RecLM 不是只让 LLM 生成 profile，而是把 profile generation 和 downstream interaction prediction 放在同一条 instruction 里。

这让 profile 更接近 recommender objective。

### Layer 3: LLM SFT

LoRA SFT 主要在：

```text
llm/lora/sft_base.py
llm/lora/sft_base_mask.py
llm/lora/sft_base_item.py
llm/lora/sft_base_naive.py
```

`sft_base.py` 是基础 user-side profile tuning。

它使用：

```python
model_id = "meta-llama/Llama-2-7b-chat-hf"
BitsAndBytesConfig(load_in_4bit=True, ...)
LoraConfig(r=8, lora_alpha=16, lora_dropout=0.1)
SFTTrainer(...)
```

也就是：

```text
Llama-2-7B-Chat
+ 4-bit quantization
+ LoRA
+ SFT
```

`sft_base_mask.py` 更有意思。

它显式构造两阶段 collaborative instruction：

```text
Stage 1:
  generate user preference profile based on history.

Stage 2:
  use the generated profile and similar users' interactions
  to predict whether target user will interact with target item.
```

代码里的 system prompt 也直接说明：

```text
You are a recommendation system capable of predicting user-item interactions based on collaborative filtering.
```

它的训练标签 mask 也很明确：

```text
Input 部分 label = ignore_index
Response 部分参与 loss
```

所以它不是普通文本续写，而是 supervised instruction tuning。

`sft_base_item.py` 则是 item-side profile generator。

它给 LLM：

```text
target item text information
similar items
targeting users' profiles
```

然后要求生成：

```text
item 的目标用户画像
```

这对应推荐系统里的 item profile。

对 Quant OS 来说，这可以平移为：

```text
strategy-side profile:
  这个策略适合哪些 market regime / asset / risk preference？

factor-side profile:
  这个因子适合哪些资产、行业、时间尺度和风险环境？
```

### Layer 4: RLHF Profile Enhancement

RLHF 相关代码在：

```text
llm/lora/rlhf/reward_modeling.py
llm/lora/rlhf/rl_training.py
llm/lora/rlhf/accuracy.py
```

`reward_modeling.py` 读取：

```text
query
chosen
rejected
```

然后训练一个 sequence classification reward model。

它使用：

```python
AutoModelForSequenceClassification
RewardTrainer
LoraConfig(task_type="SEQ_CLS")
```

`rl_training.py` 再使用：

```python
AutoModelForCausalLMWithValueHead
PPOTrainer
reward model pipeline
```

流程是：

```text
profile generator 生成 response
reward model 给 profile 打分
PPO 根据 reward 更新 generator
```

这一步的系统意义是：

```text
profile 质量不能只靠 SFT，要用偏好/反馈进一步校准。
```

对我们来说，这可以对应：

```text
PM preference
research success feedback
backtest validation
risk review outcome
```

如果我们未来做 Quant R&D Agent，profile / plan / diagnosis 都可以被 reward model 约束。

### Layer 5: Profile-Augmented Recommender

RecLM 最现实的部分在 `base_models/`。

这里不是 LLM 自己做所有推荐，而是把 LLM 生成的 profile embedding 接入传统推荐模型。

以 `base_models/LightGCN/Model.py` 为例。

模型里有：

```python
self.trans_profile = nn.Sequential(...)
self.trans_original = nn.Sequential(...)
self.concat_layer = nn.Sequential(...)
```

`DataHandler.py` 会读取：

```text
item_original_features.npy
item_profile/item_profile_embeddings.npy
user_profile/user_profile_embeddings.npy
```

当：

```text
--user_aug 1
```

时，模型会使用 profile features。

LightGCN 里的逻辑大致是：

```python
item_feats = trans_original(item_original_features)
item_profile_feats = trans_profile(item_profile_embeddings)
item_feats = concat_layer([item_feats, item_profile_feats])

user_feats = trans_profile(user_profile_embeddings)
user_embeds = concat_layer([learned_user_embedding, user_feats])
```

也就是说：

```text
LLM profile -> embedding -> recommender feature augmentation
```

这就是 RecLM 的工程核心。

它非常适合迁移到量化。

## RecLM 的系统价值

RecLM 最值得学的不是某个具体模型，而是一种系统范式：

```text
LLM 不直接替代 ranking model。
LLM 生成更好的 semantic profile。
Ranking model 使用 profile 做更稳的排序。
```

这比“让 LLM 直接选股票”靠谱得多。

在 Quant OS 里，我们可以这样类比：

| Recommendation | Quant Research OS |
|---|---|
| user | researcher / PM / portfolio / strategy consumer |
| item | factor / strategy / paper / dataset / repo / trade idea |
| user profile | researcher preference / mandate / risk appetite |
| item profile | factor profile / strategy profile / paper profile |
| interaction | click / purchase / rating |
| research interaction | studied / backtested / accepted / rejected / deployed |
| reward | profile preference |
| quant reward | backtest quality / PM approval / live performance |

所以 RecLM 可以启发我们做：

```text
Research Recommendation Layer
```

它回答：

```text
下一个该看哪个 repo？
下一个该读哪篇 paper？
哪个因子 idea 最值得做？
哪个策略适合当前市场环境？
哪个 backtest 异常最值得诊断？
哪个 PR opportunity 最适合我们？
```

## 和前面 HKUDS 主线的连接

RecLM 接在 Graph/Knowledge 主线后面很自然。

前面我们有：

```text
LightRAG / MiniRAG / VideoRAG -> knowledge ingestion
GraphAgent / OpenGraph / GraphGPT / HiGPT -> graph-language representation
OpenSpace / OpenHarness / AnyTool -> agent workspace and runtime
```

但系统还需要一个东西：

```text
which object should we act on next?
```

这就是 recommendation。

如果 Research OS 只是把知识存起来，还不够。

它还要能推荐：

```text
下一步动作
下一篇文章
下一个实验
下一个可复用 skill
下一个需要 PM review 的风险
```

RecLM 是这条 recommendation intelligence 的第一块。

## 对 Quant Research OS 的直接启发

### 1. Factor Profile Generator

我们可以让 LLM 为每个因子生成 profile。

输入：

```text
factor formula
economic intuition
historical performance
turnover
coverage
industry exposure
regime performance
failure cases
```

输出：

```text
Factor identity:
  value / momentum / quality / sentiment / microstructure / event-driven

Factor interests:
  works in which regimes
  vulnerable to which risks
  overlaps with which known factors
```

这相当于 RecLM 的 user profile / item profile。

### 2. Strategy Profile Generator

每个 strategy 也可以有 profile。

```text
Strategy identity:
  short-horizon stat-arb
  medium-horizon factor allocation
  news-driven event model
  portfolio overlay

Strategy interests:
  preferred asset universe
  holding period
  liquidity constraint
  risk budget
  rebalance frequency
```

### 3. Researcher / PM Profile

Research OS 还可以记录人类 PM 的偏好：

```text
喜欢高解释性还是高 Sharpe？
能接受多大 turnover？
偏好 alpha idea 还是 infrastructure work？
更关注 publishability 还是 deployability？
```

这会帮助系统推荐下一步研究。

### 4. Profile-Augmented Ranking

最终不是让 LLM 直接决定。

我们可以像 RecLM 一样做：

```text
semantic profile features
+ numeric performance features
+ graph features
+ validation status
-> ranking model
```

然后输出：

```text
research priority score
deployment readiness score
PM review priority
paper/repo reading priority
```

这就是 Quant OS 的 recommendation layer。

## 一个轻量版实现路线

短期我们不需要训练 Llama-2-7B。

可以先做 v0。

### Stage 1: Define Objects

先定义可推荐对象：

```text
paper
repo
factor
strategy
dataset
experiment
PR opportunity
```

### Stage 2: Generate Profiles

用现成 LLM 生成 profile：

```text
object identity
object use case
required capability
risk
expected payoff
related objects
```

### Stage 3: Embed Profiles

用 embedding model 编码：

```text
profile text -> vector
```

### Stage 4: Build Interaction Log

记录：

```text
viewed
studied
implemented
backtested
published
accepted by PM
rejected by PM
```

### Stage 5: Rank Next Actions

先不用复杂模型。

可以从 hybrid score 开始：

```text
semantic match
+ historical success similarity
+ novelty
+ feasibility
+ strategic value
+ PM priority
```

等数据多了，再考虑 RecLM-style SFT / reward model。

## 可以提 PR 的地方

RecLM 代码很有研究价值，但也有一些具体可改进点。

第一，很多脚本硬编码 GPU。

例如：

```python
os.environ['CUDA_VISIBLE_DEVICES'] = '2'
os.environ["CUDA_VISIBLE_DEVICES"] = "3"
```

这些应该改成 CLI 参数或环境变量读取。

第二，很多路径硬编码为 Netflix 或 MIND。

例如：

```python
load_from_disk("./../../sft_data/netflix/netflix_hf")
load_dataset("csv", data_files="./../../sft_data/netflix/cf_instruction_data.csv")
```

可以统一成：

```text
--dataset netflix
--sft_data_dir ...
--output_dir ...
```

第三，`TextEncoder.py` 还是 placeholder path。

例如：

```python
with open('./path/to/file/dict.pkl', 'rb') as f:
np.save('./path/to/file/embedding.npy', indexed_array)
```

这个很适合改成 CLI 工具。

第四，README 里有一些拼写问题。

例如：

```text
instrucntion
limition
devided
mentiond
```

这些属于低风险 docs PR。

第五，项目没有顶层 `requirements.txt`。

README 只列了环境版本，但没有可直接安装的 requirements 文件。可以补：

```text
requirements-base.txt
requirements-llm.txt
```

第六，五个 base recommender 目录里有大量重复结构。

例如：

```text
DataHandler.py
Params.py
Utils/
Main.py
```

如果后续维护，可以抽公共模块，减少重复。

第七，`rl_training.py` 默认路径似乎还指向旧目录。

例如：

```python
dataset_name="./../../../data/netflix/rlhf/netflix_data_v0/rl.csv"
```

而 repo 里实际有：

```text
sft_data/netflix/rlhf/rl.csv
```

这个需要确认运行路径，但从代码和 repo 结构看，容易导致复现失败。

第八，README 可以加一张 end-to-end pipeline。

现在 README 有图，但如果补一个 command-level flow 会更实用：

```text
SFT user profile -> collaborative instruction tuning -> reward model -> PPO -> inference -> TextEncoder -> base recommender
```

这些 PR 都不需要改核心算法，适合从 reproducibility / docs / CLI ergonomics 入手。

## 和 XRec 的关系

下一篇 `HKUDS029 -> XRec` 很适合作为 RecLM 的延续。

RecLM 重点是：

```text
profile generation + profile-augmented recommendation
```

XRec 听名字和 README 初步定位更偏：

```text
LLM for explainable recommendation
```

所以两者可以连成：

```text
RecLM -> 让推荐更准、更能 cold-start
XRec  -> 让推荐更可解释、更能被人类理解
```

量化里也一样。

我们不仅要推荐策略，还要解释：

```text
为什么推荐？
相似历史案例是什么？
风险在哪里？
PM 应该如何审核？
```

## 总结

`HKUDS028 RecLM` 打开了 recommendation / ranking 主线。

它最重要的启发是：

```text
LLM 生成 profile。
Profile 进入 ranking model。
Ranking model 负责推荐。
Feedback 再反向优化 profile generator。
```

这比单纯让 LLM 直接做推荐更稳。

对我们的 Quant Research OS，这一篇对应：

```text
research idea recommendation
factor / strategy profile generation
PM preference modeling
profile-augmented ranking
next-action recommendation
```

如果前面的 GraphGPT / HiGPT 解决的是：

```text
如何理解结构化关系？
```

那么 RecLM 解决的是：

```text
理解之后，下一步应该推荐什么？
```

这正是 Research OS 从 memory / graph 进入 decision / ranking 的关键一步。

## 下一步

按路线继续：

```text
HKUDS029 -> XRec
```

重点看：

```text
large language models for explainable recommendation
```

这会补上 RecLM 后面的解释层。
