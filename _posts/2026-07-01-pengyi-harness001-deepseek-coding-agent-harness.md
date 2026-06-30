---
title: "PENGYI_HARNESS001: DeepSeek Coding Agent Harness - Claude Code vs Codex 的产品启示与工程预演"
date: 2026-07-01 00:30:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-harness, harness001, deepseek, coding-agent, claude-code, codex, agent-harness, product-taste, engineering-rehearsal, research-os]
---

这一篇接在 `PENGYI_HARNESS_MAP000` 后面。

`MAP000` 做的是总览：

```text
Agent / Research / Quant / Tool / Memory / Product 六类 Harness
```

这一篇进入一个更具体、更接近真实产品决策的场景：

```text
PENGYI_HARNESS001:
如果我们以 DeepSeek PM 视角，要做 DeepSeek 的 Coding Agent Harness，
应该如何理解当前市面上最值得学习的两类代表：
Claude Code 与 Codex？
```

核心目标不是说谁更强。

核心目标是抽出：

```text
产品 taste
工程 taste
研究 taste
DeepSeek Harness 的启示
最小工程预演
```

## 一句话结论

```text
Claude Code 是 cockpit-first。
Codex 是 factory-first。
DeepSeek Coding Agent Harness 不应该只复制其中一个，
而应该做成 mode-aware agent execution OS。
```

更完整地说：

```text
Claude Code 默认更像本地驾驶舱：
人类开发者高频在环，观察、打断、批准、修正、继续。

Codex 默认更像云端任务工厂：
人类提交任务，agent 在隔离环境里执行，返回 diff、PR、review 或任务结果。

DeepSeek Harness 的机会不是在 UI 上模仿，
而是在本地 cockpit、云端 worker、权限、评测、trace、成本和企业部署之间做统一执行 OS。
```

## 先定义问题

如果我是 DeepSeek 的 coding agent harness PM，我不会把问题定义成：

```text
我们也做一个 Claude Code。
```

或者：

```text
我们也做一个 Codex。
```

这两个定义都太浅。

真正的问题应该是：

```text
如何把 DeepSeek 模型能力包装成一个可执行、可控、可评估、可复现、可规模化的软件工程执行系统？
```

也就是：

```text
DeepSeek Coding Agent Harness =
  model intelligence
  + repo context
  + task contract
  + tool runtime
  + permission boundary
  + execution trace
  + test / eval
  + diff / PR artifact
  + human review
  + memory writeback
```

这不是一个聊天机器人。

这是一个把模型能力转成真实工程交付的系统层。

## 官方信息给出的基本事实

Claude Code 的官方定位是 agentic coding tool：它可以读代码库、编辑文件、运行命令，并集成到开发工具里；官方也明确列出 terminal、IDE、desktop app、browser 等 surface。

Codex 的官方定位是 OpenAI 的 coding agent：Codex web 可以在云端后台任务里工作，包括并行任务，并可以连接 GitHub、为 repo 工作、创建 pull request。Codex CLI 也存在本地终端形态，可以读、改、运行当前目录里的代码。

所以更准确的产品判断是：

```text
Claude Code 的默认味道更偏 cockpit。
Codex 的 web/cloud 默认味道更偏 factory。
Codex CLI 则补上了 cockpit 侧。
Claude Code web / desktop 也在补 cloud / multi-session 侧。
```

因此不能把二者讲成绝对二分。

真正的二分是：

```text
default product philosophy
```

而不是：

```text
absolute capability boundary
```

## Claude Code: Cockpit-first Harness

Claude Code 的产品 taste 是：

```text
developer stays in the cockpit
```

它进入开发者的工作现场：

```text
terminal
IDE
repo
file system
shell
git workflow
debug loop
local reasoning flow
```

它的核心循环更像：

```text
Human intent
  -> Agent reads context
  -> Agent proposes / edits / runs
  -> Human observes
  -> Human interrupts / approves / redirects
  -> Execution feedback
  -> Next step
```

这个 loop 的价值不是单次吞吐，而是：

```text
control
visibility
trust building
exploration
debugging
local context grounding
```

Claude Code 的安全设计也支撑这个 cockpit 味道。

官方文档强调它默认采用严格的 read-only 权限；当需要编辑文件、运行测试、执行命令时，会请求用户显式许可。它还支持 sandboxed bash、写入范围限制、allowlist、Accept Edits mode 等机制。

这说明 Claude Code 的默认产品哲学是：

```text
agent 可以很强，但人类应该清楚它正在做什么，以及什么时候授权它继续。
```

### Claude Code 适合什么任务

Claude Code 特别适合：

```text
1. 不知道问题在哪里，需要探索 repo
2. 需要一边读代码一边推理
3. bug 很隐蔽，需要本地运行和反复调试
4. 修改风险高，人类需要持续在环
5. 开发者想学习代码库，而不是只要最终 diff
6. 信任关系需要通过可见过程建立
```

它像驾驶舱，不是因为它不能自动执行，而是因为它默认让人留在操作面板前。

### Claude Code 的工程难点

做一个 Claude-style harness，难点不是聊天 UI。

难点是：

```text
terminal UX
file editing safety
shell approval policy
local sandbox
repo indexing
context compression
diff visualization
command explanation
interrupt / resume
session trace
MCP permissions
workspace memory
```

真正难的是：

```text
既让 agent 足够能干，又不让用户失去控制感。
```

## Codex: Factory-first Harness

Codex 的产品 taste，尤其是 Codex web / cloud，是：

```text
delegate work to an agent worker
```

它的核心循环更像：

```text
Task brief
  -> Cloud environment
  -> Agent execution
  -> Tests / self-check
  -> Diff / PR / answer
  -> Human review
  -> Iterate or merge
```

OpenAI 官方文档直接使用 `Delegate to Codex in the cloud` 这个 framing，并说明 Codex cloud 可以在后台执行任务，包括并行执行。Codex 还可以连接 GitHub，让它在 repo 上工作并创建 PR。

这就是 factory-first 的核心：

```text
用户定义任务。
agent 在隔离环境里执行。
输出变成可 review 的工程 artifact。
```

Codex 不是纯黑箱。

Codex CLI 可以在本地终端交互运行。Codex 的 sandbox 与 approval policy 也能控制它什么时候自动继续，什么时候停下来请求批准。官方文档里，默认权限是 workspace-write + on-request：可以在 workspace 内读写和运行常规命令，但使用 internet 或越过 workspace boundary 时会请求确认。

所以更准确的说法是：

```text
Codex is factory-first by default in cloud workflows,
but it can support cockpit-like interaction and clarification loops through CLI, task spec, planning, sandbox, approvals, and review.
```

### Codex 适合什么任务

Codex 特别适合：

```text
1. 任务边界清楚
2. 输出可以定义成 diff / PR / review
3. 多个任务可以并行跑
4. repo 环境可以标准化 setup
5. 团队希望扩大工程吞吐
6. 人类更想 review artifact，而不是每一步陪跑
```

它像工厂，不是因为它没有交互，而是因为它默认把 agent 当作一个可以接任务、跑环境、交付结果的工程 worker。

### Codex 的工程难点

做一个 Codex-style harness，难点是：

```text
cloud sandbox orchestration
repo clone and sync
dependency setup
environment reproducibility
task queue
parallel worker scheduling
test execution
PR generation
diff review
cost / latency control
security isolation
approval routing
audit logging
```

真正难的是：

```text
让异步交付足够可靠，让人类愿意把真实工程任务交给 agent。
```

## Cockpit vs Factory 总表

| Dimension | Claude Code | Codex |
|---|---|---|
| 默认产品哲学 | Collaborative control | Delegated execution |
| 隐喻 | Cockpit | Factory |
| 用户角色 | Driver / pair engineer | Task owner / reviewer |
| 主要 surface | Terminal / IDE / desktop / web | Web cloud / CLI / IDE / GitHub |
| 默认交互 | 高频、可观察、可打断 | 任务式、可异步、可并行 |
| 控制方式 | 人类持续在环 | task spec + sandbox + review |
| 权限模型 | 默认 read-only，越权请求许可 | sandbox + approval policy |
| 典型输出 | 本地编辑、命令运行、解释、diff | diff、PR、review、任务结果 |
| 最适合 | 探索、调试、理解复杂 repo | PR 生产、批量任务、例行修复、并行吞吐 |
| 主要风险 | 本地权限和命令安全 | 环境 setup、任务歧义、异步调试成本 |
| trust 建立方式 | 过程可见性 | 隔离、测试、review、审计 |

## PM 视角：DeepSeek 不应该只做一个入口

如果 DeepSeek 要做 coding agent harness，最危险的产品误区是只选一个形态：

```text
只做 terminal cockpit
```

或者：

```text
只做 cloud task factory
```

因为真实软件工程任务有不同风险和不确定性。

一个成熟 harness 应该支持多种 execution mode：

```text
1. Cockpit Mode
2. Clarify-before-Execute Mode
3. Delegated Worker Mode
4. Review-only Mode
5. Autonomous Batch Mode
6. Research / Planning Mode
```

### 1. Cockpit Mode

```text
高频交互，本地执行，人类持续在环。
```

适合：

```text
debugging
repo exploration
high-risk refactor
learning a codebase
security-sensitive changes
```

产品设计重点：

```text
visible tool calls
interrupt button
approve command
diff preview
rollback
session notes
```

### 2. Clarify-before-Execute Mode

```text
agent 先读任务，判断歧义与风险，再问关键问题。
```

适合：

```text
需求不完整
成功标准不清
影响范围未知
权限边界不清
需要 PM / TL 决策
```

关键是不要让 agent 为了显得主动而盲动。

最低要求：

```text
If uncertainty_score > threshold:
  stop
  ask 1-3 blocking questions
  propose execution options
```

### 3. Delegated Worker Mode

```text
人类提交任务，agent 在隔离环境执行，返回 artifact。
```

适合：

```text
well-scoped bug fix
test generation
dependency update
small feature
documentation update
migration milestone
```

核心是：

```text
task spec -> cloud sandbox -> verification -> diff -> PR
```

### 4. Review-only Mode

```text
只读分析，不写文件。
```

适合：

```text
code review
architecture review
security review
onboarding
technical decision support
```

Review-only 是重要模式。

它降低风险，也能建立信任。

### 5. Autonomous Batch Mode

```text
多个 worker 并行跑一批任务。
```

适合：

```text
lint fix
test expansion
docs update
codemod
large migration
issue triage
```

Batch mode 必须强依赖：

```text
task queue
worker isolation
dedupe
failure grouping
cost cap
human approval gate
```

### 6. Research / Planning Mode

```text
agent 不直接改代码，而是读 repo、形成 plan、估计风险、给出 milestone。
```

适合：

```text
large refactor
new module design
infra migration
paper-to-code implementation
benchmark design
```

这对 DeepSeek 很重要。

因为国产和企业场景里，很多需求不是一个小 patch，而是复杂系统改造。直接上手改，失败率会很高。

## DeepSeek Harness 的核心产品命题

DeepSeek 的机会不是只说：

```text
我们的模型便宜。
```

便宜是优势，但不是完整产品。

真正的产品命题应该是：

```text
DeepSeek Coding Agent Harness =
  cost-efficient model execution
  + enterprise-friendly deployment
  + local cockpit
  + cloud / private-cloud workers
  + strict permissions
  + Chinese / English developer workflow support
  + evaluation and trace as first-class objects
```

也就是：

```text
低成本推理只是底座。
真正的 moat 是 harness。
```

## 建议的 DeepSeek Harness 架构

我会把 DeepSeek Coding Agent Harness 拆成八层：

```text
1. Surface Layer
2. Task Contract Layer
3. Context Layer
4. Mode Router
5. Tool Runtime
6. Permission / Sandbox Layer
7. Evaluation / Verification Layer
8. Artifact / Review / Memory Layer
```

### 1. Surface Layer

入口不应该只有一个。

至少需要：

```text
CLI
IDE extension
Web task board
GitHub / GitLab integration
Chat / enterprise IM integration
API / SDK
```

不同入口服务不同 mode：

| Surface | 更适合的模式 |
|---|---|
| CLI | Cockpit / Review / Research |
| IDE | Cockpit / Clarify / Diff review |
| Web | Delegated Worker / Batch / Task board |
| GitHub / GitLab | PR review / Issue to PR |
| IM | lightweight task creation / status update |
| SDK | enterprise automation |

### 2. Task Contract Layer

所有任务必须先变成 contract。

```yaml
task_id: DS-HARNESS-001
task_type: bugfix
repo: deepseek-harness-demo
objective: Fix failing parser test without changing public API.
success_criteria:
  - pytest tests/parser/test_edge_cases.py passes
  - no public API signature changes
  - changelog note if behavior changes
risk_level: medium
mode_hint: clarify-before-execute
allowed_tools:
  - file_read
  - file_write_workspace
  - shell_pytest
forbidden_actions:
  - git_push_direct
  - delete_user_files
  - network_access
human_review_required: true
```

没有 Task Contract，agent 就会退化成聊天。

### 3. Context Layer

Context Layer 要回答：

```text
agent 应该读什么？
不应该读什么？
哪些信息是新鲜的？
哪些信息来自用户？
哪些信息来自 repo？
哪些信息来自长期 memory？
```

最小组件：

```text
RepoMap
SymbolIndex
DependencyGraph
RecentDiffs
IssueContext
DocsContext
TestFailureContext
MemoryRecall
```

DeepSeek 如果要打企业场景，context layer 必须能进入：

```text
monorepo
private docs
internal API
CI logs
issue tracker
knowledge base
```

但这必须通过权限和审计管理。

### 4. Mode Router

Mode Router 是这篇文章最重要的设计点。

输入：

```text
task uncertainty
risk level
context completeness
repo sensitivity
user preference
expected artifact
deadline / throughput need
```

输出：

```text
Cockpit
Clarify-before-Execute
Delegated Worker
Review-only
Autonomous Batch
Research / Planning
```

一个简单规则：

```python
def route(task):
    if task.risk_level == "high":
        return "cockpit"
    if task.uncertainty_score > 0.45:
        return "clarify_before_execute"
    if task.expected_artifact == "review":
        return "review_only"
    if task.batch_size > 1 and task.risk_level in ["low", "medium"]:
        return "autonomous_batch"
    if task.scope == "large_refactor":
        return "research_planning"
    return "delegated_worker"
```

真实系统会更复杂，但这个方向是对的。

因为成熟 harness 不应该把所有任务塞进同一种交互。

### 5. Tool Runtime

Tool Runtime 是 agent 的手。

最小工具：

```text
file read / write
shell
test runner
git diff
package manager
search
browser / docs
MCP tools
issue tracker
PR API
```

工具不能只是能调。

必须有 contract：

```yaml
tool_name: shell_pytest
capability: run_python_tests
input_schema:
  command: string
permission: workspace_command
timeout_seconds: 120
network: false
side_effect: read_and_temp_write
validation:
  - parse_exit_code
  - capture_test_summary
fallback:
  - run_single_test_file
```

没有 Tool Contract，就没有可靠 agent execution。

### 6. Permission / Sandbox Layer

这里要同时学习 Claude 和 Codex。

Claude 的启示：

```text
默认保守。
需要修改和命令时显式授权。
人类要能看见和批准。
```

Codex 的启示：

```text
用 sandbox 给 agent 一个可自动工作的边界。
边界内低摩擦执行。
越界就进入 approval flow。
```

DeepSeek Harness 的推荐默认值：

```text
read repository: allowed
write workspace: allowed only in task branch
run tests: allowed for safe commands
network: disabled by default
package install: approval required
delete files: approval required
git push: approval required
production secrets: never exposed
```

### 7. Evaluation / Verification Layer

这是 DeepSeek 如果要做得严肃，必须重视的一层。

Coding agent 的评测不能只看：

```text
回答是否像样
```

必须看：

```text
tests pass?
diff minimal?
public API stable?
regression risk?
lint pass?
type check pass?
security issue?
human accepts?
rollback available?
```

一个最小 VerificationReport：

```yaml
verification_id: VR-001
task_id: DS-HARNESS-001
commands:
  - pytest tests/parser/test_edge_cases.py
result: pass
changed_files:
  - src/parser/core.py
  - tests/parser/test_edge_cases.py
api_change: false
risk_summary: Low public API risk. Parser branch narrowed.
human_review_required: true
```

### 8. Artifact / Review / Memory Layer

输出应该是 artifact，不是聊天记录。

最小 artifact：

```text
Plan
Diff
Test Log
Verification Report
Risk Note
PR Description
Review Decision
Memory Candidate
```

Memory 也不能随便写。

只能写：

```text
稳定事实
项目规则
踩坑经验
可复用模式
```

不能写：

```text
私密代码片段
临时猜测
用户敏感信息
未经确认的偏好
```

## 工程预演：一个 DeepSeek Coding Agent Harness v0

现在做一个工程级预演。

任务：

```text
修复一个 parser edge case bug。
要求不改变 public API。
要求测试通过。
要求输出 PR。
```

### Step 1: Intake

用户输入：

```text
Fix parser failure when input has trailing comma in nested call.
Do not change public API.
Add regression test.
```

系统生成：

```text
TaskSpec
```

并自动计算：

```text
risk_level = medium
uncertainty_score = 0.38
recommended_mode = delegated_worker
human_review_required = true
```

如果 uncertainty_score 高于阈值，则进入 clarification：

```text
I need to confirm:
1. Should trailing comma be accepted in all nested calls or only function calls?
2. Is this behavior documented anywhere?
3. Should old invalid-input errors remain unchanged?
```

### Step 2: Context Build

系统构建上下文：

```text
RepoMap:
  parser/
  tests/parser/
  docs/syntax.md

SymbolIndex:
  parse_call_expr
  parse_args
  ParserError

TestFailureContext:
  failing test output

RecentDiffs:
  last parser-related changes
```

### Step 3: Plan

agent 生成 plan：

```text
1. Reproduce failing parser test.
2. Locate argument-list parsing code.
3. Add regression test for nested trailing comma.
4. Modify parser branch narrowly.
5. Run parser tests.
6. Summarize risk and create PR description.
```

系统进入 plan review gate：

```text
If mode = cockpit:
  show plan and wait for user

If mode = delegated_worker:
  proceed inside sandbox

If mode = clarify-before-execute:
  ask blocking questions first
```

### Step 4: Execute in Sandbox

worker 执行：

```text
git checkout -b agent/parser-trailing-comma
pytest tests/parser/test_edge_cases.py
rg "parse_args|parse_call" src tests
edit src/parser/core.py
edit tests/parser/test_edge_cases.py
pytest tests/parser/test_edge_cases.py
pytest tests/parser
git diff
```

所有 tool call 进入 trace：

```json
{"step": 1, "tool": "shell", "cmd": "pytest tests/parser/test_edge_cases.py", "exit": 1}
{"step": 2, "tool": "search", "query": "parse_args|parse_call", "matches": 4}
{"step": 3, "tool": "file_edit", "path": "src/parser/core.py", "summary": "Allow trailing comma before closing paren"}
{"step": 4, "tool": "shell", "cmd": "pytest tests/parser", "exit": 0}
```

### Step 5: Verify

VerificationReport：

```yaml
test_status: pass
changed_files:
  - src/parser/core.py
  - tests/parser/test_edge_cases.py
api_change: false
behavior_change: accepts nested trailing comma
risk: medium-low
needs_human_review:
  - grammar compatibility
  - error message stability
```

### Step 6: Deliver

输出 PR artifact：

```text
Title:
Fix nested parser trailing-comma edge case

Summary:
- Add regression coverage for nested call with trailing comma.
- Narrow parser branch to accept trailing comma before closing paren.
- Keep public API unchanged.

Verification:
- pytest tests/parser/test_edge_cases.py
- pytest tests/parser

Risk:
- Behavior expands accepted syntax in one parser branch.
- Human reviewer should confirm this matches language spec.
```

### Step 7: Review and Memory

人类 review 后：

```text
approved / request changes / reject
```

如果通过，写入 memory candidate：

```yaml
memory_type: repo_rule
scope: parser
content: Parser changes require regression tests in tests/parser/test_edge_cases.py and syntax compatibility review.
evidence:
  - PR #123
retention: project
```

这就是最小闭环：

```text
TaskSpec
  -> Context
  -> Mode Router
  -> Plan
  -> Sandbox Execution
  -> Trace
  -> Verification
  -> PR Artifact
  -> Human Review
  -> Memory
```

## DeepSeek Harness MVP 路线

不要一开始做巨大平台。

建议分四个版本。

### v0.1: Local Cockpit

目标：

```text
DeepSeek CLI can read repo, edit files, run tests, show diff, request approval.
```

必须有：

```text
TaskSpec
local workspace
file edit
shell runner
git diff
approval prompt
trace log
```

不要急着做：

```text
multi-agent
cloud batch
enterprise dashboard
full memory system
```

### v0.2: Cloud Worker

目标：

```text
User submits task, worker runs in isolated environment, returns diff and report.
```

必须有：

```text
repo clone
env setup
branch creation
test runner
diff artifact
PR draft
cost cap
worker timeout
```

### v0.3: Mode Router

目标：

```text
Same product supports cockpit, clarify, delegated, review-only, batch.
```

必须有：

```text
uncertainty score
risk score
mode selection
mode override
human approval policy
```

这是从工具走向 harness OS 的关键版本。

### v0.4: Eval and Trace Dashboard

目标：

```text
Measure whether the agent actually improves engineering delivery.
```

必须有：

```text
task success rate
test pass rate
human accept rate
rollback rate
average cost
average latency
failure taxonomy
tool error rate
security approval events
```

没有 evaluation，coding agent 产品会很快陷入 demo 好看、真实不可用的问题。

## DeepSeek 可以打出的差异化

DeepSeek 不应该只在模型层讲故事。

Harness 层有更现实的差异化。

### 1. Cost-efficient agent execution

Coding agent 的成本不是一次回答。

它是：

```text
long context
tool calls
test loops
retry
review
parallel workers
```

如果 DeepSeek 能把单位任务成本打下来，factory mode 的可用性会明显提高。

### 2. Private / enterprise deployment

很多企业不愿意把代码、CI log、内部文档、issue tracker 全部放到外部云。

DeepSeek 可以重点做：

```text
private cloud
on-prem
VPC deployment
local model gateway
enterprise permission policy
audit log
```

### 3. Chinese developer workflow

中国工程团队的工作流可能包括：

```text
企业微信
飞书
钉钉
GitLab
Gitee
内部 Jira-like system
私有 CI
中文需求文档
中英混合代码注释
```

这不是小事。

这是产品入口和 context layer 的差异化。

### 4. Research + Coding 一体

DeepSeek 的长期机会不是只做代码修复。

更大的方向是：

```text
research question
  -> paper / doc reading
  -> implementation plan
  -> code
  -> experiment
  -> report
```

这会连接我们前面说的：

```text
Research Task Harness
Agent Execution Harness
Tool Harness
Memory Harness
```

也就是 AI Scientist 和 Research OS 的方向。

## 关键风险

### 1. 只做模型，不做 harness

模型强，但没有：

```text
permissions
trace
eval
workflow
artifact
review
```

就无法进入真实工程组织。

### 2. 只做 demo，不做 setup

Coding agent 的失败经常不是因为不会写代码。

而是：

```text
dependency 装不上
测试跑不起来
环境变量缺失
CI 和本地不同
repo 太大
上下文取错
```

所以 environment setup 是核心产品功能，不是边角料。

### 3. 只追求自动化，不处理 uncertainty

不确定任务应该先问。

如果强行执行，会制造错误 diff。

所以 DeepSeek Harness 必须把 uncertainty 作为一等公民。

### 4. 只看 pass rate，不看 accept rate

测试通过不等于业务接受。

真实指标应该包括：

```text
human accept rate
review comment count
rollback rate
time-to-merge
post-merge incident
```

## 最终启示

从 Claude Code 学：

```text
local cockpit
interactive control
permission-first trust
developer flow
```

从 Codex 学：

```text
cloud worker
delegated throughput
sandbox boundary
PR artifact
parallel execution
review-driven delivery
```

DeepSeek 应该做：

```text
mode-aware agent execution OS
```

也就是：

```text
DeepSeek Coding Agent Harness =
  Cockpit for high-control work
  + Clarify-first for ambiguous work
  + Cloud worker for delegated work
  + Review-only for audit work
  + Batch mode for scale work
  + Eval / trace / permission / memory as shared system layer
```

## 一句话收束

```text
Claude Code teaches control.
Codex teaches delegation.
DeepSeek Harness should teach execution governance.
```

如果 `PENGYI_HARNESS_MAP000` 讲的是 harness 总抽象，

那么这一篇 `PENGYI_HARNESS001` 的核心结论就是：

```text
真正的 coding agent moat 不只是模型，
而是让模型在真实软件工程里可控、可测、可交付、可复用的 harness。
```

## 参考资料

- OpenAI Codex Web: <https://developers.openai.com/codex/cloud>
- OpenAI Codex CLI: <https://developers.openai.com/codex/cli>
- OpenAI Codex Sandbox: <https://developers.openai.com/codex/concepts/sandboxing>
- OpenAI Codex Workflows: <https://developers.openai.com/codex/workflows>
- OpenAI Codex Agent Approvals & Security: <https://developers.openai.com/codex/agent-approvals-security>
- Anthropic Claude Code Overview: <https://code.claude.com/docs/en/overview>
- Anthropic Claude Code Security: <https://code.claude.com/docs/en/security>
