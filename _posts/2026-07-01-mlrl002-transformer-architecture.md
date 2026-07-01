---
title: "MLRL002: Transformer 架构"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, AI Foundations]
tags: [pengyi-mlrl-map, mlrl002, transformer, attention, llm, deep-learning, model-architecture, ai-foundations]
---

这是 `PENGYI_ML_RL_MAP` 的第三篇：

```text
MLRL002 -> Transformer 架构
```

`MLRL001` 讲的是 PyTorch 训练底座。
这一篇讲当前 LLM 的核心模型结构：

```text
Transformer
```

我现在对 Transformer 的判断是：

```text
Transformer 是一种通过 attention 在 token 之间建立上下文关系的神经网络架构。
```

它的历史意义不只是“效果好”。
更关键的是，它把大规模并行训练、长上下文建模、语言建模、跨模态建模都放进了一个可扩展的结构里。

## 一句话定义

```text
Transformer = token embeddings processed by repeated attention and feed-forward blocks.
```

中文说：

```text
Transformer = token 表示经过多层 attention 和 MLP block，不断融合上下文，最后输出可用于预测或生成的表示。
```

对 decoder-only LLM 来说，整体链路是：

```text
text
  -> tokenizer
  -> token ids
  -> token embeddings
  -> positional information
  -> transformer blocks
  -> logits
  -> next token distribution
```

LLM 的生成，本质上是：

```text
根据已有上下文，反复预测下一个 token。
```

## Transformer 的总结构

一个典型 Transformer block：

```text
x
  -> LayerNorm
  -> Multi-Head Self-Attention
  -> Residual Add
  -> LayerNorm
  -> Feed-Forward Network
  -> Residual Add
```

堆叠很多层：

```text
embedding
  -> block 1
  -> block 2
  -> block 3
  -> ...
  -> block N
  -> output head
```

核心组件：

```text
Tokenization
Embedding
Positional information
Q / K / V projection
Scaled dot-product attention
Multi-head attention
Feed-forward network
Residual connection
Layer normalization
Causal mask
Output logits
```

如果只记一个结构图：

```text
tokens -> embeddings -> [attention + MLP] x N -> logits
```

## Tokenization: 文本变成 token

模型不能直接处理自然语言文本。
第一步是 tokenization：

```text
raw text -> tokens -> token ids
```

例如一句话：

```text
I love machine learning.
```

会被切成若干 token，然后映射到整数 id。

tokenizer 的意义：

```text
1. 定义模型的基本输入单位
2. 决定词表大小
3. 影响上下文长度利用效率
4. 影响中英文、多语言、代码的表示效率
```

对 LLM 来说，tokenizer 不是小事。
同样的文本，不同 tokenizer 会产生不同长度的 token 序列。
这会影响：

```text
context window cost
generation speed
multilingual quality
code modeling quality
rare word handling
```

## Embedding: token id 变成向量

token id 是离散整数，神经网络需要连续向量。

Embedding 层做的是：

```text
token id -> dense vector
```

如果词表大小是 `V`，hidden size 是 `d`，embedding table 就是：

```text
V x d
```

输入 token ids：

```text
[12, 305, 91]
```

查表后变成：

```text
[3, d] 的向量序列
```

Embedding 的直觉：

```text
每个 token 都有一个可训练的语义坐标。
```

Transformer 后续层会不断更新这些 token 表示，使它们融合上下文。

## Positional Information: 位置信息

纯 attention 本身不天然知道顺序。
所以需要加入位置相关信息。

常见方式：

```text
absolute positional embedding
relative positional encoding
RoPE
ALiBi
```

面试里不一定要展开每一种，但要能讲清楚：

```text
Transformer needs positional information because attention alone is permutation-invariant.
```

中文：

```text
如果不给位置信息，模型只知道有哪些 token，不知道 token 的顺序。
```

对 LLM 来说，位置编码还关系到长上下文能力。

## Attention 的核心直觉

Attention 解决的问题是：

```text
当前 token 应该关注上下文里的哪些 token？
```

例如：

```text
The bank approved the loan because it trusted the company.
```

模型需要判断：

```text
it 指代什么？
loan 和 company 的关系是什么？
bank 是银行还是河岸？
```

Attention 就是在 token 之间建立动态关联。

## Q / K / V

Self-attention 会从输入 `x` 线性变换出三组向量：

```text
Q = query
K = key
V = value
```

直觉：

```text
Q: 我现在想找什么信息？
K: 我能被什么查询匹配到？
V: 如果被关注，我实际提供什么内容？
```

计算过程：

```text
Q 和 K 做相似度匹配
  -> softmax 得到 attention weights
  -> 用 weights 对 V 加权求和
  -> 得到新的 token 表示
```

可以写成概念公式：

```text
Attention(Q, K, V) = softmax(QK^T / sqrt(d_k)) V
```

公式不用背死。
要理解：

```text
attention score 决定关注谁
value aggregation 形成上下文表示
```

## Multi-Head Attention

单个 attention head 只能从一个表示子空间看关系。
Multi-head attention 做的是：

```text
并行使用多个 attention head，让模型从不同角度建立 token 关系。
```

例如不同 head 可以关注：

```text
syntax relation
coreference
local phrase
long-range dependency
code indentation
function call relation
```

不是说每个 head 一定天然可解释。
但多头结构确实增加了模型同时建模多种关系的能力。

整体过程：

```text
x
  -> project to Q/K/V for each head
  -> compute attention per head
  -> concatenate heads
  -> output projection
```

## Causal Mask

decoder-only LLM 训练的是 next-token prediction。
它不能在预测当前位置时偷看未来 token。

所以需要 causal mask：

```text
token i can attend to tokens <= i
token i cannot attend to tokens > i
```

直觉：

```text
模型只能看过去，不能看未来。
```

这就是自回归生成的结构基础。

没有 causal mask，训练会泄露答案。
模型在训练时看到未来 token，就不是正常语言建模了。

## Feed-Forward Network

Attention 负责 token 之间的信息交换。
Feed-forward network 负责对每个 token 的表示做非线性变换。

常见结构：

```text
Linear d -> hidden
Activation
Linear hidden -> d
```

很多现代 LLM 使用门控结构，例如 SwiGLU 一类变体。
但抽象上仍然是：

```text
per-token nonlinear transformation
```

可以这样理解：

```text
Attention mixes information across tokens.
FFN transforms information within each token position.
```

## Residual Connection

Residual connection 做的是：

```text
output = x + sublayer(x)
```

它的作用：

```text
1. 改善深层网络训练
2. 保留原始信息通路
3. 让梯度更容易传播
```

Transformer 可以堆很多层，很大程度上依赖 residual connection 和 normalization。

## LayerNorm

LayerNorm 用于稳定训练。

常见两种结构：

```text
Post-LN
    x -> sublayer -> add -> norm

Pre-LN
    x -> norm -> sublayer -> add
```

现代大模型常见 Pre-LN 变体。
直觉是：

```text
在进入 attention 或 FFN 前先规范化表示，让训练更稳定。
```

LayerNorm、residual、optimizer、learning rate schedule 共同决定训练稳定性。

## Encoder-only、Decoder-only、Encoder-decoder

Transformer 有三种主流形态。

### Encoder-only

代表：

```text
BERT-style models
```

特点：

```text
双向看上下文
适合理解、分类、检索、embedding
常用 masked language modeling
```

典型任务：

```text
sentence classification
token classification
retrieval embedding
reranking
semantic matching
```

### Decoder-only

代表：

```text
GPT-style LLMs
```

特点：

```text
causal mask
next-token prediction
适合生成、对话、代码、tool use、agent
```

现在大多数通用 LLM 都是 decoder-only 或接近这个范式。

### Encoder-decoder

代表：

```text
T5-style models
```

特点：

```text
encoder 理解输入
decoder 生成输出
适合 sequence-to-sequence
```

典型任务：

```text
translation
summarization
structured transformation
text-to-text tasks
```

## Transformer 和 LLM

LLM 可以粗略看成：

```text
decoder-only Transformer
  + massive pretraining
  + instruction tuning
  + preference optimization
  + tool / memory / harness integration
```

预训练目标通常是：

```text
predict next token
```

但 next-token prediction 学到的不只是语言表面。
在足够大数据和模型规模下，它会逼迫模型学习：

```text
syntax
semantics
world knowledge
reasoning patterns
code structure
tool-use patterns
domain knowledge
```

这就是 Transformer 架构和规模化训练结合后的威力。

## Transformer 的工程维度

如果我们要从工程角度理解 Transformer，必须看这些维度：

```text
context length
hidden size
number of layers
number of heads
vocabulary size
activation function
normalization
position encoding
attention implementation
precision
KV cache
batching
throughput
latency
memory footprint
```

面试和项目里，不能只说“用了 Transformer”。
要能解释：

```text
为什么这个模型大小合适？
上下文长度够不够？
推理成本多大？
attention 是否是瓶颈？
能不能用 KV cache？
训练和推理显存如何估计？
```

这就是从算法走向系统。

## KV Cache

LLM 生成时，每次生成一个新 token。
如果每一步都重新计算所有历史 token 的 K/V，会很浪费。

KV cache 的思路：

```text
把历史 token 的 key 和 value 缓存起来。
新 token 只需要计算自己的 Q/K/V，并和历史 K/V 做 attention。
```

作用：

```text
降低自回归生成的重复计算
提升推理速度
增加显存占用
```

这也是为什么长上下文推理成本高。
上下文越长，KV cache 越大。

## 对 RAG 和 Agent 的意义

RAG 本质上是在给 Transformer 提供更好的上下文：

```text
query
  -> retrieve relevant documents
  -> put documents into context
  -> LLM attends over context
  -> generate answer
```

Graph RAG 也是类似：

```text
graph structure
  -> relevant entities / relations / communities
  -> textual or structured context
  -> LLM reasoning
```

Agent 也是：

```text
task history
tool observations
memory
current instruction
  -> context
  -> Transformer predicts next action / message / tool call
```

所以理解 Transformer，就是理解 LLM 如何消费上下文。

## 对 Quant 的意义

量化里 Transformer 可以用于：

```text
financial text modeling
news / filing understanding
time-series representation
cross-asset sequence modeling
event extraction
market regime modeling
multi-modal signal fusion
```

但需要谨慎：

```text
financial data is noisy
signals are weak
time leakage is dangerous
distribution shift is severe
backtest overfitting is common
```

Transformer 能处理复杂上下文，不代表天然能赚钱。
量化里必须接上：

```text
data cleaning
time-aware split
cost-aware backtest
risk constraints
out-of-sample validation
factor diagnosis
```

这就是模型能力和 quant harness 的区别。

## 面试可用表达

如果被问“Transformer 架构是什么”，可以这样说：

```text
A Transformer maps token ids into embeddings, adds positional information,
and processes the sequence through repeated blocks of multi-head self-attention
and feed-forward networks with residual connections and layer normalization.

Self-attention computes query, key, and value projections for each token.
The query-key similarity determines attention weights, and the weighted sum of values
produces context-aware token representations.

Encoder-only models are usually used for understanding and representation tasks.
Decoder-only models use causal masking and next-token prediction, which is the standard setup for modern LLMs.
Encoder-decoder models are useful for sequence-to-sequence transformation.
```

再接一句工程化表达：

```text
For agent and RAG systems, the key is that Transformer models reason over whatever context we provide.
So retrieval quality, context construction, memory management, and evaluation harness directly affect model behavior.
```

## 当前结论

`MLRL002` 的核心结论：

```text
Transformer = token embedding + positional information + repeated attention/FFN blocks + output logits。
```

Attention 负责跨 token 融合信息。
FFN 负责每个位置的非线性变换。
Residual 和 LayerNorm 负责训练稳定。
Causal mask 让 decoder-only LLM 能做 next-token prediction。

理解 Transformer，才真正能理解 LLM、RAG、Agent、AI Harness 的底层结构。
