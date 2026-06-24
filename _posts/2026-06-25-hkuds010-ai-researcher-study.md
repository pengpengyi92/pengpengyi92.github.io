---
title: "HKUDS010: AI-Researcher 作为 Autonomous Scientific Discovery 与 Research Agent Benchmark Layer"
date: 2026-06-25 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds010, hkuds, ai-researcher, autonomous-scientific-discovery, research-agent, inno-bench, ai-scientist, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第十一篇。

```text
HKUDS010 -> AI-Researcher
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
HKUDS009 -> DeepCode
```

现在来看 `AI-Researcher`。我对它的定位是：

```text
AI-Researcher = Autonomous Scientific Discovery Pipeline + Research Agent Benchmark Layer
```

如果说前面几个项目分别解决：

```text
RAG-Anything = multimodal document ingestion
LightRAG     = research memory
Vibe-Trading = quant research workflow
nanobot      = personal agent shell
CLI-Anything = software action layer
AI-Trader    = live trading platform layer
AgentSpace   = organizational agent workspace
AutoAgent    = self-developing agent factory
DeepCode     = research-to-code implementation layer
```

那么：

```text
AI-Researcher = 从论文和任务出发，自动完成 idea、survey、plan、implementation、experiment、refinement、paper-writing 的 AI scientist 系统
```

这正好打到我们这条主线的核心：

```text
AI scientist
Research OS
顶会产出
agentic research loop
paper -> idea -> experiment -> report -> next plan
```

DeepCode 解决的是 research-to-code。AI-Researcher 进一步往上走，解决的是 research-to-discovery。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `AI-Researcher`。

| Item | Value |
|---|---|
| repo | `AI-Researcher` |
| upstream | `https://github.com/HKUDS/AI-Researcher` |
| upstream branch | `main` |
| source commit | `f9a6f84` |
| source commit date | `2025-10-16` |
| source commit message | `Add files via upload` |
| local format | Windows-compatible source snapshot |
| package name | `ai-researcher` |
| package version | `0.2.0` |
| Python requirement | `>=3.11` |
| description | Fully-Automated scientific discovery agent framework |

一个重要细节：

```text
当前本地目录不是完整 git checkout，而是 Windows-compatible snapshot。
```

原因是 upstream 里的 `paper_agent` 包含 Windows 不能创建的带冒号文件名，所以本地导出时排除了 `paper_agent`：

| Included | Excluded |
|---|---|
| root files, assets, benchmark, benchmark_collection, docker, examples, research_agent | paper_agent |

本地导出信息：

```text
files_written = 896
bytes_written = 943886635
excluded_reason = paper_agent contains filenames with ':' that cannot be created on Windows
```

所以这篇笔记会重点分析本地可核验的：

```text
research_agent
benchmark
benchmark_collection
examples
web_ai_researcher.py
main_ai_researcher.py
```

README 中关于 `Paper Writing Agent` 的描述会作为项目文档能力记录，但不把它当成本地已读源码结论。

## 一句话理解

AI-Researcher 最核心的价值是：

```text
把科学研究流程 agent 化，并且用 benchmark instance 把“研究能力”变成可以评估的任务。
```

它不是只做一个 “paper reader”，也不是只做 “code generator”。它覆盖的是：

```text
Literature Review
Idea Generation
Algorithm Design
Implementation
Validation
Refinement
Result Analysis
Manuscript Creation
Benchmark Evaluation
```

这就是它和 DeepCode 的区别：

```text
DeepCode     = 论文 / 需求 -> 代码实现
AI-Researcher = 研究任务 / 参考论文 -> idea -> 代码 -> 实验 -> 分析 -> paper
```

对我们来说，AI-Researcher 是目前 HKUDS 里最贴近 `AI Scientist` 总目标的项目之一。

## 两种输入模式

README 里把输入分成两层。

第一层是 `Detailed Idea Description`：

```text
用户已经有一个具体研究 idea
系统根据这个 idea 和参考论文，做 survey、plan、implementation、experiment
```

这对应：

```text
Level 1 = human gives idea, agent conducts research
```

第二层是 `Reference-Based Ideation`：

```text
用户只给参考论文
系统自己生成创新 idea，然后实现和验证
```

这对应：

```text
Level 2 = human gives references, agent generates idea and conducts research
```

这两个层级非常关键。

因为真实 AI scientist 系统应该支持两种工作方式：

```text
1. 人类 PM / PI 已经有方向，agent 负责执行和扩展
2. 人类只给领域材料，agent 自己提出 hypothesis
```

映射到我们的 Quant R&D Agent：

```text
Level 1:
  我们给一个明确因子假设
  agent 做实现、回测、诊断、报告

Level 2:
  我们给 WorldQuant 风格因子库、论文、研报、market observation
  agent 自己生成候选因子假设
  再做实现和实验
```

这就是我们要的自动科研循环。

## Project 用途

AI-Researcher 的用途可以分成四层。

第一层是研究自动化：

```text
reference papers
  -> literature review
  -> research gap finding
  -> idea generation
```

第二层是算法工程：

```text
idea
  -> implementation plan
  -> reference codebase selection
  -> ML project implementation
```

第三层是实验验证：

```text
implementation
  -> actual dataset
  -> two-epoch train/test smoke run
  -> judge review
  -> refinement
  -> extended experiments
```

第四层是科研产物：

```text
result analysis
  -> paper writing
  -> benchmark evaluation
  -> research report
```

它最强的地方不是“每一步都完美”，而是它把完整科研流程拆成了可以自动化、可以缓存、可以评估的 agent workflow。

## 核心目录结构

本地目录：

```text
AI-Researcher/
  assets/
  benchmark/
  benchmark_collection/
  docker/
  examples/
  research_agent/
  .env.template
  Communication.md
  global_state.py
  main_ai_researcher.py
  pyproject.toml
  README.md
  README_WINDOWS_CHECKOUT.md
  setup.cfg
  web_ai_researcher.py
  _WINDOWS_EXPORT_INFO.json
```

核心源码集中在：

```text
research_agent/
  constant.py
  run_infer_idea.py
  run_infer_plan.py
  run_infer_level_1.sh
  run_infer_level_2.sh
  inno/
```

`inno/` 里是系统核心：

```text
inno/
  agents/
  environment/
  memory/
  repl/
  tools/
  workflow/
  core.py
  main.py
  registry.py
  types.py
```

这说明 AI-Researcher 是一个完整 agent runtime，而不是单文件脚本。

## Entry Points

本地有三个重要入口。

第一个是 Gradio Web GUI：

```bash
python web_ai_researcher.py
```

`web_ai_researcher.py` 负责：

```text
Gradio interface
environment variable configuration
log file setup
log streaming
mode selection
call main_ai_researcher()
```

第二个是 Python wrapper：

```text
main_ai_researcher.py
```

它根据 mode 分发：

```text
Detailed Idea Description -> run_infer_plan.main()
Reference-Based Ideation  -> run_infer_idea.main()
Paper Generation Agent    -> paper_agent.writing
```

第三个是 shell scripts：

```text
research_agent/run_infer_level_1.sh
research_agent/run_infer_level_2.sh
```

它们分别对应：

```text
Level 1: 给定 idea
Level 2: 只给 reference papers
```

这套入口设计说明它面向真实使用：

```text
Web GUI for easier use
scripts for repeatable experiments
Python functions for integration
```

## Config Layer

`.env.template` 定义了三类配置。

第一类是 container 配置：

```text
DOCKER_WORKPLACE_NAME
BASE_IMAGES
GPUS
CONTAINER_NAME
WORKPLACE_NAME
CACHE_PATH
PORT
PLATFORM
```

第二类是 LLM 配置：

```text
COMPLETION_MODEL
CHEEP_MODEL
GITHUB_AI_TOKEN
OPENROUTER_API_KEY
OPENROUTER_API_BASE
```

第三类是 task 配置：

```text
CATEGORY
INSTANCE_ID
TASK_LEVEL
MAX_ITER_TIMES
```

这里的设计很清楚：

```text
agent runtime = model + docker environment + benchmark instance
```

这对我们未来很有启发。Quant Research OS 也应该把配置拆成：

```text
model config
data config
universe config
backtest config
risk config
task config
output config
```

否则科研流程无法复现。

## Benchmark Layer

AI-Researcher 的一个核心贡献是 benchmark。

本地 benchmark 目录：

```text
benchmark/
  final/
  process/
```

`benchmark/final` 有五类任务：

```text
diffu_flow
gnn
reasoning
recommendation
vq
```

本地共有：

```text
27 final JSON benchmark instances
```

每个 instance 不是只有题目，而是包含：

```text
target paper
authors
year
url
abstract
venue
citations
source_papers
task1
task2
```

比如 `vq/one_layer_vq.json` 里包含：

```text
target = Addressing Representation Collapse in Vector Quantized Models with One Linear Layer
instance_id = one_layer_vq
source_papers = VQ-VAE, VQGAN, STE, CLIP, FSQ, VAE, Gumbel-Softmax ...
task1 = detailed implementation-oriented task
task2 = higher-level research challenge description
```

这很重要。

AI-Researcher 不是让 agent 自己在空气里研究，而是给它：

```text
target paper
reference papers
task instructions
datasets
baselines
metrics
evaluation setup
```

这才是严肃研究自动化的基础。

## Benchmark Collection

`benchmark_collection/` 是 benchmark 构建流程。

README 说它分三步：

```text
1. 在 papers_to_search 里放 paper titles 或 keywords
2. 运行 0_crawl_paper.py 收集 papers 和 metadata
3. 运行 1_create_inno_graph.py 得到 innovation dataset
```

输出包括：

```text
innovation_graph/innovation_graph_final.json
merged_papers_with_fields.json
```

这说明 AI-Researcher 不只是给了 benchmark 结果，也给了 benchmark construction pipeline。

对我们很有启发。

如果未来要做 Quant Research Agent benchmark，也需要类似：

```text
factor tasks
reference papers
market datasets
baseline formulas
evaluation metrics
expert-written reports
failure labels
```

否则 agent 的 quant research 能力无法稳定评价。

## Core Runtime: MetaChain

核心 runtime 在：

```text
research_agent/inno/core.py
```

核心类：

```text
MetaChain
```

它负责：

```text
agent instructions
tool schema generation
LiteLLM completion
tool call handling
function result handling
agent transfer
context_variables propagation
retry and error handling
streaming / async execution
```

这个 runtime 的思想和 AutoAgent 里的 MetaChain 很接近：

```text
Agent = instructions + model + functions + tool_choice
MetaChain = run agent, execute tools, pass context, transfer agents
```

这很像一个轻量版 multi-agent operating system。

对 Research OS 来说，关键不是“一个 agent 多聪明”，而是：

```text
agent 能不能调用工具
agent 能不能切换角色
agent 能不能带着 context 继续
agent 能不能在失败后 retry
agent 的输出能不能被下游 agent 使用
```

AI-Researcher 正是在这层 runtime 上构建了科研流程。

## Workflow Runtime: FlowModule

工作流抽象在：

```text
research_agent/inno/workflow/flowcache.py
```

三个关键类：

```text
FlowModule
ToolModule
AgentModule
```

`FlowModule` 是整体 workflow base class。

`ToolModule` 包一层 tool call，并把结果缓存到：

```text
cache_path/tools/<tool_name>.json
```

`AgentModule` 包一层 agent call，并把 agent messages/context 缓存到：

```text
cache_path/agents/<agent_name>.json
```

这件事很实用。

科研 agent 任务通常很贵、很慢、很容易中断。没有 cache，很难做长流程。

AI-Researcher 的 cache 设计给了我们一个启发：

```text
每个 research workflow step 都应该可缓存、可复用、可 resume。
```

Quant Research OS 也应该有：

```text
factor_idea_cache
data_profile_cache
implementation_cache
backtest_cache
diagnostic_cache
report_cache
```

否则每次都从头跑，会非常浪费。

## Main Workflow: InnoFlow

Level 1 的核心类在：

```text
research_agent/run_infer_plan.py
class InnoFlow
```

它的 pipeline 是：

```text
load instance
  -> search GitHub repos for source papers
  -> Prepare Agent selects reference codebases
  -> download arXiv source papers
  -> Survey Agent reviews idea and papers
  -> import dataset metaprompt
  -> Coding Plan Agent creates implementation plan
  -> Machine Learning Agent implements project
  -> Judge Agent reviews implementation
  -> optional implementation refinement loop
  -> submit code for statistical results
  -> Experiment Analysis Agent analyzes results
  -> Machine Learning Agent runs further experiments
```

这条链路非常完整。

它不是：

```text
生成 idea -> 写一点 code -> 结束
```

而是：

```text
reference codebase selection
paper source retrieval
survey notes
dataset/baseline/metric planning
self-contained implementation
actual dataset training/testing
judge review
experiment refinement
analysis report
```

这已经非常接近一个真实 junior researcher + engineer 的工作流。

## Level 2 Workflow: Idea Generation

Level 2 的核心类在：

```text
research_agent/run_infer_idea.py
class InnoFlow
```

它比 Level 1 多了 idea generation。

它的主链路是：

```text
load instance
  -> search GitHub repos
  -> Prepare Agent selects reference codebases
  -> download paper sources
  -> Idea Agent generates multiple ideas
  -> Idea Agent selects and refines best idea
  -> Code Survey Agent reviews codebases
  -> Coding Plan Agent creates plan
  -> Machine Learning Agent implements
  -> Judge Agent reviews
  -> refinement loop
  -> experiment analysis loop
```

其中它会生成多个 idea：

```text
IDEA_NUM = 5
```

然后让 idea agent：

```text
select the most novel one
enhance missing information
output refined idea report
```

这正是 autonomous scientific discovery 的关键一跳：

```text
not only execute a given idea
but generate and select an idea from references
```

对我们未来做 factor research：

```text
reference papers / reports / historical factor zoo
  -> generate 5 candidate factor hypotheses
  -> select the most promising one
  -> implement and backtest
  -> analyze failure
  -> generate next round
```

这条线非常直接。

## Agent 1: Prepare Agent

代码：

```text
research_agent/inno/agents/inno_agent/prepare_agent.py
```

Prepare Agent 的任务是：

```text
根据论文和 GitHub 搜索结果，选择最相关、最有用的 reference codebases
```

它的选择标准包括：

```text
stars
recency
README quality
code structure clarity
comments and explanations
Python preference
PyTorch preference
local runnable preference
```

它可以用工具：

```text
execute_command
gen_code_tree_structure
read_file
terminal navigation tools
```

最终输出：

```text
reference_codebases
reference_paths
reference_papers
```

这个 agent 很重要。

科研工程不是凭空写代码，而是要知道：

```text
哪些 repo 可以参考
哪些 repo 可维护
哪些 repo 能跑
哪些 repo 和这个 idea 真相关
```

未来我们做 PR、复现论文、搭 quant system，也需要这个能力。

## Agent 2: Survey Agent / Idea Agent

相关文件：

```text
research_agent/inno/agents/inno_agent/survey_agent.py
research_agent/inno/agents/inno_agent/idea_agent.py
```

Survey Agent 负责：

```text
paper survey
code survey
merge academic definition and code implementation notes
```

Idea Agent 负责：

```text
review papers
find limitations
generate innovative ideas
refine mathematical formula
connect idea to code implementation
```

这两个 agent 的组合很关键。

一个研究 idea 不是一句话。严肃 idea 至少要包含：

```text
problem definition
method motivation
mathematical formulation
algorithmic steps
implementation mapping
experimental setup
expected advantage
limitations
```

AI-Researcher 通过 paper survey + code survey 把 idea 变成可实现的计划。

## Agent 3: Coding Plan Agent

代码：

```text
research_agent/inno/agents/inno_agent/plan_agent.py
```

Coding Plan Agent 要生成四个计划：

```text
Dataset Plan
Model Plan
Training Plan
Testing Plan
```

它调用的 planning tools 包括：

```text
plan_dataset
plan_model
plan_training
plan_testing
```

它要求先 review reference codebases，再输出 plan。

这个设计很对。因为研究实现不是单纯模型文件：

```text
dataset
data loader
preprocessing
model architecture
loss
optimizer
training config
evaluation metric
testing script
logging
```

如果 plan 没把这些讲清楚，后面的 implementation agent 一定会失控。

对应到 Quant Research OS：

```text
Factor Plan
Dataset Plan
Universe Plan
Backtest Plan
Risk Diagnosis Plan
Report Plan
```

必须提前结构化。

## Agent 4: Machine Learning Agent

代码：

```text
research_agent/inno/agents/inno_agent/ml_agent.py
```

Machine Learning Agent 是执行层。

它可以调用：

```text
gen_code_tree_structure
execute_command
read_file
create_file
write_file
list_files
create_directory
run_python
terminal_page_down / up / to
case_resolved
case_not_resolved
```

它的核心约束非常明确：

```text
所有代码必须在 /<working_dir>/project
不能直接 import reference codebases
要把参考代码理解后改写和整合
必须 self-contained
必须使用真实 dataset
必须实现每个 plan component
不能用 toy data
不能用 placeholder
```

这比一般 demo 严格很多。

尤其是这一条：

```text
NO direct imports from reference codebases
```

这很重要。因为如果直接 import reference repo，其实不是自己的实现，也很容易有环境、license、依赖问题。

AI-Researcher 要求：

```text
study reference
understand logic
adapt into self-contained project
document origin and modifications
```

这才像一个真实 researcher / engineer 的工作。

## Agent 5: Judge Agent

代码：

```text
research_agent/inno/agents/inno_agent/judge_agent.py
```

Judge Agent 的任务是：

```text
review implementation
check atomic idea one by one
compare with reference codebases
decide fully_correct
give suggestion
```

它里面还嵌套了：

```text
Code Review Agent
```

Code Review Agent 可以读文件、看代码树，然后把 review report 交回 Judge Agent。

最终 `case_resolved` 输出：

```json
{
  "fully_correct": true_or_false,
  "suggestion": {...}
}
```

主流程会检查：

```text
if '"fully_correct": true' in judge response:
    break
```

这就是自动科研里的 reviewer loop。

对我们来说，这可以直接迁移：

```text
Implementation Agent 写 factor
Judge Agent 检查:
  - 是否有未来函数
  - 是否使用真实数据
  - 是否 universe 对齐
  - 是否处理停牌/缺失
  - 是否计算交易成本
  - 是否违反 signal definition
  - 是否结果可复现
```

## Agent 6: Experiment Analysis Agent

代码：

```text
research_agent/inno/agents/inno_agent/exp_analyser.py
```

Experiment Analysis Agent 在代码提交后工作。

它需要：

```text
analyze experimental results
compare reference codebases and papers
produce analysis_report
produce further_plan
```

主流程中：

```text
EXP_ITER_TIMES = 2
```

也就是它会做两轮实验分析和 refinement。

这很像真正做研究：

```text
first run
  -> result analysis
  -> ablation / sensitivity / visualization plan
  -> more experiments
  -> updated conclusion
```

这也是顶会工作最关键的一部分。

很多人以为研究是“写个模型”，但真正的研究产出在：

```text
why it works
when it fails
how it compares
whether improvement is robust
what ablation proves
```

AI-Researcher 把这部分也放进 agent loop。

## Tools Layer

AI-Researcher 的工具层分散在：

```text
research_agent/inno/tools/
research_agent/inno/tools/inno_tools/
```

关键工具包括：

```text
arxiv paper metadata
arxiv source download
GitHub repo search
GitHub code search
file surfing
terminal commands
run_python
code report
RAG code search
RAG code tree
web tools
planning tools
```

它还有多个环境：

```text
DockerEnv
BrowserEnv
RequestsMarkdownBrowser
```

这说明它的 agent 不只是聊天，而是有真实工具：

```text
search papers
search code
download code
read code
write code
run scripts
inspect output
browse web pages
evaluate implementation
```

这和我们前面反复看到的规律一致：

```text
agent capability = model reasoning + tool/action layer + memory/cache + evaluation loop
```

## Memory Layer

本地有：

```text
research_agent/inno/memory/
  code_memory.py
  codetree_memory.py
  paper_memory.py
  rag_memory.py
  tool_memory.py
```

这说明 AI-Researcher 也在处理长流程中的记忆问题。

科研 agent 需要多种 memory：

```text
paper memory       -> 论文内容和结论
code memory        -> reference implementation
codetree memory    -> 项目结构
tool memory        -> 工具调用结果
rag memory         -> 检索增强
```

对 Pengyi Research OS 来说，memory 也应该分类型：

```text
paper memory
factor memory
experiment memory
data memory
portfolio memory
conversation memory
decision memory
```

不同 memory 不能混在一起，否则后面会变成不可用的聊天记录。

## Docker Environment

AI-Researcher 用 Docker 承载实验环境。

README 里建议：

```bash
docker pull tjbtech1/airesearcher:v1
```

或者：

```bash
cd ./docker && docker build -t tjbtech1/airesearcher:v1 .
```

`research_agent` 的执行也会创建：

```text
workplace_paper/task_<instance>_<model>/<workplace_name>
container_name = paper_eval_<instance>_<model>
```

这说明每个 research task 都应该有自己的隔离 workspace 和 container。

这是严肃自动实验必须有的。

因为 agent 会执行代码、下载数据、跑训练，如果没有隔离：

```text
依赖会污染
文件会覆盖
实验不可复现
安全边界不清楚
```

对我们未来做 quant research，也应该考虑：

```text
per-task workspace
per-run environment
dataset readonly mount
artifact output directory
execution sandbox
```

## Examples

本地 `examples/` 里有多个示例：

```text
con_flowmatching
dccf
fsq
gnn_difformer
gnn_nodeformer
hgcl
rotation_vq
```

这些示例包括：

```text
paper.pdf / paper.png / paper.gif
scrolling_code.gif
project/
training/
testing/
model/
experiments/
run_training_testing.py
run_ablation.py
visualization scripts
logs
```

这说明 AI-Researcher 的目标不是只输出一段文本，而是输出一套研究项目 artifact。

对我们来说，未来每个 factor research 也应该输出：

```text
factor_spec.md
implementation.py
backtest_config.yaml
results.csv
diagnostics.md
plots/
report.md
next_plan.md
```

只有这样才能积累成 portfolio 和 open-source assets。

## 它和 DeepCode 的关系

AI-Researcher 和 DeepCode 很容易混淆，但它们的位置不同。

| Project | Core Position |
|---|---|
| DeepCode | paper / requirement -> code implementation |
| AI-Researcher | references / idea -> full research lifecycle |

DeepCode 更像：

```text
implementation engine
```

AI-Researcher 更像：

```text
research lifecycle orchestrator
```

AI-Researcher 内部也有 implementation agent，但它的上游和下游更长：

```text
idea generation
survey
reference codebase selection
benchmark instance
experiment analysis
paper writing
```

所以在 Pengyi Research OS 里可以这样放：

```text
AI-Researcher = AI scientist master workflow
DeepCode      = research-to-code sub-engine
```

未来我们可以把 DeepCode 当成 AI-Researcher 里的 implementation backend。

## 它和 AutoAgent 的关系

AutoAgent 是：

```text
self-developing agent factory
```

AI-Researcher 是：

```text
research agent application system
```

AutoAgent 解决：

```text
如何自动创建 tools / agents / workflows
```

AI-Researcher 解决：

```text
如何让这些 agents 完成科研任务
```

如果组合起来：

```text
AutoAgent creates specialized research agents
AI-Researcher orchestrates scientific discovery tasks
DeepCode implements code projects
```

这条线就很强。

## 它和 RAG-Anything / LightRAG 的关系

AI-Researcher 需要大量输入：

```text
papers
source code
benchmark specs
datasets
experiment logs
reports
```

RAG-Anything 可以做：

```text
multimodal paper / PDF / table / formula ingestion
```

LightRAG 可以做：

```text
research memory and graph retrieval
```

AI-Researcher 可以做：

```text
research workflow orchestration
```

组合起来：

```text
RAG-Anything -> ingest
LightRAG     -> remember and retrieve
AI-Researcher -> discover and experiment
DeepCode     -> implement
```

这就是 Research OS 的核心链路。

## 它和 Quant R&D Agent 的关系

AI-Researcher 对我们的 Quant R&D Agent 启发非常直接。

可以把它的科研流程迁移成：

```text
reference papers / factor zoo / market report
  -> generate factor idea
  -> select reference code / formula
  -> write factor implementation plan
  -> implement factor
  -> run backtest on actual data
  -> judge bias and robustness
  -> analyze results
  -> create next experiment plan
  -> write research memo
```

对应 agent：

| AI-Researcher | Quant R&D Agent |
|---|---|
| Prepare Agent | Reference Factor / Codebase Selector |
| Idea Agent | Factor Hypothesis Agent |
| Survey Agent | Paper / Factor Zoo Survey Agent |
| Coding Plan Agent | Factor Implementation Plan Agent |
| Machine Learning Agent | Factor Implementation Agent |
| Judge Agent | Bias / Robustness Judge Agent |
| Experiment Analysis Agent | Backtest Diagnostics Agent |
| Writer Agent | Research Memo Agent |

这几乎就是我们之前定义的：

```text
自动提出因子假设
自动实现
自动回测
自动诊断偏差
自动生成下一轮研究计划
人类 PM 审核
```

AI-Researcher 给的是一个可以借鉴的系统骨架。

## 最值得学习的设计

我认为 AI-Researcher 最值得学习的有七点。

第一，任务 benchmark 化。

```text
不是只给 agent 一个开放问题，而是给 structured benchmark instance。
```

第二，reference-based ideation。

```text
只给 reference papers，也能生成 idea。
```

第三，reference codebase selection。

```text
先选高质量 repo，再做实现。
```

第四，implementation 必须 self-contained。

```text
不能直接 import reference repo。
```

第五，必须真实 dataset。

```text
NO toy data.
```

第六，必须有 judge loop。

```text
implementation 后必须 review atomic ideas。
```

第七，必须有 experiment analysis loop。

```text
不只跑结果，还要分析结果，并规划进一步实验。
```

这些都应该写进 Pengyi Research OS 的设计原则。

## 当前实现的不足和风险

AI-Researcher 很有启发，但也要冷静看。

第一，本地 Windows snapshot 不完整。

```text
paper_agent 被排除，所以 paper writing 源码这里无法核验。
```

第二，README 中有一些路径和镜像命名不完全一致。

例如：

```text
.env.template: BASE_IMAGES=tjbtech1/airesearcher:v1
README script: BASE_IMAGES=tjbtech1/paperagent:latest
```

这不一定是 bug，但说明实际运行前需要仔细对齐配置。

第三，自动科研非常消耗资源。

它需要：

```text
LLM API
GitHub search
paper download
Docker
GPU
dataset
training time
```

这不是轻量 demo。

第四，agent 生成的研究 idea 和实验结论不能直接相信。

必须人工审核：

```text
novelty
correctness
baseline fairness
data validity
implementation completeness
statistical significance
writing claims
```

第五，金融场景更高风险。

如果迁移到 quant：

```text
不能把私有数据喂给公开模型
不能发布未经脱敏的策略
不能用含未来函数的结果做结论
不能把 toy backtest 当实盘能力
```

## 可以提 PR / issue 的方向

如果我们后续使用 AI-Researcher，可以从真实使用中找小 PR。

可能方向：

| Direction | Why |
|---|---|
| Windows checkout documentation | 当前 paper_agent 文件名导致 Windows checkout 问题，可以补文档 |
| Config consistency | README / .env.template / scripts 的镜像名和 model config 可以更统一 |
| Minimal Level 1 demo | 给一个可快速跑通的小 benchmark instance |
| Dry-run mode | 不训练，只验证 workflow / config / Docker / env |
| Better missing paper_agent warning | Windows 用户启动 paper writing 时给明确提示 |
| Quant-style benchmark template | 用 synthetic data 做一个金融研究任务模板 |
| Cache docs | 解释 ToolModule / AgentModule cache 如何 resume |
| Safety docs | 说明自动执行代码、下载 repo、使用 API key 的边界 |

最适合我们的第一步还是：

```text
先真实跑一个最小 Level 1 task
记录配置/环境/失败点
再决定提文档 PR 或小 bugfix
```

## 对 Pengyi Research OS 的设计草图

从 AI-Researcher 学到的结构，可以转成我们的版本：

```text
Pengyi Quant Research OS

Input:
  - factor paper
  - WorldQuant-style alpha
  - market report
  - PM instruction
  - historical experiment memory

Benchmark Instance:
  - universe
  - data range
  - rebalance frequency
  - signal definition
  - baseline factor
  - metrics
  - risk checks

Agents:
  - Reference Collector
  - Factor Idea Agent
  - Research Survey Agent
  - Implementation Plan Agent
  - Factor Coding Agent
  - Backtest Agent
  - Bias Judge Agent
  - Experiment Analysis Agent
  - Research Memo Agent
  - Human PM Gate

Outputs:
  - factor implementation
  - backtest result
  - diagnostics
  - plots
  - research memo
  - next-round plan
```

这就是把 AI-Researcher 的科研自动化框架迁移到量化研究。

## AI Scientist 路线意义

AI-Researcher 对我们最重要的意义是：它把 `AI scientist` 从一句口号变成了工程拆解。

一个 AI scientist 系统至少要有：

```text
1. 读文献
2. 找 gap
3. 生成 hypothesis
4. 选 baseline
5. 找 reference code
6. 写实现
7. 跑实验
8. 做 ablation
9. 分析结果
10. 写 paper / report
11. 进入下一轮
```

AI-Researcher 试图把这 11 件事放进一个 agent workflow。

这和我们现在的路线完全一致：

```text
不是只做 dirty work
不是只做流程优化
不是只做简历项目
而是构建一个能持续产出 research assets 的个人 AI scientist system
```

它对我们的启发是：

```text
顶会产出不是“灵感一闪”，而是可以被系统化、工程化、循环化。
```

## Study Map 更新

加入 AI-Researcher 之后，HKUDS 第一阶段地图变成：

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
| HKUDS010 | AI-Researcher | autonomous scientific discovery and research-agent benchmark layer |

`HKUDS010` 的一句话总结：

```text
AI-Researcher 把 AI scientist 从“读论文 + 写代码”推进到“自动科研生命周期”。
```

它对我们的路线非常关键，因为我们最终想做的不是一个单点工具，而是：

```text
能持续提出研究问题
能持续做实验
能持续写报告
能持续积累开源资产
能持续冲顶会
```

的个人 Research OS。

如果 DeepCode 是实现层，那么 AI-Researcher 就是科研总流程层。

这两个项目放在一起，就是：

```text
AI-Researcher decides and orchestrates what to research.
DeepCode helps implement research into code.
```

未来我们的 Pengyi Quant Research OS 可以沿着这条线继续做下去。
