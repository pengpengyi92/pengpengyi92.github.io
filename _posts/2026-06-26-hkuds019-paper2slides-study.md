---
title: "HKUDS019: Paper2Slides 作为 Research-to-Presentation Artifact Generation Layer"
date: 2026-06-26 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds019, hkuds, paper2slides, research-to-presentation, slides, poster, rag-anything, artifact-generation, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第二十篇。

```text
HKUDS019 -> Paper2Slides
```

上一篇 `HKUDS018` 看的是 `MiniRAG`：

```text
MiniRAG = Lightweight Graph RAG + On-Device Knowledge Layer
```

`MiniRAG` 解决的是低成本、轻量、本地、端侧的知识层问题。

这一篇 `Paper2Slides` 进入另一个非常关键的层：

```text
Paper2Slides = Research-to-Presentation Artifact Generation Layer + Scientific Communication Layer
```

也就是：

```text
把论文、报告、项目文档、技术材料，转成可以对外沟通的 slides / poster / visual artifact。
```

这个 repo 对我们的意义非常直接。

Research OS 不是只读 paper，也不是只写 notebook，更不是只把结果放在本地文件夹里。
真正能产生影响力的研究系统，最后必须能把研究产物变成：

```text
talk slides
poster
demo deck
RA pitch
PhD pre-communication material
project review
strategy presentation
investment committee material
```

所以 `Paper2Slides` 是我们之前一系列 HKUDS 项目的自然下游。

```text
RAG-Anything / LightRAG / MiniRAG -> understand and organize knowledge
AI-Researcher / DeepInnovator -> produce research direction and experiment trace
AnyTool / OpenHarness -> execute and manage tools
Paper2Slides -> turn research into communicable artifact
```

这一步很关键。

因为研究价值不只在“想到了什么”，也在“能不能把它讲清楚，让别人理解，让组织采纳，让资源流向你”。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `Paper2Slides`。

| Item | Value |
|---|---|
| repo | `Paper2Slides` |
| remote | `https://github.com/HKUDS/Paper2Slides.git` |
| branch | `main` |
| local head | `0785051` |
| full commit | `0785051d1f52814097e94c44751e3f12b83f7c8a` |
| latest local commit date | `2026-05-20 17:08:05 +0800` |
| latest local commit | `chore: restore env example` |
| status | clean, synced with `origin/main` after fetch |
| local tags | none |
| license | `MIT`, copyright `2025 HKUDS` |
| Python requirement | `Python 3.12+` |
| tracked files by `git ls-files` | 102 |
| Python files | 52 |
| frontend tracked files | 20 |
| dependency file | `requirements.txt` |
| frontend stack | React 18 + Vite + TailwindCSS |
| backend stack | FastAPI + Uvicorn |
| RAG stack | `lightrag-hku`, MinerU, embedded `raganything` implementation |
| image generation providers | OpenRouter by default, Google Gemini API optional |
| syntax check | `py -m compileall -q paper2slides api` passed |
| package metadata | no root `pyproject.toml`, `setup.py`, or root `package.json` observed |

一句话先行：

```text
Paper2Slides 把“科研材料理解”拆成 RAG、summary、plan、generate 四个阶段，并用 checkpoint 管理中间产物，让论文到 slides/poster 的过程可以恢复、复用、换风格、重新生成。
```

它不是一个简单的 prompt wrapper。

更准确地说，它是一个 research artifact pipeline。

## 它解决什么问题

科研表达里最痛的事情之一是：

```text
材料很多
图表很多
贡献点分散
slides/poster 要重新组织
还要考虑风格、版式、信息密度、故事线
```

人工做这件事的流程通常是：

```text
读 paper
摘 abstract / intro / method / experiment
截图 figure 和 table
整理每页 slide 的主线
调版式
导出 PDF
复查信息有没有漂移
```

`Paper2Slides` 的目标就是把这条链路自动化。

它支持两个输出方向：

| Output | Meaning |
|---|---|
| `slides` | 多页 presentation slides |
| `poster` | 单张 infographic / academic poster |

它支持两类内容：

| Content Type | Meaning |
|---|---|
| `paper` | 科研论文，重点抽取 title、authors、motivation、method、results、contributions |
| `general` | 通用报告、文档、资料，重点保持细节并组织主题结构 |

它也支持多种风格：

| Style | Meaning |
|---|---|
| `academic` | 学术、专业、干净 |
| `doraemon` | 内置的更活泼视觉风格 |
| custom text | 用户自然语言描述的风格 |

从 Research OS 角度看，这不是“做 PPT”，而是：

```text
把 research object 转换为 communication object。
```

研究系统里的对象可以是：

```text
paper
report
experiment result
benchmark table
research plan
strategy write-up
```

输出对象可以是：

```text
slides deck
poster
视觉化 summary
项目介绍页
导师沟通附件
PM pitch material
```

这就是它对我们的核心启发。

## 四阶段 Pipeline

README 和代码都把 `Paper2Slides` 明确拆成四个阶段：

```text
rag -> summary -> plan -> generate
```

对应到代码：

| Stage | Main File | Output |
|---|---|---|
| RAG | `paper2slides/core/stages/rag_stage.py` | `checkpoint_rag.json` |
| Summary | `paper2slides/core/stages/summary_stage.py` | `checkpoint_summary.json`, `summary.md` |
| Planning | `paper2slides/core/stages/plan_stage.py` | `checkpoint_plan.json` |
| Generation | `paper2slides/core/stages/generate_stage.py` | `slide_*.png`, `slides.pdf`, poster image |

这条链路非常适合我们学习，因为它把一个看起来“端到端”的任务拆成了可调试、可复用的中间层。

```text
raw document
  -> parsed markdown / RAG results
  -> structured summary
  -> content plan
  -> visual artifact
```

这就是工程上真正可维护的方式。

如果只是一条 prompt：

```text
please read this paper and make slides
```

那几乎没有办法调试。

但拆成四个阶段以后，可以分别判断：

```text
是不是文档解析失败？
是不是 RAG query 漏了内容？
是不是 summary 抽取不够结构化？
是不是 plan 页数和故事线不好？
是不是 image generation 风格不稳定？
```

这就是 `Paper2Slides` 值得学的地方。

## Stage 1: RAG

`rag_stage.py` 是入口。

它负责：

```text
解析输入文档
构建或跳过 RAG index
执行 paper/general 对应的 query set
保存 rag checkpoint
```

它有两种模式。

| Mode | Meaning | Good For |
|---|---|---|
| normal | 完整解析并构建 RAG index | 长 paper、多文件、复杂文档 |
| fast | 跳过 RAG index，直接把 markdown 和图片喂给 LLM query | 短文档、快速预览、快速换风格 |

normal mode 里会使用 `RAGClient`。

`RAGClient` 本质上封装了：

```text
RAGAnything
LightRAG
OpenAI-compatible LLM
OpenAI-compatible embedding
vision model function
```

代码位置：

```text
paper2slides/rag/client.py
paper2slides/rag/config.py
paper2slides/raganything/
```

`RAGConfig` 里默认支持的文件扩展包括：

```text
.pdf, .jpg, .jpeg, .png, .bmp, .tiff, .tif, .gif, .webp,
.doc, .docx, .ppt, .pptx, .xls, .xlsx, .txt, .md
```

这说明核心 RAG 层是按 multi-format document ingestion 设计的。

但 Web API 目前有一个实现差异，后面 PR 机会会说。

fast mode 的实现也值得注意。

它会先用 `BatchParser` 把文档解析成 markdown，然后把 markdown 里的图片引用替换成 base64 inline image content，再按 query category 并发调用 LLM。

也就是说 fast mode 不是完全不解析，而是：

```text
parse only
no RAG indexing
direct multimodal LLM query
```

这对短 paper 很有用。

如果我们之后做 Research OS，可以借鉴这个思路：

```text
短材料 -> fast direct query
长材料 -> full RAG indexing
重要材料 -> full RAG + cached summary
```

## Stage 2: Summary

`summary_stage.py` 负责从 RAG results 里抽出结构化内容。

对于 `paper` 类型，它重点抽：

```text
paper_info
motivation
solution
results
contributions
```

代码里 `PaperContent` 的结构很清楚：

```text
paper_info
figures
tables
equations
motivation
solution
results
contributions
raw_rag_results
```

这里有一个实用设计：

```text
paper metadata 直接从 markdown 前几千字符抽取，不完全依赖 RAG。
```

原因很合理。

论文标题、作者、机构一般就在开头，直接抽比通过 RAG 查询更稳。

然后它会对 motivation、solution、results、contributions 这些关键段落再用 LLM 做结构化抽取。

同时，`summary/extractors/` 里还有表格和图片抽取器：

```text
extractors/table_extractor.py
extractors/figure_extractor.py
extractors/__init__.py
```

它们会从 markdown 中找：

```text
<table>...</table>
![alt](images/...)
```

再在附近搜索 caption。

这一步非常重要。

因为 slides/poster 不是只有文字，paper 的图和表往往是核心证据。

如果没有把 figure/table 当成 first-class object，后续 presentation 会非常虚。

`Paper2Slides` 把这些原始元素保存为：

```text
TableInfo
FigureInfo
OriginalElements
```

这样 planning 阶段就能显式引用：

```text
这页用 Figure 1
这页用 Table 2 的某部分
这页强调某个图里的结构
```

这对我们的 Quant OS 也很关键。

量化研究报告里的关键对象不是只有文字，还有：

```text
IC curve
factor decay
turnover table
drawdown chart
PnL curve
sector exposure table
correlation heatmap
ablation table
```

所以 Paper2Slides 的 `OriginalElements` 思路可以迁移成：

```text
QuantOriginalElements = charts + tables + metrics + configs + backtest traces
```

## Stage 3: Planning

`plan_stage.py` 会把 summary 和 original elements 交给 `ContentPlanner`。

`ContentPlanner` 位于：

```text
paper2slides/generator/content_planner.py
```

它做的事情不是直接生成图片，而是先生成一个 content plan。

这个 plan 里每个 section 具有：

```text
id
title
type
content
tables
figures
```

对于 slides，它会根据 `short / medium / long` 控制页数范围：

| Length | Page Range |
|---|---|
| `short` | 5-8 |
| `medium` | 8-12 |
| `long` | 12-15 |

对于 poster，它会根据 `sparse / medium / dense` 控制信息密度。

这一步非常值得学。

因为从文档到 presentation 的核心不是“摘要”，而是“重新组织故事线”。

一个好的 deck 需要：

```text
opening
problem
method
evidence
result
limitation
conclusion
```

一个好的 poster 需要：

```text
title/header
motivation
method overview
key results
visual elements
takeaway
```

`Paper2Slides` 把这些结构放在 prompt 和 plan object 里。

这比直接让图像模型读全文生成 slide 稳很多。

在我们的 Research OS 里，planning layer 可以更泛化：

| Artifact | Plan Object |
|---|---|
| RA email attachment | 1-page research pitch plan |
| PI meeting | 6-slide talk plan |
| project demo | demo narrative plan |
| quant PM pitch | strategy pitch plan |
| PhD statement | statement outline plan |
| workshop paper | section outline plan |

所以 `Paper2Slides` 的 `ContentPlan` 思路可以成为我们的 artifact planner。

## Stage 4: Generation

`generate_stage.py` 会读取：

```text
checkpoint_plan.json
checkpoint_summary.json
```

然后重建：

```text
ContentPlan
GenerationInput
OriginalElements
```

最后交给：

```text
paper2slides/generator/image_generator.py
```

`ImageGenerator` 支持两个 provider：

| Provider | How |
|---|---|
| `openrouter` | OpenAI-compatible chat completions with image output |
| `google` | Google Gemini API `generateContent` |

默认 provider 是：

```text
IMAGE_GEN_PROVIDER=openrouter
```

默认模型如果走 OpenRouter，是：

```text
google/gemini-3-pro-image-preview
```

生成 slides 时，它有一个很实用的风格一致性设计：

```text
第 1-2 页顺序生成。
第 2 页生成后，被当成后续 slides 的 style reference image。
第 3 页之后可以并行生成。
```

这说明它意识到一个真实问题：

```text
多页 slides 最大的问题不是每页好不好看，而是整套风格是否一致。
```

所以它不是简单并发所有页，而是：

```text
先建立风格 reference，再并行扩展。
```

CLI 里的 `--parallel` 就是控制这个并行生成。

```bash
python -m paper2slides --input paper.pdf --output slides --style doraemon --length medium --fast --parallel 2
```

其中：

```text
--parallel      -> 默认 2 workers
--parallel 4    -> 4 workers
不传 --parallel -> sequential, max_workers = 1
```

生成结束以后，slides 会保存为：

```text
slide_01.png
slide_02.png
...
slides.pdf
```

poster 通常是一张 poster image。

## Checkpoint Design

这个 repo 最值得我们工程上学习的一点，是 checkpoint。

README 给出的输出结构是：

```text
outputs/
  <project_name>/
    <content_type>/
      <mode>/
        checkpoint_rag.json
        checkpoint_summary.json
        summary.md
        <config_name>/
          state.json
          checkpoint_plan.json
          <timestamp>/
            slide_01.png
            slide_02.png
            slides.pdf
        rag_output/
```

代码里对应：

```text
paper2slides/core/paths.py
paper2slides/core/state.py
paper2slides/core/pipeline.py
```

这带来几个能力：

| Ability | Meaning |
|---|---|
| resume | 同一个命令重新运行，可以从缺失 checkpoint 处继续 |
| change style | `--from-stage plan`，跳过 RAG 和 summary |
| regenerate images | `--from-stage generate`，保留 plan，只重新生成图片 |
| full restart | `--from-stage rag`，完全重来 |

这对 Research OS 特别重要。

因为研究工作不是一次性完成的。

我们经常会：

```text
换一个 talk 风格
换一个 audience
同一个 paper 生成 short/medium/long 三个版本
同一个 research report 做导师版、PM 版、公开版
同一个 quant strategy 做 internal pitch 和 sanitized public demo
```

checkpoint 的核心价值就是：

```text
 expensive understanding work should be cached
 cheap presentation variants can be regenerated
```

这句话应该成为我们自己系统的原则。

## CLI

CLI 入口是：

```text
paper2slides/main.py
paper2slides/__main__.py
```

核心参数：

| Option | Meaning | Default |
|---|---|---|
| `--input`, `-i` | 输入文件或目录 | required |
| `--content` | `paper` or `general` | `paper` |
| `--output` | `poster` or `slides` | `poster` |
| `--style` | `academic`, `doraemon`, or custom | `doraemon` |
| `--length` | slides 长度 | `short` |
| `--density` | poster 密度 | `medium` |
| `--output-dir` | 输出目录 | `outputs` |
| `--from-stage` | 从某阶段重跑 | auto |
| `--list` | 列出输出 | false |
| `--debug` | debug logging | false |
| `--fast` | fast mode | false |
| `--parallel` | slide 并行生成 workers | sequential by default |

几个使用方式：

```bash
python -m paper2slides --input paper.pdf --output slides --length medium
python -m paper2slides --input paper.pdf --output poster --density medium
python -m paper2slides --input paper.pdf --output slides --fast
python -m paper2slides --input paper.pdf --output slides --from-stage plan --style academic
python -m paper2slides --list
```

这对我们后续做自己的工具也有启发。

一个好的 Research OS CLI 应该天然支持：

```text
run
resume
list
from-stage
debug
output-dir
variant parameters
```

而不是每次都靠手动改脚本。

## Web UI

`Paper2Slides` 也有 Web UI。

后端：

```text
api/server.py
```

前端：

```text
frontend/
```

前端技术栈：

```text
React 18
Vite
TailwindCSS
lucide-react
axios
```

前端组件包括：

```text
ChatWindow
ConfigPanel
FileUpload
WorkflowPanel
SlidePreview
MessageList
ConversationList
HistoryPanel
```

这说明它不是只做 CLI，而是已经朝产品界面走。

`WorkflowPanel` 里展示四个默认 stage：

```text
RAG
Summary
Plan
Generate
```

这和后端 pipeline 对齐。

这是一个很好的 UX 原则：

```text
用户不只需要结果，也需要知道系统正在做哪一步。
```

尤其是论文解析和图像生成这种长任务，如果没有 workflow status，用户会觉得系统卡住了。

在我们的 Research OS 里也一样。

将来任何长任务都应该有：

```text
stage status
current step
intermediate artifact
resume point
error message
downloadable output
```

## Backend Session Design

FastAPI backend 有 session 管理。

`SessionManager` 做了几件事：

```text
只允许一个 running session
支持 cancel
记录 cancelled session ids
后台任务跑 pipeline
前端轮询 status/result
```

主要接口包括：

| Endpoint | Meaning |
|---|---|
| `/health` | 健康检查 |
| `/api/session/running` | 查询当前是否有任务 |
| `/api/cancel/{session_id}` | 取消任务 |
| `/api/chat` | 上传文件并启动后台 pipeline |
| `/api/status/{session_id}` | 查询 stage 状态 |
| `/api/result/{session_id}` | 获取生成结果 |
| `/api/download/{filepath}` | 下载输出文件 |

这也是完整产品原型需要的东西。

如果只是在 notebook 里生成一张图，不需要 session。

但只要进入 Web 产品：

```text
upload
background task
polling
cancel
result cache
static file serving
download
```

这些都必须要有。

所以 `Paper2Slides` 对我们学工程产品化很有价值。

## Docker

repo 里有 Docker 配置：

```text
docker/Dockerfile.backend
docker/Dockerfile.frontend
docker/docker-compose.yml
docker/nginx.conf
docker/README.md
```

compose 里后端暴露：

```text
8000
```

前端暴露：

```text
5173
```

后端挂载：

```text
../outputs:/app/outputs
../paper2slides/.env:/app/paper2slides/.env
```

这说明它已经考虑了部署和结果持久化。

对我们来说，这种结构可以迁移到未来的 Quant Research OS：

```text
backend service -> experiment / report / artifact generation
frontend workspace -> upload configs, inspect progress, download output
outputs volume -> persistent research artifacts
env mount -> private API keys and provider config
```

这比纯脚本更接近可以给别人用的系统。

## RAG-Anything 的位置

`Paper2Slides` 和前面 `RAG-Anything` 的关系非常自然。

`RAG-Anything` 负责：

```text
多格式文档解析
多模态内容处理
表格/图片/公式入库
把复杂文档变成可检索知识
```

`Paper2Slides` 则把这些能力接到：

```text
slides/poster generation
```

所以两者关系可以这样理解：

| Repo | Position |
|---|---|
| `RAG-Anything` | Multimodal document ingestion and retrieval layer |
| `Paper2Slides` | Research artifact generation layer |

如果只做 RAG-Anything，结果通常是：

```text
问答
摘要
检索结果
structured notes
```

如果再接 Paper2Slides，结果变成：

```text
可展示、可下载、可分享、可用于 meeting 的 artifact。
```

这就是它在 HKUDS 生态里的位置。

## 和 MiniRAG 的关系

`MiniRAG` 是 lightweight knowledge layer。

`Paper2Slides` 是 artifact layer。

两者可以接起来：

```text
MiniRAG stores distilled notes and local research memory.
Paper2Slides turns selected notes / papers / reports into presentation artifacts.
```

对我们来说：

```text
MiniRAG helps remember.
Paper2Slides helps present.
```

这是 Research OS 的两个不同层。

一个负责把知识留住。

一个负责把知识讲出去。

## 和 AI-Researcher 的关系

`AI-Researcher` 更偏研究流程：

```text
idea
literature
experiment
paper draft
review
iteration
```

`Paper2Slides` 可以成为它的下游。

```text
AI-Researcher output -> research report / draft paper
Paper2Slides -> talk slides / poster / project demo
```

如果未来我们做自己的 AI Scientist workflow，不能只输出一堆 markdown。

应该至少输出：

```text
report.md
paper_draft.md
slides.pdf
poster.png
experiment_trace.json
review_notes.md
next_plan.md
```

`Paper2Slides` 就是在提醒我们：

```text
research output should be artifact-complete.
```

## 和 DeepResearch-Eval 的关系

`DeepResearch-Eval` 关注报告质量和 factuality。

`Paper2Slides` 关注报告到 presentation 的生成。

两者之间可以形成一个闭环：

```text
Deep Research system writes report.
DeepResearch-Eval checks quality and factuality.
Paper2Slides turns approved report into slides/poster.
Human reviews the final artifact.
```

这对真实工作非常重要。

因为 presentation 的问题不只是“好看”，还包括：

```text
有没有事实错误
有没有夸大结论
有没有漏掉关键 limitation
图表和原始材料是否对应
```

所以如果我们未来使用 Paper2Slides 思路，前面最好加：

```text
factuality check
source grounding check
claim-evidence alignment
```

否则 slides 做得越漂亮，错误传播得越快。

## 对 Research OS 的启发

`Paper2Slides` 对我们的 `Pengyi Research OS` 最大启发是：

```text
研究系统必须有 artifact generation layer。
```

一个完整的 Research OS 可以这样分层：

| Layer | HKUDS Reference | Role |
|---|---|---|
| document ingestion | `RAG-Anything` | 吃论文、PDF、报告、图片、表格 |
| knowledge memory | `LightRAG`, `MiniRAG` | 组织长期知识和轻量本地记忆 |
| research agent | `AI-Researcher`, `DeepInnovator` | 生成 idea、实验、报告 |
| tool execution | `OpenHarness`, `AnyTool` | 调工具、跑代码、做实验 |
| evaluation | `DeepResearch-Eval` | 检查报告质量和事实 |
| artifact generation | `Paper2Slides` | 生成 slides、poster、demo deck |
| human review | PM / PI / reviewer | 最终判断可否对外 |

这正好对应我们一直说的：

```text
自动提出假设
自动实现
自动回测
自动诊断偏差
自动生成下一轮研究计划
人类 PM 审核
```

但现在可以补上最后一层：

```text
自动生成沟通材料。
```

也就是：

```text
experiment -> report -> slides -> meeting -> feedback -> next experiment
```

这个闭环才是组织里的研究系统。

## 对 Quant Research OS 的启发

量化研究里，`Paper2Slides` 的意义非常大。

Quant research 不是只跑 backtest。

真正组织内产生价值，需要把策略讲给：

```text
PM
CIO
risk team
trading team
data team
compliance
research committee
```

所以每一个策略研究都需要 artifact。

可以设计一条 Quant artifact pipeline：

```text
factor hypothesis
  -> implementation
  -> backtest
  -> bias diagnostics
  -> risk diagnostics
  -> research report
  -> PM pitch deck
  -> investment committee deck
  -> sanitized public demo
```

`Paper2Slides` 可以给我们提供 deck generation 的架构参考。

量化场景里的 `OriginalElements` 可以是：

| Quant Object | Presentation Role |
|---|---|
| IC curve | 因子有效性证据 |
| IC decay | holding period 选择依据 |
| factor return | 收益贡献展示 |
| drawdown curve | 风险说明 |
| turnover table | 交易成本压力 |
| correlation matrix | 因子冗余检查 |
| sector exposure | 风格与行业偏移 |
| ablation table | 组件贡献验证 |
| capacity estimate | 实盘可扩展性 |

然后 planner 生成：

```text
Slide 1: Strategy Thesis
Slide 2: Data and Universe
Slide 3: Factor Definition
Slide 4: Backtest Setup
Slide 5: Performance
Slide 6: Risk and Bias Diagnostics
Slide 7: Capacity and Trading Cost
Slide 8: Failure Cases
Slide 9: Deployment Plan
Slide 10: Next Research Plan
```

这就是我们可以从 Paper2Slides 迁移出来的东西。

不是简单把 quant report 喂给 LLM 做 PPT，而是定义：

```text
QuantContent
QuantOriginalElements
QuantPitchPlan
QuantSlideGenerator
```

这会非常强。

## 对 RA / PhD 申请的启发

`Paper2Slides` 也直接服务于我们最近的目标。

我们现在需要：

```text
RA 套磁
PhD 预沟通
导师邮件附件
AI/Quant research role 材料
科研项目补充材料
网站 research statement 内容源
```

这些材料本质上也可以被 artifact pipeline 处理。

我们可以为每个方向准备：

```text
one-page CV
research statement
project report
5-slide project pitch
1-page visual map
public website post
private talk note
```

`Paper2Slides` 的启发是：

```text
不要每次临时做材料。
先把内容结构化，再生成不同 audience 的版本。
```

例如同一个 `Pengyi Quant Research OS` 项目，可以生成：

| Audience | Artifact |
|---|---|
| RA PI | research potential deck |
| quant PM | strategy system pitch |
| open-source visitor | public project page |
| PhD committee | research statement and technical appendix |
| collaborator | architecture walkthrough |

这就是 artifact generation layer 的价值。

## 对网站内容的启发

我们的网站现在已经开始记录：

```text
HKUDS study map
LLMQuant study map
Research OS notes
Quant OS notes
career planning notes
```

`Paper2Slides` 提醒我们，网站内容也可以进一步分层。

每篇 deep dive 可以自动派生：

```text
blog post
one-page summary
talk slides
project card
learning map entry
private action checklist
```

也就是说，写一篇文章不是终点。

更好的方式是：

```text
write once
structure once
publish many artifacts
```

这是很适合个人研究者的杠杆。

如果我们之后把 `Paper2Slides` 的思想接进网站，就可以做到：

```text
从一篇 markdown 学习笔记生成 public blog
从同一篇笔记生成 RA talk slides
从同一篇笔记生成 private planning memo
从同一篇笔记生成 project card
```

这就是内容资产复用。

## PR Opportunities

这次读下来，有几个比较实际的 PR / issue 机会。

### 1. Web API input support 和 README 描述不完全一致

README 和 Docker 文档都说支持：

```text
PDF, Word, Excel, PowerPoint, Markdown
```

核心 RAG 配置里也确实有多种扩展名。

但 `api/server.py` 的 `generate_slides_with_pipeline()` 当前只筛选：

```python
pdf_files = [f for f in files if f['filename'].lower().endswith('.pdf')]
if not pdf_files:
    raise ValueError("No PDF file found in uploaded files")
```

这意味着 Web UI 上传非 PDF 文件时，可能会和 README 承诺不一致。

可提改进方向：

```text
把 pdf_files 改成 supported_files
复用 RAGConfig.batch.supported_file_extensions
或在 Web UI / README 明确写 Web currently accepts PDF only
```

这个 PR 比较清楚，边界也小。

### 2. CLI 默认 `CUDA_VISIBLE_DEVICES=1`

`main.py` 顶部有：

```python
os.environ.setdefault("CUDA_VISIBLE_DEVICES", "1")
```

这对单 GPU 机器、CPU 环境、Docker 默认环境都可能不友好。

更稳的方式是：

```text
不要默认指定 GPU
或者通过 CLI/env 显式配置
或者只在文档中说明用户如何设置 CUDA_VISIBLE_DEVICES
```

这个也是适合提 issue / PR 的点。

### 3. `run_pipeline()` 捕获 stage 异常后 break，但不 re-raise

`pipeline.py` 里每个 stage 出错时会：

```text
mark failed
save error
break
```

但不会把异常继续抛出。

这对 CLI 或后台任务可能导致一个问题：

```text
上层任务以为 run_pipeline 正常返回，但 state 里面其实失败了。
```

更稳的方式可以是：

```text
return explicit pipeline result
or raise after saving failed state
or let caller choose strict / non-strict behavior
```

这属于工程语义改进。

### 4. custom style 的 config directory name 不够稳

`core/paths.py` 里 custom style 的目录名使用：

```python
suffix = custom[:16].replace(" ", "_").replace("/", "_")
```

问题是：

```text
Windows invalid path chars 可能保留
前 16 字相同的 custom style 会冲突
中文和特殊符号可能造成目录名不稳定
```

更稳的方式：

```text
sanitize all unsafe path chars
append short hash
keep human-readable prefix
```

例如：

```text
custom_minimal_blue_a1b2c3d4
```

### 5. `_update_state_on_error()` 的 config 可能和真实 config 不一致

`api/server.py` 的 `_update_state_on_error()` 构造 config 时没有完整包含：

```text
input_path
custom_style
```

而真实 `generate_slides_with_pipeline()` 会把 message 变成 custom style。

这可能导致 error update 找错 config_dir。

可改进：

```text
在 background task 中保存真实 config
或者让 state error update 通过 session_id 搜索 state.json
不要重建一个可能不一致的 config
```

这个 PR 也很工程化。

### 6. dependency pinning 可以更 reproducible

`requirements.txt` 中：

```text
lightrag-hku
huggingface_hub
openai>=1.0.0
```

一些核心依赖没有完全 pin version。

对研究型项目早期可以理解，但如果要稳定复现 demo，后续可以提供：

```text
requirements.lock
uv.lock
或 Docker pinned build
```

这个更适合 issue，不一定第一优先级 PR。

## 我们可以怎么用

对我们来说，Paper2Slides 的直接行动不是马上接入它跑所有材料，而是先学它的结构。

第一阶段可以做：

```text
把我们的核心 markdown 学习笔记结构化
提取标题、核心观点、系统位置、启发、行动项
生成 5-slide talk outline
人工审核 outline
再决定是否接 image generation
```

也就是先做：

```text
PengyiNote2SlidesPlan
```

不要一开始就追求漂亮图片。

先解决：

```text
内容结构
audience
slide narrative
evidence mapping
```

然后再生成视觉。

这是更稳的推进顺序。

## 对我们的 Project Roadmap

`Paper2Slides` 可以进入我们的 Research OS backlog：

| Module | Description |
|---|---|
| `artifact_planner` | 从 research note 生成 artifact plan |
| `slide_outline_generator` | 生成 slides outline，不直接生成图片 |
| `evidence_mapper` | 每个 claim 对应 source / chart / table |
| `audience_adapter` | PI / PM / public / committee 不同版本 |
| `deck_renderer` | 后续接 reveal.js、Marp、PowerPoint、image model |
| `poster_renderer` | 生成 one-page visual summary |
| `artifact_evaluator` | 检查 factuality、coverage、claim-evidence alignment |

这个方向非常适合我们。

尤其是 quant 场景：

```text
backtest result -> PM pitch deck
factor report -> strategy review deck
research log -> weekly update slides
paper reading -> idea pitch slides
```

这比单纯写文章更接近真实组织协作。

## 系统位置

到现在，HKUDS 这一条 Research OS 主线可以这样排：

| HKUDS Repo | System Position |
|---|---|
| `LightRAG` | 主知识图谱 RAG 层 |
| `RAG-Anything` | 多模态文档入口层 |
| `MiniRAG` | 轻量本地知识层 |
| `AI-Researcher` | 自动科研流程层 |
| `DeepInnovator` | idea 和创新生成层 |
| `DeepResearch-Eval` | research report 评估层 |
| `OpenHarness` | agent harness runtime |
| `UpSkill` | failure-to-skill self-improvement layer |
| `AnyTool` | universal tool-use / capability routing layer |
| `Paper2Slides` | research artifact generation / presentation layer |

如果画成链路：

```text
Documents
  -> RAG-Anything
  -> LightRAG / MiniRAG
  -> AI-Researcher / DeepInnovator
  -> AnyTool / OpenHarness
  -> DeepResearch-Eval
  -> Paper2Slides
  -> Human PM / PI / audience review
```

这条链路非常符合我们想做的：

```text
AI scientist + quant research engineer + open-source research system builder
```

## 一句话总结

`Paper2Slides` 的价值不是“自动做 PPT”。

更准确地说：

```text
Paper2Slides 把科研材料从 knowledge artifact 转换成 communication artifact。
```

它提醒我们：

```text
研究系统必须能读、能想、能做、能评估，也必须能讲。
```

对我们的 `Pengyi Research OS` 和 `Pengyi Quant Research OS` 来说，最后这一步非常关键。

因为真实世界里，资源、合作、offer、RA、PhD、funding、position，很多时候都不是只给“做了事的人”，而是给“能把事情讲清楚、让组织相信并愿意下注的人”。

所以这一篇的核心启发是：

```text
build the research engine,
but also build the presentation engine.
```

## Next

下一篇继续接 `HKUDS020`：

```text
HKUDS020 -> FutureShow
```

`Paper2Slides` 已经进入 slides/poster 生成，`FutureShow` 可以继续看 HKUDS 在演示、展示、交互式表达方向的延展。

如果说：

```text
MiniRAG helps remember.
Paper2Slides helps present.
```

那下一步就要继续看：

```text
future research artifacts can become interactive, dynamic, and product-like.
```
