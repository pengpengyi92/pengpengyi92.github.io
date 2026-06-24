---
title: "HKUDS006: AgentSpace 作为 Organizational Agent Workspace 与 Digital Employee Operating Layer"
date: 2026-06-24 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds006, hkuds, agentspace, agent-workspace, digital-employee, agentrouter, governance, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第七篇。

```text
HKUDS006 -> AgentSpace
```

到目前为止，HKUDS 第一阶段我们已经看了：

```text
HKUDS000 -> study map
HKUDS001 -> LightRAG
HKUDS002 -> Vibe-Trading
HKUDS003 -> nanobot
HKUDS004 -> CLI-Anything
HKUDS005 -> AI-Trader
```

现在看黄超老师组最新开源的 `AgentSpace`。我对它的定位是：

```text
AgentSpace = Organizational Agent Workspace + Digital Employee Operating Layer
```

如果说：

```text
LightRAG     = research memory
Vibe-Trading = quant research workflow
nanobot      = personal agent shell
CLI-Anything = software action layer
AI-Trader    = live trading platform layer
```

那么：

```text
AgentSpace = 让 agent 从个人工具进入组织工作空间，成为可管理、可调度、可授权、可审计的数字员工
```

这非常关键。前几个项目已经让我们看到：

```text
agent 能读知识
agent 能做研究
agent 能调用工具
agent 能进入交易平台
```

但真实组织里的问题不是“一个 agent 能不能完成一次任务”，而是：

```text
谁拥有这个 agent
谁能调用它
它能看哪些文档
它能用哪个 runtime
它做了哪些操作
哪些动作需要人类批准
任务失败后如何诊断
多天任务如何持续推进
产物如何回收到正式工作流
```

`AgentSpace` 的价值就在这里。它不是单 agent framework，而是 human + agent team 的工作空间。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `AgentSpace`。

| Item | Value |
|---|---|
| repo | `AgentSpace` |
| remote | `https://github.com/HKUDS/AgentSpace.git` |
| branch | `main` |
| local head | `a197533` |
| latest local commit | `ci: stop web service before production build` |
| status | clean, synced with `origin/main` |
| license | Apache 2.0 |
| package manager | npm |
| recommended Node.js | Node.js 24 |
| recommended database | PostgreSQL 16 |
| web | Next.js App Router |
| language | TypeScript |

本地规模：

| Metric | Count |
|---|---:|
| total files | 619 |
| TypeScript / TSX / MJS files | 568 |
| Markdown files | 6 |
| test files | 150 |

主结构：

```text
AgentSpace/
  apps/
    web/       # Next.js workspace UI
    cli/       # local control CLI
  packages/
    domain/    # shared domain model and daemon API types
    db/        # PostgreSQL persistence and runtime records
    services/  # business services used by web and CLI
    daemon/    # remote daemon package and AgentRouter CLI
    sandbox/   # sandbox abstraction and local/Cube scaffold
  deploy/
    postgres/
    systemd/
    nginx/
  asset/
```

这已经是一个非常完整的 product repo，而不是 research demo。

## Core Thesis

README 的标题是：

```text
AgentSpace: Human + Agents. One Team. One Workspace
```

这句话把它和普通 agent framework 区分开了。

普通 agent framework 关注：

```text
prompt
tool
memory
planner
executor
one conversation
one task
```

`AgentSpace` 关注：

```text
workspace
member
agent employee
owner
role
channel
task
document
runtime
permission
approval
audit
cost
performance
remote execution
```

也就是说，它不是问：

```text
How can one user call one agent?
```

而是问：

```text
How can a team work with many agents as accountable digital employees?
```

这就是它对我们的启发。

## The Real Problem

README 里对当前 agent workflow 的批判很准确。

现在很多 agent 仍然是：

```text
one person
one terminal
one chat session
one private account
```

这在个人效率场景里很好，但进入组织后会立刻出问题：

| Problem | Meaning |
|---|---|
| agents stay private | 强 agent 锁在某个人电脑或账号里，团队不可见 |
| context scattered | 消息、文档、审批、运行文件分散在各处 |
| execution fragmented | Claude Code、Codex、OpenClaw、Hermes 等 runtime 各有 session 和日志 |
| governance missing | 凭证、文件、工具调用、外发动作缺少统一审计 |
| work does not persist | 多天任务、handoff、重试、产物回收没有统一系统 |

所以 `AgentSpace` 的判断是：

```text
agents are powerful in isolation, but weak in teams
```

这句话非常值得记。

我们现在自己也有类似感受：一个强大的 coding agent 可以在本地帮我们写文章、改网站、提交 GitHub，但如果把它放进真实公司、实验室、quant team，就必须回答：

```text
它归谁管
谁能看它的输出
它能不能访问客户数据
它能不能发邮件
它能不能写 Google Sheet
它能不能动生产系统
它花了多少钱
它失败了谁来处理
```

这些不是模型能力问题，而是组织系统问题。

## Four Capabilities

`AgentSpace` 把核心能力分成四块：

```text
scheduling
capability sharing
collaboration
security / governance
```

我把它翻译成我们自己的系统语言：

| Capability | AgentSpace 含义 | 对我们自己的启发 |
|---|---|---|
| Scheduling | 同一个 agent 可以被路由到不同 runtime | agent identity 和执行 runtime 要解耦 |
| Capability | 私有 agent 变成组织可见资产 | 好 agent 应该有 owner、role、skills、knowledge |
| Collaboration | 人和 agent 在频道、任务、文档里一起工作 | agent 不能只活在 terminal，要进入工作流 |
| Security | 权限、审批、审计、撤销、诊断 | AI 时代组织最核心的不是放开，而是可控地放大 |

这四个能力非常适合我们之后理解公司、实验室、quant team 的 agent infrastructure。

## Digital Employee

`AgentSpace` 里最重要的概念是：

```text
digital employee
```

它不是把 agent 当函数调用，而是当一名组织成员。

一个 digital employee 需要这些字段：

```text
name
role
owner
summary
traits
instructions
skills
knowledge
channels
runtime binding
status
```

代码里 `ActiveEmployee` 的结构也接近这个方向：

```text
name
role
remarkName
ownerUserId
origin
summary
traits
fit
skillIds
channels
instructions
channelMemberAccess
status
```

这和我们过去说的“agent 小叮当”很像，但这里更组织化。

个人 agent 可以叫助手；组织 agent 必须叫员工。

因为一旦它进入真实组织，就必须有：

```text
岗位
责任
权限
上级
交付物
审计记录
成本记录
绩效记录
```

这才是 AI agent 真正进入企业的形态。

## Workspace As Operating Context

`AgentSpace` 的另一个关键判断是：

```text
work happens inside shared context
```

所以它把很多东西放进同一个 workspace：

```text
human members
digital employees
channels
direct conversations
tasks
documents
knowledge pages
skills
attachments
runtime outputs
approvals
notifications
costs
performance
settings
```

这像一个面向 agent 的 Feishu / Slack / Notion / task board / runtime console 的合体。

注意这里的重点不是 UI 组合，而是 context 统一。

没有 workspace 时，agent 的工作会散落在：

```text
terminal output
chat history
local files
Google Docs
spreadsheet
GitHub issue
email
personal memory
```

有 workspace 后，agent 的工作可以被绑定到：

```text
channel
task
document
approval
runtime output
audit event
notification
cost record
```

这就是从个人生产力工具走向组织生产力系统的分水岭。

## AgentRouter

`AgentRouter` 是 `AgentSpace` 最核心的执行抽象之一。

README 里说：

```text
AgentRouter is the provider harness normalization layer.
```

它不替代 workspace，也不拥有业务队列。它只做一件事：

```text
launch different agent CLIs and normalize events, results, sessions, diagnostics
```

当前支持：

| Provider | Execution path | Diagnostics |
|---|---|---|
| Claude Code | AgentRouter | stream-json events, session fallback, tool approval bridge |
| Codex CLI | AgentRouter | JSON events, session fallback, runtime tool capability diagnostics |
| OpenCode | AgentRouter | JSON events, session propagation, timeout/nonzero/empty diagnostics |
| OpenClaw | AgentRouter | health/preflight, auth/profile/model/tool/protocol diagnostics |
| Hermes Agent | AgentRouter | text output, executable checks, timeout and empty-response diagnostics |
| Gemini CLI | legacy provider runtime | one-shot CLI |
| NanoBot | legacy provider runtime | one-shot CLI |

这和我们看的 `CLI-Anything` 是上下游关系。

`CLI-Anything` 解决：

```text
how can agents call real software?
```

`AgentRouter` 解决：

```text
how can a workspace route one employee to different agent runtimes?
```

它的请求结构里包含：

```text
harness
prompt
cwd
model
mode
sessionId
env
timeoutMs
outputFormat
maxTurns
permissionMode
allowedTools
runtimeToolCapabilities
onApprovalRequest
```

返回结构包含：

```text
status
harness
sessionId
outputText
events
diagnostics
exitCode
startedAt
finishedAt
```

这就是 execution contract。

换句话说，workspace 不应该直接依赖某一个 provider 的 CLI 输出格式，而应该依赖统一 contract：

```text
event
tool call
approval request
session update
diagnostic
result
```

## Normalized Events

AgentRouter 把不同 provider 的原生事件归一成统一事件。

统一事件包括：

```text
harness_detected
harness_started
text_delta
thought_delta
approval_requested
tool_started
tool_output
tool_finished
session_updated
harness_exited
```

这个设计很重要。

不同 agent runtime 的输出格式一定不一样：

```text
Claude has stream-json
Codex has item.started / item.completed / thread.started
OpenClaw has its own JSON event shapes
OpenCode has run --format json
Hermes currently returns text output
```

如果上层 workspace 直接读每个 provider，就会越来越乱。

所以必须有一层归一化：

```text
provider-specific events
  -> AgentRouter event mapping
  -> workspace execution events
  -> UI / audit / task history
```

这对我们未来做 `Pengyi Research OS` 也一样。

无论底层用：

```text
Codex
Claude Code
OpenClaw
nanobot
local script
cloud worker
```

上层都应该看到同一套：

```text
task started
tool called
artifact created
approval requested
task completed
diagnostic emitted
```

## Remote Daemon

`AgentSpace` 的 remote daemon 是另一个关键。

它不是一个附属脚本，而是远程执行底座：

```text
agent-space-daemon
```

它的职责：

```text
connect remote machine to AgentSpace
detect provider CLIs
register runtimes
send heartbeat
poll tasks
materialize input bundle
run provider task
collect output bundle
write runtime outputs back
handle logs, PID, retry, graceful shutdown
```

启动形态：

```bash
agent-space-daemon start \
  --foreground \
  --server-url "https://your-agentspace-domain" \
  --daemon-token "adt_xxx" \
  --daemon-id "daemon-prod-01" \
  --device-name "prod-daemon-host-01" \
  --runtime-name "Remote Agent" \
  --task-timeout "43200000" \
  --state-dir "$HOME/.agent-space-daemon"
```

这对真实组织非常重要。

因为团队的 agent 任务不一定应该在网页服务器上跑。它可能需要：

```text
GPU machine
internal network machine
research workstation
isolated runtime
specific provider CLI login
company-controlled daemon host
```

所以系统要拆成：

```text
web app
database
task queue
remote daemon
provider runtime
output bundle
```

这也是我们做 quant system 时需要考虑的。

数据、执行、UI、审计不能全部混在一个 notebook 里。

## Task Queue

`AgentSpace` 的 task flow 很有组织感。

从代码看，创建任务会：

```text
validate channel
validate assignee employee
check whether current user can use this employee in this channel
check whether current user can use the bound runtime
create workspace task
write stored task
push channel message
if employee has runtime binding, enqueue native task
```

这就是组织里的真实约束：

```text
不是任何人都能随便让任何 agent 做任何事
不是任何 agent 都能在任何 channel 干活
不是任何人都能使用任何 runtime
```

这和我们之前对“后台三驾马车”的观察也能接起来。

真正的组织运行，不只是前台业务动作，而是：

```text
授权
流程
审批
记录
问责
资源调配
```

`AgentSpace` 把这些东西放到了 agent 工作流里。

## Approval System

审批系统是 `AgentSpace` 的核心治理层。

approval 类型包括：

```text
task_output
document_update
message_draft
runtime_tool
knowledge_proposal
```

其中最关键的是：

```text
runtime_tool
```

因为 agent 在执行任务时可能会请求敏感工具：

```text
write file
send message
access document
run command
use external credential
modify Google Sheet
```

这些不能全部自动批准。

所以 `AgentSpace` 里可以出现：

```text
agent asks
system creates approval
owner/admin reviews
decision recorded
agent receives result
task continues or stops
```

这就是 human-in-the-loop 的企业形态。

不是让人类手动做所有事，而是让人类只在风险边界处做决策。

## Permission Control Plane

`AgentSpace` 的权限模型非常值得学习。

它不是只做一个简单的 `admin/member`，而是把权限拆到多种资源：

```text
workspace
workspace invitation
channel
channel invitation
channel access request
agent
agent fork invitation
agent access request
runtime
daemon
file
document
external document
skill
knowledge page
oauth credential
```

主体也不只是人：

```text
human
agent
daemon_token
oauth_credential
system
```

权限来源包括：

```text
workspace_role
direct_grant
channel_participant
document_collaborator
document_agent_access
document_permission_request
runtime_grant
agent_owner
agent_fork
agent_access_request
agent_channel_member_access
knowledge_assignment
skill_assignment
oauth_delegation
external_drive_permission
derived
```

这非常像真实公司里的权限系统。

一个 agent 能不能做某件事，不应该只看一句 prompt，而要看：

```text
它是谁
谁拥有它
它在哪个 workspace
它在哪个 channel
它被分配了哪些 skill
它能读哪些 knowledge page
它绑定了哪个 runtime
它是否被授予 runtime use
它是否被授权访问 Google credential
这个动作是否需要审批
```

这就是组织级 agent safety 的核心。

## Database Schema

PostgreSQL schema version 当前是：

```text
18
```

表的覆盖面非常完整。

workspace 和成员：

```text
workspace
users
auth_identity
session
workspace_membership
workspace_invitation
workspace_channel
channel_participant
channel_access_request
channel_invitation
```

agent 和 runtime：

```text
workspace_employee
employee_runtime_binding
daemon_connection
daemon_api_token
agent_runtime
workspace_runtime_grant
workspace_runtime_display_name
```

任务和执行：

```text
workspace_task
agent_task_queue
agent_task_attempt
agent_router_session
agent_router_provider_session
agent_router_event
agent_router_context_snapshot
task_execution_event
task_message
```

文档、知识、skill：

```text
document_agent_access
document_permission_request
skill
skill_file
runtime_app_skill_binding
skill_import_event
agent_skill
knowledge_page_assignment_policy
agent_knowledge_page
knowledge_proposal
```

治理和成本：

```text
workspace_notification
google_oauth_credential
agent_google_workspace_delegation
budget
token_usage
model_pricing
audit_log
attachment
```

这套 schema 说明一件事：`AgentSpace` 已经在认真处理组织级 agent 系统的长期状态。

它不只是 memory，而是 operational state。

## Web App Surface

`apps/web` 是 Next.js App Router。

workspace 里有很多页面：

```text
agents
approvals
automations
calendar
contacts
costs
im
inbox
knowledge
market
org-chart
performance
settings
skills
tables
task-board
templates
```

这说明 `AgentSpace` 的 product thinking 很完整。

它不是只做一个聊天页面，而是围绕组织工作流建立多个入口：

| Page | Meaning |
|---|---|
| `im` | 工作区消息和频道 |
| `agents` | digital employee 管理 |
| `task-board` | 任务看板 |
| `approvals` | 审批队列 |
| `knowledge` | 知识和文档页面 |
| `skills` | skill 库 |
| `costs` | 成本和预算 |
| `performance` | 绩效看板 |
| `settings` | 权限、成员、runtime、workspace 设置 |

这个产品结构对我们自己的个人 OS 也有启发。

如果我们未来做 `Pengyi Research OS` 的 web UI，也不能只有聊天窗口。至少应该有：

```text
projects
agents
experiments
data sources
backtests
reports
approvals
tasks
knowledge
costs
outputs
```

## CLI Surface

`apps/cli` 提供本地控制台。

命令入口包括：

```text
doctor
db
daemon
dev
workspace
im
channel
employee
material
message
task
skill
output
cost
```

这点也很关键。

一个 serious system 应该同时有：

```text
web UI for humans
CLI for automation
API for agents
database for state
daemon for execution
```

如果只有 web UI，agent 很难自动化。如果只有 CLI，团队治理和可视化又不够。

`AgentSpace` 同时保留了这两个入口。

## Skills And Knowledge

`AgentSpace` 也有 skill 和 knowledge 系统。

它支持：

```text
file-backed workspace skills
skill import / export
assign skills to agents
knowledge pages
materials
attachments
channel docs
generated knowledge proposals
agent knowledge page assignment
```

这和我们之前看的 `CLI-Anything`、`nanobot`、`LightRAG` 能接上。

一名 digital employee 不应该只有 prompt，它应该有：

```text
instructions
skills
knowledge
runtime
documents
permissions
owner
channels
```

这才像一个真正可以上岗的 agent。

## Google Workspace

`AgentSpace` 里也考虑了 Google Workspace。

环境变量里有：

```text
AGENT_SPACE_GOOGLE_CLIENT_ID
AGENT_SPACE_GOOGLE_CLIENT_SECRET
AGENT_SPACE_GOOGLE_CALLBACK_URL
AGENT_SPACE_GOOGLE_WORKSPACE_CLIENT_ID
AGENT_SPACE_GOOGLE_WORKSPACE_CLIENT_SECRET
AGENT_SPACE_GOOGLE_WORKSPACE_TOKEN_ENCRYPTION_KEY
AGENT_SPACE_GOOGLE_DRIVE_PARENT_FOLDER_ID
```

DB 里有：

```text
google_oauth_credential
agent_google_workspace_delegation
```

这说明它不是把 Google Docs / Sheets 当外部玩具，而是纳入权限体系。

关键点是：

```text
agent-scoped delegation
```

也就是说，agent 使用 Google 资源时，不应该无限继承某个人的所有权限，而应该被明确授权。

这对真实组织很重要。

## Cost And Budget

`AgentSpace` 还有 cost 和 budget。

CLI 支持：

```text
agent-space cost summary
agent-space cost agent
agent-space cost recent
agent-space cost pricing
agent-space cost budget list
agent-space cost budget set
agent-space cost budget check
```

DB 里也有：

```text
model_pricing
token_usage
budget
```

这点非常现实。

AI agent 一旦进入组织，不只是“能不能做”，还要问：

```text
花了多少钱
哪个 agent 花得最多
哪个 channel 花得最多
哪个 workspace 花得最多
是否超过预算
超过预算后 warn / pause / approve
```

这就是财务视角进入 agent workflow。

我们之前聊“财务、法务、人力”是组织后台三驾马车。`AgentSpace` 里其实也有对应影子：

```text
finance -> cost / budget / pricing
legal / compliance -> permission / approval / audit
HR / org -> member / digital employee / owner / role / org chart
```

这点非常有意思。

## Audit Log

DB 里有 `audit_log`，services 里也有 `tryRecordWorkspaceAuditEventSync`。

对 agent 系统来说，audit log 不是可选项。

因为 agent 会做真实动作：

```text
create task
read document
write document
request runtime tool
use Google credential
send message
bind runtime
grant permission
revoke permission
delete agent
```

如果没有 audit log，组织无法追责，也无法复盘。

所以 agent organization 的基础设施一定包括：

```text
what happened
who initiated it
which agent acted
which resource was touched
which permission source allowed it
who approved it
what output was produced
```

这就是 agent governance 的底层逻辑。

## Sandbox Layer

`packages/sandbox` 提供 sandbox 抽象。

当前包括：

```text
local sandbox
Cube scaffold
```

daemon README 里明确说，当前 `cube` 还只是 lifecycle scaffold，不要把它当成已经可生产执行的隔离 runtime。

这个表述很务实。

隔离执行是未来必须做的，因为 agent 能力越强，风险越高。

未来 agent 任务最好进入：

```text
isolated workdir
isolated credentials
network policy
filesystem policy
artifact boundary
approval boundary
destroy / snapshot lifecycle
```

这和公司里“生产权限”和“沙盒环境”的逻辑一样。

## Relation To Previous HKUDS Projects

现在我们可以把 HKUDS 前几篇接起来。

| Project | Layer | Role |
|---|---|---|
| LightRAG | knowledge memory | 让 agent 读论文、文档、报告，有 source-grounded memory |
| Vibe-Trading | quant research workflow | 让 agent 把金融问题变成研究、策略、回测 |
| nanobot | personal agent shell | 让 agent 长期运行，接消息和工具 |
| CLI-Anything | software action layer | 让 agent 调用真实软件 |
| AI-Trader | trading platform layer | 让 agent 发信号、交易、跟单、比赛、导出研究数据 |
| AgentSpace | organizational workspace layer | 让人和 agent 在同一个组织上下文里协作、授权、审计 |

这张图非常清楚：

```text
LightRAG gives memory.
Vibe-Trading gives research workflow.
nanobot gives personal runtime shell.
CLI-Anything gives software actions.
AI-Trader gives trading society.
AgentSpace gives organizational workspace.
```

换成我们的系统语言：

```text
knowledge
  -> research
  -> personal execution
  -> software action
  -> platform interaction
  -> organization governance
```

这已经是一个完整 Research OS 的雏形。

## Difference From AI-Trader

`AI-Trader` 和 `AgentSpace` 都是 platform，但平台对象不同。

| Dimension | AI-Trader | AgentSpace |
|---|---|---|
| Core object | trading agent | digital employee |
| Main environment | trading market | organizational workspace |
| Primary actions | signal, trade, copy, challenge | message, task, document, runtime, approval |
| Evaluation | PnL, leaderboard, signal quality, network edges | task completion, cost, performance, audit, permission |
| Governance | trading-specific risk and platform auth | organization-wide permission, approval, credential delegation |
| Research value | multi-agent trading behavior | human-agent organization and digital labor infrastructure |

两者结合起来非常强：

```text
AgentSpace manages the organization.
AI-Trader manages the trading arena.
```

对于 quant team 来说，可以是：

```text
AgentSpace = team operating workspace
AI-Trader  = trading simulation / competition / live signal platform
Vibe-Trading = research pipeline
LightRAG = research memory
```

## Difference From nanobot

`nanobot` 更像个人 agent shell：

```text
one agent
always-on
messages
tools
MCP
personal workspace
```

`AgentSpace` 更像组织层：

```text
many humans
many agents
many runtimes
many documents
many permissions
many approvals
many tasks
```

所以可以这样理解：

```text
nanobot = a strong personal agent process
AgentSpace = a company where many agents can work
```

两者可以互补。`nanobot` 可以作为一种 runtime / employee shell，被 `AgentSpace` 管理和调度。

## Difference From CLI-Anything

`CLI-Anything` 解决的是软件动作表面：

```text
turn software into agent-native CLI
```

`AgentSpace` 解决的是组织管理：

```text
who can use which runtime, which skill, which document, under which approval boundary
```

可以这样接：

```text
CLI-Anything exposes tools.
AgentSpace governs who can use them.
```

这对真实企业很关键。

有工具不等于可以随便用工具。

组织系统必须知道：

```text
which CLI app is installed
which runtime can access it
which agent has the skill
which user can trigger it
which action needs approval
where output goes
```

## Pengyi Use Case

`AgentSpace` 对我们自己的启发非常直接。

我们现在想做：

```text
Pengyi Quant Research OS v0
R&D Agent for Quant Research
AI scientist workflow
RA / PhD / open-source project pipeline
```

之前我们更多关注：

```text
idea generation
factor implementation
backtest
diagnosis
report
next research plan
```

`AgentSpace` 提醒我们，还要做组织层：

```text
agent role
agent owner
agent permission
agent task queue
agent runtime binding
research approval
data access control
artifact audit
budget and cost
team workspace
```

也就是说，`Pengyi Research OS` 不只是一个代码 repo，而应该是一个 workspace：

```text
Human PM
  -> approves risky research decisions

Research Agent
  -> proposes hypotheses and reads papers

Developer Agent
  -> implements factor and tests code

Backtest Agent
  -> runs experiments and checks leakage

Reviewer Agent
  -> audits bias, data quality, and report claims

Writer Agent
  -> turns results into paper / blog / README
```

每个 agent 都应该有：

```text
role
instructions
skills
knowledge
runtime
permission
tasks
outputs
audit log
```

这就是我们之后可以做的组织化 R&D Agent。

## Quant Team Mapping

如果映射到 quant team，可以这样设计：

| Human / Agent | Role |
|---|---|
| Human PM | 决定研究方向、审核高风险动作、判断是否进入下一阶段 |
| Research Agent | 读 paper、提出因子假设、整理研究背景 |
| Data Agent | 检查数据源、字段、缺失、权限、版本 |
| Dev Agent | 实现因子、写 pipeline、跑测试 |
| Backtest Agent | 运行回测、输出 metrics、画图 |
| Risk Agent | 检查暴露、换手、容量、泄漏、过拟合 |
| Report Agent | 写研究报告、网站笔记、PRD、paper draft |
| Ops Agent | 跟踪任务、整理产物、维护知识库 |

`AgentSpace` 给这套角色体系提供了工程参考。

尤其是：

```text
role
owner
runtime binding
channel
task board
approval
permission
audit
cost
performance
```

这些都是 quant research 真正工程化之后会遇到的东西。

## What To Copy Into Our Own System

我认为最值得吸收的设计：

| AgentSpace Design | Pengyi 可以吸收什么 |
|---|---|
| digital employee | 把 agent 从脚本升级成有角色和 owner 的研究成员 |
| workspace | 把项目、任务、文档、报告、产物放进统一上下文 |
| AgentRouter | 底层 runtime 可切换，上层 agent identity 不变 |
| remote daemon | 研究任务可以在专门机器上长时间执行 |
| task queue | 多天任务、重试、handoff、状态变更可追踪 |
| approval system | 高风险操作需要人类 PM 审核 |
| permission center | 数据、文档、runtime、skill 权限要统一 |
| audit log | 所有研究动作和产物可复盘 |
| cost budget | token、模型、运行成本进入系统 |
| knowledge proposals | agent 可以提出知识更新，但需要审核后入库 |

这就是从“AI 帮我做事”走向“AI 作为组织生产力”的关键。

## Possible PR Directions

如果之后我们给 `AgentSpace` 提 PR，比较自然的方向是从真实使用出发。

可以考虑：

| Direction | Why It Is Useful |
|---|---|
| Windows local setup guide | 我们本地就是 Windows，可以真实踩坑后补文档 |
| AgentSpace x Codex quickstart | 给 Codex runtime 的最小可运行流程 |
| AgentSpace x nanobot guide | 把 nanobot 作为 runtime / employee shell 的说明 |
| AgentSpace x CLI-Anything guide | agent runtime 如何发现、安装、调用 CLI-Hub apps |
| Research team template | 做一个 research lab / quant team 的 workspace 模板 |
| Approval workflow examples | runtime_tool、knowledge_proposal、document_update 的具体案例 |
| Permission model docs | 用图解释 subject/resource/source/status |
| AgentRouter diagnostic table | 把各 provider 常见错误整理成 troubleshooting 文档 |
| Cost/budget tutorial | 如何设置 workspace / agent / channel 预算 |
| Demo seed data | 一键创建 founder team / research team / quant team demo |

这些都是真实使用中会遇到的问题，不是为了 PR 而 PR。

## Risks And Boundaries

`AgentSpace` 的方向很强，但也有几个边界要清楚。

第一，它是一个活跃开发中的产品仓库，变化会很快。我们学习时要以当前 commit 为准：

```text
a197533
```

第二，组织级 agent 系统会直接碰到安全问题：

```text
credential leakage
over-permissioned agent
wrong document access
tool abuse
runtime host compromise
unbounded spending
unreviewed external action
audit gap
```

第三，remote daemon 非常强，但也意味着本机/服务器上的 provider CLI 和凭证要被认真管理。

第四，sandbox 现在还不是完全成熟的生产隔离执行层。当前 `cube` 是 scaffold，不能把它误解为已经生产可用的隔离 runtime。

这些不是缺点，而是组织级 agent 系统必须面对的真实问题。

## Why It Matters For Us

`AgentSpace` 对我们最大的启发是：

```text
AI 生产力放大，不只是更强的模型，而是更好的组织系统。
```

一个人用 agent 可以提升效率。

一个组织用 agent，需要：

```text
身份
权限
流程
审批
审计
成本
任务
知识
运行时
产物回收
```

这和我们正在理解的公司后台视角完全一致。

财务、法务、人力为什么重要？因为它们掌握：

```text
资源
风险
权限
合同
岗位
组织结构
责任边界
```

未来 agent 进入组织后，也必须被这些系统接住。

所以 `AgentSpace` 不只是一个开源项目，它其实是在回答：

```text
AI agent 如何成为组织里的数字员工？
```

这对我们之后做顶会研究、做开源项目、做 AI scientist 叙事都很重要。

## Final Map

现在 HKUDS 第一阶段可以更新为：

```text
HKUDS000 -> Study Map
HKUDS001 -> LightRAG
  = source-grounded research memory

HKUDS002 -> Vibe-Trading
  = agentic quant research workflow

HKUDS003 -> nanobot
  = personal always-on agent shell

HKUDS004 -> CLI-Anything
  = agent-native software action layer

HKUDS005 -> AI-Trader
  = agent-native live trading platform layer

HKUDS006 -> AgentSpace
  = organizational agent workspace and digital employee operating layer
```

把它们合起来：

```text
knowledge memory
  -> research workflow
  -> personal agent shell
  -> software action layer
  -> trading platform
  -> organizational workspace
```

这已经不是单点项目学习了，而是在形成我们的系统设计地图。

下一步可以继续做 `HKUDS007`：把 `LightRAG + Vibe-Trading + nanobot + CLI-Anything + AI-Trader + AgentSpace + LLMQuant` 合成 `Pengyi Quant Research OS v0` 的完整架构图。
