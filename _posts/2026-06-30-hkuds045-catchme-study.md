---
title: "HKUDS045: CatchMe 作为 Personal Digital Footprint、Agent Memory Layer 与 Research OS Context Engine"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds045, hkuds, catchme, personal-ai, memory-layer, digital-footprint, agent-memory, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS045`。

```text
HKUDS045 -> CatchMe
```

上一篇是：

```text
HKUDS044 -> ViMax
```

`ViMax` 讲的是：

```text
idea / novel / script -> video artifact
```

它是一个多模态 production layer。

这一篇看 `CatchMe`。

`CatchMe` 讲的是：

```text
personal digital activity -> hierarchical memory -> agent-queryable context
```

它是一个个人 context engine。

一句话定位：

```text
CatchMe = always-on local digital footprint recorder
        + SQLite event store
        + screenshot / keyboard / window / clipboard / idle capture
        + hierarchical activity tree
        + bottom-up LLM summarization
        + tree-based retrieval
        + CLI / Web / MCP / Agent Skill interface
```

这是非常关键的一类 project。

因为未来的 personal agent 不是只靠 prompt。

真正的 personal agent 需要：

```text
知道你看过什么
知道你写过什么
知道你搜过什么
知道你在哪些文件里工作
知道你的长期研究路径
知道你的上下文和证据
```

这就是 CatchMe 的位置。

它不是一个简单 screen recorder。

它更像是：

```text
personal memory infrastructure for agents
```

## 为什么 HKUDS045 看 CatchMe

前面我们已经看了很多 HKUDS agent product：

```text
NanoBot
CLI-Anything
AutoAgent
OpenHarness
AgentSpace
OpenSpace
ClawTeam
ClawWork
FastAgent
Litewrite
ViMax
```

这些系统都需要一个底层问题：

```text
agent 怎么知道用户过去做了什么？
```

普通 agent 的上下文主要来自：

```text
当前 prompt
当前 workspace files
当前 terminal output
当前 chat history
```

但真实工作不是这样。

真实工作是连续的：

```text
昨天看的 paper
上午调的代码
刚刚复制过的 error
浏览器里打开过的网页
PDF 里看到的一段话
会议里记下的线索
某个文件改了又删
某个 issue 读过但没保存
```

这些内容如果没有被结构化记录，agent 就无法复用。

所以 CatchMe 的核心启发是：

```text
personal agent 需要一个持续记录、结构化、可检索、可审计的个人上下文系统。
```

对 Pengyi Research OS 来说，这非常重要。

我们要做的是 AI Scientist / Quant Research OS。

这类系统不只是生成文章或代码。

它还需要长期积累：

```text
research trace
coding trace
reading trace
experiment trace
writing trace
application trace
networking trace
```

CatchMe 给的是这类系统的底层 memory pattern。

## 本次阅读状态

本次阅读的是本地 HKUDS 工作区里的 `CatchMe`。

阅读前执行了远端同步，当前已经是最新 `origin/main`。

| item | value |
| --- | --- |
| repo | `CatchMe` |
| remote | `https://github.com/HKUDS/CatchMe.git` |
| branch | `main` |
| latest commit | `12c735360882bc9c3a8a580351b49172ac414fbb` |
| commit date | `2026-05-27T20:05:00+08:00` |
| latest message | `Merge pull request #9 from salem221094/fix/ci-lint-errors-v3` |
| package | `catchme` |
| version | `0.1.0` |
| Python | `>=3.11` |
| license | `Apache-2.0` |

文件规模：

| type | count |
| --- | ---: |
| tracked files | 107 |
| Python | 48 |
| JavaScript | 17 |
| PNG assets | 13 |
| Markdown | 8 |
| CSS | 6 |
| SVG | 5 |
| JSON | 2 |
| HTML | 2 |

本地验证：

```text
py -m compileall -q catchme
```

结果：

```text
passed
```

测试命令：

```text
py -m pytest
```

当前本机 Python 环境缺少 `pytest`：

```text
No module named pytest
```

这不是项目测试失败，而是当前环境没有安装 dev dependency。

## 项目用途

`README.md` 的主标题是：

```text
CatchMe: Make Your AI Agents Truly Personal
```

它的目标是：

```text
Capture Your Entire Digital Footprint
Lightweight
Vectorless
Powerful
```

它覆盖的使用场景包括：

| 场景 | 用户问题 |
| --- | --- |
| personal coding assistant | 今天我在 Claude Code / Cursor 里写了什么 |
| personal deep research | 昨天我读了哪些 AI 内容 |
| personal files manager | 我今天改过哪些文件 |
| digital life overview | 我下午怎么花掉的 |

从产品角度看，CatchMe 的价值不是“帮你截图”。

它的价值是：

```text
把用户的电脑活动转成 agent 可查询的个人记忆。
```

这句话非常关键。

因为屏幕、键盘、文件、网页本来是非结构化的。

CatchMe 把它们转成：

```text
raw events
filtered spans
activity tree
LLM summaries
retrieval evidence
final answer
```

这就从“日志”变成了“记忆”。

## 总体架构

CatchMe 的架构可以分成七层：

```text
Layer 1: Recorder Layer
  window / keyboard / mouse / clipboard / idle / notification

Layer 2: Raw Event Store
  SQLite events_raw + FTS5

Layer 3: Filtering Layer
  window spans + keyboard clusters + mouse clusters + clipboard grouping

Layer 4: Activity Tree Layer
  Day -> Session -> App -> Location -> Action

Layer 5: Summarization Layer
  L0 mouse vision summary
  L1 action summary
  L2 location / app summary
  L3 session summary

Layer 6: Retrieval Layer
  time resolve -> tree navigation -> deeper inspection -> answer

Layer 7: Interface Layer
  CLI / Web UI / MCP / agent skill
```

这不是一个单点功能。

它是一个完整 personal memory stack。

## 目录结构

核心目录：

```text
CatchMe/
  catchme/
    __init__.py
    run.py
    config.py
    engine.py
    store.py
    organizer.py
    summary_queue.py
    web.py
    mcp_server.py
    recorder.py
    recorders/
    pipelines/
    services/
    extractors/
    extension/
    static/
    tests/
  CATCHME-light.md
  CATCHME-full.md
  README.md
  pyproject.toml
```

关键文件定位：

| 文件 | 作用 |
| --- | --- |
| `catchme/run.py` | CLI 入口，处理 `init / awake / web / ask / mcp / cost / disk / ram` |
| `catchme/__init__.py` | `CatchMe` Python API，封装 Engine / Store / Recorder |
| `catchme/config.py` | 默认路径与 interval 配置 |
| `catchme/store.py` | SQLite raw event store + FTS5 |
| `catchme/engine.py` | recorder orchestration + writer thread + organizer thread |
| `catchme/organizer.py` | boundary-event-driven tree rebuild + summary enqueue |
| `catchme/summary_queue.py` | 异步 LLM summarization queue |
| `catchme/pipelines/filter.py` | window span / keyboard / mouse / clipboard clustering |
| `catchme/pipelines/tree.py` | hierarchical activity tree |
| `catchme/pipelines/summarize.py` | bottom-up LLM summary |
| `catchme/pipelines/retrieve.py` | tree-based retrieval |
| `catchme/services/llm.py` | OpenAI-compatible LLM wrapper |
| `catchme/web.py` | Flask dashboard API |
| `catchme/mcp_server.py` | MCP stdio server |
| `catchme/extension/` | browser page content capture extension |
| `CATCHME-light.md` | 给 agent 的轻量 skill |
| `CATCHME-full.md` | 给 agent 的完整安装与运维 skill |

## 本地数据布局

`Config` 默认把所有数据放在：

```text
~/.catchme
```

下面有：

```text
~/.catchme/data.db
~/.catchme/blobs/
~/.catchme/trees/
~/.catchme/workspace/
~/.catchme/config.json
~/.catchme/llm_usage.json
~/.catchme/summary_updates.jsonl
~/.catchme/monitor_history.json
```

这个设计很干净。

它把：

```text
code repo
runtime data
user private data
```

分开了。

对个人 AI 工具来说，这是正确方向。

因为 user memory 不应该混在项目源码里。

## Raw Event Store

`catchme/store.py` 是底层事件库。

它用 SQLite 存一张核心表：

```text
events_raw
  id
  ts
  kind
  data
  blob
  processed
```

其中：

| 字段 | 含义 |
| --- | --- |
| `ts` | Unix timestamp |
| `kind` | event 类型，例如 window / keyboard / mouse / clipboard / idle |
| `data` | JSON 字符串，存事件上下文 |
| `blob` | 图片等外部 blob 路径 |
| `processed` | 后续管线处理标记 |

它还建了：

```text
events_raw_fts
```

也就是 SQLite FTS5。

这个细节很重要。

CatchMe 虽然主打：

```text
vectorless retrieval
```

但它不是完全没有 search。

它的 raw search 是：

```text
SQLite FTS5
```

而不是：

```text
embedding + vector database
```

这让系统非常轻。

对个人本地 memory 来说，SQLite + FTS5 是一个很务实的起点。

## Recorder Layer

CatchMe 的 recorder 不是单一屏幕录制。

它包括：

```text
WindowRecorder
KeyboardRecorder
MouseRecorder
ClipboardRecorder
IdleRecorder
NotificationRecorder on macOS
```

非 macOS 下默认是五类：

```text
window
keyboard
mouse
clipboard
idle
```

macOS 多一个：

```text
notification
```

### WindowRecorder

`WindowRecorder` 记录：

```text
app
title
pid
x / y / w / h
url
filepath
```

如果当前 app 是浏览器，会尝试拿 URL。

如果不是浏览器，会尝试从窗口 title / pid 推断文档文件路径。

这非常关键。

因为一个 window event 不只是“打开了 Chrome”。

更有价值的是：

```text
打开了哪个 URL
打开了哪个文件
窗口标题是什么
停留了多久
```

这些字段之后会成为 `Location` 节点。

### MouseRecorder

`MouseRecorder` 的设计很有意思。

它不是按固定时间截图。

它是在鼠标事件触发时截图：

```text
click -> capture full screen
click -> annotate crosshair
click -> save full.webp
click -> save detail.webp
click -> write metadata.json
```

它还处理 scroll session：

```text
scroll_start
scroll_end
```

这样比“每秒截图”更节省空间。

因为对于用户行为，click 和 scroll 往往比纯时间采样更有信息量。

这是一个很实用的工程选择。

### KeyboardRecorder

Keyboard recorder 是平台相关实现。

仓库里有：

```text
keyboard_windows.py
keyboard_macos.py
keyboard.py
```

它会区分：

```text
text
key
shortcut
```

同时 `filter.py` 里有 IME 处理：

```text
strip IME pinyin composition
keep committed CJK text
```

这个细节说明作者真的考虑了中文输入场景。

### Browser Extension

`catchme/extension/` 里有一个 Chrome extension：

```text
manifest.json
background.js
content.js
lib/Readability.js
```

它做的是：

```text
extract page content
send to Python backend through websocket
```

`content.js` 先尝试 Readability。

如果 Readability 不适合，就 fallback 到 DOM walk。

它会提取：

```text
url
title
description
siteName
byline
excerpt
content
method
```

这对 research assistant 很重要。

因为用户看网页时，只有 screenshot 是不够的。

如果能拿到正文，就能做更强的 recall。

## Engine

`Engine` 是 runtime 中心。

它负责：

```text
start writer thread
start organizer thread
start all recorders
receive recorder events
write raw event batches
notify organizer
call user on_event callback
flush on stop
```

事件流是：

```text
Recorder
  -> Engine.emit
  -> Queue
  -> Store.insert_raw
  -> Organizer.on_event
  -> optional CLI display
```

`Engine` 有两个重要工程点：

1. recorder 事件不直接写库，而是先进 Queue。
2. Organizer 的 `on_event` 只做轻量 boundary 标记。

这避免了 recorder 被 LLM / tree rebuild 阻塞。

对 always-on 系统来说，这个设计是必要的。

## Organizer

`Organizer` 是 CatchMe 的实时结构化核心。

它不是每来一个 event 就重建树。

它只在边界事件出现时触发：

```text
window switch
idle / locked
fallback interval
```

然后做：

```text
load or build today's tree
extend existing tree
save latest tree snapshot
enqueue closed nodes for summarization
```

这个模式非常好：

```text
raw event stream is continuous
semantic tree is boundary-driven
LLM summary is async
```

这三者分开，系统才稳定。

## Activity Tree

`catchme/pipelines/tree.py` 是 CatchMe 最核心的算法文件之一。

它构建两种模式：

```text
time mode:
  Day -> Session -> App -> Location -> Action

app mode:
  Day -> App -> Location -> Action
```

默认主检索使用的是 `time` mode。

`Day -> Session -> App -> Location -> Action` 这棵树非常适合个人活动记忆。

原因是：

| 层级 | 对应真实工作 |
| --- | --- |
| Day | 某一天 |
| Session | 一段连续工作流 |
| App | 使用的软件 |
| Location | URL / file path / window title |
| Action | 具体输入、点击、复制、滚动 |

这比 flat log 强很多。

flat log 的问题是：

```text
信息太碎
LLM 不能一次读完
很难知道哪些片段重要
```

树结构的好处是：

```text
先看高层摘要
再选择相关分支
最后 drill down 到证据
```

这就是 CatchMe 的 vectorless retrieval 基础。

## Window Span 与 Action Cluster

`filter.py` 先把 window event 转成 span。

它做了几件事：

```text
filter short dwell
merge same app/title adjacent spans
attach brief windows to owner span
cap long spans for idle/session split
```

然后对 keyboard / mouse / clipboard 做时间聚类。

其中 mouse cluster 有一个非常重要的规则：

```text
如果 scroll session 没有结束，即使时间间隔超过 gap，也不切 cluster。
```

这说明系统不是简单按时间切。

它理解：

```text
scroll_start -> scroll_end
```

是一段连续行为。

这种小设计会显著提升 activity summary 的质量。

## Stable Node ID

Activity tree 里的 node_id 是稳定生成的。

例如：

```text
day id = dYYYYMMDD
session id = day_id + start timestamp
app id = sanitized app name
location id = short hash of URL/file/title
action id = location id + action start timestamp
```

这很重要。

因为 tree 会被增量 rebuild。

如果 node_id 不稳定，之前的 summary 就无法复用。

CatchMe 有 `merge_summaries` 和 `_apply_merge`，会根据 node_id 把旧 summary / evidence / mouse_summaries 复制到新树。

这就是 personal memory system 的工程基本功。

## Summarization Pipeline

`summarize.py` 把 activity tree 做 bottom-up summary。

层级是：

```text
L0 Mouse Cluster -> vision LLM -> mouse_summaries
L1 Action        -> text LLM   -> action.summary
L2 Location      -> text LLM   -> location.summary
L2 App           -> text LLM   -> app.summary
L3 Session       -> text LLM   -> session.summary
```

它要求每个 summary 都有结构：

```text
Summary
Evidence
```

这点非常重要。

因为个人记忆不能只是“概括”。

它必须保留证据。

如果没有 evidence，后面 agent 很容易产生幻觉。

### Closed Node Summarization

CatchMe 默认只总结 closed nodes。

也就是：

```text
children[:-1] are closed
children[-1] is active
```

当前正在进行的最后一个节点不会被自动总结。

这个设计也很务实。

原因是：

```text
active node 还在变化
过早总结会不断失效
会浪费 LLM cost
```

等 node 关闭以后再总结，就更稳定。

### SummaryQueue

`SummaryQueue` 用 priority queue 和 thread pool 做异步总结。

它的优先级逻辑是：

```text
action level first
location / app next
session later
```

总结成功后，会 cascade 到 parent。

也就是：

```text
child summary ready
  -> parent may become ready
  -> enqueue parent summary
```

这就是 bottom-up memory compression。

## Retrieval Pipeline

`retrieve.py` 是 CatchMe 另一个核心文件。

它的检索流程可以概括成：

```text
load saved time trees
resolve query time range
filter days / sessions
LLM selects tree nodes
LLM evaluates details
if needed, go deeper
inspect raw keyboard / file / URL / screenshot
generate final answer with sources
```

它不是 embedding retrieval。

它是：

```text
tree navigation retrieval
```

这和 HKUDS 的 `PageIndex` 思想一致：

```text
先组织成树，再让 LLM 沿树导航。
```

### Time-Aware Retrieval

第一步是解析时间。

用户可能问：

```text
我今天上午在做什么？
昨天我读了什么？
上周五我打开过哪个文件？
```

`retrieve.py` 会先让 LLM 把自然语言时间解析成：

```text
dates
start_hour
end_hour
```

然后过滤 tree。

这样可以大幅缩小检索范围。

对个人 activity memory 来说，时间过滤是非常关键的。

因为大多数个人 recall 问题都是时间相关问题。

### Select / Evaluate Loop

主循环是：

```text
format current frontier as ToC
LLM select relevant nodes
LLM read selected node summaries/evidence
LLM decide next action:
  answer
  deeper
  siblings
```

如果需要 deeper：

```text
session -> app
app -> location
location -> action
action -> raw keyboard / raw mouse
```

如果 location 是文件路径：

```text
read actual file content
```

如果 location 是 URL：

```text
fetch page content
```

如果 action 有 mouse screenshot：

```text
inspect screenshot through vision LLM
inspect detail crop if useful
```

这就是 CatchMe 和普通日志搜索最大的不同：

```text
它能从 summary drill down 到 evidence。
```

## CLI

`run.py` 提供这些命令：

```text
catchme init
catchme awake
catchme web
catchme ask -- <question>
catchme mcp
catchme cost
catchme disk
catchme ram
catchme help
```

这些命令覆盖了完整生命周期：

| command | 作用 |
| --- | --- |
| `init` | 配置 LLM provider / API key / model |
| `awake` | 启动 always-on recorder daemon |
| `web` | 启动 web dashboard |
| `ask` | 对 activity history 做自然语言查询 |
| `mcp` | 启动 MCP stdio server |
| `cost` | 查看 LLM token 用量 |
| `disk` | 查看存储用量 |
| `ram` | 查看进程内存占用 |

这说明 CatchMe 不只是库。

它是一个完整本地工具。

## Web UI

`web.py` 用 Flask 提供 dashboard 和 API。

核心 endpoint：

| endpoint | 作用 |
| --- | --- |
| `/api/search` | SQLite FTS raw event search |
| `/api/events` | 查询 raw events |
| `/api/stats` | event kind 统计 |
| `/api/timeline` | 按 kind 分组的 timeline |
| `/api/filtered` | filtered window/action view |
| `/api/tree` | build / load activity tree |
| `/api/chat` | SSE 形式运行 retrieval |
| `/api/config/summarize` | summary config 读写 |
| `/api/config/llm` | LLM config 读写 |
| `/api/llm/status` | LLM budget 状态 |
| `/api/events/summaries` | summary notification SSE |
| `/api/digest` | flatten summaries |
| `/api/monitor` | disk / memory / event / token monitor |
| `/blobs/<path>` | 展示 screenshot blob |

这里有两个很强的产品点：

1. `/api/chat` 是 streaming retrieval。
2. `/api/events/summaries` 是 summary 更新的 SSE。

这意味着前端可以实时看到 memory tree 在长出来。

## MCP Server

`mcp_server.py` 提供 MCP stdio server。

暴露四个 tools：

```text
search_activity(query, date="")
list_days()
get_session(session_id)
get_tree(date)
```

这非常关键。

因为 CatchMe 的真正目标不是自己做一个聊天窗口。

它的目标是让其他 agent 可以调用它。

MCP 让 CatchMe 成为：

```text
personal context provider
```

这和我们前面看的 agent product 主线完全接上了。

## Agent Skill

仓库里有两个 skill 文件：

```text
CATCHME-light.md
CATCHME-full.md
```

`light` 版本假设：

```text
CatchMe 已经安装
catchme awake 已经在后台运行
agent 只负责查询
```

`full` 版本负责：

```text
发现 conda env
安装 CatchMe
写 config
启动 awake
使用 ask / web / cost / disk / ram
```

这个设计很有启发。

它把 agent integration 分成两种权限级别：

```text
read-only query memory
full lifecycle management
```

对个人工具来说，这是正确的分层。

因为不是每个 agent 都应该有权限安装、启动、配置你的 recorder。

## LLM Provider Abstraction

`services/llm.py` 是 OpenAI-compatible wrapper。

支持：

```text
OpenRouter
AiHubMix
SiliconFlow
OpenAI
Anthropic
DeepSeek
Gemini
Groq
Mistral
Moonshot / Kimi
MiniMax
Zhipu AI
DashScope
VolcEngine
BytePlus
Ollama
vLLM
LM Studio
```

它还支持：

```text
Chat Completions API
Responses API through wire_api = responses
vision messages
sync / async calls
streaming
retry on 429 / 502 / 503 / 504
token usage tracking
LLM call budget
```

这里最值得学的是：

```text
cost control is part of memory infrastructure
```

always-on memory 如果没有 cost budget，很容易变成不可控系统。

CatchMe 把 `llm.max_calls`、token history、`cost` command、web monitor 都做出来了。

这很务实。

## 为什么说它 Vectorless

CatchMe 的 `README` 强调：

```text
No Vector Complexity
Tree-Based Retrieval
```

这里的 vectorless 不是说没有任何检索。

而是说：

```text
不依赖 embedding + vector DB 作为主 memory retrieval 机制。
```

它的主机制是：

```text
activity tree
summary
evidence
LLM navigation
```

它的 raw text keyword search 是 SQLite FTS5。

这其实很适合个人数字足迹。

原因是：

1. 个人活动天然有时间结构。
2. 个人活动天然有 app / URL / file 结构。
3. 大量问题需要精确证据，不只是语义相似。
4. 本地运行要轻，不一定要上 vector DB。

如果我们做 Research OS，也可以借鉴：

```text
first structure, then retrieval
```

不要一上来就把所有东西塞进 embedding。

## 和 HKUDS 其他项目的关系

| Project | CatchMe 关系 |
| --- | --- |
| `NanoBot` | CatchMe 可以给 NanoBot 提供个人长期记忆 |
| `CLI-Anything` | CLI-Anything 让软件 agent-native，CatchMe 让个人活动 agent-readable |
| `OpenHarness` | OpenHarness 管 agent execution，CatchMe 管 user context |
| `AgentSpace` | AgentSpace 是 workspace，CatchMe 是 personal activity memory |
| `OpenSpace` | OpenSpace 偏 self-evolving workspace，CatchMe 偏 private context trace |
| `ClawWork` | AI coworker 需要知道你过去做了什么，CatchMe 提供 evidence |
| `ClawTeam` | 团队 agent 如果要接个人工作流，CatchMe 是个人端 context collector |
| `Litewrite` | Litewrite 生成文档，CatchMe 记录写作过程和材料来源 |
| `ViMax` | ViMax 生成视频，CatchMe 记录项目制作和素材研究过程 |
| `VideoRAG` | VideoRAG 管视频知识，CatchMe 管个人屏幕活动 |
| `PageIndex` | CatchMe 的 tree-based retrieval 受 PageIndex 启发 |

所以 CatchMe 可以被看成 HKUDS agent ecosystem 里的：

```text
personal memory substrate
```

它不是最炫的 agent。

但它非常底层。

底层系统才决定 agent 能不能长期有用。

## 和 Research OS 的关系

对 Pengyi Research OS 来说，CatchMe 的启发非常直接。

我们要做一个 AI Scientist OS，不能只保存最终论文和最终代码。

更重要的是保存：

```text
我为什么走到这个 idea
我读过哪些材料
我在哪个实验卡住
我怎么 debug
我和谁沟通过
我如何形成研究判断
我怎么从一个 project 迁移启发到另一个 project
```

这就是 research trace。

CatchMe 提供的是：

```text
research trace capture architecture
```

它可以帮助我们构建：

```text
Personal Research Flight Recorder
```

也就是一个研究黑匣子。

每一天的研究轨迹都能被压缩成：

```text
day summary
session summary
paper reading summary
coding summary
experiment summary
writing summary
application summary
```

然后后续 agent 可以问：

```text
我上周为什么觉得 QuantMind 和 X2Strategy 有相似性？
我昨天对 ViMax 的核心结论是什么？
我最近哪些 project 可以写成 PR？
我申请 RA 材料最近改了哪些点？
我有哪些 evidence 可以支持我的研究叙事？
```

这就是我们网站和个人项目要走的方向。

## 和 Quant OS 的关系

CatchMe 表面上不是 quant 项目。

但它对 Quant OS 很有价值。

量化研究里有大量过程型信息：

```text
读 paper
查数据源
写 factor
跑 backtest
看 error
调参数
分析 drawdown
记录 PM feedback
准备 pitch
整理 meeting notes
```

这些信息如果只存在临时窗口里，很容易丢。

CatchMe 可以启发我们做：

```text
Quant Research Activity Trace
```

例如：

```text
factor idea -> code edit -> backtest run -> result screenshot -> diagnosis -> next plan
```

这正好对应我们之前的 R&D Agent for Quant Research：

```text
自动提出因子假设
自动实现
自动回测
自动诊断偏差
自动生成下一轮研究计划
人类 PM 审核
```

CatchMe 不负责生成因子。

它负责记录和查询研究过程。

这可以成为 Quant OS 的：

```text
context memory layer
```

## 但 Quant / Bank / Workplace 场景必须谨慎

CatchMe 的能力非常强。

但越强越需要边界。

它会记录：

```text
screenshots
keystrokes
clipboard
window titles
URLs
file paths
notifications
```

这在个人电脑上是 useful。

但在公司、银行、量化机构、客户数据场景里，风险很高。

必须明确：

```text
不要在未授权工作设备上记录敏感信息。
不要把公司数据、客户数据、代码、交易信息发给云端 LLM。
不要违反劳动合同、合规要求、数据安全制度。
```

如果未来我们要做类似能力，必须有：

```text
local-first
explicit consent
pause button
app allowlist / denylist
secret redaction
retention policy
encryption
audit log
export / delete
cloud LLM off switch
```

这不是形式主义。

这是 personal agent memory 能不能真正落地的基本条件。

## 对我们自己的启发

CatchMe 对我们的核心启发可以总结成六条。

### 1. Memory 要 Event-Sourced

不要只存最终总结。

要先存 raw event。

```text
raw event -> derived structure -> summary -> answer
```

因为 summary 会错。

raw evidence 才是可审计基础。

### 2. Memory 要 Hierarchical

不要把所有内容塞进一个长 context。

要组织成：

```text
Day
Session
App
Location
Action
Evidence
```

研究系统也一样。

可以是：

```text
Project
Question
Hypothesis
Experiment
Artifact
Evidence
Decision
Next Plan
```

### 3. Retrieval 要先结构后语义

CatchMe 的路线是：

```text
tree first
LLM navigate second
raw evidence last
```

这比“全部 embedding”更适合长期个人记忆。

### 4. Active Context 和 Closed Context 要分开

正在进行的工作不要急着总结。

等它关闭以后再总结。

这个思想可以迁移到 Research OS：

```text
active experiment
closed experiment
published artifact
archived trace
```

不同阶段有不同处理方式。

### 5. Agent Memory 应该有 CLI / MCP

个人记忆系统不能只服务一个 UI。

它应该被多种 agent 调用：

```text
CLI
MCP
Python API
Web API
Skill file
```

CatchMe 这点做得很好。

### 6. Cost 是一等公民

always-on system 的成本必须可见。

CatchMe 有：

```text
max_calls
token usage
cost command
monitor API
web dashboard
```

这对任何 R&D Agent 都重要。

## 可以迁移到 Pengyi Research OS 的模块

我们可以把 CatchMe 的思想迁移成：

```text
pengyi_context_engine/
  record/
  store/
  tree/
  summarize/
  retrieve/
  evidence/
  mcp/
```

但我们的第一版不一定要记录整个屏幕。

更合理的起点是：

```text
Git activity
Markdown notes
paper PDFs
browser bookmarks / exported reading list
terminal commands
experiment logs
website posts
application materials
manual journal
```

也就是先做低风险、明确授权的数据源。

可以设计成：

```text
Project Memory Tree
  Project
    Day
      Session
        Artifact
          Evidence
          Decision
          Next Action
```

后续再考虑 screen trace。

## 可以形成的网站内容类型

CatchMe 也启发我们网站可以有一种新内容：

```text
Research Trace Post
```

它不是普通学习笔记。

它记录：

```text
我今天读了什么
我怎么理解它
我和之前哪些项目连起来
我产生了什么新的 project idea
我可以做什么 PR
我下一步要做什么
```

这会让网站从展示结果，升级成展示成长轨迹。

对于 RA / PhD / open-source contributor 叙事，这很有价值。

## Possible PR Ideas

读完代码以后，可以看到一些适合我们未来贡献的小点。

这些不一定马上提。

但可以作为 PR radar。

### 1. Wheel Package Static Assets

`pyproject.toml` 里 package data 目前主要包含：

```text
**/*.yaml
**/*.json
```

但 `catchme/static/` 里有：

```text
HTML
CSS
JS
PNG
```

`catchme/extension/` 里也有：

```text
JS
Readability.js
manifest.json
```

如果通过 wheel 非 editable 安装，静态文件是否完整打包需要验证。

这可能是一个很实用的 packaging PR。

### 2. README Data Path Clarification

代码默认路径是：

```text
~/.catchme
```

README 里 privacy notice 的表达容易被理解成：

```text
~/data/
```

可以统一为：

```text
~/.catchme/data.db
~/.catchme/blobs/
~/.catchme/trees/
```

这是文档 PR。

### 3. CLI Help Config Path

`run.py` 的 help 文案里提到：

```text
catchme/services/config.json
```

但当前代码实际使用：

```text
~/.catchme/config.json
```

这里可能是历史遗留文案。

可以提一个小 PR 修正。

### 4. Browser Extension Permission

`background.js` 里使用了：

```text
chrome.webNavigation?.onCompleted
```

但 `manifest.json` 的 permissions 目前主要是：

```text
activeTab
tabs
```

如果希望稳定启用 navigation complete capture，可能需要确认是否应加入：

```text
webNavigation
```

这里需要先实测，再提 PR。

### 5. Windows Skill Instructions

`CATCHME-light.md` 和 `CATCHME-full.md` 的 conda env discovery 主要是 Unix shell 风格。

但项目支持 Windows。

可以补一个 PowerShell 版本。

这对跨平台项目很实用。

### 6. Full Retrieval Tests

当前测试覆盖了：

```text
Store
Config
Engine
Filter
Tree
LLM wrapper
Extractors
_sessions_in_range
```

但 retrieval 主循环可以再加一个 synthetic tree + mocked LLM 的端到端测试。

例如测试：

```text
time filter
select session
go deeper to location
read raw file
generate answer sources
```

这是质量提升 PR。

### 7. Recorder Count Documentation

README 里说 six background recorders。

但代码里：

```text
macOS: window / keyboard / mouse / clipboard / idle / notification
other platforms: window / keyboard / mouse / clipboard / idle
```

可以把文档改成：

```text
five core recorders, plus notification on macOS
```

这样更准确。

## 与我们之前想做的 R&D Agent 的连接

我们之前定义过：

```text
R&D Agent for Quant Research
  = 自动提出因子假设
  + 自动实现
  + 自动回测
  + 自动诊断偏差
  + 自动生成下一轮研究计划
  + 人类 PM 审核
```

CatchMe 不是这个 agent 本身。

它是这个 agent 所需要的：

```text
memory and evidence layer
```

因为 R&D Agent 必须知道：

```text
上一轮研究做了什么
为什么失败
代码改在哪里
哪些结果可信
PM 之前否掉了什么
下一轮计划依据是什么
```

如果这些东西没有结构化记录，agent 只能靠 chat history。

chat history 不够。

真实 Research OS 需要 CatchMe 这种底层。

## 实际使用路线

如果我们自己要试用 CatchMe，我建议分三步。

### Phase 1: 只在个人电脑低敏场景试用

只用于：

```text
开源项目学习
个人 coding
公开网页阅读
公开 paper 阅读
个人网站写作
```

不要用于：

```text
银行内部系统
客户资料
公司代码
交易信息
聊天隐私
密码/API key
```

### Phase 2: 只开本地模型或低风险云模型

优先：

```text
Ollama
LM Studio
vLLM
```

如果使用云 LLM，要明确知道哪些数据会被发出。

### Phase 3: 抽象成 Research OS Memory

不要直接依赖全屏录制。

先做：

```text
notes -> project tree
git commits -> coding trace
paper PDFs -> reading trace
blog posts -> public artifact trace
experiment logs -> research evidence
```

这更适合我们当前阶段。

## CatchMe 的核心价值

CatchMe 最强的地方不是 recorder。

Recorder 很多项目都能做。

它最强的是把 recorder 之后的链条打通：

```text
record
store
filter
tree
summarize
retrieve
answer
agent interface
```

这才是完整产品。

这也是我们做项目要学的地方：

```text
不要只做一个 cool module。
要把它做成可以被真实使用的 system。
```

## 最后总结

`HKUDS045 CatchMe` 的核心结论：

```text
CatchMe 是 personal AI agent 的 memory infrastructure。
```

它把用户的数字活动转成：

```text
raw events
structured activity tree
LLM summaries
evidence-backed retrieval
agent-callable memory
```

对 Pengyi Research OS 来说，它对应：

```text
personal research context engine
```

对 Quant OS 来说，它对应：

```text
quant research trace and evidence memory layer
```

它给我们的最大启发是：

```text
未来强大的 agent，不只是会执行任务。
它还要能长期、私密、可审计地记住人的工作过程。
```

下一步可以继续：

```text
HKUDS046 -> PageIndex / OpenPhone / MineContext direction
```

如果继续沿 CatchMe 的 memory 主线，最自然是看 PageIndex 思想。

如果继续沿 personal agent 主线，也可以看 `OpenPhone`。
