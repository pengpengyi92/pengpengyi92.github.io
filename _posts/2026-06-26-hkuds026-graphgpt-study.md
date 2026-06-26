---
title: "HKUDS026: GraphGPT 作为 Graph Instruction Tuning 与 Graph-Language Alignment Layer"
date: 2026-06-26 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds026, hkuds, graphgpt, graph-instruction-tuning, graph-language-alignment, graph-llm, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第二十七篇。

```text
HKUDS026 -> GraphGPT
```

图智能主线已经连续三篇：

```text
HKUDS024 GraphAgent -> agentic graph language assistant
HKUDS025 OpenGraph  -> open graph foundation model
HKUDS026 GraphGPT   -> graph instruction tuning for LLMs
```

这三篇的关系很清楚。

`GraphAgent` 关注：

```text
agent 如何规划任务、构图、调用 graph-language model。
```

`OpenGraph` 关注：

```text
graph model 本身如何跨图 zero-shot generalize。
```

`GraphGPT` 关注：

```text
如何把图结构知识对齐到 LLM 的语言空间，并通过 instruction tuning 让 LLM 能做图任务。
```

也就是说，GraphGPT 是这条线里的 graph-language alignment layer。

对我们来说，这篇非常关键，因为未来的 Research OS / Quant OS 不只是要把图存起来，也不只是要让图模型给 embedding，而是要让 LLM 能以自然语言方式理解和解释图任务：

```text
这篇论文属于哪个方向？
这两个节点之间是否应该有边？
这个因子为什么可能失效？
这个事件会沿着哪些资产/行业关系传播？
这个 project 和我们的 Quant OS 哪个模块最相关？
```

GraphGPT 的目标就是：

```text
让 LLM 通过图 instruction tuning 学会处理 graph-structured data。
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `GraphGPT`。

| Item | Value |
|---|---|
| repo | `GraphGPT` |
| remote | `https://github.com/HKUDS/GraphGPT.git` |
| branch | `main` |
| local head | `db25a66` |
| full commit | `db25a66fd23b861156e6d7324f9ee8bc91c6ce7c` |
| latest local commit date | `2024-06-25 21:27:00 +0800` |
| latest local commit | `Update README.md` |
| status | clean, synced with `origin/main` after fetch |
| paper | `GraphGPT: Graph Instruction Tuning for Large Language Models`, SIGIR 2024 full paper |
| authors | Jiabin Tang, Yuhao Yang, Wei Wei, Lei Shi, Suqi Cheng, Dawei Yin, Chao Huang |
| tracked files by `git ls-files` | 111 |
| Python files | 76 |
| markdown files | 4 |
| YAML files | 3 |
| JSON files | 2 |
| shell scripts | 7 |
| main folders | `graphgpt`, `text-graph-grounding`, `scripts`, `playground`, `tests`, `assets`, `images` |
| main model | `GraphLlamaForCausalLM` |
| graph encoder choices | `MPNN`, `clip_gcn_arxiv`, `clip_gt`, `clip_gt_arxiv`, `clip_gt_arxiv_pub` |
| base LLM | Vicuna 7B v1.1 / v1.5 in README examples |
| released checkpoint | `Jiabin99/GraphGPT-7B-mix-all` |
| released graph encoder | `Jiabin99/Arxiv-PubMed-GraphCLIP-GT` |
| released data | `Arxiv-PubMed-mix-NC-LP`, `GraphGPT-eval-instruction`, `All_pyg_graph_data`, `graph-matching` |
| validation | `py -m compileall -q graphgpt text-graph-grounding scripts tests playground` passed |
| runtime check | not run locally because full run needs Vicuna checkpoint, graph data, graph encoder checkpoint, CUDA, and large dependencies |

一句话先行：

```text
GraphGPT 把 LLaVA-style multimodal alignment 从 image-language 迁移到 graph-language：
graph encoder 负责把子图编码成 graph tokens，
graph projector 把 graph tokens 对齐到 LLM hidden size，
instruction tuning 让 Vicuna 学会用自然语言完成 node classification / link prediction 等图任务。
```

它的关键词是：

```text
text-graph grounding
dual-stage graph instruction tuning
CoT distillation
GraphLlama
graph tokens
```

## 它解决什么问题

普通 LLM 很擅长文本，但图任务有两个问题：

```text
1. 图结构不是自然语言序列。
2. 图任务需要利用 node / edge / neighborhood / topology。
```

如果只把图转成一段文本描述，LLM 会遇到几个限制：

```text
子图太大，prompt 很快爆上下文。
拓扑关系容易丢。
node features 和 edge_index 很难自然表达。
link prediction / node classification 很难只靠文字稳定完成。
```

GraphGPT 的思路是：

```text
不要只把 graph 描述成文字。
先用 graph encoder 得到 graph node embeddings，
再通过 graph projector 注入 LLM embedding space，
最后用 instruction tuning 教 LLM 如何回答图任务。
```

这和 LLaVA 很像：

```text
LLaVA:
  image encoder -> visual tokens -> LLM

GraphGPT:
  graph encoder -> graph tokens -> LLM
```

它的核心价值是把 graph structure 从外部数据变成 LLM 可消费的内部 token。

## 总体结构

GraphGPT 可以拆成六层：

| Layer | Code | Role |
|---|---|---|
| Text-Graph Grounding | `text-graph-grounding` | 训练图编码器，让 graph embedding 对齐文本语义 |
| Graph Encoder | `graphgpt/model/graph_layers` | MPNN / GCN / graph transformer，对子图编码 |
| GraphLlama | `graphgpt/model/GraphLlama.py` | Llama/Vicuna + graph tower + graph projector |
| Data Pipeline | `graphgpt/train/train_graph.py` | 从 instruction JSON 加载子图、展开 graph tokens |
| Two-Stage Tuning | `scripts/tune_script` / `scripts/tune_script_light` | stage 1 graph matching，stage 2 NC/LP 任务调优 |
| Evaluation / Serving | `graphgpt/eval`, `graphgpt/serve` | 评估 NC/LP，提供 CLI/Web/API serving |

主链路可以写成：

```text
instruction JSON
  -> read graph field
  -> sample subgraph node_list and edge_index
  -> load node features from all_graph_data.pt
  -> Data(graph_node, edge_index, target_node)
  -> graph encoder
  -> graph_projector
  -> <g_start> <g_patch> ... <g_end>
  -> Vicuna / GraphLlama
  -> natural-language answer
```

其中 graph patch token 是：

```text
<graph>
<g_patch>
<g_start>
<g_end>
```

`<graph>` 会根据子图节点数展开成一串 `<g_patch>`，如果启用 start/end，则变成：

```text
<g_start><g_patch><g_patch>...<g_end>
```

然后模型 forward 时，用 graph embeddings 替换这些 token 位置。

## Component 1: Text-Graph Grounding

目录：

```text
text-graph-grounding
```

这是 GraphGPT 的底层对齐步骤。

它做的事情类似 CLIP：

```text
graph encoder output
  <-> node text embedding
  <-> neighbor text embedding
```

训练代码在：

```text
text-graph-grounding/main_train.py
```

核心 loss 有三项：

```text
node_loss: graph feature vs source node text
gt_loss:   graph feature vs target neighbor text
tt_loss:   source node text vs target neighbor text
```

总 loss：

```text
all_loss = node_loss + edge_coef * gt_loss + edge_coef * tt_loss
```

这一步的意义是：

```text
让 graph encoder 输出的向量和自然语言语义处在可对齐的空间。
```

如果没有这一步，graph encoder 可能只懂拓扑，不懂文本语义；LLM 可能只懂文本，不懂拓扑。Text-graph grounding 是桥。

对我们来说，这很关键。因为 Research OS / Quant OS 里的节点不是纯 ID，而是带文本语义的：

```text
paper title / abstract
repo README
factor formula
backtest diagnosis
event description
asset business description
risk explanation
```

所以未来如果做自己的 graph-language layer，也需要类似：

```text
node text <-> graph neighborhood <-> task label
```

的对齐。

## Component 2: Graph Encoder

GraphGPT 支持多种 graph tower：

```text
MPNN
clip_gcn_arxiv
clip_gt
clip_gt_arxiv
clip_gt_arxiv_pub
```

主要代码在：

```text
graphgpt/model/graph_layers
```

其中 `clip_graph.py` 里有 CLIP-style text-graph model，`graph_transformer.py` 里有 graph transformer。

`graph_transformer` 的输入是 PyG-style graph：

```text
Data(
  graph_node = node_features,
  edge_index = edge_index,
  target_node = target_node
)
```

它先把 node features 投影到 attention dimension：

```text
W_P: gnn_input -> att_d_model
```

再经过若干 `GTLayer`。`GTLayer` 在图边上做 attention：

```text
rows, cols = g.edge_index
q = row embeddings
k = col embeddings
v = col embeddings
edge-wise attention
aggregate back to row nodes
residual + layer norm
```

最后再投回：

```text
inverW_P: att_d_model -> gnn_output
```

这和 OpenGraph 的 graph transformer 不完全一样。OpenGraph 更关注跨图泛化和 anchor-sampled global attention；GraphGPT 这里更关注把 graph encoder 作为 LLM 的 graph tower，服务 instruction tuning。

## Component 3: GraphLlama

核心文件：

```text
graphgpt/model/GraphLlama.py
```

关键类：

```text
GraphLlamaConfig
GraphLlamaModel
GraphLlamaForCausalLM
```

它的结构是：

```text
LlamaModel
  + graph_tower
  + graph_projector
  + graph special tokens
```

`GraphLlamaModel.forward` 的核心逻辑：

```text
1. input_ids 先变成 text embeddings。
2. graph_tower 对 graph_data 编码，得到 node-level graph features。
3. graph_projector 把 graph features 投到 LLM hidden size。
4. 在 input_ids 中找到 <g_patch> 或 <g_start>/<g_end> 的位置。
5. 用 graph features 替换这些 token embeddings。
6. 送入 Llama decoder。
```

也就是说，它不是把图写成一段 prompt，而是真的把图向量插入 LLM 的 embedding 序列。

这一步就是 GraphGPT 的核心工程点。

用一句话概括：

```text
GraphGPT 把 graph encoder 输出伪装成 LLM 能读的连续 token。
```

这也是它和纯文本 graph prompting 的区别。

## Component 4: Instruction Data Pipeline

训练数据格式大致是：

```json
{
  "id": "dataset_split_nodeidx_tasktype",
  "graph": {
    "node_idx": 0,
    "edge_index": [[...], [...]],
    "node_list": [...]
  },
  "conversations": [
    {"from": "human", "value": "Given a citation graph: <graph> ..."},
    {"from": "gpt", "value": "..."}
  ]
}
```

如果是 link prediction，数据里会有两个图：

```text
edge_index_1
node_list_1
node_idx_1

edge_index_2
node_list_2
node_idx_2
```

训练时：

```text
graph_type = id.split("_")[0]
graph_node_rep = graph_data_all[graph_type].x[node_list]
```

然后构造：

```text
Data(graph_node=graph_node_rep, edge_index=edge_index, target_node=target_node)
```

如果是 LP，则构造：

```text
{
  "graph_1": Data(...),
  "graph_2": Data(...)
}
```

这说明 GraphGPT 的 instruction data 不是纯 JSON 文本，而是：

```text
instruction text + graph pointer + graph tensor store
```

这对我们很有启发。未来我们自己的 Research OS 也可以这样组织：

```text
instruction:
  "判断这个 factor 是否属于 momentum family，并解释原因"

graph pointer:
  factor node id
  local factor-neighborhood edge list
  related backtest / risk / data source nodes

tensor store:
  factor features
  node text embeddings
  graph embeddings
```

这样自然语言任务和结构化图数据就能连起来。

## Component 5: Dual-Stage Graph Instruction Tuning

GraphGPT 的 tuning paradigm 有两阶段。

Stage 1:

```text
self-supervised instruction tuning
```

README 里用的是 graph matching：

```text
graph_matching.json
```

训练脚本：

```text
scripts/tune_script/graphgpt_stage1.sh
```

主要目标是训练：

```text
graph_projector
special graph token embeddings
```

让 LLM 初步学会把 graph tokens 和文本任务对齐。

Stage 2:

```text
task-specific instruction tuning
```

数据包括：

```text
node classification
link prediction
mixing data for multitasking
CoT instruction data
```

训练脚本：

```text
scripts/tune_script/graphgpt_stage2.sh
```

Stage 2 会加载 Stage 1 提取出来的 projector：

```text
pretrain_graph_mlp_adapter
```

并在具体任务上继续 tuning。

这个设计很像：

```text
Stage 1: learn modality alignment
Stage 2: learn task behavior
```

对我们未来做 Quant GraphGPT 也很自然：

```text
Stage 1:
  factor / paper / repo / asset graph matching

Stage 2:
  factor family classification
  factor failure diagnosis
  asset-event link prediction
  next experiment recommendation
  portfolio risk explanation
```

## Component 6: CoT Distillation

README 特别提到：

```text
Chain-of-Thought (CoT) Distillation
```

原因是 graph data 有 distribution shift：

```text
不同图的类别数不同
结构模式不同
节点语义不同
任务形式不同
```

如果模型只输出答案，很容易不稳。CoT 的作用是让模型生成：

```text
step-by-step reasoning
```

这对图任务尤其重要，因为图任务的正确答案通常需要解释：

```text
为什么这个节点属于某个类别？
为什么这两个节点应该连接？
哪几个邻居支持这个判断？
哪些结构证据和文本证据共同导致结论？
```

这和我们的 PM review / human audit 非常接近。

Quant Research OS 里也不应该只输出：

```text
buy / sell
factor good / bad
class = momentum
```

而要输出：

```text
evidence
reasoning path
risk caveat
failure mode
next validation
```

GraphGPT 的 CoT distillation 给了这个方向。

## Evaluation 与 Serving

评估代码在：

```text
graphgpt/eval
```

主要包括：

```text
run_graphgpt.py
run_graphgpt_LP.py
run_vicuna.py
cal_metric_arxiv.py
```

评估脚本会：

```text
1. 读取 eval instruction JSON。
2. 根据 instruction 里的 graph 字段加载子图。
3. 把 <graph> 替换成 graph patch tokens。
4. 加载 GraphLlamaForCausalLM。
5. 手动加载 graph tower。
6. generate 输出。
7. 写入 JSON 结果。
```

Serving 代码在：

```text
graphgpt/serve
```

它大量继承 FastChat/LLaVA 风格，包含：

```text
controller
model_worker
openai_api_server
gradio web server
CLI
gateway
monitor
```

这说明 GraphGPT 不只是离线训练代码，也有部署和 demo 形态。

不过从本地阅读看，真正最关键的还是：

```text
graph instruction tuning pipeline
```

Serving 层更多是沿用 FastChat 的服务框架。

## 和 GraphAgent / OpenGraph 的区别

三者可以这样分工：

| Project | 更像什么 | 关键问题 |
|---|---|---|
| GraphAgent | agentic graph-language assistant | agent 如何规划、构图、执行 graph task？ |
| OpenGraph | graph foundation model | graph model 如何跨图 zero-shot generalize？ |
| GraphGPT | graph instruction tuning | LLM 如何通过 instruction tuning 学会图任务？ |

如果组合成一个系统：

```text
GraphAgent:
  任务规划和图生成

OpenGraph:
  更开放的图表示和跨图泛化

GraphGPT:
  图结构和自然语言任务对齐
```

对我们的 Research OS / Quant OS，三者可以对应：

```text
Planner:
  GraphAgent-style task planning

Graph Encoder:
  OpenGraph-style graph representation

LLM Interface:
  GraphGPT-style instruction tuning and explanation
```

这是非常完整的一条路线。

## 对 Pengyi Research OS 的意义

Research OS 需要把大量研究对象组织成图：

```text
paper
repo
method
dataset
benchmark
author
lab
project
PR opportunity
research question
```

但只存图还不够。我们需要能问：

```text
这个 repo 应该归到哪条研究主线？
这个 paper 和哪个 project 最相关？
这个 idea 应该连接哪些已有技术？
这个 PR opportunity 是否值得做？
这个 project 能否支撑 RA / PhD application narrative？
```

GraphGPT 给我们的启发是：

```text
把这些问题做成 graph instruction data。
```

例如：

```json
{
  "instruction": "Given a research project graph: <graph>, classify the target repo into one research track and explain why.",
  "graph": "local neighborhood around repo node",
  "answer": "Graph / Knowledge Graph, because ..."
}
```

这就能把我们现在写的 HKUDS 学习地图变成训练数据源。

也就是说，我们的网站文章不只是输出成果，还可以反过来成为：

```text
Research OS graph instruction dataset
```

## 对 Pengyi Quant Research OS 的意义

Quant OS 更适合做 graph instruction tuning。

可以构造很多任务：

```text
Node classification:
  给一个 factor node，判断它属于哪个 factor family。

Link prediction:
  给两个 asset / event / factor nodes，判断是否存在有效关系。

Failure diagnosis:
  给一个 backtest-neighborhood graph，解释 alpha 为什么失效。

Risk explanation:
  给一个 portfolio exposure graph，解释主要风险来源。

Next experiment planning:
  给一个 factor research graph，生成下一轮实验计划。
```

这些任务都可以写成：

```text
Given a quant research graph: <graph>
...
```

然后让模型输出：

```text
classification
prediction
diagnosis
reasoning
next action
```

这和我们一直说的 R&D Agent 完全接上：

```text
自动提出因子假设
自动实现
自动回测
自动诊断偏差
自动生成下一轮研究计划
人类 PM 审核
```

GraphGPT 对应的是：

```text
让 LLM 看懂 factor / asset / event / backtest graph，并能用语言解释和规划。
```

## 我们怎么吸收

短期不复刻 GraphGPT-7B。成本太高。

我们可以做轻量版：

```text
1. 把 HKUDS / LLMQuant 学习文章抽成 graph JSON。
2. 为每个 graph neighborhood 生成 instruction-answer pairs。
3. 先用普通 LLM 做 graph-to-text prompt，不训练模型。
4. 做一个 small benchmark：project classification / relation prediction / PR opportunity ranking。
5. 积累足够数据后，再考虑 LoRA / projector / graph encoder。
```

第一版甚至可以不用 graph tokens，只做：

```text
graph serialized as JSON / edge list / path list
```

但数据格式要按 GraphGPT 的思路设计：

```text
instruction + graph pointer + answer + reasoning
```

这样以后可以平滑升级到真正的 graph-token model。

最重要的是：

```text
从现在开始，我们写的每篇学习笔记都可以成为 future graph instruction data。
```

## 可以提 PR 的地方

GraphGPT 目前有一些具体、可落地的小改进点。

第一，README 的 code structure 里出现了：

```text
pyproject.toml
```

但本地仓库没有这个文件。可以修正文档，或者补一个最小 `pyproject.toml`。

第二，`requirements.txt` 里包含一些 OS/Ubuntu 绑定包：

```text
python-apt==1.6.5+ubuntu0.7
PyGObject==3.26.1
unattended-upgrades==0.1
```

这些在普通 pip 环境里不一定能安装。可以拆成核心 requirements 和 platform-specific notes。

第三，README 和 `scripts/tune_script/graphgpt_stage2.sh` 里有一行：

```text
--use_graph_start_end True\
```

`True` 和反斜杠之间缺少空格，shell 里可能会把它拼成异常参数。应该改成：

```text
--use_graph_start_end True \
```

第四，评估脚本里硬编码了 graph tower 路径：

```text
load_model_pretrained(CLIP, './clip_gt_arxiv_pub')
```

更合理的是加一个 CLI 参数：

```text
--graph_tower_path
```

第五，评估和模型代码里大量直接 `.cuda()`，比如：

```text
model.cuda()
input_ids.cuda()
graph_data.cuda()
```

这会限制 CPU/MPS/多 GPU device mapping。可以统一用 `device` 参数。

第六，`GraphLlama_pl.py` 里疑似有变量名问题：

```text
target_modules=find_all_linear_names(model)
model = get_peft_model(model, lora_config)
```

但这个作用域里主要对象是 `self.model`，不是 `model`。如果启用 LoRA，可能触发错误。需要实际验证后提 PR。

第七，`GraphLlama_pl.py` 里：

```text
self.model.config.pretrain_graph_model_path = self.model.config.pretrain_graph_model_path + model_args.graph_tower
```

这依赖 base config 里已经有 `pretrain_graph_model_path`，README FAQ 也提到过这个问题。更稳妥的是显式传参或检查属性存在。

第八，`run_graphgpt.py` / `run_graphgpt_LP.py` 中每个 worker 都在函数内部加载 `graph_data_all = torch.load(graph_data_path)`，而 `load_graph` 每次调用都会加载一次。可以把 graph data 缓存在 worker 级别，避免重复 IO。

第九，`run_graphgpt_LP.py` 结果写入时总是使用：

```text
"node_idx_1"
"node_idx_2"
```

但如果不是 LP 路径，可能和数据结构不完全匹配。代码里虽然按 task_type 分支加载图，但最后 append 逻辑没有完全分支化，值得检查。

这些都适合从 docs / scripts / eval ergonomics 入手，不需要碰核心算法。

## 和我们的主线连接

GraphGPT 对我们的最大启发是：

```text
未来的 Quant Research OS 不能只让 LLM 读文本。
它应该让 LLM 读 graph-conditioned instruction。
```

这会把我们的研究系统从：

```text
chat with notes
```

推进到：

```text
chat with structured research graph
```

再推进到：

```text
train / tune agent on graph-grounded research tasks
```

完整路线可以是：

```text
Stage 1: Graph Memory
  把文章、repo、factor、backtest 抽成 graph。

Stage 2: Graph Instruction Dataset
  为 graph neighborhood 生成 question-answer-reasoning pairs。

Stage 3: Graph-to-Text Reasoning
  用普通 LLM 做 graph prompt + answer。

Stage 4: Graph Token Alignment
  用 GraphGPT-style projector / graph tower 对齐 LLM。

Stage 5: Quant GraphGPT
  专门回答因子、事件、风险、回测诊断、下一步计划。
```

这条路非常长，但方向是对的。

## 下一步

图智能主线剩下：

```text
HKUDS027 -> HiGPT
```

这会补上 heterogeneous graph intelligence。

目前三篇可以组成一个小闭环：

```text
GraphAgent: graph agent workflow
OpenGraph:  graph foundation representation
GraphGPT:   graph-language instruction tuning
```

接下来 HiGPT 可以回答：

```text
heterogeneous graph 怎么进入 LLM / graph intelligence？
```

这正好连接 QuantMind / factor graph / Research OS memory layer。
