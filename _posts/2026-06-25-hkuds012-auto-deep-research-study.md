---
title: "HKUDS012: Auto-Deep-Research 作为 Open Deep Research Product 与 AutoAgent Application Layer"
date: 2026-06-25 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds012, hkuds, auto-deep-research, autoagent, deep-research, research-agent, gaia, browser-agent, file-agent, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第十三篇。

```text
HKUDS012 -> Auto-Deep-Research
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
HKUDS010 -> AI-Researcher
HKUDS011 -> DeepInnovator
```

现在来看 `Auto-Deep-Research`。我对它的定位是：

```text
Auto-Deep-Research = Open Deep Research Product + AutoAgent Application Layer
```

它和 `AutoAgent` 的关系非常关键：

```text
AutoAgent          = agent framework / agent factory
Auto-Deep-Research = built-on-AutoAgent 的可直接使用产品
```

也就是说，`Auto-Deep-Research` 不是单独发明一套新的底层 agent 框架，而是把 `AutoAgent` 里最适合 deep research 的部分抽出来，做成一个能一键运行的 personal research assistant。

这对 Pengyi Research OS 很重要。因为我们不只是需要一个会写代码的 agent，也不只是需要一个 RAG 系统，而是需要一个每天可以使用的研究入口：

```text
提出研究问题
-> 自动搜索网页
-> 阅读网页 / PDF / docx / xlsx / pptx
-> 必要时写代码处理材料
-> 汇总证据
-> 形成 research memo
-> 人类 PM 再决定下一步
```

这就是 `Auto-Deep-Research` 在整个 HKUDS map 里的位置。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `Auto-Deep-Research`。

| Item | Value |
|---|---|
| repo | `Auto-Deep-Research` |
| remote | `https://github.com/HKUDS/Auto-Deep-Research.git` |
| branch | `main` |
| local head | `9d671a3` |
| latest local commit date | `2025-10-16` |
| latest local commit | `Update Communication.md` |
| status | clean, synced with `origin/main` after fetch |
| package name | `auto-deep-research` |
| package version | `0.1.3` |
| Python requirement | `>=3.10` |
| console script | `auto = autoagent.cli:cli` |
| main command | `auto deep-research` |
| tracked files by `rg --files` | 71 |

核心目录结构：

```text
autoagent/
  agents/
  environment/
  flow/
  memory/
  repl/
  tools/
  workflows/
loop_utils/
README.md
constant.py
setup.cfg
pyproject.toml
.env.template
```

一句话先行：

```text
Auto-Deep-Research 把 AutoAgent 的三类能力打包成了一个研究产品：
web browsing + file surfing + coding execution。
```

## 它解决什么问题

OpenAI 的 Deep Research 类产品给了一个非常强的产品范式：用户给一个研究问题，系统自动浏览、查证、归纳、输出长报告。

但对于我们做研究工程的人来说，只有闭源产品是不够的。我们还需要回答：

```text
能不能自己部署？
能不能换模型？
能不能接自己的文件？
能不能看执行链路？
能不能把它嵌入自己的 Research OS？
能不能针对 quant / AI scientist / RA application 场景定制？
```

`Auto-Deep-Research` 的价值就在这里。README 对它的定位是一个开源、低成本、可以使用自己 API key 的 Deep Research alternative，并且强调它基于 `AutoAgent` framework。

所以它不是一个单点工具，而是一个研究工作流产品样例。

## 用户看到的产品形态

安装之后，用户的入口非常简单：

```bash
auto deep-research
```

这个命令由 `setup.cfg` 里的 console script 暴露：

```text
auto = autoagent.cli:cli
```

`autoagent/cli.py` 里注册了 `deep-research` command：

```text
@cli.command(name='deep-research')
```

命令参数包括：

| 参数 | 作用 |
|---|---|
| `--container_name` | Docker container name，默认 `deepresearch` |
| `--port` | agent 与 Docker 环境通信端口，默认 `12346` |

启动之后，它会创建：

```text
code_env = DockerEnv(...)
web_env  = BrowserEnv(...)
file_env = RequestsMarkdownBrowser(...)
```

然后进入一个交互式 prompt：

```text
Tell me what you want to do (type "exit" to quit):
```

用户还可以通过 `@` mention 指定 agent，例如让某个 agent 接手当前任务。

它也支持上传文件。CLI 里把 `Upload_files` 作为一个特殊入口，用户选择文件后会复制到 workspace 的 `files` 目录里，让 File Surfer Agent 读取。

这很像一个 terminal-native 的 personal research cockpit。

## 运行链路

整体运行链路可以拆成七步。

### 1. 读取配置

`constant.py` 会读取 `.env` 与环境变量：

| 变量 | 作用 |
|---|---|
| `OPENAI_API_KEY` | OpenAI provider |
| `DEEPSEEK_API_KEY` | DeepSeek provider |
| `ANTHROPIC_API_KEY` | Anthropic provider |
| `GEMINI_API_KEY` | Gemini provider |
| `HUGGINGFACE_API_KEY` | HuggingFace provider |
| `GROQ_API_KEY` | Groq provider |
| `XAI_API_KEY` | xAI / Grok provider |
| `COMPLETION_MODEL` | 主 LLM |
| `API_BASE_URL` | OpenAI-compatible endpoint |
| `FN_CALL` | 是否使用 function calling |
| `DOCKER_WORKPLACE_NAME` | Docker workspace name |
| `BASE_IMAGES` | Docker base image |

它用 `litellm` 做多 provider 适配，这使它不是绑定某一家模型供应商。

这里有一个值得注意的小细节：README 的配置段落里写到默认模型是 `claude-3-5-sonnet-20241022`，但本地 `constant.py` 当前默认值是：

```text
claude-3-5-haiku-20241022
```

这不是大问题，但属于文档和代码的轻微 drift，可以作为一个很小的 PR 切入点。

### 2. 启动 Docker code environment

`DockerEnv` 的作用是给 agent 一个可以执行代码和命令的隔离环境。

`autoagent/environment/docker_env.py` 会：

```text
创建 local workspace
检查或启动 Docker container
把 local workspace mount 到 container
在 container 里启动 tcp_server.py
通过 socket 把命令发给 container 执行
返回 status/result
```

Docker image 默认根据机器架构选择：

```text
x86 / amd64 -> tjbtech1/metachain:amd64_latest
arm         -> tjbtech1/metachain:latest
```

这说明它的 coding agent 不是在宿主机里随便跑命令，而是通过一个 container workspace 执行。

这对研究 agent 很重要，因为 deep research 经常需要：

```text
下载文件
解析文件
写脚本
跑 Python
处理表格
提取 PDF
生成中间结果
```

没有执行环境，deep research 就会停留在“读网页然后总结”的层面。

### 3. 启动 BrowserEnv

`BrowserEnv` 负责真实网页交互。

Web Surfer Agent 可以调用：

| tool | 作用 |
|---|---|
| `web_search` | 用 Bing 搜索 query |
| `visit_url` | 直接打开 URL |
| `click` | 点击页面元素 |
| `input_text` | 输入文本 |
| `page_down` / `page_up` | 页面滚动 |
| `history_back` / `history_forward` | 浏览器历史 |
| `sleep` | 等页面加载 |
| `get_page_markdown` | 把当前页面转成 markdown |

这让 deep research 可以从网页入口开始，而不是只处理用户贴进来的文本。

`get_page_markdown` 这个工具很关键。因为网页的视觉 DOM 对 agent 来说不一定是最好的阅读形态；把页面转成 markdown 后，agent 更容易做精读、摘取和归纳。

### 4. 启动 file browser

`RequestsMarkdownBrowser` 是 File Surfer Agent 的环境。

File Surfer Agent 可以处理：

```text
.html
.htm
.xlsx
.pptx
.wav
.mp3
.flac
.pdf
.docx
其他文本文件
```

工具包括：

| tool | 作用 |
|---|---|
| `open_local_file` | 打开本地文件并转成 markdown/text viewport |
| `page_up_markdown` | 文件视图向上翻页 |
| `page_down_markdown` | 文件视图向下翻页 |
| `find_on_page_ctrl_f` | 在文件视图中搜索 |
| `find_next` | 查找下一个匹配 |
| `visual_question_answering` | 对图片/视频做视觉问答 |

这里的 `visual_question_answering` 不只是图片。代码里也处理视频：抽关键帧、抽音频、用 whisper transcribe，再把 frames + transcription 发给多模态 LLM。

这说明 `Auto-Deep-Research` 已经在向多模态 research assistant 靠近。

### 5. 构建 System Triage Agent

最核心的 agent 分工在：

```text
autoagent/agents/system_agent/system_triage_agent.py
```

它创建三个 sub-agent：

```text
File Surfer Agent
Web Surfer Agent
Coding Agent
```

System Triage Agent 的职责不是自己解决所有问题，而是判断当前子任务应该交给谁：

| agent | 擅长任务 |
|---|---|
| `File Surfer Agent` | 打开、阅读和搜索本地文件 |
| `Web Surfer Agent` | 搜索、浏览、访问网页 |
| `Coding Agent` | 写代码、跑脚本、执行复杂处理 |

它有三个 transfer tool：

```text
transfer_to_filesurfer_agent
transfer_to_websurfer_agent
transfer_to_coding_agent
```

每个 sub-agent 完成阶段性任务后，再用：

```text
transfer_back_to_triage_agent
```

把控制权交回 System Triage Agent。

这就是一个非常清楚的 coordinator-worker 结构。

### 6. 通过 MetaChain runtime 执行

`autoagent/core.py` 里的 `MetaChain` 是实际的 agent runtime。

它做几件事：

```text
构造 system prompt
把 Python function 转成 tool schema
调用 litellm completion / acompletion
解析 tool calls
执行对应 Python function
把 tool result 加回 history
根据 Result.agent 切换 active_agent
循环直到任务完成
```

它对 function-calling 与 non-function-calling 模型都做了适配。

如果模型支持 function calling：

```text
messages + tools -> litellm completion
```

如果模型不支持 function calling：

```text
tools -> textual tool description
model output -> convert_non_fncall_messages_to_fncall_messages
```

这就是它能支持 DeepSeek、Grok、Llama 类模型的原因之一。

这个设计对我们很有启发：未来自己的 Research OS 不能只假设“模型一定支持 OpenAI tool calling”。工程上要给不同模型留适配层。

### 7. 通过 case_resolved 形成终止条件

系统里有两个终止类工具：

```text
case_resolved
case_not_resolved
```

这类工具的意义是把“任务是否完成”变成 agent 可显式调用的动作。

在 research agent 里，这是很重要的。否则 agent 很容易无限搜索，或者过早停止。

一个真正可用的 research workflow 需要：

```text
问题是否回答了
证据是否足够
文件是否读完了
代码是否跑过了
失败原因是什么
下一步是否需要人类 PM 决策
```

`case_resolved` / `case_not_resolved` 是最小版本的任务闭环。

## 三类 Agent 的详细理解

### System Triage Agent

System Triage Agent 是调度层。

它的 prompt 明确要求：

```text
Based on the state of solving user's task,
determine which agent is best suited,
and transfer the conversation to that agent.
```

这类 agent 不应该被设计成“万能大脑”，而应该被设计成“任务路由器”。

它需要知道：

```text
现在缺网页信息 -> Web Surfer
现在缺本地文件信息 -> File Surfer
现在需要执行代码 -> Coding Agent
现在已经足够回答 -> case_resolved
现在确实失败 -> case_not_resolved
```

对我们自己的 R&D Agent 来说，这个 pattern 可以直接复用：

```text
Research PM Agent
-> Literature Agent
-> Data Agent
-> Factor Implementation Agent
-> Backtest Agent
-> Bias Diagnosis Agent
-> Report Agent
```

### Web Surfer Agent

Web Surfer Agent 是公开网络信息入口。

它适合：

```text
搜索项目
查论文页面
查 GitHub README
查 benchmark
查新闻
查博客
查人物 / 实验室 / 公司公开信息
```

它的一个重要约束是：如果网页下载了文件，Web Surfer 不能直接打开下载文件，而是应该回到 Triage，再交给 File Surfer。

这说明系统在 agent capability boundary 上是有意识的：

```text
Web agent browses web.
File agent reads files.
Coding agent executes code.
Triage agent coordinates.
```

这个边界很重要。agent 分工不清楚，后面系统会越来越混乱。

### File Surfer Agent

File Surfer Agent 是资料阅读层。

它不是简单的 `cat file`。它更像一个 text browser：

```text
打开文件
转成 markdown/text
分页
搜索
继续查找
对图片/视频问答
```

这对科研很实用。我们经常需要处理：

```text
论文 PDF
项目文档
简历 / PS / RP
excel 数据
ppt 材料
会议录音
课程视频
截图
```

如果把它接入 Pengyi Research OS，它可以成为：

```text
Research Material Reader
Application Material Reviewer
Quant Report Reader
Project README Reader
PDF-to-Memo Processor
```

### Coding Agent

Coding Agent 是执行层。

它能调用：

| tool | 作用 |
|---|---|
| `gen_code_tree_structure` | 看项目结构 |
| `execute_command` | 执行 shell command |
| `read_file` | 读文件 |
| `create_file` | 新建文件 |
| `write_file` | 写文件 |
| `list_files` | 列目录 |
| `create_directory` | 建目录 |
| `run_python` | 跑 Python 脚本 |
| `terminal_page_up/down/to` | 浏览长终端输出 |

它的 prompt 里有一个很强的工程原则：

```text
Talk is cheap, show me the code with tools.
```

这和我们的 Research OS 方向高度一致。

研究 agent 不能只是“提出想法”。它必须能：

```text
实现
运行
测试
回测
诊断
保存证据
输出报告
```

`Auto-Deep-Research` 这里已经给了一个最小执行层：Docker + terminal tools + Python runner。

## 与前面 HKUDS 项目的关系

把前面几个项目放在一起看，脉络会非常清楚：

| Project | 更像什么 |
|---|---|
| `LightRAG` | knowledge memory / graph retrieval |
| `RAG-Anything` | multimodal document ingestion |
| `Vibe-Trading` | quant workflow and trading-agent style |
| `nanobot` | personal agent shell |
| `CLI-Anything` | natural language to software action |
| `AI-Trader` | live trading platform layer |
| `AgentSpace` | organizational multi-agent workspace |
| `AutoAgent` | agent framework / self-developing agent factory |
| `DeepCode` | paper-to-code / research-to-code implementation |
| `AI-Researcher` | autonomous scientific discovery benchmark/workflow |
| `DeepInnovator` | train idea-generation model |
| `Auto-Deep-Research` | practical deep research assistant product |

我觉得它们可以组成一个很完整的路线：

```text
RAG-Anything / LightRAG
-> 把资料变成可用 knowledge

AutoAgent
-> 提供 agent framework

Auto-Deep-Research
-> 做 daily deep research assistant

DeepCode
-> 把 paper / idea 变成 code

AI-Researcher
-> 跑完整科研发现流程

DeepInnovator
-> 训练更会提出科研 idea 的模型
```

所以 `Auto-Deep-Research` 是一个非常现实的中间层。

它不如 `AI-Researcher` 那么“宏大”，也不像 `DeepInnovator` 那样直接进入 model training，但它更像每天能用的工具。

## 和 AI-Researcher 的区别

这两个项目名字都和 research 相关，但重点不同。

| 维度 | Auto-Deep-Research | AI-Researcher |
|---|---|---|
| 产品形态 | personal deep research assistant | autonomous scientific discovery workflow |
| 输入 | 用户自然语言问题、网页、文件 | benchmark task、reference papers、research problem |
| 核心能力 | browse, read, code, synthesize | idea, survey, plan, implement, judge, analyze, write |
| 目标 | 帮用户完成研究型信息任务 | 评估/推进 autonomous research |
| 工程入口 | `auto deep-research` | benchmark/workflow scripts |
| 更适合我们现在 | 日常学习、项目调研、套磁准备、资料整理 | 未来自动科研系统与顶会 benchmark |

如果用一句话讲：

```text
Auto-Deep-Research = 帮人做 research
AI-Researcher     = 让 agent 自己做 scientific discovery
```

前者更像 daily tool，后者更像 research agenda。

## 和 DeepCode 的区别

`DeepCode` 关注的是从论文、URL、文档、自然语言需求生成代码项目。

`Auto-Deep-Research` 的范围更前置：

```text
先研究清楚问题
再判断是否需要写代码
如果需要，再调用 Coding Agent
```

所以在 Pengyi Research OS 里，可以这样组合：

```text
Auto-Deep-Research -> 调研问题、收集材料、形成 memo
DeepCode           -> 根据 memo / paper / spec 生成实现
```

这就是 research-to-build 的自然路径。

## 和 LightRAG / RAG-Anything 的区别

`LightRAG` 和 `RAG-Anything` 更像 knowledge infrastructure。

`Auto-Deep-Research` 更像 task execution product。

关系可以这样理解：

```text
RAG-Anything = 把复杂资料吃进来
LightRAG     = 把知识组织和检索起来
Auto-Deep-Research = 围绕一个问题主动查、读、写、总结
```

未来如果我们做自己的系统，一个合理结构是：

```text
external web / files
-> RAG-Anything ingestion
-> LightRAG memory
-> Auto-Deep-Research style task agent
-> DeepCode implementation
-> Human PM review
```

## 对 Pengyi Quant Research OS 的启发

我们一直在想：

```text
R&D Agent for Quant Research
= 自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ Human PM 审核
```

`Auto-Deep-Research` 对这里的启发是：在进入 factor implementation 之前，需要有一个 deep research layer。

它可以负责：

```text
读公开论文
读 quant blog
读项目 README
读策略说明
读市场结构介绍
读公司/团队公开资料
读我们自己的 research notes
整理成结构化 memo
提出可验证 hypothesis
```

也就是说，在完整 quant research loop 里：

```text
Deep Research Layer
-> Hypothesis Layer
-> Implementation Layer
-> Backtest Layer
-> Bias Diagnosis Layer
-> Report Layer
-> Human PM Review
```

`Auto-Deep-Research` 对应第一层和一部分第二层。

它不是 backtesting engine，也不是 live trading system。

它更像：

```text
Quant Research Intelligence Desk
```

先把公开信息和本地资料研究透，再进入工程实现。

## 可以直接应用的场景

### 1. 读论文和项目

比如我们可以让它做：

```text
请调研 LightRAG、RAG-Anything、AutoAgent 的区别，
重点关注架构、输入输出、适合接入 Pengyi Research OS 的方式，
最后输出一份 markdown memo。
```

它会需要：

```text
Web Surfer -> 查项目与 README
File Surfer -> 读本地资料
Coding Agent -> 如果需要解析文件或统计代码结构
System Triage -> 汇总并决定是否完成
```

这正是它擅长的任务。

### 2. RA / PhD 导师调研

我们可以让它做：

```text
调研某位老师最近三年的论文、项目、学生、研究方向，
整理出适合我套磁的切入点。
```

这类任务非常适合 Auto-Deep-Research：

```text
公开网页
论文页面
实验室主页
Google Scholar / DBLP / arXiv
GitHub 项目
我们自己的 CV / PS / RP
```

最终输出可以是：

```text
导师研究方向摘要
与我经历的匹配点
可以提的问题
邮件 pitch 角度
潜在风险
下一步行动
```

### 3. Quant project landscape

比如：

```text
调研最近开源 trading agent / quant agent 项目，
比较它们的数据源、回测、执行、agent 架构和可复现性。
```

这可以直接服务我们的网站学习地图，也可以服务未来 Quant Research OS 的项目选型。

### 4. 文件资料整理

如果我们有 PDF、PPT、Excel、docx，它也可以通过 File Surfer Agent 处理。

这对申请材料、研究材料、行业报告都很实用。

需要注意的是：对版权材料和机构内部资料要严格控制边界。能不能上传、能不能处理、能不能放进开源项目，要按数据权限来判断。

### 5. 从研究到代码

如果调研过程中发现需要跑一个脚本，比如：

```text
统计一个 repo 里有哪些 Python 模块
提取 PDF 中的表格
把多个 markdown 合并成 summary
把下载文件转成 clean text
```

它可以转给 Coding Agent。

这就是它比纯搜索工具强的地方。

## 它当前不是做什么的

为了避免误用，需要明确它不是：

```text
不是完整 backtesting engine
不是 live trading execution system
不是数据供应商
不是自动生成 alpha 的闭环系统
不是保证和闭源 Deep Research 同等质量的系统
不是绕过网站权限和付费墙的工具
```

它是一个 research assistant product scaffold。

对我们来说，它的价值在于：

```text
学习 HKUDS 如何把 agent framework 产品化
学习 deep research 的 agent decomposition
学习 web/file/code 三环境如何组合
学习多模型兼容和 function calling fallback
学习如何把 daily research workflow 工程化
```

## 技术细节：function calling fallback

`constant.py` 里有一组模型兼容判断：

```text
NOT_SUPPORT_SENDER = ["mistral", "groq"]
MUST_ADD_USER = ["deepseek-reasoner", "o1-mini", "deepseek-r1"]
NOT_SUPPORT_FN_CALL = ["o1-mini", "deepseek-reasoner", "deepseek-r1", "llama", "grok-2"]
NOT_USE_FN_CALL = ["deepseek-chat"] + NOT_SUPPORT_FN_CALL
```

如果 `FN_CALL` 没有显式设置，它会根据模型名自动判断是否开启 function calling。

在 `MetaChain` 里：

```text
FN_CALL=True:
  use native tools

FN_CALL=False:
  convert tools into text description
  force model to output tool-like actions
  convert text back into tool calls
```

这是 agent 工程中非常重要的一层。

因为真实世界里，我们不可能只服务一种模型：

```text
OpenAI
Anthropic
DeepSeek
Gemini
Groq
HuggingFace
OpenRouter
OpenAI-compatible endpoint
```

如果一个 Research OS 不能切模型，它就很容易被单一供应商的价格、速率、能力边界卡住。

## 技术细节：workspace 与文件边界

CLI 创建的 local root 大致是：

```text
workspace_meta_showcase/showcase_<container_name>/
```

Docker workspace 是：

```text
/<DOCKER_WORKPLACE_NAME>
```

默认：

```text
DOCKER_WORKPLACE_NAME=workplace_meta
```

Web 下载的文件会放到：

```text
/<workspace>/downloads
```

File Surfer Agent 读取文件时，会在 local path 和 docker path 之间转换。

这类路径边界对 agent 系统很关键。否则不同工具会互相找不到文件。

我们未来做自己的 Research OS，也需要明确：

```text
user_uploads/
downloads/
generated_reports/
code_workspace/
backtest_results/
logs/
memory/
```

路径和权限边界不清楚，系统一复杂就会不可维护。

## 技术细节：browser cookie

README 里提到可以导入 browser cookies，让 agent 更好访问某些网站。

这里要谨慎看待。

一方面，这是 practical 的：有些网站登录后才能看到内容。

另一方面，这是安全边界：cookies 本质上是登录凭证，不能随便交给 agent，也不能放进公开 repo。

本地还发现一个小文档 drift：

```text
README: ./metachain/environment/cookie_json/README.md
actual: autoagent/environment/cookie_json/README.md
```

这也是一个很适合作为首次 PR 的小修复。

## 技术细节：GAIA benchmark

README 强调它在 GAIA benchmark 上有不错表现。

这说明项目方把它定位成一种 general assistant / deep research assistant，而不是只做网页总结。

但从我们本地阅读来看，README 没有给出足够完整的最小复现实验说明。

如果要严谨复现，需要知道：

```text
具体 GAIA split
模型版本
temperature / max tokens
是否使用 browser cookies
是否使用 file tools
每题成本
失败样例
evaluation script
random seed / run count
```

这也是未来可以补文档或 issue 的方向。

## 适合我们提 PR 的点

我觉得有几个低风险、高价值切入点。

| PR idea | 价值 |
|---|---|
| 修正 README cookie path | 小而确定，适合第一次贡献 |
| 对齐 README 默认模型与 `constant.py` 默认模型 | 消除文档/代码 drift |
| 增加 quickstart troubleshooting | Docker、port、API key、Playwright/browser 常见问题 |
| 增加 provider matrix smoke test | 说明哪些模型走 function calling，哪些走 non-function fallback |
| 增加一个 public deep research example | 给用户一个可以复现的完整任务 |
| 增加 GAIA reproducibility note | 让 benchmark claim 更可信 |
| 增加 privacy note | cookie、uploaded files、downloaded files 的安全边界 |
| 增加 quant research demo prompt | 对我们自己的方向也有帮助 |

这里最适合马上做的是：

```text
README cookie path fix
README default model clarification
```

原因是它们范围很小，不需要理解全部系统，也不容易破坏功能。

这符合我们之前说的 contributor 路线：

```text
先使用
再发现真实问题
再提 issue / PR
再逐步提高贡献深度
```

## Pengyi Research OS 中的模块位置

如果把它接到我们自己的系统里，我会这样放：

```text
Pengyi Research OS
  01 Knowledge Intake
    - RAG-Anything
    - LightRAG

  02 Deep Research Assistant
    - Auto-Deep-Research style web/file/code agent

  03 Idea Generation
    - DeepInnovator style idea model
    - Human PM review

  04 Implementation
    - DeepCode style paper-to-code
    - factor implementation agent

  05 Experiment
    - backtest
    - ablation
    - robustness

  06 Diagnosis
    - leakage
    - overfit
    - data snooping
    - transaction cost

  07 Report
    - research memo
    - strategy card
    - next-round plan
```

`Auto-Deep-Research` 对应的是第二层：

```text
把世界上的公开信息和本地资料快速研究清楚。
```

没有这一层，后面的因子假设很容易变成拍脑袋。

## 一个可执行的学习计划

我会把 `HKUDS012` 后续拆成几个练习。

### Exercise 1: 本地跑通 quickstart

目标：

```text
安装环境
配置 API key
运行 auto deep-research
用一个公开项目调研问题测试
```

验证标准：

```text
能够启动 CLI
能够进入 prompt
能够搜索网页
能够输出 summary
```

### Exercise 2: 文件阅读任务

目标：

```text
上传一份 PDF 或 docx
让 File Surfer Agent 阅读并提取结构化摘要
```

验证标准：

```text
能正确读取文件
能翻页
能搜索关键字
能输出 evidence-based memo
```

### Exercise 3: web + file + code 联合任务

目标：

```text
调研一个 GitHub project
下载或读取 README / paper
用 Coding Agent 统计代码结构
输出项目学习地图
```

验证标准：

```text
Web Surfer 负责找资料
File Surfer 负责读下载材料
Coding Agent 负责统计项目结构
System Triage 能把结果合并
```

### Exercise 4: Quant research prompt template

目标：

```text
写一个 public quant deep research prompt
让 agent 调研某一类公开策略 / 因子 / 市场结构
输出 hypothesis card
```

验证标准：

```text
不使用私有数据
不抓取违反 ToS 的内容
输出包括假设、证据、风险、可测试路径
```

## 对我们当前阶段的意义

我们现在的核心不是“收藏项目”，而是把这些项目融会贯通成自己的能力。

`Auto-Deep-Research` 给我们的启发很明确：

```text
框架要能产品化。
研究要能日常化。
agent 要能接网页、文件、代码环境。
模型要能切换。
输出要能沉淀为 memo。
人类 PM 要保留最后判断权。
```

这正好对应我们现在的状态：

```text
申请材料
RA / PhD 套磁
quant 机会调研
HKUDS / LLMQuant 项目学习
个人网站输出
未来 Research OS 设计
```

我们可以把它当成 daily research cockpit 的参考实现。

## 最后总结

`HKUDS012` 的一句话总结：

```text
Auto-Deep-Research 是 AutoAgent 的第一个实用级 deep research product layer：
它把 web browsing、file surfing、coding execution 和 multi-agent triage 组合起来，
让用户可以用自己的模型和 API key 运行一个开源 deep research assistant。
```

它对 Pengyi Research OS 的最大价值不是“替代所有研究流程”，而是补上一个非常关键的入口层：

```text
Deep Research before Deep Implementation.
```

先把问题研究清楚，再进入因子假设、代码实现、回测诊断和下一轮研究计划。

这条线非常值得继续深入。
