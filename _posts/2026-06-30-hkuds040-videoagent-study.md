---
title: "HKUDS040: VideoAgent 作为 Agentic Video Workflow、Meeting Intelligence 与 Multimodal Production OS"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds040, hkuds, videoagent, video-agent, multimodal-agent, video-workflow, meeting-intelligence, videorag, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS040`。

```text
HKUDS040 -> VideoAgent
```

前面几篇 Agent Product / Workspace 系列已经有了很清楚的产品层次：

```text
HKUDS033 ClawTeam  -> AI organization layer
HKUDS034 ClawWork  -> AI coworker economic accountability layer
HKUDS035 FastAgent -> AI agent execution engine
HKUDS036 Litewrite -> AI research writing workspace
HKUDS037 OpenPhone -> AI phone agent and real-world mobile app interface
HKUDS038 MoChat    -> agent-native communication and networking interface
HKUDS039 UpSkill   -> agent skill growth layer
```

这一篇看：

```text
HKUDS040 VideoAgent -> agentic video workflow and multimodal production layer
```

一句话定位：

```text
VideoAgent = natural language video agent
           + intent analysis
           + graph-powered workflow planning
           + autonomous tool use
           + video understanding / summarization / QA
           + video editing / compilation
           + creative video remaking
```

如果说 `HKUDS021 VideoRAG` 解决的是：

```text
把长视频变成 segment-level、timestamped、multimodal、graph-indexed 的 knowledge object
```

那么 `HKUDS040 VideoAgent` 解决的是：

```text
围绕视频执行任务。
```

也就是：

```text
看视频
理解视频
转写视频
问答视频
总结视频
找片段
按节奏剪辑
生成讲解稿
合成声音
拼接成新视频
把视频素材变成 multimodal artifact
```

这对我们非常现实。
我们一直说喜欢看高质量访谈、课程、讲座、田渊栋访谈、硅谷 101、技术发布会、quant / AI seminar。
这些不应该只是“下饭视频”。
在 Research OS 里，它们应该进入：

```text
video ingestion
video understanding
video note
video evidence
video task
video artifact
```

VideoRAG 给我们 video memory。
VideoAgent 给我们 video workflow。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `VideoAgent`。阅读前已执行 `git fetch --all --prune`，本地 `main` 与 `origin/main` 对齐。

| Item | Value |
|---|---|
| repo | `VideoAgent` |
| remote | `https://github.com/HKUDS/VideoAgent.git` |
| branch | `main` |
| local head | `8afe17c` |
| full commit | `8afe17cd880d1dfddefa701fb56f74cbb679cce9` |
| latest local commit date | `2026-06-23 11:40:28 +0800` |
| latest local commit | `Update readme.md` |
| root license | `MIT` |
| `pyproject.toml` license field | `Apache` text, inconsistent with root license |
| tracked files by `git ls-files` | 851 |
| Python files | 414 |
| tracked `.pyc` files | 86 |
| YAML files | 78 |
| Markdown files | 60 |
| image assets | 58+ |
| audio demo assets | 25 `.wav` |
| local Python syntax check | `py -m compileall -q main.py environment` passed |

项目结构：

```text
VideoAgent/
  readme.md
  readme_zh.md
  demos_documents.md
  main.py
  pyproject.toml
  requirements.txt
  LICENSE

  environment/
    agents/
      base.py
      multi.py
    config/
      config.yml
      graph.txt
      intents.yml
      registry.json
      llm.py
    roles/
      audio_extractor.py
      transcriber.py
      mixer.py
      merge.py
      vid_preloader.py
      vid_searcher.py
      vid_editor.py
      vid_conversion.py
      voice_generator.py
      vid_qa/
      vid_summ/
      vid_comm/
      vid_news/
      vid_rhythm/
      tts/
      svc/
      stand_up/
      cross_talk/

  tools/
    videorag/
    ImageBind/
    audio-preprocess/
    CosyVoice/
    fish-speech/
    seed-vc/
    DiffSinger/

  dataset/
    presentation_style/
    voice/

  assets/
    framework.jpg
    overview.png
    demo covers and evaluation images
```

这是一个重资产多模态仓库。
它依赖的不是一个 LLM API，而是：

```text
Claude / GPT / Gemini / DeepSeek
Whisper
ImageBind
CosyVoice
Fish Speech
Seed-VC
DiffSinger
VideoRAG
MoviePy / Librosa / Demucs / ONNXRuntime / Torch
```

所以它不是轻量 demo。
它更像一个 early-stage multimodal production OS。

## 项目用途

README 对 VideoAgent 的定位是：

```text
Comprehensive Video Intelligence:
An All-in-One Framework for Understanding, Editing, and Generation
```

核心能力有三条：

| Capability | Meaning |
|---|---|
| Video Understanding | 分析、转写、摘要、问答、insight extraction |
| Video Editing | 片段拼接、节奏剪辑、故事化组装、音乐同步 |
| Video Remaking | meme、音乐视频、跨文化喜剧、声音克隆、内容重制 |

它对标的不是单一工具，而是把：

```text
Director
Funclip
NarratoAI
NotebookLM
VideoRAG
TTS / SVC / video editing tools
```

这些能力揉进一个自然语言 video agent。

所以 VideoAgent 的产品野心是：

```text
用户只说需求，系统自动分析意图、选择工具、规划 workflow、执行多模态生产。
```

## 三个创新点

README 里明确写了三个关键创新点：

```text
Intent Analysis
Autonomous Tool Use & Planning
Multi-Modal Understanding
```

这三点可以转成更工程化的解释：

```text
Intent Analysis:
  把用户自然语言拆成 explicit / implicit sub-intents。

Autonomous Tool Use & Planning:
  用 agent graph 把任务转成可执行工具链。

Multi-Modal Understanding:
  把原始视频、音频、字幕、视觉片段转成可检索、可剪辑、可生成的中间对象。
```

这和我们前面看的 FastAgent / AnyTool / MoChat / UpSkill 都能接起来。

VideoAgent 不是“一个视频模型”。
它是：

```text
多角色工具注册表
intent-to-tool mapping
LLM graph router
multi-agent execution engine
video/audio/model toolchain
```

## 主入口

`main.py` 很简单：

```python
from environment.agents.multi import MultiAgent

def main():
    print_banner()
    print_welcome_message()
    multi_agent = MultiAgent()
    multi_agent.run()
```

也就是说系统入口是 `MultiAgent`。

用户运行：

```bash
python main.py
```

然后输入：

```text
User Requirement: ...
```

接下来就进入：

```text
requirement
-> intent analysis
-> tool selection
-> agent graph generation
-> graph judgment / reflection
-> agent chain execution
```

这说明 VideoAgent 是 conversational workflow engine，不是固定脚本集合。

## MultiAgent 执行链

`environment/agents/multi.py` 是系统大脑。

它做几件事：

```text
1. FunctionRegistry.auto_register("environment/roles")
2. 读取 intents.yml
3. 用 Claude 做 intent analysis
4. 根据 intent 找对应 tools
5. 用 graph.txt prompt 生成 Agent Graph / Agent Chain / User Input Graph
6. 用 judge_agent_graph 做校验
7. 如果失败，反思并重生成，最多 3 轮
8. 按 agent_chain 执行每个 role
9. 把上游 output 传给下游 input
```

整体像这样：

```text
User Requirement
  |
  v
Intent Analyst
  |
  v
intents.yml
  |
  v
tool candidates from registry
  |
  v
Agent Graph Designer
  |
  v
Agent Graph + Agent Chain + User Input Graph
  |
  v
Graph Judge + Reflection
  |
  v
Tool Execution Chain
```

这里最有价值的地方是：

```text
它没有把 workflow 写死，而是让 LLM 根据 tool metadata 生成 agent graph。
```

这和我们自己的 Research OS 很像。
未来我们也不应该把所有 workflow 写成固定脚本。
更好的方式是：

```text
注册 research tools
注册 data tools
注册 backtest tools
注册 writing tools
注册 communication tools
让 planner 根据任务自动生成 workflow
再用 evaluator 验证 workflow 合理性
```

VideoAgent 给了一个视频领域版本。

## Tool Registry

`environment/agents/base.py` 定义了 `BaseTool` 和 `FunctionRegistry`。

每个 role 都继承：

```python
class SomeRole(BaseTool):
    class InputSchema(BaseTool.BaseInputSchema):
        ...

    class OutputSchema(BaseModel):
        ...

    def execute(self, **kwargs):
        ...
```

`FunctionRegistry.auto_register()` 会扫描 `environment/roles` 下的 Python 文件，自动导入所有 `BaseTool` 子类，并抽取：

```text
name
description
input_params
output_params
```

这就是 agent graph planner 的工具元数据。

这点非常值得吸收：

```text
工具能不能被 agent 正确规划，取决于工具 schema 是否清楚。
```

如果工具 description、input、output 写得不清楚，LLM 生成的 Agent Graph 就会乱。

## Intents

`environment/config/intents.yml` 把用户意图映射到 role 列表。

例子：

```yaml
Video Edit:
  - AudioExtractor
  - Merge
  - VoiceGenerator
  - VideoPreloader
  - VideoSearcher
  - VideoEditor

Video QA:
  - VideoContentQA

Audio Overview:
  - VideoSummarizationGenerator

Rhythm-cut:
  - RhythmDetector
  - RhythmContentGenerator

Commentary:
  - CommentaryContentGenerator
  - VoiceGenerator

News:
  - NewsContentGenerator
  - VoiceGenerator
```

这相当于先做一个 coarse routing：

```text
需求属于哪类？
这类需求可能用哪些工具？
```

然后再让 graph planner 在候选工具里规划执行顺序。

对 Research OS 来说，我们也需要类似 intent map：

```text
Paper Reading:
  - PDFParser
  - ClaimExtractor
  - ExperimentExtractor
  - ResearchMemoWriter

Factor Backtest:
  - DataLoader
  - FeatureBuilder
  - LeakageChecker
  - BacktestRunner
  - ReportWriter

RA Outreach:
  - ProfileMatcher
  - EmailDraftWriter
  - CVSelector
  - FollowupPlanner
```

这个映射会让 agent workflow 更稳定。

## Graph Prompt

`environment/config/graph.txt` 要求 LLM 输出严格 JSON：

```json
{
  "Feasibility": "Feasible",
  "Agent Graph": ...,
  "Agent Chain": ...,
  "User Input Graph": ...,
  "Reasoning": ...
}
```

它要求每个 Agent Node 写清：

```text
node
inputs
outputs
links
```

还特别强调：

```text
outputs.links 必须指向下一个 Agent 实际存在的 input parameter。
output description 和下游 input 要匹配。
没有入边的参数统一视为 user input。
```

这个 prompt 很工程化。
因为 multi-agent workflow 最容易错的就是：

```text
参数名对不上
文件路径传成目录
上游没产出下游需要的字段
agent chain 顺序不合理
重复调用冗余工具
```

VideoAgent 显式做了 graph judge 和 reflection，就是为了解决这些 workflow 结构错误。

## LLM 分工

`environment/config/llm.py` 用 OpenAI-compatible client 包装多个模型前缀：

```text
deepseek()
claude()
gemini()
gpt()
```

README 里也明确了不同 LLM 的用途：

| Model Route | Used For |
|---|---|
| Claude | Agentic Graph Router / TTS / SVC / Stand-up / CrossTalk |
| GPT | Video Editing / Overview / Summarization / QA / Commentary |
| Gemini | MLLM caption and fine-grained video understanding |
| DeepSeek | Video remixing / TTS / SVC / Stand-up / CrossTalk |

这也很现实。
多模态工作流里不一定一个模型包打天下。

更稳的方式是：

```text
planner 用强 reasoning 模型
vision selection 用 multimodal 模型
text writing 用便宜稳定模型
audio generation 用专门模型
editing 用 deterministic library
```

对 Quant OS 也一样：

```text
research planner
data checker
coding agent
math/stat reviewer
report writer
risk reviewer
```

应该分模型、分工具、分责任。

## 代表性 Role

VideoAgent 的 role 很多，这里抓几个核心。

### VideoPreloader

`VideoPreloader` 做视频预处理。

输入：

```text
video_dir
```

它会：

```text
创建 dataset/video_edit/*
扫描 source mp4
加载 VideoRAG
VideoRAG.insert_video(video_path_list=...)
把素材写入 videosource-workdir
```

也就是说，VideoAgent 直接复用了 VideoRAG 的视频索引能力。

这就把 HKUDS021 和 HKUDS040 串起来了：

```text
VideoRAG = video memory backend
VideoAgent = video workflow frontend
```

### VideoSearcher

`VideoSearcher` 读取：

```text
video_scene_path
```

然后从 `video_scene.json` 里拿：

```text
segment_scene
```

用 VideoRAG query：

```python
QueryParam(mode="videoragcontent")
videoragcontent.query(query=query, param=param)
```

这说明它不是按文件名硬剪视频，而是根据 storyboard / scene semantics 去找相匹配的视觉片段。

这是 video agent 的关键：

```text
text idea -> scene semantics -> video retrieval -> clip candidates
```

### VideoEditor

`VideoEditor` 做真实剪辑。

它读取：

```text
video_segments
kv_store_video_segments.json
cut_points / timestamp_path
storyboard_file
audio_path
```

然后：

```text
根据 beat timestamps 划分 time periods
读取 storyboard sections
从候选 segment 中抽帧
用 Gemini 分析 frames，选择最匹配 scene description 的起始帧
用 MoviePy subclip
concat clips
加背景音乐或混合原音
输出 dataset/final.mp4
```

这是很完整的视频生产链：

```text
semantic retrieval
visual frame selection
duration alignment
audio-video composition
final rendering
```

对我们来说，核心启发不是 MoviePy，而是：

```text
LLM / MLLM 做语义判断，传统工具做确定性执行。
```

### VideoSummarizationGenerator

`VideoSummarizationGenerator` 支持：

```text
video_dir
present_style_path
output_path
user_idea
```

它会：

```text
加载 Whisper large-v3-turbo
转写单个视频或视频目录
也支持直接读取 transcript txt
根据 presentation style 生成总结稿
保存到 output_path
```

这很适合我们的访谈、课程、讲座学习。

未来我们可以把：

```text
硅谷 101 访谈
田渊栋访谈
AI seminar
quant lecture
导师组公开 talk
论文作者 presentation
```

变成：

```text
transcript
structured summary
research worldview notes
follow-up questions
website blog source
```

### VideoContentQA

`VideoContentQA` 做视频问答。

它会：

```text
扫描目录中的视频文件
用 Whisper 转写全部视频
合并 transcript
进入 interactive Q&A session
只允许基于 transcript 回答
保存 QA history
```

它的 QA prompt 很重要：

```text
Only use information from the transcripts.
If answer cannot be found, say not enough information.
Mention source video file when relevant.
Do not make up information.
```

这就是 evidence-grounded video QA。

对 Research OS 来说，视频问答必须有这种边界。
否则 agent 很容易把视频没说过的内容脑补出来。

### RhythmDetector / RhythmContentGenerator

`RhythmDetector` 用 Librosa 分析音频节奏：

```text
RMS energy
peak detection
timestamp output
rhythm_detection.png
rhythm_distribution.png
cut_points.json
```

`RhythmContentGenerator` 再根据节奏点和用户 idea 生成 storyboard。

这条线用于 rhythm-cut music video。

但它对我们也有启发：

```text
视频不是只有语义，还有节奏。
```

讲座、访谈、会议也有节奏：

```text
开场
问题提出
核心观点
案例
反驳
总结
call to action
```

未来我们做 video learning agent，也可以做：

```text
topic rhythm detection
argument transition detection
highlight moment detection
```

## 和 VideoRAG 的区别

`HKUDS021 VideoRAG` 和 `HKUDS040 VideoAgent` 很容易混，但它们的职责不同。

| Project | Core Role | Main Output |
|---|---|---|
| VideoRAG | 视频知识入口 / 多模态索引 | segment、caption、transcript、embedding、graph、evidence |
| VideoAgent | 视频任务执行 / 多模态工作流 | summary、QA、storyboard、edited video、remade video |

可以这样理解：

```text
VideoRAG asks:
  How do we store and retrieve video knowledge?

VideoAgent asks:
  How do we do useful work with videos?
```

两者组合才完整：

```text
VideoRAG
  -> ingest video into memory

VideoAgent
  -> plan and execute workflows around video memory
```

这对 Pengyi Research OS 很关键。
我们不只是要能“问视频”，还要能：

```text
从视频生成研究笔记
从视频生成问题清单
从视频定位关键片段
从视频提炼导师/科学家 worldview
从视频生成 blog / lecture notes
从视频触发后续 paper reading
```

## 和 MoChat / UpSkill 的连接

VideoAgent 接到 MoChat 后，可以形成：

```text
video discussion workflow
```

例如：

```text
MoChat panel 里有人分享一场 AI seminar 视频
VideoAgent 自动转写并摘要
Research Agent 提取关键 claim
Paper Agent 找相关论文
Human PM 在 DM 里审核是否值得深入
Litewrite 生成 blog / memo
UpSkill 沉淀本次视频学习流程
```

这就是完整闭环：

```text
MoChat -> capture opportunity
VideoAgent -> process video
VideoRAG -> store evidence
Litewrite -> produce artifact
UpSkill -> distill workflow
```

这比单纯“看视频”强很多。

## 对 Pengyi Research OS 的启发

我们可以把视频能力放进 Research OS：

```text
Video Layer
  ingestion:
    VideoRAG
    transcript
    segment metadata
    evidence timestamp

  understanding:
    summary
    QA
    key claims
    speaker worldview
    research directions

  workflow:
    topic extraction
    follow-up paper list
    code / repo links
    study map update
    blog draft

  production:
    short explanation video
    interview summary card
    lecture note
    public website post
```

这样我们看的视频就不会散掉。

比如田渊栋访谈可以变成：

```text
1. transcribe
2. segment by topic
3. extract worldview:
   - research taste
   - AI scientist intuition
   - long-term career view
   - system-building principles
4. connect to papers / repos
5. write personal learning note
6. create follow-up skill:
   "how to learn from high-quality AI researcher interview"
```

这才是真正的学习系统。

## 对 Quant OS 的启发

量化领域也有很多视频输入：

```text
macro strategist interviews
FOMC press conference
earnings call video / audio
quant seminar
PM roundtable
trading conference
lectures on market microstructure
fund manager interviews
```

VideoAgent 可以做：

```text
transcribe
summarize
extract market thesis
extract risk scenarios
extract time horizon
extract evidence
tag assets / sectors / macro variables
generate follow-up backtest ideas
```

例如：

```text
Video: macro interview
Agent output:
  thesis: liquidity tightening affects small-cap growth
  horizon: 3-6 months
  assets: Russell 2000, NASDAQ, USD liquidity proxies
  evidence: timestamped claims
  research task: backtest small-cap/growth factor under liquidity regimes
```

这就把视频变成 quant research input。

## 和我们当前生活的连接

我们之前说，很多高质量访谈是“下饭视频”。
这句话背后其实有一个很重要的问题：

```text
高质量输入如果只是情绪激励，很快就消散。
高质量输入如果进入系统，就能变成长期资产。
```

VideoAgent / VideoRAG 可以帮我们把：

```text
喜欢看
爱看
反复看
大受裨益
```

变成：

```text
可引用的笔记
可复习的片段
可检索的观点
可行动的研究计划
可公开的 blog
可沉淀的 skill
```

这对我们冲 AI Scientist 很重要。
顶会 paper、开源 project、career strategy、quant intuition，很多都是从高质量输入中长出来的。

## 工程上值得学习的点

VideoAgent 里值得吸收的工程点：

| Point | Why It Matters |
|---|---|
| `BaseTool` + `InputSchema` / `OutputSchema` | 工具可被 LLM planner 读取和组合 |
| `FunctionRegistry.auto_register` | 新增 role 后自动进入工具库 |
| `intents.yml` | 先 coarse route，降低 planner 搜索空间 |
| `graph.txt` strict JSON | 让 workflow 结构可解析 |
| `judge_agent_graph` | 对 agent graph 做二次验证 |
| reflection loop | 失败后重规划，而不是直接执行错链 |
| VideoRAG integration | 把视频索引作为 workflow 中间层 |
| Whisper transcription | 视频知识先落成 text evidence |
| Gemini frame selection | 视觉匹配交给 MLLM |
| MoviePy deterministic execution | 最后剪辑交给确定性工具 |
| presentation style files | 输出格式可配置 |

这些都可以迁移到 Research OS。

## 风险和限制

VideoAgent 当前也有明显工程风险。

第一，依赖很重。

```text
torch
onnxruntime-gpu
Whisper
ImageBind
CosyVoice
Fish Speech
Seed-VC
DiffSinger
GPU / CUDA
ffmpeg
```

这意味着本地部署成本高。
对我们来说，第一阶段不适合完整部署。

第二，仓库里有不少 tracked `.pyc` / `__pycache__` 文件。
这会增加仓库体积，也不利于跨 Python 版本。

第三，部分 metadata 不一致。
根目录 `LICENSE` 是 MIT，但 `pyproject.toml` 写 `license = {text = "Apache"}`。

第四，`registry.json` 和实际 role 目录需要持续保持一致。
如果 registry 指向不存在模块，MultiAgent 动态加载会失败。

第五，workflow graph 强依赖 LLM JSON 输出稳定性。
虽然系统有 regex JSON extraction 和 judge/reflection，但仍然需要更强 schema validation。

这些都是可提 issue / PR 的地方。

## 可以提 PR 的方向

1. 清理 tracked pycache

   仓库里有 86 个 `.pyc` tracked 文件，可以建议移除并更新 `.gitignore`。

2. 统一许可证元数据

   根 `LICENSE` 是 MIT，`pyproject.toml` 是 Apache。可以开 issue 确认真实 license，然后同步。

3. registry consistency test

   写一个测试：

   ```text
   load registry.json
   import every module
   assert class exists
   assert subclass BaseTool
   assert InputSchema / OutputSchema parseable
   ```

4. graph JSON schema validation

   为 `Agent Graph` / `Agent Chain` / `User Input Graph` 加 Pydantic schema，而不是只靠手写 key check。

5. lightweight video summary mode

   给用户一个只依赖 transcript + GPT 的轻量模式，不强制下载全部 CosyVoice / SVC / DiffSinger 模型。

6. Research OS demo

   加一个 demo：

   ```text
   ingest an AI research interview
   produce structured notes
   extract paper/repo follow-ups
   export markdown
   ```

   这会非常适合我们的网站和学习流。

## 我们自己的最小可行版本

我们现在不需要全量部署 VideoAgent。
更务实的路线：

```text
Phase 1: transcript-first
  yt-dlp / local video
  Whisper or existing transcript
  summary
  topic segmentation
  timestamp notes

Phase 2: evidence memory
  VideoRAG / simple vector index
  timestamped evidence
  query by topic

Phase 3: research workflow
  extract worldview
  extract research questions
  generate follow-up paper list
  write blog draft

Phase 4: production
  short video note
  highlight clip list
  slide / article / memo generation
```

这就是 Pengyi Video Research OS v0。

它服务的不是做短视频娱乐，而是：

```text
把高质量视频输入转成 AI scientist 的研究资产。
```

## 小结

`HKUDS040 VideoAgent` 在当前学习地图里的位置是：

```text
Video / Meeting Agent Layer
```

它和前面几篇的关系：

```text
VideoRAG -> video memory
VideoAgent -> video workflow
MoChat -> video opportunity and discussion channel
Litewrite -> video notes and reports output
UpSkill -> video learning workflow becomes skill
```

对我们来说，最重要的一句话是：

```text
不要只是看视频，要让视频进入 Research OS。
```

访谈、课程、讲座、会议、发布会、quant seminar、AI researcher interview，都应该变成：

```text
timestamped evidence
structured notes
follow-up tasks
research worldview
public blog
private strategy memo
reusable skill
```

下一篇可以进入：

```text
HKUDS041 -> Auto-Deep-Research / DeepResearch-Eval Revisited
```

也就是把 agent product 系列最后接回 AI scientist 的 deep research loop。
