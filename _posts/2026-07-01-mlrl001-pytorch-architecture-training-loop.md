---
title: "MLRL001: PyTorch 架构与训练循环"
date: 2026-07-01 00:00:00 +0800
categories: [Learning, AI Foundations]
tags: [pengyi-mlrl-map, mlrl001, pytorch, deep-learning, training-loop, autograd, nn-module, ai-engineering, research-os]
---

这是 `PENGYI_ML_RL_MAP` 的第二篇：

```text
MLRL001 -> PyTorch 架构与训练循环
```

`MLRL000` 先搭了总地图。
这一篇开始进入实现层。

我现在对 PyTorch 的判断是：

```text
PyTorch 不是一个“会调包”的工具。
PyTorch 是深度学习系统的工程底座：
tensor + autograd + nn.Module + data pipeline + training loop + evaluation + checkpoint。
```

如果我们要做 AI Scientist、coding agent harness、quant model、RLHF、embedding model、reward model，最后都会落到同一个问题：

```text
如何把一个学习目标稳定地训练、评估、保存、复现出来。
```

PyTorch 就是回答这个问题的核心工具。

## 一句话定义

```text
PyTorch = an imperative tensor computation framework with automatic differentiation and modular neural network abstractions.
```

中文说就是：

```text
PyTorch = 张量计算 + 自动求导 + 神经网络模块化 + 训练循环工程化。
```

它最重要的不是 API 数量，而是这条训练链：

```text
data
  -> batch
  -> model forward
  -> prediction
  -> loss
  -> backward
  -> optimizer update
  -> evaluation
  -> checkpoint
```

这个链路打通之后，我们才真正开始拥有深度学习工程能力。

## PyTorch 的核心分层

可以把 PyTorch 拆成七层：

```text
Layer 1: Tensor
    数据和计算的基本载体。

Layer 2: Autograd
    自动记录计算图，并根据 loss 计算梯度。

Layer 3: nn.Module
    用标准对象组织模型结构和参数。

Layer 4: Dataset / DataLoader
    把原始数据变成可训练 batch。

Layer 5: Loss / Optimizer / Scheduler
    定义目标、更新参数、控制学习率。

Layer 6: Train / Eval Loop
    把训练、验证、日志、保存串起来。

Layer 7: Experiment Harness
    管理配置、随机种子、checkpoint、指标、复现和报告。
```

越往上，越接近我们的 `Research OS`。
越往下，越接近模型训练的计算底座。

## Tensor: 数据、形状、设备

`torch.Tensor` 是 PyTorch 的基本对象。
理解 Tensor，至少要盯住四件事：

```text
shape
    张量维度。

dtype
    数据类型，例如 float32、float16、int64。

device
    计算位置，例如 cpu、cuda。

requires_grad
    是否需要 autograd 跟踪梯度。
```

很多 PyTorch bug，本质上都不是算法 bug，而是这四件事没对齐：

```text
shape mismatch
dtype mismatch
device mismatch
gradient tracking mismatch
```

例如：

```python
import torch

x = torch.randn(32, 128)              # batch_size = 32, feature_dim = 128
w = torch.randn(128, 10, requires_grad=True)

logits = x @ w                        # [32, 10]
loss = logits.mean()
loss.backward()

print(w.grad.shape)                   # [128, 10]
```

这段代码已经包含了 PyTorch 的核心：

```text
tensor computation
loss scalar
backward
gradient stored on parameter
```

## Autograd: 自动求导

`autograd` 是 PyTorch 的灵魂。

只要一个 Tensor 需要梯度，PyTorch 就会在 forward 过程中记录计算图。
当我们调用：

```python
loss.backward()
```

PyTorch 会从 loss 反向沿着计算图，把每个参数的梯度算出来。

可以这样理解：

```text
forward builds the computation graph.
backward traverses the graph to compute gradients.
optimizer.step uses gradients to update parameters.
```

最常见的梯度相关操作：

```python
optimizer.zero_grad()
loss.backward()
optimizer.step()
```

这三行不能机械背。
它们对应的是：

```text
clear old gradients
compute new gradients
update parameters
```

### 为什么要 zero_grad

PyTorch 默认会累积梯度。
如果不清空梯度，多个 batch 的梯度会叠加。

这是一个常见坑：

```python
for x, y in dataloader:
    y_hat = model(x)
    loss = criterion(y_hat, y)
    loss.backward()
    optimizer.step()
```

这段少了：

```python
optimizer.zero_grad()
```

结果就是梯度不断累积，训练行为会变得不受控。

更推荐的写法：

```python
optimizer.zero_grad(set_to_none=True)
```

它可以减少一些内存写入，也能更清晰地表达“旧梯度无效”。

## nn.Module: 模型组织方式

`nn.Module` 是 PyTorch 组织模型的标准方式。

一个最小模型：

```python
import torch
from torch import nn


class MLP(nn.Module):
    def __init__(self, input_dim: int, hidden_dim: int, output_dim: int):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(input_dim, hidden_dim),
            nn.ReLU(),
            nn.Linear(hidden_dim, output_dim),
        )

    def forward(self, x):
        return self.net(x)
```

这里的关键不是 MLP 本身，而是结构：

```text
__init__
    定义模型组件和参数。

forward
    定义输入到输出的计算过程。

parameters()
    自动收集需要训练的参数。

state_dict()
    保存模型权重。
```

`nn.Module` 的重要能力：

```text
1. 自动注册子模块
2. 自动管理参数
3. 支持 model.to(device)
4. 支持 model.train() / model.eval()
5. 支持 state_dict 保存和加载
```

如果一个人能讲清楚 `nn.Module`，他就不是只会写脚本，而是在理解深度学习工程对象模型。

## Dataset 和 DataLoader

模型训练之前，数据必须被组织成 batch。

PyTorch 的标准抽象是：

```text
Dataset
    定义一个样本怎么取。

DataLoader
    定义如何 batch、shuffle、并行加载、collate。
```

最小 Dataset：

```python
from torch.utils.data import Dataset


class FactorDataset(Dataset):
    def __init__(self, features, labels):
        self.features = features
        self.labels = labels

    def __len__(self):
        return len(self.features)

    def __getitem__(self, idx):
        return self.features[idx], self.labels[idx]
```

DataLoader：

```python
from torch.utils.data import DataLoader

loader = DataLoader(
    dataset,
    batch_size=64,
    shuffle=True,
    num_workers=0,
)
```

这里最容易出问题的是：

```text
shuffle 是否应该开
train / valid / test 是否泄露
batch 内样本是否需要 padding
time series 是否破坏时间顺序
label 是否和 feature 对齐
collate_fn 是否正确
```

在量化场景里，尤其要小心：

```text
不能让未来信息进入训练 batch。
不能随机切分时间序列造成 leakage。
不能把同一交易日、同一资产的上下文打乱到不可解释。
```

PyTorch 技术问题，经常会变成研究设计问题。

## Loss、Optimizer、Scheduler

训练需要一个目标。
这个目标通常由 loss function 表达：

```text
classification -> cross entropy
regression -> MSE / MAE / Huber
ranking -> pairwise loss / listwise loss
contrastive learning -> InfoNCE-style loss
policy optimization -> policy gradient objective
```

Optimizer 负责用梯度更新参数：

```text
SGD
    经典随机梯度下降。

Adam
    自适应一阶、二阶矩估计。

AdamW
    常用于 Transformer 训练，把 weight decay 处理得更合理。
```

Scheduler 负责控制学习率变化：

```text
warmup
cosine decay
step decay
linear decay
reduce on plateau
```

面试里如果被问“训练为什么不稳定”，不能只说调参。
要能定位到：

```text
learning rate
batch size
initialization
normalization
optimizer
weight decay
gradient clipping
data distribution
label noise
train/eval mismatch
```

这就是训练诊断能力。

## 标准训练循环

一个更完整的训练循环：

```python
import torch


def train_one_epoch(model, dataloader, criterion, optimizer, device):
    model.train()
    total_loss = 0.0

    for x, y in dataloader:
        x = x.to(device)
        y = y.to(device)

        optimizer.zero_grad(set_to_none=True)

        y_hat = model(x)
        loss = criterion(y_hat, y)

        loss.backward()
        torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)
        optimizer.step()

        total_loss += loss.item() * x.size(0)

    return total_loss / len(dataloader.dataset)
```

这段代码里有几个工程点：

```text
model.train()
    打开训练模式，影响 dropout / batchnorm。

x.to(device)
    把数据移动到 CPU/GPU 一致的设备。

zero_grad
    清空旧梯度。

loss.backward()
    计算梯度。

clip_grad_norm_
    防止梯度爆炸，尤其在 RNN / Transformer / RL 中常见。

loss.item()
    从 tensor 取 Python 数值用于日志。
```

## 标准验证循环

验证循环不能更新参数。
标准写法：

```python
def evaluate(model, dataloader, criterion, device):
    model.eval()
    total_loss = 0.0

    with torch.no_grad():
        for x, y in dataloader:
            x = x.to(device)
            y = y.to(device)

            y_hat = model(x)
            loss = criterion(y_hat, y)

            total_loss += loss.item() * x.size(0)

    return total_loss / len(dataloader.dataset)
```

关键点：

```text
model.eval()
    进入评估模式。

torch.no_grad()
    不记录计算图，节省显存和计算。

no optimizer.step()
    验证不能更新参数。
```

很多人训练模型时会忘记 `model.eval()`。
这会导致 dropout 和 batchnorm 行为错误，评估指标不稳定。

## Checkpoint 和复现

一个严肃的训练系统一定要能保存和恢复。

常见 checkpoint：

```python
torch.save(
    {
        "epoch": epoch,
        "model_state_dict": model.state_dict(),
        "optimizer_state_dict": optimizer.state_dict(),
        "best_valid_loss": best_valid_loss,
    },
    "checkpoint.pt",
)
```

加载：

```python
checkpoint = torch.load("checkpoint.pt", map_location=device)
model.load_state_dict(checkpoint["model_state_dict"])
optimizer.load_state_dict(checkpoint["optimizer_state_dict"])
```

复现还需要：

```text
random seed
data split version
feature generation version
model config
optimizer config
training logs
environment information
git commit hash
```

这就是为什么我们一直讲 `Research OS`。
没有这些信息，训练结果只是一次偶然运行。

## Mixed Precision 和显存意识

大模型训练经常用 mixed precision。
典型是 AMP：

```python
from torch.cuda.amp import autocast, GradScaler

scaler = GradScaler()

for x, y in dataloader:
    x = x.to(device)
    y = y.to(device)

    optimizer.zero_grad(set_to_none=True)

    with autocast():
        y_hat = model(x)
        loss = criterion(y_hat, y)

    scaler.scale(loss).backward()
    scaler.step(optimizer)
    scaler.update()
```

它的目标是：

```text
更少显存
更快计算
尽量保持数值稳定
```

这类能力在大模型、Transformer、长序列模型里很重要。

## 常见坑位

PyTorch 常见坑可以做成一张排查表：

```text
Shape mismatch
    检查每一层输入输出维度。

Device mismatch
    model 和 data 是否都在同一个 device。

Gradient accumulation bug
    是否忘记 zero_grad。

Train/eval mismatch
    是否正确调用 model.train() 和 model.eval()。

Data leakage
    train / valid / test 是否穿越。

Exploding gradients
    是否需要 gradient clipping。

Vanishing signal
    loss 是否没有下降，feature 是否无效，初始化是否不合理。

Wrong target dtype
    cross entropy 通常需要 class index，而不是 one-hot float。

No torch.no_grad in eval
    验证时无意义地占用显存。

Broken checkpoint
    是否只保存了 model，没保存 optimizer 和 config。
```

真正工程化的 PyTorch 能力，就是能从这些坑里快速定位原因。

## 对 Quant Research 的意义

量化里用 PyTorch，不是为了炫技。
它可以做：

```text
cross-sectional return prediction
time-series forecasting
factor representation learning
text signal modeling
news / filing embedding
portfolio risk model
strategy selection model
execution policy model
```

但量化场景有自己的约束：

```text
time leakage
survivorship bias
look-ahead bias
non-stationarity
transaction cost
turnover
capacity
regime shift
```

所以 PyTorch 模型训练必须接上 quant harness：

```text
data version
feature version
train window
validation window
test window
backtest
risk report
diagnosis
```

否则就只是一个深度学习 demo。

## 对 Agent Harness 的意义

如果我们做 coding agent harness 或 research agent harness，PyTorch 也会出现在后面：

```text
fine-tune small models
train reward models
train evaluator models
train embedding models
train rerankers
experiment with policy optimization
```

一个 agent 产品看起来是 prompt、tool、memory。
但如果要走到训练层，它仍然需要：

```text
data
model
loss / reward
optimizer
evaluation
checkpoint
deployment
```

这就是 PyTorch 的长期价值。

## 面试可用表达

如果被问“你怎么理解 PyTorch”，可以这样说：

```text
I view PyTorch as the engineering substrate for deep learning.
At the bottom, tensors represent data and computation.
Autograd records the dynamic computation graph and computes gradients from a scalar loss.
nn.Module provides the object model for organizing parameters and forward computation.
Dataset and DataLoader turn raw data into reproducible mini-batches.
The training loop connects forward pass, loss, backward pass, optimizer update, evaluation, and checkpointing.

For research engineering, the important part is not only writing the loop,
but also controlling leakage, train/eval modes, metrics, random seeds, checkpoints, and experiment reproducibility.
```

这段话可以直接进入 AI / Quant / Research Engineer 面试。

## 当前结论

`MLRL001` 的核心结论：

```text
PyTorch = tensor + autograd + nn.Module + data pipeline + training loop + evaluation + checkpoint。
```

我们后面看 Transformer、RL、RLHF，本质上都会回到这套工程结构。

所以 PyTorch 不是孤立的一门技术。
它是我们把 research idea 变成可训练、可复现、可解释系统的底座。
