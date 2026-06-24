---
title: "HKUDS008: AutoAgent 作为 Self-Developing Agent Factory 与 Zero-Code Workflow Creation Layer"
date: 2026-06-24 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds008, hkuds, autoagent, metachain, agent-factory, zero-code-agent, deep-research, workflow, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第九篇。

```text
HKUDS008 -> AutoAgent
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
```

现在来看 `AutoAgent`。我对它的定位是：

```text
AutoAgent = Self-Developing Agent Factory + Zero-Code Workflow Creation Layer
```

如果说：

```text
RAG-Anything = multimodal document ingestion
LightRAG     = research memory
Vibe-Trading = quant research workflow
nanobot      = personal agent shell
CLI-Anything = software action layer
AI-Trader    = live trading platform layer
AgentSpace   = organizational agent workspace
```

那么：

```text
AutoAgent = 用自然语言自动创建 tools、agents、workflows 的 agent factory
```

它和 AgentSpace 不一样。AgentSpace 更像组织级 workspace，关心 agent 的 owner、role、runtime、permission、approval、audit。AutoAgent 更像一个自举式 agent 工厂，关心如何从自然语言需求生成可运行的工具、agent 和 workflow。

这对 Pengyi Research OS 的启发很直接：

```text
不是只使用 agent，而是让系统能自动生产 agent。
```

如果我们未来做 Quant R&D Agent，不能每次都手写所有工具和子 agent。更理想的形态是：人类 PM 给出研究需求，系统自动形成 agent form、补齐工具、创建 agent、测试 agent、生成 workflow，再进入执行和复盘。

AutoAgent 正好站在这个位置。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `AutoAgent`。

| Item | Value |
|---|---|
| repo | `AutoAgent` |
| remote | `https://github.com/HKUDS/AutoAgent.git` |
| branch | `main` |
| local head | `16c12b0` |
| latest local commit | `Update Communication.md` |
| status | clean, synced with `origin/main` |
| license | MIT |
| package | `autoagent` |
| previous name | `MetaChain` |
| console command | `auto` |
| Python requirement | `>=3.10` |

本地规模：

| Metric | Count |
|---|---:|
| total files | 276 |
| Python files | 99 |
| docs files | 133 |

核心目录：

```text
autoagent/
  core.py
  types.py
  registry.py
  cli.py
  main.py
  agents/
  tools/
  workflows/
  flow/
  memory/
  environment/
  cli_utils/

evaluation/
  gaia/
  math500/
  multihoprag/

docs/
  docs/
  src/
```

这个项目不是单纯 agent demo。它同时包含：

```text
agent runtime
tool registry
agent registry
workflow registry
meta-agent creation pipeline
CLI interaction
Docker / browser / file / code environments
RAG memory tools
benchmark evaluation scripts
Docusaurus docs site
```

但也要注意：它有明显研究原型气质。README 写了 v0.2.0，`setup.cfg` 里 package version 仍是 `0.1.0`；代码里还保留不少 `MetaChain` / `mc` 旧命名；部分 docs 页面只是骨架。这说明它很适合学习架构和思想，但如果要生产化，需要先做稳定性、命名一致性、测试和工程边界整理。

## 1. 项目用途

README 对 AutoAgent 的表述是：

```text
Fully-Automated and Self-Developing framework
Natural Language Alone
Zero-Code Framework
```

从代码看，它主要有三种使用模式：

| Mode | Purpose |
|---|---|
| `user mode` | 直接使用 deep research agents 完成检索、分析、报告任务 |
| `agent editor` | 用自然语言创建 agent，不需要手写 agent 文件 |
| `workflow editor` | 用自然语言创建由多个 agents 组成的 workflow |

所以 AutoAgent 的核心用途不是“帮用户聊天”，而是：

```text
从自然语言需求自动构建 agent 系统。
```

它要解决的问题是：

```text
用户不知道如何设计 agent
用户不知道需要哪些 tools
用户不知道 multi-agent workflow 应该怎么拆
用户不想手写 registry / code / test
```

AutoAgent 试图把这些工作自动化：

```text
natural language requirement
  -> agent/workflow form
  -> tool creation
  -> agent creation
  -> workflow creation
  -> run and test
```

这就是我把它称为 agent factory 的原因。

## 2. 整体架构

AutoAgent 可以拆成六层：

| Layer | Key Files | Responsibility |
|---|---|---|
| Runtime | `core.py`, `types.py` | agent 执行、tool call、handoff、context 更新 |
| Registry | `registry.py` | 注册 tools、agents、plugin tools、plugin agents、workflows |
| System Agents | `agents/system_agent/` | triage、web surfer、file surfer、coding agent |
| Meta Agents | `agents/meta_agent/`, `cli_utils/` | 自动创建 tools、agents、workflows |
| Environments | `environment/` | DockerEnv、LocalEnv、BrowserEnv、MarkdownBrowser |
| Evaluation | `evaluation/` | GAIA、MultiHopRAG、Math500 |

主入口是 CLI：

```bash
auto main
auto deep-research
auto agent
auto workflow
```

其中：

```text
auto main          -> full AutoAgent: user mode + agent editor + workflow editor
auto deep-research -> lightweight deep research user mode
auto agent         -> run a specific agent
auto workflow      -> run a specific workflow
```

README 里有些旧示例还写 `mc agent`，这是 MetaChain 旧命名残留。实际 `setup.cfg` 暴露的 console script 是：

```text
auto = autoagent.cli:cli
```

这是一个值得后续 PR 或 issue 关注的小工程点。

## 3. 核心抽象：Agent / Result / Response

`autoagent/types.py` 定义了三个核心数据结构。

`Agent`：

```python
class Agent(BaseModel):
    name: str = "Agent"
    model: str = "gpt-4o"
    instructions: Union[str, Callable[[], str]] = "You are a helpful agent."
    functions: List[AgentFunction] = []
    tool_choice: str = None
    parallel_tool_calls: bool = False
    examples: Union[List[Tuple[dict, str]], Callable[[], str]] = []
    handle_mm_func: Callable[[], str] = None
    agent_teams: Dict[str, Callable] = {}
```

这个结构和 OpenAI Swarm 风格很接近：

```text
agent = instructions + model + functions + optional team/handoff logic
```

`Result`：

```python
class Result(BaseModel):
    value: str = ""
    agent: Optional[Agent] = None
    context_variables: dict = {}
    image: Optional[str] = None
```

`Result` 是关键。它可以同时返回：

```text
value                 -> tool observation
agent                 -> handoff target
context_variables     -> shared state update
image                 -> visual observation
```

也就是说，一个 tool call 不只是返回字符串，还可以把执行权交给另一个 agent，或者更新全局上下文。

`Response`：

```python
class Response(BaseModel):
    messages: List = []
    agent: Optional[Agent] = None
    context_variables: dict = {}
```

这就是一次 agent run 的输出：消息轨迹、当前 agent、上下文状态。

## 4. Runtime：MetaChain

AutoAgent 的底层 runtime 仍叫 `MetaChain`。

核心文件是：

```text
autoagent/core.py
```

关键函数：

| Function | Responsibility |
|---|---|
| `get_chat_completion` | 构造 system prompt、history、tools，调用 LiteLLM |
| `handle_tool_calls` | 执行 tool calls，处理 Result、handoff、context update |
| `run` | 同步 agent loop |
| `run_async` | 异步 agent loop |
| `run_and_stream` | streaming loop |

执行逻辑大概是：

```text
active_agent = agent
history = user messages

while max_turns not reached:
    completion = LLM(active_agent.instructions, history, active_agent.functions)
    if no tool call: stop
    partial_response = handle_tool_calls(...)
    history += tool observations
    context_variables.update(...)
    if partial_response.agent:
        active_agent = partial_response.agent
```

`handle_tool_calls` 是最关键的地方：

```text
1. 根据 tool name 找到 Python function
2. 把 JSON arguments 解析成 args
3. 如果 function 需要 context_variables，就注入 context_variables
4. 执行 function
5. 把返回值包装成 Result
6. 把 Result.value 写入 tool observation
7. 如果 Result.context_variables 非空，更新上下文
8. 如果 Result.agent 非空，切换 active agent
9. 如果 Result.image 非空，追加 image message
```

这套机制让 AutoAgent 支持：

```text
tool execution
agent handoff
shared memory
visual observation
multi-step solving
```

它不是一个复杂的分布式 runtime，而是一个轻量、直接、可读的 agent loop。

## 5. Registry：把工具、agent、workflow 注册成可发现资产

`autoagent/registry.py` 定义全局 registry。

支持五类注册：

```text
tool
agent
plugin_tool
plugin_agent
workflow
```

对应 decorator：

```python
register_tool
register_agent
register_plugin_tool
register_plugin_agent
register_workflow
```

registry 会记录：

```text
name
func_name
args
docstring
body
return_type
file_path
```

这很关键。因为 AutoAgent 的 meta agents 需要先知道系统里已经有哪些 tools 和 agents，然后决定：

```text
复用已有工具
创建新工具
复用已有 agent
创建新 agent
生成 orchestrator
生成 workflow
```

所以 registry 是 AutoAgent 自我编辑能力的基础。没有 registry，agent factory 就不知道自己能调用什么，也不知道自己改了什么。

## 6. User Mode：Deep Research Agent Team

`user mode` 的入口在 `autoagent/cli.py`。

它会创建：

```text
System Triage Agent
File Surfer Agent
Web Surfer Agent
Coding Agent
```

`System Triage Agent` 在 `agents/system_agent/system_triage_agent.py` 里。

它的职责是：

```text
根据当前任务状态，把子任务转交给最合适的 agent。
```

它可以调用三个 transfer tool：

```text
transfer_to_filesurfer_agent
transfer_to_websurfer_agent
transfer_to_coding_agent
```

这些 transfer tool 返回：

```python
Result(value=sub_task_description, agent=target_agent)
```

这就是 handoff。

三个子 agent 的分工：

| Agent | Purpose |
|---|---|
| File Surfer Agent | 打开本地文件、浏览文件内容 |
| Web Surfer Agent | 浏览网页、搜索、点击、转 markdown |
| Coding Agent | 写代码、执行命令、计算、调试 |

这个设计和 Magentic-One 很接近：一个 triage / orchestrator，把任务分给专门 agent。

对我们来说，它对应：

```text
research assistant mode
```

比如：

```text
读一篇论文
打开一个 PDF
上网查资料
写代码验证
综合输出报告
```

这正是 AI scientist daily workflow 的基本形态。

## 7. Agent Editor：自然语言创建 agent

这是 AutoAgent 最有价值的部分。

入口在：

```text
autoagent/cli_utils/metachain_meta_agent.py
```

它把 agent creation 拆成三步：

```text
1. Agent Former Agent
2. Tool Editor Agent
3. Agent Creator Agent
```

### 7.1 Agent Former Agent

`Agent Former Agent` 的任务是：

```text
把用户自然语言需求转成 XML agent form。
```

这个 form 包含：

```text
<agents>
  <system_input>
  <system_output>
  <global_variables>
  <agent>
    <name>
    <description>
    <instructions>
    <tools category="existing">
    <tools category="new">
    <agent_input>
    <agent_output>
```

它会查询现有工具和 agent：

```python
list_tools(context_variables)
list_agents(context_variables)
```

然后决定哪些工具复用，哪些工具新建。

这一步本质上是：

```text
natural language requirement -> agent specification
```

### 7.2 Tool Editor Agent

如果 form 里有新工具，`Tool Editor Agent` 会被调用。

它的任务是：

```text
创建新 tool
写 Python 文件
注册 plugin tool
运行 tool 测试
直到所有工具测试通过
```

相关工具在：

```text
autoagent/tools/meta/edit_tools.py
```

关键函数：

| Function | Purpose |
|---|---|
| `list_tools` | 列出可用 plugin tools |
| `create_tool` | 写入新 tool 文件 |
| `delete_tool` | 删除 tool |
| `run_tool` | 创建测试文件并运行 tool |

这里最重要的是：它不是只“生成代码”，而是要求创建后运行测试。

这就是 self-developing agent 的核心：

```text
generate -> execute -> observe -> fix -> retry
```

### 7.3 Agent Creator Agent

工具准备好后，`Agent Creator Agent` 会创建 agent。

相关文件：

```text
autoagent/agents/meta_agent/agent_creator.py
autoagent/tools/meta/edit_agents.py
```

关键函数：

| Function | Purpose |
|---|---|
| `create_agent` | 创建或更新 agent 文件 |
| `read_agent` | 读取已有 agent 定义 |
| `list_agents` | 列出可用 agents |
| `run_agent` | 运行 agent 完成测试任务 |
| `create_orchestrator_agent` | 为多 agent 场景创建 orchestrator |

多 agent 情况下，它要求创建 orchestrator agent。

这对我们很重要，因为 Quant R&D Agent 不是单 agent：

```text
factor hypothesis agent
data validation agent
backtest agent
bias diagnosis agent
report agent
PM review agent
```

AutoAgent 的 agent editor 给的是一个自动搭建这些角色的思路。

## 8. Workflow Editor：自然语言创建 workflow

workflow editor 的入口在：

```text
autoagent/cli_utils/metachain_meta_workflow.py
```

它有两个主要 agent：

```text
Workflow Former Agent
Workflow Creator Agent
```

`Workflow Former Agent` 把需求转成 XML workflow form。

workflow form 包含：

```text
<workflow>
  <name>
  <system_input>
  <system_output>
  <agents>
  <global_variables>
  <events>
```

每个 event 包含：

```text
name
inputs
task
outputs
listen
agent
```

outputs 还支持 action：

```text
RESULT
ABORT
GOTO
```

这让 workflow 可以表达：

```text
sequential flow
conditional branch
parallel branch
retry / goto
abort
```

`create_workflow` 在 `autoagent/tools/meta/edit_workflow.py` 里。它会把 workflow form 编译成 Python event code，并使用 `autoagent.flow` 的 event engine 执行。

这可以理解成：

```text
natural language -> workflow XML -> event graph -> executable Python workflow
```

这件事对 Research OS 很重要，因为 R&D Agent 的核心不是单步问答，而是长期 workflow：

```text
读资料
提出假设
实现因子
跑回测
诊断偏差
生成下一轮计划
人类 PM 审核
```

AutoAgent 的 workflow editor 正是在尝试自动生成这类流程。

## 9. Flow Engine：事件驱动 workflow

`autoagent/flow/` 是一个轻量事件引擎。

关键文件：

```text
flow/core.py
flow/types.py
flow/dynamic.py
```

核心概念：

| Concept | Meaning |
|---|---|
| `BaseEvent` | 一个异步事件节点 |
| `EventGroup` | 多个 parent events 的组合触发条件 |
| `EventInput` | 事件输入 |
| `ReturnBehavior` | DISPATCH / GOTO / ABORT / INPUT |
| `goto_events` | 跳转到指定事件 |
| `abort_this` | 中止当前流程 |

执行逻辑是：

```text
queue starts with on_start
run current event
store result
find events listening to satisfied parent groups
append next events to queue
run async tasks
handle GOTO / ABORT
return run context
```

这不是很复杂，但很实用。它让 workflow 从“线性 chain”升级成“事件图”。

如果未来我们做 Quant R&D Workflow，可以有：

```text
on_start
load_data
validate_data
generate_factor
run_backtest
diagnose_bias
if result_good -> write_report
if result_bad -> revise_hypothesis
if data_bad -> abort_to_data_fix
pm_review
```

这比简单 agent loop 更适合研究流程。

## 10. Environment：让 agent 有真实操作环境

AutoAgent 的 environment 层很重要。

主要包括：

| Environment | Purpose |
|---|---|
| `DockerEnv` | 在 Docker 容器里执行命令 |
| `LocalEnv` | 本地 conda 环境执行命令 |
| `BrowserEnv` | 浏览器交互环境 |
| `RequestsMarkdownBrowser` | 文件/网页转 markdown 浏览 |

`DockerEnv` 会启动容器，把本地 workspace mount 进去，并通过 `tcp_server.py` 和容器通信。

`LocalEnv` 会尝试找 conda.sh，并运行：

```text
conda activate auto
```

所以 AutoAgent 的实际运行依赖比较重：

```text
Docker
Conda
Playwright / browsergym
LiteLLM
Docling
ChromaDB
OpenAI / Anthropic / other model keys
```

这就是为什么我不建议直接把它当稳定生产框架。它是很好的 research prototype，但要在我们的机器上完整跑起来，需要先做环境清理。

## 11. Built-in Tools

AutoAgent 自带几类工具。

### 11.1 Terminal Tools

`autoagent/tools/terminal_tools.py`

包括：

```text
read_file
create_file
write_file
list_files
create_directory
execute_command
run_python
terminal_page_up/down/to
gen_code_tree_structure
```

这些是 Coding Agent 的基础能力。

一个值得注意的设计是：长输出会被写到临时 terminal output 文件，再通过 markdown browser 分页阅读。这是在解决 agent 读超长终端输出的问题。

### 11.2 Web Tools

`autoagent/tools/web_tools.py`

包括：

```text
click
page_down
page_up
history_back
history_forward
web_search
input_text
sleep
visit_url
get_page_markdown
```

它把浏览器状态转成 accessibility tree 或 markdown，然后返回给 agent。

### 11.3 File Surfer Tools

`autoagent/tools/file_surfer_tool.py`

支持：

```text
open_local_file
page_up_markdown
page_down_markdown
find_on_page_ctrl_f
find_next
visual_question_answering
video / audio related processing
```

这让 user mode 可以处理本地文件、图片、视频、音频等材料。

### 11.4 RAG Tools

`autoagent/tools/rag_tools.py`

关键工具：

```text
save_raw_docs_to_vector_db
query_db
modify_query
answer_query
can_answer
```

底层 memory 在：

```text
autoagent/memory/rag_memory.py
```

使用 ChromaDB，并支持 OpenAI embedding 或 sentence transformer embedding。

这说明 AutoAgent 内部也有一个轻量 Agentic RAG 路线。不过和 `RAG-Anything + LightRAG` 相比，它更像内置工具型 RAG，不是完整 graph/multimodal ingestion 系统。

## 12. Evaluation：GAIA / MultiHopRAG / Math500

AutoAgent 带了评测目录：

```text
evaluation/gaia/
evaluation/multihoprag/
evaluation/math500/
```

GAIA 评测脚本会：

```text
1. 加载数据集
2. 为每个样本启动独立 Docker workspace
3. 如果有文件，把文件复制进 workspace
4. 构造 instruction
5. 创建 System Triage Agent
6. 注入 code_env / web_env / file_env
7. 运行 agent
8. 解析 <solution>...</solution>
9. 评分并保存 messages
10. 清理容器和端口
```

还有一个很关键的 fallback：

```text
如果现有 agent system 没解决，尝试用 Tool Editor Agent 创建新工具继续解决。
```

这就是 self-developing 的评测体现。

MultiHopRAG 路径则展示了 Agentic RAG 的使用。Math500 路径展示了 math solver agent / workflow 的尝试。

不过代码里也有版本漂移：Math500 里有 `metachain` import，当前包名是 `autoagent`。这也说明评测代码需要整理后才能直接稳定复现。

## 13. 和 Auto-Deep-Research 的关系

本地也有 `Auto-Deep-Research`，而 AutoAgent README 也说：

```text
auto deep-research
```

是更轻量的 user mode，类似 Auto-Deep-Research。

可以这样理解：

```text
Auto-Deep-Research = 专注 deep research user mode
AutoAgent          = deep research + agent editor + workflow editor + self-developing framework
```

所以 AutoAgent 更大，目标更接近一个通用 agent factory。

如果我们后面做 HKUDS009，也可以专门看 Auto-Deep-Research，把它和 AutoAgent 的 user mode 做对比。

## 14. 和其他 HKUDS 项目的关系

现在 HKUDS map 的拼图更完整。

```text
RAG-Anything
  -> 读复杂文档、图片、表格、公式、PDF、Office

LightRAG
  -> 建 research memory 和 graph RAG

AutoAgent
  -> 自动创建 tools、agents、workflows

Vibe-Trading
  -> 把 quant research intent 转成 workflow/backtest/evidence

AI-Trader
  -> agent-native live trading platform

AgentSpace
  -> human + agent organization workspace

CLI-Anything
  -> 把软件动作包装成 agent-native CLI tools

nanobot
  -> personal always-on agent shell
```

AutoAgent 的位置是：

```text
agent production layer
```

它不是知识层，也不是交易平台层，而是“生产 agent 和 workflow 的层”。

## 15. 对 Pengyi Quant Research OS 的启发

### 15.1 R&D Agent 不应该全靠手写

我们之前定义的 R&D Agent 是：

```text
自动提出因子假设
自动实现
自动回测
自动诊断偏差
自动生成下一轮研究计划
人类 PM 审核
```

手写第一版可以。但如果系统持续发展，会出现很多新需求：

```text
新的数据源
新的因子族
新的回测框架
新的报告格式
新的风控检查
新的论文解析方式
新的 PM checklist
```

这些都不应该每次都人工硬编码。

AutoAgent 给的启发是：

```text
让系统具备创建工具和子 agent 的能力。
```

比如：

```text
用户：我需要一个 A 股公告事件因子研究 agent。

AutoAgent-style pipeline:
1. 生成 agent form
2. 判断需要哪些已有工具
3. 判断缺哪些新工具
4. 创建公告下载 / 清洗 / 事件抽取工具
5. 创建 event factor agent
6. 创建 backtest workflow
7. 跑一个 sample task
8. 输出可审查结果
```

这就是 R&D Agent 的自我扩展能力。

### 15.2 Human PM 审核更重要

AutoAgent 的自动创建能力很强，但越自动，越需要 PM 审核。

因为它会：

```text
创建代码
运行代码
装依赖
改 agent 文件
改 workflow 文件
上网
访问文件
调用模型
```

这意味着在真实 Research OS 中必须有：

```text
permission boundary
diff review
execution sandbox
data access policy
secret management
approval gate
audit log
rollback
```

这正好和 AgentSpace 互补：

```text
AutoAgent creates and edits agents.
AgentSpace governs and audits agents.
```

所以两个项目可以融会贯通：

```text
AutoAgent = builder
AgentSpace = manager
```

### 15.3 Natural Language to Agent 是真正的 leverage

对个人 AI scientist 来说，最强的 leverage 不是只让 LLM 回答问题，而是：

```text
把想法变成可运行系统。
```

AutoAgent 的路线是：

```text
idea -> form -> tool -> agent -> workflow -> test
```

这很接近我们想做的“从研究想法到实验系统”的链路：

```text
research idea -> experiment spec -> data tool -> factor agent -> backtest workflow -> report
```

所以它不一定要直接被我们复用，但它的架构思想非常值得吸收。

## 16. 可能的 Pengyi Version

如果把 AutoAgent 思想放进 Pengyi Research OS，我会这样设计：

```text
Pengyi Research OS
  agent_factory/
    spec_former.py
    tool_builder.py
    agent_builder.py
    workflow_builder.py
    test_runner.py
    pm_review_gate.py

  tools/
    data_tools/
    backtest_tools/
    report_tools/
    rag_tools/

  agents/
    factor_hypothesis_agent.py
    factor_implementation_agent.py
    backtest_agent.py
    bias_diagnosis_agent.py
    pm_review_agent.py

  workflows/
    factor_research_flow.py
    paper_to_factor_flow.py
    report_to_hypothesis_flow.py
```

核心原则：

```text
所有自动创建的工具和 agent 必须先进入 review queue。
所有自动生成代码必须有 diff。
所有可执行动作必须有 sandbox。
所有使用真实金融数据的流程必须有 data policy。
```

这就是把 AutoAgent 的 self-developing 能力和真实组织/合规要求结合起来。

## 17. 可以提 PR / Issue 的方向

基于本地阅读，我觉得有几个真实改进点：

| Direction | Why Useful |
|---|---|
| README 命名一致性 | `auto` / `mc` / `MetaChain` / `AutoAgent` 命名混用，容易让新用户困惑 |
| package version alignment | README 写 v0.2.0，但 `setup.cfg` 是 0.1.0 |
| Math500 imports cleanup | `evaluation/math500` 里仍有 `metachain` import |
| docs completion | 多个 docs 页面只有 front matter，可以补最小可运行教程 |
| Windows setup guide | 本地 Windows + Docker + conda + browser 依赖较重 |
| `agent editor` minimal example | 给一个从需求到生成 agent 的可复制样例 |
| `workflow editor` minimal example | 给一个 workflow XML 到可运行 workflow 的最小样例 |
| safety / sandbox doc | 自动创建工具和执行代码需要安全边界说明 |
| test coverage | registry、create_tool、create_agent、workflow compilation 都值得有单元测试 |

最适合我们的贡献路线：

```text
先跑一个最小 agent editor demo
记录卡点
把命名/文档/Windows setup 的问题整理成 issue
再提一个小 PR
```

这类 PR 是自然的，不是为了刷贡献而贡献。

## 18. 使用风险和注意事项

AutoAgent 的能力很强，但使用时要谨慎。

主要风险：

```text
自动写文件
自动运行命令
自动安装依赖
自动访问网页
自动读取文件
自动创建工具
自动创建 agent
```

如果接入真实金融数据、公司材料、客户资料、银行材料，必须先隔离：

```text
public demo workspace
private research workspace
company/confidential workspace
```

公开网站和 GitHub 只能使用：

```text
公开数据
自造样例
脱敏样例
获得授权的材料
```

这点和 RAG-Anything 一样。工具越强，边界越重要。

## 19. Study Map Position

现在 HKUDS map 可以更新成：

| ID | Project | Pengyi Interpretation |
|---|---|---|
| HKUDS000 | Study Map | 第一阶段项目地图 |
| HKUDS001 | LightRAG | source-grounded research memory |
| HKUDS002 | Vibe-Trading | agentic quant research workflow |
| HKUDS003 | nanobot | personal always-on agent shell |
| HKUDS004 | CLI-Anything | agent-native software action layer |
| HKUDS005 | AI-Trader | agent-native live trading platform layer |
| HKUDS006 | AgentSpace | organizational agent workspace |
| HKUDS007 | RAG-Anything | multimodal document ingestion and all-in-one RAG layer |
| HKUDS008 | AutoAgent | self-developing agent factory and zero-code workflow creation layer |

`HKUDS008` 的一句话总结：

```text
AutoAgent 是把自然语言需求编译成 tool、agent、workflow 的 agent factory。
```

它对我们最重要的启发是：

```text
AI scientist 不只是使用 agent，而是要拥有持续生产 agent 和 workflow 的系统。
```

这和我们想做的 Quant R&D Agent 完全同频。未来我们自己的系统也应该有一个 agent factory，但必须配上 PM review、sandbox、diff、audit、data governance。AutoAgent 提供了很好的研究原型，AgentSpace 提供了组织治理方向，二者合起来就是更接近真实可用的 Research OS。
