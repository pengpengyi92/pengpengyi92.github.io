---
title: "HKUDS035: FastAgent 作为 DeepResearch + Computer Use 的高速 Agent Execution Engine"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds035, hkuds, fastagent, agent-product, computer-use, deepresearch, mcp, tool-rag, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS035`。

```text
HKUDS035 -> FastAgent
```

我们现在确实进入 agent 时代。

前两篇先把两个大底座看完了：

```text
HKUDS033 ClawTeam -> AI organization layer
HKUDS034 ClawWork -> AI coworker economic accountability layer
```

这一篇看：

```text
HKUDS035 FastAgent -> AI agent execution engine
```

一句话定位：

```text
FastAgent = HostAgent planning
          + GroundingAgent execution
          + EvalAgent verification
          + Kanban workflow
          + unified Shell / GUI / MCP / Web / System tool layer
          + Smart Tool RAG
          + memory compression
          + recording / audit trail
```

它的核心目标不是只做一个 chat agent。
它要解决的是更现实的问题：

```text
真实任务往往同时需要 DeepResearch 和 Computer Use。

先查资料、理解问题、找数据；
再操作网页、文件、Excel、PPT、代码、API、MCP 工具；
最后还要验证产物是否真的完成。
```

FastAgent 想把这件事变成一个：

```text
unified, simple, fast agent framework
```

对我们的启发也很直接：

```text
Pengyi Research OS / Quant Research OS 需要的不只是一个会聊天的 research agent。
我们需要一个能规划、能执行、能调工具、能看屏幕、能读文件、能调用 MCP、能验证、能记录轨迹的 agent execution substrate。
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `FastAgent`。

| Item | Value |
|---|---|
| repo | `FastAgent` |
| remote | `https://github.com/HKUDS/FastAgent.git` |
| branch | `main` |
| local head | `c7c819c` |
| full commit | `c7c819c77482102a4557a5cde793a5e255d1d034` |
| latest local commit date | `2026-02-10 11:29:43 +0800` |
| latest local commit | `feat: complete AnyTool integration` |
| Python requirement | `3.10+` in README, quick start uses `python=3.12` |
| tracked files by `rg --files` | 149 |
| Python files | 133 |
| Markdown files | 3 |
| JSON files | 5 |
| TS/JS files | 0 |
| assets | `FastAgent_framework.png`, `FastAgent_logo.png` |

项目主结构：

```text
FastAgent/
  README.md
  requirements.txt
  COMMUNICATION.md

  fastagent/
    __main__.py
    fastagent.py

    agents/
      host_agent.py
      grounding_agent.py
      eval_agent.py
      coordinator.py
      base.py
      content_processor.py
      agent_data_manager.py

    workflow/
      engine.py
      rules.py
      context_manager.py

    kanban/
      kanban.py
      enums.py

    grounding/
      core/
        grounding_client.py
        provider.py
        session.py
        search_tools.py
        tool/
        quality/
        security/
        system/
        transport/
      backends/
        shell/
        gui/
        mcp/
        web/

    memory/
      memory.py
      storage_manager.py
      summarizer.py

    prompts/
      host_agent_prompts.py
      grounding_agent_prompts.py
      eval_agent_prompts.py

    llm/
    config/
    local_server/
    recording/
    platform/
    utils/
```

核心依赖：

```text
litellm
python-dotenv
openai
anthropic
mcp
jsonschema
pydantic
requests
flask
pyautogui
pillow
```

README 里说明还需要平台相关依赖：

```text
macOS: pyobjc / atomacos
Linux: python-xlib / pyatspi / scrot
Windows: pywinauto / pywin32 / PyGetWindow
```

## 项目用途

FastAgent 面向的任务不是简单问答。
它面向的是这种端到端任务：

```text
Research industry information
Compare products / vendors / prices
Operate websites or desktop software
Generate Excel / PPT / report files
Call domain APIs or MCP servers
Verify final artifacts
```

README 给出的例子包括：

```text
Business intelligence reports
Event planning and management
Smart shopping and price optimization
AI coding assistant competitive analysis
```

它的关键假设是：

```text
DeepResearch 和 Computer Use 不应该分裂成两个系统。
```

真实任务经常是：

```text
先 research，再 operate。
先查网页，再写 Excel。
先理解资料，再操作 GUI。
先调用 MCP 工具，再生成报告。
```

FastAgent 把这些能力统一到一个 grounding layer：

```text
Shell
GUI
MCP
Web
System
```

然后用多 agent workflow 来调度。

## 为什么 FastAgent 重要

当前很多 agent framework 的问题是：

```text
1. 工具很多，但上下文塞不下
2. GUI 操作慢且容易错
3. 多步任务会累积错误
4. 不同工具后端接口不统一
5. 没有清晰的 workflow state
6. 没有中间验证，最后才发现全错
7. 没有执行轨迹，难以复盘
```

FastAgent 给出的工程答案是：

```text
用 Kanban 管状态。
用 HostAgent 做计划。
用 GroundingAgent 执行。
用 EvalAgent 验证。
用 Tool RAG 选择少量相关工具。
用 memory / content processor 控制上下文。
用 local server 抽象真实桌面和 shell。
用 recording 保存轨迹。
```

这不是“多 agent 角色扮演”。
它更像一个轻量 production workflow engine。

## 主流程

FastAgent 的主入口在：

```text
fastagent/fastagent.py
fastagent/__main__.py
```

用户可以：

```bash
python -m fastagent
python -m fastagent --query "Create a competitive analysis report..."
```

如果要使用 GUI / Shell 的本地计算机控制，需要先启动 local server：

```bash
python -m fastagent.local_server.main
```

主流程可以抽象成：

```text
1. CLI / API 接收自然语言任务
2. FastAgent.initialize()
3. 初始化 LLMClient
4. 初始化 GroundingClient 和各 backend provider
5. 初始化 AgentCoordinator
6. 创建 HostAgent / GroundingAgent / EvalAgent
7. 启动 RecordingManager
8. 启动 WorkflowEngine
9. 用户 query 变成 PLANNING card
10. WorkflowEngine 按规则触发 agent
11. 等待所有 planning / execution / response / evaluation card 完成
12. 汇总 user_response、kanban_summary、workflow_stats、failure details
```

更具体的链路：

```text
FastAgent.run(query)
  -> Kanban.add_card(type=PLANNING, status=TODO)
  -> WorkflowEngine sees PLANNING/TODO
  -> HostAgent.process()
  -> HostAgent creates EXECUTION / RESPONSE cards
  -> WorkflowEngine sees EXECUTION/TODO
  -> GroundingAgent.process()
  -> GroundingAgent retrieves relevant tools
  -> GroundingAgent executes Shell / GUI / MCP / Web tools
  -> EXECUTION card becomes DONE or BLOCKED
  -> WorkflowEngine optionally creates EVALUATION card
  -> EvalAgent.process()
  -> RESPONSE card is completed
  -> FastAgent returns final result
```

## 三个核心 Agent

FastAgent 的 agent 分工非常清晰。

```text
HostAgent      -> planner / PM
GroundingAgent -> executor / tool user
EvalAgent      -> verifier / QA
```

这也是我们未来 Research OS 应该采用的基本结构。

### HostAgent: 计划与拆解

核心文件：

```text
fastagent/agents/host_agent.py
```

HostAgent 的职责：

```text
high-level planning
task decomposition
Kanban updates
dependency tracking
replanning after failures
```

它没有 backend scope。
也就是说：

```text
HostAgent 不直接执行工具。
```

这很重要。
因为 planner 如果直接执行，很容易把规划、执行、验证混在一起。

HostAgent 会读取：

```text
backend descriptions
current Kanban summary
blocked task details
EvalAgent feedback
user request
```

然后输出：

```text
thought
plan
task_updates
message_to_grounding
status
```

`task_updates` 会被 `AgentCoordinator.execute_kanban_updates()` 转换成真实 Kanban cards。

这等价于：

```text
自然语言任务 -> 结构化工作流图
```

对我们来说，HostAgent 就是未来的：

```text
PM Agent
Research Planner
Experiment Planner
Application Planner
```

它不应该自己跑回测。
它应该拆任务、定依赖、分配 worker、等待验证。

### GroundingAgent: 跨后端执行

核心文件：

```text
fastagent/agents/grounding_agent.py
```

GroundingAgent 的默认 backend scope：

```text
gui
shell
mcp
web
system
```

它是真正的执行者。

流程：

```text
1. 读取 execution instruction
2. 检查 workspace artifacts
3. 调用 _get_available_tools()
4. 通过 GroundingClient.get_tools_with_auto_search() 做工具检索
5. 用 LLMClient.complete() 带 tools 多轮执行
6. 收集 tool results
7. 如果看到 <COMPLETE> 结束
8. 构建 final result
9. 记录 execution 到 RecordingManager / Memory
```

它有几个关键工程细节。

第一，它不是把所有工具都塞进 prompt。
它会先做 tool retrieval：

```text
task_description -> Smart Tool RAG -> top relevant tools
```

第二，它有 `max_iterations`。
默认 config 里 GroundingAgent 是：

```text
max_iterations = 20
visual_analysis_timeout = 60.0
```

第三，它对 GUI 结果有视觉增强。
如果 GUI tool 返回 screenshot，GroundingAgent 会把截图交给 VLM 做视觉分析，再把分析结果追加到工具结果里。

第四，它会防止长上下文爆掉。
5 轮以后会对 message history 做截断：

```text
keep system messages
keep first user instruction
keep recent rounds
```

这就是 FastAgent 的“fast”之一：

```text
少拿工具、少带上下文、少做无效推理。
```

### EvalAgent: 选择性验证

核心文件：

```text
fastagent/agents/eval_agent.py
```

EvalAgent 用来判断：

```text
当前 execution step 是否真的完成
最终任务是否真的完成
失败是否需要重规划
```

它支持：

```text
step-level evaluation
final evaluation
status determination
screenshot-assisted evaluation
workspace file evidence
dependency-aware context
```

这点很关键。

很多 agent 失败不是因为不会执行第一步。
而是因为：

```text
第一步错了，第二步继续基于错误结果执行。
错误层层叠加，最后输出看起来完整但实际不可用。
```

FastAgent 用 EvalAgent 在中间插入质量闸门。
不过它不是每一步都强制评估。

默认 `EvaluationConfig` 是：

```text
mode = selective
backends = ["gui", "mcp"]
always_eval_last = True
```

这很务实：

```text
GUI / MCP 这种更容易出错或更关键的执行才优先评估。
最后一步永远评估。
```

这比“每一步都评估”更快，也比“完全不评估”更稳。

## Kanban: Agent Workflow 的状态层

核心文件：

```text
fastagent/kanban/enums.py
fastagent/kanban/kanban.py
```

FastAgent 的任务状态不是散落在 prompt 里。
它有明确 card schema：

```text
CardType:
  planning
  execution
  evaluation
  response

CardStatus:
  todo
  in_progress
  done
  blocked

KanbanEvent:
  on_card_added
  on_card_updated
  on_card_deleted
  on_step_recorded
```

每张 `KanbanCard` 有：

```text
card_id
agent_name
card_type
status
title
description
created_at
updated_at
step
metadata
```

这让 agent workflow 变成可观测、可恢复、可复盘的系统。

对比一下，如果没有 Kanban：

```text
agent 只是一个长对话。
```

有了 Kanban：

```text
agent workflow = explicit state machine
```

对 Research OS 来说，这一点非常重要。
我们的研究任务也需要这样的状态：

```text
research_planning
paper_reading
factor_implementation
backtest_execution
bias_evaluation
report_response
pm_review
```

每一步都应该有：

```text
todo / in_progress / done / blocked
```

而不是藏在聊天记录里。

## WorkflowEngine: 事件驱动调度

核心文件：

```text
fastagent/workflow/engine.py
fastagent/workflow/rules.py
```

`WorkflowEngine` 是 Kanban-driven event loop。

它注册默认规则：

```text
PLANNING / TODO   -> HostAgent
EXECUTION / TODO  -> GroundingAgent
EVALUATION / TODO -> EvalAgent
RESPONSE / TODO   -> auto-complete or wait for execution
EXECUTION / DONE  -> create evaluation or link next execution
```

它还会处理：

```text
task timeout
active task tracking
dependency linking
sequential execution
evaluation gating
blocked task handling
```

这里有一个很有意思的选择：

```text
max_concurrent_tasks = 1
```

也就是说，FastAgent 默认不是疯狂并行。
它强调的是：

```text
serial execution to reduce error propagation
```

这对复杂电脑操作很合理。
很多 GUI / 文件 / 网页任务不能乱并行，否则状态会互相污染。

这也提醒我们：

```text
agent 时代不是所有东西都并行。
真正重要的是把依赖关系建清楚。
```

## GroundingClient: 统一工具和后端层

核心文件：

```text
fastagent/grounding/core/grounding_client.py
fastagent/grounding/core/tool/base.py
fastagent/grounding/core/types.py
```

FastAgent 的工具层核心抽象是：

```text
BackendType:
  shell
  gui
  mcp
  web
  system

BaseTool
ToolSchema
ToolResult
ToolRuntimeInfo
SessionConfig
SessionInfo
SecurityPolicy
```

`GroundingClient` 负责：

```text
register providers
initialize providers
create / reuse sessions
list tools
cache tools
bind runtime info
invoke tools
track tool quality
system provider registration
```

这是一层非常重要的抽象。
因为它把不同工具后端统一成：

```text
list_tools()
invoke_tool()
create_session()
close_session()
```

未来我们的 Quant OS 也可以这样做：

```text
BackendType:
  market_data
  factor_library
  backtest
  risk
  portfolio
  paper_search
  code
  web
  mcp
```

然后所有工具都统一成：

```text
ToolSchema
ToolResult
ToolRuntimeInfo
```

这样 Research Agent 才能动态选择工具，而不是写死在 prompt 里。

## Smart Tool RAG: 工具越多，越需要检索

核心文件：

```text
fastagent/grounding/core/search_tools.py
```

FastAgent 的 `ToolRanker` 支持：

```text
keyword
semantic
hybrid
```

默认配置：

```json
{
  "tool_search": {
    "embedding_model": "BAAI/bge-small-en-v1.5",
    "max_tools": 40,
    "search_mode": "hybrid",
    "enable_llm_filter": true,
    "llm_filter_threshold": 50,
    "enable_cache_persistence": true
  }
}
```

它的逻辑是：

```text
如果工具数量 <= max_tools:
  直接返回

如果工具数量 > max_tools:
  先按 backend / server / tool 做筛选
  再用 keyword / semantic / hybrid ranking
  再把 top tools 给 GroundingAgent
```

这就是 FastAgent 的另一个关键启发：

```text
agent 不应该知道所有工具。
agent 应该按任务动态拿到当前最相关的少量工具。
```

这和 RAG 检索知识完全同构。

```text
Knowledge RAG: query -> relevant documents
Tool RAG: task -> relevant tools
```

未来 Quant OS 工具会很多：

```text
get_price_data
get_fundamental_data
get_factor
run_backtest
compute_turnover
plot_drawdown
run_leakage_check
query_paper
query_news
export_report
```

如果全部塞给 agent，prompt 会变脏。
应该像 FastAgent 一样：

```text
task -> retrieve top 20-40 tools -> execute
```

## Tool Quality: 工具也需要信用分

核心文件：

```text
fastagent/grounding/core/quality/
```

FastAgent 不只是检索工具。
它还记录工具质量：

```text
total_calls
success_count
execution_time
recent_success_rate
consecutive_failures
description_quality
penalty
```

`ToolQualityManager` 会根据工具执行表现调整排序。

核心思想：

```text
经常失败的工具，后续排序应该下降。
描述不清楚的工具，也应该被识别出来。
```

这对 agent 系统非常重要。
因为工具世界不是静态的。
MCP server 会变。
网页会变。
API 会挂。
本地环境会缺包。

FastAgent 的做法是：

```text
工具不是平等的。
工具有历史表现。
工具有信用。
工具排名要随执行结果动态演化。
```

这和我们一直说的 `credit` 很像。
未来 Research OS 里也可以给工具打信用：

```text
data source reliability
backtest engine reliability
factor implementation success rate
paper parser success rate
report generator quality
```

## Local Server: Computer Use 的本地桥

核心目录：

```text
fastagent/local_server/
```

FastAgent 的 GUI / Shell 操作依赖一个本地 Flask 服务。
它把本地电脑能力暴露成 HTTP API。

README 里列出的 endpoint 包括：

```text
GET  /
GET  /platform
POST /execute
POST /execute_with_verification
POST /run_python
POST /run_bash_script
GET  /screenshot
GET  /cursor_position
GET/POST /screen_size
POST /list_directory
```

这就是 Computer Use 的底层桥。

它把：

```text
mouse / keyboard
screenshot
window
file I/O
python execution
bash execution
screen recording
platform information
```

统一给上层 agent。

对我们来说，这很关键。
Research OS 不一定一直只在命令行里跑。
很多真实任务会涉及：

```text
网页系统
券商/数据商客户端
PDF/Excel/Word
浏览器
内部系统
申请系统
GitHub 页面
```

Computer Use 让 agent 能进入真实工作界面。

不过也要注意：

```text
Computer Use 权限很高，安全和审计必须做好。
```

FastAgent 也有 security config。
它会配置 block commands，例如删除、格式化、关机等危险命令。

## 四类 Backend

FastAgent 的 backend 设计很清楚。

### Shell Backend

核心文件：

```text
fastagent/grounding/backends/shell/
```

Shell backend 提供一个：

```text
shell_agent
```

它内部会让 LLM 写 Python 或 Bash 代码，通过 local server 执行。
它还会自动重试修错。

适合：

```text
读写文件
跑脚本
检查环境
生成报告
处理数据
安装/调用命令行工具
```

对 Quant OS 来说，shell backend 就是：

```text
run factor code
run backtest
generate plots
validate data
export report
```

### GUI Backend

核心文件：

```text
fastagent/grounding/backends/gui/
```

GUI backend 提供：

```text
gui_agent
```

它的循环是：

```text
observe screenshot
plan next action
execute click/type/drag/hotkey/etc.
verify state
repeat until DONE / FAIL / max_steps
```

这就是 Computer Use。

适合：

```text
浏览器操作
桌面软件
GUI-only systems
需要视觉理解的任务
```

### MCP Backend

核心文件：

```text
fastagent/grounding/backends/mcp/
```

MCP backend 支持：

```text
multiple MCP server sessions
stdio / HTTP / websocket / SSE transports
tool metadata cache
schema sanitization
dependency checking
auto install
lazy or eager session creation
```

这非常适合 agent 时代。

因为越来越多工具会以 MCP server 形式出现：

```text
GitHub
Gmail
Google Drive
database
browser
search
finance data
internal tools
```

FastAgent 的 MCP backend 把这些工具纳入统一 tool layer。

### Web Backend

核心文件：

```text
fastagent/grounding/backends/web/
```

Web backend 是 knowledge research backend。
适合：

```text
search
browsing
source collection
knowledge retrieval
```

这和 DeepResearch 对接。

## Memory 和 Content Processor

核心文件：

```text
fastagent/memory/
fastagent/agents/content_processor.py
```

FastAgent 把内容分成三层：

```text
Memory   -> agent-local working memory
Context  -> task-level knowledge accumulator
Response -> user-facing answer
```

`ContentProcessor` 会判断内容类型：

```text
OPERATION       -> GUI clicks / inputs / operations
DATA_RETRIEVAL  -> web / MCP / file / API data
VERIFICATION    -> check / verify / test
TRANSFORMATION  -> convert / generate / format
```

也会分内容粒度：

```text
FULL
SUMMARY
MINIMAL
```

这背后的原则很对：

```text
不是所有执行结果都值得完整放进上下文。
```

例如：

```text
GUI click 操作只需要 minimal summary。
网页搜索结果可能需要保留更多数据。
文件读取和 API 返回可能是 critical context。
验证结果只需要 success / failure 和原因。
```

`MemorySummarizer` 会在 memory 到达阈值后压缩历史：

```text
preserve key decisions
preserve state changes
preserve causal relationships
preserve tool usage
extract patterns
maintain temporal order
```

这对长任务非常关键。
Agent 时代的核心瓶颈之一就是：

```text
上下文不是越多越好。
上下文要被分层、过滤、压缩、复用。
```

## Recording: 轨迹和审计

核心目录：

```text
fastagent/recording/
```

FastAgent 默认开启 recording：

```text
enable_recording = True
enable_screenshot = True
enable_video = True
enable_conversation_log = True
recording_log_dir = ./logs/recordings
```

它会记录：

```text
agent actions
tool executions
Kanban events
screenshots
video
conversations
trajectory
```

这和 Research OS 的实验记录高度相关。

未来我们的系统应该能回答：

```text
这个因子是谁提出的？
哪个 agent 实现的？
用了哪些数据？
跑了哪些命令？
产生了哪些文件？
哪个评估步骤通过/失败？
为什么进入下一轮？
```

这需要 recording / audit trail。

## 和 ClawTeam / ClawWork 的关系

FastAgent 和前两篇的关系很清楚。

| Project | 核心问题 | 对我们的启发 |
|---|---|---|
| ClawTeam | 多 agent 如何组织成 team | AI research organization |
| ClawWork | agent 如何作为 coworker 交付、评分、赚钱 | economic accountability |
| FastAgent | agent 如何快速规划、执行、调用工具、验证 | execution engine |

如果把三者合起来：

```text
ClawTeam 管组织。
ClawWork 管价值和账本。
FastAgent 管执行和工具。
```

这已经很接近我们想做的 Research OS。

```text
PM Agent 用 ClawTeam-like team protocol 管任务。
Research Worker 用 ClawWork-like budget / score / payment 管价值。
Execution Engine 用 FastAgent-like tool RAG / grounding / eval 管执行。
```

## 对 Pengyi Research OS 的启发

FastAgent 给我们的启发非常工程化。

### 1. Research OS 要有 planner / executor / evaluator 分工

不要把所有东西塞给一个 agent。

未来可以设计：

```text
HostAgent / PMAgent:
  拆研究任务、定义依赖、分配执行步骤

GroundingAgent / ResearchExecutor:
  调用 paper search、market data、factor lib、backtest、code、web、MCP

EvalAgent / ResearchReviewer:
  验证数据泄露、回测正确性、代码可复现性、报告质量
```

这比单 agent 更稳定。

### 2. Quant 工具要统一成 Tool Layer

现在我们还会很容易陷入：

```text
某个脚本接一个数据源
某个 notebook 跑一个回测
某个 prompt 读一篇 paper
```

FastAgent 提醒我们：

```text
所有工具都应该被注册成可检索、可调用、可记录、可评分的 tools。
```

Quant OS 可以有：

```text
market_data.get_ohlcv
market_data.get_fundamental
factor.compute_alpha
backtest.run
risk.check_leakage
risk.compute_drawdown
report.export_pdf
paper.search
paper.summarize
github.create_pr
```

然后让 agent 根据任务检索 top tools。

### 3. Tool RAG 是 agent 时代基础设施

知识越来越多，需要 RAG。
工具越来越多，也需要 RAG。

FastAgent 的 `max_tools=40` 很有现实意义。
超过这个数量，就应该检索。

我们未来可以做：

```text
Quant Tool RAG:
  task description -> retrieve data / factor / backtest / report tools

Research Memory RAG:
  current question -> retrieve papers / notes / previous experiments

Artifact RAG:
  current report -> retrieve charts / logs / prior outputs
```

### 4. EvalAgent 是 Research OS 的质量闸门

我们的 R&D Agent 不能只产出。
它必须被检查。

检查可以包括：

```text
代码能否运行
结果是否复现
有没有 look-ahead bias
有没有 survivorship bias
样本内/样本外是否分开
交易成本是否建模
drawdown 是否可接受
报告是否引用证据
是否达到 PM 要求
```

FastAgent 的 EvalAgent 可以作为模板。

### 5. GUI / Computer Use 不能忽视

很多真实世界任务没有干净 API。

尤其是：

```text
银行内部系统
学校申请系统
数据商客户端
网页后台
Excel / PPT / PDF 工具链
GitHub / 网站后台
```

所以 Research OS 不能只想命令行。
Computer Use 是现实世界 interface 的最后一公里。

## 可以直接迁移到 Quant OS 的架构草图

```text
Pengyi Quant Research OS v0

planner/
  pm_agent.py
  task_decomposer.py

workflow/
  kanban.py
  rules.py
  context_manager.py

grounding/
  client.py
  tool_schema.py
  backends/
    market_data/
    backtest/
    risk/
    paper/
    github/
    shell/
    web/
    mcp/

agents/
  research_executor.py
  eval_agent.py
  report_agent.py

memory/
  experiment_memory.py
  paper_memory.py
  factor_memory.py
  summarizer.py

quality/
  tool_quality.py
  experiment_quality.py
  rubric_evaluator.py

recording/
  trajectory.jsonl
  commands.jsonl
  artifacts.jsonl

dashboard/
  kanban board
  experiment lineage
  cost / ROI / quality
```

最小闭环：

```text
User: 研究一个成交量异常 alpha 因子

PMAgent:
  创建 planning card
  拆成 data -> factor -> backtest -> bias check -> report

ResearchExecutor:
  检索 market_data / factor / backtest tools
  执行代码
  生成 artifacts

EvalAgent:
  检查数据泄露、回测结果、报告完整性

Response:
  生成 research report
  更新 Kanban
  保存 memory / recording
```

这就是 FastAgent 给我们的直接蓝图。

## 可以提 PR / 改进的方向

从本地阅读看，FastAgent 已经是一个相当完整的工程项目。
如果我们以后给它提 PR，建议从低风险、高价值的文档和开发体验开始。

### 1. README 提到 `.env.example`，但本地未看到对应文件

README 里说：

```text
refer to fastagent/.env.example
```

但本地 `rg --files` 没看到这个文件。

可以提一个文档/示例 PR：

```text
fastagent/.env.example
```

包含：

```text
ANTHROPIC_API_KEY=
OPENAI_API_KEY=
EMBEDDING_API_KEY=
EMBEDDING_BASE_URL=
LOCAL_SERVER_URL=
```

这会明显降低上手摩擦。

### 2. MCP 示例配置需要彻底占位符化

`config_mcp.json.example` 里出现了一个看起来像真实 API key 的字符串。
公开 example 文件最好只保留：

```text
YOUR_TAVILY_API_KEY
```

这种占位符。

这类 PR 很合适：

```text
replace literal-looking key with placeholder
add note: never commit real API keys
```

### 3. Tool RAG optional dependency 文档要更清楚

代码里 semantic search 会尝试使用：

```text
fastembed
rank_bm25
```

但 `requirements.txt` 里没有把它们作为核心依赖。
代码有 fallback，这是好的。
但 README 可以明确：

```text
basic mode works without fastembed
semantic tool search requires fastembed or remote embedding API
keyword ranking can use rank_bm25 if installed
```

更好的方式是 extras：

```text
pip install fastagent[tool-rag]
```

### 4. Python 版本要求需要对齐

README badge 写的是：

```text
Python 3.10+
```

但 quick start 使用：

```text
python=3.12
```

本地用 Python 3.11 parser 做 AST 检查时，`fastagent/utils/ui.py` 有一处嵌套 f-string 写法无法解析。
这类写法在 Python 3.12 语法放宽后更合理。

所以可以提一个小 PR：

```text
要么把 README / badge 改成 Python 3.12+
要么把该 f-string 写法改成 3.10/3.11 兼容格式
```

这属于很实际的开发体验修复。

### 5. 增加 dry-run / no-local-server smoke test

因为 GUI / Shell 需要 local server。
新用户很容易在环境配置上卡住。

可以加一个：

```bash
python -m fastagent --query "Say hello" --no-ui --no-workflow
```

或者更明确：

```bash
python -m fastagent.doctor
python -m fastagent.smoke_test
```

检查：

```text
LLM key
local server
MCP config
tool cache
GUI permission
Shell permission
embedding availability
```

### 6. 增加 Quant / Research OS 示例 workflow

FastAgent 的结构天然适合 research workflow。
可以补一个 example：

```text
examples/research_report_agent/
```

包含：

```text
search papers
collect sources
write markdown report
verify citations
export final artifact
```

这对我们也最有用。

## 当前限制和注意点

### 1. GUI automation 依赖本地权限

Computer Use 很强，但现实中需要：

```text
screen recording permission
accessibility permission
PyAutoGUI / pywinauto / X11 / macOS adapter
local server running
```

这些都会影响稳定性。

### 2. 默认串行执行更稳，但不是最高吞吐

`WorkflowEngine` 默认：

```text
max_concurrent_tasks = 1
```

这对 GUI / 复杂任务合理。
但如果是 paper search / batch backtest / independent data queries，未来可以扩展并行执行。

关键是：

```text
并行必须基于依赖图，而不是盲目并行。
```

### 3. Tool quality 需要长期运行数据

工具信用分很重要，但新系统初期数据少。
因此需要：

```text
bootstrap rules
manual pinning
domain prior
human feedback
```

否则可能过早惩罚某些偶发失败工具。

### 4. LLM planning 仍然需要强 prompt / schema 约束

HostAgent 输出 task_updates。
如果 schema 不稳，整个 workflow 会受影响。
未来如果用于高价值 research workflow，可以强化：

```text
Pydantic validation
JSON schema enforcement
retry with repair
human PM approval
```

## 对我们当前路线的结论

FastAgent 是 HKUDS Agent Product / Workspace 系列里非常关键的第三块拼图。

```text
ClawTeam 让 agent 组织起来。
ClawWork 让 agent 承担经济责任。
FastAgent 让 agent 快速执行真实电脑任务。
```

它对我们的最大启发是：

```text
Agent 时代的核心不是“一个更会聊天的模型”。
而是一套完整执行系统：

planner
executor
evaluator
tool layer
tool retrieval
memory
workflow state
security
recording
UI / local computer bridge
```

我们做 Pengyi Research OS / Quant Research OS，也要从这个角度搭。

不是只做：

```text
一个 R&D Agent prompt
```

而是要做：

```text
一个能把研究任务变成工作流、把工具变成可检索资源、把执行变成可审计轨迹、把结果变成可验证 artifact 的 agent operating system。
```

这就是我们进入 agent 时代后真正要抓住的东西。

下一篇 `HKUDS036` 按路线应该看 `Litewrite`。
FastAgent 偏执行和工具，Litewrite 更可能偏写作 / workspace / product 化输出。
这条线正好接到我们的网站、申请材料、research report 和公开输出系统。
