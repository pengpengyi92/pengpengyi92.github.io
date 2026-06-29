---
title: "HKUDS029: XRec 作为 Explainable Recommendation 与 Collaborative-Signal-to-Language Layer"
date: 2026-06-28 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds029, hkuds, xrec, recommendation, explainable-recommendation, lightgcn, moe-adapter, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS029`。

```text
HKUDS029 -> XRec
```

上一篇 `HKUDS028` 我们研究了 `RecLM`：

```text
RecLM = recommendation instruction tuning
      + user/item profile generation
      + profile-augmented ranking
```

这一篇进入同一条主线下的第二个关键项目：

```text
XRec = explainable recommendation
```

如果说 `RecLM` 解决的是：

```text
如何用 LLM 生成更好的 user/item profile，并服务 recommender ranking
```

那么 `XRec` 解决的是：

```text
如何让 LLM 生成的推荐解释，真正被 collaborative filtering signal 约束
```

这对我们做 Quant Research OS 很关键。

量化研究里很多输出都不应该只是一个裸结论：

```text
这个因子值得看。
这个策略值得回测。
这个 repo 值得深入。
这个 paper 值得复现。
这个 regime 下应该优先关注这类信号。
```

更重要的是：

```text
为什么？
依据是什么？
这个判断来自哪些历史交互、结构信号、用户偏好、市场条件？
解释是否和真实 ranking / recommendation signal 对齐？
```

XRec 给我们的启发就是：

```text
explanation 不能只是 LLM 的语言能力。
explanation 应该被推荐系统里的 user/item embedding、graph signal、interaction history 约束。
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `XRec`。

| Item | Value |
|---|---|
| repo | `XRec` |
| remote | `https://github.com/HKUDS/XRec.git` |
| branch | `main` |
| local head | `8b73f3f` |
| full commit | `8b73f3f04f6dde71cd1339654b087bac0e2d80d5` |
| latest local commit date | `2024-09-24 21:20:22 +0800` |
| latest local commit | `Update README.md` |
| paper | `XRec: Large Language Models for Explainable Recommendation`, arXiv `2406.02377` |
| venue | EMNLP 2024 |
| authors | Qiyao Ma, Xubin Ren, Chao Huang |
| tracked files by `rg --files` | 99 |
| Python files | 26 |
| Markdown files | 2 |
| JSON files | 12 |

项目结构很集中：

```text
data/          processed recommendation datasets and generated artifacts
encoder/       graph collaborative filtering encoder
explainer/     LLaMA-based explanation model
generation/    GPT-generated user/item profiles and explanations
evaluation/    BERTScore / GPTScore explanation evaluation
process/       raw data processing scripts and notes
```

XRec 的 README 把它定位为：

```text
model-agnostic framework
+ graph-based collaborative filtering
+ large language models
+ comprehensive recommendation explanations
```

一句话：

```text
XRec 是把协同过滤图信号接入 LLM explanation generation 的推荐解释系统。
```

## 一句话定位

XRec 不是普通的：

```text
输入 user profile 和 item profile，然后让 LLM 写一段推荐理由
```

它真正做的是：

```text
LightGCN user/item embedding
-> MoE adapter
-> LLaMA hidden space
-> special-token embedding replacement
-> modified attention Q/K/V injection
-> grounded natural language explanation
```

这里最重要的不是“用了 LLaMA”。

最重要的是：

```text
collaborative signal 被显式注入到了语言模型的生成过程里。
```

也就是说，解释不是凭空生成的。

解释被两个东西同时约束：

```text
1. profile text:
   user summary
   item summary
   item title / business title

2. collaborative embedding:
   user embedding from interaction graph
   item embedding from interaction graph
```

这比纯 prompt-based explanation 更接近真实推荐系统需要的解释方式。

## XRec 想解决什么问题

推荐系统经常需要解释。

传统推荐模型可以做 ranking：

```text
user u
item i
score(u, i)
```

但是它很难自然地解释：

```text
为什么推荐这个 item？
哪些 user preference 支撑了这个推荐？
这个 item 的哪些属性和 user history 对齐？
```

LLM 很擅长解释，但 naive LLM explanation 有一个核心问题：

```text
LLM 可以写得很像解释，但不一定被真实推荐信号约束。
```

XRec 要解决的就是这个断裂：

```text
collaborative filtering knows the interaction structure
LLM knows how to generate readable language
XRec connects them
```

所以它的研究问题可以写成：

```text
How can we inject collaborative filtering signals into LLMs
so that recommendation explanations are both fluent and grounded?
```

## 总体架构

XRec 可以拆成五层：

```text
Layer 1: raw user-item interactions and profile/explanation data
Layer 2: LightGCN collaborative encoder
Layer 3: MoE adapter from CF embedding space to LLaMA hidden space
Layer 4: LLaMA explanation model with special-token injection
Layer 5: explanation evaluation by BERTScore and GPTScore
```

展开以后就是：

```text
interaction data
  -> train LightGCN
  -> save user_emb.pkl and item_emb.pkl
  -> build explanation prompt with <USER_EMBED>, <ITEM_EMBED>, <EXPLAIN_POS>
  -> use MoE adapter to convert 64-dim CF embeddings to 4096-dim LLaMA embeddings
  -> replace special-token embeddings
  -> inject user/item embeddings into LLaMA attention Q/K/V
  -> train only the adapter / converter
  -> generate recommendation explanation
  -> evaluate against reference explanation
```

这是一个很清晰的 research engineering pipeline。

## 数据层

README 里列了三个数据集：

```text
Amazon-books -> amazon
Google-reviews -> google
Yelp -> yelp
```

每个数据集目录里对应的结构包括：

```text
data.json
trn.pkl
val.pkl
tst.pkl
total_trn.csv
total_val.csv
total_tst.csv
user_emb.pkl
item_emb.pkl
user_converter.pkl
item_converter.pkl
tst_pred.pkl
tst_ref.pkl
```

这里有三类资产：

```text
1. interaction split:
   trn / val / tst

2. collaborative representation:
   user_emb.pkl
   item_emb.pkl

3. explanation artifacts:
   generated prediction
   reference explanation
```

`process/README.md` 说明了原始数据处理链路：

```text
raw dataset
-> total.csv
-> para_dict.pickle
-> profile / explanation JSON
-> data.json
-> train / validation / test split
```

所以 XRec 不是只给一个模型文件。

它有完整的：

```text
data processing
profile generation
graph embedding
LLM explanation
evaluation
```

这点对我们很重要，因为 Quant Research OS 也必须避免只做单点 demo。

我们真正要的也是完整链路：

```text
raw market / research / strategy data
-> structured dataset
-> embedding / graph / profile
-> reasoning / recommendation / explanation
-> benchmark / evaluation
-> PM review
```

## Encoder: LightGCN Collaborative Signal

XRec 的协同过滤信号来自 `encoder/train_encoder.py` 和 `encoder/models/lightgcn.py`。

核心模型是 `LightGCN`。

它做的事情是：

```text
user-item interaction graph
-> graph propagation
-> user embeddings
-> item embeddings
-> BPR-style recommendation training
```

训练完成后保存：

```text
./data/{dataset}/user_emb.pkl
./data/{dataset}/item_emb.pkl
```

这些 embedding 是后面 LLM explanation 的关键条件。

也就是说，XRec 不是把 user 和 item 只当文本。

它还把它们当成 interaction graph 里的节点：

```text
user = node with behavioral neighborhood
item = node with interaction neighborhood
```

这点非常接 Quant。

在量化研究里，我们也可以把对象图化：

```text
factor <-> asset
strategy <-> regime
paper <-> method
repo <-> capability
PM <-> preference
experiment <-> result
```

然后 embedding 不是“好看”的 embedding，而是从真实交互和结果中学习出来的：

```text
which factors were researched
which strategies were backtested
which signals survived out-of-sample
which PM accepted or rejected the idea
which market regime made a method fail
```

这就是 XRec 对 Quant OS 的第一层启发：

```text
解释层之前，必须先有行为图和协同过滤信号。
```

## Explainer: LLaMA + Special Tokens

XRec 的 explanation model 在：

```text
explainer/models/explainer.py
```

它加载的是：

```text
meta-llama/Llama-2-7b-chat-hf
```

并添加三个特殊 token：

```text
<USER_EMBED>
<ITEM_EMBED>
<EXPLAIN_POS>
```

这三个 token 的角色很清楚：

```text
<USER_EMBED>  = user collaborative embedding 的注入位置
<ITEM_EMBED>  = item collaborative embedding 的注入位置
<EXPLAIN_POS> = explanation loss 和 generation 的起点
```

`DataHandler` 构造出来的 prompt 大概是：

```text
<s>[INST] <<SYS>>
Explain why the user would buy with the book within 50 words.
<</SYS>>

user record: <USER_EMBED>
book record: <ITEM_EMBED>
book name: ...
user profile: ...
book profile: ...
<EXPLAIN_POS>
ground truth explanation
[/INST]
```

对 Yelp / Google 这类 business 场景，system prompt 会变成：

```text
Explain why the user would enjoy the business within 50 words.
```

这说明 XRec 的输入不是一个裸 embedding。

它是：

```text
collaborative embedding + textual profile + controlled explanation target
```

## MoE Adapter: 从 64 维到 4096 维

XRec 的一个关键组件是 `MoEAdaptorLayer`。

它做的是：

```text
64-dim user/item embedding
-> 4096-dim LLaMA hidden embedding
```

为什么需要 adapter？

因为 LightGCN 的 embedding space 和 LLaMA 的 token embedding space 不是同一种空间：

```text
LightGCN embedding:
  collaborative filtering space
  reflects interaction graph
  dimension = 64

LLaMA hidden embedding:
  language model representation space
  reflects text semantics and generation dynamics
  dimension = 4096
```

所以中间必须有一个桥：

```text
CF space -> language hidden space
```

XRec 用的是 Mixture of Experts adapter：

```text
8 experts
noisy gating
MLP projection
dropout
```

这比单个 linear projector 更有表达力。

可以理解为：

```text
不同 user/item embedding 可能需要不同的 projection expert。
```

对 Quant OS 的映射也很明显：

```text
factor embedding
strategy embedding
market regime embedding
PM preference embedding
research history embedding
```

这些都不能直接塞进 LLM。

中间要有：

```text
quant signal adapter
```

把结构信号投影到语言模型能用的空间。

## 双重注入机制

XRec 最值得注意的是它不只做 embedding replacement。

它有两层注入。

第一层：

```text
replace <USER_EMBED> token embedding with converted user embedding
replace <ITEM_EMBED> token embedding with converted item embedding
```

第二层在：

```text
explainer/models/modeling_explainer.py
```

这是修改过的 LLaMA。

在 attention 里，它会把 user/item embedding 加到对应位置的：

```text
query states
key states
value states
```

也就是说，协同过滤信号不只是作为一个普通 token 进入上下文。

它还直接改变了 self-attention 的计算：

```text
attention query
attention key
attention value
```

这点很重要。

因为如果只替换 token embedding，模型可能仍然很快把这个 signal 弱化。

但如果在 Q/K/V 层也注入，它会影响：

```text
which context is attended
how user/item signal participates in token interaction
how explanation token generation conditions on CF embeddings
```

这是 XRec 的核心工程贡献之一：

```text
collaborative embedding is injected into the internal attention dynamics of LLaMA.
```

## 训练目标

XRec 冻结 LLaMA 参数。

训练的主要是：

```text
user_embedding_converter
item_embedding_converter
```

也就是两个 MoE adapter。

这样做有两个好处：

```text
1. 成本更低，不需要 full finetune LLaMA。
2. 目标更清晰，只学习如何把 CF embedding 接入语言模型。
```

loss 的设计也很关键。

`Explainer.loss()` 会把 `<EXPLAIN_POS>` 之前的 token mask 掉：

```text
prompt part -> -100
explanation part -> cross entropy loss
```

也就是说模型不是训练去复读 prompt，而是只训练：

```text
given prompt + user embedding + item embedding
generate explanation after <EXPLAIN_POS>
```

这个设计很干净。

对 Quant OS 来说，未来我们也可以做类似结构：

```text
given factor profile + market graph embedding + backtest summary
generate PM explanation after <EXPLAIN_POS>
```

或者：

```text
given paper summary + repo embedding + strategy graph
generate why-this-is-worth-reproducing explanation
```

## 生成与评估

训练后运行：

```bash
python explainer/main.py --mode generate --dataset {dataset}
```

生成 predictions 和 references。

评估脚本在：

```text
evaluation/main.py
evaluation/metrics.py
```

指标包括：

```text
BERTScore precision / recall / f1
GPTScore
unique sentence ratio
```

这里 GPTScore 通过 OpenAI API 调用 `gpt-3.5-turbo`，根据 system prompt 评估 prediction 和 reference 的解释质量。

这类 evaluation 对 explanation task 很常见。

不过它也提醒我们：

```text
explanation evaluation 本身仍然是难点。
```

仅靠 BERTScore 不够，因为解释可能语义等价但表述不同。

仅靠 GPTScore 也不够，因为 LLM judge 有成本、稳定性和可复现问题。

对 Quant Research OS 来说，解释评估应该更严格：

```text
1. explanation faithfulness:
   解释是否真的引用了被模型使用的 signal？

2. decision usefulness:
   PM 看完解释是否更容易做 yes/no/research-next-step？

3. outcome alignment:
   解释里的风险点是否能解释后续 backtest / live performance？

4. auditability:
   解释能否追溯到 dataset、experiment、factor、market regime？
```

这会是我们以后做 R&D Agent 很重要的一层。

## 和 RecLM 的关系

`RecLM` 和 `XRec` 都在 Recommendation / Finance-adjacent 主线里。

但它们分工不同。

| Project | Core Question | Output |
|---|---|---|
| RecLM | 如何生成更好的 user/item profile，并增强 ranking？ | profile + ranking feature |
| XRec | 如何生成被 CF signal 约束的推荐解释？ | grounded explanation |

RecLM 更像：

```text
profile generator and ranking augmenter
```

XRec 更像：

```text
explanation generator with collaborative grounding
```

放在 Quant Research OS 里：

```text
RecLM -> 帮我们形成 factor / strategy / PM / paper profile
XRec  -> 帮我们解释为什么推荐这个 factor / strategy / repo / paper
```

二者可以组成一套很自然的系统：

```text
Research Memory
  -> profile generation
  -> representation learning
  -> recommendation / ranking
  -> grounded explanation
  -> PM review
```

## 对 Quant Research OS 的启发

XRec 对我们的启发可以分成五点。

### 1. Recommendation 之后必须有 Explanation

一个 R&D Agent 不能只说：

```text
下一个研究 A。
```

它必须说清楚：

```text
为什么是 A？
这个推荐来自哪些历史研究？
这个 idea 和当前 portfolio / regime / PM preference 的关系是什么？
它最大的风险是什么？
下一步最小验证实验是什么？
```

所以我们的系统要有：

```text
recommendation layer
+ explanation layer
+ PM review layer
```

### 2. Explanation 必须被结构信号约束

纯 prompt 的解释很容易漂亮但虚。

XRec 告诉我们：

```text
解释要从 interaction graph、embedding、profile、history 中拿约束。
```

Quant 里对应的结构信号包括：

```text
factor history
backtest lineage
market regime graph
asset-event graph
researcher/PM preference
paper/repo similarity graph
```

### 3. Adapter 是连接结构模型和 LLM 的关键

XRec 的 MoE adapter 很值得借鉴。

我们的 Quant OS 未来也可以有：

```text
factor adapter
strategy adapter
regime adapter
PM preference adapter
paper/repo adapter
```

这些 adapter 的职责是：

```text
把非语言结构信号接到 LLM  reasoning / generation space。
```

### 4. Special Token 是很实用的接口设计

`<USER_EMBED>`、`<ITEM_EMBED>`、`<EXPLAIN_POS>` 的设计非常工程化。

我们可以映射成：

```text
<FACTOR_EMBED>
<STRATEGY_EMBED>
<REGIME_EMBED>
<PM_PREF_EMBED>
<EVIDENCE_POS>
<DIAGNOSIS_POS>
<NEXT_PLAN_POS>
```

这样 prompt 不只是自然语言模板。

它变成了一个有明确计算接口的 programmatic prompt。

### 5. Explanation Evaluation 要进入闭环

XRec 有 BERTScore 和 GPTScore。

我们的 Quant OS 未来可以做更贴近研究的 evaluation：

```text
Does the explanation cite the right evidence?
Does it identify leakage / bias / overfitting risk?
Does it propose a falsifiable next experiment?
Does PM accept the reasoning?
Does it predict later failure mode?
```

这就从“写得像解释”升级成：

```text
解释是否真正提高研究决策质量。
```

## 可以怎么改造成 Quant XRec

我们可以把 XRec 的 user/item recommendation 改造成 quant research recommendation。

原始 XRec：

```text
user
item
user profile
item profile
user embedding
item embedding
recommendation explanation
```

Quant XRec：

```text
researcher / PM / current project
factor / strategy / paper / repo
researcher preference profile
factor / strategy / paper / repo profile
research interaction embedding
candidate artifact embedding
research recommendation explanation
```

具体任务可以是：

```text
Explain why this factor is worth researching next.
Explain why this repo should be studied for our Research OS.
Explain why this strategy matches the current market regime.
Explain why this paper is relevant to our R&D Agent roadmap.
Explain why this backtest result is suspicious.
```

这和我们正在做的学习地图高度一致。

现在我们已经在持续积累：

```text
LLMQuant project notes
HKUDS project notes
QuantMind / X2Strategy comparisons
Research OS architecture notes
RA / PhD / quant talk preparation
```

这些其实都可以变成：

```text
profile
embedding
interaction graph
recommendation candidate
explanation target
```

## 工程上值得注意的 PR 点

XRec 这个 repo 很适合学习，也有一些很现实的 improvement opportunity。

第一，`Explainer.__init__` 里直接调用：

```text
huggingface_hub.login()
```

这会让脚本进入交互式登录，不太适合自动化训练、CI 或服务器环境。

更好的做法是：

```text
优先读取 HF_TOKEN 环境变量
如果已经登录则跳过
必要时再提示用户登录
```

第二，generation 和 evaluation 里 OpenAI key 仍然是代码内空字符串：

```text
openai.api_key = ''
client = OpenAI(api_key="")
```

更好的做法是：

```text
读取 OPENAI_API_KEY
在缺失时给出明确错误
不要要求用户改源码
```

第三，模型名和维度硬编码较多：

```text
meta-llama/Llama-2-7b-chat-hf
64 -> 4096
n_exps = 8
```

更好的做法是把这些参数放进 config / CLI：

```text
--base_model
--cf_dim
--llm_hidden_dim
--num_experts
--adapter_dropout
```

第四，生成文件名和评估文件名有潜在不一致。

`explainer/main.py` 生成：

```text
tst_predictions.pkl
tst_references.pkl
```

但 `evaluation/metrics.py` 读取：

```text
tst_pred.pkl
tst_ref.pkl
```

如果用户按 README 从头跑，可能会遇到路径不一致问题。

这非常适合作为一个小 PR：

```text
统一 prediction/reference 文件名
或者给 evaluation 添加 CLI 参数
```

第五，`generate()` 里用：

```text
outputs[0].find("[")
```

来截断生成结果。

这个规则比较脆弱。

更好的做法是：

```text
用 tokenizer eos token
或明确的 stop sequence
或 postprocess 函数集中处理
```

第六，special token position 默认每个样本都有且只有一个。

更稳健的工程写法应该加 assertion：

```text
exactly one <USER_EMBED>
exactly one <ITEM_EMBED>
exactly one <EXPLAIN_POS>
```

否则数据格式错了时，报错会比较难追。

这些 PR 点都不是为了“刷 PR”。

它们是真实能提高复现体验、自动化体验和工程清晰度的点。

## 我们可以怎么学

XRec 值得重点学三件事。

第一，学习它的架构桥接：

```text
graph recommender embedding -> LLM explanation
```

第二，学习它的 adapter 设计：

```text
MoE projection from structured embedding space to language hidden space
```

第三，学习它的 prompt interface：

```text
special token as injection point
special token as loss boundary
```

这三件事都可以迁移到我们的 Research OS。

尤其是我们要做的 R&D Agent：

```text
自动提出因子假设
自动实现
自动回测
自动诊断偏差
自动生成下一轮研究计划
人类 PM 审核
```

这里每一步都需要 explanation：

```text
为什么提出这个假设？
为什么这个实现是合理的？
为什么这个回测可疑？
为什么下一轮应该这么做？
为什么 PM 应该接受或拒绝？
```

XRec 是我们做这个 explanation layer 的一个很好的范式参考。

## 最终总结

XRec 的核心价值不是“LLM 会写推荐理由”。

它真正重要的是：

```text
用 collaborative filtering embedding 约束 LLM explanation generation。
```

它把推荐系统里的：

```text
interaction graph
user/item embedding
profile text
LLM generation
explanation evaluation
```

连成了一条完整链路。

对我们来说，XRec 可以映射成：

```text
Quant Research Recommendation Explanation Layer
```

未来我们自己的系统里，推荐 factor、strategy、paper、repo、dataset、next experiment 的时候，都不应该只输出一个 candidate list。

更好的形态是：

```text
candidate
+ score
+ evidence
+ collaborative / graph signal
+ grounded explanation
+ risk diagnosis
+ next action
+ PM review decision
```

这就是 XRec 对 Pengyi Quant Research OS 的核心启发。
