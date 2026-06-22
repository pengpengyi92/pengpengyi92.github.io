---
title: "LLMQuant 学习地图: 从项目阅读到个人 Research OS"
date: 2026-06-22 14:50:00 +0800
categories: [Learning, Quant Research]
tags: [llmquant, project-map, research-os, open-source, study-log]
---

LLMQuant 给我的启发不是某一个 repo，而是一个生态视角：

```text
data access
  -> skills / workflows
  -> knowledge base
  -> agent system
  -> docs / wiki / book
  -> strategy and research output
```

这说明一个真正有生命力的 AI + finance 项目，通常不是单点模型，而是一套持续迭代的系统。

## 我看到的子系统

从学习角度看，可以把这类项目拆成几个层次：

| Layer | Meaning |
|---|---|
| Data Layer | 数据获取、清洗、schema、MCP 接口 |
| Knowledge Layer | wiki、book、research notes、domain concepts |
| Skill Layer | 可复用任务流程和 agent skills |
| Agent Layer | 多 agent 协作、研究规划、工具调用 |
| Experiment Layer | backtest、benchmark、artifact、report |
| Application Layer | 策略研究、项目 demo、论文和开源输出 |

我的个人 Research OS 也应该按这个方向长出来。

## 对自己的启发

我不应该只做一个 isolated repo。

更好的结构是：

```text
Pengyi Quant Research OS
  -> project specs
  -> factor specs
  -> experiments
  -> artifacts
  -> reports
  -> technical notes
  -> public website index
```

也就是说，代码、实验、文章、申请材料不是分散的，它们应该相互引用。

## 学习路线

接下来学习一个开源项目时，我会按这个模板记录：

```text
1. 这个项目解决什么问题？
2. 它的系统边界在哪里？
3. 它的核心数据结构是什么？
4. 它的 agent/workflow 如何组织？
5. 它有哪些可以复用到 Quant R&D Agent？
6. 它有哪些不适合直接复用？
7. 我能做出什么 public-safe demo？
```

这会把“看项目”变成研究资产，而不是临时浏览。

## 近期目标

我希望把 LLMQuant、RD-Agent、LightRAG、WorldQuant 复现项目等学习成果统一到同一个公开叙事里：

```text
AI research agents for quantitative discovery.
```

这条主线足够清晰：

- 有真实金融场景。
- 有 AI agent 技术深度。
- 有工程系统空间。
- 有 RA / PhD / paper / open-source 的连接点。

后续每看一个项目，都应该能回答：

```text
它怎样增强我的 Quant R&D Agent 和 Research OS？
```
