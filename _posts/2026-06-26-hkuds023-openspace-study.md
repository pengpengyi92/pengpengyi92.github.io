---
title: "HKUDS023: OpenSpace 作为 Self-Evolving Agent Workspace 与 Skill Economy Layer"
date: 2026-06-26 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds023, hkuds, openspace, agent-workspace, self-evolving-agent, skill-evolution, mcp, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第二十四篇。

```text
HKUDS023 -> OpenSpace
```

前面三篇形成了一条很清楚的链路：

```text
HKUDS020 FutureShow -> forecast / judgment ledger
HKUDS021 VideoRAG   -> video memory / multimodal knowledge ingestion
HKUDS022 FastCode   -> repo-level code intelligence / coding acceleration
```

这一篇进入 `OpenSpace`：

```text
OpenSpace -> self-evolving agent workspace
```

如果把它放进我们的主线里，它不是一个单点工具，而是一个更大的组织方式：

```text
agent
  -> tools
  -> skills
  -> execution recordings
  -> analysis
  -> skill evolution
  -> cloud sharing
  -> dashboard
```

也就是说，OpenSpace 想解决的问题不是“让 agent 完成一次任务”，而是：

```text
让 agent 从每一次任务里学习，
让成功经验沉淀成 skill，
让失败经验触发 repair，
让一个 agent 学到的能力可以共享给其他 agent。
```

这和我们一直说的 `R&D Agent for Quant Research` 很接近。真正的研究系统不能只会生成一次答案，它要能长期积累、复盘、改进和组织化。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `OpenSpace`。

| Item | Value |
|---|---|
| repo | `OpenSpace` |
| remote | `https://github.com/HKUDS/OpenSpace.git` |
| branch | `main` |
| local head | `228f8f7` |
| full commit | `228f8f78073dc4ed0e63fff01c19596c50115d40` |
| latest local commit date | `2026-06-03 00:04:45 +0800` |
| latest local commit | `fix: Windows PID check` |
| status | clean, synced with `origin/main` after fetch |
| package name | `openspace` |
| package version | `0.1.0` |
| Python requirement | `>=3.12` |
| license | MIT |
| tracked files by `git ls-files` | 863 |
| Python files | 166 |
| TypeScript / TSX / JS / JSX files | 100 |
| main folders | `openspace`, `frontend`, `gdpval_bench`, `showcase`, `assets` |
| main entrypoints | `openspace`, `openspace-mcp`, `openspace-dashboard`, `openspace-gateway` |
| frontend stack | React 18 + Vite 6 + Tailwind + TypeScript |
| benchmark | `gdpval_bench` |
| validation | `pyproject.toml` parsed; `import openspace` passed |
| syntax check | `py -m compileall -q openspace gdpval_bench` failed due `openspace/utils/ui.py` f-string syntax error |
| local frontend check | `npm` unavailable on current machine, frontend build not run |

一句话先行：

```text
OpenSpace 把 agent execution 变成一个可记录、可评估、可进化、可共享的 skill lifecycle system。
```

它真正有价值的地方不只是 “agent 可以做任务”，而是它把任务之后的复盘和能力沉淀做成了系统。

## 它解决什么问题

今天很多 AI agent 的问题是：

```text
每次任务都从零开始
成功经验不沉淀
失败原因不复盘
工具 API 变了以后旧 skill 静默失效
一个 agent 学会的东西不能迁移给另一个 agent
上下文越来越贵
复杂任务里重复踩坑
```

OpenSpace 的核心判断是：

```text
agent 不应该只是执行器。
agent 应该拥有可演化的技能库。
```

这非常关键。因为我们未来做 Research OS / Quant OS，也会遇到同样问题：

```text
今天读一个 repo 的流程，明天还要重来
今天做一个 backtest 的坑，明天还会踩
今天写 RA 套磁材料的结构，后天又要重新想
今天整理导师/项目/论文的模板，下一次不能复用
```

OpenSpace 说的是：

```text
把这些重复成功路径，抽象成 skill。
把失败恢复路径，也抽象成 skill。
把 skill 放进一个可演化的版本系统。
```

这就是它的系统价值。

## 总体架构

OpenSpace 可以拆成八层：

| Layer | Component | Role |
|---|---|---|
| Main Runtime | `OpenSpace` / `OpenSpaceConfig` | 初始化 LLM、grounding、recording、skills、evolver |
| Execution Agent | `GroundingAgent` | 多轮 tool use，完成任务 |
| Grounding Backends | shell / web / MCP / GUI / system | 给 agent 提供真实工具 |
| Skill Registry | `SkillRegistry` | 发现、检索、选择、注入 `SKILL.md` |
| Skill Store | `SkillStore` | SQLite 持久化 skill、lineage、quality、analysis |
| Execution Analyzer | `ExecutionAnalyzer` | 任务后复盘，判断 skill 是否有效、是否需要进化 |
| Skill Evolver | `SkillEvolver` | FIX / DERIVED / CAPTURED 三种进化 |
| Surfaces | CLI / MCP / dashboard / gateway / cloud | 给人、agent、团队、社区使用 |

主链路可以写成：

```text
task
  -> skill discovery / selection
  -> inject selected SKILL.md
  -> GroundingAgent executes with tools
  -> recording logs conversations and tool calls
  -> ExecutionAnalyzer reviews task outcome
  -> SkillEvolver suggests FIX / DERIVED / CAPTURED
  -> SkillStore persists lineage and metrics
  -> optional cloud upload / dashboard visualization
```

这个结构比普通 agent 更像一个工作系统：

```text
execution + memory + review + evolution + sharing
```

## OpenSpace 主入口

主类在：

```text
openspace/tool_layer.py
```

核心类是：

```python
class OpenSpace:
    ...
```

初始化时，它会依次创建：

```text
LLMClient
GroundingClient
RecordingManager
GroundingAgent
SkillRegistry
SkillStore
ExecutionAnalyzer
SkillEvolver
```

这说明 OpenSpace 并不是把所有逻辑塞在一个 agent prompt 里，而是把执行、记录、分析、进化分开。

这是很好的系统边界。因为 agent 系统一旦变复杂，最怕的是：

```text
prompt 里什么都有
运行时没有日志
失败后没有结构化证据
改 skill 没有版本记录
不同任务之间没有长期状态
```

OpenSpace 的结构正好相反。它把 agent 执行变成了可审计流程。

## Skill 是核心资产

OpenSpace 的核心对象不是 tool，而是 skill。

tool 是底层能力：

```text
shell
browser
MCP tool
GUI control
file operation
web search
```

skill 是可复用工作方法：

```text
如何写一个合规 PDF
如何处理 Excel merged cells
如何在 sandbox 失败后 fallback 到 shell
如何做 TypeScript typecheck
如何创建 full-stack dashboard panel
如何安全地完成 zip deliverable
```

这点非常重要。因为真正提高 agent 效率的，不只是工具数量，而是：

```text
知道什么时候用什么工具，
按什么顺序用，
遇到失败怎么恢复，
产出后怎么验证。
```

这就是 skill。

OpenSpace 的 `SkillRegistry` 会扫描 skill directories。每个 skill 是一个目录，里面有：

```text
SKILL.md
.skill_id
optional scripts / examples / config files
```

`.skill_id` 很关键。它让 skill 的身份不会因为目录移动就丢失：

```text
imported skill: {name}__imp_{uuid8}
evolved skill:  {name}__v{generation}_{uuid8}
```

这说明 OpenSpace 把 skill 当成长期资产，而不是临时 prompt 片段。

## Skill Selection

OpenSpace 不会把所有 skill 都塞进上下文。它先做选择。

选择流程大致是：

```text
discover all skills
BM25 / embedding prefilter
LLM selection
inject selected SKILL.md into system messages
```

默认配置里：

```text
skills.max_select = 2
```

这非常克制。它不追求“大而全”，而是只把当前任务最相关的 skill 注入。

`GroundingAgent` 还有一个细节很值得学：

```text
skill context 第一轮注入
第二轮开始从 messages 中移除
```

原因很明确：

```text
技能只需要引导第一轮规划。
后续上下文里已经有 plan、tool results、执行轨迹。
继续保留完整 SKILL.md 会浪费 token。
```

这就是 README 里 token efficiency 的工程实现之一。

对我们的 Research OS 也有启发：

```text
不要把所有笔记、所有规则、所有模板都塞给模型。
应该按任务检索少量高相关工作方法。
```

## GroundingAgent 执行层

`GroundingAgent` 是实际干活的 agent。

它会：

```text
检查 workspace 已有 artifacts
检索可用 tools
构建 messages
多轮调用 LLM
执行 tool calls
记录每轮 delta messages
看到 <COMPLETE> 后结束
```

它使用的 backend scope 默认包括：

```text
gui
shell
mcp
web
system
```

其中 tool retrieval 也不是简单列出全部工具，而是：

```text
get_tools_with_auto_search
ToolRanker
BM25 / embedding / hybrid ranking
optional LLM filter
tool cache
quality ranking
```

所以 OpenSpace 有两层检索：

```text
skill retrieval
tool retrieval
```

这两个层次不能混在一起：

```text
skill = 工作方法
tool = 执行能力
```

一个成熟 agent workspace 必须同时管理这两层。

## Recording 是进化的前提

如果没有记录，就没有复盘。

OpenSpace 的 `RecordingManager` 会记录：

```text
metadata
conversation logs
tool executions
iteration contexts
trajectory
screenshots / video if enabled
final outcome
```

`ExecutionAnalyzer` 读取这些记录，构造分析 prompt，然后输出结构化 `ExecutionAnalysis`。

分析对象包括：

```text
task_completed
execution_note
tool_issues
skill_judgments
evolution_suggestions
analyzed_by
analyzed_at
```

每个 skill 会被判断：

```text
skill_applied
note
```

这很像我们未来需要的 research review。

比如一个 quant research agent 做完一次实验后，也应该留下：

```text
task_completed
experiment_result
data issues
backtest bias
which template was useful
which tool failed
next improvement plan
```

没有这些，agent 就只是在不断生成内容，不是在成长。

## 三种 Evolution

OpenSpace 的 skill evolution 有三种：

| Evolution | Meaning |
|---|---|
| `FIX` | 修复坏掉或过期的 skill，同名同目录，新版本记录 |
| `DERIVED` | 从已有 skill 派生增强版或组合版，新目录 |
| `CAPTURED` | 从成功执行中捕获全新可复用模式 |

这三种刚好覆盖了真实工作里的三种成长：

```text
旧方法坏了 -> fix
旧方法有用但不够强 -> derive
刚发现一个新方法 -> capture
```

`SkillEvolver` 的触发也有三类：

```text
post-execution analysis
tool degradation
periodic skill metric monitor
```

也就是：

```text
任务结束后复盘
工具质量下降时触发
定期扫描技能指标
```

这已经很接近一个小型的组织学习系统。

## SkillStore：技能账本

OpenSpace 用 SQLite 存 skill 和分析记录。

核心表包括：

```text
skill_records
skill_lineage_parents
execution_analyses
skill_judgments
skill_tool_deps
skill_tags
```

每个 `SkillRecord` 有：

```text
skill_id
name
description
path
is_active
category
visibility
lineage
tool_dependencies
critical_tools
total_selections
total_applied
total_completions
total_fallbacks
recent_analyses
```

这就是 skill ledger。

它能回答：

```text
这个 skill 被选中过多少次？
真正被应用过多少次？
应用后任务完成率多少？
失败 fallback 多少次？
它从哪个 parent skill 进化来？
它的 diff 是什么？
它在哪个 task 中被捕获？
```

这非常重要。因为没有 ledger 的 skill library 很快会变成垃圾堆：

```text
不知道哪个 skill 有用
不知道哪个版本更好
不知道哪些 skill 已经过期
不知道哪些 skill 只是写得漂亮但没人用
```

OpenSpace 用数据结构把这个问题压住了。

## Patch 与版本管理

Skill evolution 最后要落到文件系统。

`openspace/skill_engine/patch.py` 支持三种 LLM 输出格式：

```text
FULL  -> 完整文件内容
DIFF  -> SEARCH/REPLACE
PATCH -> *** Begin Patch 多文件 patch
```

然后对应三种操作：

```text
fix_skill
derive_skill
create_skill
```

每次都会生成：

```text
content_diff
content_snapshot
```

这说明 skill evolution 不是“模型说自己改好了”，而是：

```text
模型输出修改
系统应用修改
系统生成 diff
系统保存完整快照
系统写 lineage
```

这是一个重要的工程质量点。

我们以后如果做 `Pengyi Research OS` 的 template evolution，也应该这么做：

```text
template version
parent template
diff
usage metrics
review note
active flag
```

否则模板会越积越乱。

## MCP Server：给其他 Agent 用

OpenSpace 暴露了 MCP server，核心工具是四个：

```text
execute_task
search_skills
fix_skill
upload_skill
```

这四个工具构成了一个很干净的交互协议：

| Tool | Role |
|---|---|
| `execute_task` | 委托任务，自动搜索 skill、执行、分析、进化 |
| `search_skills` | 单独搜索本地和云端 skill |
| `fix_skill` | 手动修复某个坏掉的 skill |
| `upload_skill` | 上传本地 skill 到云端社区 |

这和 FastCode 的 MCP 不同。

FastCode MCP 是：

```text
让 agent 更懂代码仓库。
```

OpenSpace MCP 是：

```text
让 agent 拥有一个可进化的外包工作空间。
```

所以两者可以连接：

```text
Codex / Claude Code
  -> FastCode: understand repo
  -> OpenSpace: delegate complex workflow and evolve skills
```

## Cloud Skill Community

OpenSpace 还有云端 skill community。

它提供：

```text
upload_skill
download_skill
search_record_embeddings
public / private visibility
artifact staging
lineage metadata
```

这就是 collective intelligence 的部分：

```text
one agent learns
all agents can benefit
```

对开源社区来说，这个想法很强。因为普通开源项目共享的是代码，而 OpenSpace 想共享的是：

```text
agent work patterns
failure recovery methods
tool usage recipes
quality assurance workflows
```

这更接近“工作方法市场”。

当然这里也有边界：

```text
公开上传前必须判断是否包含隐私、公司信息、客户信息、API key、项目特定路径。
```

所以它的 `upload_skill` 支持：

```text
public
private
```

这对我们很重要。未来我们自己的 private repo / pitch / career plan / quant data workflow，不能随便公开上传。

## Dashboard

OpenSpace dashboard 后端在：

```text
openspace/dashboard_server.py
```

前端在：

```text
frontend/
```

它展示：

```text
skills
skill stats
skill detail
lineage graph
workflow sessions
execution history
artifacts
pipeline stages
```

dashboard 的价值是把 skill evolution 从后台数据库变成可见资产。

这对我们未来非常重要。因为 Research OS 不能只有文件夹，它应该有 dashboard：

```text
今年读了多少 repo
哪些 repo 产生了 PR opportunity
哪些研究假设进入 backtest
哪些 template 被复用最多
哪些 workflow 失败率最高
哪些导师/RA 沟通材料已经成熟
```

OpenSpace 的 dashboard 给了一个参考形态。

## GDPVal Benchmark

OpenSpace 的 benchmark 很有意思。

它用 GDPVal 的真实职业任务来评估：

```text
220 occupational tasks
44 occupations
9 sectors
```

本地 `gdpval_bench/README.md` 说明了两阶段设计：

```text
Phase 1: Cold start
Phase 2: Warm start
```

也就是：

```text
第一轮顺序跑任务，让 skill 积累
第二轮用第一轮积累出的 skill library 重跑同样任务
```

这非常合理。因为 OpenSpace 的核心假设不是“一次任务更强”，而是：

```text
积累过 skill 之后，同类任务应该更便宜、更稳、更好。
```

README 里给出的核心结果是：

```text
4.2x higher income vs ClawWork baseline
45.9% token usage reduction in Phase 2 vs Phase 1
165 evolved skills across 50 Phase 1 tasks
```

这个 benchmark 最有价值的地方，不只是分数，而是评估方式。

它让我们看到一种测试 agent 进化能力的方法：

```text
same task distribution
cold start run
warm rerun
measure quality
measure token
measure economic value
inspect evolved skills
```

这对我们未来做 Quant Research OS 很有启发。

## 对 Quant Research OS 的启发

量化研究也可以类似两阶段：

```text
Phase 1: cold research
Phase 2: warm research with evolved workflows
```

比如第一轮做一批因子研究：

```text
momentum
reversal
volume-price divergence
earnings surprise
industry neutralization
turnover constraint
transaction cost adjustment
```

系统应该捕获出一批 skill：

```text
factor-data-cleaning
survivorship-bias-check
ic-decay-analysis
turnover-cost-diagnosis
sector-neutral-backtest
walk-forward-validation
factor-report-generation
pm-review-template
```

第二轮再做新因子时，就不应该从零开始。

它应该自动复用：

```text
数据检查
偏差诊断
回测模板
成本调整
报告结构
下一轮计划
```

这就是我们之前说的：

```text
R&D Agent for Quant Research
= 自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ 人类 PM 审核
```

OpenSpace 给我们的启发是：这些都应该沉淀成 skill，而不是散落在聊天记录里。

## 对 Career / Research OS 的启发

OpenSpace 不只适合技术任务。对我们的 career system 也有启发。

我们现在有很多重复工作：

```text
RA 套磁
PhD 预沟通
导师邮件附件
AI / Quant research role pitch
科研项目申请材料
网站 research statement
GitHub project study map
quant interview 问题准备
```

这些完全可以变成 skills：

```text
ra-email-draft
phd-pi-premeeting-brief
research-statement-one-page
quant-interview-advice-template
github-study-post-generator
project-pr-opportunity-scanner
```

每次真实使用后再进化：

```text
哪个 pitch 收到回复？
哪个材料更清楚？
哪个邮件模板太长？
哪个导师沟通问题更有效？
```

这就是 career workflow 的 skill evolution。

所以 OpenSpace 对我们的意义很直接：

```text
把人生和研究中的重复高价值动作，沉淀为可进化 skill。
```

## 和 FastCode 的连接

上一篇 `FastCode` 是 repo understanding engine。

这一篇 `OpenSpace` 是 self-evolving workspace。

两者可以组合：

```text
FastCode
  -> 快速理解一个开源 repo
  -> 找到架构、调用链、文档/代码不一致
  -> 生成 PR opportunity

OpenSpace
  -> 把这套读 repo / 提 PR 的流程沉淀成 skill
  -> 每次提 PR 后复盘
  -> 进化成更好的 repo study workflow
```

这很适合我们现在的开源路径：

```text
use project
read project
find issue
make PR
write study note
publish website
evolve workflow
```

换句话说：

```text
FastCode 加速单次读 repo。
OpenSpace 让读 repo 的方法本身进化。
```

## 和 AgentSpace / OpenHarness / AutoAgent 的区别

前面我们已经看过一些 agent framework：

| Repo | Main Role |
|---|---|
| `AgentSpace` | agent workspace / environment organization |
| `OpenHarness` | benchmark / tool runtime / evaluation harness |
| `AutoAgent` | agent construction and automation |
| `FastCode` | repo-level code intelligence |
| `OpenSpace` | skill evolution + collective agent workspace |

OpenSpace 的独特位置是：

```text
skill lifecycle
```

它关注的是：

```text
技能如何被发现
技能如何被选择
技能如何被使用
技能如何被评估
技能如何被修复
技能如何被派生
技能如何被捕获
技能如何被共享
```

这比“agent 能不能调用工具”更进一步。

## 安全边界

OpenSpace 是一个能力很强的 workspace worker，所以安全边界必须认真看。

默认 security config 里：

```text
allow_shell_commands: true
allow_network_access: true
allow_file_access: true
sandbox_enabled: false
```

同时也有 blocked commands：

```text
rm -rf
shutdown
reboot
format
taskkill /f
diskpart
...
```

这说明它不是纯沙盒玩具，而是偏真实执行环境。

因此我们的使用原则应该是：

```text
public demo 和个人练习可以放宽
真实公司/客户/私密数据必须隔离
涉及上传 cloud skill 必须人工审核
涉及 destructive / credential / private note 必须禁用或手动确认
```

这点对我们非常重要。因为我们有很多私密材料：

```text
黄超老师沟通
金敦泓老师材料
子健哥 quant talk
RA / PhD pitch
个人合同 / credit / position 规划
```

这些不应该进入公开 skill cloud。

## PR Opportunities

这次读下来，有几个比较清晰的 issue / PR 方向。

### 1. `openspace/utils/ui.py` 有 f-string 语法错误

本地运行：

```bash
py -m compileall -q openspace gdpval_bench
```

失败在：

```text
openspace/utils/ui.py:427
SyntaxError: f-string: unmatched '('
```

问题行是嵌套 f-string 引号冲突：

```python
colorize(f'{result.get('execution_time', 0):.2f}s', 'c')
```

同仓库 `openspace/utils/cli_display.py` 已经有更稳的写法：

```python
exec_time = result.get('execution_time', 0)
f"  Execution Time:  {colorize(f'{exec_time:.2f}s', 'c')}"
```

这是一个非常明确的小 PR：

```text
提取 exec_time 变量
修复 ui.py f-string
加一个 compileall 或 import UI smoke check
```

### 2. UI status mapping 没覆盖 `success`

`OpenSpace.execute()` 日志里把成功状态判断为：

```text
status == "success"
```

但 `ui.py` 的 `status_display` 主要覆盖：

```text
completed
timeout
error
```

CLI display 也更偏 `completed`。可以统一状态枚举：

```text
success
completed
timeout
error
incomplete
max_iterations_reached
cancelled
```

这是一个小的产品一致性 PR。

### 3. README 末尾有一个小语法问题

README 末尾写的是：

```text
Make You Agent Self-Evolve
```

更自然应该是：

```text
Make Your Agent Self-Evolve
```

这是非常小的 docs PR，但边界清楚。

### 4. 安全配置可以更明确地区分 demo 和 production

当前默认配置能力很强：

```text
shell / file / network allowed
sandbox disabled
blocked commands list enabled
```

可以在 docs 里补一个更清晰的 profile：

```text
local demo profile
private workspace profile
production/team profile
restricted/sandbox profile
```

这对用户理解风险有帮助。

### 5. Frontend build 环境依赖可以在 dashboard 文档里更明确

README 写 Node.js >= 20，frontend 里是 Vite/React。可以进一步说明：

```text
npm install
npm run build
openspace-dashboard serves frontend/dist if it exists
otherwise API-only JSON fallback
```

这能减少用户看到 “frontend not built” 时的困惑。

## 我们可以怎么吸收

不要一上来就复制 OpenSpace 全系统。

我们可以先吸收三层：

### 1. Skill Folder

先建一个私有目录：

```text
pengyi_skills/
  repo-study/
  quant-interview-prep/
  ra-email-draft/
  phd-pi-premeeting/
  website-post-generator/
  factor-backtest-checklist/
```

每个 skill 就是：

```text
SKILL.md
examples/
templates/
```

### 2. Skill Ledger

先不用复杂 DB，也可以用 markdown / json：

```text
skill_id
version
used_at
task
result
what_worked
what_failed
next_fix
```

### 3. Evolution Review

每次真实用完后问：

```text
这次有没有可复用步骤？
有没有失败恢复方法？
有没有模板需要改？
有没有应该公开的版本？
有没有必须保留 private 的内容？
```

这就是 OpenSpace 思路的轻量版。

## 对我们当前主线的意义

现在 HKUDS020-023 可以连成一个更完整的系统：

| ID | Repo | System Position |
|---|---|---|
| `HKUDS020` | `FutureShow` | judgment ledger / forecast benchmark |
| `HKUDS021` | `VideoRAG` | video memory / evidence ingestion |
| `HKUDS022` | `FastCode` | repo understanding / code intelligence |
| `HKUDS023` | `OpenSpace` | self-evolving agent workspace / skill economy |

连起来就是：

```text
VideoRAG
  -> 把高价值视频、访谈、讲座变成 knowledge input

FastCode
  -> 把 repo 和代码变成 structured engineering input

FutureShow
  -> 把判断变成可验证 forecast / ledger

OpenSpace
  -> 把重复工作方法变成可进化 skill
```

这就是 Research OS 的四个重要底座：

```text
knowledge ingestion
code understanding
judgment evaluation
workflow evolution
```

## 一句话总结

`OpenSpace` 的价值不是“多一个 agent 工具”。更准确地说：

```text
OpenSpace 把 agent 的工作过程变成可记录、可复盘、可进化、可共享的 skill economy。
```

对我们的 `Pengyi Research OS` 和 `Pengyi Quant Research OS` 来说，它意味着：

```text
真正该积累的不只是 notes 和 code，
还有一套不断进化的工作方法。
```

所以这篇的核心启发是：

```text
build the research engine,
but also build the self-evolving skill workspace.
```

## Next

下一组会进入 Graph / Knowledge Graph 主线：

```text
HKUDS024+ -> GraphAgent / OpenGraph / GraphGPT / HiGPT
```

如果说：

```text
OpenSpace gives us evolving workflows.
```

那么下一步就是：

```text
Graph projects give us structured knowledge and graph reasoning.
```

这会接回我们一直关心的：

```text
QuantMind
Research OS memory layer
knowledge graph
factor hypothesis graph
project / person / paper / repo relationship graph
```

也就是把 skill workspace 和 knowledge graph 接起来。
