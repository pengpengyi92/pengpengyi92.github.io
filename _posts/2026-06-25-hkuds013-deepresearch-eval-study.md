---
title: "HKUDS013: DeepResearch-Eval 作为 Report-Centric Evaluation 与 Factuality Checking Layer"
date: 2026-06-25 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds013, hkuds, deepresearch-eval, report-evaluation, factuality-checking, llm-as-a-judge, qwen-deepresearch, research-agent, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第十四篇。

```text
HKUDS013 -> DeepResearch-Eval
```

上一篇 `HKUDS012` 我们看了 `Auto-Deep-Research`：

```text
Auto-Deep-Research = Open Deep Research Product + AutoAgent Application Layer
```

这一篇自然接上评测层：

```text
DeepResearch-Eval = Report-Centric Evaluation + Factuality Checking Layer
```

也就是说，`Auto-Deep-Research` 负责生成 research report，`DeepResearch-Eval` 负责回答一个更硬的问题：

```text
这个 research report 到底好不好？
它是不是全面？
结构是不是清楚？
表达是不是一致？
有没有洞见？
是不是事实可靠？
是不是大量重复？
```

这对 Pengyi Research OS 非常关键。因为一个 research agent 如果只能生成报告，但不能评估报告，它就很难进入可迭代的 R&D loop。

真正的闭环应该是：

```text
research question
-> deep research report
-> report quality evaluation
-> factuality checking
-> redundancy diagnosis
-> next-round research plan
-> Human PM review
```

`DeepResearch-Eval` 正是在补这个 evaluation layer。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `DeepResearch-Eval`。

| Item | Value |
|---|---|
| repo | `DeepResearch-Eval` |
| remote | `https://github.com/HKUDS/DeepResearch-Eval.git` |
| branch | `main` |
| local head | `d57f8c9` |
| latest local commit date | `2025-10-16` |
| latest local commit | `Update README_ZH.md` |
| status | clean, synced with `origin/main` after fetch |
| paper in README | `arXiv:2510.07861` |
| paper title in README | `Understanding DeepResearch via Reports` |
| Python requirement | `3.10+` |
| tracked files by `rg --files` | 14 |
| topic data count | 100 |
| Qwen report data count | 100 |
| syntax check | `py -m py_compile judge_score.py judge_fact.py Atools.py Aprompts.py` passed |

项目结构很小：

```text
DeepResearch-Eval/
  judge_score.py
  judge_fact.py
  Atools.py
  Aprompts.py
  judge_score.sh
  judge_fact.sh
  data/
    topic/high_quality_topics.jsonl
    report/qwen-reports.jsonl
  example/
    judge_fact_result/
    judge_score_result/
  README.md
  README_ZH.md
  Communication.md
```

一句话先行：

```text
DeepResearch-Eval 不评估 agent 内部轨迹，而是通过最终报告反推 DeepResearch 系统质量。
```

## 为什么通过 Report 理解 DeepResearch

README 的核心观点很直接：

```text
Reports are the most canonical and representative outputs of DeepResearch.
```

这句话非常关键。

DeepResearch 系统内部可能有很多形式：

```text
搜索
浏览
引用
思考
摘要
规划
工具调用
多轮反思
```

但最后用户真正拿到的，通常是一份 report。

所以评估 DeepResearch，最现实的入口就是评估 report：

```text
report 是否覆盖问题？
report 是否有结构？
report 是否有信息密度？
report 是否有可靠引用？
report 是否减少重复？
report 是否支持后续决策？
```

这和我们自己的 Research OS 方向完全一致。

在 quant research 里，最终给 PM 看的也不是 agent 的所有中间动作，而是：

```text
research memo
factor card
backtest report
risk diagnosis
next-round plan
```

所以 report-centric evaluation 是一个非常务实的评价入口。

## 项目解决什么问题

`DeepResearch-Eval` 主要解决三个问题：

```text
1. Report quality scoring
2. Report redundancy detection
3. Report factuality checking
```

对应两个主要脚本：

| Script | Purpose |
|---|---|
| `judge_score.py` | 对完整 report 做质量评分和重复度检测 |
| `judge_fact.py` | 对 report 中的句子和网页证据做 factuality checking |

核心工具与 prompt：

| File | Role |
|---|---|
| `Atools.py` | LLM 调用、网页抓取、文本切分、heading 提取、重复度 pair 生成 |
| `Aprompts.py` | quality judge、fact check、repeatability judge 的 prompt |

数据：

| Data | Content |
|---|---|
| `data/topic/high_quality_topics.jsonl` | 100 个高质量 research topics |
| `data/report/qwen-reports.jsonl` | 100 份 Qwen-DeepResearch 生成的 report |
| `example/judge_fact_result/` | fact checking 输入输出示例 |
| `example/judge_score_result/` | report scoring 输出示例 |

## 两条评估链

这个项目的核心可以画成两条线。

### 1. Quality + Redundancy Chain

```text
topic + report
-> split paragraphs
-> extract ## sections
-> LLM-as-a-Judge quality scoring
-> random section-pair sampling
-> LLM-as-a-Judge repeatability scoring
-> per-report JSON result
```

入口：

```bash
python judge_score.py \
  --inputpath data/report/qwen-reports.jsonl \
  --outputpath exp/score_results
```

输入 schema：

```json
{"topic": "...", "report": "# Report Title\n\n## Section\n..."}
```

输出 schema：

```json
{
  "file_id": "...",
  "topic": "...",
  "compare_list": [],
  "repeat_results": [],
  "comprehensiveness_score": 2,
  "coherence_score": 3,
  "clarity_score": 4,
  "insight_score": 3,
  "overall_score": 3,
  "repeat_score": 3.12,
  "quality_reason": "..."
}
```

这里的关键是：它不只是给一个 overall 分数，而是把 report 拆成多个可诊断维度。

### 2. Fact Checking Chain

```text
url + report contexts
-> normalize URL
-> scrape page content with Jina / Firecrawl
-> judge each sentence against scraped markdown
-> output factual label
```

入口：

```bash
python judge_fact.py \
  --inputpath example/judge_fact_result/example_fact_judge_input.jsonl \
  --outputpath example/judge_fact_result/example_fact_judge_output.jsonl \
  --provider jina \
  --limit 3 \
  --task judge
```

输入 schema：

```json
{"https://example.com/page": {"contexts": ["Sentence A", "Sentence B"]}}
```

输出 schema：

```json
{"url": "...", "context": "...", "label": {"is_factual": 1, "sentence_support": "..."}}
```

事实性标签：

| Label | Meaning |
|---:|---|
| `1` | fully supported |
| `0` | uncertain / partially supported |
| `-1` | not supported |

这条链非常重要。因为很多 DeepResearch 报告最大的问题不是写得不好，而是“看起来很好，但事实不稳”。

## 五个 Report Quality 维度

`Aprompts.py` 里把报告质量拆成五个维度：

| Dimension | 中文理解 |
|---|---|
| `Comprehensiveness_Score` | 全面性和深度 |
| `Coherence_Score` | 结构清晰度和逻辑 |
| `Clarity_Score` | 表达流畅性和一致性 |
| `Insightfulness_Score` | 材料整合和原创洞见 |
| `Overall_Score` | 整体偏好和综合质量 |

评分是 0 到 4 分。

它的 prompt 里还有一个重要约束：

```text
A satisfactory performance deserves around 2 points,
with higher scores for excellence and lower scores for deficiencies.
```

这说明它不是默认给高分，而是把 2 分作为满意基线。

对我们做 Research OS 来说，这一点很重要。评估体系不能天然鼓励“看起来都不错”，否则后面无法驱动改进。

## Redundancy Detection

报告重复度检测的实现方式很直接：

```text
extract_first_level_headings(report)
-> sections_with_headings
-> filter sections shorter than 200 chars
-> generate_random_pair_with_label(...)
-> judge_repeatability_pair(passage1, passage2)
-> average repeat_score
```

注意这里的 repeatability score 是“越高越少重复”：

| Score | Meaning |
|---:|---|
| `4` | almost no repetition |
| `3` | slight repetition |
| `2` | some repetition |
| `1` | severe repetition |
| `0` | excessive repetition |

这是一个非常实用的设计。

DeepResearch 报告经常有一个问题：

```text
表面很长，但多个 section 反复讲同一件事。
```

重复度检测就是在衡量：

```text
报告长度是否真的转化成了信息增量。
```

这对 quant research memo 也完全适用。一个策略研究报告不能只是反复讲“这个因子有效”，而要不断增加新证据：

```text
经济直觉
数据覆盖
分组收益
换手
交易成本
稳定性
行业中性
风险暴露
失败案例
下一轮实验
```

## Fact Checking 的实现

`judge_fact.py` 的实现非常简洁：

```text
read JSONL line
-> normalize_url(raw_url)
-> scrape page content
-> for each context sentence:
     check_factual(sentence, page_content)
-> write JSONL result
```

`normalize_url` 处理一种现实情况：URL 可能被 markdown 包了一层，例如：

```text
https://a.com/x](https://a.com/x)
```

它会用正则抓取最后一个 `http(s)://` URL。

网页抓取在 `Atools.py` 的 `SearchAgent.scrape` 里：

| Provider | Method |
|---|---|
| `jina` | `https://r.jina.ai/{url}` |
| `firecrawl` | `FirecrawlApp.scrape_url(..., formats=['markdown'])` |

`check_factual` 使用 `gpt-4o` 做判断，只允许基于给定网页内容，不允许依赖外部知识。

这条原则很关键：

```text
Fact checking should judge against evidence, not model memory.
```

如果 future Research OS 要接入这个模块，也必须保留这个原则。

## LLM-as-a-Judge 的边界

`DeepResearch-Eval` 是 LLM-as-a-Judge 工具包，但它没有假装 LLM judge 就等于真理。

README 里说它是 hybrid evaluation：

```text
LLM-as-a-Judge for automated report-quality assessment
+ expert judgments for reliability
```

这对我们很重要。

在真实科研或量化场景里，LLM judge 适合做：

```text
初筛
结构化评分
发现明显问题
生成诊断理由
降低人工审阅成本
```

但最终判断仍然需要：

```text
domain expert
human PM
实验结果
真实数据
可复现证据
```

所以它不是 Human PM 的替代，而是 Human PM 的放大器。

## 与 Auto-Deep-Research 的关系

这两篇可以直接连起来：

| Layer | Project | Role |
|---|---|---|
| Generation | `Auto-Deep-Research` | 生成 deep research report |
| Evaluation | `DeepResearch-Eval` | 评估 report 质量、重复度、事实性 |

组合起来就是：

```text
Auto-Deep-Research
-> report
-> DeepResearch-Eval
-> score + factuality labels + redundancy diagnosis
-> next research iteration
```

这才是 agent system 进入“自我改进”的基本形式。

如果没有 evaluation，agent 只能产出。

有了 evaluation，agent 才能迭代。

## 与 AI-Researcher 的关系

`AI-Researcher` 更像完整的 autonomous scientific discovery workflow：

```text
idea
survey
plan
implementation
judge
experiment analysis
paper writing
```

`DeepResearch-Eval` 更专注于 final report 的质量。

它们的关系可以这样理解：

```text
AI-Researcher = research workflow benchmark
DeepResearch-Eval = research report evaluation benchmark/toolkit
```

AI-Researcher 评的是“能不能完成科研流程”。

DeepResearch-Eval 评的是“最终报告是否可靠、有结构、有信息密度”。

两者都重要，但切面不同。

## 与 DeepInnovator 的关系

`DeepInnovator` 的目标是训练更会提出科研 idea 的模型。

`DeepResearch-Eval` 可以为它提供一种结果评价信号：

```text
好的 idea
-> 生成 research report
-> report quality / factuality / redundancy
-> 作为 reward 或 filtering signal
```

当然，report 好不等于 idea 本身一定好。

但 report evaluation 可以成为 idea evaluation 的一部分：

```text
idea novelty
idea feasibility
report comprehensiveness
evidence reliability
implementation path clarity
```

这对 future AI scientist training 很有价值。

## 对 Pengyi Research OS 的启发

我们之前写过：

```text
R&D Agent for Quant Research
= 自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ 人类 PM 审核
```

现在要补一层：

```text
+ 自动评估研究报告质量
+ 自动核查关键事实和引用
+ 自动诊断重复和信息密度
```

也就是说，未来 Pengyi Quant Research OS 可以有这样的模块：

```text
Research Report Evaluator
  - completeness score
  - structure score
  - clarity score
  - insight score
  - factuality labels
  - redundancy score
  - missing evidence list
  - next-round research checklist
```

这对 Human PM 特别重要。

PM 不应该从零读一份长报告，而应该先看到：

```text
质量评分
事实风险点
重复段落
缺失维度
需要人工复核的证据
下一步实验建议
```

这才是 AI 解放生产力。

## Quant Research 场景怎么用

在量化研究中，report evaluation 可以评估几类文档。

### 1. Factor Research Memo

输入：

```text
某个因子假设 + 研究报告
```

评价：

```text
经济逻辑是否完整
是否解释数据来源
是否说明 universe / rebalance / holding period
是否讨论交易成本
是否讨论拥挤度和容量
是否有 out-of-sample 和 robustness
```

### 2. Market Structure Report

输入：

```text
公开市场结构调研报告
```

评价：

```text
市场机制是否讲清楚
关键制度是否有来源
数据口径是否明确
引用是否支持关键结论
```

### 3. Project Due Diligence

输入：

```text
某个开源 quant / trading agent project 的调研报告
```

评价：

```text
是否覆盖 README / code / issues / examples
是否区分 claim 和 verified fact
是否说明 limitations
是否提出可执行接入路径
```

### 4. RA / PhD Lab Research

输入：

```text
导师/实验室调研 memo
```

评价：

```text
是否覆盖近期论文
是否覆盖项目和学生
是否提炼研究路线
是否有具体套磁切入点
是否有证据链接支持
```

这和我们当前阶段非常贴合。

## 工程实现里的关键点

### 1. JSON repair

`Atools.py` 使用：

```text
json_repair
```

这是一个务实选择。LLM judge 常常输出不完全合法 JSON，直接 `json.loads` 会很脆。

在 evaluation pipeline 里，robust parsing 很重要。

### 2. Checkpoint

`judge_score.py` 设计了 `CheckpointManager`：

```text
processed_files
current_index
start_time
total_files
```

报告评估通常调用 LLM 很慢，而且成本高。checkpoint 是必要的。

但本地代码有一个问题：main 里读取 checkpoint 后，又无条件执行：

```text
checkpoint_manager.checkpoint_data['processed_files'] = []
checkpoint_manager.checkpoint_data['current_index'] = 0
checkpoint_manager.checkpoint_data['total_files'] = len(all_json_data)
```

这会让 `--resume` 的实际效果变弱，甚至和 README 描述不一致。

这是一个很适合提 PR 的点。

### 3. Random Pair Sampling

重复度检测使用随机 section pair：

```text
random.sample(all_possible_pairs, pair_nums)
```

但目前没有暴露 seed。

这会导致同一份 report 的 repeat_score 可能在不同 run 之间略有变化。

如果用于 benchmark，应该支持：

```text
--seed 42
```

这样结果才更可复现。

### 4. Hardcoded Judge Model

`Atools.py` 里多个地方硬编码：

```text
model="gpt-4o"
```

这对 demo 没问题，但对可复现实验和成本控制不够灵活。

更好的方式是：

```text
JUDGE_MODEL env var
--model cli arg
```

尤其是我们未来要评估大量 report，judge model 成本和一致性会很重要。

### 5. README 示例路径

README 的 quality evaluation quickstart 写的是：

```bash
python judge_score.py \
  --inputpath data/topic/high_quality_topics.jsonl \
  --outputpath exp/score_results
```

但 `judge_score.py` 实际需要每行同时有：

```text
topic
report
```

而 `data/topic/high_quality_topics.jsonl` 只有 topic，没有 report。

真正更匹配的示例应该是：

```bash
python judge_score.py \
  --inputpath data/report/qwen-reports.jsonl \
  --outputpath exp/score_results
```

这也是一个非常明确的小 PR。

## 可以提的 PR

这次我看到几个很适合 contributor 的切入点：

| PR Idea | Why it matters |
|---|---|
| 修正 README quality evaluation 输入路径 | 当前 quickstart 指向 topic-only 文件，脚本需要 report |
| 修复 `--resume` checkpoint 逻辑 | README 宣称支持 resume，但 main 里无条件重置 processed list |
| 给重复度抽样增加 `--seed` | benchmark 结果更可复现 |
| 增加 `--model` 或 `JUDGE_MODEL` | 不硬编码 `gpt-4o`，方便成本控制和复现实验 |
| 把 checkpoint 路径放到 output dir | 避免多个实验共享 cwd checkpoint |
| 增加 mini CI smoke test | 用极小 mock JSONL 检查 schema 和输出路径 |
| 增加 fact-check input builder | 从 report reference section 自动生成 url/context 输入 |
| 增加 result aggregation script | 把 per-report JSON 汇总成 CSV / leaderboard |

最适合马上做的两个是：

```text
1. README inputpath fix
2. --resume checkpoint fix
```

这两个范围小、影响明确、容易 review。

## 它在 HKUDS Map 中的位置

更新一下 HKUDS map：

| ID | Project | Role |
|---|---|---|
| HKUDS000 | Study Map | project map |
| HKUDS001 | LightRAG | graph-based knowledge memory |
| HKUDS002 | Vibe-Trading | quant research workflow |
| HKUDS003 | nanobot | personal agent shell |
| HKUDS004 | CLI-Anything | software action layer |
| HKUDS005 | AI-Trader | live trading platform layer |
| HKUDS006 | AgentSpace | organizational agent workspace |
| HKUDS007 | RAG-Anything | multimodal document ingestion |
| HKUDS008 | AutoAgent | agent framework / factory |
| HKUDS009 | DeepCode | research-to-code implementation |
| HKUDS010 | AI-Researcher | autonomous scientific discovery workflow |
| HKUDS011 | DeepInnovator | scientific idea model training |
| HKUDS012 | Auto-Deep-Research | practical deep research assistant |
| HKUDS013 | DeepResearch-Eval | report evaluation and factuality checking |

这一篇补上的不是 generation，而是 evaluation。

从系统角度看，这非常重要：

```text
without evaluation, no iteration
without factuality checking, no trust
without redundancy diagnosis, no information density
```

## Pengyi Research OS 的闭环图

把 HKUDS012 和 HKUDS013 接起来，可以形成一个最小闭环：

```text
User / PM asks research question
        |
        v
Auto-Deep-Research style agent
        |
        v
Deep research report
        |
        v
DeepResearch-Eval
  - quality scoring
  - redundancy scoring
  - factuality checking
        |
        v
Diagnosis report
        |
        v
Human PM review
        |
        v
Next research plan
```

如果换成 quant research：

```text
Quant PM asks factor question
        |
        v
Deep research agent gathers papers/blogs/market structure
        |
        v
Factor hypothesis memo
        |
        v
Report evaluator checks:
  - economic logic completeness
  - evidence coverage
  - factuality of public claims
  - missing data assumptions
  - repetitive sections
        |
        v
Implementation/backtest agent
        |
        v
Backtest diagnosis
        |
        v
Human PM go/no-go
```

这就是我们要的 R&D Agent 形状。

## 一个后续练习

我会把 `HKUDS013` 后续拆成三个练习。

### Exercise 1: 跑通 report scoring

目标：

```text
用 data/report/qwen-reports.jsonl 跑 judge_score.py 的最小样例
```

验证：

```text
生成 per-report JSON
包含 quality scores
包含 repeat_score
包含 quality_reason
```

注意：

```text
需要 OPENAI_API_KEY
当前默认 judge model 是 gpt-4o
```

### Exercise 2: 跑通 fact checking

目标：

```text
使用 example_fact_judge_input.jsonl
用 jina provider 抓取网页并验证 contexts
```

验证：

```text
输出 url/context/label
label 里有 is_factual 和 sentence_support
```

注意：

```text
需要 JINA_API_KEY 或 FIRECRAWL_KEY
不同网页抓取质量会影响 fact checking
```

### Exercise 3: 改成 Quant Research Evaluator

目标：

```text
基于 Aprompts.py 改出 quant research memo scoring prompt
```

维度可以是：

```text
economic intuition
data specification
backtest design
risk control
transaction cost awareness
robustness
implementation clarity
next-step usefulness
```

这会非常适合我们的 Quant Research OS。

## 最后总结

`HKUDS013` 的一句话总结：

```text
DeepResearch-Eval 是一个围绕最终 research report 建立的评估工具包：
它用 LLM-as-a-Judge 评估全面性、结构、表达、洞见和整体质量，
用段落 pair 判断信息重复度，
再用网页抓取 + factuality judge 检查关键句是否被证据支持。
```

它对 Pengyi Research OS 的核心启发是：

```text
Research agent 必须有 evaluator。
```

只会生成报告的系统还不够。

能评估报告、指出事实风险、减少重复、推动下一轮研究计划的系统，才更接近真正的 R&D Agent。
