---
title: "HKUDS009: DeepCode 作为 Paper2Code 与 Agentic Coding Implementation Layer"
date: 2026-06-25 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds009, hkuds, deepcode, paper2code, agentic-coding, mcp, code-rag, research-to-code, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第十篇。

```text
HKUDS009 -> DeepCode
```

到目前为止，HKUDS 第一阶段我们已经看了：

```text
HKUDS000 -> study map
HKUDS001 -> LightRAG
HKUDS002 -> Vibe-Trading
HKUDS003 -> nanobot
HKUDS004 -> CLI-Anything
HKUDS005 -> AI-Trader
HKUDS006 -> AgentSpace
HKUDS007 -> RAG-Anything
HKUDS008 -> AutoAgent
```

现在来看 `DeepCode`。我对它的定位是：

```text
DeepCode = Paper2Code + Agentic Coding Implementation Layer
```

如果说前面的几个项目分别解决：

```text
RAG-Anything = multimodal document ingestion
LightRAG     = research memory
Vibe-Trading = quant research workflow
nanobot      = personal agent shell
CLI-Anything = software action layer
AI-Trader    = live trading platform layer
AgentSpace   = organizational agent workspace
AutoAgent    = self-developing agent factory
```

那么：

```text
DeepCode = 把论文、文档、URL、自然语言需求转成可运行代码项目的 implementation engine
```

这件事非常关键。

因为 Pengyi Research OS 最终不是只要读论文、存知识、生成想法，而是要把想法落到代码、实验、回测、报告和可复现工程里。

也就是：

```text
paper / idea / requirement
  -> plan
  -> code architecture
  -> reference mining
  -> implementation
  -> testing
  -> report
  -> next research plan
```

DeepCode 站在这个链路的中后段。它不是单纯聊天式 coding assistant，而是一个面向 research-to-code 的多 agent 工程系统。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `DeepCode`。

| Item | Value |
|---|---|
| repo | `DeepCode` |
| remote | `https://github.com/HKUDS/DeepCode.git` |
| branch | `main` |
| local head | `5fbfa27` |
| latest local commit date | `2026-05-18` |
| latest local commit | `Update README.md` |
| package | `deepcode-hku` |
| package version | `1.2.0` |
| console command | `deepcode` |
| positioning | `Open Agentic Coding` |
| core promise | transform ideas, papers, URLs, and requirements into production-ready code |

本地核心目录：

```text
DeepCode/
  cli/
  config/
  core/
  deepcode_docker/
  nanobot/
  new_ui/
  prompts/
  schema/
  tests/
  tools/
  ui/
  utils/
  workflows/
  deepcode.py
  deepcode_config.json.example
  nanobot_config.json.example
  requirements.txt
  setup.py
```

这个结构已经说明它不是一个小 demo，而是一个完整应用：

```text
launcher + CLI + Web UI + backend API + workflows + MCP tools + config runtime + observability + Docker + nanobot integration
```

## 一句话理解

DeepCode 最核心的价值是：

```text
把“研究材料”和“工程交付”之间的空白，用多 agent coding workflow 补上。
```

过去我们读一篇论文，真正做复现通常要经历：

```text
读论文
理解任务
拆模块
找参考代码
搭文件结构
写代码
修环境
跑实验
补文档
定位 bug
反复重构
```

DeepCode 想把这条链路产品化：

```text
input paper / URL / requirement
  -> document parsing
  -> research analysis
  -> planning
  -> reference retrieval
  -> code implementation
  -> task logs and session persistence
  -> output code project
```

这就是为什么它对我们重要。我们做 AI scientist，不可能只停留在想法层。顶会级工作一定要进入：

```text
hypothesis -> system -> experiment -> ablation -> benchmark -> paper -> open-source release
```

DeepCode 对应的就是中间的 `system / experiment / open-source release` 生产层。

## Project 用途

DeepCode 的用途可以分三类。

第一类是 `Paper2Code`：

```text
paper
  -> understand method
  -> extract algorithm
  -> generate reproduction plan
  -> implement codebase
```

这是最 research-native 的场景。输入论文，输出可运行代码。对我们未来做顶会复现、顶会 baseline、quant factor paper reproduction、AI4Finance research reproduction 都有直接启发。

第二类是 `Text2Web`：

```text
natural language requirement
  -> frontend / backend architecture
  -> implementation
  -> runnable web app
```

这对应的是产品原型能力。我们以后做 research demo、实验 dashboard、quant research workspace，都需要快速把想法做成可展示的 interface。

第三类是 `Text2Backend`：

```text
requirement
  -> API design
  -> backend services
  -> file / data / task management
```

这对 Research OS 更关键。真正的 research platform 不是只靠 notebook，而是需要：

```text
task queue
experiment storage
dataset registry
backtest service
artifact tracking
report generation
permission boundary
```

DeepCode 的后端生成能力，给我们提供了一个可以学习的 implementation layer。

## 实现方式总览

DeepCode 的实现方式可以压成一条主链路：

```text
Input Layer
  -> Requirement / Document Analysis
  -> Planning Runtime
  -> Reference Mining / Code Indexing
  -> MCP Tool Execution
  -> Code Implementation Workflow
  -> Session / Logging / WebSocket Observability
  -> UI / CLI / nanobot Entry Points
```

拆开看：

| Layer | What it does |
|---|---|
| Input layer | 接收 PDF、URL、文件、自然语言需求 |
| Analysis layer | 解析论文和需求，抽取实现目标 |
| Planning layer | 生成结构化 implementation plan |
| Reference layer | 下载、索引、搜索参考仓库 |
| Tool layer | 通过 MCP server 执行文件、命令、下载、代码检索 |
| Implementation layer | 逐文件生成代码、检测进度、输出状态 |
| Runtime layer | 管理 provider、model、token、phase-specific LLM |
| Observability layer | session、task logs、WebSocket streaming |
| Interface layer | Docker、local Web UI、CLI、nanobot |

它的核心不是“一个大 prompt”，而是把 workflow 拆成多个工程组件。

## 关键组件 1: Launcher

入口文件是：

```text
deepcode.py
```

它负责启动模式选择：

```text
deepcode            -> Docker mode
deepcode --docker   -> Docker mode
deepcode --local    -> local backend + frontend
deepcode --cli      -> Docker CLI
deepcode --classic  -> classic Streamlit UI
```

本地 UI 主要端口：

```text
frontend -> http://localhost:5173
backend  -> http://localhost:8000
docs     -> http://localhost:8000/docs
```

这说明 DeepCode 已经把 agentic coding 做成了一个真实产品，而不是只在脚本里跑。

对我们有启发：

```text
Pengyi Research OS 也应该同时有 CLI + Web UI + background task service。
```

CLI 适合自己高频研究，Web UI 适合展示、演示、PM 审核、未来团队协作。

## 关键组件 2: New UI Backend

后端入口：

```text
new_ui/backend/main.py
```

它是一个 FastAPI backend，核心路由包括：

```text
/api/v1/workflows
/api/v1/requirements
/api/v1/config
/api/v1/files
/api/v1/sessions
```

WebSocket 路由包括：

```text
/ws/workflow
/ws/code_stream
/ws/logs
```

这很重要。Agentic coding 任务通常是长任务，不适合普通 request-response。一个 paper-to-code workflow 可能跑很久，需要：

```text
status tracking
progress streaming
log streaming
user-in-loop confirmation
cancel
resume
```

DeepCode 用 FastAPI + WebSocket 做了这层任务服务化。

这对我们未来做 Quant R&D Agent 非常关键。因为回测、数据清洗、因子实验也都是长任务：

```text
factor idea
  -> implementation
  -> backtest
  -> diagnostics
  -> report
```

每一步都应该有状态、日志、可中断、可恢复。

## 关键组件 3: Workflow API

工作流路由在：

```text
new_ui/backend/api/routes/workflows.py
```

主要 endpoint：

```text
POST /paper-to-code
POST /chat-planning
GET  /status/{task_id}
POST /cancel/{task_id}
POST /respond/{task_id}
GET  /interaction/{task_id}
GET  /active
GET  /recent
```

这里可以看出 DeepCode 的产品抽象：

```text
workflow is a task
task has status
task can wait for human response
task can be cancelled
task can be resumed / inspected
```

这就是我们一直说的人类 PM 审核。

对 Research OS 来说，未来可以直接借鉴这个 API 形态：

```text
POST /factor-research
POST /paper-reproduction
POST /backtest
POST /diagnose-bias
POST /next-plan
POST /respond/{task_id}
```

人类不需要干所有细活，但必须在关键节点把关：

```text
approve plan
reject invalid assumption
choose dataset
choose benchmark
decide whether to continue
```

## 关键组件 4: Agent Orchestration Engine

核心 orchestration 在：

```text
workflows/agent_orchestration_engine.py
```

文件开头直接写清楚了角色：

```text
Research Analysis Agent
Workspace Infrastructure Agent
Code Architecture Agent
Reference Intelligence Agent
Repository Acquisition Agent
Codebase Intelligence Agent
Code Implementation Agent
```

这是 DeepCode 的主脑。

它做的不是“一次 prompt 生成代码”，而是先分析、再规划、再找参考、再进入 implementation workflow。

可以理解成：

```text
orchestrator
  -> analyze research material
  -> prepare workspace
  -> generate plan
  -> retrieve references
  -> index code
  -> implement
  -> report
```

这个设计对我们非常重要。

未来我们的 Quant R&D Agent 也应该拆成类似结构：

```text
Factor Hypothesis Agent
Data Availability Agent
Research Plan Agent
Implementation Agent
Backtest Agent
Bias Diagnosis Agent
Report Agent
Next-Round Planning Agent
Human PM Gate
```

重点不是 agent 名字，而是职责边界要清楚。

## 关键组件 5: Planning Runtime

DeepCode 的 planning 不是随便输出一段文字。它有：

```text
workflows/planning_runtime.py
workflows/plan_review_runtime.py
```

在 orchestration engine 里可以看到：

```text
validate_plan_text
coerce_text_to_minimal_plan
read_planning_meta
write_planning_meta
append_planning_attempt
run_plan_review_gate
```

这说明它对 plan 有结构化要求，并且会处理 LLM 输出不稳定的问题。

比如：

```text
LLM 没有按格式输出
LLM deferred planning
LLM tool-call 行为不符合预期
plan validation failed
plan review cancelled
```

DeepCode 的处理方式是：

```text
retry
validate
fallback to minimal valid plan
human review gate
persist planning metadata
```

这正是严肃 agent 系统必须做的事情。因为 LLM workflow 不能假设每次都会输出完美结果。

对我们未来做 research agent 的启发：

```text
每一次 research plan 都应该可验证、可保存、可审查、可回退。
```

## 关键组件 6: Code Implementation Workflow

代码实现层在：

```text
workflows/code_implementation_workflow.py
```

核心类：

```text
CodeImplementationWorkflow
```

它做几件事：

```text
read implementation plan
create file tree
run pure code implementation loop
track progress
detect loops
return truthful status
```

这里有一个非常值得注意的工程细节：它不是无条件返回成功，而是返回更细的状态：

```text
status
inner_status
abort_reason
files_completed
total_files
unimplemented_files
iterations
elapsed_seconds
```

这点很关键。很多 agent demo 最大的问题是“看上去做完了”，但实际文件没写完、测试没过、逻辑缺失。

DeepCode 在 implementation workflow 里保留：

```text
completed
incomplete
error
abort reason
pending files
```

这才是可以进入严肃工程系统的写法。

对 Quant R&D Agent 来说也一样：

```text
backtest completed != research valid
factor implemented != signal useful
report generated != conclusion reliable
```

我们也需要返回真实状态：

```text
data_missing
lookahead_risk_detected
universe_mismatch
turnover_too_high
transaction_cost_not_applied
benchmark_invalid
experiment_incomplete
```

## 关键组件 7: MCP Tool Layer

DeepCode 的工具层主要由 MCP servers 组成。配置在：

```text
deepcode_config.json.example
```

核心 MCP server：

| MCP server | Purpose |
|---|---|
| `code-implementation` | workspace 内文件读写、代码执行、搜索 |
| `code-reference-indexer` | 索引和搜索参考代码仓库 |
| `command-executor` | 执行 shell 命令 |
| `document-segmentation` | 大文档分段分析 |
| `fetch` | web content retrieval |
| `file-downloader` | 下载和转换 PDF / DOCX 等文件 |
| `filesystem` | 文件系统访问 |
| `github-downloader` | 下载 GitHub 仓库 |

这就是 DeepCode 的行动能力来源。

LLM 本身不会“真的做工程”。真正做工程需要：

```text
read files
write files
run commands
download references
index code
parse documents
stream logs
inspect outputs
```

MCP layer 把这些动作标准化。

对我们未来的 Research OS 来说，也应该有一组 quant-native MCP tools：

```text
market-data-loader
factor-library
backtest-runner
risk-diagnostics
portfolio-constructor
experiment-registry
report-generator
paper-loader
codebase-indexer
```

这样 agent 才不是只会聊天，而是能真实操作研究资产。

## 关键组件 8: Command Executor

命令执行工具在：

```text
tools/command_executor.py
```

这个文件有一个非常实际的点：它把常见 Unix file-tree 命令转成跨平台 native 操作。

例如：

```text
mkdir -p
touch
rm -rf
cp -r
mv
```

会通过 `pathlib` / `shutil` 在 Windows 上执行。

这点对我们当前环境尤其重要，因为我们主要在 Windows workspace 里做研究。

这说明 DeepCode 不是只为 Linux demo 写的，它在修真实用户会遇到的问题：

```text
Windows encoding
path compatibility
shell command portability
native file operations
```

这也提醒我们：如果要把 Pengyi Research OS 做成长期项目，Windows / Linux / Docker 的兼容性必须早处理。

## 关键组件 9: Config Runtime

配置文件：

```text
deepcode_config.json.example
```

它是 single source of truth。主要配置块：

```text
agents
providers
tools.mcpServers
workspace
documentSegmentation
logger
llmLogger
```

Provider 包括：

```text
openai
anthropic
openrouter
gemini
deepseek
zhipu
dashscope
ollama
vllm
```

DeepCode 还把 planning 和 implementation 分开配置：

```text
agents.defaults
agents.planning
agents.implementation
```

这很对。因为 planning 和 implementation 需要的模型能力不同：

```text
planning       -> reasoning, structure, research understanding
implementation -> long-context coding, tool use, repair loop
```

对我们未来做 Quant R&D Agent，也应该分 phase 选模型：

```text
hypothesis generation -> creative reasoning model
data inspection       -> tool-using model
implementation        -> coding model
diagnostics           -> statistical reasoning model
report writing        -> writing model
```

不要把所有任务都塞给同一个 agent / model。

## 关键组件 10: Document Segmentation

DeepCode 有大文档分段能力：

```text
documentSegmentation.enabled = true
documentSegmentation.sizeThresholdChars = 50000
```

对应工具：

```text
tools/document_segmentation_server.py
workflows/agents/document_segmentation_agent.py
```

这说明它考虑到了论文和长文档 context 太长的问题。

实际 research-to-code 场景里，论文可能包含：

```text
abstract
method
algorithm
theory
experiments
appendix
tables
implementation details
hyperparameters
```

不是所有内容都同等重要。做 code reproduction 时，最重要的是：

```text
method
algorithm
loss function
data processing
training protocol
evaluation metric
hyperparameters
ablation setup
```

DeepCode 的分段策略，就是为了让 planner 抽取最相关内容，而不是被整篇论文淹没。

这对我们处理 WorldQuant factor library、research paper、broker report、macro report 也有启发。

未来我们可以做：

```text
financial-document-segmentation
  -> factor idea section
  -> data requirement section
  -> universe / rebalance section
  -> risk section
  -> implementation details
  -> caveats
```

## 关键组件 11: Code Reference Indexer

DeepCode 不是只凭空写代码，它有：

```text
code-reference-indexer
github-downloader
file-downloader
```

这意味着它会主动利用外部参考。

对 research reproduction 来说，这很合理。很多论文复现不是从零开始，而是：

```text
find official repo
find similar repo
inspect code structure
borrow implementation pattern
adapt to target method
```

这也是优秀工程师真实工作的方式。

DeepCode 把这个过程 agent 化：

```text
reference mining
  -> repository acquisition
  -> codebase intelligence
  -> implementation plan
```

对我们未来提 PR、读开源项目、做自己的 Research OS 都有直接价值。

我们可以把这条链路迁移到量化研究：

```text
research question
  -> search related papers
  -> search open-source baselines
  -> index implementation
  -> compare assumptions
  -> implement our version
  -> run benchmark
```

## 关键组件 12: Session and Logging

DeepCode 在 2026-04-28 的更新里强调了 persistent sessions 和 dual-layer logging。

本地 README 里描述：

```text
sessions -> ~/.deepcode/sessions/<id>/
global logs -> logs/server-YYYYMMDD.jsonl
per-task logs -> deepcode_lab/tasks/<task_id>/logs/{system,llm,mcp}.jsonl
WebSocket log streaming
```

这很重要。

长期 research workflow 最大的问题之一是不可追踪：

```text
之前跑了什么？
用了哪个模型？
哪个 plan 被审核过？
哪个文件是 agent 写的？
哪一步失败？
为什么失败？
能不能 resume？
```

DeepCode 的 session/log 设计在解决这些问题。

对 Pengyi Research OS 来说，我们也必须保存：

```text
research idea
plan
data version
code version
backtest config
model/tool calls
diagnostic result
human decision
final report
next action
```

否则长期研究会变成大量不可复现的聊天记录。

## 关键组件 13: CLI

CLI 入口：

```text
cli/main_cli.py
```

它支持：

```text
--optimized
--no-plan-review
--disable-segmentation
--segmentation-threshold
--session
--session-title
```

也支持 session 命令：

```text
deepcode session list
deepcode session show <id>
deepcode session new
deepcode session resume <id>
deepcode session delete <id>
```

交互模式里也支持类似：

```text
/resume
/new
/session
```

文件输入可以用：

```text
@/path/to/paper.pdf
@"C:\path with spaces\paper.pdf"
@https://...
```

这说明 DeepCode 把 CLI 当成一等入口，而不是 Web UI 的附属品。

这对我们很对。我们每天高强度 coding 和研究，CLI 是最高效的生产入口。

未来 Pengyi Quant Research OS 应该也有：

```text
pengyi factor new
pengyi factor backtest
pengyi paper reproduce
pengyi report generate
pengyi run resume
pengyi task logs
pengyi pm review
```

## 关键组件 14: nanobot Integration

DeepCode 还有 nanobot integration。

这意味着 agentic coding 不一定只能在 IDE 或浏览器里发生，也可以通过聊天入口触发。

它的想法可以理解为：

```text
chat naturally
  -> create coding task
  -> run DeepCode workflow
  -> receive progress / result
```

这与我们前面 HKUDS003 对 nanobot 的定位正好连起来：

```text
nanobot = personal agent shell
DeepCode = coding implementation engine
```

组合起来就是：

```text
phone / chat / CLI / Web UI
  -> trigger research-to-code task
  -> agent implements
  -> human reviews
```

这正是未来个人 AI scientist 工作台的形态。

## DeepCode 和 AutoAgent 的区别

AutoAgent 和 DeepCode 听起来都和 agent 生成有关，但它们的核心位置不同。

| Project | Core Question |
|---|---|
| AutoAgent | 如何从自然语言需求创建 tools / agents / workflows |
| DeepCode | 如何从论文 / 文档 / 需求实现出可运行代码项目 |

AutoAgent 更像：

```text
agent factory
```

DeepCode 更像：

```text
implementation factory
```

AutoAgent 生成的是 agent 和 workflow。

DeepCode 生成的是工程代码、项目结构和 research reproduction 产物。

在 Pengyi Research OS 里可以这样放：

```text
AutoAgent -> 生产研究 agent 和 workflow
DeepCode  -> 生产代码项目和实现结果
```

组合起来：

```text
human PM
  -> describe research workflow
  -> AutoAgent creates agents/tools/workflow
  -> DeepCode implements required codebase
  -> Vibe-Trading / AI-Trader runs quant workflow
  -> LightRAG / RAG-Anything store and retrieve knowledge
```

## DeepCode 和 RAG-Anything / LightRAG 的关系

RAG-Anything 解决输入：

```text
PDF / Office / image / table / formula / multimodal document
  -> structured multimodal content
```

LightRAG 解决记忆：

```text
knowledge graph + vector retrieval
  -> reusable research memory
```

DeepCode 解决实现：

```text
research material + plan
  -> runnable code project
```

三者连起来就是：

```text
RAG-Anything -> ingest documents
LightRAG     -> organize and retrieve knowledge
DeepCode     -> implement code and experiments
```

这就是 Research OS 的重要骨架。

## DeepCode 和 Vibe-Trading / AI-Trader 的关系

Vibe-Trading 更偏向：

```text
strategy idea -> trading workflow -> backtest / execution loop
```

AI-Trader 更偏向：

```text
agent-native live trading platform
```

DeepCode 可以作为它们上游的 implementation engine：

```text
factor paper / strategy idea
  -> DeepCode implements prototype
  -> Vibe-Trading runs research workflow
  -> AI-Trader handles live platform / competition / agent operations
```

对我们未来做 quant system，这个组合非常清晰：

```text
DeepCode = 写代码
Vibe-Trading = 组织策略研究
AI-Trader = 平台化和实盘化
LightRAG = 研究记忆
RAG-Anything = 文档入口
AgentSpace = 多 agent 组织层
```

## 对 Pengyi Research OS 的直接启发

我认为 DeepCode 对我们的启发可以落成五条。

第一，Research OS 必须有 `implementation layer`。

```text
只生成 idea 不够。
只生成 plan 不够。
必须能生成代码、运行实验、定位失败、输出报告。
```

第二，planning 和 implementation 要分开。

```text
planner 决定做什么
implementation agent 决定怎么写
human PM 决定是否继续
```

第三，所有长任务必须 task 化。

```text
task_id
status
logs
cancel
resume
recent
active
interaction
```

第四，工具层必须标准化。

```text
MCP servers / tools are the real hands of the agent.
```

第五，必须留下研究轨迹。

```text
session
plan
logs
files
reports
human decisions
```

这是我们未来能够持续产出、复盘、投顶会、开源和申请 RA/PhD 的根基。

## Pengyi Quant R&D Agent 版本

如果把 DeepCode 的思想迁移到我们自己的 Quant R&D Agent，可以设计成：

```text
Input
  -> factor paper
  -> WorldQuant style factor idea
  -> broker report
  -> market observation
  -> PM instruction

Planning
  -> define hypothesis
  -> define universe
  -> define data requirement
  -> define signal formula
  -> define backtest setup
  -> define risk checks

Implementation
  -> implement factor
  -> implement cleaning
  -> implement neutralization
  -> implement backtest
  -> implement diagnostics

Evaluation
  -> IC / RankIC
  -> turnover
  -> drawdown
  -> capacity
  -> transaction cost
  -> exposure
  -> lookahead bias
  -> survivorship bias

Report
  -> research memo
  -> failure diagnosis
  -> next iteration plan
  -> human PM review
```

这就是我们一直想做的：

```text
自动提出因子假设
自动实现
自动回测
自动诊断偏差
自动生成下一轮研究计划
人类 PM 审核
```

DeepCode 给了其中 `自动实现` 和 `工程组织` 的参考答案。

## 可以学习的工程习惯

DeepCode 里面有几个非常值得我们学习的工程习惯。

第一，配置集中。

```text
deepcode_config.json = single source of truth
```

第二，phase-specific model config。

```text
planning model != implementation model
```

第三，MCP server 明确拆分。

```text
download
fetch
filesystem
command execution
code indexing
document segmentation
implementation
```

第四，task status 不说假话。

```text
success / incomplete / error
unimplemented files
abort reason
iterations
elapsed seconds
```

第五，session 和 log 先做。

```text
没有 logging 的 agent system 很难进入长期生产。
```

第六，兼容 Windows。

```text
真实用户环境比论文 demo 复杂。
```

## 可以提 PR / issue 的方向

如果我们后面真的使用 DeepCode，可以从实际使用出发提 PR，不要为了 PR 而 PR。

可能方向：

| Direction | Why it is useful |
|---|---|
| Windows setup verification | 我们当前环境就是 Windows，可以验证 README 和脚本 |
| Minimal Paper2Code demo | 做一个小论文 / 小 spec 的公开可跑 demo |
| Quant research example | 用脱敏 synthetic data 做 factor research reproduction example |
| Config docs cleanup | 把 provider / model / env var 配置讲得更清楚 |
| MCP tool tests | 给 command executor / file downloader / segmentation 加更多边界测试 |
| RAG-Anything integration note | 说明如何把多模态文档入口接到 DeepCode |
| LightRAG memory integration note | 说明如何把研究记忆接到 planning / reference retrieval |
| Honest benchmark reproduction | 如果 README claim 里有 benchmark，可以补复现脚本或复现实验说明 |

最适合我们的第一步，不是立刻大改，而是：

```text
拿一个小 paper / 小 quant research spec
  -> 跑 DeepCode
  -> 记录失败点
  -> 修文档或修 bug
  -> 提一个非常小、非常真实的 PR
```

这才是高质量 contributor 路径。

## 风险和注意事项

DeepCode 很强，但我们不能盲目使用。

第一，README 里的 benchmark claim 要当成项目方 claim，而不是我们独立验证过的结论。

比如 PaperBench 相关结果，除非我们复现实验，否则文章里应该说：

```text
project reports / README claims
```

第二，agentic coding 生成的代码必须 review。

```text
generated code is not automatically correct
```

尤其是涉及金融、交易、数据权限、真实资金、公司资料时，必须人工审查。

第三，配置里有 secrets。

```text
deepcode_config.json should not be committed
API keys should use env vars
```

第四，自动执行命令要有 workspace boundary。

DeepCode 的工具层已经有一些边界设计，但我们自己接入时也要保持：

```text
never run generated commands blindly on sensitive folders
never mix private data with public demo
never publish company or proprietary material
```

## 未来学习路线

DeepCode 后面可以继续深入几个方向：

```text
1. 跑通本地 DeepCode minimal demo
2. 阅读 workflows/agent_orchestration_engine.py 全链路
3. 阅读 workflows/code_implementation_workflow.py 的 implementation loop
4. 阅读 tools/code_implementation_server.py 的 workspace boundary
5. 阅读 tools/code_reference_indexer.py 的 reference retrieval
6. 阅读 core/config.py 和 core/llm_runtime.py 的 runtime design
7. 尝试接入一个 quant paper / factor spec
8. 总结可提 PR 点
```

如果我们做 `HKUDS009A`，可以专门做：

```text
DeepCode Code Walkthrough: agent_orchestration_engine.py
```

如果做 `HKUDS009B`，可以专门做：

```text
DeepCode Hands-on: minimal Paper2Code reproduction
```

如果做 `HKUDS009C`，可以专门做：

```text
DeepCode for Quant Research: synthetic factor implementation demo
```

## Study Map 更新

加入 DeepCode 之后，HKUDS 第一阶段地图变成：

| Index | Project | Role in Pengyi Research OS |
|---|---|---|
| HKUDS000 | Study Map | HKUDS project navigation |
| HKUDS001 | LightRAG | research memory and graph retrieval |
| HKUDS002 | Vibe-Trading | quant research workflow reference |
| HKUDS003 | nanobot | personal always-on agent shell |
| HKUDS004 | CLI-Anything | agent-native software action layer |
| HKUDS005 | AI-Trader | agent-native live trading platform layer |
| HKUDS006 | AgentSpace | organizational agent workspace |
| HKUDS007 | RAG-Anything | multimodal document ingestion layer |
| HKUDS008 | AutoAgent | self-developing agent factory |
| HKUDS009 | DeepCode | paper-to-code and research-to-code implementation layer |

`HKUDS009` 的一句话总结：

```text
DeepCode 让 Research OS 从“读懂研究”走向“实现研究”。
```

这是 AI scientist 路线里非常核心的一块。

未来真正有竞争力的不是只会读论文的人，也不是只会写 demo 的人，而是能把：

```text
paper
idea
data
system
experiment
benchmark
report
open-source release
```

全部打通的人。

DeepCode 正好站在这条链路的 implementation layer。
