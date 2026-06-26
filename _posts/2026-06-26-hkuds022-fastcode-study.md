---
title: "HKUDS022: FastCode 作为 Code Intelligence Acceleration 与 Repo-Level Research Engineering Layer"
date: 2026-06-26 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds022, hkuds, fastcode, code-intelligence, repo-understanding, mcp, ai-coding, research-engineering, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第二十三篇。

```text
HKUDS022 -> FastCode
```

上一篇 `HKUDS021` 看的是 `VideoRAG`：

```text
VideoRAG gives us video memory.
```

这一篇进入 `FastCode`：

```text
FastCode gives us coding speed.
```

这句话非常关键。因为我们现在反复确认的一件事是：

```text
只要我们在 coding，就在产出。
只要我们在读 repo，就在积累工程判断。
只要我们能更快理解代码，就能更快提出 PR、做系统、写研究。
```

所以 `FastCode` 不是一个普通的代码问答 demo。它更像是一个 repo-level 的代码理解加速层：

```text
repo -> semantic structure -> vector / BM25 / graph index -> iterative code scout -> grounded answer
```

这非常适合放进我们的 `Pengyi Research OS` 和 `Pengyi Quant Research OS`。因为我们后面要长期读 HKUDS、LLMQuant、量化系统、agent framework、开源 trading repo。单靠人工一层层点文件，会慢；单靠大模型把全仓库塞进上下文，会贵且不稳。FastCode 的核心价值就是把 repo 先结构化，让 agent 有地图，再回答问题。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `FastCode`。

| Item | Value |
|---|---|
| repo | `FastCode` |
| remote | `https://github.com/HKUDS/FastCode.git` |
| branch | `main` |
| local head | `590e49b` |
| full commit | `590e49bacad6bc94a733d41119a2680c24af822d` |
| latest local commit date | `2026-03-20 02:21:41 +0800` |
| latest local commit | `Add MCP tool expansion and incremental indexing` |
| status | clean, synced with `origin/main` after fetch |
| tracked files by `git ls-files` | 121 |
| Python files | 81 |
| TypeScript / JavaScript files | 4 |
| main folders | `fastcode`, `nanobot`, `assets`, `config` |
| main entrypoints | `main.py`, `api.py`, `mcp_server.py`, `web_app.py` |
| Python requirement in README | Python 3.12+ |
| syntax check | `py -m compileall -q fastcode main.py api.py mcp_server.py web_app.py` passed |
| import smoke | failed locally because dependencies are not installed: `rank_bm25` missing |
| license state | README says MIT and links `LICENSE`, but root `LICENSE` file is missing locally |

一句话先行：

```text
FastCode 把代码仓库拆成 file / class / function / documentation 四层 CodeElement，
再用 semantic embedding、BM25、call/dependency/inheritance graph 和 iterative agent，
让模型用更少 token、更快定位、更稳地理解一个 repo。
```

## 它解决什么问题

普通 AI coding workflow 经常有几个问题：

```text
不知道该先读哪个文件
一上来就把大量代码塞进上下文
读到局部实现但看不到调用关系
跨 repo 时容易把不同项目的符号混在一起
问答之后缺少长期 session memory
大仓库 token 成本很高
```

FastCode 的判断是：

```text
代码理解不能只靠 raw context stuffing。
应该先构建 repo map，再按问题动态检索、导航、补上下文。
```

这和我们读项目的方式很像。我们真正需要的不是“把整个仓库复制给模型”，而是：

```text
这个项目是干什么的？
入口在哪里？
核心类和函数在哪里？
数据流怎么走？
调用链怎么走？
哪些文件值得先读？
有没有明显的文档/代码不一致？
哪里能提 PR？
```

FastCode 的定位就是让这些问题变成系统能力。

## 总体架构

FastCode 可以拆成九层：

| Layer | Component | Role |
|---|---|---|
| Repository Loading | `RepositoryLoader` | clone / load local repo / upload zip / scan files |
| Parsing | `CodeParser` | AST / tree-sitter / language-specific extraction |
| Indexing | `CodeIndexer` | build `CodeElement` for file/class/function/docs |
| Embedding | `CodeEmbedder` | turn code elements into semantic vectors |
| Vector Store | `VectorStore` | FAISS persistence and metadata storage |
| Graph Building | `CodeGraphBuilder` | call graph, dependency graph, inheritance graph |
| Retrieval | `HybridRetriever` | semantic + BM25 + repo overview + file selection |
| Iterative Agent | `IterativeAgent` + `AgentTools` | multi-round code exploration under budget |
| Serving | Web / REST / MCP / Nanobot | expose FastCode to browser, API, IDE agent, Feishu |

整体链路是：

```text
repo source
  -> load files
  -> parse AST / imports / functions / classes
  -> create CodeElement objects
  -> embed code elements
  -> build vector store
  -> build BM25 index
  -> build call/dependency/inheritance graphs
  -> generate repo overview
  -> answer query with hybrid retrieval and iterative exploration
```

这是一套比较完整的 code intelligence stack。

## CodeElement 是核心对象

FastCode 的最小知识对象是 `CodeElement`。它不是只按文件切 chunk，而是按结构切：

```text
file
class
function / method
documentation
```

每个 element 会带上：

```text
id
type
name
file_path / relative_path
language
start_line / end_line
code
signature
docstring
summary
metadata
repo_name
url
```

这比普通 RAG chunk 更适合代码。因为代码理解里，边界很重要：

```text
函数边界
类边界
文件边界
import 边界
调用边界
继承边界
```

如果只按固定 token chunk 切，会很容易把函数切断，也很难回答“这个 class 有哪些方法”“这个函数在哪里被调用”这类问题。

FastCode 的第一层贡献就是：

```text
把代码从文本块变成结构化对象。
```

## Parser 层

`CodeParser` 负责从源码中提取结构。它支持多语言：

```text
Python
JavaScript / TypeScript
Java
Go
C / C++
Rust
C#
```

Python 侧主要走 AST；其他语言会结合 tree-sitter / generic parser。

这里有几个工程细节值得学：

```text
提取 function / class / import / docstring
计算复杂度
保留行号
对生成代码里的语法异常做局部修复
递归访问 If / Try 等 top-level block，避免隐藏定义漏掉
```

这说明 FastCode 不是只做表层 grep。它真正在做 repo understanding 的第一步：

```text
code parsing as structural indexing.
```

对我们以后读量化系统很重要。比如一个 backtest engine，最关键的往往不是某个 README，而是：

```text
data loader
factor computation
portfolio construction
order simulation
slippage / fee model
risk attribution
result reporting
```

这些都需要从代码结构里抽出来。

## Indexer 层

`CodeIndexer` 把 parser 结果变成四层索引对象。

大致逻辑是：

```text
scan supported files
parse file
add file-level element
add class-level element
add function/method-level element
add documentation element
generate repository overview
embed all elements
save embedding and embedding_text
```

这个设计比较稳，因为它同时保留了粗粒度和细粒度：

| Granularity | Value |
|---|---|
| repository overview | 先判断这个 repo 是否相关 |
| file element | 理解模块职责 |
| class element | 理解对象抽象 |
| function element | 定位具体逻辑 |
| documentation element | 使用说明和设计意图 |

我们自己之后做 `Pengyi Repo Study Accelerator` 也应该沿用这个思路：

```text
先 repo overview
再 module map
再 class/function drill-down
最后生成学习笔记和 PR checklist
```

## Graph 层

FastCode 构建三类图：

```text
call graph
dependency graph
inheritance graph
```

对应的问题分别是：

| Graph | Question |
|---|---|
| call graph | 这个函数调用谁？谁调用它？ |
| dependency graph | 这个文件依赖哪些模块？模块之间怎么连？ |
| inheritance graph | class 之间的继承关系是什么？ |

`CodeGraphBuilder` 里有几个值得注意的点：

```text
dependency graph 只加文件节点
inheritance graph 只加 class 节点
call graph 加 function / method / class 节点
ModuleResolver 负责 import 到文件的解析
SymbolResolver 负责 inheritance / symbol resolution
避免跨 repo 符号混淆
```

这层非常关键。因为代码问答里，很多答案不是“相似文本”能找出来的，而是要走结构关系。

比如：

```text
query: 为什么这个 API 返回的结果会被过滤？

需要看的可能是：
endpoint handler
service function
query builder
filter predicate
config default
test fixture
```

单纯 semantic search 可能只找到 handler。graph 能帮助继续走调用链。

## Retrieval 层

FastCode 的检索不是单一路径，而是 hybrid。

标准流程大致是：

```text
query process / rewrite
repository overview selection
semantic vector search
pseudocode semantic search
BM25 keyword search
combine scores
rerank by element type
filter
diversify
optional LLM file selection
final repo safety filter
```

多 repo 场景下，它会先做 repo selection：

```text
user query
  -> repo overviews
  -> select relevant repositories
  -> load selected indexes
  -> retrieve inside selected repos
```

这点对我们很有用。因为我们本地现在有很多项目：

```text
HKUDS
LLMQuant
X2Strategy
Yuandong Tian related repos
个人网站
Research OS notes
```

如果未来统一进入一个 code knowledge workspace，第一步必须是 repo selection。否则问一个问题，系统可能在完全无关的 repo 里找答案。

## Iterative Agent

FastCode 最有意思的地方是 `IterativeAgent`。

它不是一次检索就结束，而是多轮探索：

```text
Round 1: 初始检索与判断
Round 2+: 评估当前信息是否足够
         决定保留/丢弃哪些文件
         调用 read/search/list 等工具补上下文
         计算 confidence / ROI / line budget
         满足条件后停止
```

它的停止条件不是简单轮数，而是结合：

```text
confidence threshold
marginal confidence gain
tool call usefulness
line budget
query complexity
repo size
iteration count
```

这个设计很重要。因为真正读 repo 时，最难的是“何时停止”：

```text
读太少 -> 答案不稳
读太多 -> token 爆炸
无限探索 -> 没有产出
```

FastCode 的思路是把代码阅读变成一个有预算的探索过程：

```text
code understanding as budgeted investigation.
```

这很适合接到我们的工作流。比如我们以后看一个量化 repo：

```text
目标：找一个可提 PR 的 improvement
预算：最多读 12,000 行
策略：先 overview，再核心模块，再文档/代码不一致，再最小 PR
输出：issue / PR draft / study note
```

这就是工程化的 repo study。

## AgentTools：只读安全工具层

`AgentTools` 给 iterative agent 提供实际探索工具：

```text
list_directory
search_codebase
get_file_info
read file ranges
```

它有两个好的工程取舍：

```text
只读
限定 repo_root 安全边界
自动避开 .git / venv / node_modules / hidden dirs
```

这说明 FastCode 的 agent 不是直接拿 shell 乱跑，而是通过受控工具看代码。

这对我们以后做 Research OS 也有启发：

```text
研究 agent 可以读数据、读代码、跑回测，
但不同阶段应该有不同权限。
```

比如：

| Stage | Permission |
|---|---|
| study | read-only |
| hypothesis | write note/json only |
| implementation | edit feature branch |
| backtest | run controlled scripts |
| publish | human approval required |

这比“全权限 agent”更可控。

## Incremental Indexing

FastCode 最近的 commit 明确加入了 incremental indexing。

当前 manifest 记录的是：

```text
mtime
size
element_ids
```

变化检测逻辑是：

```text
added
modified
deleted
unchanged
```

然后复用 unchanged 文件的 metadata / embeddings，只对 changed files 重新 index，最后重建：

```text
FAISS
BM25
graphs
manifest
```

这非常实用。因为真实 repo 会频繁变动，不能每次都全量索引。

不过这里也暴露出一个 PR 机会：`utils.py` 里已经有 `compute_file_hash()`，但 manifest 当前主要用 `mtime + size`。长期看，hash 会更稳：

```text
mtime 可能因为 checkout / copy / restore 变化
size 相同不代表内容相同
content hash 更适合判断真实内容变化
```

可以做一个小改进：

```text
manifest 增加 hash 字段
优先比较 hash
mtime/size 作为快速 precheck
```

这是一个很适合我们提的工程型 PR。

## Serving Surfaces

FastCode 不只是一段 library，它已经有多个使用入口。

| Surface | File | Use |
|---|---|---|
| CLI | `main.py` | 命令行 query |
| REST API | `api.py` | FastAPI service |
| Web UI | `web_app.py` + `web_interface.html` | 浏览器界面 |
| MCP Server | `mcp_server.py` | 给 Cursor / Claude Code / Windsurf 等 IDE agent 用 |
| Nanobot | `nanobot/` | 接入 Feishu 等消息渠道 |

REST API 暴露了完整生命周期：

```text
/load
/index
/load-and-index
/load-repositories
/index-multiple
/upload-zip
/query
/query-stream
/new-session
/sessions
/summary
/status
/clear-cache
/refresh-index-cache
/repository
```

MCP 更关键。因为它让 FastCode 能作为 coding agent 的外部工具。

README 里列出的 MCP tools 包括：

```text
code_qa
list_indexed_repos
list_sessions
get_session_history
delete_session
delete_repo_metadata
```

但本地 `mcp_server.py` 实际还包括：

```text
search_symbol
get_repo_structure
get_file_summary
get_call_chain
reindex_repo
```

这说明 README 和实现已经有一点不同步。这也是一个非常清晰的小 PR：

```text
更新 README 的 MCP tool list，
把新增工具的参数和使用示例补上。
```

这类 PR 很适合我们：

```text
低风险
边界清楚
对项目有实际帮助
能证明我们认真读了代码
```

## Nanobot / Feishu 层

FastCode 还把 Nanobot 放进来了。README 里的架构是：

```text
Feishu User
  -> Feishu Open Platform
  -> Nanobot WebSocket
  -> FastCode API
```

Nanobot 注册的工具包括：

```text
fastcode_load_repo
fastcode_query
fastcode_list_repos
fastcode_status
fastcode_session
```

这个方向非常有启发。因为它说明 code intelligence 不一定只在 IDE 里用，也可以进入组织协作工具：

```text
Feishu / Slack / Discord / Teams
```

未来团队里可以直接问：

```text
这个 repo 的数据入口在哪里？
这个函数谁调用？
最新 PR 改动会影响哪些模块？
这个 bug 应该先看哪些文件？
```

这和我们一直说的组织生产力有关。AI 不只是个人 IDE 插件，也可以成为组织内部的代码知识接口。

## 和 DeepCode 的区别

前面我们看过 `DeepCode`。它和 FastCode 都属于 coding / research engineering 主线，但位置不同。

| Repo | Main Role |
|---|---|
| `DeepCode` | research-to-code / paper-to-implementation / code generation |
| `FastCode` | repo-level code understanding / navigation / retrieval |

更直白地说：

```text
DeepCode helps us create code from research.
FastCode helps us understand existing code faster.
```

两者可以连起来：

```text
FastCode
  -> read existing repo
  -> find architecture and constraints
  -> identify missing feature or improvement

DeepCode
  -> generate implementation plan
  -> produce code
  -> support experiment / benchmark
```

如果我们要做开源贡献，FastCode 可能更先用上。因为提 PR 的第一步不是生成代码，而是理解项目：

```text
read repo
understand boundary
find improvement
make minimal patch
write clear PR
```

FastCode 正好服务这个流程。

## 和 Quant Research OS 的关系

FastCode 对量化的意义不只是“帮我写代码”。更重要的是，它能帮助我们快速理解量化开源系统：

```text
backtest engine
factor library
data pipeline
portfolio optimizer
risk model
execution simulator
trading agent
research notebook framework
```

我们现在最大的现实问题之一是：

```text
想做 quant research / develop，但真实数据源和工程系统门槛高。
```

这时开源项目就是训练场。FastCode 可以帮助我们快速回答：

```text
这个项目的数据源在哪里？
有没有 survivorship bias 控制？
有没有 transaction cost / slippage model？
有没有 walk-forward validation？
factor 是在哪里计算的？
backtest result 是在哪里汇总的？
portfolio construction 是否独立？
配置和代码是否一致？
```

这会直接提高我们读量化系统的速度。

如果未来做 `Pengyi Quant Research OS v0`，FastCode 可以作为 code-understanding layer：

```text
open-source quant repo
  -> FastCode indexing
  -> architecture map
  -> factor/backtest/data flow extraction
  -> bias checklist
  -> PR opportunities
  -> study note / website post
```

这就是把“读项目”变成可复用流程。

## 和 PR 贡献路径的关系

我们最近一直在讨论：

```text
使用项目
发现问题
提 issue
做 PR
commit
approve
become contributor
```

FastCode 很适合支撑这个路径。

最小流程可以是：

```text
1. clone target repo
2. FastCode index
3. ask architecture questions
4. ask docs/code consistency questions
5. inspect candidate files
6. write small issue or PR
7. update personal website study note
```

这比“为了提 PR 而提 PR”更自然。我们可以真的从使用和阅读中发现 improvement possibility。

## 可吸收成我们的模块

我会把 FastCode 吸收成一个概念模块：

```text
Pengyi Repo Study Accelerator
```

它的最小版本可以不需要完整复刻 FastCode。第一版可以先做：

```text
repo metadata snapshot
file tree summary
entrypoint detection
core module map
README / code consistency checklist
PR opportunity checklist
markdown study note generator
website post generator
```

等这个流程稳定后，再加：

```text
AST parser
function/class index
call graph
embedding search
multi-repo selection
MCP integration
```

这比较现实，也符合我们的当前阶段：先把学习输出变成稳定 artifact，再逐步系统化。

## PR Opportunities

这次读下来，我看到几个可以考虑的 issue / PR 方向。

### 1. 根目录 `LICENSE` 缺失

README 顶部有：

```text
[![License](...)](LICENSE)
```

README 末尾也写：

```text
FastCode is released under the MIT License. See LICENSE for details.
```

但本地根目录没有 `LICENSE` 文件，只有 `nanobot/LICENSE`。

这是一个非常明确的小 PR：

```text
新增根目录 MIT LICENSE
或者修正文档链接，明确 license 文件位置
```

如果项目方确认就是 MIT，这个 PR 边界很小。

### 2. README 的 MCP tools 列表需要更新

实际 `mcp_server.py` 有 11 个 `@mcp.tool()`，比 README 列出的更多。

可以补充：

```text
search_symbol
get_repo_structure
get_file_summary
get_call_chain
reindex_repo
```

并加几个用法例子：

```text
Find where function X is defined
Show call chain for method Y
Summarize file fastcode/main.py
Force reindex after local changes
```

这个 PR 很适合我们作为第一次贡献。

### 3. `graph_weight` 配置和标准 retrieval 路径不完全一致

`config/config.yaml` 里有：

```text
graph_weight: 1
```

但 `retriever.py` 标准路径里的 graph expansion 代码块当前是注释状态。后面 agency mode 和相关逻辑仍可能用图，但对普通配置读者来说，`graph_weight` 的含义会有点不清楚。

可以做一个 docs PR：

```text
说明 graph_weight 当前在哪些路径生效
说明 standard retrieval 是否启用 graph expansion
或者恢复/重构 graph expansion block
```

这属于文档/配置一致性问题。

### 4. Incremental manifest 可以加入 content hash

当前 incremental indexing 主要靠：

```text
mtime + size
```

但 `utils.py` 已有：

```text
compute_file_hash(file_path)
```

可以做：

```text
manifest 增加 hash
mtime/size 先快速判断
hash 用于最终确认
```

这会让增量索引更稳。

### 5. 增加最小 smoke tests

当前 repo 没看到独立 tests 目录。考虑到 parser / indexer / retriever 是核心，最小测试可以从这些开始：

```text
parse tiny Python file
index tiny repo
build graph for simple function call
MCP tool list sanity
config path resolution
```

这类测试不需要真实 LLM API，也能提高维护稳定性。

### 6. 依赖和运行文档可以拆得更清楚

本地语法检查能过，但 import smoke 因为没有安装 `rank_bm25` 失败。这是正常的，因为当前机器没装 requirements。

长期可以把依赖拆成：

```text
core
web
mcp
nanobot
dev
```

这样用户不一定一开始就装全部依赖。

## 系统位置

放到当前 HKUDS 主线里，FastCode 的位置很清楚：

| ID | Repo | System Position |
|---|---|---|
| `HKUDS020` | `FutureShow` | forecast benchmark / judgment ledger |
| `HKUDS021` | `VideoRAG` | long-context video knowledge ingestion |
| `HKUDS022` | `FastCode` | repo-level code intelligence acceleration |

三者连起来就是：

```text
VideoRAG
  -> absorb high-value video knowledge

FutureShow
  -> turn knowledge into forecast and judgment ledger

FastCode
  -> read codebases faster and convert research into engineering output
```

这正好对应我们的三种输入/输出：

```text
video learning
research judgment
code production
```

## 一句话总结

`FastCode` 的价值不是“让模型回答代码问题”这么简单。更准确地说：

```text
FastCode 把一个代码仓库变成 structured, searchable, graph-aware, agent-navigable 的 code knowledge object。
```

对我们的 `Pengyi Research OS` 来说，它意味着：

```text
读 repo 不再只是人工翻文件，
而是可以变成可索引、可追问、可复用、可产出 PR 的工程流程。
```

所以这篇的核心启发是：

```text
build the research engine,
but also build the repo understanding engine.
```

## Next

下一篇进入：

```text
HKUDS023 -> OpenSpace
```

如果说：

```text
VideoRAG gives us video memory.
FutureShow gives us forecasting.
FastCode gives us coding speed.
```

那么下一步就应该看：

```text
OpenSpace / Agent Workspace gives us a self-evolving work environment.
```

也就是把知识、代码、工具、agent 和 workspace 进一步组织起来。
