---
title: "HKUDS053: OpenOPC - AI-Native Company / Self-Built Self-Run Self-Grown"
date: 2026-07-13 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds053, hkuds, openopc, ai-native-company, agent-organization, company-mode, office-ui, self-built, self-run, self-grown, research-os, founder-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS053`。

这一次看的是 HKUDS 最新非常值得关注的项目：

```text
HKUDS053 -> OpenOPC
```

我对它的一句话定位是：

```text
OpenOPC = AI-native company OS.
```

不是单个 chatbot。
不是单个 coding agent。
不是单个 workflow runner。

它更像是把多个 AI employee、role、org chart、work item、approval、review、memory、office UI、CLI、channel、skill、tool runtime 组织起来，形成一个“可以自建、自运行、自成长”的 AI company prototype。

这和我们最近一直在讲的 A / B / C 面非常相关：

```text
B 面：coding / research / system design
A 面：真实业务 / 金融组织 / 客户 / 风控 / 交付
C 面：founder / product / team / market / capital
```

`OpenOPC` 对我们最重要的启发不是“又一个 agent 项目”，而是：

```text
真正的 founder / delivery system 不是一个人无限写代码。
它需要组织结构、角色分工、任务流转、复盘学习、审批边界和可视化管理。
```

## Latest Snapshot

本次观察时间：`2026-07-14`。

| Item | Value |
|---|---|
| repo | `HKUDS/OpenOPC` |
| URL | <https://github.com/HKUDS/OpenOPC> |
| description | `OpenOPC: Build Your Personal AI-Native Company — Self-Built, Self-Run, Self-Grown` |
| default branch | `main` |
| language | `Python` |
| stars | `723` |
| forks | `110` |
| open issues | `6` |
| pushed at | `2026-07-13T15:05:26Z` |
| updated at | `2026-07-13T16:31:02Z` |
| root structure | `.opc`, `config`, `docs`, `opc`, `scripts`, `skills`, `tests`, `pyproject.toml`, `README.md`, `README.zh-CN.md` |

最近十个 commit 很说明问题：

| Date | Commit | Message |
|---|---|---|
| 2026-07-13 | `4bc18dc` | `fix(ui): stabilize workplace chat scrolling` |
| 2026-07-13 | `4e7aa75` | `fix(company): stop failed children from wedging the tree and soften the dispatch guard` |
| 2026-07-09 | `a040252` | `fix(office-ui): stop progress-row flicker and surface native role replies end to end` |
| 2026-07-09 | `35fa717` | `config: give all native org roles the full toolset via empty tools lists` |
| 2026-07-09 | `e3bed49` | `perf(office-ui): stop per-token full-tree renders, sleep hidden Phaser loop, cut broadcast churn` |
| 2026-07-08 | `5aa57e6` | `fix: release comms-blocking parks when replies arrive and give every role final-reply tools` |
| 2026-07-08 | `4b29b89` | `refactor: unify tool approval into a single engine and cut prompt storms` |
| 2026-07-08 | `447516d` | `fix: converge approval park/resume through the work-item state machine` |
| 2026-07-07 | `818189a` | `fix: guarantee engine replies reach the UI channel and stop same-scope duplicate sends` |
| 2026-07-07 | `3a2c935` | `fix: deduplicate re-delivered session sends on the client message id` |

这说明 OpenOPC 已经不是停留在 README 概念层。

它最近的更新集中在：

```text
UI stability
company runtime failure recovery
office UI rendering performance
role reply routing
tool approval engine
work-item state machine
communication blocking / resume
duplicate message handling
```

也就是说，它在进入真实 product hardening 阶段。

## Core Thesis

OpenOPC README 里给了三个核心词：

```text
Self-Built
Self-Run
Self-Grown
```

我理解如下。

### Self-Built

`Self-Built` 解决的是：

```text
一个复杂目标来了，应该由哪些角色完成？
```

它不是默认只有一个 agent，而是先搭组织：

```text
goal
  -> org chart
  -> roles
  -> reporting lines
  -> employee assignment
  -> talent reuse or fresh hire
```

这非常像真实公司。

一个 FICC quant demo 不是只有“写代码”：

```text
PM / product owner
quant researcher
data engineer
backend engineer
frontend engineer
risk reviewer
documentation lead
deployment owner
```

如果每个角色都由 agent 或 human-agent hybrid 承担，那么项目就从“个人写脚本”变成“组织化 delivery”。

### Self-Run

`Self-Run` 解决的是：

```text
组织搭好之后，任务如何流动？
```

OpenOPC 的关键不是简单 task list，而是 work-item state machine：

```text
brief
  -> decomposition
  -> work items
  -> dependency DAG
  -> assign owner
  -> execute / delegate / review / integrate / rework
  -> blocker handling
  -> human escalation
  -> final deliverable
```

这里最重要的是：

```text
真实任务无法完全 upfront planning。
```

所以它需要：

```text
dynamic collaboration orchestration
kanban status
role ownership
runnability check
review gate
blocker handling
human approval
```

这和我们自己的 Transition OS、CV OS、FICC Project OS 完全能接上。

### Self-Grown

`Self-Grown` 解决的是：

```text
做完一轮之后，系统怎么变聪明？
```

它不是把所有成败都归给“整个公司”，而是把反馈归因到具体 role / employee / work item。

```text
execution trace
  -> per-role evaluation
  -> employee experience profile
  -> recurring lesson
  -> shared playbook
  -> next run better
```

这就是我们一直说的：

```text
self-evolution loop
```

我们自己的项目也应该这样做。

比如：

```text
FICC rates project v0.7 Cloudflare deploy failed once
  -> root cause: Worker vs Pages config mismatch
  -> lesson: static frontend should use Pages, not Worker wrangler deploy
  -> playbook: Cloudflare Pages setting checklist
  -> next project: directly reuse deploy checklist
```

这才是真正的 OS 化。

## System Design View

OpenOPC 的系统分层可以这样看：

```text
User Goal / Brief
  -> Company Builder
  -> Org Chart / Roles / Reporting Lines
  -> Talent / Employee Assignment
  -> Work Item Planner
  -> Work Item State Machine
  -> Role Runtime
  -> Native / External Agent Execution
  -> Tool Approval / Permissions
  -> Comms / Blockers / Human Escalation
  -> Office UI / Kanban / Workspace
  -> Memory / Experience / Playbooks
  -> Deliverable
```

它的 root structure 也对应这些方向：

| Directory / File | Meaning |
|---|---|
| `.opc` | local OPC workspace / runtime state / config convention |
| `config` | system, agent, company, channel configuration |
| `docs` | visual assets, guides, design docs |
| `opc` | core Python package |
| `scripts` | maintenance / reset / utility scripts |
| `skills` | reusable skill library |
| `tests` | test suite |
| `pyproject.toml` | Python package and dependency definition |
| `README.md` / `README.zh-CN.md` | bilingual project narrative |

这就是一个标准的 AI application / agent runtime repo 结构：

```text
package code
config
docs
skills
tests
scripts
UI assets
quickstart
```

它不是 toy README。

## Office UI

OpenOPC 的 Office UI 很关键。

它不是只给你一个 chat box，而是给你：

```text
Workspace
Office
Org
Kanban
Execution Progress
Agents tab
Comms tab
Team cockpit
role inspector
talent market
organization editor
```

这说明它把 agent work 从“文本对话”升级成了“组织管理界面”。

对于复杂工作，chat box 是不够的。

我们做 FICC project、CV project、paper-to-code project、research proposal project，都需要：

```text
谁负责什么？
哪些任务卡住了？
哪些需要 review？
哪些已经完成？
哪些要 human approval？
最终 artifact 在哪里？
复盘 lesson 写进哪里？
```

这就是 Office UI 的价值。

## OpenOPC vs AgentSpace vs OpenHarness

之前我们已经看过：

```text
HKUDS006 -> AgentSpace
HKUDS015 -> OpenHarness
```

现在看 OpenOPC，可以这样区分：

| Project | Layer |
|---|---|
| OpenHarness | agent harness runtime: model + tools + skills + permissions + memory |
| AgentSpace | human + agents workspace: team collaboration surface |
| OpenOPC | AI-native company: org chart + employees + work items + company mode + self-growth |

我的理解：

```text
OpenHarness 更偏 runtime。
AgentSpace 更偏 workspace。
OpenOPC 更偏 organization / company operating system。
```

三者不是互相替代，而是越来越上层：

```text
runtime
  -> workspace
  -> company
```

这条线和 HKUDS 近期生态非常一致。

## OpenOPC in the HKUDS Ecosystem

现在 HKUDS 的项目已经可以拼成一张更大的图：

```text
LightRAG       -> knowledge memory / graph RAG
RAG-Anything   -> multimodal document RAG
nanobot        -> personal agent shell
CLI-Anything   -> software action / CLI tool layer
OpenHarness    -> agent runtime / harness
AgentSpace     -> human-agent workspace
Vibe-Trading   -> finance / trading research workflow
AI-Trader      -> agent-native trading platform
OpenOPC        -> AI-native company / organization OS
```

OpenOPC 的位置是最高一层：

```text
它不是某个 agent 会不会调工具。
它关心的是一个 AI organization 如何自己组队、分工、执行、复盘、成长。
```

这对我们尤其重要。

因为我们未来不只是要做一个 demo，而是要把多个方向组织起来：

```text
CV delivery
FICC project
AI agent harness
quant research workflow
learning log
interview grilling
public website
GitHub profile
Cloudflare deployment
```

这些本质上都可以被看成：

```text
Pengyi Personal AI-Native Company v0
```

## Connection to Our A / B / C OS

我们之前一直在拆：

```text
A 面：金融业务 / 客户 / 组织 / 资源 / 风控
B 面：coding / research / AI / quant / system design
C 面：founder / product / team / market / capital
```

OpenOPC 给了一个很直接的工程化表达：

```text
C 面 founder 不是抽象愿景。
C 面 founder 是把 A 面真实业务和 B 面技术生产力组织成一个可运行公司。
```

对我们来说：

```text
A 面提供真实场景：
  bank workflow, credit process, industry chain, FICC, client needs

B 面提供技术能力：
  Python, agent, RAG, quant, frontend, backend, deployment

C 面需要组织化：
  role design, task decomposition, review, human approval, memory, self-growth
```

OpenOPC 的系统设计正好在讲这个。

## How We Can Use This

我们可以把 OpenOPC 的思想迁移到自己的 Research OS：

### 1. Project as Company

每个项目都可以有“虚拟公司结构”。

比如 `FICC Rates Bond Quant`：

```text
Founder / Product Owner
  -> Quant Researcher
  -> Backend Engineer
  -> Frontend Engineer
  -> Documentation Lead
  -> Deployment Engineer
  -> Risk / Public-safe Reviewer
```

每个角色负责不同 artifact：

```text
quant engine
API design
dashboard UI
README
tests
version log
Cloudflare deploy
CV bullet
interview story
```

### 2. CV as Company

CV 也可以被拆成角色：

```text
Positioning Lead
Evidence Reviewer
Quant Story Owner
AI Project Owner
Banking Experience Owner
English Version Owner
LaTeX Production Owner
Interview Grilling Owner
```

这样 CV 不是“写一页纸”，而是一个 delivery workflow。

### 3. Interview as Company

面试准备也能组织化：

```text
WorldQuant interviewer
FICC quant interviewer
AI agent interviewer
banking interviewer
behavioral interviewer
resume truth reviewer
```

每个角色都拷打不同风险点。

### 4. Learning as Company

每篇 learning 可以拆成：

```text
source reader
system design analyst
coding analyst
resume storyteller
public-safe reviewer
website publisher
```

这就是我们现在做 HKUDS learning 的方式。

## Why OpenOPC Matters for Founder OS

OpenOPC 最值得我们记住的是：

```text
founder 不是单点能力。
founder 是组织化复杂任务的能力。
```

如果我们以后做 C 面：

```text
拿资金
带团队
做产品
服务客户
对抗竞品
迭代市场
交付营收
```

那本质上就是：

```text
Self-Built: 组队
Self-Run: 执行
Self-Grown: 复盘升级
```

OpenOPC 把这个东西工程化成了一个 AI-native system。

这和我们的长期目标高度共鸣。

## Interview Hooks

如果以后面试里讲 OpenOPC，我们可以这样说：

### 30-second version

```text
I studied OpenOPC as an example of an AI-native organization system.
It goes beyond a single agent by introducing org charts, role assignment, work-item state machines, office UI, review loops, blockers, human escalation, and self-growth memory.
For me, it is useful because it shows how AI agents can move from isolated tools into structured team-based delivery systems.
```

### Chinese version

```text
我研究 OpenOPC 的重点不是“又一个 agent”，而是它把 agent 放进组织结构里：
角色、上下级关系、任务拆解、Kanban、review、blocker、人类审批、经验沉淀。
这对 AI + Quant Research OS 很关键，因为真实研究和真实交付都不是一个 prompt 就结束，而是一个持续协作、复盘、升级的系统。
```

### If asked: why relevant to finance?

```text
Finance workflows are naturally role-based and approval-heavy:
researcher, trader, risk reviewer, compliance, product owner, client-facing team.
OpenOPC's role/task/review/escalation design gives a useful template for building AI-assisted financial research and decision-support systems with human-in-the-loop control.
```

## Active Mock Questions

如果要主动拷打，这篇可以准备这些问题：

1. OpenOPC 和普通 multi-agent framework 的区别是什么？
2. 什么是 `Self-Built / Self-Run / Self-Grown`？
3. 为什么真实任务需要 work-item state machine？
4. 为什么 Office UI 比 chat UI 更适合复杂 agent work？
5. AgentSpace、OpenHarness、OpenOPC 的层级区别是什么？
6. OpenOPC 最近 commit 说明它处于什么阶段？
7. 它如何处理 blocker、approval、review 和 human escalation？
8. 这种系统如何迁移到 FICC / Quant Research / Banking workflow？
9. 如果让你做一个 `Pengyi Research OPC`，你会设计哪些 role？
10. 它对 C 面 founder OS 有什么启发？

## Final Takeaway

这篇 `HKUDS053` 的结论很明确：

```text
OpenOPC 是 HKUDS 从 agent runtime / workspace 进一步走向 organization OS 的关键项目。
```

它对我们的价值也很明确：

```text
我们不能只做散点项目。
我们要把项目、CV、面试、投递、研究、网站、GitHub、部署都变成一个可运行、可复盘、可升级的 Personal AI-Native Company。
```

这就是：

```text
Pengyi Research OS
  -> Pengyi Delivery OS
  -> Pengyi Founder OS
```

OpenOPC 是我们理解这个跃迁的一块关键拼图。

## Sources

- HKUDS OpenOPC: <https://github.com/HKUDS/OpenOPC>
- OpenOPC README: <https://github.com/HKUDS/OpenOPC/blob/main/README.md>
- OpenOPC Chinese README: <https://github.com/HKUDS/OpenOPC/blob/main/README.zh-CN.md>
- HKUDS organization: <https://github.com/HKUDS>
