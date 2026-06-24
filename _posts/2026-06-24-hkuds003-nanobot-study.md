---
title: "HKUDS003: nanobot 作为 Personal Agent Shell 与 Always-On Research Workspace"
date: 2026-06-24 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds003, hkuds, nanobot, personal-agent, agent-shell, webui, mcp, memory, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第四篇。

```text
HKUDS003 -> nanobot
```

前两篇核心项目分别是：

```text
HKUDS001 -> LightRAG
HKUDS002 -> Vibe-Trading
```

我的当前定位是：

```text
LightRAG     = Research Memory Layer
Vibe-Trading = Agentic Quant Research Workflow Layer
nanobot      = Personal Agent Shell / Always-On Research Workspace
```

也就是说，`nanobot` 不是一个专门的量化策略项目，也不是一个单纯的 RAG 项目。它更像一个可以长期运行的个人 AI agent runtime：把 CLI、WebUI、聊天软件、模型 provider、工具调用、记忆、定时任务、MCP、workspace、安全边界这些东西放到一个小核心系统里。

这对我们非常关键。因为 `Pengyi Quant Research OS v0` 不能只是一堆 notebook，也不能只是一堆散落的 prompt。我们需要一个常驻的 agent shell：

```text
human intent
  -> agent session
  -> memory / skills / workspace context
  -> model reasoning
  -> tools
  -> files / shell / web / MCP / cron
  -> artifact
  -> report
  -> next task
  -> long-term memory
```

`nanobot` 的价值就在这里：它提供的是一个可以被自己拥有、自己部署、自己改造的 personal agent operating layer。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `nanobot` 快照。

| Item | Value |
|---|---|
| repo | `nanobot` |
| branch | `main` |
| local head | `3ce0cd9` |
| package | `nanobot-ai` |
| version | `0.2.1` |
| license | MIT |
| Python | `>=3.11` |
| status | 有 2 个本地 modified 文件，本文只读不改 |

本地被修改但没有触碰的文件：

```text
nanobot/agent/tools/long_task.py
nanobot/providers/openai_compat_provider.py
```

项目规模：

| Metric | Count |
|---|---:|
| total files | 642 |
| built-in channel modules | 15 |
| built-in tool modules | 14 |
| built-in skills | 12 |
| provider specs | 39+ |

关键目录：

```text
nanobot/
  agent/
    loop.py
    runner.py
    context.py
    memory.py
    skills.py
    tools/
  bus/
    events.py
    queue.py
    runtime_events.py
  channels/
  config/
  cron/
  providers/
  security/
  session/
  skills/
  templates/
webui/
bridge/
docs/
tests/
```

第一眼看上去，`nanobot` 的目录很多。但核心其实很清楚：

```text
message comes in
  -> build context
  -> call model
  -> execute tools
  -> save session / memory
  -> send response out
```

它不是靠复杂框架取胜，而是靠边界清晰的小核心取胜。

## What It Is

README 对 `nanobot` 的定义很直接：

```text
open-source, ultra-lightweight personal AI agent you can truly own
```

我把这句话拆成三个关键词：

| Keyword | Meaning |
|---|---|
| personal | 面向个人长期使用，而不是一次性 demo |
| lightweight | 核心 agent loop 保持小而可读 |
| own | 可以自部署、可检查、可定制、可扩展 |

它解决的问题不是“某一个任务怎么做”，而是“我怎样拥有一个长期在线、可积累上下文、能调用工具、能跨入口工作的个人 agent”。

这和我们正在做的 Research OS 非常贴近。因为研究不是一次 prompt，研究是持续过程：

```text
daily note
paper reading
repo reading
experiment
backtest
debug
meeting prep
email draft
report writing
next plan
```

如果没有一个 persistent agent runtime，这些动作就会散落在不同窗口、不同目录、不同聊天记录里。`nanobot` 想做的就是把它们收束到一个 agent workspace 里。

## Core Mental Model

`docs/architecture.md` 里的核心链路可以概括为：

```text
Channel
  -> MessageBus
  -> AgentLoop
  -> ContextBuilder
  -> AgentRunner
  -> Provider
  -> ToolRegistry
  -> Tools
  -> Session / Memory / Workspace
  -> OutboundMessage
  -> Channel
```

把它翻译成研究工作流就是：

```text
入口：CLI / WebUI / Telegram / Slack / Email
协调：AgentLoop 选择 session，准备上下文
推理：AgentRunner 与模型交互
行动：ToolRegistry 执行文件、shell、web、MCP、cron 等工具
沉淀：SessionManager 和 MemoryStore 保存历史与长期记忆
返回：通过原 channel 回到用户
```

这套架构非常适合做 personal research cockpit。用户不需要每次重新解释自己是谁、当前项目是什么、上次做到了哪里。agent 可以围绕 workspace 和 session 继续工作。

## AgentLoop vs AgentRunner

`nanobot` 一个很好的工程切分是把 `AgentLoop` 和 `AgentRunner` 分开。

| Component | File | Responsibility |
|---|---|---|
| `AgentLoop` | `nanobot/agent/loop.py` | 面向产品层和 session 层的 turn orchestration |
| `AgentRunner` | `nanobot/agent/runner.py` | 面向模型和工具调用的 execution loop |

`AgentLoop` 管的是一轮用户请求的生命周期：

```text
RESTORE
  -> COMPACT
  -> COMMAND
  -> BUILD
  -> RUN
  -> SAVE
  -> RESPOND
  -> DONE
```

这很重要，因为 agent 系统一旦开始长期运行，就不能只想“调用一次模型”。它必须处理：

```text
session restore
history compaction
commands
context build
tool registry
runtime progress
injections
session save
outbound delivery
```

`AgentRunner` 则更接近我们熟悉的 tool-calling loop：

```text
messages
  -> provider call
  -> assistant response
  -> tool calls
  -> tool results
  -> new provider call
  -> final response
```

这个拆分的好处是，`AgentLoop` 不被 provider 细节污染，`AgentRunner` 也不需要知道 Telegram、WebUI、session title、heartbeat 这些产品层问题。

这就是一个成熟 agent runtime 应该有的结构。

## Message Bus

`nanobot/bus/queue.py` 里的 `MessageBus` 很小，只做两件事：

```text
inbound queue
outbound queue
```

channel 把外部消息转成 `InboundMessage`，agent 处理完以后再把 `OutboundMessage` 发回 channel。

`InboundMessage` 的关键字段：

```text
channel
sender_id
chat_id
content
timestamp
media
metadata
session_key_override
```

`OutboundMessage` 的关键字段：

```text
channel
chat_id
content
reply_to
media
metadata
buttons
```

这说明 `nanobot` 的 channel 层和 agent 层是解耦的。Telegram、Discord、WebUI、Email 的差异不会直接进入 agent core。它们都先统一成 message event。

对我们来说，这个设计可以复用到 Research OS：

```text
WebUI question
Telegram reminder
CLI deep work
Email draft
scheduled research task
```

这些都可以进入同一个 message bus，最终落在同一个 workspace 和 session 管理体系里。

## Channels

`nanobot/channels/` 里有很多入口：

```text
dingtalk
discord
email
feishu
matrix
mochat
msteams
napcat
qq
signal
slack
telegram
websocket
wecom
weixin
whatsapp
```

`BaseChannel` 的抽象也很明确：

```text
start()
stop()
send()
send_delta()
send_reasoning_delta()
send_file_edit_events()
_handle_message()
```

每个 channel 要做的事包括：

```text
connect to platform
receive message
check allowlist / pairing
normalize inbound message
publish to bus
send outbound response
optional streaming / reasoning / file edit events
```

这让我想到一个实际使用方式：我们可以把 `nanobot` 作为自己的 always-on research assistant，不一定一开始就接所有聊天软件。最小可行入口是：

```text
CLI
WebUI
Telegram or Slack
```

这样就能覆盖三种场景：

| Scenario | Entry |
|---|---|
| 本地深度 coding | CLI |
| 可视化工作台 | WebUI |
| 移动端 reminder / quick note | Telegram or Slack |

## WebUI

`nanobot` 的 v0.2.1 被 README 称作 Workbench Release。WebUI 已经打包进 wheel，不需要单独 build 才能使用。

基本启动路径是：

```text
enable websocket channel
run nanobot gateway
open http://127.0.0.1:8765
```

其中 gateway 不是单纯 Web server。它会启动：

```text
enabled channels
websocket channel
workspace-scoped cron service
Dream / heartbeat jobs
health endpoint
```

这里的关键不是“有个网页聊天框”，而是 WebUI 成为 agent workbench。它可以展示：

```text
session
model / context controls
reasoning timeline
tool progress
file edit activity
project workspace
long-running goals
```

这正是我们需要的 Research OS 前端：研究不只是问答，而是一个有 artifact、有过程、有上下文、有状态的工作台。

## Tools

`nanobot` 的工具系统由几层组成：

| Component | File | Role |
|---|---|---|
| `Tool` | `agent/tools/base.py` | 所有工具的抽象基类 |
| `Schema` | `agent/tools/base.py`, `agent/tools/schema.py` | 参数 schema 和校验 |
| `ToolRegistry` | `agent/tools/registry.py` | 注册、排序、参数 cast、执行 |
| `ToolLoader` | `agent/tools/loader.py` | 扫描内置工具和 entry point 插件 |
| `mcp.py` | `agent/tools/mcp.py` | 把 MCP server 工具包装成 nanobot tool |

内置工具模块包括：

```text
apply_patch
cli_apps
cron
exec_session
filesystem
image_generation
long_task
message
path_utils
search
self
shell
spawn
web
```

这套系统有几个值得学习的点。

第一，工具不是随便塞进 prompt 的字符串，而是有 schema 的。

```text
tool.to_schema()
cast_params()
validate_params()
execute()
```

第二，tool registry 会做稳定排序，把 built-in tools 和 MCP tools 区分开。这对 prompt cache 和模型稳定性都有意义。

第三，MCP 工具会被包装成原生工具名：

```text
mcp_{server_name}_{tool_name}
```

并且会 sanitize 成模型 provider 可接受的名字。

这对我们的启发很直接：后面我们不应该把 LightRAG、Vibe-Trading、LLMQuant 硬塞进一个大脚本，而应该把它们作为 tool / MCP capability 暴露给 personal agent shell。

```text
nanobot shell
  -> mcp_lightrag_query
  -> mcp_vibetrading_backtest
  -> mcp_llmquant_factor_search
  -> local file / shell / report tools
```

## Providers

`nanobot/providers/registry.py` 是 provider metadata 的 single source of truth。

本地快照里可以看到大量 provider spec，包括：

```text
custom
azure_openai
bedrock
openrouter
huggingface
skywork
aihubmix
siliconflow
novita
volcengine
byteplus
anthropic
openai
openai_codex
github_copilot
deepseek
gemini
zhipu
dashscope
moonshot
minimax
mistral
stepfun
vllm
ollama
lm_studio
groq
qianfan
```

provider 设计里有几个关键概念：

| Concept | Meaning |
|---|---|
| `ProviderSpec` | provider 的名称、关键词、API key、backend、gateway/local/OAuth 信息 |
| `modelPresets` | 把 model、provider、temperature、context window 等打包成 preset |
| `FallbackProvider` | 主模型失败时可以切换 fallback model |
| `openai_compat` | 大量 provider 走 OpenAI-compatible backend |
| local providers | vLLM、Ollama、LM Studio 等本地模型入口 |

`providers/factory.py` 负责从 config 生成真实 provider。核心流程是：

```text
resolve model preset
  -> infer provider name
  -> load provider config
  -> choose backend
  -> instantiate provider
  -> attach generation settings
  -> wrap fallback provider if configured
```

这说明 `nanobot` 是 model-agnostic 的。它不是绑定某一个模型 API，而是把模型当成可替换的 runtime dependency。

这点对我们尤其重要。Research OS 要长期演进，不能押注单一 provider。未来可能是：

```text
fast model for routing
strong model for reasoning
local model for private notes
code model for development
vision model for screenshots
fallback model for reliability
```

`nanobot` 的 provider layer 已经给了这种路由基础。

## Session And Memory

`nanobot/session/manager.py` 负责 session history。每个 session 由 key 标识，通常是：

```text
channel:chat_id
```

session 里保存：

```text
messages
created_at
updated_at
metadata
last_consolidated
```

它还有一些细节处理，比如：

```text
avoid starting history mid-turn
drop orphan tool results
sanitize assistant replay artifacts
recover image breadcrumbs
attach CLI app / MCP preset breadcrumbs
trim by max messages and token budget
```

这说明 session 不是简单 append。它是为“把历史安全地重新喂给模型”服务的。

长期记忆在 `nanobot/agent/memory.py`。核心文件包括：

```text
workspace/memory/MEMORY.md
workspace/memory/history.jsonl
workspace/SOUL.md
workspace/USER.md
workspace/memory/.cursor
workspace/memory/.dream_cursor
```

其中：

| File | Role |
|---|---|
| `MEMORY.md` | 长期事实和稳定偏好 |
| `history.jsonl` | append-only 历史记录 |
| `SOUL.md` | agent identity / behavior |
| `USER.md` | user profile / preference |
| `.dream_cursor` | Dream consolidation 进度 |

`Dream` 可以理解为一个周期性的 memory consolidation 过程：把 session/history 里有价值的信息压缩进长期记忆。

这对我们有直接启发。我们的 Research OS 也需要区分：

```text
raw session
experiment log
research note
stable memory
public-safe summary
private planning memory
```

不能把所有东西都混在一个聊天记录里。

## Goals, Cron, Heartbeat

`nanobot` 的 v0.2.0 加入了 `/goal` 方向的 sustained objectives。代码里对应 `long_task` 和 `complete_goal` 工具。

`long_task` 的逻辑不是开一个神秘的新 orchestrator，而是把目标记录到当前 session metadata，然后每一轮通过 runtime context 暴露出来。

```text
session.metadata["goal_state"] = {
  "status": "active",
  "objective": "...",
  "ui_summary": "...",
  "started_at": "..."
}
```

完成后再写成：

```text
"status": "completed"
"completed_at": "..."
"recap": "..."
```

这是一种很朴素但有效的长期任务设计：目标状态必须保存在 session，而不是只靠模型短期上下文记住。

`cron/service.py` 负责定时任务。它支持：

```text
at
every
cron expression
timezone
agent_turn payload
run history
corrupt file protection
```

研究场景里，这可以做很多事：

```text
每天早上生成 research plan
每晚整理当天 notes
每周总结开源进展
定时检查实验结果
定时提醒联系导师 / quant mentor
定时把网页资料转成 structured notes
```

如果把 `long_task + cron + memory + tools` 合起来，就可以得到一个最小版 always-on research manager。

## Skills

本地内置 skills 包括：

```text
clawhub
cron
github
image-generation
long-goal
memory
my
skill-creator
summarize
tmux
update-setup
weather
```

我理解 skill 层的作用是：把 agent 的某类稳定工作方式写成可加载的 instruction package，而不是每次都让用户重复解释。

这对我们也很有用。我们以后可以设计自己的 Research OS skills：

```text
pengyi-factor-research
pengyi-paper-reading
pengyi-quant-backtest-review
pengyi-ra-email
pengyi-phd-pitch
pengyi-public-blog-writing
pengyi-private-strategy-sanitization
```

一个好的 skill 不只是 prompt，它应该规定：

```text
when to use
what files to read
what output format to produce
what checks to run
what should never be exposed publicly
```

`nanobot` 的 skill 系统给了我们一个可参考的组织方式。

## Security Boundaries

长期运行 agent 最危险的地方是权限。`nanobot` 在这方面已经有几类边界：

| Boundary | Files / Areas |
|---|---|
| workspace access | `security/workspace_access.py`, `security/workspace_policy.py` |
| shell sandbox | `agent/tools/shell.py`, `agent/tools/sandbox.py` |
| network safety | `security/network.py`, `agent/tools/web.py`, `agent/tools/mcp.py` |
| channel access | `channels/base.py`, allowlist, pairing |
| session scope | `session/keys.py`, unified session config |

这些边界非常重要。因为 personal agent 一旦接了 chat apps、shell、filesystem、web、MCP，就不再是一个 harmless chatbot。

对我们的 Research OS，至少要有三条基本规则：

```text
private workspace and public workspace must be separated
strategy / factor / pitch materials must have public-safe export path
shell and file tools must default to workspace scope
```

如果以后把 quant data、导师沟通材料、职业规划、private notes 放进系统里，权限和脱敏就是基础设施，不是最后再补的功能。

## Comparison With LightRAG And Vibe-Trading

现在 HKUDS 第一阶段三个项目可以拼起来看。

| Project | Core Role | What It Gives Us |
|---|---|---|
| `LightRAG` | Research Memory Layer | 把论文、PDF、文档、notes 转成可检索、可引用、带 graph structure 的知识记忆 |
| `Vibe-Trading` | Agentic Quant Research Workflow Layer | 把自然语言 finance question 转成数据加载、策略实现、回测、报告、研究证据 |
| `nanobot` | Personal Agent Shell | 把模型、工具、记忆、WebUI、chat channels、cron、MCP、workspace 串成常驻个人 agent |

三者的关系可以画成：

```text
              Human / PM
                  |
          CLI / WebUI / Chat
                  |
              nanobot
          Personal Agent Shell
                  |
      +-----------+------------+
      |                        |
   LightRAG              Vibe-Trading
 Research Memory       Quant Workflow
      |                        |
 papers / docs          data / factors / backtests
 notes / PDFs           reports / diagnostics / artifacts
```

如果说 `LightRAG` 是大脑里的知识库，`Vibe-Trading` 是研究动作流水线，那么 `nanobot` 就是把人、模型、工具和工作空间连接起来的操作界面。

## Why It Matters For Pengyi Research OS

我们的目标不是“看懂一个项目”。真正的目标是把这些开源项目融会贯通，形成自己的系统。

我现在对 `Pengyi Quant Research OS v0` 的系统分层是：

```text
1. Personal Agent Shell
   nanobot

2. Research Memory
   LightRAG / notes / markdown / PDFs / project docs

3. Quant Workflow
   Vibe-Trading / LLMQuant / backtest / factor research

4. Public Artifact Layer
   website / reports / sanitized demos / GitHub repos

5. Private Career & Research Workspace
   RA / PhD / mentor talks / pitch / private plans
```

`nanobot` 最适合作为第一层。它不需要一开始懂所有量化细节，它只需要成为统一入口：

```text
I want to study this repo
I want to turn this paper into factor hypotheses
I want to backtest this idea
I want to generate a public-safe report
I want to remember this mentor advice
I want to schedule next follow-up
```

这些请求都可以进入同一个 personal agent shell，然后由 shell 调用不同能力层。

## Minimal Pengyi Demo Plan

如果我们要基于 `nanobot` 做一个自己的 demo，我建议从最小闭环开始。

第一步，创建 private workspace：

```text
pengyi-agent-workspace/
  SOUL.md
  USER.md
  HEARTBEAT.md
  memory/
  projects/
  public_exports/
  private_notes/
```

第二步，配置 provider preset：

```text
fast-router
strong-reasoner
code-agent
local-private
fallback
```

第三步，只启用必要入口：

```text
CLI
WebUI websocket
one chat channel
```

第四步，接入最小 tools：

```text
filesystem
shell
web search / fetch
cron
long_task
MCP placeholder
```

第五步，做一个完整任务：

```text
read one quant paper
extract hypotheses
search related notes
write implementation plan
create toy backtest scaffold
generate report
save memory
schedule follow-up
export public-safe blog post
```

这个 demo 比单纯“我做了一个 chatbot”更有说服力。因为它体现的是 research loop，而不是 chat loop。

## Possible PR Opportunities

如果我们之后要给 `nanobot` 提 PR，应该从真实使用中出发，不要为了 PR 而 PR。当前可以关注这些方向：

| Direction | Possible Contribution |
|---|---|
| Windows usage | Windows / PowerShell setup、MCP、path handling 的文档补充 |
| Research OS example | 一个 minimal research workspace template |
| LightRAG connector | nanobot + LightRAG 的 MCP / skill demo |
| Vibe-Trading connector | nanobot 调 Vibe-Trading research task 的 example |
| Chinese guide | 中文 quickstart / WebUI usage / chat app guide |
| Long-goal tutorial | sustained research task 的完整案例 |
| Security docs | private/public workspace、allowlist、tool permission 的实践说明 |

对我们来说，最好的 PR 路径是：

```text
use it
hit real issue
write minimal reproduction
open issue
propose patch or docs improvement
submit PR
```

这样比较自然，也更容易被维护者接受。

## What It Is Not

也要讲清楚边界。

`nanobot` 不是：

```text
full quant trading system
factor library
market data platform
portfolio optimizer
production trading engine
paper reproduction framework
```

它是：

```text
personal agent runtime
multi-channel interface
tool-calling shell
persistent workspace
memory and session manager
automation layer
extension surface
```

所以它不会替代 `Vibe-Trading` 或 `LLMQuant`，而是给这些项目提供一个统一的人机交互入口和长期运行环境。

## My Current Conclusion

`nanobot` 对我最大的启发是：一个强大的 AI scientist system 不一定从大而全的平台开始。它可以从一个小而清楚的 personal agent loop 开始：

```text
message
  -> context
  -> model
  -> tools
  -> memory
  -> artifact
```

然后逐渐接入：

```text
LightRAG
Vibe-Trading
LLMQuant
personal website
private pitch repo
RA / PhD application materials
mentor conversation notes
daily research plans
```

这就是 `nanobot` 在 HKUDS study map 里的位置：

```text
nanobot = Pengyi Research OS 的 personal always-on agent shell
```

下一步我们可以做 `HKUDS004`：把 `LightRAG + Vibe-Trading + nanobot + LLMQuant` 放到一张系统图里，设计第一版 `Pengyi Research OS v0` 的真实工程架构。
