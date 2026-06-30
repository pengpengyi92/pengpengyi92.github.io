---
title: "HKUDS037: OpenPhone 作为 AI Phone Agent、现实 App 操作入口与 Mobile Research Agent"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds037, hkuds, openphone, phone-agent, mobile-agent, ios-agent, ai-phone, agent-product, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS037`。

```text
HKUDS037 -> OpenPhone
```

前面四篇 Agent Product / Workspace 系列已经形成一个清晰链条：

```text
HKUDS033 ClawTeam  -> AI organization layer
HKUDS034 ClawWork  -> AI coworker economic accountability layer
HKUDS035 FastAgent -> AI agent execution engine
HKUDS036 Litewrite -> AI research writing workspace
```

这一篇看：

```text
HKUDS037 OpenPhone -> AI phone agent and real-world mobile app interface
```

一句话定位：

```text
OpenPhone = OpenPhone-3B mobile agentic foundation model
          + iOS / Android mobile agent evaluation playground
          + OpenPhone CLI for external AI agents
          + PhoneClaw autonomous iOS Ralph Loop
          + two-layer self-learning memory
          + device-cloud collaboration framework
```

它的意义非常现实。

很多 agent 项目还停留在：

```text
浏览器
代码仓库
文件系统
terminal
网页 API
```

但真实世界里，人和组织大量信息流、关系流、业务流都在手机 app 里：

```text
微信
飞书
钉钉
邮件
日历
地图
外卖
打车
支付
电商
CRM / OA
银行 app
交易 app
```

OpenPhone 把 agent 的手伸进这个现实入口：

```text
看手机屏幕
理解 GUI
点击、输入、滑动、打开 app
执行多步任务
失败后自我修复
从历史经验里学习
把手机任务变成可评测、可训练、可复现的 agent problem
```

这对 Pengyi Research OS / Quant OS 的启发是：

```text
Research OS 不能只连接论文、代码、数据和网页。
它最终要连接真实业务界面、真实沟通界面、真实组织界面。

手机 agent 是 agent 进入现实业务现场的一个关键入口。
```

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `OpenPhone`。阅读前已执行 `git fetch --all --prune` 和 `git pull --ff-only`，本地已快进到远端 `main` 最新状态。

| Item | Value |
|---|---|
| repo | `OpenPhone` |
| remote | `https://github.com/HKUDS/OpenPhone.git` |
| branch | `main` |
| local head | `4b8f810` |
| full commit | `4b8f8106e863eaca11f3d4e89682c0c4219e5e7a` |
| latest local commit date | `2026-06-29 16:23:40 +0800` |
| latest local commit | `Update README.md` |
| license | `MIT` |
| tracked files by `git ls-files` | 354 |
| Python files | 132 |
| Markdown files | 14 |
| JSON files | 16 |
| YAML files | 22 |
| MP3 demo assets | 9 |
| tracked `__pycache__` / `.pyc` files | 104 |
| local Python syntax check | 132 Python source files parsed with Python 3.11, 0 errors |

项目主结构：

```text
OpenPhone/
  README.md
  requirements.txt

  cli/                  unified OpenPhone CLI
    main.py
    commands/
      device.py         snapshot / tap / type / swipe / open / wait / keyboard
      run.py            run / daemon / learn
      memory.py         memory show / list / query

  PhoneClaw/            autonomous iOS agent
    loop.py             Ralph Loop
    planner.py          task decomposition
    executor.py         iOS action execution
    evaluator.py        screenshot-based pass/fail evaluation
    memory.py           UserMemory
    experience.py       ExperienceLog
    learn.py            human demo recorder
    actions.py          WDA iOS action primitives

  ios_agent/            iOS automation framework and mail RAG pipeline
  evaluation/           AndroidLab-style benchmark and task evaluation
  prepare_data/         long reasoning / reflection data generation
  model_training/       SFT and GRPO training scripts
  page_executor/        mobile page execution variants
  skills/openphone/     AI agent skill definition for using OpenPhone CLI
  configs/              model / environment configs
  test_script/          batch benchmark scripts
  vllm_script/          vLLM serving script
```

依赖很偏 mobile agent / VLM / evaluation：

```text
requests
Pillow
opencv-python
openai
zhipuai
backoff
lxml
pyyaml
docker
datasets
pandas
openpyxl
fuzzywuzzy
Levenshtein
```

## 项目用途

OpenPhone 的 README 标题是：

```text
Mobile Agentic Foundation Models for AI Phone
```

它解决的问题是：

```text
大多数 AI phone agents 依赖云端大模型。
这带来隐私、延迟、成本和部署约束。
```

OpenPhone 的路线是：

```text
训练一个 3B 级别、面向手机 GUI 任务的 agentic vision-language model。
把它放进手机 agent 任务循环。
再用 device-cloud collaboration 弥补小模型能力边界。
```

这不是单点工具，而是四层系统：

| Layer | Purpose |
|---|---|
| OpenPhone-3B | 面向移动 GUI 的轻量 agentic VLM |
| Evaluation Playground | AndroidLab + 扩展 app 任务，用于评测手机 agent |
| OpenPhone CLI | 给外部 AI agent 和人类统一控制 iOS 设备 |
| PhoneClaw | 内置 VLM 的自主 iOS agent，执行 Ralph Loop 和长期记忆 |

从项目形态看，它同时是：

```text
model project
agent framework
benchmark playground
CLI product
iOS automation tool
self-learning mobile assistant
```

## 为什么是 3B

README 里强调 3B 的原因很直接：

```text
手机 agent 最终要考虑端侧部署。
模型不能只追求越大越好。
```

3B 的价值在于：

```text
能在 consumer GPU / mobile NPU 预算里运行
比 7B / 9B 更快
持续云端调用成本更低
隐私数据不必每一步都上传
在 GUI task 上经过训练后仍然有可用性能
```

README 给出的 speed comparison 里：

| Model | GPU | Size | SR | Time Cost / Step |
|---|---|---:|---:|---:|
| Qwen2.5-VL-7B-Instruct | Single 3090 | 7B | 10.1 | 6289.15 ms |
| OpenPhone | Single 3090 | 3B | 15.2 | 4170.63 ms |
| GLM-4.1V-9B-Thinking | Two 3090s | 9B | 24.6 | 14584.89 ms |
| Qwen2.5-VL-7B-Instruct | Two 3090s | 7B | 10.1 | 4587.79 ms |
| OpenPhone | Two 3090s | 3B | 15.2 | 3524.25 ms |

这个表说明一个工程事实：

```text
最大模型可能有更高最终成功率，
但小模型在真实设备交互里有速度、成本和端侧部署优势。
```

对 Research OS / Quant OS 来说，也有类似规律：

```text
不是所有步骤都要用最强云端模型。
大量 routine action、UI 操作、状态检查、格式化、抽取、简单判断，可以交给轻量模型。
复杂 reasoning、关键决策、最终审核，再调用强模型或人类 PM。
```

这就是 model routing / device-cloud collaboration 的工程基础。

## Device-Cloud Collaboration

OpenPhone 的 device-cloud collaboration 逻辑是：

```text
端侧小模型负责大量低成本、低延迟步骤。
云端强模型在复杂任务、失败恢复、关键 reasoning 时介入。
```

README 提到它能减少大约 10% 的 cloud API calls，同时尽量保持性能。

这不是一个巨大的数字，但方向重要：

```text
手机 agent 每一步都调用云模型，成本和延迟会放大。
能减少 10% 云调用，意味着真实长期使用成本下降。
```

对我们有两个启发：

第一，agent 系统要有 routing：

```text
small local model
cloud model
domain expert model
tool call
human approval
```

第二，routing 不是只看模型价格：

```text
还要看任务风险、隐私、延迟、错误代价、是否需要审计。
```

在 Quant Research OS 中，这可以对应：

```text
本地小模型 -> 日常报告整理、文件分类、简单摘要
云端强模型 -> 因子假设、复杂诊断、策略解释
专用工具 -> 回测、数据校验、风险计算
人类 PM -> 是否进入下一轮研究、是否进入实盘候选
```

## OpenPhone CLI

OpenPhone 最新 README 把 CLI 放在很靠前的位置。

这是我认为最有现实产品价值的一层。

CLI 入口是：

```bash
python -m cli.main
```

它提供两种模式：

```text
Agent-driven mode -> 外部 AI agent 一步一步控制设备
Autonomous mode   -> OpenPhone 内置 VLM 端到端执行任务
```

### Agent-Driven Mode

给外部 AI agent 用的原子命令包括：

```text
snapshot
tap
type
swipe
press
open
wait
keyboard
```

典型 workflow：

```text
open <app>
wait 1.0
snapshot --json
analyze elements and screenshot
tap @e5
type "..."
keyboard
wait 0.5
snapshot --json
repeat until done
```

`snapshot --json` 返回：

```json
{
  "success": true,
  "screenshot": "<base64 png>",
  "width": 1179,
  "height": 2556,
  "app": "Safari",
  "elements": [
    {
      "ref": "@e1",
      "type": "XCUIElementTypeButton",
      "name": "搜索",
      "label": "Search",
      "bounds": {"x": 300, "y": 100, "width": 80, "height": 44},
      "center": {"x": 340, "y": 122},
      "enabled": true
    }
  ],
  "element_count": 42
}
```

这对 AI agent 非常友好。

因为外部 agent 不需要直接理解 WDA API。
它只需要：

```text
拿 screen state
选择 element ref
调用 tap / type / swipe
再 snapshot 验证
```

这就是把手机变成 agent 可操作的 tool environment。

## skills/openphone

OpenPhone repo 里有：

```text
skills/openphone/SKILL.md
```

它的作用是让 AI coding agent 自动知道怎么用 OpenPhone CLI。

skill 文件把工具能力写成：

```text
allowed-tools: Bash(python -m cli.main:*)
```

并说明什么时候使用：

```text
用户要求操作 iPhone / iPad
打开 app
截图
点击
输入
滚动
读屏幕
执行 autonomous phone task
```

这点非常关键。

OpenPhone 不只是“人类可以运行的脚本”。
它已经把自己包装成：

```text
agent-discoverable capability
```

这和我们之前看的 Litewrite / nanobot / FastAgent 是同一个趋势：

```text
工具要能被 agent 发现、理解、调用、验证。
```

对 Pengyi Research OS 来说，未来也应该有：

```text
skills/quant-backtest/SKILL.md
skills/research-report/SKILL.md
skills/factor-lab/SKILL.md
skills/data-quality/SKILL.md
skills/mobile-business/SKILL.md
```

这样 agent 不只是“知道概念”，而是知道怎么调用真实工具。

## PhoneClaw

PhoneClaw 是 OpenPhone 里最有产品感的一部分：

```text
autonomous iOS GUI agent
```

它的核心方法叫：

```text
Ralph Loop
```

README 里的流程是：

```text
EXECUTE -> EVALUATE -> FIX -> REPEAT
```

源码结构也对应得很清楚：

```text
PhoneClaw/
  run_phoneclaw.py
  loop.py
  planner.py
  evaluator.py
  executor.py
  state.py
  memory.py
  experience.py
  learn.py
  actions.py
```

一个任务的完整路径是：

```text
User task
  -> memory-first retrieval
  -> planner decomposes subtasks
  -> RalphLoop executes each subtask
  -> executor calls WDA actions
  -> evaluator checks screenshot against success criteria
  -> failure reason injected into next retry
  -> final answer generated from current screen
  -> UserMemory records task and insight
  -> ExperienceLog extracts procedural lessons
```

这比普通 GUI agent 更严肃。

普通 GUI agent 常见问题是：

```text
执行一步
不知道是否成功
失败后重复同样动作
没有长期记忆
下一次重新犯错
```

PhoneClaw 显式处理这些问题：

```text
每个 subtask 有 success_criteria
每步 action 后都 evaluator pass/fail
失败动作进入 failed_actions
重复同样失败动作会被提示
retry 超限后 skip 或 abort
任务状态写入 filesystem
trace / screenshot / XML 都被记录
```

这和我们一直强调的 Research OS 质量闸门一致：

```text
不要只让 agent 生成结果。
要让 agent 解释目标、执行步骤、验证结果、记录失败、学习经验。
```

## Planner

`TaskPlanner` 做的事情是：

```text
high-level task -> ordered subtasks with success criteria
```

它要求 LLM 返回 JSON list：

```text
id
instruction
success_criteria
```

如果 LLM 返回无法解析的内容，最多重试 3 次。
全部失败时 fallback 成单一 subtask：

```text
instruction = original task
success_criteria = task appears completed
```

它还会注入 `UserMemory` 的 user context。

这点很重要：

```text
手机任务经常和用户本人有关。
地点、常用 app、历史订单、常用联系人、语言偏好都会影响任务计划。
```

对 Research OS 来说，Planner 也应该注入：

```text
研究目标
当前项目状态
历史实验
失败模式
PM 偏好
数据可用性
当前 deadline
```

否则 plan 会很通用，无法贴合真实工作流。

## Executor

`IOSExecutor` 接收 VLM 生成的动作代码。

支持的动作包括：

```text
tap(rx, ry)
long_press(rx, ry)
swipe(rx1, ry1, rx2, ry2)
type("text")
text("text")
back()
home()
wait(seconds)
finish("message")
launch("app")
```

这里的坐标不是绝对像素，而是归一化坐标：

```text
(0.0, 0.0) -> top-left
(1.0, 1.0) -> bottom-right
```

Executor 会把 `[0, 1]` 坐标转成 physical pixels，再由 WDA 转成 logical coordinates。

这是一个很实用的选择。

因为不同 iPhone 分辨率不一样，VLM 如果直接输出像素，泛化会很差。
用归一化坐标可以把动作空间标准化。

但代码里也暴露一个工程风险：

```text
Executor 使用 exec(code_snippet, {}, local_context)
```

虽然 local context 只暴露了有限动作函数，但从安全角度，任何执行 LLM 生成代码的设计都应该非常谨慎。

作为 contributor，后面可以考虑把它改成更严格的 parser：

```text
只允许 function call AST
只允许白名单函数
只允许 literal 参数
拒绝任意 Python 语句
```

这是一个潜在 PR / improvement opportunity。

## Evaluator

`SubTaskEvaluator` 用 VLM 判断：

```text
当前 screenshot 是否满足 success_criteria
```

它要求输出 JSON：

```json
{
  "passed": true,
  "reason": "..."
}
```

解析失败时最多重试 2 次。
如果还是失败，保守返回 fail。

这体现了一个重要设计：

```text
GUI agent 不应该只相信自己刚才点了什么。
它要看结果画面是否符合标准。
```

对 Quant Research OS，对应是：

```text
不是生成了 backtest report 就算完成。
要有 evaluator 检查：
数据范围是否正确
是否有 look-ahead
是否有 survivorship bias
交易成本是否计入
样本外是否分离
结果是否可复现
```

PhoneClaw 的 evaluator 是一个很好的产品形态参考。

## Two-Layer Memory

PhoneClaw 的自学习记忆分两层：

```text
UserMemory
ExperienceLog
```

### UserMemory

`UserMemory` 存储：

```text
profile
app_usage
task_history
insights
frequent_patterns
stats
```

默认路径：

```text
PhoneClaw/data/user_profile.json
```

它支持 memory-first retrieval：

```text
如果历史 profile 已经能回答用户问题，就不碰设备，直接回答。
```

这很现实。

比如：

```text
用户问“我的美团账号名是什么”
如果之前任务已经看过并保存了，就不用再打开 app。
```

### ExperienceLog

`ExperienceLog` 存储 app-specific execution lessons：

```text
successful_navigation
failed_approach
ui_knowledge
timing
general
```

默认路径：

```text
PhoneClaw/data/experience_log.json
```

它会在执行前给 Executor 注入 hints：

```text
过去成功走过的路径
过去失败过的坐标
app UI 布局知识
需要等待多久
```

它还做：

```text
semantic deduplication
reinforcement counter
confidence upgrade
auto compaction when one app has >= 20 lessons
target <= 8 high-quality lessons per app
```

这比“把所有历史都塞 prompt”更工程化。

对 Research OS 来说，记忆也应该分层：

```text
User / PM memory
Project memory
Tool execution memory
Failure pattern memory
Market regime memory
Experiment result memory
```

并且不能无限膨胀，要能去重、强化、压缩。

## Learning Mode

PhoneClaw 有一个很现实的 `learn` 模式：

```bash
python -m cli.main learn Safari --describe "Send a message"
```

`learn.py` 的流程是：

```text
1. DemoRecorder 约 8 fps 轮询设备截图
2. 比较前后帧差异
3. 优先用 HoughCircles 检测 iOS Show Touches 的触点圆圈
4. 检测不到时用最大变化区域的 centroid 兜底
5. 保存带点击标注的 frame
6. 录制结束后调用 VLM 抽取可复用 navigation lessons
7. 写入 ExperienceLog
```

这意味着：

```text
人正常操作手机
agent 观察
agent 从演示中学 app 操作路径
```

这非常接近现实“组织知识传承”。

在企业里，很多业务流程不是 API，而是：

```text
老员工打开某个系统
点某个菜单
查某个字段
复制某个表
发给某个群
```

OpenPhone 的 learn mode 给了一个思路：

```text
用 human demonstration 把隐性流程变成 agent 可复用经验。
```

对我们也有直接启发。

如果未来做 Quant Research OS / Business OS：

```text
人类 PM 演示一次如何看研究报告
人类 analyst 演示一次如何检查数据异常
人类 sales 演示一次如何跟进客户系统
agent 记录、抽取、沉淀、复用
```

这就是从“自动化”走向“组织流程学习”。

## iOS Agent and Mail RAG

除了 PhoneClaw，项目里还有：

```text
ios_agent/
```

它提供更基础的 iOS automation framework：

```text
connection.py
controller.py
executor.py
task.py
recorder.py
run_ios_agent.py
application/mail/
```

其中 `application/mail` 展示了一个完整 pipeline：

```text
打开 Mail app
进入 inbox / mail list
识别最近 5 封邮件
逐封打开
截图记录
用 RAG 分析截图
输出文本报告和 JSON 数据
```

这说明 OpenPhone 不只是“会点手机”。

它已经在做：

```text
mobile GUI operation -> screenshot collection -> multimodal RAG analysis -> structured report
```

这个链路对我们特别有启发。

未来很多真实业务任务可以这样做：

```text
打开业务 app
按流程查看数据
截图/抽取关键页面
用 VLM / RAG 汇总
生成结构化报告
回写到 Research OS / CRM / 笔记系统
```

这就是 mobile agent 和 knowledge system 的结合。

## Evaluation Playground

OpenPhone 基于 AndroidLab 做评测，并扩展任务。

README 里提到：

```text
all_test_cloud_v1_hyper.sh -> 138 AndroidLab benchmark tasks
all_test_cloud_v1_hyper_add.sh -> 额外 4 个 mobile apps
```

评测相关目录：

```text
evaluation/
  auto_test.py
  task.py
  parallel.py
  configs.py
  config/
  tasks/

generate_result.py
eval.py
```

结果生成：

```bash
python generate_result.py \
  --input_folder ./logs/evaluation/ \
  --output_folder ./logs/evaluation/ \
  --output_excel ./logs/evaluation/test_name.xlsx
```

它还用 LLM evaluator 替代部分 rule-based evaluation：

```text
更适合判断真实 GUI task 是否完成
```

这点对我们也重要。

很多现实任务的评价不是简单 assert：

```text
页面是否真的到达目标
邮件内容是否正确提取
订单信息是否完整
报告是否回答问题
研究结论是否符合证据
```

这些都需要更灵活的 evaluator。

Quant Research OS 也一样：

```text
有些指标能 rule-based 检查
有些研究质量要 LLM judge + human PM judge
```

## Data and Training Pipeline

OpenPhone 的训练分两步：

```text
SFT
GRPO-style RL
```

`prepare_data/README.md` 说明数据生成链路：

```text
visual_model_data/data_maker.py
  -> generate visual CoT data with long reasoning and reflection

visual_model_data/o1_data_visual_cot_all.json
  -> raw data

visual_model_data/sft_data_maker.py
  -> Alpaca-style SFT data

visual_model_data/alpaca_format_o1_data_visual_cot.json
  -> SFT-ready dataset

rl/convert_to_hf_vl.py
  -> Hugging Face vision-language dataset for GRPO
```

`model_training/README.md` 说明：

```text
SFT 用 LLaMA Factory
Qwen2.5-VL-3B training
max context length 6144
max image pixels 2500000
2x A100 40GB for SFT
DeepSpeed ZeRO-3
BF16
gradient checkpointing
```

GRPO 训练基于 R1-V：

```text
Qwen2.5-VL-3B
至少 4x A100 40GB
3 GPUs for optimization
1 GPU for vLLM inference
```

这里最值得记录的是：

```text
OpenPhone 不只是 prompt engineering。
它把 mobile agent 做成了 data -> SFT -> RL -> eval -> CLI/product 的完整链路。
```

这对我们冲顶会非常重要。

一个有顶会潜力的系统项目，通常不只是 demo：

```text
problem formulation
dataset / benchmark
model / method
training recipe
evaluation
ablation
deployment artifact
open-source tool
```

OpenPhone 这条线很完整。

## 对 Pengyi Research OS 的启发

OpenPhone 对我们的最大启发是：

```text
agent 要进入真实 interface。
```

我们前面看的系统，多数还在研究环境：

```text
论文
代码
RAG
workspace
report
agent framework
```

OpenPhone 把 agent 放进手机：

```text
真实 app
真实账号
真实 UI
真实通讯
真实业务流程
真实隐私约束
真实失败模式
```

这对我们的方向很关键。

未来 Pengyi Research OS 可能有两类接口：

```text
1. Research interface
   paper, code, data, backtest, report

2. Business interface
   phone, IM, email, calendar, CRM, broker app, bank system, OA system
```

OpenPhone 属于第二类。

它让 agent 不只是“写研究报告”，还可以：

```text
看日程
查消息
整理邮件
操作业务 app
拉取页面信息
跟进沟通
生成结构化摘要
把现实业务流同步回 Research OS
```

这非常接近我们之前说的：

```text
回归组织
卖方
与人链接
与企业链接
直接产生 business
```

技术上，OpenPhone 给了这件事一个落点。

## Quant Research OS 映射

OpenPhone 到 Quant Research OS 的映射可以这样理解：

| OpenPhone | Quant / Research OS |
|---|---|
| mobile app GUI | broker / data vendor / bank / CRM / IM interface |
| snapshot | state observation |
| tap / type / swipe | action primitive |
| Ralph Loop | execute -> evaluate -> fix research/action loop |
| SubTaskEvaluator | backtest / report / data quality evaluator |
| UserMemory | PM / user preference / task history |
| ExperienceLog | tool failure pattern / workflow lessons |
| learn mode | human PM / analyst demonstration learning |
| OpenPhone CLI | external agent-accessible operation layer |
| OpenPhone-3B | lightweight local model for routine operations |
| device-cloud routing | small model / cloud model / tool / human routing |

一个未来场景：

```text
1. Quant PM 在手机上收到某个市场消息
2. Mobile Agent 抽取消息、相关链接、截图
3. Research Agent 根据消息生成因子假设
4. Data Agent 查数据源可用性
5. Backtest Agent 跑初步实验
6. Report Agent 生成 memo
7. PM 在手机上审核
8. 系统把决策和证据写入 Research OS
```

OpenPhone 主要解决第 1、2、7 层：

```text
真实手机入口
真实 app 操作
手机端 PM 审核与反馈
```

## 和 Litewrite 的关系

HKUDS036 Litewrite 解决的是：

```text
research output workspace
```

HKUDS037 OpenPhone 解决的是：

```text
real-world mobile interface
```

两者结合非常有意思：

```text
OpenPhone 把手机里的信息、沟通、页面和业务流程拿进来。
Litewrite 把它们整理成报告、论文、proposal、memo 和可审阅 artifact。
```

一个完整 Research OS 需要两端：

```text
input side: 真实世界信息入口
output side: 研究产出和发布入口
```

OpenPhone 是 input / action side。
Litewrite 是 output / writing side。

## 可以快速应用的方向

我们短期不需要马上接真实手机做自动化。

但可以先吸收它的产品结构：

```text
1. 给每个工具做 agent-friendly CLI
2. 每个 CLI 都支持 --json 输出
3. 每个工具都配一个 SKILL.md
4. 每个任务都设计 observe -> act -> evaluate -> fix loop
5. 每个失败都记录为 experience
6. 每个重复流程都允许 human demo -> lesson extraction
```

这比“做一个大 agent”更可持续。

对我们现在最实用的版本是：

```text
Pengyi Research CLI
  snapshot-project
  list-experiments
  run-backtest
  inspect-result
  generate-report
  evaluate-report
  memory show
  memory query
```

然后配：

```text
skills/pengyi-research/SKILL.md
```

让 Codex / Claude Code / future agents 能自动发现和调用。

## PR / Improvement Opportunities

从源码阅读角度，OpenPhone 有一些很适合 contributor 的切入点。

第一类是 repo hygiene：

```text
当前 tracked files 里有 104 个 __pycache__ / .pyc 文件。
可以提交 PR 清理这些生成文件，并补充 .gitignore。
```

这类 PR 小而明确，非常适合我们。

第二类是 CLI docs：

```text
补一张 CLI command map
补 snapshot JSON schema
补 agent-driven workflow examples
补 common failure cases
补 WDA setup troubleshooting
补 supported app bundle ID extension guide
```

第三类是安全：

```text
IOSExecutor 目前执行 LLM 输出 code_snippet。
可以改成 AST whitelist parser，只允许 tap / swipe / type / wait / launch / finish 等函数调用。
```

第四类是测试：

```text
给 cli.utils 的 JSON output 加单测
给 coordinate parsing 加单测
给 memory query / experience dedup 加单测
给 evaluator JSON parsing 加单测
```

第五类是 Research OS template：

```text
补一个从 phone task trace -> structured report 的最小 example
补一个 mobile task lesson extraction demo
补一个 business app workflow case study
```

我们提 PR 的原则不变：

```text
先真实阅读和使用。
只提能改善文档、测试、稳定性、可复现性的 PR。
不为了 PR 而 PR。
```

## 小结

OpenPhone 在 HKUDS Agent Product / Workspace 系列中的位置是：

```text
HKUDS033 ClawTeam
  -> agent organization

HKUDS034 ClawWork
  -> agent economic accountability

HKUDS035 FastAgent
  -> agent execution engine

HKUDS036 Litewrite
  -> research writing / artifact workspace

HKUDS037 OpenPhone
  -> real-world mobile interface and AI phone agent
```

它把 agent 从电脑里的 research workspace，推进到真实手机 app 和真实业务界面。

对我们来说，这一篇的关键词是：

```text
mobile interface
agent-friendly CLI
Ralph Loop
evaluate-and-fix
self-learning memory
human demonstration learning
device-cloud routing
real-world app operation
```

下一阶段如果继续 Agent Product / Workspace 系列，可以看：

```text
MoChat
UpSkill
VideoAgent
Auto-Deep-Research / DeepResearch-Eval
```

其中 OpenPhone 和 MoChat 这条线，会更接近：

```text
agent 与真实沟通、真实组织、真实业务入口的连接。
```

这是 Research OS 最后要走向现实生产力系统的关键部分。
