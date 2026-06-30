---
title: "HKUDS049: UrbanGPT 作为 Spatio-Temporal LLM、Urban Forecasting Foundation Model 与 Quant OS 时空预测层"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds049, hkuds, urbangpt, spatio-temporal, urban-ai, forecasting, instruction-tuning, quant-os, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS049`。

```text
HKUDS049 -> UrbanGPT
```

上一篇是：

```text
HKUDS048 -> MGP
```

`MGP` 讲的是：

```text
governed persistent memory
```

这一篇进入一条新的主线：

```text
Urban / Spatio-Temporal AI
```

`UrbanGPT` 的一句话定位：

```text
UrbanGPT = Spatio-Temporal Large Language Model
         = ST encoder + instruction tuning + forecasting token + numeric prediction head
         = use LLM to understand time, space, region semantics, and urban prediction tasks
```

更直白一点：

```text
普通 LLM:
  读文字很强
  但直接预测数值时间序列很弱

传统 ST-GNN:
  预测交通/城市数据很强
  但跨城市、跨区域、跨任务 zero-shot 泛化有限

UrbanGPT:
  把 spatio-temporal encoder 的结构化时空信号接入 LLM
  再用 instruction tuning 让 LLM 学会不同城市任务
  最后不直接吐数字文本，而是用 forecasting token hidden state 接预测头
```

这对我们非常关键。

因为这不是单纯的 Urban AI。

它给了我们一个可以迁移到 `Quant Research OS` 的模式：

```text
结构化时间序列信号
  -> domain encoder
  -> special token injection
  -> LLM reasoning / instruction following
  -> prediction token hidden state
  -> numeric prediction head
  -> evaluation / backtest
```

这就是一个可迁移的：

```text
time-series LLM adapter pattern
```

## 当前阅读状态

本地 repo：

```text
E:\2026\B面\香港大学\PENGYI笔记\PENGYI superCODEX PROJECT笔记！\hkuds\UrbanGPT
```

远程：

```text
https://github.com/HKUDS/UrbanGPT.git
```

当前同步状态：

```text
Already up to date.
```

最新 commit：

```text
79b834ed93763f74b6590703aeaaba39eb000126
2025-04-19T00:48:30+08:00
Update load_dataset.py
```

repo 规模：

```text
files = 90
python = 70
markdown = 3
shell = 3
pdf = 1
```

主要目录：

```text
checkpoints/
instruction_generate/
metric_calculation/
playground/
tests/
urbangpt/
```

目录体量：

| directory | files | size |
|---|---:|---:|
| root | 7 | 1.94 MB |
| checkpoints | 1 | 0.39 MB |
| instruction_generate | 8 | 6.44 MB |
| metric_calculation | 3 | 12.73 MB |
| playground | 7 | 0.01 MB |
| tests | 3 | near 0 |
| urbangpt | 61 | 0.58 MB |

项目状态：

```text
KDD 2024 accepted
paper PDF included locally
ST encoder checkpoint included locally
model weights / datasets are hosted on Hugging Face
```

## 论文问题

UrbanGPT 处理的是：

```text
spatio-temporal prediction under data scarcity
```

城市数据天然是时空数据。

例如：

```text
taxi inflow / outflow
bike inflow / outflow
crime occurrence
cross-region traffic flow
cross-city traffic flow
```

传统问题是：

```text
一个城市、一个区域、一个任务上训练出来的模型
换城市、换区域、换任务后就可能明显掉性能
```

原因很直接：

```text
1. 标注数据不够
2. 城市不同区域的功能不同
3. 时间周期不同
4. POI 和区域语义不同
5. 传统 ST 模型容易贴着训练分布
6. 普通 LLM 又不擅长直接处理数值时序和空间依赖
```

UrbanGPT 的目标是：

```text
让 LLM 获得 spatio-temporal prediction 能力
尤其是 zero-shot / cross-region / cross-city 泛化能力
```

论文里的核心判断是：

```text
LLM 有泛化和语义理解能力
ST encoder 有时空依赖建模能力
两者需要一个对齐机制
```

所以 UrbanGPT 不是让 LLM 直接读一串数字然后硬猜。

它做的是：

```text
historical ST data
  -> ST encoder
  -> ST dependency representation
  -> projector
  -> LLM embedding space
  -> instruction tuning
  -> forecast token hidden state
  -> regression / classification head
```

这条路径非常值得我们记下来。

## 为什么是 HKUDS049

之前我们已经看过：

```text
HKUDS020 FutureShow:
  forecasting agent / prediction benchmark

HKUDS024-027 GraphAgent / OpenGraph / GraphGPT / HiGPT:
  graph and structured reasoning

HKUDS028-031 RecLM / XRec / AutoCF / KGRec:
  recommendation and user behavior modeling

HKUDS045 CatchMe:
  personal trace capture

HKUDS047 SepLLM:
  long context compression

HKUDS048 MGP:
  memory governance
```

`UrbanGPT` 接上的位置是：

```text
structured temporal signal
structured spatial signal
domain encoder
LLM alignment
numeric forecasting
zero-shot transfer
```

这对我们有两层意义。

第一层是研究意义：

```text
AI scientist 不只是写文章和调 agent
还要能处理真实世界的结构化动态系统
```

第二层是量化意义：

```text
quant data 本质上也是 time-series / cross-asset / cross-market / cross-regime signal
```

所以 UrbanGPT 的模式可以抽象成：

```text
UrbanGPT for city
  city region x time x flow/crime/POI

QuantGPT for market
  asset/sector x time x price/volume/factor/news/regime
```

## 项目总览

核心文件：

```text
README.md
UrbanGPT_paper.pdf
urbangpt_train.sh
urbangpt_eval.sh

urbangpt/model/ST_Llama.py
urbangpt/model/st_layers/ST_Encoder.py
urbangpt/model/st_layers/ST_Encoder.conf
urbangpt/model/st_layers/args.py

urbangpt/train/train_mem.py
urbangpt/train/train_st.py
urbangpt/train/stchat_trainer.py

urbangpt/eval/run_urbangpt.py

instruction_generate/instruction_generate.py
instruction_generate/dataloader.py
instruction_generate/load_dataset.py
instruction_generate/add_window.py
instruction_generate/normalization.py

metric_calculation/result_test.py
metric_calculation/metrics.py
```

核心模块：

| module | role |
|---|---|
| `ST_Encoder` | 把历史时空序列编码成 spatio-temporal dependency representation |
| `st_projector` | 把 ST encoder 输出投影到 LLM hidden size |
| special tokens | `<ST_HIS>` / `<ST_PRE>` / `<ST_patch>` / `<ST_start>` / `<ST_end>` |
| `STLlamaModel` | 在 LLaMA/Vicuna embedding 序列中插入 ST 表征 |
| `STLlamaForCausalLM` | 在语言模型之上加入预测头和多任务损失 |
| instruction generator | 把城市数据转成 instruction tuning 数据 |
| evaluator | 用 Ray 多 GPU 跑预测并抽 hidden state 输出数值 |
| metrics | 计算 MAE / RMSE / MAPE / F1 / Recall |

## 不是纯文本预测

UrbanGPT 最重要的一点是：

```text
它不是让 LLM 直接输出未来 12 个数。
```

它让 LLM 输出：

```text
<ST_PRE>
```

然后拿 `<ST_PRE>` 位置附近的 hidden states，接数值预测头。

这解决了一个关键问题：

```text
LLM token prediction 是离散词表分类
时空预测通常是连续值回归或二分类
```

如果直接让 LLM 生成数字文本，会有几个问题：

```text
1. 数值精度不稳定
2. tokenization 对数字不友好
3. 多步预测很容易格式漂移
4. 语言建模损失和连续值回归目标不一致
```

UrbanGPT 的做法更工程化：

```text
LLM 负责融合语义、上下文、任务描述、时空 token。
numeric head 负责输出真正的连续预测值或分类概率。
```

这就是我们要记住的设计原则：

```text
LLM is not always the final numeric decoder.
LLM can be a reasoning and representation backbone.
```

## Special Tokens

源码中的核心 token：

```text
DEFAULT_STHIS_TOKEN = "<ST_HIS>"
DEFAULT_STPRE_TOKEN = "<ST_PRE>"
DEFAULT_ST_PATCH_TOKEN = "<ST_patch>"
DEFAULT_ST_START_TOKEN = "<ST_start>"
DEFAULT_ST_END_TOKEN = "<ST_end>"
```

语义：

| token | meaning |
|---|---|
| `<ST_HIS>` | historical spatio-temporal data placeholder |
| `<ST_PRE>` | predictive / forecasting token placeholder |
| `<ST_patch>` | ST representation patch token |
| `<ST_start>` | ST token span begin |
| `<ST_end>` | ST token span end |

instruction 中会出现：

```text
historical data as tokens <ST_HIS>
predictive tokens in the form "<ST_PRE>"
```

训练和评估时，代码会把它替换成：

```text
<ST_start><ST_patch><ST_patch><ST_end>
```

这里 `cur_token_len = 2`。

也就是说：

```text
每个区域的 ST 表征被注入成两个 patch token
通常对应 inflow / outflow 两个 feature
```

## ST Encoder

核心文件：

```text
urbangpt/model/st_layers/ST_Encoder.py
```

配置文件：

```text
urbangpt/model/st_layers/ST_Encoder.conf
```

默认配置：

```text
num_nodes = 80
input_window = 12
output_window = 12
output_dim = 2

gcn_true = False
buildA_true = False
dropout = 0.3
conv_channels = 32
residual_channels = 32
skip_channels = 64
end_channels = 128
layers = 3
```

注意这里：

```text
gcn_true = False
buildA_true = False
```

这说明它的 ST encoder 不是依赖显式图结构的复杂 GNN。

论文里的理由是：

```text
zero-shot 场景下，目标城市/区域的图结构可能未知或不稳定。
```

所以 UrbanGPT 更重视：

```text
temporal convolution
multi-level temporal patterns
semantic instruction
POI and region context
```

`ST_Enc` 主要结构：

```text
start_conv
filter_convs
gate_convs
residual_convs
skip_convs
end_conv_1
end_conv_2
```

`DilatedInception` 的 kernel set：

```text
[2, 3, 6, 7]
```

这意味着它会捕捉不同时间跨度的局部模式。

forward 流程：

```text
source
  -> transpose to batch, feature_dim, num_nodes, input_window
  -> start_conv
  -> gated temporal convolution
  -> residual connection
  -> skip connection
  -> end conv
  -> return prediction output and x_emb
```

返回：

```text
x       -> normal ST prediction output
x_emb   -> ST dependency embedding used by LLM side
```

这就是 UrbanGPT 的关键：

```text
ST encoder 不只是做预测
还要产出可以注入 LLM 的 hidden representation
```

## ST-LLaMA

核心文件：

```text
urbangpt/model/ST_Llama.py
```

它定义了：

```text
STLlamaConfig
STLlamaModel
STLlamaForCausalLM
```

`STLlamaModel` 继承：

```text
LlamaModel
```

`STLlamaForCausalLM` 继承：

```text
LlamaForCausalLM
```

核心新增部件：

```text
self.st_tower
self.st_projector
self.st_pred_linear_1
self.st_pred_linear_2
self.st_pred_linear_3
```

含义：

| component | role |
|---|---|
| `st_tower` | ST encoder |
| `st_projector` | ST hidden size -> LLM hidden size |
| `st_pred_linear_1` | historical ST token hidden -> prediction latent |
| `st_pred_linear_3` | forecasting token hidden -> prediction latent |
| `st_pred_linear_2` | concat latent -> future time steps |

预测头的形状逻辑：

```text
hidden state
  -> linear_1 / linear_3
  -> ReLU
  -> concat
  -> linear_2
  -> output_window time steps
```

它不是只看一个 token。

训练时会取两类 hidden state：

```text
1. historical ST span around st_start_id0
2. predictive ST span around st_start_id1
```

然后融合两者：

```text
st_pre_final = linear_2(concat(linear_1(history_hidden), linear_3(pred_hidden)))
```

这可以理解为：

```text
历史时空表征
+ LLM 经过 instruction reasoning 后的预测 token 表征
-> 数值预测
```

这是一个很好的 hybrid design。

## Embedding Replacement

`STLlamaModel.forward` 里最核心的是：

```text
1. 先把 input_ids 变成普通 token embeddings
2. 用 st_tower 编码 st_data_x 和 st_data_y
3. 按 region_start / region_end 选择当前区域
4. 用 st_projector 投影到 LLM hidden size
5. 找到 <ST_start> 位置
6. 把中间的 ST patch token embedding 替换成真实 ST features
7. 再调用 LlamaModel.forward
```

抽象为：

```text
text token embeddings
  + projected ST embeddings
  -> one fused LLM input embedding sequence
```

这和 `LLaVA` 的图像 token 注入思路很像。

但这里注入的不是 image patch。

而是：

```text
spatio-temporal patch
```

这对我们迁移到量化非常有启发。

未来我们也可以有：

```text
<FACTOR_HIS>
<MARKET_HIS>
<NEWS_REGIME>
<PORTFOLIO_STATE>
<BACKTEST_ARTIFACT>
```

然后用 domain encoder 替换这些 token 的 embedding。

## Loss Function

UrbanGPT 的训练损失不是单一语言建模损失。

在 `STLlamaForCausalLM.forward` 里：

```text
loss = language_modeling_loss
     + regression_loss
     + classification_loss
```

其中：

```text
language_modeling_loss = CrossEntropyLoss
regression_loss = MAE
classification_loss = BCEWithLogitsLoss
```

任务切换逻辑：

```text
task_type == 3 or task_type == 4:
  classification task
  crime prediction

else:
  regression task
  taxi / bike / traffic flow prediction
```

所以 UrbanGPT 同时支持：

```text
continuous value prediction
binary occurrence prediction
language instruction following
```

这就是它比普通 LLM forecasting prompt 更严肃的地方。

## Instruction Generation

核心文件：

```text
instruction_generate/instruction_generate.py
```

支持的数据集：

```text
NYCmulti    -> training
NYCtaxi     -> testing
NYCbike     -> testing
NYCcrime1   -> testing
NYCcrime2   -> testing
CHItaxi     -> testing
```

脚本参数：

```text
-dataset_name
-for_zeroshot
-for_supervised
-for_ablation
-his
-pre
-batch_size
-input_base_dim
-input_extra_dim
-part_of_region
-region_start
-region_end
```

默认窗口：

```text
historical window = 12
prediction window = 12
```

生成两个文件：

```text
generated_file/<dataset>.json
generated_file/<dataset>_pkl.pkl
```

JSON 负责：

```text
instruction conversations
```

PKL 负责：

```text
真实数值时空 tensor
```

每条 instruction 的 id 编码：

```text
train_<dataset>_region_<region_start>_<region_end>_len_<i>
```

评估/训练时通过 id 取回：

```text
region_start
region_end
i4data_all
```

这就是 text 和 tensor 的对齐索引。

## Temporal Instructions

脚本会把时间特征解码成自然语言。

包括：

```text
month
day
year
time of day
week day
interval
```

支持的时间粒度：

```text
5-minute intervals
30-minute intervals
1-day intervals
```

这对应不同任务。

例如：

```text
taxi / bike:
  30-minute or 5-minute intervals

crime:
  daily intervals
```

这里的启发是：

```text
时间戳不能只作为 one-hot feature。
它也可以作为 instruction 语义进入 LLM。
```

迁移到 quant：

```text
market open / close
earnings season
FOMC week
option expiry
month-end rebalance
holiday liquidity
crisis regime
```

这些都可以转成 instruction context。

## Spatial Instructions

脚本也会把区域信息解码成自然语言。

包括：

```text
borough / city
nearby POI categories
region granularity
```

NYC taxi 用：

```text
zone_poi.json
```

NYC bike / crime 用：

```text
region_poi.json
```

Chicago taxi 用：

```text
CHI_taxi_POIs.json
```

这说明 UrbanGPT 不只看数值序列。

它还把区域语义作为上下文：

```text
this region is in which borough
near which POI categories
what kind of functional area this may be
```

这是 zero-shot 的关键。

传统模型看到的是：

```text
node id
historical series
maybe adjacency
```

UrbanGPT 看到的是：

```text
node series
+ time semantics
+ region semantics
+ POI semantics
+ task instruction
```

这让它更容易跨区域迁移。

## Instruction Template

一个 taxi flow instruction 的结构大概是：

```text
Given historical taxi flow over 12 time steps in a region of NYC,
the recorded taxi inflows are ...
the recorded taxi outflows are ...
the recording time is ...
the region information is ...
now predict taxi inflow and outflow for the next 12 time steps ...
the ST model encodes historical taxi data as tokens <ST_HIS> ...
please generate predictive tokens in the form <ST_PRE>.
```

回答模板：

```text
Based on the given information,
the predictive tokens of taxi inflow and outflow in this region are <ST_PRE>.
```

注意：

```text
回答里也不直接放真实未来数值。
```

真实监督信号来自：

```text
st_data_y
```

也就是 PKL 里的 label tensor。

这就是文本 instruction 与数值监督的解耦。

## Training Pipeline

入口脚本：

```text
urbangpt_train.sh
```

默认配置：

```text
model_path = ./checkpoints/vicuna-7b-v1.5-16k
instruct_ds = ./data/train_data/multi_NYC.json
st_data_path = ./data/train_data/multi_NYC_pkl.pkl
pretra_ste = ST_Encoder
output_model = ./checkpoints/UrbanGPT
```

启动：

```text
python -m torch.distributed.run
  --nnodes=1
  --nproc_per_node=8
  --master_port=20001
  urbangpt/train/train_mem.py
```

关键训练参数：

```text
--version v1
--st_tower ST_Encoder
--tune_st_mlp_adapter True
--st_select_layer -2
--use_st_start_end
--bf16 True
--num_train_epochs 3
--per_device_train_batch_size 4
--learning_rate 2e-3
--model_max_length 2048
--gradient_checkpointing True
--lazy_preprocess True
```

`train_mem.py` 做的事很简单：

```text
1. monkey patch LLaMA attention with FlashAttention
2. import train from train_st.py
3. run train()
```

主训练逻辑在：

```text
urbangpt/train/train_st.py
```

训练主流程：

```text
parse HfArgumentParser
load STLlamaForCausalLM if st_tower is not None
load tokenizer
initialize ST modules
initialize ST tokenizer
build LazySupervisedDataset_ST
build STChatTrainer
train
save model
```

如果：

```text
tune_st_mlp_adapter = True
```

代码会：

```text
freeze full model
unfreeze st_projector
unfreeze st_pred_linear_1
unfreeze st_pred_linear_2
unfreeze st_pred_linear_3
```

这说明它倾向于：

```text
冻结大模型主体
训练 ST adapter 和预测头
```

这是一个实用工程策略。

原因：

```text
1. Vicuna 7B 全量训练成本高
2. domain alignment 主要发生在 projector / prediction head
3. ST encoder 已经预训练
4. 目标是让 LLM 接受 ST token，而不是重训 LLM 全部能力
```

## Dataset Loader

`LazySupervisedDataset_ST` 做了几件事：

```text
1. 读取 instruction JSON
2. 读取 ST tensor PKL
3. 从 id 解析 region_start / region_end / i4data_all
4. 把 <ST_HIS> 和 <ST_PRE> 替换成 patch token span
5. tokenize conversation
6. 返回 input_ids / labels / st_data_x / st_data_y / region_start / region_end
```

`DataCollatorForSupervisedDataset` 会 batch：

```text
input_ids
labels
attention_mask
st_data_x
st_data_y
region_start
region_end
```

这里的关键是：

```text
text token batch 和 tensor batch 同时进入 trainer。
```

这也是我们以后做 Quant OS 时需要的能力。

不能只有：

```text
prompt string
```

还要有：

```text
structured tensor payload
alignment key
artifact id
label tensor
evaluation metadata
```

## Evaluation Pipeline

入口：

```text
urbangpt_eval.sh
```

默认示例：

```text
output_model = ./tw2t_multi_reg-cla-gird
datapath = ./data/NYC_taxi/NYC_taxi.json
st_data_path = ./data/NYC_taxi/NYC_taxi_pkl.pkl
res_path = ./result_test/cross-region/NYC_taxi
start_id = 0
end_id = 51920
num_gpus = 8
```

核心文件：

```text
urbangpt/eval/run_urbangpt.py
```

评估流程：

```text
1. load prompting file
2. 按 num_gpus 切分
3. Ray remote 多 GPU eval
4. load STLlamaForCausalLM
5. set_st_tower
6. add special tokens
7. load st_data_all from pkl
8. 对每条 instruction 替换 <ST_HIS> / <ST_PRE>
9. model.generate
10. 抽取 hidden states
11. 用 st_pred_linear_1 / 2 / 3 输出数值预测
12. 保存预测结果
```

这里也很关键。

生成文本不是最终答案。

真正用于评估的是：

```text
hidden state -> prediction head -> st_pre_inflow / st_pre_outflow
```

## Metrics

核心文件：

```text
metric_calculation/result_test.py
metric_calculation/metrics.py
```

回归指标：

```text
MAE
RMSE
MAPE
```

分类指标：

```text
Accuracy
Precision
Recall
MicroF1
MacroF1
F1
```

论文评估任务：

```text
NYC-taxi
NYC-bike
NYC-crime
CHI-taxi
```

场景：

```text
zero-shot region
cross-city
supervised
ablation
robustness
case study
```

论文结果里最重要的结论是：

```text
UrbanGPT 在 zero-shot 场景下明显强于传统 ST baselines。
```

例如论文表格里：

```text
NYC-taxi zero-shot:
  UrbanGPT inflow MAE 6.16
  UrbanGPT outflow MAE 6.83

NYC-bike zero-shot:
  UrbanGPT inflow MAE 2.02
  UrbanGPT outflow MAE 2.01

NYC-crime zero-shot:
  UrbanGPT Macro-F1 0.67 / 0.69
```

这背后的核心不是某个指标本身。

而是：

```text
LLM semantic generalization + ST encoder structure prior
```

两者组合带来了 zero-shot 迁移。

## Ablation 的启发

论文 ablation 里有几个关键组件：

```text
-STC
-Multi
-STE
T2P
```

对应：

| ablation | removed / changed |
|---|---|
| `-STC` | 去掉 spatial / temporal context |
| `-Multi` | 不使用多数据源 instruction tuning |
| `-STE` | 去掉 spatio-temporal encoder |
| `T2P` | 让模型直接输出文本预测值，而不是 token hidden state + prediction head |

启发非常清楚：

```text
1. 时间和空间语义真的有用
2. 多任务/多数据源训练带来迁移能力
3. ST encoder 是必要的
4. 直接 text-to-prediction 不如 hidden-state-to-head
```

迁移到 Quant OS：

```text
1. market context 不能丢
2. 多市场/多资产/多周期训练很重要
3. time-series encoder 不能省
4. 预测最好接 numeric head 或 backtest evaluator
```

## 与普通 LLM Forecasting 的区别

很多 LLM forecasting 方法是：

```text
把历史数值序列转成文本
让 LLM 预测未来数字
```

UrbanGPT 更像：

```text
把历史数值序列转成 domain embedding
把 domain embedding 注入 LLM
让 LLM 产生 forecasting token representation
再用专业预测头输出未来值
```

两者差异很大。

前者是：

```text
prompt trick
```

后者是：

```text
model architecture
```

我们更应该学习后者。

因为在真正严肃的 quant / scientific prediction 里：

```text
只靠 prompt 让 LLM 预测数字，不够稳。
```

## 与 OpenCity 的关系

后面我们可能会看：

```text
OpenCity
```

它也是 spatio-temporal foundation model。

粗略区别：

| project | focus |
|---|---|
| UrbanGPT | ST data + LLM + instruction tuning + forecast tokens |
| OpenCity | ST foundation model for traffic prediction, more foundation-model / pretraining oriented |
| EasyST | distill complex ST-GNN into lightweight MLP |
| AutoST | automated spatio-temporal graph contrastive learning |

所以 UrbanGPT 是这条线的入口很好。

因为它最接近我们当前的主线：

```text
LLM + structured signal + forecasting + instruction tuning
```

## 与 GraphGPT / LLaVA 的关系

UrbanGPT 的实现风格明显受到多模态 LLM 的影响。

可以类比：

```text
LLaVA:
  image encoder -> projector -> LLM token embedding

GraphGPT:
  graph encoder -> projector -> LLM token embedding

UrbanGPT:
  ST encoder -> projector -> LLM token embedding
```

共同模式是：

```text
domain encoder
  -> alignment projector
  -> special token span
  -> LLM instruction tuning
```

这就是我们要抽象出来的东西。

未来我们可以有：

```text
QuantGPT:
  market encoder -> projector -> LLM token embedding

FactorGPT:
  factor panel encoder -> projector -> LLM token embedding

BacktestGPT:
  experiment artifact encoder -> projector -> LLM token embedding
```

## 对 Quant Research OS 的启发

UrbanGPT 对我们最直接的启发是：

```text
Quant 不是只能让 LLM 读研报、新闻、因子描述。
Quant 也可以把真实数值张量接入 LLM。
```

金融数据可以类比城市数据：

| UrbanGPT | Quant OS |
|---|---|
| region | asset / sector / market |
| city | exchange / country / asset universe |
| taxi inflow/outflow | volume / order flow / return / volatility |
| crime occurrence | event occurrence / drawdown / signal trigger |
| POI context | fundamentals / industry / macro tags / news regime |
| time interval | bar interval / trading session / earnings calendar |
| ST encoder | time-series encoder / panel encoder / graph encoder |
| `<ST_HIS>` | `<MARKET_HIS>` / `<FACTOR_HIS>` |
| `<ST_PRE>` | `<RETURN_PRE>` / `<RISK_PRE>` / `<SIGNAL_PRE>` |

可以设计：

```text
QuantUrbanGPT-like architecture:

historical factor panel
  -> factor/time-series encoder
  -> projected market tokens
  -> instruction with asset metadata, regime, event context
  -> LLM
  -> predictive token hidden state
  -> return / risk / direction / ranking head
```

这比单纯让 LLM 看一段行情文本更强。

## Quant 迁移要谨慎

但不能简单照搬。

金融市场和城市交通不同。

主要风险：

```text
1. 金融时间序列更非平稳
2. label leakage 风险更高
3. 交易成本和滑点会吞掉预测优势
4. regime shift 更剧烈
5. 公开数据训练出来的规律可能很快失效
6. prediction accuracy 不等于 strategy profitability
```

所以 Quant OS 迁移时必须增加：

```text
walk-forward validation
purged cross validation
transaction cost model
slippage model
capacity estimate
factor decay analysis
turnover analysis
out-of-sample regime diagnosis
```

UrbanGPT 给的是 architecture pattern。

不是直接给交易 alpha。

## 对 R&D Agent 的启发

我们的 R&D Agent 目前设想是：

```text
自动提出因子假设
自动实现
自动回测
自动诊断偏差
自动生成下一轮研究计划
人类 PM 审核
```

UrbanGPT 可以放进这个 loop：

```text
factor hypothesis
  -> build factor panel
  -> encode historical factor/return/risk tensor
  -> instruction with market/asset/regime context
  -> LLM fused representation
  -> prediction head
  -> backtest
  -> bias diagnosis
  -> next hypothesis
```

也就是说：

```text
LLM 不只负责写计划。
LLM 也可以成为结构化预测模型的一部分。
```

但是它要和：

```text
domain encoder
numeric head
evaluation harness
governed memory
audit trail
```

一起工作。

这正好把前面的 HKUDS 串起来：

```text
LightRAG:
  research knowledge retrieval

QuantMind:
  structured quant idea memory

MGP:
  governed persistent memory

UrbanGPT:
  time-series / spatio-temporal prediction model pattern

DeepResearch-Eval:
  evaluation and fact checking

OpenHarness:
  runtime and benchmark harness
```

## Pengyi Research OS 映射

UrbanGPT 可以映射成 Research OS 的一个层：

```text
Spatio-Temporal Prediction Layer
```

更具体：

```text
Research OS:
  paper / project / idea / experiment memory
  artifact generation
  evaluation

UrbanGPT-like layer:
  structured numerical signal
  domain encoder
  LLM alignment
  numeric prediction
  task-specific evaluation
```

如果我们做自己的系统，可以拆成：

```text
1. Signal Tensor Store
2. Domain Encoder
3. Prompt / Instruction Builder
4. Special Token Adapter
5. LLM Backbone
6. Prediction Head
7. Evaluation Harness
8. Experiment Memory
9. Audit / Governance
```

这就是：

```text
Quant Research OS v0 的预测模型子系统
```

## 小型复刻方向

不需要一上来复刻 UrbanGPT 7B。

我们可以做一个小型 `UrbanQuantGPT` demo：

```text
data:
  synthetic asset panel or public OHLCV sample

history:
  12 bars or 60 bars

prediction:
  next 1 / 5 / 12 bars return or volatility

encoder:
  small TCN / GRU / Transformer encoder

special tokens:
  <MARKET_HIS>
  <MARKET_PRE>

LLM:
  small open-source model or encoder-only bridge

head:
  regression head
  direction classification head

evaluation:
  MAE
  directional accuracy
  rank IC
  backtest return
  max drawdown
  turnover
```

最小目标：

```text
证明我们可以把真实数值 tensor 注入 LLM workflow。
```

不是追求一次就赚钱。

而是建立：

```text
engineering substrate
```

## 可 PR / 可改进点

本地阅读时发现几个工程风险。

第一，文件命名可能有不一致：

```text
existing file:
  urbangpt/model/ST_Llama.py

__init__.py import:
  from urbangpt.model.STLlama import STLlamaForCausalLM, load_model_pretrained
```

在严格大小写和精确文件名的环境里，这可能导致 import 问题。

第二，`builder.py` 里还有一些 `graphgpt` 引用：

```text
from graphgpt.model import *
from graphgpt.constants import ...
```

这可能是从 GraphGPT / LLaVA 系代码迁移后残留的命名。

第三，README 和 shell 脚本里的数据路径有轻微差异：

```text
README:
  ./ST_data_urbangpt/train_data/...

local shell:
  ./data/train_data/...
```

第四，复现依赖很重：

```text
Vicuna 7B
torch 2.0.1
CUDA 11.7
deepspeed
ray
flash-attn
torch_geometric
8 GPU training example
Hugging Face datasets / checkpoints
```

这些都不是论文思想的问题。

但如果要复现或提 PR，可以从：

```text
1. import path smoke test
2. README path consistency
3. minimal CPU data-generation test
4. task_type documentation
5. eval output schema documentation
```

这些低风险位置切入。

## 我们不应该误读它

UrbanGPT 不是：

```text
一个聊天机器人
```

也不是：

```text
一个靠 prompt 预测城市交通的 demo
```

它更像：

```text
spatio-temporal representation learning
+ LLM instruction tuning
+ numeric forecasting head
```

它也不是万能 zero-shot。

它的 zero-shot 建立在：

```text
1. 多数据源 instruction tuning
2. ST encoder 预训练
3. 城市区域/时间/POI 语义
4. 同类任务之间的 transferable patterns
```

所以迁移到 finance 时，我们也要问：

```text
哪些 pattern 是 transferable 的？
哪些只是 train universe 的过拟合？
哪些 context 能帮助模型理解 regime？
哪些 metadata 会造成 leakage？
```

这是严肃研究必须面对的问题。

## 与我们当前人生主线的关系

我们现在做 HKUDS study map，不只是收集项目。

我们是在建立自己的研究操作系统。

UrbanGPT 给的启发是：

```text
AI scientist 不能只做文本 agent。
AI scientist 也要能把真实世界的结构化数据接入 AI 系统。
```

对我们来说，真实世界包括：

```text
city data
financial market data
business operations data
career / organization data
research artifact data
```

这些都不是纯文本。

所以 Research OS 必须具备：

```text
text intelligence
structured signal intelligence
time-series intelligence
graph intelligence
memory governance
artifact evaluation
```

UrbanGPT 正好补上：

```text
time-series / spatio-temporal intelligence
```

## 一句话总结

```text
UrbanGPT 的核心不是 Urban。
UrbanGPT 的核心是：

把结构化时空信号通过 domain encoder 对齐进 LLM，
再用 forecasting token hidden state 接数值预测头，
从而获得跨区域、跨城市、跨任务的预测泛化能力。
```

对我们来说，它的最大价值是：

```text
Quant OS 可以学习这个架构。
```

也就是：

```text
不要只让 LLM 读行情文字。
要让 LLM 接入真实数值张量。
```

最终抽象：

```text
UrbanGPT:
  city time-space tensor -> ST tokens -> LLM -> forecast head

Pengyi QuantGPT:
  market/factor tensor -> market tokens -> LLM -> alpha/risk/backtest head
```

这就是 `HKUDS049 UrbanGPT` 的核心启发。

## 下一步

Urban / Spatio-Temporal 系列后面可以继续看：

```text
HKUDS050 -> OpenCity
HKUDS051 -> EasyST
HKUDS052 -> AutoST
```

这三篇可以形成一个完整对比：

```text
UrbanGPT:
  LLM + ST instruction tuning

OpenCity:
  traffic ST foundation model

EasyST:
  distillation from complex ST-GNN to lightweight model

AutoST:
  automated ST graph contrastive learning
```

这条线可以最后回到我们的目标：

```text
Quant Research OS 的时间序列预测层。
```
