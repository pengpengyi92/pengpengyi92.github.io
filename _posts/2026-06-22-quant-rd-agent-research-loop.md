---
title: "Quant R&D Agent: 从因子假设到下一轮研究计划"
date: 2026-06-22 12:45:00 +0800
categories: [Quant Research, AI Agents]
tags: [quant, rd-agent, llm-agent, backtesting, factor-research]
---

我现在最核心的研究工程方向是 Quant R&D Agent。

它不是一个简单的聊天机器人，而是一个面向量化研究流程的 agentic loop：

```text
自动提出因子假设
  -> 自动实现
  -> 自动回测
  -> 自动诊断偏差
  -> 自动生成下一轮研究计划
  -> 人类 PM 审核
```

这个流程里最关键的不是“自动化一切”，而是把研究动作结构化。

## 为什么是 R&D

量化研究天然分成两个互相依赖的部分：

- Research: 提出假设、解释经济含义、判断实验结果是否可信。
- Development: 实现数据处理、回测、评估、复现实验。

如果只做 Research，想法无法落地。

如果只做 Development，就容易变成机械实现。

R&D Agent 的目标是让二者进入一个闭环：

```text
hypothesis -> implementation -> evaluation -> diagnosis -> next hypothesis
```

## 人类 PM 为什么重要

金融研究不能把最终判断交给自动 agent。原因很直接：

- 数据可能有 look-ahead bias。
- 回测可能有 survivorship bias。
- 因子可能只是过拟合。
- 实验指标可能无法解释真实交易约束。
- 研究系统可能产生看似合理但实际不稳的结论。

所以我更认可这个结构：

```text
Agent 负责扩大搜索、执行和记录。
Human PM 负责方向、风险、解释和取舍。
```

这也是我想把 Quant R&D Agent 做成研究系统，而不是“自动炒股工具”的原因。

## Public-safe 训练场

WorldQuant 因子库和公开 alpha 资料可以成为训练场，但公开发布时必须脱敏：

- 不发布私有账号导出的完整因子库。
- 不发布违反平台条款的数据。
- 只保留少量公开范例或自造 synthetic examples。
- 把重点放在 workflow、schema、diagnosis 和 report generation。

这样既能训练系统，也能保护合规边界。

## 可以形成的论文方向

这个项目有潜力走几类技术报告或论文方向：

- Agentic workflow for quantitative research
- Research memory and experiment lineage for factor discovery
- Bias diagnosis in LLM-assisted backtesting
- Human-in-the-loop PM review for financial research agents
- Benchmarking LLM agents on public-safe alpha discovery tasks

现在第一阶段目标不是直接冲 full paper，而是先做出一个可信 public demo：

```text
repo + examples + tests + benchmark note + technical report
```

有了这个，RA / PhD / research engineer 的叙事会更扎实。
