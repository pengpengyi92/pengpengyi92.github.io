---
title: "HKUDS015: OpenHarness 作为 Agent Harness Runtime 与 Personal Agent Infrastructure Layer"
date: 2026-06-25 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds015, hkuds, openharness, ohmo, agent-harness, agent-runtime, tool-use, mcp, skills, plugins, swarm, memory, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第十六篇。

```text
HKUDS015 -> OpenHarness
```

上一篇 `HKUDS014` 我们看了 `DeepTutor`：

```text
DeepTutor = Agent-Native Personalized Tutoring + AI Scientist Self-Training Layer
```

`DeepTutor` 解决的是：

```text
怎么训练人？
怎么把学习、知识、记忆、Mastery Path、Co-Writer、Subagents 接起来？
```

这一篇自然接到 agent runtime 本身：

```text
OpenHarness = Agent Harness Runtime + Personal Agent Infrastructure Layer
```

如果说 `DeepTutor` 更像一个 personal learning workspace，那么 `OpenHarness` 更像一个“把 LLM 包装成真正 agent 的底层 harness”。

它的定位不是单个 chatbot，而是：

```text
model
+ tools
+ permissions
+ memory
+ skills
+ plugins
+ MCP
+ terminal UI
+ provider workflows
+ background tasks
+ swarm coordination
+ personal agent gateway
```

这对 Pengyi Research OS 的意义很直接：

```text
Research OS 需要上层 workflow。
但 workflow 要真的运行起来，需要一个可靠的 harness runtime。
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `OpenHarness`。

| Item | Value |
|---|---|
| repo | `OpenHarness` |
| remote | `https://github.com/HKUDS/OpenHarness.git` |
| branch | `main` |
| local head | `9b2efd7` |
| latest local commit date | `2026-06-04 02:42:36 +0000` |
| latest local commit | `fix(config): preserve profile auth when overriding model` |
| status | clean, synced with `origin/main` after fetch |
| latest local tag | `v0.1.9` |
| package name | `openharness-ai` |
| version in `pyproject.toml` | `0.1.9` |
| Python requirement | `>=3.10` |
| license | `MIT` |
| tracked files by `rg --files` | 463 |
| test files | 121 under `tests/` |
| syntax check | `py -m compileall -q src ohmo` passed |
| CLI entries | `openharness`, `oh`, `openh`, `ohmo` |

一句话先行：

```text
OpenHarness 是一个轻量但完整的 agent harness：
它把 model loop、streaming tool-use、permission governance、skills/plugins/MCP、memory/compaction、tasks/swarm、provider auth、TUI 和 personal agent app 组织成一个可扩展 runtime。
```

## 什么是 Agent Harness

README 里给了一个非常好的定义：

```text
An Agent Harness is the complete infrastructure that wraps around an LLM to make it a functional agent.
```

这句话很关键。

LLM 本身只提供 intelligence。

要让它成为 agent，还需要：

```text
hands   -> tools / file / shell / browser / MCP
eyes    -> read / grep / web_fetch / image_to_text / LSP
memory  -> MEMORY.md / session history / auto-compact / auto-dream
safety  -> permissions / path rules / command deny / hooks / sandbox
runtime -> streaming loop / retry / cost tracking / context management
surface -> CLI / TUI / JSON output / channel gateway
team    -> tasks / subagents / swarm / mailbox / worktrees
```

这就是 harness 的价值。

很多 agent 项目最容易卡在这里：

```text
prompt 很强，但工具不稳。
工具很多，但权限不可控。
能改代码，但不能长期记忆。
能跑任务，但没有 trace 和恢复机制。
能接一个模型，但换 provider 就崩。
能手动聊天，但无法脚本化和 CI 化。
```

OpenHarness 的目标就是补这些 agent 基建。

## OpenHarness 和 ohmo

OpenHarness repo 里其实有两个重要入口：

```text
oh / openharness / openh -> OpenHarness CLI and terminal agent
ohmo                    -> personal AI agent app built on OpenHarness
```

README 的定位是：

```text
OpenHarness delivers core lightweight agent infrastructure:
tool-use, skills, memory, and multi-agent coordination.
```

而 `ohmo` 是：

```text
a personal AI agent built on OpenHarness
```

它可以通过 Feishu / Slack / Telegram / Discord 等渠道工作，并复用 Claude Code 或 Codex subscription。

这说明它的架构分层是：

```text
OpenHarness = harness runtime
ohmo        = personal-agent application built on that runtime
```

这点对我们非常重要。

因为我们自己的系统也应该分层：

```text
Pengyi Agent Harness       -> 底层运行时
Pengyi Research OS         -> research workflow
Pengyi Quant Research OS   -> quant workflow
Pengyi Personal Agent      -> daily working assistant
```

不要把 runtime 和 application 混成一坨。

## 项目结构

本地结构非常清晰。

```text
OpenHarness/
  src/openharness/
    engine/
    tools/
    permissions/
    skills/
    plugins/
    mcp/
    memory/
    prompts/
    api/
    ui/
    commands/
    tasks/
    swarm/
    sandbox/
    hooks/
    channels/
    autopilot/
    config/
  ohmo/
  frontend/terminal/
  autopilot-dashboard/
  tests/
  docs/
  scripts/
```

README 里把架构概括成 10 个 subsystem：

```text
engine      -> Agent Loop
tools       -> File/Shell/Search/Web/MCP tools
skills      -> on-demand skill loading
plugins     -> commands, hooks, agents, MCP servers
permissions -> safety and governance
api         -> provider clients
ui          -> terminal UI
mcp         -> Model Context Protocol client
memory      -> persistent cross-session memory
prompts     -> system prompt assembly
```

从代码来看还可以补充几个同样关键的 subsystem：

```text
tasks       -> background shell / local-agent tasks
swarm       -> multi-agent team lifecycle and mailbox
hooks       -> PreToolUse / PostToolUse / Stop / Notification
sandbox     -> srt / docker execution isolation
channels    -> Feishu / Slack / Telegram / Discord / etc.
autopilot   -> repo autopilot queue, verification, policy, dashboard
ohmo        -> personal agent workspace and gateway
```

这已经是一个比较完整的 agent runtime reference。

## 核心一：QueryEngine

我读了 `src/openharness/engine/query_engine.py`。

`QueryEngine` 是高层 conversation engine。

它持有：

```text
api_client
tool_registry
permission_checker
cwd
model
system_prompt
conversation messages
cost tracker
hook executor
tool metadata
settings
```

每次用户输入进入 `submit_message` 后，它会：

```text
记录用户目标
准备 session memory
清理 conversation messages
触发 USER_PROMPT_SUBMIT hook
构造 QueryContext
注入 coordinator context
调用 run_query
更新 session memory
抽取 durable memories
调度 auto_dream
```

这说明 OpenHarness 不是一个“裸 API wrapper”。

它真正管理的是一次 agent turn 的生命周期。

```text
user prompt
-> session memory
-> hooks
-> model loop
-> tool calls
-> tool results
-> memory extraction
-> auto-dream / consolidation
-> next turn
```

对于 Pengyi Research OS，这个结构非常重要。

我们的 research agent 不应该只是：

```text
call LLM(prompt)
```

而应该是：

```text
goal tracking
context assembly
permission governance
tool execution
artifact recording
verification
memory update
next-step planning
```

OpenHarness 提供了一个可参考的实现。

## 核心二：run_query Streaming Tool Loop

我读了 `src/openharness/engine/query.py`。

`run_query` 是真正的 tool-aware model loop。

它的主流程是：

```text
while turn_count < max_turns:
  auto-compact if needed
  preprocess images for non-vision models
  stream model response
  append assistant message
  if no tool_uses: stop
  execute tool calls
  append tool results
```

这里有几个工程点很重要。

### Auto-Compaction

每一轮模型调用前都会检查 token 压力。

如果上下文太长，它会先尝试：

```text
microcompact -> 清理旧 tool result 内容
full compact -> LLM summarization
reactive compact -> provider 报 prompt too long 后再压缩重试
```

这件事对 long-running agent 非常关键。

因为真正的 agent session 会持续很多轮：

```text
读文件
跑命令
看错误
改代码
跑测试
再改
写总结
```

如果没有 compaction，agent 很快就会被 tool result 堆爆上下文。

### Streaming

OpenHarness 的 API client 会把 model response streaming 成事件：

```text
ApiTextDeltaEvent
ApiRetryEvent
ApiMessageCompleteEvent
```

再映射成 UI / CLI 的：

```text
AssistantTextDelta
StatusEvent
AssistantTurnComplete
ToolExecutionStarted
ToolExecutionCompleted
ErrorEvent
CompactProgressEvent
```

这意味着上层 UI 不需要知道底层 provider 细节，只要消费统一 stream event。

### Tool Execution

如果只有一个 tool call，就顺序执行并即时返回事件。

如果有多个 tool call，就：

```text
先发出所有 ToolExecutionStarted
asyncio.gather 并发执行
每个 tool result 都回填
即使单个 tool 抛异常，也不让整批 tool call 留下未回复 tool_use
```

这一点非常现实。

很多 agent runtime 会在一个 tool 报错后破坏 Anthropic / OpenAI 的 tool_use / tool_result 对齐，导致下一轮 API 请求失败。

OpenHarness 在这里做了防御。

## 核心三：Tool Registry

我读了 `src/openharness/tools/base.py` 和 `src/openharness/tools/__init__.py`。

工具抽象很标准：

```text
BaseTool
ToolExecutionContext
ToolResult
ToolRegistry
```

每个工具需要：

```text
name
description
input_model
execute()
is_read_only()
to_api_schema()
```

默认 registry 注册了一批核心工具：

```text
bash
ask_user_question
read_file / write_file / edit_file
notebook_edit
lsp
mcp_auth
glob / grep
image_to_text / image_generation
skill / tool_search
web_fetch / web_search
config / brief / sleep
worktree enter/exit
todo_write
plan mode enter/exit
cron create/list/delete/toggle
remote_trigger
task create/get/list/stop/output/update
agent
send_message
team create/delete
MCP resources and MCP tool adapters when MCP manager exists
```

README 把它概括成 `43+ tools`。

更重要的是，工具不是直接裸跑。

每次工具执行都会经过：

```text
input validation
permission check
optional user confirmation
tool execution
large output offload
carryover metadata recording
post-tool hook
```

这就是 harness 的工程价值。

## 核心四：Permissions / Governance

我读了 `src/openharness/permissions/checker.py`。

`PermissionChecker` 支持：

```text
sensitive path protection
denied_tools
allowed_tools
path_rules
denied_commands
permission modes
read-only auto allow
plan mode blocking
default mode confirmation
full_auto mode
```

内置敏感路径保护包括：

```text
.ssh
.aws
gcloud
.azure
.gnupg
.docker/config.json
.kube/config
.openharness/credentials.json
.openharness/copilot_auth.json
```

这点非常关键。

因为 coding agent / research agent 会读文件、跑 shell、接外部工具。

如果没有 permission layer，prompt injection 或误操作会直接变成安全事故。

OpenHarness 这里的工程判断是：

```text
read-only tools can run
mutating tools require confirmation in default mode
plan mode blocks mutations
full_auto only在明确模式下使用
sensitive credential paths always denied
```

对我们自己的 Research OS，这应该是底线设计。

特别是未来如果我们接：

```text
WorldQuant notes
private pitch materials
RA / PhD application materials
GitHub token
broker / trading credentials
bank / contract / legal docs
```

permission layer 不能省。

## 核心五：Hooks

我读了 `src/openharness/hooks/executor.py`。

OpenHarness hook 可以在关键事件上执行：

```text
USER_PROMPT_SUBMIT
PRE_TOOL_USE
POST_TOOL_USE
STOP
NOTIFICATION
```

hook 类型包括：

```text
command hook
http hook
prompt hook
agent hook
```

并且 hook 可以：

```text
根据 matcher 过滤事件
用 $ARGUMENTS 注入 payload
设置 timeout
block_on_failure
用 LLM 做 prompt-like validation
```

这对 governance 很有意义。

例如我们自己的 quant/research OS 可以写：

```text
PreToolUse hook: 禁止读取 private credential
PreToolUse hook: 禁止在 public repo 泄露未脱敏 factor
PostToolUse hook: 自动记录实验 artifact
Stop hook: 自动生成 research memo
Notification hook: 重要任务推送到 Feishu / Slack
```

OpenHarness 把这些都放在 runtime extension 层，而不是硬编码到某个 agent 里。

## 核心六：Skills

我读了 `src/openharness/skills/loader.py`。

OpenHarness skill 采用 `SKILL.md` 布局，并兼容多个目录来源：

```text
bundled skills
~/.openharness/skills
~/.claude/skills
~/.agents/skills
project .openharness/skills
project .agents/skills
project .claude/skills
plugin skills
```

项目级 skills 默认会从当前目录向上找到 git root。

同时它有安全边界：

```text
allow_project_skills
project_skill_dirs must be relative
absolute path / .. path ignored
```

这很适合构建 project-specific operating playbook。

例如我们可以为每个研究项目放：

```text
.openharness/skills/repo-review/SKILL.md
.openharness/skills/factor-diagnosis/SKILL.md
.openharness/skills/research-memo/SKILL.md
.openharness/skills/pr-review/SKILL.md
```

这和我们现在做的 HKUDS / LLMQuant study map 是同一条线。

真正强的 agent 不应该只靠“临时 prompt”，而应该靠：

```text
可版本化的 skills
可项目化的 playbooks
可迁移的 workflow memory
```

## 核心七：Plugins

我读了 `src/openharness/plugins/loader.py`。

OpenHarness plugin 支持类似 Claude Code plugin 的结构。

它可以加载：

```text
plugin.json
skills
commands
agents
hooks
MCP servers
tools
```

并且区分：

```text
user plugins
project plugins
extra roots
enabled_plugins
enabled_by_default
```

这里的一个工程选择值得注意：

```text
project-local plugins disabled by default
```

如果工作区里发现 project-local plugins，但 settings 没开 `allow_project_plugins`，它会 warning。

这很合理。

因为 plugin 比 skill 更危险。

skill 通常只是 prompt / docs，plugin 可能带：

```text
hook
tool
MCP server
command
agent definition
```

所以项目级 plugin 必须显式信任。

对 Pengyi Research OS：

```text
public demo repo 可以启用 project plugin
private sensitive repo 要非常谨慎
```

## 核心八：MCP

我读了 `src/openharness/mcp/client.py`。

`McpClientManager` 支持：

```text
stdio MCP
HTTP / streamable HTTP MCP
connect_all
reconnect_all
list_tools
list_resources
call_tool
read_resource
connection status
auth configured state
```

它不会因为某个 MCP server 连接失败就让整个 runtime 崩掉，而是记录：

```text
pending
connected
failed
detail
transport
tools
resources
```

这在真实项目里很重要。

因为 MCP 生态里 server 质量参差不齐：

```text
有的本地命令不存在
有的 cwd 不存在
有的 HTTP url 错
有的 list_resources 不支持
有的 tool schema 不稳定
```

OpenHarness 对这些情况做了状态化管理。

## 核心九：Memory / Compaction

OpenHarness 的 memory 不只是一份聊天记录。

我读了：

```text
src/openharness/memory/manager.py
src/openharness/services/compact/__init__.py
```

memory manager 管的是项目级 memory files：

```text
MEMORY.md
structured markdown memory entries
frontmatter metadata
dedupe by signature
soft delete
team memory secret checks
file lock
atomic write
```

compaction 则处理 long-running session 的上下文压力：

```text
microcompact old tool results
full LLM summarization
session memory attachments
context collapse
prompt-too-long reactive compact
hook results as compact attachments
```

这对 agent runtime 是刚需。

因为 agent 真正能做事，必然会产生很多中间状态：

```text
read file output
grep output
test output
web result
tool error
task state
worktree path
verified work
active artifacts
```

OpenHarness 的做法是：

```text
短期上下文靠 compaction
中期 session 靠 session memory
长期知识靠 MEMORY.md / memory entries
```

这比“每次对话都从头开始”强很多。

## 核心十：Sandbox

我读了 `src/openharness/sandbox/adapter.py`。

OpenHarness 支持通过 `srt` sandbox-runtime 或 Docker 做隔离。

它会根据 settings 生成：

```text
network allowed / denied domains
filesystem allowRead / denyRead
filesystem allowWrite / denyWrite
```

并检查平台能力：

```text
Linux / WSL needs bwrap
macOS needs sandbox-exec
native Windows not supported for sandbox-runtime
Docker backend can be selected separately
```

这说明 OpenHarness 对“agent 执行命令”这件事是严肃处理的。

对我们未来做 quant / research agent：

```text
public repo demo -> 可以较宽松
private materials -> 强沙箱
trading/broker credentials -> 不允许 agent 直接触碰
remote automation -> 必须有 explicit approval / policy
```

## 核心十一：Provider Workflows

我读了 `src/openharness/config/settings.py` 和 `src/openharness/ui/runtime.py`。

OpenHarness 把 provider 当成 workflow，而不是只保存一个 API key。

内置 profile 包括：

```text
claude-api
claude-subscription
openai-compatible
codex
copilot
moonshot
gemini
minimax
nvidia
qwen
modelscope
```

这对中国开发环境很现实。

因为我们可能会频繁切换：

```text
OpenAI
Claude
Codex subscription
Claude subscription
GitHub Copilot
DashScope Qwen
Moonshot Kimi
DeepSeek / OpenRouter / SiliconFlow
Ollama local model
internal OpenAI-compatible endpoint
```

OpenHarness 的 `oh setup`、`oh provider list`、`oh provider use` 就是在处理这个问题。

对我们来说，这也提示：

```text
Research OS 不应该绑定死一个模型供应商。
Agent runtime 要支持 provider abstraction。
```

## 核心十二：Dry Run

`--dry-run` 是 OpenHarness 很值得学习的设计。

README 里说它会静态预览：

```text
resolved runtime settings
auth state
skills
commands
tools
configured MCP servers
prompt assembly
readiness verdict
next actions
likely matching skills/tools
```

而且不会：

```text
call model
execute tools
spawn subagents
connect MCP servers
```

readiness 分三类：

```text
ready
warning
blocked
```

这对高风险 agent workflow 非常有价值。

我们自己的 R&D Agent 也应该有 dry-run。

例如：

```text
pengyi-quant --dry-run backtest factor_x
```

应该告诉我们：

```text
会用哪些数据
会跑哪些脚本
会写哪些目录
会读取哪些 private files
是否有 API key
是否会触发 broker / trading action
是否需要 human approval
```

这就是工程上真正可控的 agent。

## 核心十三：Tasks / Swarm

OpenHarness 里有两类多任务能力：

```text
tasks
swarm
```

tools 里有：

```text
task_create
task_get
task_list
task_stop
task_output
task_update
agent
send_message
team_create
team_delete
```

`swarm/team_lifecycle.py` 管的是 team 的持久化元数据：

```text
~/.openharness/teams/<name>/team.json
```

一个 team 里有：

```text
members
lead_agent_id
lead_session_id
allowed_paths
team_allowed_paths
worktree_path
permissions
status
subscriptions
```

这说明 OpenHarness 不只是单 agent。

它已经在做：

```text
background local agent
subagent delegation
team lifecycle
mailbox / messages
worktree isolation
permission sync
```

对我们自己的 Research OS，这个方向非常关键。

一个 research task 很少是单线程的：

```text
agent A 读 paper
agent B 读代码
agent C 跑实验
agent D 写 report
agent E 做 review
human PM 最后审核
```

OpenHarness 的 swarm/tasks 是这类系统的底座。

## 核心十四：ohmo Personal Agent

我读了 `ohmo/runtime.py`。

`ohmo` 的做法很干净：

```text
复用 OpenHarness runtime
使用自己的 workspace
使用自己的 session backend
使用自己的 memory backend
include_project_memory=False
使用 ohmo extra skills/plugins roots
可以通过 backend host 支撑 React TUI / gateway
```

这说明 `ohmo` 不是 fork 一份 OpenHarness，而是在同一 runtime 上做 application specialization。

它隔离：

```text
~/.ohmo workspace
ohmo memory
ohmo sessions
ohmo skills
ohmo plugins
```

这对我们设计 personal agent 很有启发。

我们未来也可以有：

```text
Pengyi Daily Agent
Pengyi Research Agent
Pengyi Quant Agent
Pengyi Application Agent
```

它们不需要各自复制 runtime。

正确方式是：

```text
shared harness runtime
+ different workspace
+ different memory
+ different system prompt
+ different skills/plugins
+ different permission policy
```

## 和前面 HKUDS 项目的关系

OpenHarness 和前面项目的关系非常强。

| Project | 和 OpenHarness 的关系 |
|---|---|
| `nanobot` | nanobot 是 personal agent shell，OpenHarness 是更底层 harness runtime |
| `CLI-Anything` | CLI-Anything 偏软件动作层，OpenHarness 把 CLI/action 接进 tool loop 和 permission layer |
| `AgentSpace` | AgentSpace 偏组织级 agent workspace，OpenHarness 偏单机/个人/开发者 runtime |
| `AutoAgent` | AutoAgent 偏 agent factory / workflow generation，OpenHarness 偏执行 harness |
| `DeepCode` | DeepCode 需要 coding tool loop，OpenHarness 可以是 paper-to-code agent 的 runtime 参考 |
| `AI-Researcher` | AI-Researcher 是科学发现 workflow，OpenHarness 是可运行/可治理的 agent substrate |
| `DeepTutor` | DeepTutor 是学习 workspace，OpenHarness 是可接入 subagent/tool 的底层 harness 参考 |
| `DeepResearch-Eval` | OpenHarness 可以跑评测工具、记录结果、做 report-generation loop |

放到 HKUDS0000 四条主线里：

| 主线 | OpenHarness 的位置 |
|---|---|
| Quant / Finance | 可作为 quant research agent 的本地执行 harness |
| Research OS / AI Scientist | 可作为 paper/code/experiment/report agent 的底层 runtime |
| Agent Framework / Workspace | 核心位置，agent harness / tool-use / permission / swarm |
| RAG / Knowledge | 不是专门 RAG，但通过 MCP、memory、skills、web、project files 接知识层 |

一句话：

```text
OpenHarness 不是直接替我们做 research。
它提供“让 research agent 可运行、可扩展、可治理”的基础设施。
```

## 对 Pengyi Research OS 的启发

我们现在越来越清楚：

```text
Research OS = workflow + memory + agent runtime + evaluation + human PM review
```

OpenHarness 对其中的 agent runtime 部分非常有启发。

### 1. 所有 agent 都需要 harness

无论是：

```text
paper reading agent
repo study agent
quant backtest agent
RA application agent
PR contribution agent
research memo writer
```

底层都需要：

```text
tool registry
permission checker
system prompt assembly
memory
compaction
task lifecycle
provider abstraction
streaming event protocol
```

这就是 OpenHarness 的核心价值。

### 2. Prompt 不是产品，runtime 才是产品

很多人做 agent 会停在 prompt。

但真正能 work 的系统需要：

```text
可以跑
可以停
可以确认
可以审计
可以继续
可以切模型
可以管理工具
可以隔离权限
可以记录产物
可以复现 session
```

OpenHarness 的代码就是在处理这些问题。

### 3. Permission 是第一等公民

这点对我们尤其重要。

未来我们会有：

```text
private repo
private notes
pitch materials
CV / PS / RP
quant factor research
possibly broker / account / trading-related files
```

agent 不能默认拥有所有权限。

必须有：

```text
read-only mode
plan mode
default confirmation
full-auto only for trusted sandbox
sensitive path deny
project plugin opt-in
dry-run preview
```

### 4. Personal agent 要和 project agent 分开

`ohmo` 的设计很值得学：

```text
OpenHarness project memory
ohmo personal memory
```

它们不是同一份。

这对我们也很重要。

应该分清：

```text
public website repo memory
private pitch repo memory
quant research memory
career/application memory
daily personal agent memory
```

不要混。

## 对 Quant Research OS 的启发

OpenHarness 可以迁移到 quant research 的方式非常具体。

### Quant Harness Layer

我们可以抽象一个：

```text
Pengyi Quant Harness
```

底层能力包括：

```text
read factor code
read data dictionary
run backtest script
grep historical experiment logs
write research memo
spawn background experiment
ask human PM for approval
dry-run before touching data
record artifact path
run verification policy
```

这和 OpenHarness 的能力对应：

| Quant need | OpenHarness reference |
|---|---|
| run backtest | `bash`, task tools, autopilot verification |
| edit factor code | file tools + permission checker |
| inspect repo | read/grep/glob/LSP |
| use data services | MCP tools / custom tools |
| protect secrets | sensitive paths + path rules |
| long session | memory + auto-compact |
| multiple experiments | tasks / swarm / worktrees |
| PM approval | ask_user_question + plan mode |
| report | skills / plugins / memory |

这不是直接拿来就能做实盘。

但它是我们设计 engineering-grade quant agent 的一个很好的参考。

## 可以快速实践的练习

### Exercise 1: Dry-run 学一个 repo

```bash
oh --dry-run -p "Explain this repository and identify the key modules"
```

看它会解析：

```text
settings
auth
skills
commands
tools
MCP config
readiness
next actions
```

这一步不需要执行模型或工具，很适合先理解 runtime。

### Exercise 2: 用 OpenHarness 做 repo study

```bash
oh -p "Summarize the purpose of this repository and list the files that define the permission system"
```

这对应我们现在做 HKUDS study map 的方式。

### Exercise 3: 写一个 project skill

在项目里建：

```text
.openharness/skills/repo-study/SKILL.md
```

内容规定：

```text
1. 先读 README
2. 再读 package / pyproject
3. 再读核心 runtime
4. 最后输出 project purpose / implementation / key components / PR candidates
```

这可以变成我们的标准 project study workflow。

### Exercise 4: 权限策略演练

设置：

```text
plan mode
default mode
full_auto
denied tools
path rules
denied commands
```

然后观察不同工具调用如何被拦截。

这是 agent safety 的基本功。

### Exercise 5: 背景任务 / subagent

尝试让 agent：

```text
spawn a worker to inspect tests
main agent reads README
worker reports test architecture
main agent writes summary
```

这就是 multi-agent research workflow 的雏形。

## 可以提 PR 的方向

仍然坚持原则：

```text
真实使用
发现真实问题
提出小而清晰的改进
```

OpenHarness 上可能的 PR 方向：

### 1. Research OS Example

补一个 docs showcase：

```text
Using OpenHarness as a research project study harness
```

包括：

```text
repo reading
skill loading
permission mode
dry-run
summary export
memory entry
```

### 2. Quant Research Harness Template

提供一个 public-safe template：

```text
.openharness/skills/quant-research-review/SKILL.md
```

内容是 toy data / public factor demo，不涉及真实私有因子。

### 3. Dry-run 文档增强

`--dry-run` 是很好的功能，可以补更多场景：

```text
dry-run for prompt
dry-run for slash command
dry-run with MCP config
dry-run with plugin enabled
dry-run JSON output for CI
```

### 4. Permission Cookbook

补一篇：

```text
Permission cookbook for project-local agents
```

包括：

```text
read-only repo audit
safe docs editing
blocking secrets
blocking destructive shell commands
when to use plan mode
when not to use full_auto
```

### 5. Windows Notes

README 已经提到 Windows PowerShell 下用 `openh` 而不是 `oh`，因为 `oh` 可能解析成 `Out-Host` alias。

可以补一个更完整的 Windows troubleshooting section：

```text
PowerShell alias
native Windows sandbox limitation
path quoting
terminal TUI behavior
```

### 6. OpenHarness + DeepTutor Bridge Note

我们刚看完 DeepTutor。

可以写一个 issue / doc idea：

```text
How OpenHarness-style local agent runtime can support DeepTutor-style self-training workflows
```

不一定要直接集成，先文档化思想也有价值。

## 和我们网站的连接

`HKUDS015` 在网站学习内容里的位置是：

```text
HKUDS014 -> DeepTutor self-training layer
HKUDS015 -> OpenHarness agent harness runtime layer
```

后面可以接：

```text
HKUDS016 -> UpSkill
HKUDS017 -> AnyTool
HKUDS018 -> MiniRAG
HKUDS019 -> Paper2Slides
HKUDS020 -> OpenSpace
```

其中 `UpSkill` 和 `AnyTool` 会自然接到 OpenHarness：

```text
OpenHarness -> harness runtime
UpSkill     -> skill / capability acquisition
AnyTool     -> tool-use generalization
OpenSpace   -> workspace / collaboration surface
```

## 这篇的总结

`HKUDS015` 的一句话总结：

```text
OpenHarness 是一个围绕 LLM 构建 functional agent 的 lightweight harness runtime：
它把 streaming agent loop、tool registry、permissions、hooks、skills、plugins、MCP、memory、compaction、sandbox、tasks、swarm、provider workflows、TUI 和 ohmo personal agent 接到一起。
```

对 Pengyi Research OS 来说，它的意义是：

```text
我们不只要设计 research workflow。
我们还要理解和掌握 workflow 下面的 agent harness。
```

如果前面几篇是：

```text
DeepTutor -> train the human operator
DeepCode -> translate paper to code
AI-Researcher -> autonomous scientific discovery
DeepResearch-Eval -> evaluate research reports
```

那么 OpenHarness 的位置就是：

```text
OpenHarness -> make the agent actually runnable, governable, extensible, and persistent
```

这就是我们后面做 Pengyi Research OS、Pengyi Quant Research OS、R&D Agent 时必须掌握的底层能力。
