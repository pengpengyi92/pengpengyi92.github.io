---
title: "HKUDS044: ViMax 作为 Agentic Video Generation、AI Creative Studio 与 Research OS Multimodal Production Layer"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds044, hkuds, vimax, video-generation, multimodal-agent, agent-product, research-os, quant-os, ai-creative-studio]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS044`。

```text
HKUDS044 -> ViMax
```

上一阶段我们做完了：

```text
HKUDS041 Auto-Deep-Research / DeepResearch-Eval
HKUDS042 Agent Product Phase Review
HKUDS043 HKUDS Study Summary
```

现在继续往后走。

按照 `HKUDS043` 里的建议，`HKUDS044` 先看：

```text
ViMax -> Agentic Video Generation
```

一句话定位：

```text
ViMax = Director
      + Screenwriter
      + Producer
      + Video Generator
      + Agent Loop / TUI
      + resumable multimodal production workspace
```

如果说：

```text
HKUDS021 VideoRAG   -> video memory / video knowledge ingestion
HKUDS040 VideoAgent -> video understanding / editing / QA / workflow front-end
```

那么：

```text
HKUDS044 ViMax      -> video generation / AI creative production system
```

也就是从：

```text
理解视频
```

走到：

```text
生产视频
```

这对 Pengyi Research OS 很有意义。

因为我们未来不只是写 blog、proposal、paper note、quant memo。

我们还需要生成：

```text
project demo video
research explainer video
course-style learning clip
technical walkthrough
RA / PhD project presentation
open-source launch video
quant strategy explanation
meeting recap artifact
```

ViMax 给的是一个面向视频产出的 agent product stack。

## Local Snapshot

本次阅读的是本地 HKUDS 工作区里的 `ViMax`。阅读前执行了 `git fetch --all --prune`，发现远端有更新；本地工作区 clean，因此已用 `git pull --ff-only` 快进到最新 `origin/main`。

| Item | Value |
|---|---|
| repo | `ViMax` |
| remote | `https://github.com/HKUDS/ViMax.git` |
| branch | `main` |
| local head | `13d51c7` |
| full commit | `13d51c7b0ad8ffae0b456c5be3ebf1e3088dea75` |
| latest local commit date | `2026-06-30 19:12:53 +0800` |
| latest local commit | `fix tui input cursor navigation` |
| package name in `pyproject.toml` | `autolongvideogeneration` |
| version | `1.1.0` |
| Python requirement | `>=3.12` |
| license | MIT |
| tracked files | 152 |
| Python files | 88 |
| TS / TSX files | 8 |
| JSON files | 39 |
| Markdown files | 5 |
| YAML files | 6 |
| dependency manager | `uv` |
| syntax check | `py -m compileall -q main_agent.py main_idea2video.py main_script2video.py agent_runtime agents pipelines tools interfaces tests` passed |
| unit test attempt | `py -m pytest ...` blocked: local Python has no `pytest` |
| `uv run pytest` attempt | blocked: `uv` not in PATH |

项目结构：

```text
ViMax/
  readme.md
  README_ZH.md
  pyproject.toml
  uv.lock
  LICENSE
  vimax

  main_agent.py
  main_idea2video.py
  main_script2video.py

  agents/
    screenwriter.py
    character_extractor.py
    character_portraits_generator.py
    storyboard_artist.py
    camera_image_generator.py
    reference_image_selector.py
    novel_compressor.py
    event_extractor.py
    scene_extractor.py
    global_information_planner.py

  pipelines/
    idea2video_pipeline.py
    script2video_pipeline.py
    novel2movie_pipeline.py

  agent_runtime/
    loop.py
    tools.py
    tool_executor.py
    session_index.py
    vimax_adapters.py
    context_compactor.py
    prompts.py
    llm.py
    config.py
    models.py

  tools/
    render_backend.py
    image_generator_*.py
    video_generator_*.py
    reranker_bge_silicon_api.py
    image_orientation.py
    image_response.py

  configs/
    agent.example.yaml
    agent.local.yaml
    idea2video.yaml
    script2video.yaml

  ui/
    src/cli.tsx
    src/slashCommands.ts
    src/workspaceMeta.ts

  vimax_benchmark/
    benchmark_index.json
    *_typeA.json
    *_typeB.json
    *_typeC.json

  tests/
    test_agent_loop.py
    test_agent_tools.py
    test_vimax_adapters.py
    test_script2video_pipeline_guards.py
    ...
```

这个项目最新状态非常活跃。

README 的 news 里最近几条是：

```text
2026-06-28 Agent Loop and TUI stability update
2026-06-09 Technical report released
2026-06-08 Agents Loop + TUI workflow integrated
2026-06-07 Novel2Video workflow released
2026-06-01 Google Omni video generator support added
2026-03-23 MiniMax chat model provider support added
```

这说明 ViMax 不只是一个 demo。

它在从 pipeline repo 往：

```text
interactive agent product
```

演进。

## 它解决什么问题

当前 AI video generation 的常见问题：

```text
只能生成几秒短片
角色跨镜头不一致
场景跨镜头不一致
只关注视觉，缺少剧本、分镜、音频、叙事结构
长视频需要大量人工组织 reference image、shot、camera、style
```

ViMax 的回答是：

```text
不要只做 video model wrapper。
要做 video production agent。
```

也就是：

```text
idea
-> story
-> script
-> characters
-> portraits
-> storyboard
-> shot decomposition
-> camera tree
-> reference image selection
-> first / last frames
-> video clips
-> final concatenation
```

这更像一个微型视频制作公司，而不是单个生成模型。

## 四个产品入口

README 里列了四个入口：

| Entry | Meaning |
|---|---|
| `Idea2Video` | 从一个想法扩展成故事、剧本、角色、分镜和视频 |
| `Novel2Video` | 从小说或长文本改编成分集视频内容 |
| `Script2Video` | 从用户提供的剧本直接生成视频 |
| `AutoCameo` | 从照片生成 cameo / guest-star 风格视频 |

这四个入口对应四类创作需求：

```text
我只有一个 idea
我有长文本 / 小说
我已经有剧本
我有一张人物 / 宠物照片，想生成定制视频
```

对 Research OS 来说，我们更关心前三个：

```text
Idea2Video    -> 把项目 idea 变成 explainer video
Script2Video  -> 把 technical script 变成 demo video
Novel2Video   -> 把长课程 / 长文档 / 长报告压缩成分集视频
```

AutoCameo 可以放后面。

它很有产品感，但涉及肖像、同意、隐私、身份和生成内容边界，做公开系统时要谨慎。

## Direct Pipeline

ViMax 目前同时有两套运行形态：

```text
direct pipeline entrypoints
agent loop / TUI entrypoint
```

direct pipeline 是：

```text
main_idea2video.py
main_script2video.py
```

它们是最容易理解系统核心逻辑的入口。

### main_idea2video.py

`main_idea2video.py` 的结构很简单：

```python
pipeline = Idea2VideoPipeline.init_from_config(
    config_path="configs/idea2video.yaml"
)
await pipeline(idea=idea, user_requirement=user_requirement, style=style)
```

也就是：

```text
idea + user_requirement + style
-> Idea2VideoPipeline
```

`configs/idea2video.yaml` 里配置三类 backend：

```text
chat_model
image_generator
video_generator
```

默认示例是：

```text
chat model: google/gemini-2.5-flash-lite-preview-09-2025 via OpenRouter
image generator: ImageGeneratorNanobananaGoogleAPI
video generator: VideoGeneratorVeoGoogleAPI
working_dir: .working_dir/idea2video
```

这说明 direct pipeline 是配置驱动的。

### main_script2video.py

`main_script2video.py` 是：

```python
pipeline = Script2VideoPipeline.init_from_config(
    config_path="configs/script2video.yaml"
)
await pipeline(script=script, user_requirement=user_requirement, style=style)
```

它的输入更明确：

```text
script
user requirement
style
```

适合已经有文稿的情况。

比如我们未来可以写：

```text
HKUDS StudyMap explainer script
Pengyi Research OS intro script
Quant R&D Agent demo script
LightRAG project explanation script
```

然后走 Script2Video。

## Idea2Video Pipeline

`Idea2VideoPipeline` 的链路：

```text
idea
-> develop_story
-> extract_characters
-> generate_character_portraits
-> write_script_based_on_story
-> for each scene:
     Script2VideoPipeline
-> concatenate scene videos
-> final_video.mp4
```

对应代码里的步骤：

```text
Screenwriter.develop_story
CharacterExtractor.extract_characters
CharacterPortraitsGenerator.generate_front / side / back portraits
Screenwriter.write_script_based_on_story
Script2VideoPipeline(...)
concatenate_video_files
```

这个设计很自然。

从创意到视频，最难的不是单次生成，而是中间的结构化过渡：

```text
idea 太抽象
video model 需要具体 prompt
角色需要稳定外观
镜头需要时序
场景需要可拍摄
```

ViMax 把中间层显式拆出来。

## Script2Video Pipeline

`Script2VideoPipeline` 是 ViMax 的核心。

它的规划阶段：

```text
script
-> characters
-> storyboard
-> shot_descriptions
-> camera_tree
```

它的渲染阶段：

```text
characters
-> character portraits
-> storyboard
-> shot descriptions
-> camera tree
-> first / last frame generation
-> video clip generation
-> final video concatenation
```

核心对象包括：

| Object | Meaning |
|---|---|
| `CharacterInScene` | 场景内角色，含 static / dynamic features |
| `ShotBriefDescription` | 初始分镜，含 camera index、visual、audio |
| `ShotDescription` | 细化镜头，含 first frame、last frame、motion、variation type |
| `Camera` | 摄像机位置和 parent camera / parent shot 关系 |
| `ImageOutput` | image generator 输出封装 |
| `VideoOutput` | video generator 输出封装 |

这里的关键是：

```text
视频生成被拆成 frame generation + video clip generation。
```

对于每个 shot，ViMax 会生成：

```text
first_frame.png
last_frame.png
video.mp4
```

如果 variation type 是 `small`，可能只需要 first frame。

如果 variation type 是 `medium` 或 `large`，就需要 first frame 和 last frame。

这种结构比直接让视频模型生成整段更稳。

## Camera Tree

ViMax 很有价值的一个点是 `camera_tree`。

它不是简单按 shot 顺序生成图像，而是先分析 camera 之间的包含关系：

```text
wide shot
-> medium shot
-> close-up
-> reverse shot
```

`CameraImageGenerator.construct_camera_tree` 会让 LLM 判断：

```text
parent_cam_idx
parent_shot_idx
is_parent_fully_covers_child
missing_info
reason
```

这其实是把影视语言变成一个依赖图。

为什么重要？

因为跨镜头一致性需要 reference。

如果一个 close-up 可以从之前的 medium shot 继承场景、构图和角色信息，生成就更稳定。

ViMax 的核心思路是：

```text
用 camera tree 管理跨镜头 reference dependencies。
```

这对 Research OS 也有启发。

很多复杂产物都不是线性生成，而是依赖图生成：

```text
research report sections
code modules
figures
slides
video shots
backtest artifacts
```

都需要：

```text
artifact dependency graph
```

## Reference Image Selection

`ReferenceImageSelector` 是另一个关键组件。

它分两步：

```text
1. text-only filter
   当 reference image 太多时，先用文本描述筛到最多 8 个

2. multimodal selection
   再把图片和文字一起给模型，选择真正用于当前 frame 的 reference images
```

输出是：

```text
reference_image_path_and_text_pairs
text_prompt
```

这解决的是视频生成里的核心问题：

```text
当前镜头应该参考哪些前序图像？
角色参考哪张？
场景参考哪张？
构图参考哪张？
```

这个点非常值得学。

因为大部分多模态系统失败，不是因为模型不会生成，而是因为：

```text
reference context 没有被管理好。
```

ViMax 把 reference selection 显式工程化了。

## Novel2Video

`Novel2MoviePipeline` 顶部还有一句 `TODO: NOT IMPLEMENTED YET`，但实际代码已经实现了大量 planning/render helper。

它的 planning 阶段包括：

```text
save novel text
split novel chunks
compress chunks
aggregate compressed novel
extract events
build FAISS knowledge base
retrieve relevant chunks
rerank chunks
extract scenes
merge event-level characters
merge novel-level characters
```

这条链路更像：

```text
long document adaptation system
```

它不是简单摘要小说。

它做的是：

```text
长文本 -> 事件 -> 场景 -> 角色连续性 -> 可视频化剧本
```

对我们来说，可以类比到：

```text
长课程 -> 分集讲解视频
长技术报告 -> explainer video series
长项目文档 -> demo video sequence
长访谈 transcript -> highlight / storyline video
```

这比纯视频生成更接近 Research OS。

## Agent Loop / TUI

ViMax 现在最值得重视的部分，是它新增的：

```text
Agent Loop + TUI
```

`main_agent.py` 提供 CLI：

```text
--session
--new-session
--jsonl
--once
/compact
```

它不是直接跑 pipeline。

它先构建 runtime：

```text
build_runtime(".")
```

`build_runtime` 组合：

```text
SessionIndex
ViMax adapter specs
ToolRegistry
ToolExecutor
PromptBuilder
OpenAICompatibleLLM
ContextCompactor
AgentLoop
```

这就是 agent product layer。

direct pipeline 是：

```text
函数调用
```

Agent Loop 是：

```text
交互式工作流
```

用户可以：

```text
规划
检查
修改
继续
压缩上下文
恢复 session
触发 render
```

这比一次性生成更像真实产品。

## Runtime Event Stream

`AgentLoop.stream_events` 会输出结构化事件：

```text
turn
status
prompt_trace
tool_start
tool_progress
tool_result
token
done
session
error
```

这对 TUI 很关键。

视频生成是长任务，用户不能只等一个最终结果。

用户需要看到：

```text
正在规划故事
正在提取角色
正在设计分镜
正在构建 camera tree
正在生成 reference prompt
正在生成 frame
正在生成 video clip
正在拼接 final video
```

所以 ViMax 把 progress event 作为一等对象。

这对我们自己的 Research OS 也重要。

研究、回测、写作、视频生成都是长任务。

它们都需要：

```text
status stream
progress metadata
tool result
error recovery
session snapshot
```

## Core Loop Contract

`prompts/agent.md` 里只有几行，但很关键：

```text
Do not claim that planning, rendering, or file edits happened unless a tool result or `.working_dir` state proves it.
Do not claim render has started unless `vimax_render_video` reports that it started or completed.
```

这就是 production agent 的基本纪律。

它解决的是 agent 常见问题：

```text
说自己做了，但其实没做。
说已经开始 render，但没有 tool result。
说文件已经写了，但文件系统没有证据。
```

这个原则应该直接吸收到 Pengyi Research OS：

```text
Do not claim research was completed unless artifact exists.
Do not claim backtest passed unless result file exists.
Do not claim email draft is ready unless draft file exists.
Do not claim report was published unless URL returns 200.
```

这就是 artifact-grounded agent。

## SessionIndex

`SessionIndex` 管理：

```text
.vimax/sessions.json
.vimax/memory.md
.vimax/logs/
.working_dir/
```

每个 session 有：

```text
session_id
working_dir
idea
user_requirement
style
stage
summary
stale
recent_turn_records
compacted_summary
compaction_snapshots
created_at
updated_at
```

它还提供：

```text
artifact_checklist()
append_turn_record()
append_log()
mark_stale()
update_compaction()
memory_read / memory_write
```

这就是我们之前在 `HKUDS042` 里总结的：

```text
workspace
memory
artifact
audit trail
evaluation / stage
```

ViMax 已经把这套东西用于视频生成。

我们可以把同样结构迁移到 Research OS：

```text
.pengyi/
  sessions.json
  memory.md
  logs/

.working_dir/
  hkuds044-vimax/
    sources.jsonl
    notes.md
    report.md
    eval.json
    publish_status.json
```

## Artifact Checklist

`artifact_checklist` 很值得学。

它不是只问“session 完成了吗”，而是检查具体文件：

```text
idea2video/story.txt
idea2video/characters.json
idea2video/script.json
idea2video/scene_*/storyboard.json
idea2video/scene_*/camera_tree.json
idea2video/scene_*/shots/*/shot_description.json
idea2video/final_video.mp4

script2video/characters.json
script2video/storyboard.json
script2video/camera_tree.json
script2video/final_video.mp4

novel2video/novel/novel.txt
novel2video/events/event_*.json
novel2video/scenes/event_*/scene_*.json
novel2video/global_information/characters/*.json
```

这就是：

```text
artifact-driven workflow state
```

它比只存 `status = done` 更可靠。

对 Quant OS，可以对应：

```text
factor_hypothesis.md
data_manifest.json
features.parquet
backtest_config.yaml
backtest_result.csv
risk_report.md
pm_review.md
next_experiment.md
```

对 Research OS，可以对应：

```text
question.md
sources.jsonl
notes.md
report.md
eval.json
published_url.txt
```

## ViMax Tools

ViMax agent runtime 有两类 tool：

### Built-in Tools

包括：

```text
read_file
read_json
write_json
list_files
glob_files
search_text
memory_read
memory_write
todo_read
todo_write
run_shell
```

其中测试明确写了：

```text
run_shell is disabled by default
```

这说明它在安全边界上是克制的。

### ViMax Adapter Tools

核心三个：

```text
vimax_narrative_planning
vimax_novel_planning
vimax_render_video
```

`vimax_narrative_planning`：

```text
idea / script
-> structured text artifacts
```

它不会生成 keyframes、clips、final video。

`vimax_novel_planning`：

```text
novel_text
-> novel2video structured text artifacts
```

它也不直接 render。

`vimax_render_video`：

```text
structured artifacts
-> keyframes
-> clips
-> final video
```

这种拆分非常好。

因为它让系统有一个 human review gate：

```text
先规划
再审核
再渲染
```

视频生成成本高，不能随便一上来就 render。

这对 quant 也一样：

```text
先 thesis
再 review
再跑重 backtest
```

## Config 和 Provider

ViMax 的 `agent.example.yaml` 把 provider 分成：

```text
llm
image
video
embedding
reranker
```

其中 embedding / reranker 只在 novel2video planning 中需要。

同时支持环境变量：

```text
VIMAX_LLM_MODEL
VIMAX_LLM_BASE_URL
VIMAX_LLM_API_KEY
VIMAX_IMAGE_MODEL
VIMAX_IMAGE_BASE_URL
VIMAX_IMAGE_API_KEY
VIMAX_VIDEO_MODEL
VIMAX_VIDEO_BASE_URL
VIMAX_VIDEO_API_KEY
VIMAX_EMBEDDING_MODEL
VIMAX_RERANKER_MODEL
```

direct pipeline 的 `RenderBackend` 也很值得学：

```text
image_generator:
  class_path: tools.ImageGeneratorNanobananaGoogleAPI
  init_args:
    api_key:

video_generator:
  class_path: tools.VideoGeneratorVeoGoogleAPI
  init_args:
    api_key:
```

它通过 `class_path` 动态实例化 generator，并挂上 rate limiter。

这是一种很实用的插件式后端设计：

```text
same pipeline
different image / video provider
```

对 Research OS 也可以学：

```text
same research workflow
different LLM provider
different search provider
different data provider
different backtest engine
```

## Benchmark

`vimax_benchmark/` 里有很多 JSON benchmark case。

文件名里有 type：

```text
typeA
typeB
typeC
```

例子包括：

```text
barista_coffee_cultures_typeA
athlete_training_conditions_typeA
board_game_cafe_interior_typeB
business_partners_office_negotiation_typeC
scientists_lab_collaboration_typeC
teacher_student_tutoring_session_typeC
```

这些 benchmark case 的意义是：

```text
测试不同场景下的角色一致性、空间一致性、多人互动、场景变化和叙事连贯性。
```

这是视频生成系统必须面对的问题。

对 Research OS 来说，也要有 benchmark。

比如：

```text
repo study benchmark
research report benchmark
quant backtest diagnosis benchmark
outreach email benchmark
presentation artifact benchmark
```

没有 benchmark，就很难持续改进。

## 和 VideoRAG / VideoAgent 的区别

| Project | Core |
|---|---|
| `VideoRAG` | 把视频变成可检索知识 |
| `VideoAgent` | 围绕视频执行理解、QA、总结、编辑、remaking |
| `ViMax` | 从 idea / novel / script 生成视频 |

更直白：

```text
VideoRAG: I have videos, help me remember and query them.
VideoAgent: I have videos, help me understand and operate on them.
ViMax: I have an idea/script/novel, help me produce a video.
```

这三者组成一条完整多模态链：

```text
generate video
-> edit / understand video
-> index video into memory
```

或者反过来：

```text
use existing video knowledge
-> generate new video artifact
```

这就是 Research OS 的 multimodal production loop。

## 和 Agent Product 系列的连接

ViMax 继承了 `HKUDS033-042` 的很多思想：

| Agent Product Principle | ViMax 对应 |
|---|---|
| Owner | 用户输入 idea / script / novel，控制 creative direction |
| Role | screenwriter、storyboard artist、producer、video generator 分工 |
| Workspace | `.working_dir/<session>` 保存产物 |
| Tool Schema | `vimax_narrative_planning`、`vimax_render_video` 等工具 |
| Memory | `.vimax/memory.md` |
| Communication | TUI / JSONL event stream |
| Artifact | story、script、characters、storyboard、frames、clips、final_video |
| Evaluation | checklist、tests、guards、stale flags |
| Audit Trail | `.vimax/logs/*.jsonl` |
| Human Review | planning 和 render 分离，用户可先审 planning 再 render |

所以 ViMax 不是孤立的 video repo。

它是一个 agent product example。

## 对 Pengyi Research OS 的启发

ViMax 给我们的启发不是“我们马上要做视频生成公司”。

更重要的是：

```text
复杂多模态产物必须拆成阶段性 artifact。
```

Research OS 也应该这样。

比如写一篇研究报告：

```text
question
-> sources
-> notes
-> outline
-> draft
-> evaluation
-> final post
```

生成一个量化策略：

```text
hypothesis
-> data plan
-> feature engineering
-> backtest
-> bias diagnosis
-> report
-> PM review
```

生成一个项目 demo video：

```text
project idea
-> script
-> storyboard
-> shot plan
-> screen recording / generated frames
-> final video
```

ViMax 的核心架构可以直接复用：

```text
session
artifact checklist
stale flags
progress events
tool results
human review gate
render / publish gate
```

## 对 Quant OS 的启发

ViMax 表面上不是 quant 项目。

但它对 Quant OS 仍有启发：

### 1. Research Artifact Production

Quant research 也需要把结果讲清楚。

可以生成：

```text
factor thesis explainer
backtest result walkthrough
risk diagnosis video
portfolio update video
strategy pitch video
PM review summary
```

这些不是交易核心，但能帮助：

```text
沟通
融资
面试
团队协作
开源展示
研究复盘
```

### 2. Artifact Dependency Graph

Quant 研究也有依赖图：

```text
raw data
-> cleaned data
-> feature
-> signal
-> portfolio
-> backtest
-> risk report
-> PM decision
```

ViMax 的 camera tree / artifact checklist 提醒我们：

```text
不要只存 final result。
要存每一步 artifact 和依赖关系。
```

### 3. Human Review Before Expensive Compute

ViMax 先 planning，后 render。

Quant OS 也应该：

```text
先审 hypothesis / data plan / bias risk
再跑昂贵 backtest / large-scale experiment
```

这能避免浪费时间和算力。

## 风险和限制

ViMax 很有启发，但也有现实限制：

| Risk | Meaning |
|---|---|
| API dependency | LLM、image、video、embedding、reranker 都可能依赖外部 API key |
| Cost | 视频生成成本高，不能无节制 render |
| Quality variance | 图像/视频生成模型仍可能产生错误角色、错误镜头、错误场景 |
| Reproducibility | 同一个 prompt 多次生成可能不同，需保存 seed / provider / response metadata |
| Safety | cameo / 人像 / 成人化描述 / 私人照片都需要严格 consent 和内容边界 |
| Copyright | novel2video 和 reference images 涉及版权，公开系统要用自有或授权内容 |
| Runtime complexity | Python 3.12、uv、外部 provider、TUI、ffmpeg/moviepy 都增加部署复杂度 |
| Test environment | 本地当前缺 `pytest` 和 `uv`，只能做 compileall 语法检查 |

特别注意：

```text
main_idea2video.py 里的示例 prompt 有偏成人化表达。
如果我们把 ViMax 用于 public research portfolio，应该换成专业、安全、公开可展示的 prompt。
```

比如：

```text
Generate a short explainer video about how a personal Research OS turns repo study into public research assets.
```

这更适合我们。

## 可以提 PR 的方向

### 1. README / pyproject polish

`pyproject.toml` 里：

```text
description = "Add your description here"
```

可以改成实际项目描述。

这是很小但清晰的 PR。

### 2. Novel2Movie TODO cleanup

`pipelines/novel2movie_pipeline.py` 顶部还有：

```text
# TODO: NOT IMPLEMENTED YET
```

但文件里已经有 `plan_text_artifacts` 和 `render_video_artifacts` 等实现。

可以确认后改成更准确的注释。

### 3. Camera model duplicate field

`interfaces/camera.py` 里 `parent_shot_idx` 出现了两次。

Pydantic 最终可能覆盖前一个定义，但这是清理点。

可以提一个小 PR 删除重复字段。

### 4. Potential priority shot bug

`script2video_pipeline.py` 里已有 helper：

```python
def _collect_priority_shot_idxs(camera_tree):
    return [camera.parent_shot_idx for camera in camera_tree if camera.parent_shot_idx is not None]
```

但渲染阶段使用的是：

```python
priority_shot_idxs = [camera.parent_cam_idx for camera in camera_tree if camera.parent_cam_idx is not None]
```

这里语义上可能应该用 `parent_shot_idx`，否则 priority shot dependency 可能被 camera index 替代。

这需要进一步跑测试验证，但它是一个很值得看的小 bug / test PR。

### 5. No-API mock demo

给 README 加一个不需要真实 API 的 mock mode：

```text
planning only demo
mock image generator
mock video generator
artifact checklist demo
```

这样新用户可以先理解 workflow，不会一上来卡在 API key 和视频生成成本。

### 6. Public-safe example prompt

把 README / main demo 里示例 prompt 改成更适合开源展示的 safe example。

例如：

```text
A two-minute explainer about how AI agents help researchers turn ideas into reproducible artifacts.
```

这也适合我们自己引用。

### 7. Test docs

当前 README 推荐 `uv sync`，但可以补：

```bash
uv run pytest tests/test_agent_loop.py tests/test_agent_tools.py
```

并说明哪些测试需要真实 API key，哪些不需要。

## 我们自己的最小用法

我们现在不需要立刻跑视频生成。

最适合我们的 MVP 是：

```text
Research Explainer Video Planning
```

也就是只做 planning，不 render。

例如：

```text
Input:
  "Explain Pengyi Research OS: how repo study becomes public research assets."

Output:
  story.txt
  script.json
  characters.json
  storyboard.json
  shot_description.json
  camera_tree.json
```

然后人工审查：

```text
这个 story 是否准确？
这个 script 是否适合公开？
这个 storyboard 是否能表达技术内容？
有没有不安全、不专业、不必要的画面？
```

通过后再考虑：

```text
用真实录屏 / slides / generated frames 做视频。
```

这比直接生成 final video 更稳。

## 在网站里的位置

ViMax 可以成为我们网站里的一个新内容类型：

```text
Study Map Video Scripts
```

比如每个核心系列都可以有：

```text
HKUDS000 study map script
LLMQuant study map script
Pengyi Research OS intro script
Quant R&D Agent demo script
RA / PhD research statement video script
```

不一定马上生成视频。

先生成：

```text
script + storyboard
```

就已经很有价值。

## 和我们当前阶段的连接

我们现在最重要的是：

```text
position
credit
cashflow
contract
RA / PhD / quant opportunity
open-source project influence
```

ViMax 对这些的帮助不是直接“找工作”。

它的帮助是：

```text
把我们的项目讲得更清楚。
```

一个强的 project，不只是代码强。

还要：

```text
README 清楚
demo 清楚
文章清楚
slides 清楚
video 清楚
pitch 清楚
```

ViMax 属于：

```text
public narrative amplification layer
```

这和我们的网站、CV、GitHub、RA 申请、导师沟通是同一条链。

## 最后总结

`HKUDS044 ViMax` 的核心结论：

```text
ViMax 不是简单视频生成工具。
它是一个把 idea / novel / script 转成 video artifact 的 agentic production system。
```

它最值得我们学习的不是某个 provider，而是这套结构：

```text
direct pipeline
agent loop
TUI
session index
artifact checklist
memory
logs
stale flags
progress events
planning/render separation
human review gate
```

对 Pengyi Research OS 来说，它对应：

```text
multimodal production layer
```

对 Quant OS 来说，它对应：

```text
research communication and strategy explainer layer
```

下一步可以继续：

```text
HKUDS045 -> CatchMe
```

因为 ViMax 讲的是多模态 production，CatchMe 讲的是 personalization。

这正好继续补 Research OS 的个人化 agent 层。
