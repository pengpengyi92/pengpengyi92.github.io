---
title: "HKUDS017: AnyTool 作为 Universal Tool-Use Layer 与 Capability Routing Layer"
date: 2026-06-26 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds017, hkuds, anytool, tool-use, mcp, smart-tool-rag, gui-automation, shell-agent, tool-quality, capability-routing, agent-runtime, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第十八篇。

```text
HKUDS017 -> AnyTool
```

上一篇 `HKUDS016` 看的是 `UpSkill`：

```text
UpSkill = Failure-to-Skill Distillation + Agent Self-Improvement Layer
```

`UpSkill` 解决的是 agent 失败以后如何沉淀成 skill，下一次不再重复犯错。

这一篇的 `AnyTool` 接到的是另一个关键问题：

```text
AnyTool = Universal Tool-Use Layer + Capability Routing Layer
```

如果说 `OpenHarness` 是 agent 的运行时，`UpSkill` 是 agent 的经验沉淀层，那么 `AnyTool` 更像是 agent 的手和工具路由器。

```text
agent 想做事
-> 先判断需要什么能力
-> 从 MCP / shell / GUI / web / system 里面检索合适工具
-> 控制上下文里暴露的工具数量
-> 执行工具
-> 记录质量
-> 下一次更聪明地选工具
```

这个 repo 对我们非常关键。因为我们一直在搭 `Pengyi Research OS` 和未来的 `Pengyi Quant Research OS`，真正要做成系统，不能只停留在“会聊天”。它必须能稳定调用数据源、跑脚本、查资料、开 backtest、整理报告、甚至在没有 API 的时候操作 GUI。

`AnyTool` 给出的答案是：

```text
不要把工具调用散落在每个 agent 里面。
把工具调用独立成一层统一的 Tool-Use Layer。
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `AnyTool`。

| Item | Value |
|---|---|
| repo | `AnyTool` |
| remote | `https://github.com/HKUDS/AnyTool.git` |
| branch | `main` |
| local head | `506430f` |
| full commit | `506430fec13300853b2010e2604ae0c71b940502` |
| latest local commit date | `2026-02-28 12:39:42 +0800` |
| latest local commit | `feat: add local execution mode for shell/gui backends (no server required)` |
| status | clean, synced with `origin/main` after fetch |
| local tags | none |
| license | `MIT` |
| package name | `anytool` |
| package version | `0.1.0` |
| Python requirement | `>=3.10` |
| tracked files by `rg --files` | 132 |
| Python files | 117 |
| backend files | 39 under `anytool/grounding/backends` |
| CLI entry | `anytool = anytool.__main__:run_main` |
| server entry | `anytool-server = anytool.local_server.main:main` |
| syntax check | `py -m compileall -q anytool` failed at `anytool/utils/ui.py:427` |

一句话先行：

```text
AnyTool 把 agent 的工具使用从“把所有工具塞进上下文”改成“先检索、再路由、再执行、再记录质量”的工具能力层。
它把 MCP 生态、shell 执行、GUI 自动化、deep research 和系统查询统一包装到 GroundingAgent 下面，让 agent 可以面向任务动态选择最小够用的工具集合。
```

## 它解决什么问题

README 里把问题说得很直接。今天的 agent 自动化主要卡在三个点：

| Problem | Meaning | 对 agent 的影响 |
|---|---|---|
| overwhelming tool contexts | 工具太多，全部塞给 LLM 会污染上下文 | 模型不知道该用哪个，成本和错误率都上升 |
| unreliable community tools | MCP / third-party tools 质量参差不齐 | 工具会失败，描述不清，返回不稳定 |
| limited capability coverage | 很多工具只覆盖 web API | 真实任务还需要 shell、GUI、文件、系统、研究检索 |

所以 `AnyTool` 的核心不是再写一个 agent，而是写一层：

```text
Universal Tool-Use Layer
```

它的 API 也故意做得非常薄：

```python
async with AnyTool() as tool_layer:
    result = await tool_layer.execute("your task")
```

这背后的设计目标是：

```text
上层 agent 不需要知道到底有多少 MCP server。
上层 agent 不需要直接管理 shell / GUI / web tool。
上层 agent 只需要交给 AnyTool 一个任务。
AnyTool 负责找到合适工具、执行、记录结果。
```

## 总体架构

我把它拆成一个主链路：

```text
AnyTool
-> GroundingAgent
-> GroundingClient
-> Provider
-> Session
-> BaseTool
-> ToolResult
```

对应到代码：

| Layer | File | Role |
|---|---|---|
| public API | `anytool/tool_layer.py` | 对外暴露 `AnyTool.execute()` |
| agent loop | `anytool/agents/grounding_agent.py` | 负责规划、调用工具、迭代、判断完成 |
| tool routing | `anytool/grounding/core/grounding_client.py` | 统一管理 provider、session、tool cache、search |
| tool search | `anytool/grounding/core/search_tools.py` | Smart Tool RAG，做工具检索和排序 |
| quality memory | `anytool/grounding/core/quality/manager.py` | 记录工具成功率、延迟、失败惩罚、描述质量 |
| tool abstraction | `anytool/grounding/core/tool/base.py` | 所有工具的 schema、参数验证、执行、记录入口 |
| LLM wrapper | `anytool/llm/client.py` | LiteLLM 调用、tool schema 清洗、tool call 执行 |
| backends | `anytool/grounding/backends/*` | MCP / shell / GUI / web |
| system backend | `anytool/grounding/core/system/*` | 查询 provider 和工具状态的 meta tools |
| recording | `anytool/recording/*` | 记录 trajectory、tool execution、conversation |
| local server | `anytool/local_server/*` | HTTP server mode 下控制桌面和系统能力 |

这是一种很清楚的工程分层：

```text
上层只表达任务。
中层做工具选择。
底层做具体能力执行。
横向记录质量和轨迹。
```

## AnyToolConfig

`AnyToolConfig` 在 `tool_layer.py` 里，是整个系统的入口配置。

它管理几类东西：

| Config Area | 作用 |
|---|---|
| LLM config | 主模型、thinking、timeout、retry、rate limit |
| retrieval model | 工具检索可以单独使用模型 |
| visual model | GUI / screenshot 分析可以单独使用模型 |
| grounding config | 加载 `config_grounding.json` 等配置 |
| backend scope | 控制 agent 能访问哪些 backend |
| workspace | 每个 task 的工作目录 |
| recording | 是否记录 trajectory、screenshot、conversation |
| logging | 日志等级 |

默认模型是：

```text
openrouter/anthropic/claude-sonnet-4.5
```

这说明 AnyTool 假设自己是一个高能力 tool-use runtime，需要一个能稳定 function calling 和多步推理的模型。

`AnyTool.execute()` 的执行链路大致是：

```text
create task_id
prepare context
create workspace directory
start recording
GroundingAgent.process(context)
attach task_id and execution_time
stop recording
maybe evolve tool quality
return result
```

这里一个重要设计是：每次执行都可以绑定 workspace。对我们的 Research OS 很重要，因为研究任务一定要有产物目录：

```text
inputs/
scripts/
logs/
results/
reports/
metadata.json
```

## GroundingAgent

`GroundingAgent` 是真正的执行循环。

它做的事情可以拆成五步：

```text
1. 根据 instruction 检索可用工具
2. 把任务、workspace、tool schema 组成消息
3. 调用 LLM
4. 执行 LLM 产生的 tool calls
5. 判断是否完成，否则进入下一轮
```

关键文件：

```text
anytool/agents/grounding_agent.py
```

它默认最大迭代数来自 `config_agents.json`：

```json
{
  "name": "GroundingAgent",
  "backend_scope": ["gui", "shell", "mcp", "system", "web"],
  "max_iterations": 15,
  "visual_analysis_timeout": 60.0
}
```

这说明它不是一个“只调用一次工具”的 wrapper，而是一个 multi-step agent loop。

每轮它会：

```text
messages -> LLM -> tool_calls -> tool_results -> new messages -> next iteration
```

并且有两个现实工程考虑：

| Mechanism | Meaning |
|---|---|
| message truncation | 超过 token 预算以后保留 system、原始 user instruction 和最近上下文 |
| recording | 把 retrieved tools、iteration context、tool executions 写入 trajectory |

这对长任务很关键。agent 做真实工作一定会产生很长的 tool output。如果没有 truncation 和 recording，系统很快会变成不可控的聊天记录。

## GroundingClient

`GroundingClient` 是工具层的中枢。

它负责：

```text
provider registry
session lifecycle
tool listing
tool cache
tool search
tool invocation
tool quality manager
```

从系统角度看，它就是 `AnyTool` 的 tool kernel。

它会动态加载配置里的 provider：

```json
"enabled_backends": [
  {"name": "shell", "provider_cls": "anytool.grounding.backends.shell.ShellProvider"},
  {"name": "web", "provider_cls": "anytool.grounding.backends.web.WebProvider"},
  {"name": "mcp", "provider_cls": "anytool.grounding.backends.mcp.MCPProvider"},
  {"name": "gui", "provider_cls": "anytool.grounding.backends.gui.GUIProvider"}
]
```

`system` backend 不写在这里，因为它是特殊 backend，会自动注册：

```text
system backend = meta-level query tools
```

它提供：

| System Tool | 用途 |
|---|---|
| `list_providers` | 列出已经注册的 backend provider |
| `list_backend_tools` | 列出某个 backend 的工具 |
| `list_session_tools` | 列出某个 session 的动态工具 |
| `list_all_backend_tools` | 列出所有 backend 的工具 |

这其实是 agent 自省能力。agent 不只会用工具，还能先问：

```text
我现在有哪些 provider？
这个 session 里有什么工具？
所有 backend 能提供什么能力？
```

## Smart Tool RAG

`AnyTool` 最重要的模块是 `Smart Tool RAG`。

代码在：

```text
anytool/grounding/core/search_tools.py
```

它解决的是工具上下文爆炸：

```text
MCP server 越接越多，tool schema 越来越多。
如果全部塞进 LLM，上下文会污染，成本会上升，模型会选错工具。
```

`AnyTool` 的策略不是让 LLM 硬看全部工具，而是做工具检索。

主流程：

```text
candidate tools
-> split MCP and non-MCP tools
-> non-MCP tools always keep
-> MCP tools too多时进入 search/ranking
-> optional LLM pre-filter
-> keyword / semantic / hybrid rank
-> quality-aware reranking
-> final selected tools
```

这里一个非常关键的设计是：

```text
non-MCP tools always included
```

也就是说：

```text
shell_agent
gui_agent
deep_research_agent
system meta tools
```

这些基础能力会被保留。MCP 工具太多时才需要筛。

这对真实 agent 很合理。因为 `shell`、`GUI`、`web research` 是基础操作能力，不能因为 MCP 工具太多被随机过滤掉。

### ToolRanker

`ToolRanker` 支持三种搜索：

| Mode | Meaning |
|---|---|
| `keyword` | BM25 或 term overlap |
| `semantic` | embedding cosine similarity |
| `hybrid` | keyword prefilter + semantic rerank |

默认 embedding model：

```text
BAAI/bge-small-en-v1.5
```

它支持两种 embedding 方式：

```text
remote API via EMBEDDING_BASE_URL / EMBEDDING_API_KEY
local fastembed
```

并且有持久化 embedding cache：

```text
.anytool/embedding_cache
```

这就是 README 里说的 `Zero-Waste Processing`：工具 embedding 不应该每次重复算。

### LLM pre-filter

当 MCP 工具数量超过阈值时，`SearchCoordinator` 会启用 LLM 预筛选。

配置里：

```json
"tool_search": {
  "max_tools": 40,
  "search_mode": "hybrid",
  "enable_llm_filter": true,
  "llm_filter_threshold": 50,
  "enable_cache_persistence": true
}
```

LLM pre-filter 会把工具按 server 分组，然后让模型输出：

```text
brief_plan
utility_tools
domain_servers
```

这很聪明。因为真实 MCP 工具不是孤立的，很多工具天然按 server 成组：

```text
github server
filesystem server
database server
browser server
finance data server
```

所以检索不是只看单个 tool，还要看 server 的任务域。

## ToolQualityManager

`AnyTool` 的另一个关键点是质量感知。

代码在：

```text
anytool/grounding/core/quality/
```

它记录每个 tool 的：

```text
backend
server
tool_name
total_calls
success_count
average_latency
recent_executions
description_quality
```

tool key 形式是：

```text
backend:server:tool_name
```

质量 ranking 的核心逻辑是：

```text
semantic score * quality penalty
```

如果一个工具调用次数少于 3 次，不惩罚。调用足够多以后，如果成功率低于阈值，就降低 ranking。连续失败还会进一步惩罚。

这对真实工具生态很重要。MCP 工具不是论文里干净的 benchmark。现实里很多工具：

```text
描述不清
参数 schema 不准
server 不稳定
API key 缺失
返回格式乱
```

如果工具层没有质量记忆，agent 每次都会重新踩坑。

AnyTool 的启发是：

```text
tool selection 不应该只看语义相似度。
还要看历史成功率、延迟、错误类型、描述质量。
```

这对我们的 Quant OS 可以直接迁移：

| Quant Tool | 质量指标 |
|---|---|
| data loader | 缺失率、延迟、字段稳定性 |
| factor backtest | 是否复现、是否超时、结果是否异常 |
| broker API | order success、reject reason、latency |
| news parser | source reliability、dedup rate、summary quality |
| report generator | 是否生成完整 artifact |

## Backends

AnyTool 默认的 backend scope 是：

```text
["gui", "shell", "mcp", "system", "web"]
```

这五个 backend 对应五种能力。

| Backend | Tool | Meaning |
|---|---|---|
| `mcp` | dynamic MCP tools | 接入外部工具生态 |
| `shell` | `shell_agent` | 在本机或 server 执行 terminal task |
| `gui` | `gui_agent` | 截图、理解 UI、点击、键盘、拖拽 |
| `web` | `deep_research_agent` | 使用 Perplexity sonar deep research 做知识检索 |
| `system` | meta tools | 查询 provider/session/tool 状态 |

### MCP backend

MCP backend 的目标是接外部工具生态。

它支持：

```text
stdio
HTTP
SSE
websocket
tool cache
server lazy initialization
dependency check
auto install
```

`refresh-cache` subcommand 会逐个启动 MCP server，读取工具 schema，再保存 cache：

```bash
anytool refresh-cache
```

这解决一个现实问题：

```text
MCP server 启动慢。
如果每次 agent 开始任务都启动所有 server，会非常浪费。
```

所以 AnyTool 的方法是：

```text
先缓存工具 metadata。
真正调用时再按需初始化 server/session。
```

### Shell backend

Shell backend 提供的是：

```text
shell_agent
```

它内部还有两个 helper：

```text
_python_exec
_bash_exec
```

但暴露给外部的主要是一个 LLM-powered shell agent。

它的执行方式不是简单 `run_command(command)`，而是：

```text
task
-> internal LLM writes exactly one code block
-> execute python or bash
-> read output
-> fix errors
-> retry up to max_steps
-> return ToolResult
```

prompt 里有一个很实用的约束：

```text
复杂 JSON、多行文本、引号转义，优先用 Python，不要用复杂 bash 字符串拼接。
```

这是工程经验。很多 agent 失败不是因为不会写算法，而是因为 shell quoting 失败。

### GUI backend

GUI backend 提供：

```text
gui_agent
```

执行循环是：

```text
Observe screenshot
-> Plan next action with VLM
-> Execute action
-> Verify state
-> Repeat
```

它支持的行为来自 action space，比如点击、输入、键盘、等待、完成、失败等。

这个 backend 的价值是：

```text
当没有 API，或者 API 不稳定，或者任务天然发生在桌面软件里时，agent 还能继续做事。
```

对 Quant OS 来说，它不是第一优先级，但很有兜底意义：

```text
券商客户端没有 API
内部系统只有网页
数据终端只能 GUI 操作
某些上传/下载流程没有稳定接口
```

### Web backend

Web backend 暴露：

```text
deep_research_agent
```

它使用：

```text
OPENROUTER_API_KEY
perplexity/sonar-deep-research
```

流程是：

```text
query
-> sonar-deep-research
-> get long answer
-> LLMClient distill 400-600 word knowledge-dense summary
-> return to agent
```

这不是 browser automation，而是知识检索工具。它适合：

```text
查技术背景
比较方案
理解新项目
研究最新发展
整理专业信息
```

### System backend

System backend 不接外部服务，也不创建 session。

它是自省工具层：

```text
list_providers
list_backend_tools
list_session_tools
list_all_backend_tools
```

这个设计很适合 agent runtime。因为 agent 经常需要先知道自己有什么能力，再决定怎么行动。

## Local Mode 与 Server Mode

AnyTool 最新本地 commit 的标题就是：

```text
feat: add local execution mode for shell/gui backends (no server required)
```

这是一个很重要的工程演进。

Shell / GUI 有两种模式：

| Mode | Meaning | 适用场景 |
|---|---|---|
| local mode | 直接在当前进程里通过 subprocess / pyautogui 执行 | 控制自己的 laptop / desktop |
| server mode | 通过 HTTP 调用 `local_server` Flask 服务 | 远程 VM、隔离环境、多机器、桌面控制服务 |

默认配置是 local mode：

```json
"shell": {"mode": "local"}
"gui": {"mode": "local"}
```

这会降低普通用户的上手成本。先不需要启动 server，就可以使用 shell / GUI。

server mode 仍然保留，因为它适合更严肃的执行环境：

```text
remote workstation
VM sandbox
multi-machine automation
isolated desktop
recording and permission boundary
```

`local_server` 本身是一个跨平台 Flask 服务，支持 Windows / macOS / Linux，提供：

```text
/platform
/execute
/run_python
/run_bash_script
/screenshot
/cursor_position
/screen_size
/list_directory
```

以及更多 desktop / file / recording endpoint。

## Recording Layer

AnyTool 还很重视 execution trace。

主要文件：

```text
anytool/recording/manager.py
anytool/recording/recorder.py
anytool/recording/action_recorder.py
```

它会记录：

| File | Meaning |
|---|---|
| `metadata.json` | task metadata、backend counts、start/end time |
| `traj.jsonl` | 每一步 tool execution |
| `conversations.jsonl` | 每轮 LLM input/output |
| `agent_actions.jsonl` | 高层 agent planning / execute / evaluate action |
| `screenshots/` | GUI / visual task screenshot |
| `recording.mp4` | 可选视频记录 |

这对我们的 Research OS 很关键。

研究系统最怕的是：

```text
结果出来了，但不知道怎么来的。
```

AnyTool 的 recording layer 可以变成：

```text
experiment trace
agent audit log
debug artifact
paper appendix material
tool failure diagnosis source
future UpSkill distillation input
```

这也说明 `AnyTool` 和 `UpSkill` 很适合放在一起：

```text
AnyTool 产生执行轨迹。
UpSkill 从失败/成功轨迹里蒸馏 skill。
```

## 配置系统

配置文件分成几类：

| Config | Meaning |
|---|---|
| `config_agents.json` | agent 定义、backend scope、max iterations |
| `config_mcp.json` | MCP server registry |
| `config_grounding.json` | backend settings、Smart Tool RAG、tool quality |
| `config_security.json` | shell / network / file 权限和 blocked commands |
| `config_dev.json` | local override |

加载优先级是：

```text
config_grounding.json
-> config_security.json
-> config_dev.json
```

安全配置里默认 sandbox 是 false，但有 blocked commands：

```text
rm -rf
shutdown
reboot
poweroff
halt
mkfs
dd
diskpart
taskkill /f
...
```

这个地方我们要有清醒认识：

```text
AnyTool 是真的能执行系统操作的工具层。
```

所以在自己的 Research OS / Quant OS 里不能只讲能力，也必须讲 governance：

```text
permission
dry-run
allowlist
workspace boundary
read-only mode
human PM approval
audit log
```

## 当前代码里的工程信号

这次本地检查发现两个很明确的改进点。

### 1. `utils/ui.py` 有 f-string 语法错误

运行：

```bash
py -m compileall -q anytool
```

失败：

```text
*** Error compiling 'anytool\\utils\\ui.py'...
  File "anytool\utils\ui.py", line 427
    (colorize(f'{result.get('execution_time', 0):.2f}s', 'c'))
                             ^^^^^^^^^^^^^^
SyntaxError: f-string: f-string: unmatched '('
```

问题在嵌套引号：

```python
f'{result.get('execution_time', 0):.2f}s'
```

应该改成类似：

```python
f"{result.get('execution_time', 0):.2f}s"
```

这是一个非常适合提 PR 的点，因为它是确定性 bug，修复范围小，影响 CLI UI。

### 2. `__main__.py` 有孤立 `w`

`anytool/__main__.py` 的 `_create_argument_parser()` 里，parser 创建后有一行孤立的：

```python
w
```

这个不会被 `compileall` 抓出来，因为它是合法语法。但运行到 `_create_argument_parser()` 时会触发 `NameError`。

这也是适合提 PR 的点：

```text
remove stray "w" from CLI parser setup
```

这两个问题的价值不只是“修 bug”，而是说明我们读 repo 时应该形成固定流程：

```text
read architecture
run syntax check
run entrypoint smoke test
identify minimal PR
write issue / PR with reproduction
```

这就是从“学习项目”走向“贡献项目”的路径。

## 对 Pengyi Research OS 的启发

`AnyTool` 对我们最直接的启发是：

```text
Research OS 不能把每个能力写死在 agent prompt 里。
应该有一个独立的 capability routing layer。
```

我们的 Research OS 可以借鉴这个结构：

```text
ResearchAgent
-> CapabilityRouter
-> ToolRegistry
-> Data / Code / Backtest / Report / Web / GUI tools
-> ToolQualityMemory
-> TraceStore
```

对于 AI scientist 路线：

| AnyTool Component | Research OS 对应 |
|---|---|
| GroundingAgent | Research execution agent |
| GroundingClient | Capability router |
| MCP backend | external academic / GitHub / Drive / arXiv tools |
| Shell backend | experiment runner |
| Web backend | literature and project research |
| GUI backend | fallback for no-API systems |
| ToolQualityManager | tool reliability memory |
| RecordingManager | experiment trace and audit log |
| Smart Tool RAG | retrieve tools by research task |

这和我们之前做的 `HKUDS0000` 四大主线也能接起来：

```text
RAG / Knowledge -> 找资料和结构化知识
Agent Framework / Workspace -> 运行 agent
Research OS / AI Scientist -> 做研究任务
Quant / Finance -> 产生真实业务和策略任务
```

`AnyTool` 属于中间的能力路由层。

## 对 Pengyi Quant Research OS 的启发

量化研究最需要的不是一个单独脚本，而是一套能连续跑的 R&D loop：

```text
factor hypothesis
-> data retrieval
-> implementation
-> backtest
-> bias diagnosis
-> report
-> next research plan
-> human PM review
```

这里每一步都需要工具。

如果借鉴 AnyTool，我们可以设计：

| Quant Step | Tool Layer |
|---|---|
| data retrieval | market data MCP / local data loader / vendor API |
| factor implementation | shell code runner / notebook executor |
| backtest | backtest engine tool |
| bias diagnosis | statistical diagnostic tool |
| report | markdown / pdf / website publishing tool |
| next plan | research planner agent |
| PM review | approval gate / comment system |
| production handoff | order simulation / broker API / risk checker |

真正关键的是：

```text
每个工具都要有质量记忆。
```

比如：

```text
哪个数据源经常缺字段？
哪个 backtest runner 容易超时？
哪个因子生成器容易产生 look-ahead bias？
哪个 report template 缺图？
哪个 broker sandbox 经常 reject order？
```

这些都应该进入 `ToolQualityMemory`，影响下一次工具选择。

所以我们的 Quant OS 不是只学 AnyTool 的代码，而是学它的系统思想：

```text
tools are not static.
tool-use is a learning problem.
```

## 和 UpSkill 的组合

`UpSkill` 和 `AnyTool` 非常互补。

```text
AnyTool 解决“怎么调用工具”。
UpSkill 解决“怎么把调用工具的经验沉淀成 skill”。
```

组合以后可以形成一个闭环：

```text
task
-> AnyTool executes with selected tools
-> trajectory recorded
-> failure/success analyzed
-> UpSkill distills SKILL.md
-> skill store updated
-> next AnyTool task gets better instruction and routing
```

对于我们现在每天学习 repo、写网站、准备 RA、做 quant system，这个闭环很真实：

```text
repo study trace -> repo study skill
website publishing trace -> publishing skill
PR failure trace -> contribution skill
backtest trace -> research skill
application material trace -> pitch skill
```

这才是我们要的 AI scientist 复利系统。

## 和 OpenHarness 的关系

`OpenHarness`、`UpSkill`、`AnyTool` 可以这样放：

| Repo | Layer | 一句话 |
|---|---|---|
| `OpenHarness` | Runtime / Harness | agent 怎么被运行、被组织、被接入 channel |
| `AnyTool` | Tool-Use / Capability Routing | agent 怎么找到并调用工具 |
| `UpSkill` | Self-Improvement / Skill Memory | agent 怎么从经验里沉淀 skill |

组合起来就是：

```text
OpenHarness runs the agent.
AnyTool gives the agent hands.
UpSkill gives the agent memory of how to do better.
```

对我们的系统来说：

```text
OpenHarness = 底层运行时参考
AnyTool = 能力路由层参考
UpSkill = 技能沉淀层参考
```

## 可以提的 PR

这次最适合的 PR 方向不是大改架构，而是小而确定的修复。

### PR 1: Fix CLI UI syntax error

问题：

```text
anytool/utils/ui.py:427 has nested single quotes in f-string.
```

复现：

```bash
py -m compileall -q anytool
```

预期修复：

```text
change inner/outer quotes so package compiles.
```

### PR 2: Remove stray `w` in CLI parser

问题：

```text
anytool/__main__.py::_create_argument_parser contains a standalone "w".
```

影响：

```text
Calling the CLI parser raises NameError before parsing arguments.
```

预期修复：

```text
delete the stray line and add a minimal CLI smoke test.
```

### PR 3: Document optional search dependencies

`search_tools.py` 里使用：

```text
fastembed
rank_bm25
```

但它们不是核心 dependencies。可以在 README / extras 里更清楚地写：

```text
semantic search requires fastembed or embedding API.
keyword BM25 requires rank_bm25, otherwise falls back to term overlap.
```

这个 PR 可以不急，先做前两个确定性 bug 更合适。

## 我们怎么吸收

这期 `HKUDS017` 对我们的行动建议很明确：

```text
不要急着做一个巨大的 quant agent。
先做一个最小 Tool-Use Layer。
```

可以从四类工具开始：

| Tool Category | 最小实现 |
|---|---|
| data tool | 读取本地 CSV / parquet / sample market data |
| code tool | 运行 factor implementation script |
| backtest tool | 调用一个简化 backtest runner |
| report tool | 生成 markdown report |

然后再加：

```text
tool registry
tool search
tool quality record
trace recording
human PM approval
```

这会比“一上来写完整实盘系统”稳很多。

我们真正要的是：

```text
可执行
可追踪
可复盘
可沉淀
可贡献开源
```

`AnyTool` 的价值就在这里。它提醒我们，agent 的核心能力不只是推理，而是把推理变成可靠行动。

## Next

下一篇继续 HKUDS 主线：

```text
HKUDS018 -> MiniRAG
```

`AnyTool` 是工具能力层，`MiniRAG` 会重新回到知识层。两者正好互补：

```text
MiniRAG helps agent know.
AnyTool helps agent act.
```

我们的 Research OS 需要两者。
