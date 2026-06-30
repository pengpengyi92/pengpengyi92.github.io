---
title: "HKUDS033: ClawTeam 作为 Agent Swarm Intelligence 与 AI Organization Layer"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds033, hkuds, clawteam, agent-product, multi-agent, agent-workspace, ai-organization, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS033`。

```text
HKUDS033 -> ClawTeam
```

上一篇 `HKUDS032` 是第四张 StudyMap。

它把接下来的路线切到：

```text
Agent Product / Workspace
```

这篇正式开始第一站：

```text
ClawTeam
```

一句话定位：

```text
ClawTeam = framework-agnostic multi-agent coordination CLI
         + team/task/inbox protocol
         + git worktree isolation
         + tmux/subprocess spawn backend
         + board / Web UI monitoring
         + reusable team templates
         + agent skill / MCP interface
```

它不是普通的 multi-agent framework。

它最有价值的地方是：

```text
不是人写 orchestration code 来调 agent。
而是 agent 自己通过 clawteam CLI 创建团队、拆任务、发消息、看进度、合并工作。
```

这就是它对我们做 Pengyi Research OS 的关键启发：

```text
AI scientist 不应该只是一个单 agent。
它应该可以组织成一个可分工、可监控、可交接、可审计的 AI research team。
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `ClawTeam`。

| Item | Value |
|---|---|
| repo | `ClawTeam` |
| remote | `https://github.com/HKUDS/ClawTeam.git` |
| branch | `main` |
| local head | `0119833` |
| full commit | `01198332ef9270c32c5460b8a178f964fc0df451` |
| latest local commit date | `2026-05-09 15:25:55 +0800` |
| latest local commit | `Merge pull request #156 from HKUDS/feat/install-skills-from-scripts` |
| package | `clawteam` |
| version in `pyproject.toml` | `0.3.0` |
| Python requirement | `>=3.10` |
| tracked files by `rg --files` | 190 |
| Python files | 145 |
| Markdown files | 11 |
| TS/JS files | 3 |
| JSON files | 3 |

项目结构：

```text
clawteam/
  board/        terminal board, Web UI, activity visualization
  cli/          Typer CLI command surface
  events/       event bus and hooks
  harness/      harness-style orchestration components
  mcp/          FastMCP server and tool wrappers
  plugins/      plugin extension points
  spawn/        tmux/subprocess/WSH spawn backends and CLI adapters
  store/        task store abstraction and file implementation
  team/         team manager, mailbox, tasks, plans, lifecycle
  templates/    TOML team templates
  transport/    file and p2p transport
  workspace/    git worktree management

skills/
  clawteam/SKILL.md

tests/
  extensive test coverage for CLI, mailbox, tasks, spawn, board, MCP, workspace

website/
docs/
```

核心依赖：

```text
typer
pydantic
rich
questionary
mcp
```

可选依赖：

```text
pyzmq for p2p transport
redis for future / optional transport
```

## 一句话定位

ClawTeam 是一个：

```text
agent-native organization layer
```

它不是让人类写一个 Python 脚本来调用多个 agent。

它提供的是一个所有 agent 都能理解的组织协议：

```text
team
member
task
inbox
plan
lifecycle
workspace
board
```

每个 agent 都可以通过 CLI 做这些动作：

```bash
clawteam team spawn-team my-team -d "Build research OS" -n leader
clawteam spawn --team my-team --agent-name worker1 --task "Implement memory layer"
clawteam task list my-team --owner worker1
clawteam task update my-team <task-id> --status completed
clawteam inbox send my-team leader "Done. Summary: ..."
clawteam board show my-team
```

所以 ClawTeam 的真正抽象是：

```text
agent 不只是工具调用者。
agent 也可以成为组织成员。
```

## 为什么这很重要

单 agent 的问题很明显：

```text
上下文有限
任务串行
能力混杂
缺少分工
缺少交接
缺少组织记忆
缺少进度监控
```

复杂任务天然需要组织：

```text
PM
researcher
developer
reviewer
data engineer
tester
writer
risk manager
```

ClawTeam 把这件事工程化。

它提供一套很朴素但有效的组织底座：

```text
文件系统保存状态
CLI 暴露动作
git worktree 隔离工作
tmux 展示 agent 进程
task board 管进度
inbox 管通信
template 管团队模式
```

这比“多个 agent 在一个 prompt 里互相扮演角色”更接近真实生产系统。

因为它有真实的：

```text
process
workspace
state
message
task
branch
commit
dashboard
```

## 核心状态模型

ClawTeam 的状态模型在：

```text
clawteam/team/models.py
```

核心对象包括：

```text
TeamConfig
TeamMember
TaskItem
TeamMessage
```

任务状态：

```text
pending
in_progress
completed
blocked
```

成员状态：

```text
active
idle
shutdown
```

消息类型包括：

```text
message
join_request
join_approved
join_rejected
plan_approval_request
plan_approved
plan_rejected
shutdown_request
shutdown_approved
shutdown_rejected
idle
broadcast
```

这些看起来像普通枚举，但非常关键。

因为它们把一个松散的 agent 对话，变成了可以被机器读取和调度的组织事件。

对 Pengyi Research OS 来说，我们未来也需要类似 schema：

```text
research_task
experiment_task
backtest_task
review_task
writing_task
contact_task
application_task
```

而不是只有自然语言聊天记录。

## 文件系统作为组织状态层

ClawTeam 默认把状态放在：

```text
~/.clawteam/
```

README 里总结得很直接：

```text
teams/      who
tasks/      what
inboxes/    talk
workspaces/ isolated code
```

代码里 `TeamManager` 负责团队生命周期：

```text
create_team
discover_teams
add_member
remove_member
cleanup
list_members
resolve_inbox
```

任务用 `FileTaskStore`：

```text
{data_dir}/tasks/{team}/task-{id}.json
```

消息用 `MailboxManager`：

```text
{data_dir}/teams/{team}/inboxes/{agent}/msg-*.json
```

它还会写 event log：

```text
{data_dir}/teams/{team}/events/evt-*.json
```

写入方式是：

```text
tmp file -> os.replace
```

这保证了基本 crash safety。

任务 store 还用了 OS-specific advisory lock：

```text
Windows: msvcrt.locking
Unix: fcntl.flock
```

所以它不是纯 demo。

它在认真处理多进程并发写任务状态的问题。

## Task Board

ClawTeam 的 task layer 支持：

```text
create
get
update
list
wait
```

任务可以有：

```text
owner
priority
blocked_by
blocks
locked_by
metadata
```

当一个 task 被标记为 `completed` 时，它会自动尝试 unblock downstream tasks。

这对多 agent 非常重要。

因为 agent team 不是所有任务都能并行。

真实工作经常是：

```text
架构设计完成 -> 后端开始
后端完成 -> 测试开始
研究假设完成 -> 回测开始
回测完成 -> 风险诊断开始
风险诊断完成 -> PM review 开始
```

ClawTeam 把这种 dependency graph 放进 task board。

对我们的 Quant Research OS 来说，这直接对应：

```text
factor idea
-> data availability check
-> implementation
-> backtest
-> bias diagnosis
-> risk review
-> PM approve / reject
-> next research plan
```

## Inbox: Agent 间通信

ClawTeam 的通信层在：

```text
clawteam/team/mailbox.py
```

核心动作：

```text
send
broadcast
receive
peek
peek_count
event_log
```

默认 transport 是：

```text
file
```

可选：

```text
p2p via ZeroMQ
```

这里的设计很务实。

大部分个人/单机/共享文件系统场景，文件 inbox 就够用了。

如果需要低延迟或跨进程通信，再切到 p2p。

关键不是 transport 多高级。

关键是 agent 之间有了一个共同协议：

```bash
clawteam inbox send my-team worker1 "Please check the auth API"
clawteam inbox receive my-team --agent worker1
clawteam inbox broadcast my-team "Integration deadline in 30 min"
```

这让 agent collaboration 从“上下文里互相说话”变成：

```text
可持久化、可消费、可回放的 message queue。
```

## Spawn: 让 Agent 真的成为进程

ClawTeam 的 spawn 层在：

```text
clawteam/spawn/
```

主要后端：

```text
tmux
subprocess
WSH
```

`tmux` 是默认后端，适合交互式 agent：

```text
每个 agent 一个 tmux window
同一个 team 一个 tmux session
可以 board attach 观察所有 agent
```

`subprocess` 更适合：

```text
one-shot tool
headless script
non-interactive worker
```

spawn 时会注入环境变量：

```text
CLAWTEAM_AGENT_ID
CLAWTEAM_AGENT_NAME
CLAWTEAM_AGENT_TYPE
CLAWTEAM_TEAM_NAME
CLAWTEAM_AGENT_LEADER
CLAWTEAM_WORKSPACE_DIR
```

也会通过 `build_agent_prompt()` 给 worker 注入身份、任务、workspace 和协调协议。

这个 prompt 明确告诉 worker：

```text
查看自己的任务
开始任务
提交 commit
完成任务
给 leader 发消息
汇报 blocked
汇报 cost
继续轮询新任务
```

这点非常关键：

```text
ClawTeam 不是只启动多个进程。
它让每个进程知道自己是团队成员。
```

## Workspace: Git Worktree 隔离

ClawTeam 的 workspace layer 在：

```text
clawteam/workspace/
```

每个 agent 默认可以获得独立 git worktree：

```text
branch: clawteam/{team}/{agent}
path: ~/.clawteam/workspaces/{team}/{agent}
```

好处很实际：

```text
不同 agent 不会互相覆盖文件
每个 agent 有独立 branch
可以 checkpoint
可以 merge
可以 cleanup
可以看 diff
```

对应命令：

```bash
clawteam workspace list <team>
clawteam workspace checkpoint <team> <agent>
clawteam workspace merge <team> <agent>
clawteam workspace cleanup <team> <agent>
```

对软件工程任务，这是刚需。

对 research 任务也很重要。

例如我们未来可以让不同 agent 各自做：

```text
factor implementation branch
backtest experiment branch
report writing branch
data cleaning branch
```

最后由 leader 或 human PM 合并。

## Board and Web UI

ClawTeam 的 board layer 支持：

```text
board show
board live
board attach
board serve
board gource
```

`board show` 是终端 kanban。

`board attach` 是 tiled tmux view。

`board serve` 是 Web UI。

这说明 ClawTeam 不是只给 agent 用。

它也给人类一个观察窗口：

```text
谁在做什么？
哪些任务 blocked？
哪些 agent idle？
哪些 inbox 有消息？
哪些 worktree 有改动？
```

对我们非常重要。

因为 R&D Agent 不能变成黑箱。

它必须有：

```text
PM dashboard
human review surface
audit trail
progress ledger
```

## MCP Interface

ClawTeam 还提供：

```text
clawteam-mcp
```

MCP server 在：

```text
clawteam/mcp/server.py
```

工具清单包括：

```text
team_list
team_get
team_members_list
team_create
team_member_add
task_list
task_get
task_stats
task_create
task_update
mailbox_send
mailbox_broadcast
mailbox_receive
mailbox_peek
mailbox_peek_count
plan_submit
plan_get
plan_approve
plan_reject
board_overview
board_team
cost_summary
workspace_agent_diff
workspace_file_owners
workspace_cross_branch_log
workspace_agent_summary
```

这意味着 ClawTeam 不只是一组 shell commands。

它也可以作为 agent 工具服务器接入更大的 AI workspace。

对 Pengyi Research OS 来说，这意味着：

```text
team coordination can become a tool layer.
```

也就是 Research OS 里的主 agent 可以通过工具调用来管理整个 agent team。

## Skill Layer

ClawTeam 自带：

```text
skills/clawteam/SKILL.md
```

这个 skill 会告诉 Claude Code、Codex 等 agent：

```text
什么时候使用 ClawTeam
如何创建 team
如何 spawn worker
如何分配 task
如何通信
如何使用 board
如何处理 git context
如何做 snapshots and recovery
```

这点很重要。

因为 ClawTeam 的目标不是让人类记住所有命令。

它是让 agent 自己掌握组织协议。

换句话说：

```text
ClawTeam CLI = executable organization protocol
ClawTeam Skill = agent-readable organization manual
```

这两个合在一起，才构成真正的 agent-native organization layer。

## Templates: 组织形态产品化

ClawTeam 内置多种 TOML team templates：

```text
code-review
harness-default
hedge-fund
research-paper
software-dev
strategy-room
```

模板不是小功能。

它是把组织形态产品化的关键。

### software-dev

`software-dev` 包括：

```text
tech-lead
backend-dev
frontend-dev
qa-engineer
devops
```

这对应真实软件工程团队。

### research-paper

`research-paper` 包括：

```text
principal-investigator
literature-surveyor
methodology-designer
data-analyst
```

这对 AI scientist 非常直接。

它把一篇论文写作拆成：

```text
related work
methodology
results
synthesis
```

### strategy-room

`strategy-room` 包括：

```text
strategy-lead
systems-analyst
delivery-planner
risk-mapper
decision-editor
```

这很适合做复杂决策：

```text
要不要切方向？
要不要离职？
要不要做某个项目？
要不要追某个 research opportunity？
```

### hedge-fund

`hedge-fund` 对我们最有启发。

它包括：

```text
portfolio-manager
buffett-analyst
growth-analyst
technical-analyst
fundamentals-analyst
sentiment-analyst
risk-manager
```

这几乎就是一个简化版投资委员会。

对 Quant Research OS，可以直接改造成：

```text
PM Agent
Factor Research Agent
Backtest Agent
Risk Agent
Data Agent
Execution Agent
Report Agent
```

## 和前面 HKUDS 项目的关系

ClawTeam 不是孤立的。

它可以把前面很多能力组织起来。

```text
nanobot        -> personal agent shell
CLI-Anything   -> software action layer
AnyTool        -> universal tool-use routing
OpenHarness    -> agent harness runtime
OpenSpace      -> self-evolving skill workspace
FastCode       -> repo understanding acceleration
Auto-Deep-Research -> research assistant
DeepResearch-Eval  -> report evaluation
Paper2Slides       -> research artifact generation
```

ClawTeam 的作用是：

```text
把这些能力变成一个 team 可以协同执行的 workflow。
```

它不一定自己做 RAG、做 code intelligence、做 deep research。

但它可以组织不同 agent 去调用这些能力。

这就是 product layer 的意义。

## 对 Pengyi Research OS 的启发

ClawTeam 给我们的启发可以分成六点。

### 1. AI Scientist 应该是 Team，不只是 Agent

我们的目标不是做一个聊天机器人。

我们的目标是：

```text
AI scientist who can produce research, code, papers, reports, PRs, applications, and strategy.
```

这天然需要多个角色：

```text
PI / PM
Researcher
Developer
Reviewer
Data Engineer
Writer
Risk Critic
Opportunity Manager
```

ClawTeam 提供了一个工程化框架，让这些角色可以真的并行工作。

### 2. 任务必须显式化

很多 agent workflow 失败，是因为任务只存在于对话上下文里。

ClawTeam 把任务变成：

```text
task-{id}.json
```

有 owner、status、priority、blocked_by、metadata。

这对 Research OS 很重要。

以后每个研究 idea 都应该能落成：

```text
task
experiment
artifact
review
decision
```

### 3. 通信必须可审计

Agent 间通信不能只靠 prompt 里“我告诉你”。

它应该有 inbox 和 event log。

这样人类 PM 可以回看：

```text
谁说了什么？
什么时候 blocked？
哪个 agent 做了决定？
哪个证据被传给了下游？
```

这是安全和质量控制的基础。

### 4. 并行开发必须隔离

如果多个 agent 在同一个目录里改代码，很容易互相踩。

ClawTeam 用 git worktree 解决这个问题。

这对我们未来做开源项目、PR、研究系统都很重要：

```text
每个 agent 一个 branch
每个 branch 一个任务
最后统一 review / merge
```

### 5. 人类只应该做关键审核

ClawTeam 的方向是：

```text
human provides goal
agent team handles orchestration
human monitors and intervenes when needed
```

这和我们之前说的 PM review 一致。

人类不应该手动协调所有细节。

人类应该主要审核：

```text
goal
plan
risk
final artifact
merge decision
external communication
```

### 6. Team Template 是复用组织经验的方式

模板把一次成功的组织方式固化下来。

未来我们的 Research OS 可以有模板：

```text
factor-research-team
paper-reproduction-team
repo-pr-team
ra-application-team
phd-application-team
quant-interview-prep-team
research-blog-team
```

这比每次从零 prompt agent 更靠谱。

## Quant Research OS 映射

ClawTeam 最适合映射到我们的：

```text
R&D Agent for Quant Research
```

可以设计一个 `quant-research` team template：

```text
leader:
  portfolio-manager / research-pm

workers:
  factor-hypothesis-agent
  data-agent
  implementation-agent
  backtest-agent
  bias-diagnosis-agent
  risk-manager
  report-writer
  reviewer
```

任务流：

```text
1. PM receives goal:
   "Find a robust short-horizon equity factor idea."

2. hypothesis-agent proposes candidates.

3. data-agent checks data availability and leakage risk.

4. implementation-agent writes factor code.

5. backtest-agent runs experiment.

6. bias-diagnosis-agent checks overfitting, lookahead, survivorship, turnover, cost.

7. risk-manager evaluates drawdown and exposure.

8. report-writer produces PM memo.

9. PM / human approves next round.
```

ClawTeam provides the infrastructure:

```text
team
tasks
inboxes
worktrees
board
plans
cost summary
MCP tools
```

Our domain logic would provide:

```text
market data tools
backtest engine
factor library
risk diagnostics
research memory
PM review criteria
```

This is exactly the separation we want.

## 可以怎么使用在当前个人系统里

短期内，我们不一定要马上跑一个全自动 agent team。

更现实的使用方式是先把 ClawTeam 变成“个人研究组织模板”。

例如：

```text
Team: hkuds-study-team
Leader: study-pm
Workers:
  repo-reader
  architecture-mapper
  pr-opportunity-finder
  blog-writer
  website-publisher
```

或者：

```text
Team: ra-application-team
Leader: application-pm
Workers:
  cv-editor
  email-writer
  pi-researcher
  project-selector
  followup-manager
```

核心不是炫技。

核心是把我们现在正在做的事情系统化：

```text
学习 repo
写笔记
找 PR
更新网站
准备申请
联系导师
联系 quant senior
```

这些都可以变成 task board。

## 工程上值得注意的 PR 点

ClawTeam 工程质量相当完整，测试也很多。

但还是能看到一些实际可改进点。

第一，Windows / WSL / tmux 的使用路径可以进一步文档化。

README 写了需要 `tmux`，但对 Windows 用户，最好明确：

```text
推荐 WSL
tmux backend 的限制
subprocess backend 的可用场景
WSH backend 的状态
```

第二，`README` 很长。

可以补一个更短的：

```text
docs/quickstart-minimal.md
```

只保留：

```text
install
create team
spawn one worker
create task
send inbox
show board
cleanup
```

第三，template 可以增加 research/quant 方向的最小模板。

现在已有 `hedge-fund`，但偏股票投资委员会。

可以新增：

```text
quant-research.toml
```

角色：

```text
research-pm
factor-agent
data-agent
backtest-agent
risk-agent
report-agent
```

这会很适合我们以后提 PR。

第四，Web UI 如果只是 dashboard，可以考虑把 task/inbox/plan 的核心交互做得更强。

Agent Product 的关键不是只看状态，而是让 human PM 可以：

```text
approve plan
reject plan
reassign task
pause worker
merge workspace
request summary
```

第五，MCP 工具已经覆盖很多动作，但可以增加更高层 tool：

```text
create_project_team_from_goal
summarize_team_progress
find_blocked_agents
generate_pm_review_packet
```

这样更适合 agent 使用，而不是每次组合底层 tool。

这些都是产品化方向的真实改进。

## 我们应该怎么学 ClawTeam

这篇不是为了立刻跑 8 个 worker。

我们要学的是四层设计。

第一，组织抽象：

```text
team
member
role
task
message
plan
lifecycle
```

第二，运行抽象：

```text
spawn backend
agent process
runtime profile
environment variables
worker prompt
```

第三，隔离抽象：

```text
git worktree
branch
checkpoint
merge
cleanup
```

第四，产品抽象：

```text
board
Web UI
template
skill
MCP tools
```

这四层合在一起，才是一个 agent product。

## 最终总结

ClawTeam 的核心价值不是“能启动多个 agent”。

它真正重要的是：

```text
把 agent 组织成 team，并给这个 team 一套可执行的组织协议。
```

它用很工程化的方式把：

```text
team config
task board
inbox messaging
plan approval
lifecycle
git worktree isolation
tmux / subprocess spawning
board monitoring
team templates
MCP tools
agent skill
```

连成了一个 AI organization layer。

放进我们的路线：

```text
HKUDS032 StudyMap4 -> 规划 Agent Product / Workspace 系列
HKUDS033 ClawTeam  -> 多 agent team / AI organization layer
HKUDS034 ClawWork  -> 下一步看 agent workspace / work execution
```

对 Pengyi Research OS 来说，ClawTeam 是非常关键的一层：

```text
single AI assistant
-> AI research team
-> PM-supervised Research OS
-> repeatable open-source output machine
```

这就是 `HKUDS033 ClawTeam` 的位置。
