---
title: "HKUDS027: HiGPT 作为 Heterogeneous Graph Language Model 与 Structured Multimodal Layer"
date: 2026-06-26 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds027, hkuds, higpt, heterogeneous-graph, graph-language-model, structured-multimodal, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第二十八篇。

```text
HKUDS027 -> HiGPT
```

图智能主线到这里形成一个完整小闭环：

```text
HKUDS024 GraphAgent -> agentic graph language assistant
HKUDS025 OpenGraph  -> open graph foundation model
HKUDS026 GraphGPT   -> graph instruction tuning
HKUDS027 HiGPT      -> heterogeneous graph language model
```

如果说 `GraphGPT` 解决的是：

```text
graph tokens 如何进入 LLM 的语言空间？
```

那么 `HiGPT` 进一步解决的是：

```text
异构图里的不同节点类型、边类型、关系语义，如何进入 LLM？
```

这篇非常适合放在 `Graph-Language / Structured Multimodal` 这条线上。

它确实属于广义多模态，但不是图像、音频、视频那种多模态，而是：

```text
Text + Heterogeneous Graph
```

也就是把结构化关系数据作为一种模态，和自然语言对齐。

对我们来说，这个方向直接连接：

```text
company-event-asset graph
factor-risk-regime graph
paper-method-dataset graph
person-organization-project graph
Research OS memory graph
Quant Research OS diagnosis graph
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `HiGPT`。

| Item | Value |
|---|---|
| repo | `HiGPT` |
| remote | `https://github.com/HKUDS/HiGPT.git` |
| branch | `main` |
| local head | `2b0793e` |
| full commit | `2b0793e7bdfda693cbe84e5bd8632a59657c72a3` |
| latest local commit date | `2024-06-05 22:43:13 +0800` |
| latest local commit | `Add files via upload` |
| status | clean, synced with `origin/main` after fetch |
| paper | `HiGPT: Heterogeneous Graph Language Model`, arXiv `2402.16024` |
| venue | KDD 2024 Research Track |
| authors | Jiabin Tang, Yuhao Yang, Wei Wei, Lei Shi, Long Xia, Dawei Yin, Chao Huang |
| project page | `https://HiGPT-HKU.github.io/` |
| tracked files by `rg --files` | 210 |
| Python files | 123 |
| markdown files | 18 |
| shell scripts | 12 |
| YAML files | 2 |
| Python syntax check | `py -m compileall -q ...` passed |

README 里给出的 HuggingFace 资源包括：

| Asset | Description |
|---|---|
| `Jiabin99/In-Context-HGT` | trained in-context heterogeneous graph tokenizer |
| `Jiabin99/HiGPT` | Vicuna-7B-v1.5 based HiGPT checkpoint tuned on 60-shot IMDB graph instruction data |

项目结构上，几个最关键的目录是：

```text
HG_grounding/       text-graph contrastive alignment for heterogeneous graph tokenizer
higpt/model/        HeteroLlama, MetaHGT, graph-language model components
higpt/train/        heterogeneous graph instruction tuning
higpt/eval/         standard evaluation and graph in-context evaluation
hi_datasets/        dataset download scripts
mot_prompting/      Mixture-of-Thought graph instruction augmentation
scripts/            tune/eval/serving scripts
```

## 一句话定位

HiGPT 是一个：

```text
heterogeneous graph language model
```

更工程化地说，它是：

```text
Vicuna / Llama
+ in-context heterogeneous graph tokenizer
+ graph projector
+ heterogeneous graph instruction tuning
+ MoT augmented graph instructions
```

它试图让 LLM 不只看到文本，也看到异构图里的结构信息：

```text
node type
edge type
node feature
edge relation
heterogeneous schema
subgraph context
```

这和普通 graph LLM 的区别在于：

```text
普通 graph:
  node -> node
  edge -> edge
  schema 相对单一

heterogeneous graph:
  paper -> author -> affiliation
  movie -> actor -> director
  subject -> paper -> term
  每种 node / edge 都有不同语义
```

量化里也一样：

```text
stock -> belongs_to -> industry
company -> reports -> financial_statement
news -> mentions -> company
event -> impacts -> asset
factor -> explains -> return
portfolio -> exposes_to -> risk
```

这些不是普通 table，也不是普通文本。它们天然是异构图。

## 项目用途

HiGPT 的用途可以分成三层。

第一层是异构图任务。

典型任务包括：

```text
node classification
graph instruction answering
few-shot heterogeneous graph reasoning
graph in-context learning
```

README 里主要用到 IMDB、DBLP、ACM 这类异构图数据。

这些数据的 schema 很适合说明问题：

```text
IMDB:
  movie / actor / director 等不同节点类型

DBLP:
  paper / author / conference 等不同节点类型

ACM:
  subject / paper 等不同节点类型
```

第二层是 graph-language alignment。

模型不只是预测一个标签，而是要在 instruction format 里回答问题：

```text
Human: <graph> Given the heterogeneous graph, classify this target node.
Assistant: The likely category is ...
```

也就是说，图任务被包装成 LLM 能理解的对话任务。

第三层是 structured multimodal learning。

HiGPT 的模态不是 image，也不是 video，而是：

```text
heterogeneous graph structure
```

它回答的是：

```text
结构化关系如何作为一种模态进入 LLM？
```

这正是 Research OS / Quant OS 的关键问题。

## 三个核心创新

README 把 HiGPT 的核心拆成三个部分：

```text
In-Context Heterogeneous Graph Tokenizer
Heterogeneous Graph Instruction-Tuning
Mixture-of-Thought Augmentation
```

我把它翻译成我们的系统语言：

```text
1. 把异构图编码成 graph tokens。
2. 把 graph tokens 注入 LLM embedding space。
3. 用多种 reasoning prompt 扩充训练指令。
```

### 1. In-Context Heterogeneous Graph Tokenizer

这是 HiGPT 区别于 GraphGPT 的关键。

GraphGPT 已经能把 graph token 放进 LLM，但 HiGPT 面对的是：

```text
不同 node type
不同 edge type
不同 graph schema
```

所以它需要一个 type-aware graph tokenizer。

核心实现主要在：

```text
higpt/model/meta_hgt/meta_hgtconv_bert_all.py
higpt/model/meta_hgt/meta_linear.py
HG_grounding/
run_offline_hgt_tokenizer.py
run_offline_hgt_tokenizer_single.py
```

`MetaHGTConv` 是核心图编码器。

它接收：

```python
x_dict
edge_index_dict
node_type_feas_dict
edge_type_feas_dict
```

其中：

```text
x_dict:
  每种节点类型的 node features。

edge_index_dict:
  每种边类型的连接关系。

node_type_feas_dict:
  节点类型的语义表示。

edge_type_feas_dict:
  边类型的语义表示。
```

这就比普通 GNN 多了一层：

```text
type semantics
```

`meta_linear.py` 里有一个很关键的设计：

```python
ParameterGenerator
MetaHeteroLinear
MetaHeteroDictLinear
```

它不是给每个 node type / edge type 写死一组固定参数，而是：

```text
根据 type semantic memory 生成动态线性层参数。
```

这就是 in-context 的意思之一：

```text
node / edge type 的文本语义参与图编码过程。
```

对于我们的 Quant OS，这一点非常关键。

因为未来我们的图不是固定 schema。

一开始可能是：

```text
stock
industry
news
factor
```

后面会加入：

```text
analyst_report
macro_event
earnings_call
management
supply_chain
policy
risk_regime
```

如果 graph tokenizer 能用 type description 生成 type-aware 参数，那么 schema 扩展会更自然。

### 2. Text-Graph Contrastive Alignment

`HG_grounding/README.md` 明确写的是：

```text
Text-Graph Contrastive Alignment (Grounding) with In-Context Heterogeneous Graph Tokenizer
```

它的目标是预训练异构图 tokenizer，让图表示和文本表示对齐。

训练入口是：

```text
HG_grounding/lit_train/lit_hgt_train.py
HG_grounding/lit_models/lit_hgt.py
HG_grounding/models/clip_models/homo_clip.py
```

整体结构是 CLIP-style：

```text
graph encoder -> graph features
text encoder  -> text features
contrastive loss -> align graph and text
```

README 里的训练命令大致是：

```shell
python lit_train/lit_hgt_train.py \
  --max_epochs 5 \
  --learning_rate 1e-6 \
  --devices 4 \
  --context_length 128 \
  --dataset_dir ./datasets \
  --datalist instruct_ds_caption_paper,instruct_ds_caption_movie \
  --micro_batch_size 1 \
  --batch_size 1 \
  --gnn_type meta
```

对我们来说，这提供了一个非常明确的训练范式：

```text
graph object + text description
```

例如量化版可以是：

```text
graph object:
  company - industry - news - factor - asset

text description:
  This graph describes how an earnings surprise propagates through sector peers and valuation factors.
```

然后训练：

```text
graph encoder output
```

去接近：

```text
text description embedding
```

这就是 Quant Research OS 的 graph grounding。

### 3. Heterogeneous Graph Instruction Tuning

HiGPT 的 instruction tuning 分两阶段。

README 里写得很清楚：

```text
Stage 1:
  instruction tuning with heterogeneous graph corpus

Stage 2:
  heterogeneity-aware fine-tuning
```

训练入口主要是：

```text
higpt/train/train_hete_nopl.py
scripts/tune_script/higpt_stage_1.sh
scripts/tune_script/higpt_stage_2.sh
scripts/extract_graph_projector.py
```

Stage 1 训练 heterogeneous graph corpus。

README 示例里使用：

```text
instruct_ds_matching_author
instruct_ds_matching_movie
instruct_ds_matching_paper
instruct_ds_node_matching
instruct_ds_node_matching_imdb
```

这说明第一阶段重点是：

```text
让 LLM 学会 heterogeneous graph token matching。
```

Stage 2 是 task-specific / heterogeneity-aware fine-tuning。

README 示例里用 IMDB，并跑：

```text
1, 3, 5, 10, 20, 40, 60 shots
```

这说明 HiGPT 很关注 few-shot 异构图任务。

这也对应我们未来的现实问题：

```text
quant label 很贵
高质量因子样本很少
真实研究反馈很稀缺
```

所以 few-shot graph instruction tuning 对 Quant OS 很有价值。

## 离线 HGT Tokenizing

HiGPT 一个很工程化的点是：

```text
offline heterogeneous graph tokenizing
```

README 明确说，由于 graph tokenizer 在两个训练过程中不更新参数，所以可以提前把 graph data 处理好，加速训练。

对应脚本：

```text
run_offline_hgt_tokenizer.py
run_offline_hgt_tokenizer_single.py
scripts/tune_script/run_graph_tokenizer.sh
scripts/tune_script/run_graph_tokenizer_single.sh
```

这一步做的事情是：

```text
1. 加载预训练 MetaHGT。
2. 读取 graph_data。
3. 根据 node type / edge type feature dict 计算 graph embedding。
4. 把原 graph_data 路径替换成 graph_data_processed_MetaHGT...
5. 保存 processed graph。
6. 生成 ann_processed_MetaHGT... JSON。
```

这和 `HeteroLlama.forward()` 的实现是配套的。

在 `HeteroLlama.forward()` 里，当前主路径不是实时跑 graph tower，而是：

```python
ret_g = g.x_dict
for k in keys:
    graph_node_features.append(ret_g[k])
```

也就是说，进入 LLM 前的 `graph_data.x_dict` 已经是处理过的 heterogeneous graph token features。

然后再通过：

```python
self.graph_projector(node_feature)
```

把 graph feature 映射到 Llama hidden size。

这个设计非常适合我们。

因为我们的 Research OS / Quant OS 可以把重计算放到离线层：

```text
nightly graph build
nightly graph embedding
nightly factor/event graph update
```

在线时只需要：

```text
LLM reads graph tokens + instruction
```

这比每次提问都重新构图更现实。

## HeteroLlama 怎么把图塞进 LLM

核心文件：

```text
higpt/model/HeteroLlama.py
```

它注册了：

```python
AutoConfig.register("HeteroLlama", HeteroLlamaConfig)
AutoModelForCausalLM.register(HeteroLlamaConfig, HeteroLlamaForCausalLM)
```

整体思路和 GraphGPT 类似：

```text
1. tokenizer 增加 <g_patch>。
2. 可选增加 <g_start> / <g_end>。
3. instruction 里的 <graph> 被替换成多个 graph patch tokens。
4. graph features 经过 graph_projector。
5. graph features 替换对应 token 的 input embeddings。
6. Llama 用混合后的 embeddings 做 causal LM。
```

关键 token：

```text
<graph>
<g_patch>
<g_start>
<g_end>
```

`preprocess_graph_Hetero()` 会根据不同 node type 的 token length 构造多段 replacement：

```python
for token_len in graph_token_lens:
    replace_token = DEFAULT_GRAPH_PATCH_TOKEN * token_len
    if graph_cfg['use_graph_start_end']:
        replace_token = DEFAULT_G_START_TOKEN + replace_token + DEFAULT_G_END_TOKEN
```

这和普通 GraphGPT 的一个区别是：

```text
HiGPT 的 <graph> 可以对应多个 heterogeneous node type segment。
```

`hetero_key_order` 决定了顺序。

例如一个 DBLP graph 可以按：

```text
paper
author
conference
```

的顺序把不同类型节点特征放进去。

这对 Quant OS 很有启发。

我们也可以定义：

```text
hetero_key_order = [
  "asset",
  "company",
  "industry",
  "event",
  "factor",
  "risk_regime"
]
```

然后让模型知道每一段 graph token 的语义来源。

## MetaHGT 的关键组件

`MetaHGTConv` 的实现可以拆成四个部件。

### Type-Aware KQV

它对不同 node type 生成 K/Q/V：

```python
kqv_dict = self.kqv_lin(x_dict, node_type_feas_dict)
```

这里的 `self.kqv_lin` 是 `MetaHeteroDictLinear`。

它的参数不是固定的，而是由 node type feature 生成。

### Edge-Type-Aware Relation Transform

边类型进入：

```python
self.k_rel
self.v_rel
self.p_relTrans
```

也就是对不同 edge type 生成不同的 relation transform 和 attention bias。

这对金融图很关键。

因为：

```text
company -> belongs_to -> industry
company -> supplies -> company
event -> impacts -> asset
factor -> explains -> return
```

这些边的含义完全不同，不能当成同一种 edge。

### Skip Gate

代码里有：

```python
self.skipTrans = nn.Linear(text_cfg.width, 1)
```

然后：

```python
alpha = skip[node_type].sigmoid()
out = alpha * out + (1 - alpha) * x_dict[node_type]
```

这相当于每种 node type 都有自己的 residual gate。

含义是：

```text
不同类型节点对 message passing 的依赖程度不同。
```

比如量化里：

```text
price time series node
```

和：

```text
industry taxonomy node
```

信息更新方式肯定不一样。

### Dynamic Parameter Generation

`ParameterGenerator` 是我最喜欢的部分。

它做的是：

```python
type memory -> weights, biases
```

这让模型有机会根据 type description 生成适合当前 type 的线性变换。

这比手动维护每个 type 的 module 更灵活。

对我们未来扩展 graph schema 很重要。

## Mixture-of-Thought Augmentation

HiGPT 不只是构图和训练模型，还做 instruction augmentation。

目录：

```text
mot_prompting/
```

README 里列出支持：

```text
0: CoT without Format Constraint
1: CoT with Format Constraint
2: ToT with Multiple Round
3: ToT with Single Round
4: PanelGPT
5: GKP-1
6: GKP-2
```

这套东西很适合我们的 R&D Agent。

因为一个 factor / strategy / graph reasoning 问题，本来就不应该只有一种思考路径。

例如：

```text
问题：
  为什么这个因子最近失效？

CoT:
  step-by-step 解释暴露、换手、拥挤度、行业中性问题。

ToT:
  三个专家分别从微观结构、基本面、宏观风险思考。

PanelGPT:
  PM、researcher、risk manager 三方讨论。

GKP:
  先生成领域知识，再推理。
```

HiGPT 的 MoT 对我们的最大启发不是直接复用脚本，而是：

```text
训练数据不能只有答案，要有多种 reasoning style。
```

## Evaluation

评估入口：

```text
higpt/eval/run_higpt.py
higpt/eval/run_higpt_incontext.py
scripts/eval_script/higpt_info_imdb_cot.sh
scripts/eval_script/hetegpt_info_imdb_cot_incontext.sh
```

`run_higpt.py` 是普通 evaluation。

`run_higpt_incontext.py` 支持 Graph In-Context Learning。

区别是：

```text
run_higpt.py:
  当前 graph -> answer

run_higpt_incontext.py:
  context_graphs + current graph -> answer
```

这对我们很重要。

Quant Research OS 里也有类似结构：

```text
过去成功因子图
过去失败因子图
当前新因子图
```

然后问：

```text
这个新 idea 更像哪些历史 pattern？
它是否有过拟合风险？
它应该进入哪一个 research bucket？
```

这就是 graph in-context learning 的实际用途。

## 和 GraphGPT 的区别

GraphGPT 和 HiGPT 很像，但关注点不同。

| Project | Core Problem | Key Layer |
|---|---|---|
| GraphGPT | LLM 如何理解 graph-structured data | graph-language instruction tuning |
| HiGPT | LLM 如何理解 heterogeneous graph-structured data | type-aware graph-language instruction tuning |

GraphGPT 更像：

```text
graph -> graph tokens -> LLM
```

HiGPT 更像：

```text
heterogeneous graph schema
+ node type semantics
+ edge type semantics
+ graph tokens
+ LLM
```

所以 HiGPT 是 GraphGPT 的异构图加强版。

对我们来说：

```text
GraphGPT:
  教我们如何把图注入 LLM。

HiGPT:
  教我们如何把真实世界的复杂 schema 注入 LLM。
```

真实世界很少是单一图。

金融更不是。

## 和 OpenGraph 的区别

OpenGraph 的重点是：

```text
graph foundation representation
zero-shot graph generalization
```

HiGPT 的重点是：

```text
heterogeneous graph + LLM instruction answering
```

所以可以这样分工：

```text
OpenGraph:
  更偏 graph foundation model。

HiGPT:
  更偏 graph-language interface。
```

如果放进 Research OS：

```text
OpenGraph 负责 graph representation。
HiGPT 负责 graph-to-language reasoning。
```

## 和 GraphAgent 的区别

GraphAgent 的重点是 agent workflow。

它关注：

```text
用户问题 -> 任务规划 -> 构图 -> graph LLM -> answer
```

HiGPT 的重点是模型层。

它关注：

```text
heterogeneous graph tokens 如何进入 LLM
```

所以：

```text
GraphAgent = workflow / agent layer
HiGPT      = model / representation-to-language layer
```

未来我们自己的系统也应该这样分层。

```text
Agent layer:
  负责 decide what graph to build / retrieve / analyze。

Model layer:
  负责 encode graph and answer。
```

## 这是不是多模态

是，但要说清楚。

HiGPT 属于：

```text
Structured Multimodal AI
Graph-Language Model
Graph-Text Alignment
Heterogeneous Graph + Text Multimodal Learning
```

它不属于典型的：

```text
image-text multimodal
video-language multimodal
audio-language multimodal
```

所以我们在网站和学习地图里最好叫：

```text
Graph-Language / Structured Multimodal
```

这比直接叫 multimodal 更准确。

## 对 Quant Research OS 的启发

HiGPT 对我们最关键的启发是：

```text
金融世界本质上是 heterogeneous graph。
```

不是一个单表，也不是纯文本。

它至少包括：

```text
资产
公司
行业
供应链
管理层
财报
新闻
宏观事件
政策
因子
组合
风险暴露
交易信号
回测结果
研究员判断
PM 审核意见
```

这些对象之间的关系包括：

```text
belongs_to
mentions
impacts
correlates_with
explains
hedges
exposes_to
fails_under
validated_by
rejected_by
```

这就是异构图。

如果我们未来做 `Pengyi Quant Research OS`，可以按 HiGPT 的思路设计：

```text
Node Types:
  asset
  company
  industry
  factor
  event
  news
  backtest
  risk_regime
  research_note

Edge Types:
  belongs_to
  mentioned_by
  impacted_by
  explains_return
  has_exposure
  fails_in_regime
  supports_hypothesis
  contradicts_hypothesis
```

然后为每个 type 写 description：

```text
asset:
  This node represents a tradable asset.

factor:
  This node represents a quantitative signal that may explain future returns.

risk_regime:
  This node represents a market state where certain factors may become crowded or unstable.
```

这就能接 HiGPT 的 type semantics 思路。

## 我们自己的轻量路线

短期不需要训练一个 7B HiGPT。

可以先做轻量版。

### Stage 1: Build Quant Heterogeneous Graph

先把 Research OS 的对象图建起来：

```text
research_note
factor
backtest
asset
industry
news
event
risk
```

存成：

```text
nodes table
edges table
node_type descriptions
edge_type descriptions
```

### Stage 2: Graph Retrieval

先不用训练 graph tokenizer。

用普通 embedding + graph traversal：

```text
retrieve relevant subgraph
serialize as structured text / JSON
feed to LLM
```

这是最现实的 v0。

### Stage 3: Instruction Dataset

把问题变成 instruction data：

```json
{
  "input": "Given this factor-event-asset graph, diagnose why the factor failed in 2024Q4.",
  "graph": "...",
  "answer": "The factor likely failed because ...",
  "reasoning_type": "risk_manager_panel"
}
```

### Stage 4: Type-Aware Graph Encoder

等数据积累够了，再训练：

```text
type-aware graph encoder
```

这时可以参考：

```text
MetaHGT
GraphCLIP
HiGPT
```

### Stage 5: Quant HiGPT

最终目标才是：

```text
Quant HiGPT
```

它能回答：

```text
这个因子的结构性失败来源是什么？
这个事件沿着哪些关系影响资产？
这个 strategy 更像哪些历史成功/失败模式？
下一轮 research 应该扩展哪些节点和边？
PM 应该问 researcher 哪些关键问题？
```

这和我们的 R&D Agent 完全同向。

## 可以提 PR 的地方

HiGPT 这个 repo 很有价值，但也有不少低风险、可落地的工程改进点。

第一，repo 里 tracked 了不少 `__pycache__` / `.pyc` 文件。

例如：

```text
higpt/__pycache__/...
higpt/model/__pycache__/...
higpt/model/meta_hgt/__pycache__/...
higpt/train/__pycache__/...
```

这类文件通常不应该进入源码仓库。可以加 `.gitignore` 并清理 tracked pyc。

第二，根目录有 `.DS_Store`。

这也是典型 repo hygiene PR。

第三，`requirements.txt` 很重，而且包含 OS-bound package。

例如：

```text
python-apt==1.6.5+ubuntu0.7
PyGObject==3.26.1
unattended-upgrades==0.1
ssh-import-id==5.7
```

这类依赖在普通 pip / conda 环境里不一定能装。可以拆成：

```text
requirements-core.txt
requirements-dev.txt
requirements-linux-extra.txt
```

第四，很多路径硬编码到作者训练环境。

例如：

```text
/root/paddlejob/workspace/env_run/...
```

出现在 README、training script、offline tokenizer、`train_hete.py` 等位置。

这会降低复现性。可以改成 CLI 参数、环境变量或 repo-relative path。

第五，`run_offline_hgt_tokenizer.py` 里有：

```python
device = 'cuda:2'
```

这应该变成：

```text
--device cuda:0
```

或者自动检测。

第六，evaluation 里硬编码了：

```python
load_metahgt_pretrained(MetaHGTConv, './MetaHGT_imdb_dblp_epoch5')
```

可以改成：

```text
--graph_tower_path
```

第七，`train_hete_nopl.py` 里有：

```python
model.config.pretrain_graph_model_path = model.config.pretrain_graph_model_path + model_args.graph_tower
```

这假设 base model config 已经有 `pretrain_graph_model_path`。更稳妥的是显式参数或 `getattr` fallback。

第八，`train_hete_nopl.py` / eval 文件里 edge feature 转 tensor 处疑似有复制粘贴错误。

例如读完：

```python
self.edge_feas_dict_acm = torch.load(...)
```

后面循环却仍在处理：

```python
self.node_feas_dict_acm
```

`edge_feas_dict_imdb` 处也有类似问题。这个需要实测确认，但从代码形态看值得开 issue 或 PR。

第九，`higpt/conversation.py` 里系统提示仍然写：

```text
You are GraphChat...
graph-structral
```

这里既沿用了 GraphChat 命名，也有 `graph-structral` 拼写问题。可以改成 HiGPT / graph-structural。

第十，README 里编号有小问题。

Table of Contents 写 `4. Evaluating HiGPT`，正文标题是：

```text
## 5. Evaluating HiGPT
```

可以统一。

这些 PR 都不碰算法核心，适合从 docs / scripts / reproducibility / repo hygiene 入手。

## 我们应该怎么吸收

我们对 HiGPT 的吸收不应该是：

```text
立刻训练大模型。
```

而应该是：

```text
学习它如何把真实世界复杂关系抽象为 type-aware graph-language system。
```

具体落地为三件事。

第一，给我们的 Research OS 建立 node / edge type ontology。

```text
什么是 project？
什么是 paper？
什么是 factor？
什么是 backtest？
什么是 risk event？
什么是 PM feedback？
```

第二，把每个 type 写成自然语言 description。

这对应 HiGPT 的 node/edge type semantic memory。

第三，把每次研究过程转成 graph instruction。

例如：

```text
Given this research graph, identify the next experiment.
Given this factor graph, diagnose the likely overfitting source.
Given this project graph, decide which repo we should study next.
```

这就是我们自己的：

```text
Research OS graph instruction dataset
```

## 总结

HiGPT 是图智能主线里非常重要的一块。

它补上的是：

```text
heterogeneous graph intelligence
```

前面三篇分别是：

```text
GraphAgent -> agent workflow
OpenGraph  -> graph foundation representation
GraphGPT   -> graph-language instruction tuning
```

HiGPT 则告诉我们：

```text
真实世界的 graph-language system 必须处理 node type / edge type / schema heterogeneity。
```

这对 Quant OS 特别关键。

因为金融世界不是一张表，而是一个不断变化的异构关系网络。

所以 HKUDS024-027 这四篇可以合成一条主线：

```text
GraphAgent: workflow
OpenGraph: representation
GraphGPT:  language alignment
HiGPT:     heterogeneous schema intelligence
```

这就是我们下一阶段 Research OS / Quant Research OS 的 graph-language foundation。

## 下一步

按照第三张学习地图，图智能主线之后进入：

```text
HKUDS028 -> RecLM
```

也就是 recommendation / user behavior / finance-adjacent 方向。

这条线可以连接：

```text
用户行为建模
推荐系统
金融信号抽象
portfolio / factor selection
agent preference learning
```

对 Quant Research OS 也很有价值。
