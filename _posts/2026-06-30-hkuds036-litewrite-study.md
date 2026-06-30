---
title: "HKUDS036: Litewrite 作为 AI Research Writing Workspace 与 Vibe Writing Product"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds036, hkuds, litewrite, agent-product, writing-workspace, latex, deep-research, nanobot, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS036`。

```text
HKUDS036 -> Litewrite
```

前面三篇把 Agent Product / Workspace 系列的组织层、经济层和执行层看完：

```text
HKUDS033 ClawTeam  -> AI organization layer
HKUDS034 ClawWork  -> AI coworker economic accountability layer
HKUDS035 FastAgent -> AI agent execution engine
```

这一篇看：

```text
HKUDS036 Litewrite -> AI research writing workspace
```

一句话定位：

```text
Litewrite = collaborative LaTeX workspace
          + TAP smart completion
          + Ask / Agent writing assistant
          + Deep Research report generation
          + shadow document review flow
          + TeXLive compile server
          + Yjs real-time collaboration
          + nanobot mobile / IM interface
```

它不是一个简单的写作助手。

更准确地说，它是一个面向 research writing 的完整工作台：

```text
论文
报告
proposal
application material
technical note
research blog
LaTeX project
collaborative document
```

这些东西都可以在同一个 workspace 里被创建、编辑、编译、版本化、审阅、分享，并且被 AI agent 直接参与。

对 Pengyi Research OS 来说，这一篇非常关键。

原因很直接：

```text
Research OS 不能只有想法、代码、回测和知识库。
它必须有一个 output layer。

研究最后要变成 paper、report、blog、proposal、CV、PS、RP、talk deck、quant memo。
写作不是收尾动作。
写作本身就是研究组织、判断、沟通和发布的核心环节。
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `Litewrite`。

| Item | Value |
|---|---|
| repo | `Litewrite` |
| remote | `https://github.com/HKUDS/Litewrite.git` |
| branch | `main` |
| local head | `d8a9761` |
| full commit | `d8a97617681020e4fa51d76daaf864272c4029dc` |
| latest local commit date | `2026-02-10 23:05:32 +0800` |
| latest local commit | `Update README.md` |
| version | `1.0.7` |
| license | `AGPL-3.0-only` |
| tracked files by `rg --files` | 434 |
| Python files | 116 |
| TS / JS files | 254 |
| Markdown files | 15 |
| JSON files | 7 |
| Docker-related files | 12 |
| local Python syntax check | 116 Python files parsed with Python 3.11, 0 syntax errors |
| local frontend deps | `node_modules` not installed, TypeScript build not run |

项目主结构：

```text
Litewrite/
  app/                 Next.js App Router and API routes
  components/          React UI components
  hooks/               frontend hooks
  lib/                 auth, storage, shadow docs, diff, compile, sessions
  server/              Yjs WebSocket collaboration server
  ai-server/           Python FastAPI AI services
  compile-server/      Node.js LaTeX compile service
  nanobot/             Telegram / Feishu / Lark AI bot integration
  prisma/              database schema and migrations
  public/              assets
  docker-compose.yml
  env.example.oss
```

核心技术栈：

```text
Frontend: Next.js 14, React 18, TypeScript 5, CodeMirror 6, Tailwind, Shadcn UI
Collaboration: Yjs, y-websocket, Redis persistence
Backend: Next.js API Routes, Prisma, NextAuth
AI server: Python FastAPI, OpenRouter, arXiv, web search, SSE streaming
Compile: Node.js, Express, TeXLive
Storage: local filesystem or S3 / MinIO
Database: SQLite in dev, can be moved toward PostgreSQL in production
Bot layer: nanobot, LiteLLM, Telegram, Feishu / Lark
```

## 项目用途

Litewrite 的目标是做：

```text
AI-Powered Collaborative LaTeX Writing Platform
```

README 里用了一个很清晰的产品表达：

```text
Vibe Writing is Coming
```

这个说法很有意思。

它对应的不是传统文本编辑器，也不是只给你补一句话的 autocomplete。
它想做的是：

```text
人在 research workspace 中给方向；
AI 在上下文里读文件、补全文字、查资料、生成报告、修改 LaTeX；
人再审核、合并、编译、发布。
```

核心使用场景包括：

```text
写论文 introduction / related work / method / experiment
修改 LaTeX 结构和表达
查 arXiv 和网页资料后生成 research report
编译 PDF 并定位错误
多人协作编辑同一个 LaTeX project
通过 Telegram / Feishu 在手机或 IM 里管理项目
从 arXiv / GitHub / 上传文件导入项目
保存版本、恢复版本、分享项目
```

从 product 视角看，它很接近：

```text
Overleaf + Cursor-style agent + Deep Research + mobile bot interface
```

从 Research OS 视角看，它更像：

```text
research artifact production layer
```

## 六个核心服务

README 把 Litewrite 拆成 6 个核心服务：

| Service | Role |
|---|---|
| Next.js Web App | 用户界面、API routes、认证、项目管理 |
| WebSocket Server | Yjs 实时协作、文档同步、chat rooms |
| AI Server | TAP completion、Chat Agent、Deep Research |
| Compile Server | TeXLive 编译，输出 PDF |
| nanobot | Telegram / Feishu / Lark 里的 Litewrite 操作入口 |
| Redis | Yjs persistence、rate limiting、cache |

这不是一个单体 demo。

它已经按产品系统拆开：

```text
web user interaction
real-time document state
AI reasoning and tool execution
LaTeX compilation sandbox
mobile / IM interface
persistence and cache
```

这对我们很有启发。

如果 Pengyi Research OS / Quant Research OS 要产品化，也应该拆出这些层：

```text
Workspace UI
Research Agent API
Experiment / Backtest Engine
Artifact Storage
Collaboration / Review Flow
Report Generation
External Chat Interface
```

## TAP Smart Completion

Litewrite 的 TAP completion 是“低摩擦写作体验”的入口。

API 路径是：

```text
Frontend /api/tap/complete
  -> Python AI server /api/tap/complete
  -> TAPService
```

TAP 的输入是：

```text
preamble
prefix
suffix
projectId
```

输出 schema 是：

```text
action: insert | complete_word | fix | skip
confidence: 0.0-1.0
inserted_text
corrections
```

代码里可以看到它不是把全文直接塞给模型。

它先做 smart boundary extraction：

```text
prefix window: max 500 chars, model sees about 300 chars
suffix window: max 300 chars, model sees about 150 chars
cut by paragraph boundary, sentence boundary, LaTeX section boundary, whitespace
```

然后做 scenario detection：

```text
word completion
sentence continuation
fix / correction
skip
```

最后用 deterministic post-processing 处理：

```text
boundary fix
diff computation
prefix_diff
inserted_text
suffix_diff
```

这给我们的启发是：

```text
AI writing 的第一层不一定是重 agent。
很多时候应该先做高频、低成本、低阻断的 completion。

在 Quant Research OS 中，对应能力可以是：
factor hypothesis completion
experiment note completion
backtest report paragraph completion
research memo completion
code comment / docstring completion
```

## Ask Mode 与 Agent Mode

Litewrite 的 Chat Agent 有两个模式：

```text
ask   -> read-only Q&A
agent -> read + edit
```

MainAgent 的工具权限是按 mode 划分的：

| Mode | Tools |
|---|---|
| Ask | `read_file`, `list_files`, `web_search`, `arxiv_search`, `plan`, `done` |
| Agent | Ask tools + `edit_file`, `task` |

这个设计很重要。

因为写作 agent 如果没有权限边界，很容易变成危险的自动改文件工具。
Litewrite 的模式划分把“读”和“写”拆开：

```text
问问题、理解项目、找资料 -> ask mode
真正改文件、重写章节、生成内容 -> agent mode
```

对我们的 Research OS 来说，同样应该有：

```text
read-only research analyst mode
editable draft writer mode
experiment execution mode
production / trading forbidden mode
PM approval required mode
```

权限模型不是后端细节。
它是 agent 产品能不能可靠落地的核心。

## MainAgent + SubAgent

Litewrite 的 AI 写作架构是：

```text
ChatService
  -> MainAgent
      -> tools
      -> task tool
          -> ReadAgent
          -> EditAgent
          -> ResearchAgent
```

三个 SubAgent 的分工很清楚：

| SubAgent | Purpose | Tools |
|---|---|---|
| ReadAgent | 文件阅读和结构分析 | `read_file`, `list_files`, `done` |
| EditAgent | 复杂多段编辑 | `read_file`, `edit_file`, `done` |
| ResearchAgent | 网页和学术搜索 | `web_search`, `arxiv_search`, `done` |

MainAgent 不需要自己承担所有动作。
复杂任务可以通过 `task` tool 委托给专门的 subagent。

这和我们前面看的 ClawTeam / FastAgent 是一条线：

```text
ClawTeam  -> 多 agent 组织层
FastAgent -> planner / executor / evaluator 执行层
Litewrite -> writing workspace 里的 MainAgent / SubAgent 分工
```

对 Pengyi Research OS 可以直接映射：

```text
PM Agent
Research Reader Agent
Factor Hypothesis Agent
Backtest Agent
Bias Diagnosis Agent
Report Writer Agent
Reviewer Agent
```

写作不是一个 agent 独立完成。
写作应该是多个专门 agent 在同一个 artifact workspace 里协作。

## Tool Layer

Litewrite 的工具层是统一抽象：

```text
Tool
  name
  description
  parameters
  mode
  execute(params, context)

ToolContext
  project_id
  user_id
  mode
  direct_apply
  SSE emitter
  usage accumulator
```

内置工具包括：

```text
read_file
list_files
edit_file
web_search
arxiv_search
plan
task
done
```

这里最值得注意的是：AI server 不直接摸前端存储。

它通过 HTTP 调 Next.js internal API：

```text
read_file -> /api/internal/files/read
edit_file -> /api/internal/files/write or edit
```

所有 internal API 通过：

```text
X-Internal-Secret
```

做服务间认证。

这个边界设计比较干净：

```text
AI server 负责 reasoning
Next.js 负责项目、存储、权限、shadow doc、Yjs state
Compile server 负责 TeXLive 编译
```

对 Quant Research OS 来说，这就是一个很好用的分层模板：

```text
Research Agent 不应该直接操作所有数据库。
它应该通过受控 internal API 调 data service、backtest service、risk service、report service。
```

## Shadow Document Review Flow

Litewrite 最关键的产品安全设计之一是 shadow document。

源码里写得很清楚：

```text
Layer 1: editor-visible content, Yjs realtime sync
Layer 2: Shadow Doc, private to user, context for LLM
Layer 3: original document, Yjs shared by all users
```

AI edits 默认不是直接覆盖原文。

流程是：

```text
1. AI 读取最新 effective content
2. LLM 生成 line-based edit blocks
3. Next.js 把 edits 应用到 user-private shadow document
4. 对 original doc 和 shadow doc 计算 diff blocks
5. diff blocks 用 RelativePosition 追踪实时位置
6. 前端展示 pending edits，用户审核
```

这比“AI 直接改文件”更适合多人协作和严肃写作。

它保留了 human approval：

```text
AI proposes
human reviews
workspace records diff
then merge
```

但还有一个特殊通道：

```text
directApply
```

在普通 `/api/chat/run` 中，`directApply` 只有 internal secret 认证后才会生效。
在 `/api/chat/run-sync` 中，因为它用于 nanobot / internal caller，默认走 direct apply，并要求 `X-Internal-Secret`。

这说明项目作者意识到了一个关键问题：

```text
自动写入是高权限能力，必须限制入口。
```

对我们的 Quant Research OS 更关键。

未来如果 agent 能改策略、改参数、改配置、改交易计划，就必须有：

```text
shadow proposal
diff review
PM approval
audit log
permission gate
```

这和我们之前说的“人类 PM 审核”完全一致。

## Yjs Collaboration

Litewrite 使用 Yjs 做实时协作。

WebSocket server 负责：

```text
document sync
awareness update
project chat rooms
Yjs document restore
Redis incremental persistence
RelativePosition support
vibe lock
line-level lock
```

Redis persistence 的 key 设计也考虑了 path collision 和 URL encoding：

```text
yjs:{projectId}:{base64url(fileId)}:updates
yjs:{projectId}:{base64url(fileId)}:meta
```

这说明它已经遇到或预防了真实产品问题：

```text
中文文件名
路径冲突
服务重启后恢复
RelativePosition 持久化
旧 key 兼容迁移
```

对我们来说，Research OS 也会面对类似问题：

```text
多文件 project
多 agent 同时写报告
不同 artifact 的版本
experiment state 恢复
用户和 agent 的编辑冲突
```

因此协作状态层不能靠临时 JSON 文件长期支撑。
未来需要明确的 document state / artifact state / run state。

## Compile Server

Litewrite 的 compile server 是 Node.js + Express。

它接收：

```text
mainFile
projectFiles
```

然后用 TeXLive 编译输出 PDF。

它有一组现实工程约束：

```text
compile timeout: 300s
max concurrent compiles: 10
max queue size: 30
queue timeout: 60s
max request size: 200MB
max single file size: 50MB
max file count: 1000
```

同时还有 LaTeX 风险检查：

```text
\write18
\ShellEscape
absolute file read / write
/etc/passwd and sensitive paths
directlua os / io / debug / ffi
```

并且真正执行时依赖 `-no-shell-escape` 这类 sandbox 约束，而不是只相信 regex。

这对我们也有启发：

```text
任何能执行代码、编译、回测、拉数据、跑交易模拟的服务，都要有 timeout、queue、size limit、sandbox、error code。
```

研究系统不是只追求“能跑”。
一旦产品化，就必须能被限制、观察、排队、失败恢复。

## Deep Research

Litewrite 的 Deep Research 是一个独立服务：

```text
/api/deep-research/stream
```

输入包括：

```text
query
arxiv_papers
web_pages
structured
max_iterations
projectId
```

输出是 SSE stream：

```text
progress
search_start
search_result
analysis
iteration
outline_start / outline_done
section_start / section_chunk / section_done
report_start / report_chunk / report_done
done
```

也就是说，它不是一次性吐一个 report。
它把 research process 显式流式化：

```text
搜索
分析 gap
迭代
规划 outline
分 section 写
生成 references
生成 BibTeX
```

这非常适合写作产品。

因为用户在等待 research report 时，不只想看到 spinner。
用户需要看到：

```text
agent 现在在查什么
找到多少 paper
用了哪些 web sources
outline 是什么
哪个 section 正在写
最后 references 和 BibTeX 是什么
```

对 Quant Research OS，对应就是：

```text
factor research stream
data search stream
backtest stream
bias diagnosis stream
risk check stream
report generation stream
```

过程可见本身就是产品能力。

## nanobot: 手机和 IM 入口

Litewrite 集成了 nanobot。

nanobot 把 Telegram / Feishu / Lark 接到 Litewrite：

```text
user message
  -> nanobot agent loop
  -> Litewrite internal API
  -> AI server / compile server / project APIs
  -> reply text or PDF back to user
```

README 说它支持 20 个 Litewrite-specific tools。

但当前源码里有一个很有意思的收敛：

```text
nanobot 只注册 manager-level tools。
文件阅读、分析、写作、编辑尽量委托给 litewrite_agent。
```

也就是说，nanobot 自己更像 Manager：

```text
list projects
invoke litewrite_agent
compile PDF
create / delete / rename project
version list / save / restore
upload / create file
import arXiv / GitHub / uploaded archive
deep research
```

而复杂读写交给：

```text
litewrite_agent
```

调用链是：

```text
nanobot litewrite_agent
  -> Next.js /api/internal/agent/run
  -> AI server /api/chat/run-sync
  -> ChatService.run_sync
  -> MainAgent / SubAgent / Tools
```

它还会按 project cache session：

```text
project_id -> session_id
```

这样同一个项目的连续对话能复用上下文，并在 Litewrite Web UI 里显示为：

```text
nanobot
```

这个设计很适合我们。

未来 Pengyi Research OS 可以有：

```text
网页端 workspace
GitHub repo
本地 CLI
Telegram / Feishu / WeChat-style assistant
```

手机上可以做轻量操作：

```text
列出项目
查看今天 research plan
让 agent 编译报告
问某个 factor 的最新 backtest
让 agent 生成一页 summary
把 PDF 发回来
```

这就是现实生产力工具。

## 数据模型与版本系统

Prisma schema 里已经有：

```text
User
Project
ProjectCollaborator
Tag
ProjectTag
ChatMessage
UserSettings
ProjectVersion
Blob
Tree
TreeEntry
Template
TemplateFavorite
```

特别值得注意的是版本系统：

```text
Blob
Tree
TreeEntry
ProjectVersion
```

这更像 Git-style Merkle Tree：

```text
content-addressable blob
tree hash
rootTreeHash
parentHash
fileCount
totalSize
```

这比简单 zip snapshot 更适合长期项目。

对 Research OS 来说，版本系统非常关键。

我们不是只要保存文件。
我们要保存：

```text
research artifact version
experiment version
report version
dataset snapshot reference
model / prompt / parameter version
human approval version
```

Litewrite 的版本系统可以作为一个很好的参考。

## 对 Pengyi Research OS 的启发

Litewrite 对我们的启发可以总结成一句话：

```text
Research OS 必须有一个严肃的 output workspace。
```

我们前面一直在看：

```text
agent organization
agent execution
quant reasoning
knowledge graph
RAG
recommendation
forecasting
```

但如果没有 output workspace，研究会散掉。

我们需要一个地方承接：

```text
paper note
daily research note
quant factor memo
backtest report
bias diagnosis report
weekly review
application CV / PS / RP
PI email draft
technical blog
open-source README
conference submission draft
```

Litewrite 给了一个很完整的产品形态：

```text
写作 workspace
AI completion
AI agent editing
Deep Research
LaTeX compile
versioning
review flow
collaboration
mobile bot
```

这正好是我们需要补上的“产出层”。

## Quant Research OS 映射

如果把 Litewrite 映射到我们的量化系统，可以这样看：

| Litewrite | Quant Research OS |
|---|---|
| LaTeX project | strategy research project |
| `main.tex` | strategy report / experiment spec |
| TAP completion | factor hypothesis / report paragraph completion |
| Ask mode | read-only quant analyst |
| Agent mode | editable report / experiment writer |
| Deep Research | market / paper / factor literature research |
| `read_file` | read factor code / data schema / experiment result |
| `edit_file` | edit factor implementation / report / config |
| shadow doc | proposed strategy diff |
| pending edits | PM review queue |
| compile server | backtest / report render service |
| PDF output | quant memo / investment committee memo |
| nanobot | mobile quant assistant |
| version history | experiment lineage |

一个现实流程可以是：

```text
1. PM 提出研究方向
2. Research Agent 查 paper / news / filings / factor zoo
3. Factor Agent 生成 hypothesis
4. Develop Agent 实现代码
5. Backtest Agent 跑实验
6. Diagnosis Agent 检查 look-ahead、survivorship、turnover、capacity、cost
7. Report Agent 生成 quant memo
8. Human PM 审核 shadow proposal
9. 通过后进入下一轮研究计划
```

Litewrite 主要覆盖第 7 和第 8 层：

```text
report generation
artifact review
human approval
versioned output
```

但它的架构可以反推整个 Research OS。

## 可以快速应用的方向

我们现在不一定要马上复刻 Litewrite。

更现实的路线是先吸收它的产品骨架：

```text
1. 给我们的网站和 private workspace 建一个 writing pipeline
2. 把 HKUDS / LLMQuant 学习笔记变成 structured writing project
3. 把 quant interview notes / RA pitch / PI emails / project reports 都归档
4. 每个 research project 都有 report.md / plan.md / experiment_log.md
5. 让 agent 自动生成 draft，但必须保留 review / diff / version
6. 未来再接入 LaTeX / PDF compile / mobile bot
```

短期最值得做的不是“搭一个完整 Overleaf”。

短期最值得做的是：

```text
把我们的输出资产标准化。
```

例如：

```text
Pengyi Research OS project/
  README.md
  research_plan.md
  literature_review.md
  experiment_log.md
  backtest_report.md
  bias_diagnosis.md
  next_round_plan.md
  public_safe_summary.md
```

然后让 agent 围绕这些文件工作。

## PR / Improvement Opportunities

从源码阅读角度，Litewrite 可能有一些适合 contributor 的切入点。

第一类是文档：

```text
补一张 service architecture diagram
补 internal API contract table
补 AI tool schema docs
补 Ask mode / Agent mode 权限说明
补 shadow document review flow 图
补 nanobot manager-level tools 和 litewrite_agent delegation 的说明
```

第二类是开发者体验：

```text
增加 health check checklist
增加 local setup troubleshooting
增加 .env 字段解释
增加 ai-server / compile-server / ws-server 的最小 smoke test
```

第三类是 Research OS 模板：

```text
新增 research report template
新增 paper review template
新增 grant / proposal template
新增 quant memo template
新增 thesis / RA application template
```

第四类是安全和可靠性：

```text
检查 internal secret 在所有 internal route 是否一致
检查 directApply 是否只有 internal caller 能触发
检查 file path validation 是否覆盖所有写接口
补充 compile server security docs
补充 shadow document edge cases tests
```

这些 PR 不需要为了 PR 而 PR。

更好的方式是：

```text
先真实使用。
记录问题。
提出 issue。
给小而准的文档或测试 PR。
```

这也是我们建立开源信用的正确路径。

## 与前几篇的关系

现在 Agent Product / Workspace 系列的逻辑越来越清楚：

```text
HKUDS033 ClawTeam
  -> agent 怎么组成 team

HKUDS034 ClawWork
  -> agent work 怎么被定价、评估和追踪

HKUDS035 FastAgent
  -> agent 怎么规划、执行、验证、调用工具

HKUDS036 Litewrite
  -> agent 产出的 research artifact 放在哪里、怎么写、怎么审、怎么编译、怎么版本化
```

这四篇合起来，对 Pengyi Research OS 的架构启发是：

```text
Organization layer
Economic accountability layer
Execution engine layer
Writing / artifact workspace layer
```

下一篇 `HKUDS037` 如果继续按路线图走，可以看：

```text
OpenPhone
```

它会更偏现实工具和业务入口：

```text
AI phone / communication workflow
agent 与真实外部人、组织、业务连接
```

而 Litewrite 先把“研究输出层”补上了。

这一步很重要。

因为我们最终要成为的不是只会跑代码的人。
我们要构建的是：

```text
能研究
能开发
能写作
能发布
能协作
能被审阅
能持续产出 public assets 的 AI scientist operating system
```

