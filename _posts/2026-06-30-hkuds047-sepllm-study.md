---
title: "HKUDS047: SepLLM 作为 Long-Context Compression、KV Cache Efficiency 与 Research OS Memory Compression Layer"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds047, hkuds, sepllm, long-context, kv-cache, sparse-attention, sep-cache, memory-compression, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS047`。

```text
HKUDS047 -> SepLLM
```

上一篇是：

```text
HKUDS046 -> LightReasoner
```

`LightReasoner` 讲的是：

```text
reasoning efficiency
```

这一篇看 `SepLLM`。

`SepLLM` 讲的是：

```text
context efficiency
```

一句话定位：

```text
SepLLM = separator-aware sparse attention
       + KV cache compression
       + long-context / streaming inference acceleration
       + training-free, streaming, and training-from-scratch variants
```

更直白一点：

```text
不是所有 token 都值得长期留在上下文里。
SepLLM 认为很多 segment 的信息可以被压到 separator token 上。
因此 long context 可以保留：
1. 初始 anchor tokens
2. separator tokens
3. 最近 local window
其余 token 可以被 mask 或从 KV cache 中淘汰。
```

这篇进入的是：

```text
Long Context / KV Cache / Sparse Attention / Memory Compression
```

这条线对我们很关键。

因为 `Research OS` 和 `Quant Research OS` 最后都会遇到同一个问题：

```text
信息越来越多，不能无限塞进上下文。
```

所以需要一个 memory compression layer。

`SepLLM` 给我们的启发不是“只在 LLM 推理里省显存”。

它更像一个系统级思想：

```text
把连续上下文压缩成少量高信息量锚点。
```

## 为什么 HKUDS047 看 SepLLM

到目前为止，我们已经看了很多 HKUDS 项目：

```text
LightRAG        -> knowledge graph / retrieval
VideoRAG        -> multimodal retrieval
DeepCode        -> code understanding
FastCode        -> code speedup
AutoAgent       -> agent runtime
OpenHarness     -> agent benchmark / harness
CatchMe         -> personal context capture
LightReasoner   -> reasoning efficiency
SepLLM          -> context / KV cache efficiency
```

这里有一个自然递进：

```text
CatchMe 负责记录。
LightRAG / VideoRAG 负责检索。
LightReasoner 负责挑高价值 reasoning token。
SepLLM 负责把长上下文压缩到可运行的 memory budget 里。
```

我们之前一直在说：

```text
Research OS 要有长期记忆。
Quant OS 要有长期研究轨迹。
R&D Agent 要能读 paper、写代码、跑回测、诊断偏差、生成下一轮计划。
```

但是如果所有东西都无限堆进去，系统一定会失控。

真正需要的是：

```text
memory budget
```

也就是：

```text
哪些信息常驻？
哪些信息保留摘要？
哪些信息留在冷存储？
哪些信息只在 local window 中暂时存在？
```

`SepLLM` 给了一个非常直接的 analog：

```text
initial tokens   -> task / system / project anchor
separator tokens -> segment-level compressed memory
local window     -> current active working context
evicted tokens   -> cold storage or ignored details
```

这对我们的 `Pengyi Research OS v0` 非常有启发。

## 本次阅读状态

本次阅读的是本地 HKUDS 工作区里的 `SepLLM`。

网络恢复后重新同步，当前本地已经是远端最新。

| item | value |
| --- | --- |
| repo | `SepLLM` |
| remote | `https://github.com/HKUDS/SepLLM.git` |
| branch | `main` |
| latest commit | `f250f59503f15cb26f7b1a6e83e12c20ff069eeb` |
| commit date | `2025-07-29T23:26:28+08:00` |
| latest message | `Update README.md` |
| tracked files | 576 |
| Python files | 121 |
| Markdown files | 8 |
| shell scripts | 82 |
| YAML files | 39 |
| local size | about `1010.8 MB` |

目录体积大，主要不是因为核心代码复杂，而是因为 repo 自带了工程运行材料：

| directory | size | file count | role |
| --- | ---: | ---: | --- |
| `package` | about `576.97 MB` | 10 | packaged `transformers` wheel and DeepSpeed / DeeperSpeed wheels |
| `Streaming-SepLLM` | about `51.39 MB` | 177 | streaming inference demo, PG19 data, KV cache manager |
| `TrainingFree-SepLLM` | about `0.21 MB` | 30 | Llama 3 training-free scripts and configs |
| `Training-SepLLM` | about `20.63 MB` | 356 | GPT-NeoX style training stack, sparse attention, configs, tests |

`package` 里最大的文件是多个 DeepSpeed / DeeperSpeed wheel。

`SepLLM` 自己的 patched `transformers` wheel 大约 `8.23 MB`。

这说明：

```text
repo 大，不等于核心思想臃肿。
核心思想集中在 attention mask、KV cache manager、SepCache、training configs 这些文件里。
```

## 项目一句话

README 的标题已经很清楚：

```text
SepLLM: Accelerate Large Language Models by Compressing One Segment into One Separator
```

核心观察是：

```text
一些看上去没有语义的 separator tokens，比如标点和换行，
在 attention score 中可能承担了比直觉更大的聚合角色。
```

所以 SepLLM 做的事情是：

```text
把 segment 中的信息压缩到 separator token 上。
推理时保留 separator token 的 KV。
丢掉大量非关键 token 的 KV 或 attention。
```

这就是：

```text
one segment -> one separator
```

## 三个版本

SepLLM repo 里最重要的是三个子目录：

```text
TrainingFree-SepLLM
Streaming-SepLLM
Training-SepLLM
```

它们不是重复实现，而是三种实验/应用场景。

| module | 作用 | 重点 |
| --- | --- | --- |
| `TrainingFree-SepLLM` | 不重新训练模型，直接在已有 Llama 3 上做 SepLLM 评测 | mask-based attention / SepCache |
| `Streaming-SepLLM` | 无限长度或超长 streaming 推理评测 | token-by-token generation / KV cache eviction |
| `Training-SepLLM` | 从训练阶段引入 SepLLM 架构 | GPT-NeoX training stack / SepAttention / flex attention kernel |

README 特别提醒：

```text
不要把 Streaming-SepLLM 当成普通 training-free 任务入口。
```

原因是：

```text
Streaming setting 通常需要 positional encoding shifting。
GSM8K、MMLU 这种普通 downstream task 通常没有超过模型预训练 max_position_embeddings。
这种情况下不应该直接套 Streaming-SepLLM。
```

这点很重要。

因为很多工程问题不是“代码能不能跑”，而是：

```text
场景假设是否匹配。
```

SepLLM 的目录拆分，本质就是在告诉使用者：

```text
general downstream task
streaming long-context task
training-from-scratch task
```

这三者不能混成一个入口。

## 核心机制

SepLLM 的核心上下文结构可以写成：

```text
active context =
    initial tokens
  + separator tokens
  + local window tokens
```

三个部分分别承担不同职责。

### Initial Tokens

`initial tokens` 类似 attention sink。

它们通常在序列开头。

代码里的参数是：

```text
init_tok_max_idx
```

如果 `init_tok_max_idx = 2`，意思就是保留 index `0, 1, 2` 三个初始 token。

在 streaming 版本里对应：

```text
init_cache_size
```

例如 demo 里经常设置：

```text
init_cache_size = 4
```

这类 token 对模型稳定性很重要。

在 Research OS analog 里，它们对应：

```text
project spec
system instruction
task objective
experiment protocol
```

也就是不应该被压掉的全局 anchor。

### Separator Tokens

`separator tokens` 是 SepLLM 的主角。

它们通常包括：

```text
.
,
?
!
;
:
space
tab
newline
```

不同 tokenizer 下 token id 不一样。

在 Llama 3 training-free config 里，separator ids 是：

```text
[13, 11, 30, 0, 26, 25, 198, 220, 128000]
```

在 Pythia / GPT-NeoX config 里，separator ids 是：

```text
[15, 13, 32, 2, 28, 27, 209, 186, 187]
```

这里有一个非常工程化的注意点：

```text
separator_token_ids 必须跟 tokenizer 匹配。
```

否则你以为保留的是分隔符，实际可能保留的是完全错误的 token。

在 Research OS analog 里，separator tokens 不一定是标点。

它们可以是：

```text
heading
experiment id
commit id
decision marker
backtest run id
bug root cause
paper section boundary
meeting conclusion
human review comment
```

也就是说，我们未来做 `Pengyi Research OS memory layer` 时，不应该机械地保留标点。

我们应该保留：

```text
真实承载 segment-level 信息的边界点和决策点。
```

### Local Window

`local window` 保留最近一段 token。

训练和推理配置里分别有：

```text
prefill_local_window_size
generate_local_window_size
```

Streaming KV cache manager 里对应：

```text
local_size
```

例如 demo：

```text
local_size = 256
```

它的意义是：

```text
最近发生的内容通常仍然需要细粒度上下文。
```

这和我们做人类工作也一样。

历史项目可以被压缩成若干关键节点，但当前正在 debug 的代码、当前正在跑的回测、当前正在写的邮件，必须保留更多局部细节。

## Training-Free 入口

`TrainingFree-SepLLM` 是最适合快速理解项目的入口。

它的用途是：

```text
不训练模型，直接把已有 Llama 3 变成 SepLLM-style sparse attention 模型做评测。
```

核心文件包括：

| file | role |
| --- | --- |
| `TrainingFree-SepLLM/demo.py` | `SepCache` 最小示例 |
| `TrainingFree-SepLLM/Llama3_trnfree_sepllm_configs/llama3_sepllm_a3_n256.yml` | Llama 3 SepLLM config |
| `TrainingFree-SepLLM/Llama3_trnfree_sepllm_configs/llama3_streamingllm_a3_n256.yml` | StreamingLLM baseline config |
| `TrainingFree-SepLLM/Llama3_trnfree_sepllm_configs/llama3_fixllm_a3_n256_int5.yml` | FixLLM baseline config |
| `TrainingFree-SepLLM/Llama3_8B_Instruct_SepLLM_a3_n256_gsm8k_cot_eager.sh` | eager attention GSM8K-CoT script |
| `TrainingFree-SepLLM/Llama3_8B_Instruct_SepLLM_gsm8k_cot_SepCache_a4_s128_w256_c512_with_flash_atten2.sh` | SepCache + flash attention script |

README 里说得很清楚：

```text
eager / sdpa 的 mask-based training-free method 方便研究 attention behavior，
但它本身并不真正减少 KV cache。
```

真正减少 KV cache 的是：

```text
SepCache
```

这点必须分清：

```text
mask-based SepLLM -> 研究和评估 attention pattern
SepCache          -> 真实减少 KV cache / GPU memory
```

## SepCache

`SepCache` 是 SepLLM 最实用的工程形态之一。

`TrainingFree-SepLLM/demo.py` 里展示了基本用法：

```text
from transformers import AutoTokenizer, AutoModelForCausalLM, SepCache

past_key_values = SepCache(
    init_cache_size=4,
    sep_cache_size=128,
    local_size=256,
    cache_size=512,
    layer_num=32,
    USE_MAX_SEP_CACHE=True,
    model_type='llama'
)
```

注意几个参数：

| parameter | meaning | paper alias |
| --- | --- | --- |
| `init_cache_size` | 保留初始 tokens 的 KV 数量 | `a` |
| `sep_cache_size` | 保留 separator tokens 的 KV 数量 | `s` |
| `local_size` | 保留最近 local window 的 KV 数量 | `w` |
| `cache_size` | 总 KV cache 上限 | `c` |
| `USE_MAX_SEP_CACHE` | 是否限制 separator cache 最大长度 | bounded separator memory |

README 里也提到，`SepCache` 已经有 HuggingFace Transformers Community 版本。

本 repo 里则通过：

```text
package/transformers-4.38.0.post1+sepllm-py3-none-any.whl
```

提供 patched transformers。

wheel 里包含这些关键文件：

```text
transformers/cache_utils.py
transformers/models/llama/modeling_llama.py
transformers/models/llama/sepllm_attention.py
transformers/models/llama/sepllm_forward_input.py
transformers/models/sepllm_gpt_neox/modeling_sepllm_gpt_neox.py
transformers/models/sepllm_gpt_neox/sepllm_attention.py
transformers/models/sepllm_gpt_neox/sepllm_forward_input.py
```

这说明 SepLLM 的 training-free 部分并不是全部源码直接放在 repo 根目录，而是打进了 transformers wheel。

这也是阅读这个项目时容易踩坑的地方：

```text
如果只看 TrainingFree-SepLLM 目录，会觉得核心代码不见了。
核心代码实际在 packaged transformers 里。
```

## Streaming-SepLLM

`Streaming-SepLLM` 是另一个非常关键的工程入口。

它模拟的是：

```text
token-by-token streaming generation
```

核心文件：

| file | role |
| --- | --- |
| `Streaming-SepLLM/main/evaluate_streaming_inputs_perplexity.py` | streaming PPL evaluation loop |
| `Streaming-SepLLM/sepllm_kv_cache/kv_cache_manager.py` | SepLLM KV cache manager |
| `Streaming-SepLLM/sepllm_kv_cache/utils.py` | CLI args, model loading, sanity checks |
| `Streaming-SepLLM/sepllm_kv_cache/pos_shift/modify_llama.py` | Llama positional encoding shifting |
| `Streaming-SepLLM/eval_sepllm_on_llama3_20K_demo1.1.sh` | Llama 3 20K token demo |

评测主循环是：

```text
for each text:
  tokenize
  for each token:
    model(input_ids, past_key_values=past_key_values, use_cache=True)
    get logits and loss
    update past_key_values
    update past token ids
    compress or evict KV cache
```

SepLLM 分支是：

```text
past_key_values = kv_cache(
    past_key_values,
    SEP_ACCUMULATION=True,
    USE_MAX_SEP_CACHE=True
)
```

StreamingLLM baseline 分支是：

```text
past_key_values = kv_cache.evict_nonlocal_and_noninitial(past_key_values)
```

这两个分支的区别是：

```text
StreamingLLM:
  keep initial tokens + local window

SepLLM:
  keep initial tokens + separator tokens + local window
```

这就是 SepLLM 相对 StreamingLLM 的核心增量。

## KV Cache Manager

`Streaming-SepLLM/sepllm_kv_cache/kv_cache_manager.py` 是这个项目最直观的核心代码之一。

里面的类是：

```text
SepLLM_KVCache_Manager
```

它做几件事：

```text
1. 记录 past token ids
2. 判断当前 KV cache 是否超过 cache_size
3. 把旧 window 中的 separator token 挑出来
4. 只保留 separator token 的 KV
5. 拼接 initial KV、separator KV、local KV
6. 返回压缩后的 past_key_values
```

核心函数：

| function | role |
| --- | --- |
| `update_past_tok_ids` | 维护历史 token ids |
| `compress_past_win_2_seps` | 从旧窗口中挑出 separator token 的 KV |
| `compress_kv_cache_and_tokids` | 执行完整 KV 压缩 |
| `evict_except_for_sep` | SepLLM eviction 主入口 |
| `evict_nonlocal_and_noninitial` | StreamingLLM baseline eviction |
| `slice_kv_cache_and_tokids` | 同步切 KV 与 token ids |
| `cat_kv_cache_and_tokids` | 拼回 cache 和 token ids |

用伪代码表达就是：

```text
if seq_len <= cache_size:
    keep everything
else:
    initial = first init_cache_size tokens
    past_window = tokens between old sep boundary and local window
    local = most recent local_size tokens

    new_seps = select separator tokens from past_window

    if SEP_ACCUMULATION:
        seps = old_seps + new_seps
    else:
        seps = new_seps

    if USE_MAX_SEP_CACHE:
        seps = last sep_cache_size separators

    cache = initial + seps + local
```

这里的工程感很强。

它不是抽象讲“压缩上下文”，而是真的在处理 tensor：

```text
key tensor
value tensor
sequence dimension
batch dimension
token id alignment
layer-wise cache
```

这对我们之后做自己的 memory system 很有参考价值。

因为 Research OS 也会遇到同样的问题：

```text
memory content 和 memory index 必须同步移动。
```

不能只压文本，不压索引。

不能只保留摘要，不保留 source span。

不能只改 cache，不改 retrieval metadata。

## Streaming 参数约束

`Streaming-SepLLM/sepllm_kv_cache/utils.py` 里有重要的 sanity checks。

如果启用 KV cache manager：

```text
cache_size > 0
init_cache_size >= 0
local_size > 0
cache_size >= init_cache_size + local_size
```

如果启用 SepLLM：

```text
sep_cache_size > 0
init_cache_size + sep_cache_size + local_size < cache_size
```

如果启用 StreamingLLM：

```text
local_size == cache_size - init_cache_size
```

这其实就是一个 memory budget contract。

在我们的系统里也应该这样设计：

```text
max_active_memory
project_anchor_budget
decision_marker_budget
local_context_budget
cold_storage_budget
```

每个 budget 都要有显式约束。

不能只靠 prompt 里说“请简洁”。

## Training-SepLLM

`Training-SepLLM` 是更重的部分。

它基于 GPT-NeoX 风格训练栈，支持：

```text
SepLLM training
StreamingLLM training
Self-Adjust Softmax training
Vanilla full attention training
BiPE variants
fused kernels
DeepSpeed distributed training
```

核心文件：

| file | role |
| --- | --- |
| `Training-SepLLM/megatron/model/sepllm_forward_input.py` | 把普通 attention mask 改成 SepLLM mask |
| `Training-SepLLM/megatron/sepllm_attention.py` | SepAttention mask builder and kernel builder |
| `Training-SepLLM/megatron/model/transformer.py` | transformer attention path, flex attention integration |
| `Training-SepLLM/megatron/model/gpt2_model.py` | GPT2ModelPipe forward hook |
| `Training-SepLLM/megatron/utils.py` | argument checker and mode constraints |
| `Training-SepLLM/megatron/neox_arguments/neox_args.py` | SepLLMArgs |
| `Training-SepLLM/sample_configs/` | 训练配置样例 |
| `Training-SepLLM/training_examples/` | launch scripts |
| `Training-SepLLM/downstream_evaluation/` | lm_eval evaluation scripts and logs |

### Forward Input Wrapper

训练时最关键的入口是：

```text
sepllm_forward_input_wrapper
```

位置：

```text
Training-SepLLM/megatron/model/sepllm_forward_input.py
```

它做的事是：

```text
input:
  input_ids
  position_ids
  attention_mask

output:
  SepLLM-compatible forward_input
```

内部流程：

```text
1. 获取 neox_args.sepAtten
2. 判断 prefill / decode
3. 记录 past_ids
4. 把 causal mask 转成 bool mask
5. 构造 segmented attention mask
6. 统计 KV / attention map retention ratio
7. 如果启用 kernel accelerator，则构造 sep_atten_kernel_func
8. 返回 transformer 可消费的新 forward input
```

这层很像一个 adapter。

它把原始 causal attention 转换成：

```text
SepLLM sparse attention
```

### SepAttention

`Training-SepLLM/megatron/sepllm_attention.py` 里的 `SepAttention` 是核心类。

它负责：

```text
1. separator token id 管理
2. local window 管理
3. initial token 管理
4. prefill mask 构建
5. generate mask 构建
6. layer-wise window 配置
7. BiPE position id 构建
8. flex attention block mask 构建
9. retention ratio 统计
```

最关键的两个函数是：

```text
build_prefill_mask
build_generate_mask
```

它们共同实现：

```text
visible tokens =
    initial tokens
  + separator tokens
  + local window tokens
```

`build_prefill_mask` 里先找 separator：

```text
sep_index_tensor = token_id in separator_token_ids
```

然后添加 initial tokens：

```text
mask[:, :, :, :initial_tok_size] = True
```

再添加 local window：

```text
win_mask = local triangular window
```

最后和 lower triangular causal mask 相交：

```text
result = (separator_or_initial_or_window) AND causal_mask
```

这保证模型不能看未来，同时只看被保留的稀疏上下文。

`build_generate_mask` 逻辑类似，只不过 decode 时 query length 通常是 1。

### Flex Attention Kernel

如果启用：

```text
USE_SEP_ATTN_KERNEL_ACCELERATOR = True
```

SepLLM 会通过 PyTorch `flex_attention` 的 `create_block_mask` 构造 sparse attention kernel。

代码路径是：

```text
SepAttention.get_sep_atten_kernel_funcs
  -> create_sep_atten_kernel_function
  -> torch.nn.attention.flex_attention.create_block_mask
```

然后在 transformer attention 里：

```text
context_layer = flex_attention(
    query,
    key,
    value,
    score_mod=pos_bias_ker_func,
    block_mask=sep_atten_kernel_func
)
```

这就是训练加速的关键。

普通 mask 只是逻辑上 mask 掉 attention。

kernel accelerator 才能真正减少 attention computation。

这和 training-free 部分的区别类似：

```text
logical sparsity != physical speedup
```

要真的加速，必须让底层 kernel 看到稀疏结构。

## Mode Checker

`Training-SepLLM/megatron/utils.py` 里的 argument checker 很值得学习。

它强制以下模式最多只能开一个：

```text
USE_ORIGINAL_FULL_ATTEN
streamingLLM
USE_SEP_ATTN_KERNEL_ACCELERATOR
USE_SA_SOFTMAX
USE_SA_SOFTMAX_NO_DENO
```

原因很简单：

```text
这些不是独立开关，而是互斥实验模式。
```

如果混开，会导致结果不可解释。

这里给我们的工程启发很明确：

```text
Research OS 的实验模式必须显式互斥。
```

例如 Quant R&D Agent 里：

```text
mode = hypothesis_generation
mode = implementation
mode = backtest
mode = bias_diagnosis
mode = portfolio_simulation
mode = report_generation
```

这些阶段可以串联，但不能在一次 run 里含混地同时承担所有职责。

否则日志、指标、失败原因都不可解释。

## SepLLM 与 StreamingLLM

SepLLM 和 StreamingLLM 非常接近，但差异关键。

StreamingLLM 保留：

```text
initial tokens + local window
```

SepLLM 保留：

```text
initial tokens + separator tokens + local window
```

StreamingLLM 更像：

```text
有 attention sink，再加最近上下文。
```

SepLLM 更像：

```text
有 attention sink，再加最近上下文，再加历史 segment 的压缩节点。
```

所以 SepLLM 对我们更像 memory system。

因为研究场景里，历史不是完全丢弃。

历史要被压缩成：

```text
milestone
decision
failure
conclusion
artifact
```

这就是 separator memory。

## SepLLM 与 LightReasoner

`HKUDS046 LightReasoner` 关注：

```text
哪些 reasoning token 值得学习？
```

`HKUDS047 SepLLM` 关注：

```text
哪些 context token 值得保留？
```

二者非常像。

共同点：

```text
都反对平均主义。
```

LightReasoner 说：

```text
不是每个 reasoning token 都同等值得训练。
```

SepLLM 说：

```text
不是每个 context token 都同等值得保留。
```

这对我们做 AI Scientist 很关键。

真正的研究能力不是无限堆东西。

真正的研究能力是：

```text
知道哪些东西是关键。
```

## SepLLM 与 CatchMe

`HKUDS045 CatchMe` 是个人数字足迹捕获系统。

它负责：

```text
record everything
```

SepLLM 给的是下一步：

```text
compress what was recorded
```

这两个项目可以组合成一个 Research OS memory pipeline：

```text
CatchMe:
  capture raw activity trace

SepLLM-style compressor:
  keep project anchors
  keep decision separators
  keep current local work window
  move raw trace to cold storage

LightRAG:
  retrieve when needed

LightReasoner-style selector:
  identify high-signal reasoning / decision points
```

这条 pipeline 很强：

```text
capture -> compress -> retrieve -> reason -> update plan
```

## 对 Pengyi Research OS 的启发

我们未来的 `Pengyi Research OS v0` 可以做一个 `SepMemory` 模块。

它不是模型层面的 KV cache，而是应用层的 research memory cache。

可以这样设计：

```text
SepMemory =
    project anchors
  + decision separators
  + current local workspace
  + cold storage pointer
```

### Project Anchors

对应 SepLLM 的 initial tokens。

它们包括：

```text
project objective
research question
dataset definition
evaluation metric
baseline protocol
open-source boundary
human PM rule
```

这些信息应该长期常驻。

### Decision Separators

对应 SepLLM 的 separator tokens。

它们包括：

```text
factor hypothesis accepted / rejected
backtest anomaly found
data leakage fixed
paper insight extracted
implementation decision made
PR feedback resolved
experiment conclusion written
```

它们是研究轨迹的分隔点。

真正有价值的不是所有中间日志，而是这些分隔点。

### Local Workspace

对应 SepLLM 的 local window。

它们包括：

```text
current file
current bug
current backtest output
current paper section
current conversation
current experiment run
```

这部分需要细粒度保留。

但是过一段时间后，它也应该被压缩成 separator memory。

### Cold Storage

被淘汰的 token 不等于删除。

在 Research OS 里应该进入：

```text
raw logs
full notebooks
full backtest artifacts
full meeting notes
full browser / coding activity traces
```

然后通过 retrieval 需要时再取回。

这就是：

```text
hot memory + compressed memory + cold storage
```

## 对 Quant Research OS 的启发

量化研究里 context explosion 更严重。

一个因子研究会产生：

```text
hypothesis notes
data cleaning scripts
feature code
backtest configs
IC / RankIC tables
turnover
drawdown
transaction cost sensitivity
industry neutralization results
out-of-sample results
failure diagnosis
portfolio combination notes
PM comments
```

如果全都放进 agent context，系统会很快崩。

SepLLM 给了一个很好的抽象：

```text
factor research memory =
    initial anchor:
        factor definition
        universe
        horizon
        benchmark
        cost assumption

    separators:
        each experiment conclusion
        each bug fix
        each data issue
        each regime insight
        each PM decision

    local window:
        current code / current run / current chart

    cold storage:
        all raw backtest outputs and logs
```

这样 R&D Agent 才能长期运行。

否则它每次都会被历史噪音拖垮。

## 对 R&D Agent 的启发

我们一直在设计：

```text
R&D Agent for Quant Research
= 自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ 人类 PM 审核
```

SepLLM 可以嵌入其中的 memory layer：

```text
Round 0:
  keep project anchor

Round 1:
  run factor hypothesis
  compress result into decision separator

Round 2:
  implement variation
  keep current code in local window
  compress old code discussion into separator

Round 3:
  run backtest
  store raw artifacts in cold storage
  keep summary metrics as separator

Round 4:
  diagnose bias
  mark leakage / overfit / turnover issue as separator

Round 5:
  human PM review
  promote PM decision to long-term anchor or separator
```

这就是：

```text
research process as compressed memory stream
```

## 一个可能的 SepMemory 数据结构

未来我们可以先做一个很小的版本：

```text
ProjectMemory
  id
  title
  anchors[]
  separators[]
  local_window[]
  cold_refs[]
```

`anchors`：

```text
[
  {type: "objective", text: "..."},
  {type: "metric", text: "..."},
  {type: "dataset", text: "..."}
]
```

`separators`：

```text
[
  {
    type: "experiment_conclusion",
    run_id: "...",
    timestamp: "...",
    summary: "...",
    cold_ref: "artifacts/run_001/"
  }
]
```

`local_window`：

```text
[
  {type: "active_file", path: "..."},
  {type: "current_error", text: "..."},
  {type: "current_question", text: "..."}
]
```

`cold_refs`：

```text
[
  {type: "notebook", path: "..."},
  {type: "backtest_output", path: "..."},
  {type: "raw_log", path: "..."}
]
```

这就是应用层的 SepLLM。

## 与 LLMQuant / QuantMind 的关系

我们之前看 `QuantMind` 时说过：

```text
QuantMind 更像 quant knowledge structuring。
```

SepLLM 可以补它的 memory compression 部分。

`QuantMind` 做：

```text
paper / news / pdf / blog -> structured quant knowledge
```

SepLLM-style memory 做：

```text
structured knowledge stream -> bounded active memory
```

两者结合：

```text
knowledge extraction + memory compression
```

这对 Quant Research OS 很关键。

因为我们最终想要的不是一个无限大的知识库。

我们想要的是：

```text
在有限上下文里保留最关键的研究状态。
```

## 可以快速应用的方向

### 方向 1：研究笔记压缩器

输入：

```text
一篇 paper note
一段 coding log
一轮 backtest report
一段导师/PM 沟通记录
```

输出：

```text
anchors
separators
local next actions
cold refs
```

这可以作为 `Pengyi Research OS` 的第一个小工具。

### 方向 2：因子研究 trace 压缩器

输入：

```text
factor idea
implementation diff
backtest csv
metric summary
diagnosis notes
```

输出：

```text
factor memory card
```

这个 card 只保留：

```text
what was tried
what worked
what failed
why failed
what to try next
raw artifact links
```

### 方向 3：Agent context budget planner

给每个 agent run 设置预算：

```text
anchor_budget = 20%
separator_budget = 40%
local_budget = 30%
retrieval_budget = 10%
```

然后让 agent 明确报告：

```text
哪些内容进入 anchor
哪些内容进入 separator
哪些内容留在 local
哪些内容只进 cold storage
```

这会让 long-running agent 更可控。

### 方向 4：PR 贡献方向

SepLLM 这个项目可以考虑的贡献点：

```text
1. 给 TrainingFree / Streaming / Training 三个目录补一张 mode selection guide
2. 把 demo.py 里的 HF login token 示例改成环境变量方式
3. 增加一个 separator id inspection 小工具
4. 增加一个 cache retention visualization demo
5. 增加一个 tiny-model smoke test，降低新用户上手成本
```

这些 PR 都比较务实。

但还是老原则：

```text
先真实使用，再提真实问题。
```

## 风险和限制

SepLLM 很强，但不能误用。

### 1. Mask 不等于加速

training-free 的 eager / sdpa mask-based method 很适合分析 attention。

但它不一定真的省 KV cache。

真正工程收益来自：

```text
SepCache
flex attention kernel
actual KV eviction
```

所以我们看论文和代码时要区分：

```text
logical sparsity
physical memory reduction
physical compute speedup
```

### 2. Separator 依赖 tokenizer

separator token id 必须和模型 tokenizer 对齐。

Llama 3 的 separator ids 不能直接拿去 Pythia 用。

这对我们做 Quant OS 也一样：

```text
不同数据源的 separator 不同。
```

论文、代码、backtest log、聊天记录、PDF 表格，不应该共用同一套 separator。

### 3. 标点不一定总是好 separator

自然语言里标点可能有效。

但在下面这些场景里需要重新设计：

```text
code
math proof
financial table
JSON
CSV
market microstructure data
trade blotter
multi-column PDF
```

例如量化里更好的 separator 可能是：

```text
timestamp
run id
metric block
section heading
error type
portfolio rebalance boundary
signal generation boundary
```

### 4. Streaming 不是普通评测入口

README 已经明确提醒：

```text
Streaming-SepLLM 是 tailored streaming design。
普通 downstream task 不应该直接用它替代 TrainingFree-SepLLM。
```

这是非常重要的实验严谨性。

## 这篇的核心结论

`HKUDS047 SepLLM` 对我们最大的启发是：

```text
long context 的关键不是无限变长。
关键是学会压缩。
```

SepLLM 的工程结构可以总结为：

```text
TrainingFree-SepLLM:
  fast evaluation / mask-based exploration / SepCache usage

Streaming-SepLLM:
  real KV cache eviction / long streaming evaluation / PE shifting

Training-SepLLM:
  training-time sparse attention / flex attention kernel / distributed training
```

它的抽象可以总结为：

```text
initial anchor + separator memory + local window
```

迁移到我们的 Research OS：

```text
project anchor + decision memory + active workspace
```

迁移到 Quant Research OS：

```text
factor definition + experiment conclusion + current run
```

迁移到 R&D Agent：

```text
objective + research milestones + current execution context
```

这就是 SepLLM 对我们的真正价值。

它不是一个孤立的 inference optimization 项目。

它是一种 memory budget 思想：

```text
keep what anchors the task
keep what summarizes the past
keep what is locally active
move the rest to cold storage
```

## 下一步

如果后面要继续深化，可以做两个小实验。

第一个是 SepLLM 项目本身：

```text
跑一个最小 SepCache demo，
观察 cache_size / sep_cache_size / local_size 改变时的 KV retention。
```

第二个是我们自己的 Research OS：

```text
写一个 SepMemory prototype，
把一篇 HKUDS 学习笔记压缩成 anchors + separators + local next actions + cold refs。
```

这会直接服务我们的长期目标：

```text
AI Scientist OS
Quant Research OS
R&D Agent
```

`HKUDS047 SepLLM` 的位置非常明确：

```text
它是 Research OS 的 memory compression layer。
```
