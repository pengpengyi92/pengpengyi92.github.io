---
title: "PWEB001: Full-Stack Engineering as a Public Evidence System"
date: 2026-08-14 20:00:00 +0800
categories: [Learning, Full Stack, Systems]
tags: [pweb, full-stack, typescript, react, cloudflare-workers, hono, d1, system-design, testing, deployment, public-safe]
---

`PWEB` 是一个正在运行的全栈工程学习系统。它的目标不是做一张“我会 Full Stack”的能力海报，而是让架构、代码、测试、部署和后续学习都留下可检查的证据。

- **Live:** [pengyi-pweb.pengpengyi92.workers.dev](https://pengyi-pweb.pengpengyi92.workers.dev/)
- **Repository:** [pengpengyi92/PWEB](https://github.com/pengpengyi92/PWEB)
- **Learning surface:** [PWEB Learn](https://pengyi-pweb.pengpengyi92.workers.dev/learn)

## Why PWEB

“Full Stack” 容易被理解为前端、后端、数据库各写一点。更有用的定义是：能够把一个用户需求变成一个有边界、可验证、可部署、可维护的系统。

```text
Need
  -> Domain model
  -> UI and interaction
  -> typed API contract
  -> service and persistence
  -> tests and observability
  -> deployment
  -> evidence
  -> next constraint
```

因此，PWEB 同时是产品和学习图谱。当前公开页面把 Web 能力拆成 47 个节点，并按九层组织：

```text
1. Interface     2. Frontend       3. Boundary
4. Backend       5. Data           6. Intelligence
7. Systems       8. Production     9. Feedback
```

每层都有明确的负责人视角，而不是把所有内容堆成一个技术名词列表。图谱的作用是回答三个问题：我在学什么？这项能力依赖什么？下一步应该用什么真实交付物来证明？

## The Current System

PWEB 当前采用一套克制的 Cloudflare-first 架构：

```text
Browser
  -> React + TypeScript application
  -> typed HTTP contract
  -> Hono API on Cloudflare Workers
  -> D1 / SQLite persistence boundary
  -> repository, tests, CI and deployment evidence
```

这条链路比“前端页面加几个 API”多了几个重要约束：

| System question | PWEB answer |
| --- | --- |
| 数据是什么？ | 能力节点、层级、状态、任务、证据和关联。 |
| 合法状态是什么？ | 通过 TypeScript 类型和显式状态模型约束，而不是依赖随意字符串。 |
| 谁拥有状态？ | UI 负责交互态；Worker 负责服务边界；D1 负责持久化状态。 |
| 组件如何协作？ | 以前后端共享/对齐的 typed contract 为接口，而不是隐式字段约定。 |
| 如何证明交付？ | typecheck、lint、unit/integration、Playwright E2E、可访问性检查、CI 与线上部署。 |

这不是对所有 Web 产品都唯一正确的架构。它是一个适合当前规模的起点：前端与 Worker 可以独立演进，数据层有明确边界，公开页面不需要携带任何私有资料。

## Full Stack Is a System Design Exercise

语言和框架只是表达工具。无论使用 TypeScript、Python、OCaml、C++ 或 Rust，一个完整系统都绕不开相同问题：

```text
Data -> Type -> State -> Invariant -> Transition
     -> Function -> Interface -> Boundary -> Ownership
     -> Failure -> Effect -> Concurrency -> Persistence -> Architecture
```

PWEB 是把这套抽象落到 Web 场景的练习。

例如一个“学习节点”不是仅有标题的卡片。它需要定义数据字段、状态变化、展示接口、服务端读写边界和验证方式；当它进入公共页面后，还需要考虑移动端阅读、键盘操作、加载体验、错误处理和隐私边界。

这也解释了为什么全栈工程不等于“会 React + 会 Node.js”。真正的工作是持续处理 contract、failure mode、ownership 和 deployment constraint。

## Evidence, Not Decoration

PWEB 的能力图谱将节点区分为已有交付、正在构建与待研究，而不是把所有名词都标成已掌握。当前公开版本已有：

- 可部署的 React + TypeScript 前端与 Cloudflare Worker 服务；
- API、持久化和跨页面状态的工程边界；
- 单元、集成、端到端和基础可访问性验证；
- GitHub CI 与 Cloudflare 的持续部署路径；
- 47-node 学习图谱、九层筛选和每层的学习行动队列。

任何未测量的性能、可靠性或成本结论都应继续标记为 `UNMEASURED`。这是比展示更重要的原则：工程证据必须能说明范围，不能用部署页面替代生产承诺。

## What Changed Through Review

这个项目的 UI 也经历了工程性复查。桌面图谱适合观察关系，但不能代替可访问的文本导航；因此页面同时提供层级、节点和学习行动的语义化入口。图谱的滚轮缩放被移除，以避免浏览页面时误触缩放；Agent 页面使用完整图片展示，同时保持内容与交互本身可独立访问。

这类细节看起来小，却反映了产品工程的基本训练：不是只问“能不能做出来”，还要问“用户是否能稳定地使用、理解和返回”。

## Learning Loop

PWEB 的学习方法不是按框架目录背诵，而是按真实约束推进：

```text
Learn one concept
  -> add one bounded feature
  -> write or extend a test
  -> deploy it
  -> record evidence and limitations
  -> choose the next bottleneck
```

接下来优先研究的方向是：

1. D1 数据访问与查询性能的可测量基线；
2. 键盘导航、reduced-motion 和更完整的可访问性覆盖；
3. 结构化日志、SLO/SLI 与故障诊断；
4. 面向公开元数据的跨仓库 federation adapter；
5. 在出现可测量瓶颈后，再比较 Rust/Worker 或其他运行时的收益。

最后一条尤其重要：技术升级应该由约束和测量推动，而不是由语言品牌推动。

## Takeaway

PWEB 的价值不在于把 Web 技术清单扩得多长，而在于建立一条可重复的工程闭环：

```text
Model the system
  -> build a bounded implementation
  -> verify it
  -> deploy it
  -> expose the evidence
  -> learn from the next constraint
```

当一个项目能公开呈现它的架构、边界、测试与下一步，它本身就是 Full-Stack 能力最有说服力的证据。
