---
title: "HKUDS046: LightReasoner 作为 Reasoning Efficiency、Expert-Amateur Teaching 与 Research OS Skill Distillation Layer"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds046, hkuds, lightreasoner, reasoning, efficient-training, expert-amateur, contrastive-learning, skill-distillation, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS046`。

```text
HKUDS046 -> LightReasoner
```

上一篇是：

```text
HKUDS045 -> CatchMe
```

`CatchMe` 讲的是：

```text
personal digital footprint -> hierarchical memory -> agent-queryable context
```

它是 personal context engine。

这一篇看 `LightReasoner`。

`LightReasoner` 讲的是：

```text
small / amateur model
  -> identify critical reasoning tokens
  -> teach expert model where to improve
  -> get reasoning gains with very few tuned tokens
```

一句话定位：

```text
LightReasoner = Expert-Amateur contrast
              + KL-guided critical token selection
              + contrastive soft-label supervision
              + LoRA fine-tuning
              + reasoning efficiency layer
```

这篇进入的是：

```text
Reasoning / Efficiency / Skill Distillation
```

这条主线和前面的 agent product 不一样。

前面的 `ClawWork / FastAgent / CatchMe / ViMax` 更像产品系统。

`LightReasoner` 更像一个训练方法：

```text
怎么用更少数据、更少 token、更少时间，让模型的 reasoning 变强。
```

## 为什么 HKUDS046 看 LightReasoner

我们现在已经有几层 Research OS 图景：

```text
CatchMe       -> memory/context layer
ViMax         -> multimodal production layer
FastAgent     -> execution layer
OpenHarness   -> agent runtime layer
Litewrite     -> writing layer
DeepResearch  -> research workflow layer
```

但一个真正的 AI Scientist OS 还缺一个关键层：

```text
reasoning improvement layer
```

也就是：

```text
agent 如何通过经验变聪明？
模型如何知道自己该在哪些推理点上学习？
训练时如何避免把算力浪费在无关 token 上？
```

`LightReasoner` 正好回答这个问题。

它的核心思想非常适合我们：

```text
不要平均学习全部轨迹。
要识别真正产生推理差异的关键步骤。
```

这对 research agent、quant R&D agent 都很关键。

研究里也是这样：

```text
不是所有笔记都同等重要。
不是所有代码修改都同等重要。
不是所有 backtest 输出都同等重要。
真正重要的是少数 decision point / failure point / insight point。
```

LightReasoner 给的是一种训练上的 analog：

```text
critical token selection
```

## 本次阅读状态

本次阅读的是本地 HKUDS 工作区里的 `LightReasoner`。

阅读前执行了远端同步，当前已经是最新 `origin/main`。

| item | value |
| --- | --- |
| repo | `LightReasoner` |
| remote | `https://github.com/HKUDS/LightReasoner.git` |
| branch | `main` |
| latest commit | `fbd55c7dd105dde45f87c621006b03b7668fe279` |
| commit date | `2026-05-22T12:07:45+08:00` |
| latest message | `more` |
| license | MIT |
| Python | README says `3.10+` |

本地文件规模：

| type | count |
| --- | ---: |
| tracked files | 32 |
| Python | 12 |
| PNG assets | 10 |
| Markdown | 4 |
| requirements | 1 |
| zip sample data | 1 |

`LRsamples/LR_samples.zip` 大约：

```text
25 MB
```

仓库里有一个 submodule 配置：

```text
evaluation/Qwen2.5-Math -> https://github.com/QwenLM/Qwen2.5-Math.git
```

本地 submodule 当前显示为未初始化状态。

本地验证：

```text
py -m compileall -q data_prep.py LightR_sampling.py LightR_finetuning.py merge.py analysis evaluation
```

结果：

```text
passed
```

依赖检查：

```text
transformers
accelerate
datasets
peft
torch
```

当前本机 Python 环境未安装这些训练依赖，所以没有执行实际 sampling / finetuning / evaluation。

## 项目用途

README 的主问题是：

```text
Can small language models teach large language models reasoning?
```

这句话很反直觉。

通常我们想的是：

```text
large model teaches small model
```

也就是 distillation。

LightReasoner 反过来问：

```text
small / amateur model 能不能帮助 large / expert model 找到它应该学习的推理点？
```

它不是说小模型比大模型聪明。

更准确地说：

```text
amateur model acts as a contrastive lens.
```

小模型的作用是制造对比。

当 expert 和 amateur 在某个推理前缀下的下一 token 分布差异很大时，这个位置可能就是关键推理步骤。

因此，LightReasoner 不是传统 SFT。

它的目标是：

```text
用 Expert-Amateur 分布差异找关键 token。
只在这些关键 token 上训练 expert。
```

这就是它的效率来源。

## 它解决的问题

README 里把传统 SFT 的问题概括成三类：

```text
Data-intensive
Uniform learning
Ground-truth dependency
```

对应到训练流程里：

| 问题 | 含义 |
| --- | --- |
| Data-intensive | 需要大量人工标注或 rejection-sampled 轨迹 |
| Uniform learning | 每个 token 都同等训练，很多 token 其实没有推理价值 |
| Ground-truth dependency | 强依赖正确答案和完整示范，不利于迁移到新领域 |

LightReasoner 的判断是：

```text
我们把大量算力花在模型已经会的 token 上。
真正重要的是少数高影响力推理步骤。
```

这非常像研究。

研究系统里同样有大量低价值过程：

```text
格式修改
重复搜索
普通代码整理
重复实验
无信息量日志
```

真正让系统进步的是：

```text
错误定位
关键假设
决策分叉
反例分析
模型行为差异
新的解释路径
```

LightReasoner 训练模型的方法，对我们做 Research OS 有直接启发。

## 方法总览

LightReasoner 的训练分两阶段：

```text
Sampling Stage
Fine-tuning Stage
```

Sampling Stage 做：

```text
1. Expert model 生成推理轨迹
2. 对每个 prefix，取 Expert 的 next-token distribution
3. Amateur model 在相同 prefix 上预测 next-token distribution
4. 计算 Expert-Amateur KL divergence
5. 保留 KL divergence 高于阈值 beta 的关键步骤
6. 在 plausible token set 内构造 contrastive soft label
7. 写出 LightReasoner training samples
```

Fine-tuning Stage 做：

```text
1. 加载 Expert base model
2. 给 Expert 加 LoRA adapter
3. 用 sampling stage 生成的 soft labels 训练
4. loss 是 KL(log_probs, soft_labels)
5. 训练后可以 merge LoRA 到 base model
```

核心逻辑非常清楚：

```text
identify where expert differs from amateur
train expert to reinforce expert-side distributional advantage
```

## Sampling Script

核心文件：

```text
LightR_sampling.py
```

它的输入是：

```text
Expert model path
Amateur model path
input JSONL
output JSONL
checkpoint JSONL
alpha
beta
max_new_tokens
batch_size
```

默认概念参数：

```text
max_new_tokens = 128
alpha = 0.2
beta = 0.4
batch_size = 64
```

其中：

| 参数 | 作用 |
| --- | --- |
| `alpha` | plausibility threshold factor，用来过滤 expert distribution 里的低概率 token |
| `beta` | KL divergence threshold，只保留 Expert-Amateur 差异足够大的步骤 |
| `max_new_tokens` | Expert 生成推理轨迹的最大长度 |
| `batch_size` | Amateur 前缀批量 forward 的大小 |

### Expert 生成轨迹

脚本先让 expert model 根据 question 生成推理轨迹：

```text
expert_model.generate(...)
output_scores=True
return_dict_in_generate=True
```

因此它不仅得到 generated tokens，也得到每一步的 logits / scores。

生成结果被拆成：

```text
expert_targets
expert_probs
```

`expert_targets` 是 expert 生成出来的 token 序列。

`expert_probs` 是 expert 每一步的 next-token probability distribution。

### Amateur 预测每个 Prefix

对每个 step，脚本构造：

```text
system prompt
user question
assistant prefix
```

然后让 amateur model 在这个 prefix 上预测 next token。

为了效率，它先把所有 prefix 放入 `input_ids_batch`，再分 batch 送进 amateur model。

这一步的本质是：

```text
同一个 reasoning prefix 下，Expert 和 Amateur 对下一步该怎么走的看法有什么差异？
```

这个差异就是 LightReasoner 的信号。

## KL Divergence

核心计算：

```text
P_E = expert distribution
P_A = amateur distribution
KL(P_E || P_A)
```

代码里使用：

```text
F.kl_div(P_A.log(), P_E, reduction='sum')
```

当 KL 很高时，说明：

```text
Expert 认为某些 token/方向很重要，
Amateur 没有给出同样分布。
```

这就是一个潜在的 critical reasoning step。

这里要注意一个实现细节。

代码会先做：

```text
min_vocab_size = min(expert_dist.size(0), amateur_dist.size(0))
expert_dist = expert_dist[:min_vocab_size]
amateur_dist = amateur_dist[:min_vocab_size]
```

这要求 Expert 和 Amateur 的 tokenizer / vocab ID mapping 足够对齐。

仓库也提供了：

```text
analysis/testspace/check_vocab_alignment.py
```

用于检查 tokenizer ID 到 token 的映射是否一致。

这是一个很重要的 reproducibility guard。

如果两个模型 vocab 不对齐，直接按 index 做 KL 就可能错。

## Plausible Token Set

LightR_sampling 不是对全 vocab 的 token 都产生 label。

它先选 plausible token set：

```text
plaus_thresh = alpha * max(expert_dist)
mask = expert_dist >= plaus_thresh
```

这意味着：

```text
只看 Expert 自己认为还有可能的 token。
```

这个设计很重要。

否则大 vocabulary 里大量低概率 token 会制造噪音。

在 plausible token set 里，再计算 contrastive score：

```text
log_PE - log_PA
```

然后：

```text
weights = softmax(log_PE - log_PA)
```

这就是 soft-label 的来源。

直觉是：

```text
Expert 高、Amateur 低的 token 更能代表 Expert 的优势。
```

## 输出数据格式

Sampling 输出的每条数据包括：

```text
prompt_id
step
prefix
tokens
token_ids
weights
kl_divergence
```

这里的 `prefix` 是当前已经生成的 assistant prefix。

`token_ids` 是 plausible set 中保留下来的 token IDs。

`weights` 是 contrastive soft label。

这和普通 SFT 数据不一样。

普通 SFT 是：

```text
question -> full correct answer trajectory
```

LightReasoner 是：

```text
question + prefix -> soft distribution over selected next tokens
```

也就是：

```text
single next-token prediction instance
```

这是它 tuned tokens 少的根本原因。

## Fine-Tuning Script

核心文件：

```text
LightR_finetuning.py
```

它由四部分组成：

```text
ContrastiveSoftLabelDataset
load_lora_model
SoftLabelKLTrainer
main training config
```

### Dataset

`ContrastiveSoftLabelDataset` 读取 sampling 生成的 JSONL。

每个样本会重建：

```text
system: Please reason step by step.
user: question
assistant prefix
```

然后构造一个长度为 vocab size 的 soft label vector：

```text
labels = zeros(vocab_size)
labels[token_id] = weight
```

因此每条样本的监督目标不是单一 token。

而是一个 sparse probability distribution。

### LoRA

`load_lora_model` 使用 PEFT：

```text
r = 8
lora_alpha = 16
target_modules = ["q_proj", "v_proj"]
lora_dropout = 0.05
bias = "none"
task_type = CAUSAL_LM
```

这是一个轻量微调设置。

也符合项目定位：

```text
not full fine-tuning
but selective LoRA update
```

### KL Trainer

`SoftLabelKLTrainer` 自定义 `compute_loss`。

核心是：

```text
logits = model(...).logits
logits = logits[:, -1, :vocab_size]
log_probs = log_softmax(logits)
loss = kl_div(log_probs, soft_labels)
```

也就是说，它只训练最后一个 next-token prediction。

这和 sampling 数据格式完全对应。

这一步体现 LightReasoner 的本质：

```text
train only selected critical next-token decisions
```

不是整条 chain-of-thought 轨迹全部训练。

## Merge Script

`merge.py` 做：

```text
load base model
load LoRA adapter
merge_and_unload
save merged model
save tokenizer
```

这样训练后模型可以作为 standalone model 使用。

这点很实用。

因为如果每次推理都依赖 base + LoRA adapter，会增加部署复杂度。

## Data Prep

`data_prep.py` 支持：

```text
GSM8K
MATH
```

GSM8K 部分会输出：

```text
gsm8k_train.jsonl
gsm8k_test.jsonl
```

MATH 部分会加载：

```text
algebra
counting_and_probability
geometry
intermediate_algebra
number_theory
prealgebra
precalculus
```

输出：

```text
math_train.jsonl
math_test.jsonl
```

README 里强调默认使用 GSM8K。

原因是：

```text
GSM8K 的 reasoning 更通用，步骤更自然，amateur model 即使没有数学专项训练也能生成可解释输出。
```

如果用 MATH 这类更难的数据集，README 建议 amateur model 也要更专业，例如换成 Qwen2.5-Math 系列。

这点非常关键：

```text
amateur 不能太弱。
amateur 太弱就没有有意义的 contrast。
```

## Analysis Scripts

仓库里有一组 analysis 脚本。

它们说明作者不是只做主流程，也在分析为什么这种方法有效。

| 文件 | 作用 |
| --- | --- |
| `analysis/preliminary_analysis.py` | 对 Expert-Amateur 每一步计算 KL、entropy、top-1 token |
| `analysis/pre_analysis_stats.py` | 对 KL log 做统计和 histogram |
| `analysis/PPL_analysis.py` | 合并不同 checkpoint 后计算 perplexity |
| `analysis/case_study.py` | 比较 base 和 fine-tuned 模型的评测输出，找 improved cases |
| `analysis/testspace/check_vocab_alignment.py` | 检查 Expert/Amateur tokenizer ID mapping 是否一致 |
| `analysis/testspace/early_test.py` | 早期 sanity check，验证 tokenizer 和 next-token prediction |
| `analysis/testspace/llama_KL.py` | 在 Llama 模型上探索 shared vocabulary KL |

这里有一个很重要的启发：

```text
训练方法项目不能只给训练脚本。
还要给分析脚本。
```

因为方法论文最关键的是解释机制。

LightReasoner 的机制解释依赖：

```text
KL tail
Expert confident / Amateur uncertain
top-1 match vs mismatch
case improvements
PPL changes
```

这和我们做 Research OS 也一样。

不能只产出模型或策略。

还要产出：

```text
diagnosis
case study
failure mode
ablation
mechanism evidence
```

## Evaluation

`evaluation/README.md` 指向 Qwen2.5-Math evaluation toolkit。

流程是：

```text
clone Qwen2.5-Math
install evaluation dependencies
configure eval script
run math_eval.py
```

核心评测字段包括：

```text
model_name_or_path
data_name
prompt_type
temperature
n_sampling
top_p
use_vllm
save_outputs
```

README 主表里覆盖 7 个 benchmark：

```text
GSM8K
MATH
SVAMP
ASDiv
Minerva Math
Olympiad Bench
MMLU STEM
```

这说明 LightReasoner 的 claim 不只是 GSM8K 训练集内有效。

它强调：

```text
train only on GSM8K
generalize across multiple reasoning benchmarks
```

## 主要结果

README 中给出的核心结论包括：

```text
Qwen2.5-Math-1.5B:
  GSM8K +28.1
  MATH +25.1
  SVAMP +7.2
  ASDiv +11.7

DeepSeek-R1-Distill-Qwen-1.5B:
  GSM8K +4.3
  MATH +6.0
  OlympiadBench +17.4

Qwen2.5-Math-7B:
  GSM8K +10.4
  MATH +6.0
  SVAMP +9.3
  ASDiv +7.9
```

效率对比中，README 强调：

```text
90% less total time
80% fewer sampled problems
99% fewer tuned tokens
```

以 Qwen2.5-Math-1.5B 为例：

```text
SFT:
  4.0h
  3952 sampled problems
  1.77M tuned tokens
  +7.7 average gain

LightReasoner:
  0.5h
  1000 sampled problems
  0.02M tuned tokens
  +11.8 average gain
```

这里最值得学的不是具体数值。

更重要的是方法论：

```text
training efficiency comes from focusing update budget on high-leverage steps.
```

## Expertise Over Scale

LightReasoner 很有意思的一点是：

```text
contrast is driven by expertise gap, not simply model size gap.
```

README 里强调：

```text
Domain expertise over scale
```

这对我们非常重要。

因为未来做 quant / research agent，不一定总是最大模型最好。

我们可能需要的是：

```text
general model
specialized model
weak-but-coherent model
expert domain model
```

它们之间形成对比。

例如：

```text
general reasoning model vs quant-specialized model
coding model vs research model
PM-style reviewer vs engineer-style implementer
student model vs expert model
```

关键不是谁更大。

关键是：

```text
哪个差异能产生有效训练信号。
```

这对 R&D Agent 的 multi-agent design 也有启发。

## 和传统 SFT 的区别

传统 SFT：

```text
train on full trajectories
optimize every token
需要正确轨迹
```

LightReasoner：

```text
train on selected next-token decisions
optimize critical tokens
不直接依赖人工 ground truth label
```

它不是让模型背完整答案。

它让模型在关键推理点上更像 expert 的优势分布。

这就是：

```text
selective reasoning correction
```

从系统角度看，LightReasoner 是一种：

```text
bottleneck-focused learning
```

## 和 Contrastive Decoding 的区别

README 里也对比了 Contrastive Decoding。

传统 CD 更多是 inference-time：

```text
expert logits - amateur logits
用在生成时
```

LightReasoner 的区别是：

```text
把 contrastive signal 用在 training time
```

因此训练后的模型可以 standalone inference。

这点非常重要。

如果一个方法必须 inference-time 同时加载 expert 和 amateur，它部署成本就高。

LightReasoner 的思路是：

```text
use contrast during training
internalize contrast into expert model
then deploy only enhanced expert
```

这对真实系统更实用。

## 和 UpSkill 的关系

我们之前看过：

```text
HKUDS016 UpSkill
HKUDS039 UpSkill Revisited
```

UpSkill 讲的是：

```text
failure -> skill
agent 从失败中提炼可复用能力
```

LightReasoner 讲的是：

```text
expert-amateur distribution gap -> critical token
model 从差异中提炼 reasoning improvement
```

两者其实很像。

它们都是：

```text
找到高信息量的失败/差异点
把它转成训练信号
形成可复用能力
```

区别是：

| 项目 | 粒度 |
| --- | --- |
| UpSkill | agent behavior / task skill |
| LightReasoner | token-level reasoning decision |

所以在 Research OS 里，它们可以合并成一个思想：

```text
Skill Distillation Layer
```

## 和 CatchMe 的关系

`CatchMe` 记录用户做了什么。

`LightReasoner` 识别模型应该在哪些 token 上学习。

组合起来很有意思：

```text
CatchMe records research traces.
LightReasoner-style methods identify high-signal reasoning points.
```

如果我们把 research trace 结构化成：

```text
decision
mistake
fix
review
insight
```

那么后续 agent 就可以学习：

```text
哪些 research decision point 是高价值训练样本？
哪些 coding failure 最值得蒸馏成 skill？
哪些 quant backtest diagnosis 是关键经验？
```

这就是 Research OS 的复利系统。

## 和 AI-Researcher / DeepInnovator 的关系

`AI-Researcher` 和 `DeepInnovator` 更关注：

```text
idea generation
scientific discovery
research innovation
```

但 idea generation 不只是生成更多想法。

真正难的是：

```text
让模型学会更好的研究判断。
```

LightReasoner 给了一个训练范式：

```text
用 expert/amateur 的差异去发现模型判断的薄弱点。
```

未来可以想象：

```text
Expert researcher model
Amateur researcher model
输入同一个 paper / problem
比较它们在 idea selection / critique / experiment design 上的分歧
把高价值分歧转成训练数据
```

这就是 AI Scientist training loop。

## 和 Quant OS 的关系

LightReasoner 表面上是数学 reasoning 训练。

但它对 Quant OS 很有启发。

量化研究不是只需要更多数据。

它更需要：

```text
识别关键判断点
识别错误归因
识别信号与噪音
识别 backtest 里的假提升
识别 factor 为什么失效
```

我们可以把 LightReasoner 的思想迁移到 Quant R&D：

```text
expert quant model
amateur quant model
same factor hypothesis
same backtest result
compare reasoning / diagnosis / next action
extract high-divergence decision points
train or update R&D agent
```

例如：

```text
Amateur: factor works because IC is positive.
Expert: factor may be lookahead-biased because turnover spike and universe leakage.
```

这类分歧比普通总结更有训练价值。

它可以成为：

```text
Quant Research Skill Distillation
```

## 和我们的 R&D Agent 的连接

我们之前定义的 R&D Agent：

```text
自动提出因子假设
自动实现
自动回测
自动诊断偏差
自动生成下一轮研究计划
人类 PM 审核
```

LightReasoner 可以嵌入其中的训练闭环：

```text
1. 让多个 agent 对同一个研究样本给出诊断
2. 让 expert / PM / stronger model 给出高质量诊断
3. 比较 expert 和 amateur 的差异
4. 找到 high-divergence research decision
5. 把这些 decision 做成 skill / preference / fine-tuning data
```

这比简单保存成功案例更强。

因为失败和差异往往更有信息量。

## 对 Research OS 的模块启发

LightReasoner 可以映射成 Research OS 里的：

```text
Skill Distillation Layer
```

可能的模块：

```text
pengyi_research_os/
  memory/
  artifact/
  evaluator/
  skill_distillation/
    collect_traces.py
    compare_expert_amateur.py
    identify_critical_steps.py
    build_training_samples.py
    train_skill_adapter.py
    evaluate_skill_gain.py
```

其中 `identify_critical_steps` 不一定是 token-level。

在我们的系统里可以是：

```text
critical paper insight
critical experiment branch
critical code fix
critical factor diagnosis
critical PM review point
```

LightReasoner 给的是底层思想：

```text
learning signal should be concentrated on high-value divergence.
```

## 代码工程状态

这个 repo 更像 research release。

它不是完整 Python package。

没有：

```text
pyproject.toml
setup.py
tests/
configs/
CLI wrapper
```

核心脚本大量使用：

```text
<path_to_expert_model>
<path_to_amateur_model>
<path_to_input_jsonl>
<path_to_output_jsonl>
<device>
<torch_dtype>
```

这很常见。

但如果要复现或工程化，第一步应该是把这些 hard-coded placeholders 迁移到：

```text
YAML config
argparse CLI
example config files
```

也就是：

```text
research script -> reproducible experiment runner
```

## Possible PR Ideas

读完代码以后，有一些比较合适的 PR 方向。

### 1. Config / CLI Refactor

把 `LightR_sampling.py` 和 `LightR_finetuning.py` 的配置从脚本内变量改成：

```text
configs/qwen15_gsm8k.yaml
configs/qwen7_gsm8k.yaml
configs/deepseek15_gsm8k.yaml
```

并支持：

```text
python LightR_sampling.py --config configs/qwen15_gsm8k.yaml --max_questions 1000
python LightR_finetuning.py --config configs/qwen15_gsm8k.yaml
```

这是最实用的工程 PR。

### 2. Tokenizer Alignment Guard

把 `analysis/testspace/check_vocab_alignment.py` 集成到 sampling 前置检查。

例如：

```text
--check_vocab_alignment
--align_by_token_string
--fail_on_vocab_mismatch
```

因为 KL 计算依赖分布对齐。

这个 guard 很重要。

### 3. Requirements / Install Docs

`requirements.txt` 目前包括：

```text
transformers
accelerate
datasets
huggingface_hub
tqdm
peft
```

但没有显式写 `torch`。

真实安装通常还需要根据 CUDA 版本单独安装 PyTorch。

可以补 README：

```text
Install PyTorch separately according to your CUDA version.
```

并给 CPU / CUDA 示例。

### 4. Output Directory Robustness

sampling / finetuning 输出路径如果目录不存在，最好自动创建：

```text
os.makedirs(os.path.dirname(output_path), exist_ok=True)
```

checkpoint path 同理。

这是很小但很实用的 PR。

### 5. Tiny Mock Smoke Test

仓库没有 tests。

可以加一个不依赖大模型的 smoke test：

```text
mock expert distribution
mock amateur distribution
test KL threshold selection
test plausible token mask
test contrastive weight normalization
test soft-label dataset construction
```

这能保证核心数学逻辑不被改坏。

### 6. README-ZH Sync

英文 README 有 2026 ACL / oral presentation 的 news。

中文 README 当前 news 部分没有同步这些条目。

另外中文 README 里 `LRsamples` 段落有重复内容。

这可以作为文档 PR。

### 7. Submodule Docs

仓库有 `.gitmodules` 指向 `evaluation/Qwen2.5-Math`。

但 evaluation README 仍然写手动 clone。

可以统一：

```text
git submodule update --init --recursive
```

或者明确说明为什么不使用 submodule。

### 8. Safer pad_token Handling

sampling / finetuning 里使用：

```text
tokenizer.pad_token_id
```

如果某些 causal LM tokenizer 没有 pad token，需要 fallback：

```text
tokenizer.pad_token = tokenizer.eos_token
```

这是跨模型复现常见问题。

## 实际复现难点

这个项目不是我们当前笔记阶段能轻量跑完的。

主要难点：

```text
需要下载 Expert / Amateur 模型
需要 GPU
需要 torch / transformers / peft / datasets
sampling 阶段要生成 Expert trajectories
amateur 要对大量 prefixes 做 forward
fine-tuning 需要 LoRA training
evaluation 需要 Qwen2.5-Math toolkit / vLLM
```

README 的效率数字是在强 GPU 上测的。

所以我们当前更适合做：

```text
method understanding
code path review
PR-ready engineering cleanup
future reproduction plan
```

而不是马上完整训练。

## 对我们当前阶段的价值

LightReasoner 对我们最大的价值不是“我们马上训练一个数学模型”。

它给我们一个非常强的研究范式：

```text
不要平均用力。
找到关键差异点。
把关键差异点变成训练信号。
```

这可以用在：

```text
paper reading
coding improvement
quant research
factor diagnosis
agent skill learning
application material polishing
interview preparation
```

例如我们准备 RA / PhD：

```text
普通版本 statement
expert版本 statement
比较两者差异
提取 high-value narrative tokens / claims / evidence
反复训练自己的 pitch
```

这其实也是 LightReasoner 思想。

不是全篇平均修改。

而是识别决定质量的关键句、关键证据、关键 framing。

## 最后总结

`HKUDS046 LightReasoner` 的核心结论：

```text
LightReasoner 是一个 token-level reasoning efficiency method。
它用 Expert-Amateur KL divergence 找到关键推理步骤，
再用 contrastive soft-label + LoRA 训练 Expert，
从而用极少 tuned tokens 获得 reasoning improvement。
```

它对 Pengyi Research OS 的映射是：

```text
Skill Distillation Layer
```

它对 Quant OS 的映射是：

```text
Quant Research Decision Distillation Layer
```

它给我们的核心启发：

```text
训练、研究、写作、求职、量化开发都不应该平均用力。
要找到高信息量差异点，然后把它系统化、训练化、复利化。
```

下一篇可以继续：

```text
HKUDS047 -> SepLLM
```

因为 `LightReasoner` 讲 reasoning efficiency。

`SepLLM` 可以继续进入 LLM efficiency / long-context / compression 方向。
