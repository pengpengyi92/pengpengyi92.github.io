---
title: "HKUDS014: DeepTutor 作为 Agent-Native Personalized Tutoring 与 AI Scientist Self-Training Layer"
date: 2026-06-25 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds014, hkuds, deeptutor, agent-native-tutoring, ai-scientist, learning-space, mastery-path, knowledge-base, memory, subagents, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第十五篇。

```text
HKUDS014 -> DeepTutor
```

上一篇 `HKUDS0000` 我们做了中场 map，把 HKUDS 已经看过的项目重新归到四条主线：

```text
1. Quant / Finance
2. Research OS / AI Scientist
3. Agent Framework / Workspace
4. RAG / Knowledge
```

`DeepTutor` 正好落在第二条和第三条之间：

```text
DeepTutor = Agent-Native Personalized Tutoring + AI Scientist Self-Training Layer
```

它不是一个普通 tutoring chatbot。

更准确地说，它是一个面向学习者的 agent-native learning workspace：

```text
Chat
Quiz
Research
Visualize
Solve
Mastery Path
Co-Writer
Book
Knowledge Center
Learning Space
Memory
Partners / My Agents
Settings / Capabilities / Tools / MCP
```

这些功能不是散的。

DeepTutor 的关键在于：它用统一的 agentic loop 把学习、答疑、研究、解题、可视化、知识库、记忆、子 agent 和长期 mastery path 接起来。

对于 Pengyi Research OS，这个项目的启发非常直接：

```text
我们不只是要让 AI 帮我们做 research。
我们还要让 AI 帮我们训练自己成为更强的 AI scientist。
```

所以这一篇重点不是“DeepTutor 能不能当一个教育产品”，而是：

```text
DeepTutor 如何成为个人 AI scientist / quant researcher 的 self-training operating layer？
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `DeepTutor`。

| Item | Value |
|---|---|
| repo | `DeepTutor` |
| remote | `https://github.com/HKUDS/DeepTutor.git` |
| branch | `main` |
| local head | `30b92dfe` |
| latest local commit date | `2026-06-24 16:24:08 +0800` |
| latest local commit | `release: v1.4.12` |
| local tag | `v1.4.12` |
| status | clean, synced with `origin/main` |
| tracked files by `rg --files` | 1632 |
| Python requirement | `>=3.11` |
| frontend | `Next.js 16`, `React 19` |
| license | `Apache-2.0` |
| paper badge | `arXiv:2604.26962` |
| docs | `https://deeptutor.info` |
| CLI entry | `deeptutor = deeptutor_cli.main:main` |

一句话先行：

```text
DeepTutor 把“学习”从一次性问答升级成了一个可持续的 agent runtime：
有知识库、有路径、有记忆、有工具、有子 agent、有 mastery gate、有可视化、有写作和材料沉淀。
```

## 它解决什么问题

如果只用普通 ChatGPT 学东西，常见问题是：

```text
今天问了一个问题，明天就断了。
看了一篇 paper，没有形成知识结构。
刷了一个 project，没有形成 mastery checkpoint。
写了一段理解，没有沉淀成 research statement / blog / notebook。
代码问题和概念问题分离，没有统一上下文。
AI 记不住我正在训练什么能力。
```

DeepTutor 想解决的是一个更长期的问题：

```text
如何让学习过程变成可持续、可追踪、可诊断、可复用的 agent workflow？
```

这对我们特别关键。

因为我们现在做 HKUDS / LLMQuant / X2Strategy / RD-Agent / quant research，不应该只是“看过很多项目”。

真正有价值的是：

```text
看项目
-> 结构化理解
-> 写 study map
-> 形成 mastery path
-> 做 quiz / solve / coding exercise
-> 写成 blog / research note
-> 进入长期 memory
-> 生成下一轮学习计划
```

DeepTutor 的系统设计正好对应这条链。

## 从 Tutoring Chatbot 到 Learning Workspace

DeepTutor README 的定位是：

```text
DeepTutor: Agent-Native Personalized Tutoring
```

它的 feature 列表里最关键的是这几句：

```text
One runtime for every mode
Connected learning context
Subagents and Partners
Multi-engine knowledge
Extensible tools and skills
Inspectable memory
```

这六句话基本概括了它的系统价值。

### One Runtime

DeepTutor 不是给每个功能写一套单独流程，而是把多种学习模式跑在同一个 agent loop 上。

README 里明确说：

```text
Chat, Quiz, Research, Visualize, Solve, Mastery Path
```

这些都跑在同一个 runtime 上。

这意味着用户切换的不是引擎，而是 objective。

```text
Chat       -> 普通对话和答疑
Quiz       -> 生成测试题和检查理解
Research   -> 深度研究
Visualize  -> 生成图表 / SVG / Mermaid / interactive visualization
Solve      -> 解题
Mastery    -> 带 gate 的学习路径
```

这对 Research OS 的启发是：

```text
不同研究任务不一定需要不同系统。
可以用统一 agent loop + 不同 capability / tool surface 来承载。
```

对于 Pengyi Quant Research OS，也是一样：

```text
factor ideation
data retrieval
implementation
backtest
bias diagnosis
report writing
PM review
```

这些也可以不是六个孤岛，而是一个统一 runtime 下的不同 capability。

### Connected Context

普通学习工具最大的问题是上下文碎片化。

DeepTutor 把这些东西放在同一个 workspace：

```text
Knowledge bases
Books
Co-Writer drafts
Notebooks
Question banks
Personas
Memory
```

这件事很重要。

因为真正的学习不是一次问答，而是持续地把材料、问题、答案、错题、写作、project、paper、个人偏好连接起来。

对我们来说，最理想的形态是：

```text
HKUDS project notes
LLMQuant project notes
WorldQuant sanitized factor notes
RA / PhD application materials
paper reading notes
blog drafts
code repo study notes
interview question bank
```

这些都应该进入一个可检索、可复用、可继续训练的上下文系统。

### Subagents and Partners

DeepTutor 现在已经能接入本地 `Claude Code` / `Codex`，并且可以在对话中 live consult。

这对我们很关键。

因为“学习一个 project”的时候，单纯 LLM 解释 README 还不够。

更强的工作流是：

```text
DeepTutor 负责学习路径和 mastery gate
Codex / Claude Code 负责进入本地 repo 读代码、定位模块、解释实现、提出练习
Knowledge Center 负责存材料
Memory 负责长期沉淀
Co-Writer 负责写成文章
```

这就把 learning agent 和 coding agent 打通了。

## 核心架构：Label-Driven Agentic Loop

我读了 `deeptutor/core/agentic/loop.py`。

这个文件是 DeepTutor 的系统核心之一。

它实现的是一个 label-driven iteration scheduler。

核心思想是：

```text
每一轮 LLM 输出一个 label
系统检查这个 label 是否符合协议
如果是终止 label，就结束
如果是 tool label，就调工具
如果是 intermediate label，就把内容加入上下文继续
如果协议错误，就注入 repair message 重试
```

抽象出来是：

```text
LabelProtocol
LoopHost
run_agentic_loop
```

`LabelProtocol` 负责描述 capability 的标签协议：

```text
allowed labels
terminal labels
intermediate labels
final labels
tool label
```

`LoopHost` 则把不同 capability 的差异封装成 hook：

```text
guard_context_window
build_iteration_trace_meta
dispatch_tools
resolve_pause
emit_final
validate_terminal
protocol_retry_notice
force_finalize
before_iteration
on_intermediate
```

这套设计的价值是：

```text
loop core 不关心自己是在做 chat、solve、research 还是 mastery。
不同任务只要实现 host / protocol / tools，就可以复用同一个 agent engine。
```

这就是 DeepTutor 从产品功能升级为 agent platform 的关键。

## 工具调用：Parallel Tool Dispatch

我读了 `deeptutor/core/agentic/tool_dispatch.py`。

它实现的是并行工具调用调度。

核心点：

```text
MAX_PARALLEL_TOOL_CALLS = 8
tool calls 会并行执行
每个 tool call 都会有 sub-trace
重复 tool call 会被折叠
ask_user 只允许一个 pause call 生效
tool result 会回填到 role=tool message
```

这说明 DeepTutor 的 agent loop 不是简单“LLM 回答一下”，而是具备比较完整的运行时控制：

```text
parallelism
deduplication
trace
pause / resume
terminate
source aggregation
tool metadata
```

对我们自己的 R&D Agent 很有启发。

比如 quant research agent 里，同一轮可以并行做：

```text
读取因子定义
拉取样本数据
查找历史 backtest
检索相关 paper
检查代码实现
读取 risk note
```

但需要防止：

```text
重复调用同一个工具
一次问多个 ask_user 卡住
工具结果没有 trace
工具失败后 agent 看不到原因
```

DeepTutor 的 dispatcher 是一个可学习的 reference。

## Capability Registry

我读了两个 registry：

```text
deeptutor/capabilities/registry.py
deeptutor/runtime/bootstrap/builtin_capabilities.py
```

`registry.py` 里注册的 loop capability 包括：

```text
MasteryLoopCapability
SolveLoopCapability
ObsidianCapability
SubagentCapability
ExploreContextCapability
```

`builtin_capabilities.py` 里注册的 builtin capability 包括：

```text
chat
deep_solve
deep_question
deep_research
math_animator
visualize
mastery_path
```

这说明 DeepTutor 的功能不是散装 routes，而是通过 capability manifest / registry 被统一管理。

这种方式适合扩展。

比如未来我们自己的 Pengyi Research OS 可以有：

```text
quant_factor_hypothesis
factor_implementation
backtest_diagnosis
paper_to_code
research_memo
pm_review
phd_application_statement
project_study_map
```

每一个都可以是 capability，而不是临时 prompt。

## Knowledge Center：多引擎 RAG

我读了 `deeptutor/services/rag/factory.py`。

DeepTutor 的 RAG pipeline factory 支持：

```text
llamaindex
pageindex
graphrag
lightrag
lightrag-server
```

它不是把 RAG 写死在一个 vector store 里，而是做了 provider abstraction。

| Provider | 定位 |
|---|---|
| `llamaindex` | 默认本地 vector retrieval，支持 hybrid BM25/vector fusion |
| `pageindex` | hosted vectorless reasoning retrieval，需要 API key |
| `graphrag` | local knowledge-graph retrieval，需要可选依赖 |
| `lightrag` | HKUDS LightRAG graph + vector retrieval，可和 RAG-Anything multimodal parsing 结合 |
| `lightrag-server` | 外部 standalone LightRAG server，通过 HTTP 查询 |

这个设计直接把我们前面看过的 HKUDS 主线接上了：

```text
HKUDS001 LightRAG
HKUDS007 RAG-Anything
HKUDS014 DeepTutor
```

DeepTutor 可以把 LightRAG / GraphRAG / PageIndex 这些知识层吸收到学习 workspace 里。

换句话说：

```text
LightRAG 是 knowledge memory engine。
RAG-Anything 是 multimodal document ingestion engine。
DeepTutor 是使用这些 knowledge engine 来训练人的 learning workspace。
```

这层关系很关键。

## Document Parsing Engines

我读了 `deeptutor/services/parsing/engines/factory.py`。

DeepTutor 的 document parsing engine 支持：

```text
text_only
mineru
docling
markitdown
pymupdf4llm
```

| Engine | 定位 |
|---|---|
| `text_only` | 内置轻量文本抽取 |
| `mineru` | 高保真 multimodal parsing，适合 layout / table / formula |
| `docling` | structured document conversion |
| `markitdown` | 轻量 Markdown 输出，格式支持广 |
| `pymupdf4llm` | 轻量 PDF/e-book -> Markdown，可抽图片 |

这是一个实际产品必须处理的现实问题。

对于我们做 AI scientist / quant researcher，材料来源一定非常复杂：

```text
papers
PDF books
lecture slides
research reports
company docs
notebooks
code README
Markdown notes
Excel / docx / pptx
```

如果 ingestion 层不稳，上层 learning / research agent 都会不稳。

DeepTutor 的做法是：

```text
把 parsing engine 做成可选、懒加载、可在 Settings 里选择的组件。
```

这比“只支持一种 PDF loader”更接近真实工作流。

## Memory：L1 / L2 / L3 三层记忆

我读了 `deeptutor/services/memory/store.py`。

DeepTutor 的 memory 不是一个简单的 `memory.txt`。

它把 memory 分成三层：

```text
L1 traces
L2 surface summaries
L3 synthesis
```

从代码上看，`MemoryStore` 是三层 memory 的 high-level facade：

```text
emit L1 event
read L2 / L3 doc
update L2
update L3
apply ops payload
write preference
overview
```

这套设计很适合长期学习。

因为学习系统需要同时记录：

```text
具体发生了什么
这一类 surface 最近学到了什么
跨 surface 的长期综合结论
个人偏好和目标变化
```

普通 RAG memory 往往只有“检索过去聊天记录”。

DeepTutor 的 memory 更像：

```text
trace -> summary -> synthesis -> editable memory graph
```

对于 Pengyi Research OS，我们也需要类似结构：

```text
L1: 每次读 paper / repo / backtest / meeting 的原始 trace
L2: 每条主线的阶段性总结，比如 HKUDS、LLMQuant、Quant OS、RA application
L3: 个人长期 research thesis、career narrative、能力画像、偏好和路线
```

这是一个很好的参考。

## Mastery Path：不是“看懂了”，而是“过关了”

DeepTutor 最值得我们学习的地方之一是 `Mastery Path`。

我读了：

```text
deeptutor/capabilities/mastery/capability.py
deeptutor/capabilities/mastery/loop.py
deeptutor/learning/mastery.py
deeptutor/learning/service.py
```

`MasteryPathCapability` 的注释很关键：

```text
There is no bespoke state machine here anymore.
The chat agent loop IS the tutor.
```

也就是说，Mastery Path 不重新发明一个全新的 tutoring state machine，而是复用标准 agentic chat pipeline。

它只做几件关键事：

```text
把 context 标记为 mastery_mode
解析 mastery_path_id
挂载 mastery tools
注入 tutor playbook
把 deterministic gate 交给 learning engine
```

工具包括：

```text
mastery_status
mastery_quiz
mastery_grade
mastery_assess
mastery_build
```

真正重要的是这句话：

```text
the model decides what to teach and how to question
while the gate that decides whether the learner may advance is a deterministic engine call
```

这是非常好的工程判断。

LLM 适合：

```text
解释
追问
生成类比
设计题目
根据回答调整教学方式
```

但“能不能过关”不应该完全交给 LLM 随口判断。

DeepTutor 把 gate 设计成 deterministic engine。

### Mastery Scoring

`deeptutor/learning/mastery.py` 里有一个非常简单但清晰的 mastery policy：

```text
recent attempts
recency weighted accuracy
low-confidence cap
```

核心意思是：

```text
新的答题结果权重更高。
一两次答对不能直接宣布 mastered。
掌握程度需要足够证据。
```

这对我们做自我训练非常重要。

因为人很容易产生“我看懂了”的错觉。

真正能形成能力，需要：

```text
看懂
复述
做题
写代码
解释给别人
处理变体
隔几天还能做出来
```

DeepTutor 的 Mastery Path 至少已经把“学习必须过 gate”这件事系统化了。

## Subagents：Codex / Claude Code 进入 Learning Loop

我读了：

```text
deeptutor/services/subagent/codex.py
deeptutor/services/subagent/claude_code.py
```

Codex backend 通过本地 `codex` CLI 运行：

```text
codex exec --json --skip-git-repo-check
codex exec resume <session_id>
```

它会读取 JSONL event stream，并把：

```text
reasoning
command executions
file changes
MCP tool calls
agent messages
turn completed / failed
```

映射成 DeepTutor 的 sidebar event。

Claude Code backend 通过本地 `claude` CLI 运行：

```text
claude -p <question> --output-format stream-json --verbose --include-partial-messages
```

同样把 assistant text、tool_use、tool_result、result 等事件流式映射回来。

这说明 DeepTutor 不是只把 Codex / Claude 当作外部链接，而是把它们变成 live subagent。

对我们来说，这个功能的想象空间很大：

```text
我在 DeepTutor 里学习 HKUDS DeepCode。
DeepTutor 发现我需要理解 repo 结构。
它 live consult Codex。
Codex 进入本地 repo 读代码、解释关键模块、提出练习。
DeepTutor 把练习结果写入 Mastery Path。
Memory 把我的薄弱点沉淀下来。
Co-Writer 把理解写成 HKUDS study post。
```

这就是“学习系统 + coding agent”的组合。

## CLI：Agent-First Interface

我读了 `deeptutor_cli/main.py`。

DeepTutor CLI 的入口是：

```text
deeptutor
```

它包含：

```text
deeptutor start
deeptutor serve
deeptutor run
deeptutor chat
deeptutor kb
deeptutor skill
deeptutor memory
deeptutor partner
deeptutor plugin
deeptutor config
deeptutor session
deeptutor notebook
deeptutor provider
deeptutor book
```

`deeptutor run` 很关键，因为它可以直接运行任意 capability：

```text
deeptutor run chat "..."
deeptutor run deep_solve "..."
deeptutor run deep_research "..."
deeptutor run visualize "..."
deeptutor run math_animator "..."
deeptutor run mastery_path "..."
```

这说明 DeepTutor 不是纯 Web 产品。

它还有 command-line surface，可以被脚本化、接进自动化工作流。

对于我们这种 build-in-public / research OS 方向，CLI 很重要。

未来可以有：

```text
deeptutor run mastery_path "Build a mastery path for HKUDS LightRAG"
deeptutor kb add "LLMQuant papers"
deeptutor memory update
deeptutor notebook import hkuds014.md
```

然后把学习变成可复现的 pipeline。

## Frontend Surfaces

从 `web/app` 路由看，DeepTutor 的前端已经不是一个小 demo。

主要 surface 包括：

```text
home / chat
partners
co-writer
book
knowledge
space
space / learning
space / questions
space / skills
space / personas
space / notebooks
space / chat-history
agents
memory
memory / graph
memory / l1
memory / l2
memory / l3
settings
settings / agents / codex
settings / agents / claude-code
settings / capabilities
settings / document-parsing
settings / mcp
settings / models
profile
```

这其实已经很接近一个 personal learning OS。

对我们来说，最值得学习的是它的 product surface 组织：

```text
学习空间 Space
知识中心 Knowledge
长期记忆 Memory
子代理 Agents
伙伴 Partners
写作 Co-Writer
书 Book
设置 Settings
```

如果我们将来做 Pengyi Research OS，也可以类似分 surface：

```text
Research Space
Knowledge Center
Experiment Center
Factor Library
Backtest Lab
Report Writer
PM Review
Memory Graph
Agent Console
Application / Career Materials
```

## Release Timeline 给我们的启发

DeepTutor 的 release 节奏很快。

最近几个版本特别值得看：

| Date | Version | Key idea |
|---|---|---|
| 2026-06-24 | `v1.4.12` | LightRAG Server retrieval engine, PyMuPDF4LLM parsing, FAISS vector backend |
| 2026-06-23 | `v1.4.11` | cloud OpenAI-compatible provider native tool calling, admin Users, LaTeX quiz options |
| 2026-06-21 | `v1.4.10` | Profile page, rootless container guide, deny-by-default MCP tools |
| 2026-06-18 | `v1.4.8` | Partners under My Agents, live consultation, private memory |
| 2026-06-18 | `v1.4.7` | local Claude Code / Codex live mid-turn |
| 2026-06-17 | `v1.4.6` | Space, My Agents, Memory, Knowledge Center consolidation |
| 2026-06-14 | `v1.4.5` | Guided Learning rebuilt on chat agent loop, hard mastery gate |
| 2026-05-22 | `v1.4.0` | Auto Mode, three-layer Memory, Deep Research / Solve / Question, restart-safe runtime |
| 2026-04-04 | `v1.0.0-beta.1` | agent-native rewrite: Tools + Capabilities, CLI & SDK, TutorBot, Co-Writer, memory |

这条时间线说明它在快速从：

```text
tutoring app
```

升级成：

```text
agent-native learning workspace
```

最近几个版本最有价值的变化是：

```text
Knowledge Center 变强
Memory 变强
Subagent 变强
Mastery Path 变强
CLI / capabilities 变强
Deployment / multi-user / security 变强
```

这其实也是一个开源项目从 demo 到 platform 的路径。

## 和前面 HKUDS 项目的关系

DeepTutor 不孤立。

它和前面做过的项目关系非常强。

| Project | 和 DeepTutor 的关系 |
|---|---|
| `LightRAG` | DeepTutor 可使用 LightRAG 作为 graph + vector knowledge engine |
| `RAG-Anything` | DeepTutor 的 LightRAG multimodal ingestion 可以接 RAG-Anything |
| `nanobot` | nanobot 偏 personal agent shell，DeepTutor 偏 learning workspace |
| `CLI-Anything` | CLI-Anything 是 software action layer，DeepTutor 通过 tools / MCP / subagents 执行动作 |
| `AgentSpace` | AgentSpace 偏 organizational agent workspace，DeepTutor 偏个人学习 workspace |
| `AutoAgent` | AutoAgent 偏 agent factory，DeepTutor 偏使用 agent loop 承载学习场景 |
| `DeepCode` | DeepCode 做 paper2code，DeepTutor 可以训练用户理解和复现 paper2code |
| `AI-Researcher` | AI-Researcher 是 autonomous scientific discovery，DeepTutor 是训练人类 research operator |
| `DeepInnovator` | DeepInnovator 训练 idea model，DeepTutor 训练人的 idea / paper / code mastery |
| `Auto-Deep-Research` | Auto-Deep-Research 生成 deep research report，DeepTutor 可训练用户读、评、写 report |
| `DeepResearch-Eval` | DeepResearch-Eval 评估 report，DeepTutor 可以把评估结果纳入学习反馈 |

所以 DeepTutor 的系统位置可以这样画：

```text
LightRAG / RAG-Anything
        ↓
Knowledge Center
        ↓
DeepTutor learning workspace
        ↓
Mastery Path + Memory + Co-Writer
        ↓
AI Scientist self-training loop
```

如果放到 HKUDS0000 的四条主线里：

| 主线 | DeepTutor 的位置 |
|---|---|
| Quant / Finance | 可作为 quant learning / factor research training layer |
| Research OS / AI Scientist | 核心 self-training layer |
| Agent Framework / Workspace | 使用统一 agent loop、capabilities、subagents、partners |
| RAG / Knowledge | 使用 Knowledge Center、多引擎 RAG、document parsing、Memory |

## 对 Pengyi Research OS 的启发

DeepTutor 对我们最大的启发是：

```text
Research OS 不只要服务产出，也要服务训练。
```

我们现在的目标不是只做一个网站，也不是只读项目。

我们要逐渐形成：

```text
个人 AI scientist training loop
个人 quant researcher training loop
个人 project study loop
个人 writing loop
个人 career narrative loop
```

DeepTutor 提供了一个架构参考。

### Pengyi 版本的 Learning Loop

可以抽象成：

```text
ingest
-> understand
-> quiz
-> solve
-> code
-> write
-> evaluate
-> memory
-> next mastery path
```

对应到我们的工作：

| Stage | Pengyi action |
|---|---|
| ingest | 导入 paper / GitHub repo / README / notes / pdf |
| understand | 用 agent 解释项目结构和关键组件 |
| quiz | 生成理解题和概念题 |
| solve | 做代码练习、数学推导、系统设计题 |
| code | 用 Codex / Claude Code 读仓库、改 demo、提 PR |
| write | 写成 HKUDS / LLMQuant / Quant OS blog |
| evaluate | 用 report evaluator / human PM review 检查质量 |
| memory | 写入 long-term memory 和 learning map |
| next plan | 生成下一轮 project / paper / PR 计划 |

这就是我们要的 AI scientist self-training layer。

### 对 Quant Research 的启发

如果把 DeepTutor 的思想迁移到 quant research：

```text
Knowledge Center -> paper / factor / market microstructure / risk notes
Mastery Path -> factor research skill tree
Quiz -> interview / theory / implementation question bank
Solve -> coding / backtest / stats problem solving
Memory -> personal research preference and past mistakes
Co-Writer -> research memo / strategy note / PM update
Subagent -> Codex reads local backtest repo and implements experiment
Partner -> long-running quant companion / PM reviewer
```

这会形成一个很强的系统：

```text
Quant learning OS
+ Quant research OS
+ Quant writing OS
+ Quant interview OS
```

## 可以快速应用的地方

我觉得 DeepTutor 对我们当前最实用的应用有五个。

### 1. Project Study Mode

给每个项目建立一个 mastery path：

```text
HKUDS LightRAG
HKUDS DeepCode
HKUDS DeepTutor
LLMQuant DataMCP
LLMQuant QuantMind
X2Strategy
RD-Agent
```

每个项目都拆成：

```text
用途
架构
核心模块
实现方式
可复现 demo
可 PR 的地方
和 Pengyi Research OS 的连接
```

然后用 quiz / solve 检查自己是否真的掌握。

### 2. Paper-to-Mastery

对于 paper，不只是总结 abstract。

应该生成：

```text
problem
motivation
method
algorithm
experiment
limitations
implementation plan
comparison
possible extension
```

然后进入 mastery path。

### 3. Repo-to-Exercise

通过 Codex / Claude Code 进入本地 repo：

```text
读目录
读关键模块
解释调用链
生成实现练习
定位可改进 issue
写 PR draft
```

DeepTutor 负责学习路径，Codex 负责工程细节。

### 4. Writing-to-Memory

每次学习之后，不只是聊天结束。

要输出：

```text
blog post
study map
research memo
PR note
interview question
application statement material
```

然后写入 Memory。

### 5. Career / Application Training

DeepTutor 的 Learning Space 也可以用于 RA / PhD / quant job 申请：

```text
CV question bank
PS / RP mastery checklist
PI pitch rehearsal
quant interview drills
project explanation rehearsal
research narrative review
```

这和我们最近做的河套、黄超老师、金敦泓老师、子健哥沟通材料可以接起来。

## 可以提 PR 的方向

我们提 PR 的原则仍然是：

```text
先真实使用
发现真实问题
提出小而清晰的改进
```

DeepTutor 上我看到一些可能的 PR 方向。

### Docs: Researcher Self-Training Workflow

可以补一篇文档：

```text
How to use DeepTutor as a researcher self-training workspace
```

内容包括：

```text
导入 paper / repo notes
建立 Knowledge Base
生成 Mastery Path
使用 Quiz / Solve
调用 Codex / Claude Code subagent
导出 Co-Writer note
更新 Memory
```

这和我们自己的使用场景高度一致。

### Template: AI Scientist Learning Path

可以提供一个模板：

```text
AI Scientist Learning Path
```

模块包括：

```text
LLM agents
RAG / knowledge
paper reading
paper reproduction
benchmark design
code implementation
technical writing
open-source contribution
```

### Template: Quant Research Learning Path

可以提供一个 quant 版本：

```text
Quant Research Learning Path
```

模块包括：

```text
market data
alpha factor
backtest
risk
portfolio
execution
research memo
PM review
```

注意公开版本必须脱敏，只放 toy / public data。

### Example: DeepTutor + LightRAG Server

`v1.4.12` 新增了 LightRAG Server retrieval engine。

可以补一个最小 example：

```text
run LightRAG server
connect DeepTutor KB
query from Knowledge Center
use it inside Mastery Path
```

### Export: Mastery Path to Markdown / Website

我们非常需要：

```text
把学习路径导出为 markdown
再发布到 GitHub Pages learning log
```

如果项目里已有相关能力，可以补文档；如果没有，可以先提 issue 或小 PR。

### Docs: Local Codex Subagent Study Workflow

可以写一个示例：

```text
Use Codex subagent to study a local GitHub repository
```

步骤：

```text
set Codex in Settings
open repo path
ask DeepTutor to consult Codex
turn explanation into mastery quiz
save result to notebook
```

这正是我们正在做的事。

### Privacy Checklist

DeepTutor 涉及：

```text
uploaded materials
memory
partner channels
IM integrations
subagents
MCP tools
multi-user deployment
```

可以补一份 privacy / security checklist。

这对教育、科研、企业内部使用都很重要。

## 和我们网站内容的连接

这篇 `HKUDS014` 可以放在我们网站学习内容里，作为 HKUDS 主线的新节点。

它的位置是：

```text
HKUDS0000 -> 中场 map
HKUDS014  -> DeepTutor self-training layer
```

后面可以继续做：

```text
HKUDS015 -> OpenHarness
HKUDS016 -> UpSkill
HKUDS017 -> AnyTool
HKUDS018 -> MiniRAG
HKUDS019 -> Paper2Slides
```

DeepTutor 之后最好接 `OpenHarness` 或 `UpSkill`。

原因：

```text
DeepTutor 解决“怎么训练自己”
OpenHarness 可能解决“怎么评测/运行 agent”
UpSkill 可能解决“怎么技能化/能力升级”
```

这条线会更完整。

## 我们自己的最小落地方案

短期不用先部署完整 DeepTutor。

我们可以先吸收它的结构，做一个 Pengyi 版本的最小 self-training loop。

### Step 1: 每篇 study post 都有 Mastery Checklist

从 `HKUDS014` 开始，每篇可以附一个 checklist：

```text
我能一句话解释项目吗？
我能画出核心架构吗？
我能指出三个关键模块吗？
我能说清楚和 Pengyi Research OS 的关系吗？
我能找到一个真实可提 PR 的方向吗？
我能设计一个最小复现实验吗？
```

### Step 2: 每个项目建立 Question Bank

比如 DeepTutor 的 question bank：

```text
DeepTutor 为什么叫 agent-native？
LabelProtocol 和 LoopHost 分别解决什么？
为什么 mastery gate 不应该完全交给 LLM？
L1/L2/L3 memory 有什么区别？
LightRAG Server 在 DeepTutor 里解决什么？
Codex subagent 是怎样接入的？
```

### Step 3: 每个项目建立 PR Candidate List

不要为了 PR 而 PR。

每个项目先写：

```text
真实使用场景
我遇到的问题
项目当前文档缺口
最小可验证改动
风险和边界
```

然后再考虑 issue / PR。

### Step 4: 每周整理 Memory

每周把学习内容沉淀成：

```text
本周学到的系统设计模式
本周可复用模块
本周遇到的知识薄弱点
本周可提 PR 方向
下周 mastery path
```

这就是 DeepTutor 给我们的真实启发。

## 这篇的总结

`HKUDS014` 的一句话总结：

```text
DeepTutor 是一个 agent-native personalized learning workspace。
它把 Chat / Quiz / Research / Solve / Visualize / Mastery Path / Knowledge / Memory / Subagents / Co-Writer / Partners 接到同一个学习运行时里。
对 Pengyi Research OS 来说，它最重要的意义是：把 AI scientist 的成长过程也系统化、可追踪化、可训练化。
```

如果说前面几个项目分别是：

```text
LightRAG       -> knowledge memory
RAG-Anything   -> multimodal ingestion
DeepCode       -> paper to code
AI-Researcher  -> autonomous scientific discovery
DeepInnovator  -> idea model training
Auto-Deep-Research -> research report generation
DeepResearch-Eval  -> research report evaluation
```

那么 DeepTutor 的位置就是：

```text
DeepTutor -> train the human operator behind the Research OS
```

这对我们非常关键。

因为最终真正要变强的，不只是 agent。

是我们自己。
