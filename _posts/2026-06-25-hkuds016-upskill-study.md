---
title: "HKUDS016: UpSkill 作为 Failure-to-Skill Distillation 与 Agent Self-Improvement Layer"
date: 2026-06-25 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds016, hkuds, upskill, skill-distillation, ralph-loop, agent-self-improvement, claude-code, codex-skills, terminal-bench, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第十七篇。

```text
HKUDS016 -> UpSkill
```

上一篇 `HKUDS015` 看的是 `OpenHarness`：

```text
OpenHarness = Agent Harness Runtime + Personal Agent Infrastructure Layer
```

`OpenHarness` 解决的问题是：

```text
如何把 LLM 包成一个真正可运行的 agent runtime。
```

这一篇自然接到 agent 的长期进化问题：

```text
UpSkill = Failure-to-Skill Distillation + Agent Self-Improvement Layer
```

如果说 OpenHarness 是 agent 的身体和运行时，那么 UpSkill 更像是 agent 的经验沉淀机制。
它关心的不是单次任务能不能做完，而是：

```text
这次失败能不能变成下次可调用的 skill？
这次成功能不能变成长期可复用的 workflow？
强模型的经验能不能校准成弱模型真的会用的操作手册？
```

这对 Pengyi Research OS 的意义非常直接。

我们一直在做 HKUDS / LLMQuant / quant research / website / RA materials / project study。
真正的复利不只是“今天做完一个 repo study”，而是把每一次阅读、踩坑、修复、写作、发布都变成一个后续 agent 可以调用的技能。

```text
session
-> trace
-> diagnosis
-> skill
-> validation
-> store
-> retrieval
-> next session
```

这就是 UpSkill 最值得我们吸收的地方。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `UpSkill`。

| Item | Value |
|---|---|
| repo | `UpSkill` |
| remote | `https://github.com/HKUDS/UpSkill.git` |
| branch | `main` |
| local head | `6e7bf61` |
| latest local commit date | `2026-06-20 14:27:03 +0800` |
| latest local commit | `Update README.md` |
| status | clean, synced with `origin/main` after fetch |
| local tags | none |
| license | `MIT` |
| tracked files by `rg --files` | 928 |
| Python files | 144 |
| shell files | 200 |
| benchmark tasks | 89 under `tb_harbor_2.0/tasks` |
| train / test split | 25 train / 64 test |
| categories | 16 |
| curated ACP categories | 9 |
| syntax check | `py -m compileall -q scripts cc-integration` passed |
| integration shell test | not run locally, WSL bash is unavailable in this Windows environment |

一句话先行：

```text
UpSkill 把 agent 的失败轨迹蒸馏成 SKILL.md，再用 Ralph Loop 让弱模型带着 skill 复跑验证；只有被验证过的 skill 才进入长期 skill store，并通过 CLAUDE.md index、slash command、hook、keyword matching 在未来任务中被调用。
```

## 它解决什么问题

今天的 agent 已经很强，但有一个根本问题：

```text
agent 的能力经常被锁死在当前模型价格和当前上下文里。
```

贵模型可以解决更多问题，但成本高。
便宜模型可以全天跑，但容易在复杂任务里失败。
普通 workflow 的问题是：

```text
失败结束后，经验没有沉淀。
下一次遇到类似任务，agent 还是重新犯错。
人类修过的坑，没有进入 agent 的长期操作系统。
强模型写的一段建议，也不一定是弱模型真的能执行的建议。
```

UpSkill 的核心回答是：

```text
不要只升级模型。
要升级模型可调用的 skill library。
```

这和我们自己的路线高度一致。
我们最终想要的不是一次性 chat，而是一个越用越强的个人 Research OS / Quant OS：

```text
读 paper -> 形成 reading skill
看 repo -> 形成 repo study skill
做 backtest -> 形成 experiment skill
踩数据坑 -> 形成 data validation skill
写博客 -> 形成 technical writing skill
提 PR -> 形成 contribution skill
```

## 三个角色

UpSkill 里有三个角色：

| Role | Meaning | 作用 |
|---|---|---|
| Daily Model | 你日常使用的模型 | 正常工作，不一定参与蒸馏 |
| Teacher | 强模型 | 分析失败、生成 skill、修订 skill |
| Student | 弱模型 | 被增强的目标模型，skill 必须对它有效 |

这点非常关键。

很多系统会让强模型写一段“最佳实践”，然后直接塞给 agent。
UpSkill 更严格：

```text
Teacher 写出来的 skill 不算数。
Student 带着这个 skill 成功跑通，才算数。
```

所以 UpSkill 不是普通 prompt library。
它更像是：

```text
validated skill library
```

skill 的价值来自验证，不只是来自写得漂亮。

## 两个平面

这个 repo 其实有两个平面。

第一个平面是实验研究：

```text
tb_harbor_2.0/
scripts/
configs/
```

它用 Terminal-Bench 2.0 做实验，证明从 Student trajectory 中蒸馏出的 ACP / skill 能不能提升弱模型。

第二个平面是真实 agent integration：

```text
cc-integration/
```

它把方法做成 Claude Code integration：

```text
install.sh
hooks/
skills/
upskill-build.sh
upskill-store.sh
parse-skill.py
configure-project.sh
capture-prompt.sh
```

这两个平面要分开看。
实验平面回答“这个方法有没有效果”。
集成平面回答“真实使用时怎么让它进入 agent 工作流”。

## Benchmark 平面

`tb_harbor_2.0` 是 Terminal-Bench 2.0 的本地实验数据。

| Metric | Value |
|---|---:|
| total tasks | 89 |
| categories | 16 |
| train | 25 |
| test | 64 |
| easy / medium / hard | 4 / 55 / 30 |
| curated ACP categories | 9 |

类别分布里 `software-engineering` 最大：

| Category | Total |
|---|---:|
| software-engineering | 26 |
| system-administration | 9 |
| data-science | 8 |
| scientific-computing | 8 |
| security | 8 |
| debugging | 5 |
| file-operations | 5 |
| data-processing | 4 |
| mathematics | 4 |
| model-training | 4 |
| machine-learning | 3 |
| singleton categories | 5 |

这说明 UpSkill 的主要验证场景不是聊天问答，而是真实 terminal task：

```text
写代码
修 bug
处理数据
跑系统命令
恢复文件
构建项目
做安全/科学/机器学习任务
```

对 agent 来说，这类任务更接近真实工程。

## 实验结果

`tb_harbor_2.0/RESULTS.md` 里给出的 test set 结果：

| System | Pass Rate | Cost |
|---|---:|---:|
| Student, `deepseek-v4-flash` | 45.3% | $1.93 |
| Teacher, `deepseek-v4-pro[1m]` | 50.0% | $4.01 |
| Student + ACP | 51.6% | $2.36 |

从结果看，Student + ACP 在 held-out test set 上超过了 Teacher：

```text
Student        29 / 64 pass
Teacher        32 / 64 pass
Student + ACP  33 / 64 pass
```

GAIN tasks：

| Task | Category |
|---|---|
| `count-dataset-tokens` | model-training |
| `headless-terminal` | software-engineering |
| `mailman` | system-administration |
| `query-optimize` | data-science |
| `winning-avg-corewars` | software-engineering |

LOSS task：

| Task | Category |
|---|---|
| `tune-mjcf` | scientific-computing |

这里有一个需要认真看的口径差异。

README 里强调 serving 端：

```text
Flash + UpSkill 每个 test task 约 $0.04
Pro 每个 test task 约 $0.06
```

但 `RESULTS.md` 里也记录了完整 brewing overhead：

| Phase | Cost |
|---|---:|
| per-task Brew | $10.11 |
| Curator | $2.82 |
| Ralph Validation | ~$103 |
| total brewing | ~$116 |

所以正确理解是：

```text
UpSkill 的 serving 成本低。
但 skill 生成和 Ralph validation 不是免费。
它适合高频复用、长期摊销、组织级或个人长期 OS。
```

这对我们很重要。
如果只是一次性任务，未必值得做完整 UpSkill。
如果是长期 Research OS / Quant OS，它就非常值得。

## 实验 Pipeline

实验复现链路是：

```text
Student baseline
-> Teacher baseline
-> Brew
-> Curate
-> Ralph validation
-> Serve
-> Analyze
```

对应脚本：

| Script | Function |
|---|---|
| `scripts/run_baseline.sh` | 跑 Student / Teacher baseline |
| `scripts/run_brew.py` | 从 Student trajectory 里生成 per-task ACP |
| `scripts/run_curator.py` | 把 per-task ACP 合并成 category-level ACP |
| `scripts/run_ralph.py` | Student 带 ACP 复跑，失败则 Teacher 修订 |
| `scripts/run_serve.sh` | 把 curated ACP 部署到 test tasks |
| `scripts/run_analyze.py` | 统计 pass rate、gain/loss、token cost、category delta |

这个流程的本质是：

```text
失败轨迹不是日志垃圾。
失败轨迹是 skill training data。
```

### run_brew.py

`run_brew.py` 会读取 Student baseline 的 `trajectory.json`，截取最近若干步，然后让 Teacher 生成三类文件：

```text
/app/CLAUDE.md
/app/.claude/skills/solve-task.md
/app/memory/feedback.md
```

它不是简单总结，而是要求 Teacher 写出可执行 guidance：

```text
project guidance
ordered checklist
feedback memory
```

这三个通道后来在 `cc-integration` 里被统一成单个 `SKILL.md`。

### run_curator.py

`run_curator.py` 会按 category 聚合 per-task ACP。
例如多个 software-engineering 的 task 会被合成一个更通用的 category skill。

这一步很像人类做知识管理：

```text
一个 task 的经验太窄。
多个 task 的经验要合并、去重、抽象。
```

它让 skill 从单点经验变成 category-level playbook。

### run_ralph.py

`run_ralph.py` 是实验版 Ralph Loop。

流程是：

```text
copy original task
deploy ACP as CLAUDE.md
run Student
check pass/fail
if fail:
    extract failure trajectory
    Teacher revises ACP
    retry
```

最多 3 轮。

这点非常像真正的 R&D：

```text
第一次总结不够。
让学生带着总结再做一次。
观察它还是哪里错。
再改总结。
```

这不是静态 prompt engineering。
这是以 Student 行为为反馈的 skill calibration。

### run_serve.sh

`run_serve.sh` 会把 curated ACP 部署到 test tasks。
如果某个 category 没有 ACP，它会记录 `[no ACP]`。

这很重要，因为 test set 的 16 个类别里，train 只覆盖 11 类，curated ACP 只有 9 类。
所以 UpSkill 并不是对所有类别都有技能覆盖。

未来我们做 Research OS 也一样：

```text
skill library 的覆盖面要被显式管理。
没有 skill 的 task，不应该假装有 skill。
```

## Claude Code Integration 平面

`cc-integration` 是这篇最值得吸收的部分。

它把 UpSkill 做成一个真实使用的 agent 插件。

安装后主要写入：

```text
~/.claude/hooks/
~/.claude/skills/
~/.claude/upskill-store/
~/.claude/upskill.conf
```

核心文件：

| File | Function |
|---|---|
| `install.sh` | 安装 hook、slash command、模板和 store |
| `configure-project.sh` | 给当前项目配置 `.claude/settings.local.json` |
| `capture-prompt.sh` | `UserPromptSubmit` hook，捕获首个有效 prompt，并做 skill matching |
| `before-session.sh` | `SessionStart` hook，提示是否有 pending failure |
| `after-session.sh` | `SessionEnd` hook，保存 session，检测失败，设置 pending flag |
| `save-session.sh` | 保存 prompt、metadata、session log |
| `upskill-build.sh` | 核心 build pipeline |
| `parse-skill.py` | 解析 Teacher 输出的 `SKILL.md` |
| `upskill-store.sh` | 管理 skill store，生成全局 `CLAUDE.md` index |
| `skills/upskill-*.md` | slash command 指令文件 |

它和 OpenHarness 的关系是：

```text
OpenHarness 关注 agent runtime。
UpSkill 关注 runtime 里的 skill acquisition / skill serving。
```

## 安装与项目配置

`install.sh` 是 canonical installer。
它支持：

```bash
bash install.sh
bash install.sh --remote
bash install.sh --dry-run
bash install.sh --uninstall
```

它会安装：

```text
hook scripts
slash command skills
upskill-store
upskill.conf
```

`configure-project.sh` 会修改当前项目的 `.claude/settings.local.json`，加入：

```json
{
  "claudeMd": "~/.claude/upskill-store/CLAUDE.md",
  "hooks": {
    "UserPromptSubmit": "capture-prompt.sh",
    "SessionStart": "before-session.sh",
    "SessionEnd": "after-session.sh"
  }
}
```

这意味着 UpSkill 不是每次手动复制 prompt，而是被接进 agent lifecycle：

```text
prompt submit
session start
session end
```

这是一个非常正确的抽象。
长期 OS 不能靠人类每次手动整理。
它必须插进 workflow 的生命周期。

## Prompt Capture

`capture-prompt.sh` 做了几件事：

1. 保存 hook stdin 原始 JSON。
2. 提取 transcript path。
3. 捕获本 session 第一个 substantial prompt。
4. 跳过 `/upskill`、`/clear`、`/compact` 等控制命令。
5. 扫描 skill manifest，做关键词匹配。
6. 写入 `skill_match.txt` 和 `skill_notify.txt`。
7. 如果是 auto mode，则把匹配到的 skill 提示注入给 agent。

这对应两个 serving mode：

| Mode | Behavior |
|---|---|
| `interactive` | 只保存匹配结果，用户用 `/upskill-run` 手动选择 |
| `auto` | 每次 prompt 自动匹配，agent 主动提示是否应用 skill |

这个设计很实用。
完全自动有误用风险。
完全手动又容易忘。
所以 interactive / auto 两种模式都保留，是合理的产品边界。

## Session Capture

`before-session.sh` 很轻：

```text
如果有 pending_build，就提醒用户运行 /upskill-build。
```

`after-session.sh` 更关键：

```text
save session
detect failure
set pending_build
```

失败检测基于两类信号：

```text
non-zero exit code
self-reported failure text
```

`save-session.sh` 会保存：

```text
metadata.txt
session.log
latest symlink
```

并保留最近 10 个 session。

这对我们的 Research OS 很有启发。
我们不应该只保存最终文章，还要保存：

```text
任务描述
过程 trace
失败原因
验证命令
最终产物
```

因为 skill 只能从过程里长出来。

## upskill-build.sh

`upskill-build.sh` 是核心。

它的阶段：

```text
Phase 0: setup worktree
Phase 1: load weak trajectory
Phase 2: Teacher solves task
Phase 3: Teacher generates SKILL.md
Phase 4: parse SKILL.md
Phase 5: Ralph validation
```

### Phase 0: isolated worktree

它会创建一个临时 worktree：

```text
/tmp/upskill-worktree-XXXXXX
```

如果当前目录是 git repo：

```text
git worktree add --detach
copy modified tracked files
copy untracked files
```

如果不是 git repo，就用 `rsync` 复制，并排除：

```text
node_modules
.venv
venv
.git
build
dist
__pycache__
```

这个思路很重要。
skill build 不能直接污染用户当前 workspace。
Teacher 和 Student 复跑都应该发生在隔离环境里。

### Phase 1: load trajectory

它读取失败 session log 的最后 500 行：

```text
WEAK_TRAJECTORY=$(tail -n 500 "$FAILURE_LOG")
```

这里有一个明确取舍：

```text
只看最近上下文，避免 prompt 过长。
```

对 Research OS 来说，我们也需要类似机制：

```text
trace 太长时，要抽取关键失败片段，而不是全量塞进去。
```

### Phase 2: Teacher solve

Teacher 会在 worktree 中独立尝试解决原任务。

意义是：

```text
Teacher 不只是评论 Student 错了什么。
Teacher 还要自己走出一条可行路径。
```

这很接近人类 mentor：

```text
先自己做一遍，再回头解释学生为什么错。
```

### Phase 3: generate SKILL.md

Teacher 被要求输出一个完整 `SKILL.md`：

```text
YAML frontmatter
# Domain Knowledge
# Step-by-Step
# Feedback / Lessons
```

它还要求：

```text
short concrete sentences
explicit command examples
numbered checklists
imperative form
copy-paste-ready commands
no missing inferred steps
```

这非常关键。
skill 不是写给强模型看的。
skill 是写给弱模型执行的。

所以 skill 要降低推理负担：

```text
不要抽象。
要步骤化。
要可复制。
要有验证。
```

### Phase 4: parse

Teacher 必须用 marker 输出：

```text
===BEGIN_FILE: SKILL.md===
...
===END_FILE: SKILL.md===
```

`parse-skill.py` 负责解析。
它还做了 path traversal 防护：

```text
resolved path 必须 stay inside base dir
```

如果 Teacher 没按格式输出，`upskill-build.sh` 会最多重试 2 次。
这点很工程化。
真实 agent 系统必须预设模型会不听话。

### Phase 5: Ralph Loop

Ralph Loop 是这个项目最核心的概念。

它每轮都会：

```text
create fresh worktree
deploy Domain Knowledge to CLAUDE.md
deploy Step-by-Step to .claude/skills/solve-task/SKILL.md
deploy Feedback / Lessons to memory
run Student with task prompt
check BUILD_RESULT
if pass:
    store skill
else:
    Teacher revises skill
```

最多 3 轮。

这一步的关键不是“让 Student 再做一次”。
关键是：

```text
Teacher 的 guidance 要被 Student 行为校准。
```

如果 Student 带着 skill 还是失败，Teacher 得看新的失败轨迹，然后把 skill 改得更 explicit。

这就是：

```text
failure with guidance -> higher quality feedback signal
```

普通失败只说明 Student 不会。
带 skill 失败则说明：

```text
这个 skill 没讲清楚。
这个 step 不可执行。
这个假设对 Student 不成立。
这个验证不够强。
```

这就是 UpSkill 的真正创新点。

## SKILL.md Schema

UpSkill 的 skill 最终统一成单个文件：

```markdown
---
name: skill_20260605_001
description: ...
metadata:
  category: data-analysis
  base_model: deepseek-v4-flash
  created: 2026-06-05T12:34:56
  trigger_keywords: [csv, encoding, json, database, query]
---

# Domain Knowledge

# Step-by-Step

# Feedback / Lessons
```

三个部分分别对应：

| Section | 作用 |
|---|---|
| `Domain Knowledge` | 总体知识、常见坑、正确方法、验证 checklist |
| `Step-by-Step` | 具体执行流程，给 agent 当操作手册 |
| `Feedback / Lessons` | 可沉淀到 memory 的 Rule / Why / How |

这个 schema 对我们很有价值。

我们自己的 Research OS 也可以定义类似 schema：

```text
RESEARCH_SKILL.md
FACTOR_SKILL.md
REPO_STUDY_SKILL.md
PR_SKILL.md
WRITING_SKILL.md
```

每个 skill 都包含：

```text
什么时候用
输入是什么
步骤是什么
常见失败是什么
验证命令是什么
输出物是什么
```

## upskill-store.sh

`upskill-store.sh` 是长期 skill library。

支持命令：

```bash
upskill-store.sh add
upskill-store.sh list
upskill-store.sh search
upskill-store.sh status
upskill-store.sh remove
upskill-store.sh sync
```

它的持久结构大致是：

```text
~/.claude/upskill-store/
  CLAUDE.md
  <category>/
    manifest.yaml
    skill_YYYYMMDD_HHMMSS/
      SKILL.md
      description.txt
```

`sync` 会重新生成全局 `CLAUDE.md` index。

这个全局 index 不是把所有 skill 全量塞进上下文，而是写 summary 和 pointer：

```text
Skill: data-analysis
Base model: deepseek-v4-flash
Trigger: csv, encoding, json
- skill_xxx: description
  Read ~/.claude/upskill-store/data-analysis/skill_xxx/SKILL.md
```

这是非常重要的上下文控制：

```text
always-visible index
on-demand full skill
```

对 Research OS 也一样。
不能每次把所有笔记都塞进 agent。
应该：

```text
全局索引常驻
具体 skill 按需读取
```

## Slash Commands

`cc-integration/skills` 下有一组 slash commands：

| Command | Function |
|---|---|
| `/upskill-init` | 初始化或更新 UpSkill |
| `/upskill-configure` | 当前项目启用 hooks |
| `/upskill-build` | 从 session 中构建 skill |
| `/upskill-run` | 交互式选择并应用 skill |
| `/upskill-list` | 浏览 skill library |
| `/upskill-status` | 查看 skill 数量和 build 状态 |
| `/upskill-mode` | 切换 interactive / auto |
| `/upskill-model` | 查看 Teacher / Student 模型 |
| `/upskill-remove` | 删除 skill |
| `/upskill-uninstall` | 卸载 |

这里和 Codex skill 的思想非常接近。

关键是：

```text
skill 不只是文件。
skill 要有管理命令、触发方式、索引、验证和生命周期。
```

## 与 Codex Skills 的连接

我们现在用的 Codex 本身也支持 skills。
UpSkill 给我们的启发是：

```text
不要只手写 skills。
要让 skills 从真实工作轨迹中长出来。
```

更具体地说，未来 Pengyi OS 可以有两层：

```text
Human-authored skills
    人类主动写的规范、模板、工作流

Experience-distilled skills
    从失败 session、成功 session、PR、文章发布、实验复盘里自动沉淀出来
```

UpSkill 负责后者。

而 Codex skill 体系可以作为 serving layer：

```text
UpSkill generates SKILL.md
Codex reads SKILL.md
Pengyi Research OS stores and routes SKILL.md
```

这条线非常值得做。

## 与 DeepTutor 的关系

`HKUDS014 DeepTutor` 解决的是：

```text
怎么训练人。
```

`HKUDS016 UpSkill` 解决的是：

```text
怎么训练 agent 的可调用能力。
```

两者可以组合：

```text
DeepTutor:
    human mastery path
    quizzes
    learning space
    knowledge center

UpSkill:
    agent failure memory
    reusable skill
    Ralph validation
    skill serving
```

对我们来说：

```text
DeepTutor 训练 Pengyi。
UpSkill 训练 Pengyi 的 agent。
```

这是非常强的组合。

## 与 OpenHarness 的关系

`HKUDS015 OpenHarness` 是 runtime：

```text
tools
permissions
memory
skills
plugins
MCP
terminal UI
swarm
provider workflows
```

UpSkill 是 skill acquisition：

```text
capture failure
build skill
validate skill
store skill
serve skill
```

如果放到一个完整 agent OS 里：

```text
OpenHarness -> agent runs
UpSkill     -> agent learns
DeepTutor   -> human learns
```

这三个正好构成一条主线。

## 与 AutoAgent 的关系

`AutoAgent` 关注的是：

```text
从自然语言需求生成 agent / tool / workflow。
```

`UpSkill` 关注的是：

```text
从真实失败中生成 skill。
```

区别是：

| Project | Input | Output |
|---|---|---|
| AutoAgent | 用户需求 | agent / tool / workflow |
| UpSkill | failure trajectory / success trajectory | validated skill |

对 Research OS 来说，两者可以接：

```text
AutoAgent 负责创造新 workflow。
UpSkill 负责把 workflow 的失败变成改进后的 skill。
```

## 与 Quant Research OS 的关系

我们之前一直在说：

```text
R&D Agent for Quant Research
= 自动提出因子假设
+ 自动实现
+ 自动回测
+ 自动诊断偏差
+ 自动生成下一轮研究计划
+ 人类 PM 审核
```

UpSkill 可以放在这里的“诊断偏差”和“下一轮研究计划”中间。

完整链路可以是：

```text
factor idea
-> implementation
-> backtest
-> diagnostics
-> failure trace
-> factor research skill
-> next run uses skill
```

例子：

```text
某次 backtest 发现 lookahead bias
-> UpSkill 生成 "avoid lookahead bias in factor backtests" skill
-> 下次 agent 写因子时自动检查 shift、rebalancing date、universe snapshot
```

再比如：

```text
某次数据清洗错用了 survivorship-biased universe
-> 生成 "point-in-time universe validation" skill
-> 后续所有回测前先检查 universe construction
```

这才是 quant R&D agent 的复利。

不是只让 agent 多跑几次。
而是让每次失败都改变下一次 agent 的默认行为。

## Product Design 启发

UpSkill 的产品设计可以总结成六个模块：

| Module | Meaning |
|---|---|
| Capture | 捕获 prompt、session、failure trace |
| Distill | Teacher 分析并写 skill |
| Validate | Student 带 skill 复跑 |
| Store | 通过 manifest 和 category 存 skill |
| Serve | CLAUDE.md index + slash command + keyword matching |
| Manage | list/status/remove/mode/model/init |

我们自己的 Pengyi Research OS 也可以照这个做。

最小 MVP 不需要一开始就全自动。

可以先做：

```text
notes/skills/
  repo-study/
  quant-backtest/
  paper-reading/
  website-writing/
  pr-contribution/
```

每次完成一个任务后，手动或半自动生成：

```text
SKILL.md
trace.md
validation.md
examples.md
```

然后在 Codex / website / private workspace 里逐步接入。

## Validation 的边界

UpSkill 的真实集成版有一个需要注意的点：

```text
Ralph validation 通过 BUILD_RESULT: PASS marker 判断。
```

也就是说，Student 最后需要输出：

```text
===BUILD_RESULT: PASS===
```

这在工程上很方便，但不是强验证。

更严格的系统应该接：

```text
unit tests
benchmark tests
backtest checks
data leakage tests
lint / typecheck
external verifier
human PM approval
```

对 Quant OS 尤其重要。

我们不能让 agent 自己说“回测没问题”就入库。
必须跑真正的 verification：

```text
no lookahead
no survivorship bias
turnover sanity
transaction cost
out-of-sample split
factor neutralization
capacity check
data timestamp audit
```

所以 UpSkill 的思想要保留，但验证层必须更硬。

## Engineering Notes

读代码时看到几个后续可以 PR 的小点。

| Area | Note |
|---|---|
| `scripts/retrieve_teacher.py` | 里面硬编码了 `/Users/jiangyangqin/Desktop/research/HKUDS/Upskill`，可以改成基于 `__file__` 的 `PROJECT_DIR` |
| `scripts/run_curator.py` | 错误分支用了 `sys.exit(1)`，但文件顶部没有 import `sys` |
| shell test | `cc-integration/test/test_pipeline.sh` 对 Linux/macOS 友好，Windows/WSL 缺失时无法直接跑 |
| validation | 集成版依赖 `BUILD_RESULT: PASS` marker，可以扩展为外部 verifier |
| skill schema | 可以和 Codex skill / OpenHarness skill schema 做互操作 |

这类都是很适合开源贡献的小切口。
不是为了提 PR 而提 PR，而是真实读代码读出来的工程问题。

## 对 Pengyi 的直接行动

我们可以立刻吸收 UpSkill 的方法，不必等完整系统。

第一步，建立个人 skill store：

```text
Pengyi Research OS/
  skills/
    repo-study/
    paper-reading/
    quant-backtest/
    website-publishing/
    ra-application/
    pr-contribution/
```

第二步，每次 session 结束后做一个轻量复盘：

```text
What was the task?
What failed?
What fixed it?
What should be reused?
How to verify next time?
```

第三步，把复盘写成 `SKILL.md`：

```text
Domain Knowledge
Step-by-Step
Feedback / Lessons
Validation
```

第四步，在下一次类似任务开始前让 Codex 先读对应 skill。

这就是我们自己的 UpSkill-lite。

完整形态可以更进一步：

```text
Codex session trace
-> automatic skill proposal
-> human PM approve
-> verification run
-> skill store
-> website / private repo / Codex skill sync
```

这里的“人类 PM 审核”非常关键。
UpSkill 强调 Student validation。
我们还要加一层 human PM：

```text
不是所有通过的 skill 都应该入库。
入库意味着它会影响未来行为。
```

这就是 Research OS 的 governance。

## UpSkill 的一句话定义

```text
UpSkill 是一个把 agent 的失败与成功轨迹转化为可验证、可存储、可检索、可复用 skill 的自我改进系统。
```

它最值得我们学的不是某个脚本，而是这个闭环：

```text
failure is data
skill is product
validation is gate
store is memory
retrieval is compounding
```

翻译到我们的路线：

```text
每一次 coding、研究、投递、沟通、写作、回测、PR，都应该留下可复用技能。
```

这就是个人 AI scientist / quant researcher 的真正复利。

## Next

下一篇继续接 `HKUDS017`：

```text
HKUDS017 -> AnyTool
```

按当前主线，`AnyTool` 会自然接在 UpSkill 后面：

```text
UpSkill -> agent 怎么获得 skill
AnyTool -> agent 怎么泛化 tool use
OpenHarness -> agent 怎么运行和治理这些能力
```

这条线已经很清楚：

```text
Agent Framework / Workspace
    runtime
    skill
    tool
    workspace
    governance
```

我们继续把 HKUDS 学透，然后把它们融进 Pengyi Research OS。
