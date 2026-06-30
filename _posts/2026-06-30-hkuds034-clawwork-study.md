---
title: "HKUDS034: ClawWork 作为 AI Coworker 与 Economic Accountability Layer"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds034, hkuds, clawwork, agent-product, ai-coworker, economic-benchmark, gdpval, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS034`。

```text
HKUDS034 -> ClawWork
```

上一篇 `HKUDS033` 讲的是：

```text
ClawTeam = AI organization layer
```

这一篇接着看：

```text
ClawWork = AI coworker economic accountability layer
```

一句话定位：

```text
ClawWork = GDPVal professional task benchmark
         + token / API cost accounting
         + work artifact evaluation
         + quality-based payment
         + balance / survival dashboard
         + Nanobot / OpenClaw live integration
```

它最重要的启发不是“又一个 agent demo”。

它真正重要的地方是：

```text
把 AI agent 从 assistant 变成 coworker。

assistant 只需要回答问题。
coworker 必须完成任务、交付文件、接受评价、获得收入、承担成本、维持现金流。
```

这和我们最近一直在想的 `contract / credit / cashflow / position` 是同一个方向。
一个真正能进入生产关系的 AI 系统，不能只展示“我会思考”。
它必须证明：

```text
我能在真实任务里创造价值。
我能控制成本。
我能被评价。
我能产生收入。
我能持续生存。
```

这就是 ClawWork 对 Pengyi Research OS / Quant Research OS 的核心启发：

```text
R&D Agent 不应该只自动提出 hypothesis、写代码、跑 backtest。
它还应该有预算、质量分、成本、收益、ROI、复盘和下一轮资源分配。
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `ClawWork`。

| Item | Value |
|---|---|
| repo | `ClawWork` |
| remote | `https://github.com/HKUDS/ClawWork.git` |
| branch | `main` |
| local head | `9c73ac0` |
| full commit | `9c73ac05fdb0bffdb23febdd971eb70f44dd46eb` |
| latest local commit date | `2026-03-03 23:32:37 +0800` |
| latest local commit | `Merge pull request #28 from DorianZheng/codex/boxlite-sync-explicit` |
| Python requirement | `>=3.10` |
| tracked files by `rg --files` | 7,852 |
| Python files | 59 |
| Markdown files | 31 |
| TS/JS/JSX files | 22 |
| JSON files | 83 |
| `livebench/data` local files | 7,681 |
| `livebench/data` local size | about 719 MB |

项目主结构：

```text
ClawWork/
  README.md
  requirements.txt
  setup.py
  start_dashboard.sh
  run_test_agent.sh

  livebench/
    agent/
      live_agent.py
      economic_tracker.py
      wrapup_workflow.py
    work/
      task_manager.py
      evaluator.py
      llm_evaluator.py
    tools/
      direct_tools.py
      productivity/
        search.py
        file_creation.py
        file_reading.py
        code_execution_sandbox.py
        video_creation.py
    api/
      server.py
    prompts/
      live_agent_prompt.py
    configs/
    data/

  clawmode_integration/
    agent_loop.py
    task_classifier.py
    provider_wrapper.py
    tools.py
    artifact_tools.py
    cli.py
    skill/SKILL.md

  eval/
    meta_prompts/
    generate_meta_prompts.py
    test_single_category.py

  scripts/
    task_value_estimates/
      task_values.jsonl
      occupation_to_wage_mapping.json
      hourly_wage.csv

  frontend/
    React / Vite dashboard
```

依赖方向：

```text
Python backend:
  fastapi, uvicorn, websockets
  langchain, langgraph, langchain-openai
  pandas, pyarrow
  openai, python-dotenv
  boxlite, e2b-code-interpreter
  tavily-python
  python-docx, python-pptx, reportlab, openpyxl, xlsxwriter, pdf2image, Pillow

Frontend:
  React 18
  Vite
  Tailwind
  Recharts
  React Router
  Framer Motion
  document / Excel preview libs
```

## 项目用途

ClawWork 的目标是测试：

```text
AI agent 能不能像一个真实 coworker 一样完成专业任务，并在经济约束下持续生存。
```

它用了 `GDPVal` 任务集：

```text
220 professional tasks
44 occupations / sectors
```

这些任务不是“选择题 benchmark”。
它们更像真实工作：

```text
做 Excel
写报告
分析财务材料
处理参考文件
生成交付物
整理专业判断
产出可评价 artifact
```

ClawWork 给 agent 一个初始余额。
README 里的公开榜单使用的是 `$10` starter balance。
默认配置里也有 `initial_balance: 1000.0` 的常规配置。

agent 每次调用 LLM、搜索 API、沙箱工具，都会扣钱。
完成任务后，它提交 artifact。
系统用 LLM evaluator 和职业类别 rubric 打分。
分数乘以任务上限价格，得到理论 payment。
如果质量分低于阈值，`EconomicTracker` 会直接不给实际收入。

核心公式可以理解为：

```text
task_payment = quality_score * max_task_value
net_value = actual_payment - token_cost - api_cost - tool_cost
survival = balance > 0
```

这就把 agent benchmark 从“答得对不对”推进到：

```text
能不能赚钱。
能不能少花钱。
能不能交出专业产物。
能不能长期活下去。
```

## 为什么这件事重要

大多数 agent 项目验证的是：

```text
agent can use tools
agent can call APIs
agent can solve toy tasks
agent can produce text
```

ClawWork 更像在问：

```text
这个 agent 是否可以进入真实组织的劳动分工？
它能不能承担一个任务？
它交付的东西能不能被评价？
它创造的价值是否覆盖成本？
它在资源有限时会不会做策略选择？
```

这正好对应我们做 AI scientist / quant R&D agent 的关键问题。

如果未来我们做：

```text
factor ideation
factor implementation
backtest
bias diagnosis
report generation
next research plan
```

只做到自动化还不够。
真正要进入研究生产系统，需要再加一层：

```text
这轮研究花了多少 token？
花了多少数据查询成本？
用了多少计算？
产出的 factor 是否有质量分？
backtest 是否过审？
人类 PM 是否批准进入下一轮？
这条 research line 是否值得继续投入？
```

ClawWork 给了一个很直接的工程答案：

```text
把每个 agent action 都放进经济账本。
```

## 核心工作流

ClawWork 的 standalone simulation 可以抽象成：

```text
1. TaskManager 选择当天 GDPVal professional task
2. EconomicTracker 开始记录 task-level cost
3. LiveAgent 收到任务、余额、成本、工具和 survival status
4. Agent 决定 work 还是 learn
5. Agent 使用工具读取文件、搜索、执行代码、创建 artifact
6. Agent 调用 submit_work
7. WorkEvaluator 使用 LLM rubric 评分
8. EconomicTracker 根据质量分发放实际 payment
9. 写入 balance / token_cost / task_completion / evaluation logs
10. FastAPI + React dashboard 展示结果
```

更具体一点：

```text
GDPVal task
  -> TaskManager.select_daily_task()
  -> LiveAgent.run_daily_session()
  -> direct_tools.set_global_state()
  -> agent model.bind_tools()
  -> decide_activity()
  -> productivity tools
  -> submit_work()
  -> WorkEvaluator.evaluate_artifact()
  -> LLMEvaluator.evaluate_artifact()
  -> EconomicTracker.add_work_income()
  -> balance.jsonl / token_costs.jsonl / task_completions.jsonl
  -> dashboard
```

这是一条完整的“生产闭环”。

## TaskManager: 真实任务和定价层

核心文件：

```text
livebench/work/task_manager.py
```

`TaskManager` 负责：

```text
load tasks
filter tasks
assign daily tasks
resolve reference files
load task values
track used tasks
log assignments
```

它支持三种 task source：

```text
parquet
jsonl
inline
```

这点很重要。
因为它不只绑定 GDPVal，也可以换成我们自己的 Research OS 任务池。

比如未来我们可以有：

```text
research_tasks.jsonl
factor_tasks.jsonl
paper_reading_tasks.jsonl
backtest_tasks.jsonl
application_tasks.jsonl
```

每个任务可以有统一 schema：

```json
{
  "task_id": "factor_0001",
  "sector": "Quant Research",
  "occupation": "Factor Researcher",
  "prompt": "提出并实现一个基于成交量异常的 alpha 因子，完成样本内/样本外回测。",
  "reference_files": [],
  "max_payment": 120.0
}
```

ClawWork 还有 task value 文件：

```text
scripts/task_value_estimates/task_values.jsonl
```

这里每行都有：

```text
task_id
occupation
hours_estimate
hourly_wage
task_value_usd
sector
task_summary
```

它相当于把每个 professional task 转换成一个“可支付合同”。
这对我们非常关键。
因为 Research OS 也需要把任务定价：

```text
读一篇 paper 值多少钱？
复现一个 repo 值多少钱？
写一个 backtest 值多少钱？
修一个 data pipeline 值多少钱？
做一份导师沟通材料值多少钱？
```

一旦任务有价格，就可以进行资源分配。

## LiveAgent: 每日工作循环

核心文件：

```text
livebench/agent/live_agent.py
```

`LiveAgent.run_daily_session(date)` 是 standalone simulation 的主循环。

它做的事很具体：

```text
setup logging
check bankruptcy
select daily task
start cost tracking
prepare reference files into sandbox
set global tool state
build economic-aware system prompt
bind tools to model
run multi-iteration reasoning loop
execute tool calls
detect submit_work / learn completion
wrap up artifacts if iteration limit reached
save daily economic state
record task completion stats
```

agent 的系统 prompt 会包含：

```text
current balance
net worth
total token cost
session cost
daily cost
survival status
today's work task
max payment
reference files
iteration budget
tool instructions
```

这等于把 agent 放进一个“有财务约束的工作环境”。

普通 agent prompt 常常只说：

```text
你是一个有用的助手。
```

ClawWork 的 prompt 实际上在说：

```text
你是一个要活下去的 worker。
每次说话都花钱。
你要交付文件。
你要按时 submit。
你要考虑 ROI。
```

这个差异非常大。

## Direct Tools: 经济动作和生产力动作

核心文件：

```text
livebench/tools/direct_tools.py
livebench/tools/productivity/
```

ClawWork 的工具可以分两层。

第一层是经济动作：

```text
decide_activity(activity, reasoning)
submit_work(work_output, artifact_file_paths)
learn(topic, knowledge)
get_status()
```

第二层是生产力动作：

```text
search_web()
read_webpage()
create_file()
read_file()
execute_code_sandbox()
create_video()
```

`submit_work` 是最关键的工具。
它会：

```text
1. 校验 work_output 或 artifact_file_paths
2. 如果有文本输出，写入 work artifact 文件
3. 检查 artifact 文件是否存在
4. 调用 evaluator.evaluate_artifact()
5. 把 evaluation_score 和 payment 交给 EconomicTracker
6. 返回 accepted / payment / actual_payment / feedback / artifact_paths
```

这意味着 agent 不能只“说我完成了”。
它必须把产物交出来。

`learn` 也很有意思。
它允许 agent 不工作，而是把知识写进 memory：

```text
memory/memory.jsonl
```

这形成一个真实 trade-off：

```text
work -> 当前收入
learn -> 未来能力
```

这个设计可以直接迁移到我们的 Quant Research OS：

```text
run_backtest -> 当前策略验证收益
read_paper -> 未来研究能力
build_data_pipeline -> 未来生产力
write_report -> 当前对外展示价值
contact_person -> 现实机会流
```

## EconomicTracker: 现金流和生存账本

核心文件：

```text
livebench/agent/economic_tracker.py
```

`EconomicTracker` 是 ClawWork 的灵魂之一。
它管理：

```text
current_balance
total_token_cost
total_work_income
session_cost
daily_cost
task-level cost breakdown
LLM token costs
API call costs
work income
payment threshold
survival status
```

它的状态写入：

```text
balance.jsonl
token_costs.jsonl
task_completions.jsonl
```

成本通道包括：

```text
llm_tokens
search_api
ocr_api
other_api
```

生存状态很朴素：

```text
balance <= 0     -> bankrupt
balance < 100    -> struggling
balance < 500    -> stable
otherwise        -> thriving
```

收入逻辑也非常清晰：

```text
if evaluation_score < min_evaluation_threshold:
    actual_payment = 0
else:
    actual_payment = amount
```

默认阈值：

```text
min_evaluation_threshold = 0.6
```

这代表一个强约束：

```text
质量不过线，不给钱。
```

对我们来说，这就是 Research OS 里 PM 审核的原型。

未来我们的研究任务也可以设置：

```text
factor hypothesis quality < 0.6 -> 不进入实现
backtest reproducibility < 0.7 -> 不进入报告
bias diagnosis quality < 0.8 -> 不进入策略池
human PM approval = false -> 不进入下一轮资源投入
```

## WorkEvaluator: 质量评估和付款层

核心文件：

```text
livebench/work/evaluator.py
livebench/work/llm_evaluator.py
eval/meta_prompts/
```

`WorkEvaluator` 只做 LLM evaluation。
它明确去掉了 heuristic fallback。
如果没有 LLM evaluator，评估会失败，而不是偷偷用简化规则糊弄过去。

这点工程上很重要：

```text
evaluation quality 本身就是系统可信度的一部分。
```

`LLMEvaluator` 会：

```text
load occupation-specific meta prompt
read artifact content
build multimodal evaluation request
call evaluation model
extract 0-10 score
normalize to 0.0-1.0
payment = score * max_payment
```

`eval/meta_prompts/` 里有 44 个职业类别的评估 prompt。
这等于给每个职业建立了 domain-specific rubric。

映射到 Quant Research OS，我们也需要：

```text
factor_ideation_rubric.json
factor_implementation_rubric.json
backtest_rubric.json
bias_diagnosis_rubric.json
report_rubric.json
paper_review_rubric.json
open_source_pr_rubric.json
application_material_rubric.json
```

这样 agent 的输出才不是“看起来不错”，而是有结构化评分标准。

## Productivity Tools: 真实产物生成

核心目录：

```text
livebench/tools/productivity/
```

主要工具：

```text
file_creation.py
file_reading.py
code_execution_sandbox.py
search.py
video_creation.py
```

`execute_code_sandbox` 支持：

```text
E2B
BoxLite
```

`read_file` 支持：

```text
txt
docx
xlsx
pdf
image
pptx
```

`create_file` 支持创建：

```text
txt
md
csv
json
xlsx
docx
pdf
```

这就是 agent 从 text assistant 进入 office worker 的关键。

很多真实任务并不以聊天消息结束。
它们以这些东西结束：

```text
Excel workbook
PDF report
Word document
Python script
analysis notebook
chart
dashboard
presentation
```

ClawWork 在这点上很实用。
它没有把 agent 输出停留在聊天框。
它要求 agent 生成文件，然后提交文件，然后接受评价。

## Dashboard: 生产过程可视化

核心文件：

```text
livebench/api/server.py
frontend/src/
```

后端是 FastAPI。
前端是 React / Vite。

API 层提供：

```text
/api/agents
/api/agents/{signature}
/api/agents/{signature}/tasks
/api/agents/{signature}/learning
/api/agents/{signature}/economic
/api/leaderboard
/api/artifacts/random
/api/artifacts/file
/ws
```

看板展示：

```text
agent balance
income
cost
task completions
quality score
learning memory
artifact files
leaderboard
survival curve
```

这对我们也很关键。
Research OS 不能只有文件夹。
它需要 dashboard：

```text
本周研究投入多少？
哪些 paper 读完了？
哪些 factor 已实现？
哪些 backtest 通过？
哪些报告完成？
哪些任务 ROI 最高？
哪个 agent 最省钱？
哪个研究方向应该加码？
```

ClawWork 的 dashboard 是一个很好的参考。

## ClawMode: 嵌入 Nanobot / OpenClaw

除了 standalone simulation，ClawWork 还有：

```text
clawmode_integration/
```

这部分的作用是把 ClawWork 变成 Nanobot / OpenClaw 的 live agent mode。

核心组件：

```text
ClawWorkAgentLoop
TaskClassifier
TrackedProvider
ClawWork tools
Artifact tools
CLI gateway
Nanobot skill
```

`ClawWorkAgentLoop` 继承 nanobot 的 `AgentLoop`。
它新增：

```text
1. 注册 ClawWork 经济工具
2. 用 TrackedProvider 包住 LLM provider
3. 每条消息 start_task / end_task
4. 每次回复追加 cost footer
5. 拦截 /clawwork command
```

`/clawwork <instruction>` 的流程：

```text
1. 用户发一个自由文本任务
2. TaskClassifier 判断最适合的 occupation
3. 估计 professional hours
4. 读取 hourly wage
5. 计算 task_value = hours * hourly_wage
6. 创建 synthetic paid task
7. 重写 message，要求 agent 生成 artifact 并 submit_work
8. 走正常经济追踪和评估流程
```

这非常像一个“劳动合同入口”。

用户不只是问 agent：

```text
帮我写一下。
```

而是在发一个 paid task：

```text
这是任务。
这是职业类别。
这是估计工时。
这是上限报酬。
这是质量评价方式。
请交付文件。
```

这就是 agent product 从 chat 进入 work contract 的关键形式。

## ClawWork 和 ClawTeam 的关系

上一篇讲的 ClawTeam 是：

```text
组织层。
```

这一篇 ClawWork 是：

```text
工作经济层。
```

对比一下：

| Dimension | ClawTeam | ClawWork |
|---|---|---|
| 核心问题 | 多个 agent 如何组织协作 | 一个 agent 如何成为可付费 coworker |
| 主要对象 | team, member, task, inbox, board | task, artifact, evaluator, payment, balance |
| 状态核心 | team state / task state / message state | balance / cost / income / survival |
| 运行方式 | CLI / spawn / mailbox / worktree | LiveBench loop / Nanobot ClawMode |
| 产物 | team coordination | professional work artifact |
| 对 Research OS 的启发 | 建立 AI research organization | 建立 research task economic accountability |

两者可以合在一起：

```text
ClawTeam 管组织。
ClawWork 管每个 worker 的成本、产出和价值。
```

未来 Pengyi Research OS 可以是：

```text
ClawTeam-like research organization
  PM Agent
  Paper Agent
  Factor Agent
  Backtest Agent
  Risk Agent
  Report Agent

ClawWork-like economic accountability
  每个任务有预算
  每次工具调用扣成本
  每个 artifact 有评分
  每条 research line 有 ROI
  PM 决定是否继续投入
```

## 对 Pengyi Research OS 的启发

我们可以直接把 ClawWork 的思想迁移到我们的核心系统。

### 1. 研究任务要有合同感

不要只是：

```text
今天让 agent 看看这个因子。
```

而是：

```text
Task ID: factor_2026_001
Role: Quant Researcher
Budget: $30 equivalent token/compute cost
Max Value: $150 research credit
Deliverables:
  - factor formula
  - implementation file
  - backtest report
  - bias diagnosis
  - next plan
Evaluation:
  - novelty
  - correctness
  - reproducibility
  - risk control
  - research usefulness
Human PM:
  - approve / reject / revise
```

这会让研究从“灵感流”变成“可审计任务流”。

### 2. R&D Agent 要有现金流思维

我们之前设计的：

```text
R&D Agent for Quant Research
= 自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ 人类 PM 审核
```

现在可以加一层：

```text
+ 记录每轮研究成本
+ 记录每个 artifact 质量分
+ 计算研究 ROI
+ 给下一轮 research budget
+ 形成 PM dashboard
```

这就是从 automation 到 management system 的升级。

### 3. 不同研究动作要有不同 rubric

因子研究不是一个统一任务。
至少可以拆：

```text
Hypothesis Task
Implementation Task
Backtest Task
Bias Diagnosis Task
Report Task
Review Task
Deployment Readiness Task
```

每一种任务都要有不同评分 rubric。

例如 `Backtest Task`：

```text
data leakage check
transaction cost modeling
sample split
out-of-sample behavior
turnover
capacity
drawdown
statistical robustness
code reproducibility
```

例如 `Paper Review Task`：

```text
problem formulation
method decomposition
reproducibility
connection to our system
implementation feasibility
follow-up experiment plan
```

### 4. Agent 不只是 worker，也可以是被管理的资产

ClawWork 排行榜里比较的是：

```text
balance
income
cost
pay rate
average quality
```

这启发我们未来也可以比较：

```text
which agent is best at paper reading
which agent is best at factor ideation
which agent is best at code implementation
which agent is best at debugging backtests
which agent is most cost efficient
which agent produces most approved reports
```

这会变成一个 research agent portfolio。

## 可直接复用的模块想法

如果我们做 `Pengyi Quant Research OS v0`，可以设计：

```text
research_os/
  tasks/
    task_manager.py
    schemas.py
    research_tasks.jsonl
  economy/
    budget_tracker.py
    cost_tracker.py
    value_tracker.py
  evaluation/
    rubrics/
      factor_ideation.json
      backtest.json
      bias_diagnosis.json
      report.json
    evaluator.py
  agents/
    pm_agent.py
    research_agent.py
    dev_agent.py
    backtest_agent.py
    risk_agent.py
  artifacts/
    factor_code/
    backtest_reports/
    diagnostics/
    public_safe_reports/
  dashboard/
    api.py
    frontend/
```

最小闭环可以先做：

```text
1. 一条因子研究任务
2. 一个 agent 生成 hypothesis
3. 一个 agent 写 factor code
4. 一个 backtest runner 产出结果
5. 一个 evaluator 按 rubric 打分
6. 一个 PM markdown report 审核
7. 一个 JSONL 账本记录 cost / quality / decision
```

这就是我们自己的 ClawWork-for-Quant。

## 可以提 PR / 改进的方向

从学习角度看，ClawWork 已经很完整。
如果未来真的作为 contributor 去做，可以从低风险文档和工程体验入手。

### 1. 增加最小 smoke test

当前系统依赖 LLM、GDPVal、dashboard、sandbox、评价模型。
新用户跑起来会比较重。

可以补一个：

```text
inline fixture task
fake evaluator
no external API mode
single command smoke test
```

目标是验证：

```text
TaskManager -> submit_work -> EconomicTracker -> JSONL logs
```

先不用真的 call model。

### 2. 明确日志 schema

ClawWork 的运行数据很有价值。
可以补一份 docs：

```text
balance.jsonl schema
token_costs.jsonl schema
task_completions.jsonl schema
evaluations.jsonl schema
memory.jsonl schema
```

这会让 dashboard、分析脚本、二次开发更容易。

### 3. 拆分 dependency extras

现在 `requirements.txt` 包含：

```text
backend
dashboard
sandbox
OCR / document parsing
search
evaluation
```

可以考虑：

```text
pip install livebench[core]
pip install livebench[dashboard]
pip install livebench[sandbox-e2b]
pip install livebench[sandbox-boxlite]
pip install livebench[docs]
```

这样安装体验更清晰。

### 4. 配置校验 CLI

可以加：

```bash
python -m clawmode_integration.cli doctor
```

检查：

```text
API key
evaluation model
sandbox provider
task values path
meta prompts dir
frontend dependency
data path writable
```

### 5. Research OS adaptation template

可以写一个 `examples/research_task_source/`：

```text
把非 GDPVal 的任务源接入 ClawWork
```

这对我们、quant research、AI researcher benchmark 都很有用。

## 当前限制和注意点

ClawWork 很强，但也有几个需要清楚的地方。

### 1. LLM-as-judge 仍然是核心假设

它用 LLM evaluator 评估工作质量。
这比 heuristic 强很多，但还是会受：

```text
judge model bias
artifact parsing quality
rubric coverage
prompt sensitivity
```

影响。

未来如果用于高价值任务，需要引入：

```text
human PM review
multi-judge agreement
artifact-specific deterministic checks
unit tests
reproducibility checks
```

### 2. GDPVal 是好训练场，但不是所有工作

GDPVal 很适合测试 office / professional tasks。
但 quant research 还需要：

```text
market data
feature pipeline
strategy simulator
risk constraints
transaction costs
compliance and data licensing constraints
```

所以我们不能直接照搬任务集。
要借鉴它的“任务-产物-评价-付款-生存”机制。

### 3. 经济指标需要避免被刷

如果只有 `payment` 和 `cost`，agent 可能优化表面指标。
Research OS 需要加入：

```text
long-term usefulness
reproducibility
PM approval
out-of-sample validation
negative result documentation
anti-leakage checks
```

也就是说，真正的研究价值不能只靠短期收入模拟。

## 对我们当前路线的结论

ClawWork 是 HKUDS Agent Product / Workspace 系列里非常关键的一环。

它把 agent product 从：

```text
tool-using chatbot
```

推进到：

```text
economically accountable worker
```

这和 ClawTeam 正好互补：

```text
ClawTeam 解决组织。
ClawWork 解决产出、评价、成本、收入和生存。
```

对 Pengyi Research OS / Quant Research OS 来说，最应该吸收的是：

```text
每个研究任务都要有明确产物。
每个产物都要有评价标准。
每轮研究都要记录成本。
每条研究线都要有继续投入/停止投入的 PM 决策。
agent 不只是自动执行工具，而是进入一个有预算、有合同、有信用、有现金流的研究组织。
```

如果说 `HKUDS033 ClawTeam` 给了我们 AI research organization 的骨架，
那么 `HKUDS034 ClawWork` 给了我们 AI research worker 的经济账本。

下一步继续看 `HKUDS035 FastAgent`，我们会从 worker / organization 继续走向更轻量、更快速的 agent execution / productization 形态。
