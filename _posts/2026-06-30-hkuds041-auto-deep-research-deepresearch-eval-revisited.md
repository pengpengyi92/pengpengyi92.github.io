---
title: "HKUDS041: Auto-Deep-Research / DeepResearch-Eval Revisited 作为 Deep Research Product Loop 与 AI Scientist Evaluation Layer"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds041, hkuds, auto-deep-research, deepresearch-eval, deep-research, autoagent, evaluation, research-os, quant-os, ai-scientist]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS041`。

```text
HKUDS041 -> Auto-Deep-Research / DeepResearch-Eval Revisited
```

这不是重复 `HKUDS012` 和 `HKUDS013`。

前面已经分别看过：

```text
HKUDS012 Auto-Deep-Research -> open deep research product and AutoAgent application layer
HKUDS013 DeepResearch-Eval  -> report-centric evaluation and factuality checking layer
```

现在把它们放在 Agent Product / Workspace 系列尾部重新看，重点变成：

```text
Deep Research Product Loop

question
-> plan
-> search / read / browse / code
-> evidence
-> synthesis
-> report
-> evaluation
-> gap diagnosis
-> next research plan
```

一句话定位：

```text
Auto-Deep-Research = research report producer
DeepResearch-Eval  = research report evaluator
二者合在一起       = AI scientist 的 research production and evaluation loop
```

这对我们非常关键。

我们一直想做的 `R&D Agent for Quant Research` 是：

```text
自动提出因子假设
自动实现
自动回测
自动诊断偏差
自动生成下一轮研究计划
人类 PM 审核
```

把这个抽象到更广义的 AI scientist，就是：

```text
自动提出研究问题
自动收集证据
自动生成研究报告
自动评估报告质量
自动发现缺口
自动生成下一轮研究计划
人类 PI / PM 审核
```

`Auto-Deep-Research + DeepResearch-Eval` 正好提供了这个闭环的前半段和后半段。

## Local Snapshot

本次阅读的是本地 HKUDS 工作区中的两个仓库。阅读前已执行 `git fetch --all --prune`，两个仓库当前本地 `main` 均与 `origin/main` 对齐，工作区 clean。

### Auto-Deep-Research

| Item | Value |
|---|---|
| repo | `Auto-Deep-Research` |
| remote branch | `main [origin/main]` |
| local head | `9d671a3` |
| latest local commit date | `2025-10-16 14:38:48 +0800` |
| latest local commit | `Update Communication.md` |
| tracked files | 73 |
| package root | `autoagent/` |
| CLI entry | `auto deep-research` |
| core agents | `System Triage Agent`, `Web Surfer Agent`, `File Surfer Agent`, `Coding Agent` |
| core environments | `DockerEnv`, `BrowserEnv`, `RequestsMarkdownBrowser` |
| setup | `pip install -e .` plus Docker |

项目结构可以压成：

```text
Auto-Deep-Research/
  README.md
  pyproject.toml
  setup.cfg
  constant.py
  Communication.md
  .env.template

  autoagent/
    cli.py
    main.py
    core.py
    registry.py
    fn_call_converter.py

    agents/
      system_agent/
        system_triage_agent.py
        websurfer_agent.py
        filesurfer_agent.py
        programming_agent.py

    environment/
      docker_env.py
      browser_env.py
      markdown_browser/
      mdconvert.py

    tools/
      web_tools.py
      terminal_tools.py
      file_surfer_tool.py
      rag_tools.py

    memory/
      rag_memory.py
      paper_memory.py
      code_memory.py
```

它的 README 明确定位为 OpenAI Deep Research 的 open-source、cost-efficient alternative，并且是基于 `AutoAgent` 框架的 ready-to-use product。

启动方式不是写一堆脚本，而是：

```bash
auto deep-research
```

可以通过环境变量切换模型：

```bash
COMPLETION_MODEL=gpt-4o auto deep-research
COMPLETION_MODEL=gemini/gemini-2.0-flash auto deep-research
COMPLETION_MODEL=openrouter/deepseek/deepseek-r1 auto deep-research
```

这说明它想解决的不是单点算法，而是：

```text
让一个普通用户用自己的 LLM API key，启动一个可浏览网页、可读文件、可写代码、可生成研究结果的个人 deep research assistant。
```

### DeepResearch-Eval

| Item | Value |
|---|---|
| repo | `DeepResearch-Eval` |
| remote branch | `main [origin/main]` |
| local head | `d57f8c9` |
| latest local commit date | `2025-10-16 14:46:14 +0800` |
| latest local commit | `Update README_ZH.md` |
| tracked files | 15 |
| main scripts | `judge_score.py`, `judge_fact.py` |
| prompt file | `Aprompts.py` |
| tool file | `Atools.py` |
| data | 100 topics and Qwen-DeepResearch reports in JSONL form |
| external tools | OpenAI-compatible API, Firecrawl, Jina Reader, DashScope |

项目结构可以压成：

```text
DeepResearch-Eval/
  README.md
  README_ZH.md
  judge_score.py
  judge_fact.py
  judge_score.sh
  judge_fact.sh
  Aprompts.py
  Atools.py

  data/
    topic/
      high_quality_topics.jsonl
    report/
      qwen-reports.jsonl

  example/
    judge_score_result/
    judge_fact_result/
```

它不是再生成研究报告，而是把 deep research 的输出当作 object 来评估：

```text
input:  topic + report
output: quality score + redundancy score + factual support labels
```

核心评估维度包括：

```text
Comprehensiveness and Depth
Structural Clarity and Logic
Fluency and Consistency of Expression
Material Integration and Originality
Overall Score
Redundancy / Repeatability
Factual Support
```

事实核查脚本还支持：

```text
url + contexts
-> scrape page by Jina / Firecrawl
-> compare each context against page markdown
-> output is_factual = -1 / 0 / 1
```

这就是 report-centric evaluation。

## 为什么要二刷

第一次看 `Auto-Deep-Research`，重点是：

```text
它怎么启动
它有哪些 agent
它怎么调 browser / file / code environment
它怎么作为 AutoAgent application layer
```

第一次看 `DeepResearch-Eval`，重点是：

```text
它怎么打分
它怎么做冗余检测
它怎么做事实核查
它怎么作为 report evaluation layer
```

现在二刷，问题变了：

```text
如果我们要搭一个真实可用的 Pengyi Research OS，
这两个项目加在一起给了什么产品闭环？
```

答案是：

```text
producer + evaluator
```

更具体：

```text
Auto-Deep-Research 负责生成研究 artifact。
DeepResearch-Eval 负责审计 artifact。
二者之间缺的那一层，是我们要自己补的 task ledger、evidence schema、human PM review 和 next-plan generator。
```

## Auto-Deep-Research 作为 Producer

Auto-Deep-Research 的产品形态非常清楚：

```text
一个用户打开 CLI
输入一个复杂问题
系统自动调度 web / file / code agent
持续搜索、浏览、读文件、写代码、整合信息
最后输出一个答案或研究报告
```

它不是只做 RAG。

RAG 通常是：

```text
query -> retrieve -> answer
```

Deep Research 更像：

```text
question
-> decompose
-> search
-> browse
-> download
-> read
-> code
-> verify
-> synthesize
-> answer
```

这就是为什么它需要三个专门 agent：

| Agent | Role |
|---|---|
| `System Triage Agent` | 决定子任务交给谁，负责 agent routing 和任务完成判断 |
| `Web Surfer Agent` | 打开网页、搜索、点击、读取页面 markdown、下载文件 |
| `File Surfer Agent` | 读取本地文件，支持 PDF、DOCX、XLSX、PPTX、音频等转 markdown |
| `Coding Agent` | 写代码、运行 Python、执行 shell、处理复杂计算和数据转换 |

这四个 agent 对应的是研究工作的四种动作：

```text
plan / route
browse web
read files
compute / code
```

这比单一 chat agent 更接近真实 research。

## CLI 入口

`autoagent/cli.py` 的 `deep_research` command 做了几件事：

```text
1. 读取 container_name 和 port
2. 建立 DockerConfig
3. 初始化 Docker code environment
4. 初始化 BrowserEnv
5. 初始化 RequestsMarkdownBrowser file environment
6. 构造 context_variables
7. 创建 System Triage Agent
8. 进入 PromptSession 循环
9. 用户可以直接输入问题，也可以用 @ 指定 agent
```

这对产品设计很关键。

一个 deep research product 不能只是一段脚本。它需要：

```text
session
workspace
environment
agent roster
message history
artifact output
exit condition
```

Auto-Deep-Research 已经把这些做成一个最小产品。

## Runtime 终止条件

`autoagent/main.py` 里有两个关键工具：

```text
case_resolved(result)
case_not_resolved(failure_reason, take_away_message)
```

这两个工具非常重要。

它们让 agent 不是无限对话，而是进入：

```text
case lifecycle
```

也就是：

```text
一个 task 开始
agent 尝试解决
如果解决，必须用 case_resolved 输出最终结果
如果失败，必须解释 failure_reason 和 take_away_message
```

这对我们的 Research OS 很有启发。

我们未来每一个 research task 都应该有明确状态：

```text
open
in_progress
resolved
blocked
needs_human_review
accepted
rejected
next_round_generated
```

否则 agent 只是聊天，不是生产系统。

## System Triage Agent

`system_triage_agent.py` 的设计很直接：

```text
它自己不做所有事。
它决定把任务转给 File Surfer / Web Surfer / Coding Agent。
```

它拥有三个 transfer tools：

```text
transfer_to_filesurfer_agent(sub_task_description)
transfer_to_websurfer_agent(sub_task_description)
transfer_to_coding_agent(sub_task_description)
```

子 agent 做完后，再通过：

```text
transfer_back_to_triage_agent(task_status)
```

回到 triage。

这就是一个小型 research organization。

```text
PM / Triage Agent
  -> Web Researcher
  -> File Analyst
  -> Coding Research Engineer
  -> PM / Triage Agent
  -> final answer
```

对 Pengyi Research OS 来说，这个模式可以扩展成：

```text
PM Agent
Research Agent
Data Agent
Backtest Agent
Risk Agent
Writing Agent
Reviewer Agent
Outreach Agent
```

## Web Surfer Agent

Web Surfer 的 tool list 是：

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

它的 instructions 里特别强调：

```text
如果页面里有 YouTube、Wikipedia 或其他 media content，
或者需要细读网页文本，
应该用 get_page_markdown 转成 markdown。
```

这说明它不是只看截图，而是把网页变成研究材料。

这对我们自己的系统意味着：

```text
网页不是临时上下文。
网页应该进入 evidence layer。
```

未来 Research OS 的 evidence schema 至少应该有：

```text
source_url
title
access_time
markdown_snapshot
quoted_claims
used_in_section
confidence
```

## File Surfer Agent

File Surfer 的定位是：

```text
local file reader
```

它支持：

```text
html / xlsx / pptx / wav / mp3 / flac / pdf / docx
text files
image VQA via visual_question_answering
```

这对我们很实用。

我们的资料不是只有网页，还有：

```text
CV
PS
RP
PDF paper
project README
quant report
bank material
meeting note
worldquant factor notes
导师邮件附件
```

File Surfer 的启发是：

```text
Research OS 必须把本地文件当作一等公民。
```

而不是每次都把文件手动复制给 agent。

## Coding Agent

Coding Agent 的 instructions 很像一个 constrained software engineer：

```text
只能在指定 workspace 工作
优先写 Python
写文件前先读文件
必须用 run_python 运行 Python
需要生成命令和依赖
输出太长时分页查看 terminal
```

这对 deep research 很关键。

真正的研究问题经常需要：

```text
下载数据
清洗 JSON / CSV
统计数量
画图
验证公式
跑 baseline
检查 repo
读代码结构
```

所以 deep research assistant 不应该只会搜索。

它必须会：

```text
research engineering
```

这也是我们自己的优势方向。

## DeepResearch-Eval 作为 Evaluator

如果只有 Auto-Deep-Research，系统很容易变成：

```text
看起来很完整的长报告生成器
```

但是长报告最大的问题是：

```text
结构可能漂亮，但证据不一定可靠。
信息可能很多，但可能重复。
语言可能顺畅，但不一定有洞察。
引用可能很多，但未必支撑结论。
```

DeepResearch-Eval 的价值就在这里。

它把 report 当作可审计对象：

```text
report is not final truth
report is an artifact to be evaluated
```

这和我们一直说的 human PM review 是同一个方向。

## Quality Score

`judge_score.py` 的主链路是：

```text
JSONL input: topic + report
-> calculate file_id by topic hash
-> split paragraphs
-> extract first-level headings
-> judge quality
-> sample section pairs
-> judge repeatability
-> save one JSON per report
-> checkpoint progress
```

输出字段包括：

```text
file_id
topic
compare_list
repeat_results
comprehensiveness_score
coherence_score
clarity_score
insight_score
overall_score
repeat_score
quality_reason
```

这个结构非常适合接入我们的 Research OS。

每一篇研究报告可以形成：

```text
report.md
report_eval.json
report_evidence.jsonl
human_review.md
next_plan.md
```

这才是可迭代的研究产出。

## Redundancy Detection

DeepResearch-Eval 不只评估“好不好”，还评估重复度。

它会：

```text
按 markdown heading 切 section
过滤过短 section
随机抽取 section pair
用 LLM 判断两段之间的信息重复程度
计算 repeat_score
```

这点非常现实。

Deep research 报告经常有一个问题：

```text
写得很长，但很多段落在反复说同一件事。
```

对 AI scientist 来说，冗余不是小问题。

因为冗余意味着：

```text
信息密度低
作者没有真正压缩理解
报告看似全面，实际洞察不足
```

未来我们的文章、proposal、quant memo 都可以引入这个维度。

## Fact Checking

`judge_fact.py` 的链路是：

```text
input JSONL:
  {url: {contexts: [sentence1, sentence2, ...]}}

-> normalize_url
-> scrape page by Jina or Firecrawl
-> check each context against page markdown
-> output:
   {url, context, label}
```

`label` 是：

```text
is_factual = 1   fully supported
is_factual = 0   partially supported / uncertain
is_factual = -1  not supported
```

这对 research report 很关键。

一个报告如果不能把 claim 映射到 evidence，它就不能进入高质量研究闭环。

我们自己的 Research OS 需要把事实核查从“事后看看”变成“产出结构的一部分”：

```text
每一个关键结论都要有 source。
每一个 source 都要有 snapshot。
每一个 snapshot 都要能反查支持了哪些 claim。
```

## 合在一起的闭环

把两个项目合起来，可以得到一个明确架构：

```text
User Question
  |
  v
Auto-Deep-Research
  |
  |-- Web Surfer: search / browse / page markdown
  |-- File Surfer: local docs / PDFs / tables / media
  |-- Coding Agent: scripts / data / repo analysis
  |-- Triage Agent: routing / completion
  |
  v
Research Report
  |
  v
DeepResearch-Eval
  |
  |-- Quality scoring
  |-- Redundancy scoring
  |-- Fact checking
  |
  v
Evaluation Result
  |
  v
Gap Diagnosis
  |
  v
Next Research Plan
```

这就是 `Deep Research Product Loop`。

## 和 HKUDS040 VideoAgent 的关系

上一站 `HKUDS040 VideoAgent` 处理的是：

```text
video / meeting / lecture / interview workflow
```

Auto-Deep-Research 处理的是：

```text
web / file / code research workflow
```

DeepResearch-Eval 处理的是：

```text
report evaluation workflow
```

三者合起来就是：

```text
VideoAgent
  -> 把访谈、课程、会议变成 transcript / notes / evidence

Auto-Deep-Research
  -> 把问题、网页、文件、代码变成 research report

DeepResearch-Eval
  -> 检查 report 质量、冗余和事实支撑
```

这对我们很实用。

比如田渊栋访谈、硅谷 101、quant senior talk、导师 meeting：

```text
VideoAgent 转 transcript
Auto-Deep-Research 补充背景资料
DeepResearch-Eval 评估最终 memo
Litewrite 发布成 blog / report
UpSkill 把过程失败沉淀成 skill
MoChat / OpenPhone 做 follow-up
```

这就是个人 AI scientist workspace。

## 和 HKUDS033-039 的连接

Agent Product 系列到这里可以连成一张表：

| HKUDS | Project | 在闭环中的位置 |
|---|---|---|
| 033 | ClawTeam | 组织多个 agent，形成 team / role / task |
| 034 | ClawWork | 给任务加上成本、质量、交付和收益 |
| 035 | FastAgent | 统一 planner / executor / evaluator / tool layer |
| 036 | Litewrite | 承接研究报告、proposal、blog、paper artifact |
| 037 | OpenPhone | 进入真实 app、手机、外部沟通界面 |
| 038 | MoChat | 作为沟通、机会、协作入口 |
| 039 | UpSkill Revisited | 把失败和流程沉淀成可复用 skill |
| 040 | VideoAgent | 把视频、会议、访谈变成可处理工作流 |
| 041 | Auto-Deep-Research / DeepResearch-Eval | 把深度研究和评估合成闭环 |

所以 `HKUDS041` 是一个收束点。

它把前面的 product surface 拉回到 research core：

```text
研究问题
证据
报告
评估
下一轮
```

## 对 Pengyi Research OS 的启发

我们自己的 Research OS v0 不应该一开始就追求巨型系统。

更合理的是做一个最小闭环：

```text
research_task.yaml
sources.jsonl
notes.md
report.md
eval.json
human_review.md
next_plan.md
```

每次做一个项目学习或研究问题，都走同一个流程：

```text
1. 写清楚问题
2. 收集网页 / 文件 / repo / video evidence
3. 生成结构化笔记
4. 写报告
5. 自评质量
6. 人类 PM 审核
7. 生成下一轮任务
8. 发布 public-safe 版本
```

这比只堆工具更重要。

## 对 Quant Research OS 的启发

Quant 场景可以直接映射：

```text
Research Question:
  某个市场现象是否能形成可交易因子？

Evidence:
  paper
  blog
  exchange data
  company filing
  macro data
  price/volume data
  alternative data description

Producer:
  生成 factor thesis memo

Developer:
  实现 feature / factor

Backtest:
  生成 performance report

Evaluator:
  评估 lookahead bias、survivorship bias、turnover、capacity、regime dependency

Human PM:
  决定继续、停止、改造、上线 paper trading
```

Auto-Deep-Research 现在还不是 quant backtest agent。

但它提供了：

```text
research question -> evidence -> report
```

DeepResearch-Eval 提供了：

```text
report -> quality / redundancy / factuality
```

我们需要补的是：

```text
quant-specific evaluator
```

也就是：

```text
factor plausibility
data availability
implementation risk
backtest validity
transaction cost sensitivity
cross-market robustness
economic rationale
PM decision
```

## 对 RA / PhD 的启发

RA / PhD 申请里，最稀缺的不是“我很感兴趣”。

更有说服力的是：

```text
我能把一个开放问题变成可复现研究流程。
我能收集证据。
我能写报告。
我能评估报告质量。
我能发现下一轮研究缺口。
我能把这个流程工程化。
```

这就是 AI scientist 的雏形。

我们之后联系导师、RA、quant senior 时，可以把这个闭环讲清楚：

```text
I am building a personal Research OS for AI-assisted research production.
It combines deep research, evidence tracking, report generation, quality evaluation, and human PM review.
In quant research, the same loop becomes factor hypothesis, implementation, backtest, diagnosis, and next-plan generation.
```

## 可以提 PR 的方向

### Auto-Deep-Research

适合的 PR 方向：

| Direction | Why |
|---|---|
| Evidence log | 记录每次 web/file/code action 产生的 source 和 snapshot |
| Report artifact schema | 把最终结果保存为 `report.md`、`sources.jsonl`、`run.json` |
| Human review hook | 在 `case_resolved` 之后进入 review step |
| Example quant prompt | 增加 public-safe quant research demo |
| README encoding cleanup | README badge/emoji 在某些 Windows console 下显示异常，可补充纯文本说明 |
| Config docs | 对 `COMPLETION_MODEL`、`FN_CALL`、`API_BASE_URL` 给更明确示例 |

### DeepResearch-Eval

适合的 PR 方向：

| Direction | Why |
|---|---|
| Report schema documentation | 明确输入 JSONL、输出 JSON 的字段定义 |
| Evaluation aggregation script | 把单篇 JSON 汇总成 CSV / leaderboard |
| Claim extraction step | 从 report 自动抽取 key claims，再喂给 fact checker |
| Quant eval rubric | 增加 finance / quant research memo 的评分维度 |
| Reproducible demo | 用小样本和 mock provider 跑通无 API 的单元测试 |
| Encoding note | Windows 中文 README 展示可补充 UTF-8 使用说明 |

最适合我们的不是大改核心算法，而是：

```text
补 evidence schema、evaluation schema、demo、documentation、quant-specific rubric。
```

这和我们当前能力、网站输出、开源贡献路径最匹配。

## 我们自己的最小可行版本

可以先做一个轻量版，不要等完整 agent 系统：

```text
pengyi_research_os/
  tasks/
    hkuds041/
      task.yaml
      sources.jsonl
      notes.md
      report.md
      eval.json
      human_review.md
      next_plan.md
```

`task.yaml`：

```yaml
id: hkuds041
question: How do Auto-Deep-Research and DeepResearch-Eval form a deep research product loop?
owner: pengyi
status: human_review
inputs:
  - Auto-Deep-Research README
  - Auto-Deep-Research source tree
  - DeepResearch-Eval README_ZH
  - DeepResearch-Eval scripts
outputs:
  - report.md
  - website_post
```

`eval.json`：

```json
{
  "comprehensiveness_score": 3,
  "coherence_score": 4,
  "clarity_score": 3,
  "insight_score": 4,
  "overall_score": 4,
  "repeat_score": 3,
  "human_pm_decision": "publish",
  "next_round": [
    "build evidence schema",
    "design quant research evaluator",
    "connect blog generation to evaluation checklist"
  ]
}
```

先把流程跑起来，再逐步自动化。

## 风险和限制

这两个项目合起来很强，但也有边界：

| Risk | Meaning |
|---|---|
| API dependency | 运行需要 LLM API、Jina / Firecrawl 等外部服务 |
| Browser / Docker complexity | Auto-Deep-Research 真跑起来需要 Docker 和浏览器环境 |
| Evaluation judge bias | LLM-as-a-judge 不是绝对真理，需要 human PM 校准 |
| Claim extraction missing | DeepResearch-Eval 的 fact checking 还需要输入 contexts，不是完整自动闭环 |
| Report-centric | 它评估 report，不直接评估代码、实验、backtest |
| No quant-specific bias diagnosis | 对 quant 来说还缺 lookahead、survivorship、transaction cost 等专门检查 |

所以我们的判断应该是：

```text
它们不是终点。
它们是 Research OS 的两个关键模块。
```

## 最后总结

`HKUDS041` 的核心结论：

```text
Auto-Deep-Research 让 agent 生成研究报告。
DeepResearch-Eval 让系统评估研究报告。
二者组合后，才真正接近 AI scientist 的生产闭环。
```

它给 Pengyi Research OS 的最大启发是：

```text
不要只做会回答问题的 agent。
要做能产出、能被评估、能复盘、能进入下一轮研究计划的 agent。
```

它给 Quant Research OS 的最大启发是：

```text
因子研究也应该有 producer + evaluator：

hypothesis
-> evidence
-> implementation
-> backtest
-> bias diagnosis
-> PM review
-> next experiment
```

这就是我们要继续往前推进的方向。
