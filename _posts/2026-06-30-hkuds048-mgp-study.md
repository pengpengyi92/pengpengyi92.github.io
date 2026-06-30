---
title: "HKUDS048: MGP 作为 Governed Agent Memory Protocol、Policy/Audit Layer 与 Research OS Memory Governance Layer"
date: 2026-06-30 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds048, hkuds, mgp, memory-governance, agent-memory, protocol, audit, policy, research-os, quant-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的 `HKUDS048`。

```text
HKUDS048 -> MGP
```

上一篇是：

```text
HKUDS047 -> SepLLM
```

`SepLLM` 讲的是：

```text
long context compression
```

这一篇看 `MGP`。

`MGP` 讲的是：

```text
governed persistent memory
```

一句话定位：

```text
MGP = Memory Governance Protocol
    = write / recall / govern / audit persistent memory
    = one protocol, many memory backends
    = policy context + lifecycle + audit + adapters + compliance
```

更直白一点：

```text
MCP 是 agent 调工具的协议。
MGP 是 agent 管记忆的协议。
```

MGP README 里的判断非常关键：

```text
Use MCP for action.
Use MGP for memory.
```

这篇进入的是：

```text
Agent Memory Governance / Protocol Infrastructure / Audit / Policy / Lifecycle
```

这条线和我们之前三篇连得非常紧：

```text
HKUDS045 CatchMe       -> record personal digital footprint
HKUDS046 LightReasoner -> select high-value reasoning signals
HKUDS047 SepLLM        -> compress long context into bounded memory budget
HKUDS048 MGP           -> govern persistent memory through protocol, policy, audit, lifecycle
```

如果说 `CatchMe` 解决的是：

```text
记什么？
```

`SepLLM` 解决的是：

```text
怎么压缩？
```

那么 `MGP` 解决的是：

```text
谁可以写？
谁可以读？
记忆如何过期？
记忆如何撤回？
记忆如何删除？
记忆如何审计？
不同 backend 如何保持同一个 contract？
```

这就是从 memory engineering 进入 memory governance。

## 为什么 HKUDS048 看 MGP

我们一直在搭 `Pengyi Research OS` 和 `Quant Research OS`。

前面我们已经有：

```text
LightRAG       -> research knowledge memory
MiniRAG        -> lightweight memory
RAG-Anything   -> multimodal ingestion
VideoRAG       -> video memory
CatchMe        -> personal trace memory
SepLLM         -> bounded context compression
LightReasoner  -> high-signal reasoning selection
```

但这些还缺一个后台治理层。

任何真正长期运行的 agent memory 系统，都会遇到这些问题：

```text
1. agent 能不能自动记住用户偏好？
2. 用户能不能撤回某条记忆？
3. 敏感记忆能不能只返回 metadata？
4. 团队共享记忆和个人记忆怎么隔离？
5. 不同 memory backend 能不能切换？
6. 搜索结果能不能解释来源？
7. 每一次读写有没有 audit trail？
8. 记忆过期、撤回、软删除、硬删除是不是不同语义？
```

如果没有协议，这些问题会散落在各个 app 里。

结果就是：

```text
每个 agent 自己发明一套 memory API。
每个 backend 自己定义一套字段。
每个 runtime 自己处理权限和审计。
最后无法迁移、无法治理、无法合规、无法复用。
```

`MGP` 做的是把这些问题收束成一个协议层。

这对我们非常重要。

因为我们的 `Research OS` 不是短期 demo。

它会长期记录：

```text
学习轨迹
论文阅读
代码修改
回测实验
导师沟通
求职 pitch
私人计划
quant factor research
PM review
contract / credit / cashflow planning
```

这些东西不能只是“存进去”。

它们必须被治理。

## 本次阅读状态

本次阅读的是本地 HKUDS 工作区里的 `MGP`。

阅读前已经同步远端，当前是远端最新。

| item | value |
| --- | --- |
| repo | `MGP` |
| remote | `https://github.com/HKUDS/MGP.git` |
| branch | `main` |
| latest commit | `54ce6c00e3d0aa731ecbe17e74407cbbb5a96f10` |
| commit date | `2026-04-25T16:35:25+08:00` |
| latest message | `docs(oceanbase): document oceanbase adapter alongside postgres baseline` |
| protocol version | `v0.1.1` |
| Python | `3.11+` |
| license | MIT |
| tracked files | 309 |
| Python files | 122 |
| Markdown files | 83 |
| JSON files | 71 |
| YAML files | 12 |
| local size | about `1.41 MB` |

目录规模：

| directory | size | files | role |
| --- | ---: | ---: | --- |
| `spec` | about `0.08 MB` | 16 | protocol semantics |
| `schemas` | about `0.09 MB` | 63 | JSON Schema contracts |
| `openapi` | about `0.02 MB` | 2 | HTTP binding |
| `reference` | about `0.11 MB` | 25 | FastAPI reference gateway |
| `adapters` | about `0.18 MB` | 41 | memory backend adapters |
| `sdk` | about `0.06 MB` | 21 | Python SDK |
| `compliance` | about `0.09 MB` | 19 | conformance tests |
| `integrations` | about `0.12 MB` | 33 | Nanobot / LangGraph / minimal runtime bridges |
| `examples` | about `0.02 MB` | 13 | runnable examples |
| `docs` | about `0.18 MB` | 50 | documentation site |

这个 repo 很小，但结构很完整。

它不是模型仓库，也不是产品 app。

它是一个 protocol-first repository。

## 项目一句话

MGP README 的定义是：

```text
MGP standardizes how AI runtimes write, recall, govern, and audit persistent memory across heterogeneous backends.
```

拆开就是：

```text
write    -> 写入记忆
recall   -> 搜索/召回记忆
govern   -> 根据 policy context 决定能否读写、如何返回
audit    -> 记录每次状态变化和访问
backend  -> 允许不同 memory store 插在后面
```

这不是简单 CRUD。

普通 CRUD 是：

```text
create / read / update / delete
```

MGP 的记忆生命周期是：

```text
Write
Search
Get
Update
Expire
Revoke
Delete
Purge
BatchWrite
AuditQuery
Export
Import
Sync
```

这里最关键的是：

```text
Expire != Revoke != Delete != Purge
```

这就是治理语义。

## MGP 和 MCP

MGP 最容易理解的方式是和 MCP 对比。

| Dimension | MCP | MGP |
| --- | --- | --- |
| focus | tools and resources | governed persistent memory |
| protocol surface | tool invocation, prompt templates, resource discovery | memory CRUD, policy context, audit, lifecycle, conflict resolution |
| data model | tools, prompts, resources | memory objects, candidates, recall intents, audit events |
| governance | not in scope | policy context, access control, retention |
| lifecycle | not in scope | expire, revoke, delete, purge |
| audit | not in scope | audit trail and lineage |
| architecture level | runtime to external capabilities | runtime to memory backends |

一句话：

```text
MCP lets agents act.
MGP lets agents remember responsibly.
```

这对我们做 Research OS 非常重要。

因为一个 research agent 需要两种能力：

```text
action layer:
  run code
  call browser
  query database
  execute backtest
  create PR

memory layer:
  remember project facts
  recall prior decisions
  store experiment results
  respect privacy and retention
  audit what was read or written
```

所以未来系统形态应该是：

```text
Runtime
  -> MCP client -> tool/resource servers
  -> MGP client -> memory gateway/backends
```

这也是 agent 变成长期组织生产力时必须补齐的部分。

## 总体架构

MGP 的架构可以拆成几层：

```text
Agent Runtime
  -> SDK / Sidecar
  -> MGP Gateway
  -> Policy Hook
  -> Audit Sink
  -> Adapter Router
  -> Memory Backend
```

repo 结构正好对应这个架构：

| layer | paths | role |
| --- | --- | --- |
| protocol semantics | `spec/` | 定义操作语义和兼容边界 |
| machine contracts | `schemas/`, `openapi/` | JSON Schema 和 HTTP binding |
| reference gateway | `reference/gateway/`, `reference/policy/`, `reference/audit/` | FastAPI gateway、policy、audit、async task |
| backend normalization | `adapters/` | 把不同 backend 归一成 MGP adapter contract |
| runtime integration | `sdk/python/`, `integrations/` | SDK、sidecar、Nanobot/LangGraph/minimal runtime bridge |
| verification | `compliance/`, `examples/` | conformance tests 和 runnable examples |
| docs | `docs/`, `README.md` | 解释、使用、扩展 |

这类 repo 的核心不是某个算法文件，而是：

```text
contract stack
```

MGP 的 contract stack 是：

```text
spec/       -> semantic truth
schemas/    -> machine-readable payload shape
openapi/    -> HTTP wire surface
reference/  -> executable behavior
compliance/ -> conformance proof
docs/       -> user-facing explanation
```

这非常值得我们学习。

因为我们以后做开源 project，也要从“能跑”升级到：

```text
有 spec
有 schema
有 reference implementation
有 examples
有 tests
有 docs
有 compliance story
```

这才像真正可被别人依赖的基础设施。

## Reference Gateway

MGP 的 reference gateway 是 FastAPI。

入口是：

```text
reference/gateway/app.py
```

它做的事情很干净：

```text
app = FastAPI(...)
app.add_middleware(GatewayAuthMiddleware)
app.add_middleware(RequestContextMiddleware)
app.include_router(operational_router)
app.include_router(memory_router)
app.include_router(protocol_router)
```

也就是说，它不是把所有逻辑塞在一个 app 文件里。

它拆成：

| component | role |
| --- | --- |
| `routes/memory.py` | memory operations: write/search/get/update/expire/revoke/delete/purge |
| `routes/protocol.py` | capabilities, initialize, tasks, audit query, import/export/sync |
| `routes/operational.py` | health, readiness, version |
| `operations.py` | protocol capabilities, negotiation, audit event, write execution |
| `semantics.py` | candidate to memory, merge, recall normalization, result item building |
| `router.py` | adapter dispatch |
| `validation.py` | schema validation |
| `policy/hook.py` | policy decision and transformation |
| `audit/sink.py` | JSONL audit sink |

一个典型的 `WriteMemory` request flow 是：

```text
runtime
  -> POST /mgp/write
  -> validate request schema
  -> evaluate policy context
  -> execute write / merge / dedupe
  -> dispatch to adapter
  -> transform return view if needed
  -> emit audit event
  -> validate response schema
  -> return response envelope
```

这就是一个成熟 backend protocol 的形状。

## Memory Routes

`reference/gateway/routes/memory.py` 是最重要的运行时入口。

它实现：

```text
POST /mgp/write
POST /mgp/search
POST /mgp/get
POST /mgp/update
POST /mgp/expire
POST /mgp/revoke
POST /mgp/delete
POST /mgp/purge
POST /mgp/write/batch
```

每个 endpoint 都有相似的结构：

```text
1. extract request_id
2. validate action request
3. enforce tenant binding
4. call policy hook
5. dispatch adapter operation
6. emit audit event
7. validate response
8. return envelope
```

这对我们做 Research OS 的启发是：

```text
memory should not be raw function calls.
memory should be governed operations.
```

也就是说，我们不能只写：

```python
memory_store.append(note)
```

我们应该有：

```text
policy_context
memory_object
operation
audit_event
retention_policy
return_mode
```

## Policy Context

MGP 的核心不是只定义 memory object。

它还要求每个 request 带上：

```text
policy_context
```

Schema 里必须有：

```text
actor_agent
acting_for_subject
requested_action
```

可选字段包括：

```text
task_id
session_id
task_type
risk_level
channel
chat_id
data_zone
tenant_id
runtime_id
runtime_instance_id
correlation_id
consent_basis
assertion_origin
```

这很关键。

因为 memory governance 不是只看“这条记忆是什么”。

还要看：

```text
谁在操作？
代表谁操作？
在什么任务里操作？
风险等级是什么？
属于哪个 tenant？
来自哪个 runtime？
有没有用户 consent？
有没有 trace id？
```

这和公司后台视角很像。

财务、法务、人力这些后台，不一定直接做前台业务，但它们必须知道：

```text
谁在申请？
代表哪个部门？
合同主体是谁？
风险等级是什么？
审批链在哪里？
留痕在哪里？
```

MGP 把这种后台治理逻辑放进 agent memory。

这就是为什么它对 AI organization 很重要。

## Policy Hook

`reference/policy/hook.py` 是一个 minimal policy hook。

它不是完整授权系统，但它给了 stable contract。

policy decision shape 是：

```json
{
  "decision": "allow | deny | redact | summarize",
  "reason_code": "string",
  "applied_rules": ["string"],
  "return_mode": "raw | summary | masked | metadata_only"
}
```

reference policy 里已经实现了几个基本规则：

```text
tenant mismatch        -> deny
soft deleted memory    -> deny or metadata_only
ttl expired            -> deny
restricted sensitivity -> metadata_only
confidential           -> masked
otherwise              -> raw
```

返回模式非常关键：

| return mode | meaning |
| --- | --- |
| `raw` | 返回完整记忆 |
| `summary` | 返回摘要 |
| `masked` | 敏感字段脱敏 |
| `metadata_only` | 只返回 schema-valid 的 metadata-safe view |

这对我们的 Research OS 有直接启发。

例如：

```text
公开网站可以读 public artifact_summary。
私有 repo 可以读 internal research notes。
求职 pitch 可以读 sanitized career story。
Quant factor raw data 只能返回 metadata or summary。
个人计划、合同、信用、现金流相关内容必须 confidential。
```

未来我们不能让所有 agent 对所有 memory 都 raw access。

必须有 return mode。

## Audit Sink

MGP 的 audit 非常直接：

```text
reference/audit/sink.py
```

默认是 JSON Lines。

每个 audit event 包含：

```text
event_id
timestamp
request_id
actor
action
target_memory_id
policy_context
decision
backend
lineage_refs
result_count
correlation_id
```

这意味着：

```text
每一次 memory write / read / search / update / delete 都可以追踪。
```

这点对长期 agent 非常关键。

没有 audit 的 memory，是危险的。

因为 agent 会慢慢形成长期行为偏好：

```text
它为什么记住这个？
它什么时候记住的？
谁允许它记住的？
它根据哪条记忆做出判断？
这条记忆后来有没有被撤回？
```

这些问题都需要 audit。

对我们的 Quant Research OS 来说也是一样。

例如：

```text
某个 factor 被标记为 data leakage。
某个回测结果被标记为 invalid。
某个模型参数被 PM reject。
某个行业中性化假设被撤回。
```

这些都不能只靠口头记忆。

必须能审计。

## Memory Object

MGP 的 canonical memory object 是：

```text
memory_id
subject
scope
type
content
source
created_at
```

常见可选字段：

```text
confidence
sensitivity
retention_policy
ttl_seconds
updated_at
assertion_mode
asserted_by
confirmed_by_user
evidence_refs
evidence
derived_from
valid_from
valid_to
backend_ref
extensions
```

`scope` 枚举：

```text
user
session
task
agent
org
shared_team
```

`type` 枚举：

```text
profile
preference
episodic_event
semantic_fact
procedural_rule
relationship
checkpoint_pointer
artifact_summary
```

这几个类型对我们非常有用。

可以映射成 Research OS：

| MGP type | Research OS example |
| --- | --- |
| `profile` | researcher profile, agent profile, project identity |
| `preference` | writing style, coding preference, review preference |
| `episodic_event` | meeting, interview, experiment run, debugging session |
| `semantic_fact` | paper insight, repo fact, market fact |
| `procedural_rule` | backtest protocol, PR checklist, publish workflow |
| `relationship` | person-project, factor-dataset, paper-method relation |
| `checkpoint_pointer` | model checkpoint, experiment artifact, commit |
| `artifact_summary` | report summary, backtest summary, blog summary |

这就是一个非常好的 memory schema seed。

我们不一定直接用 MGP，但是它的类型体系很适合借鉴。

## Memory Candidate

MGP 还有一个非常有意思的对象：

```text
memory_candidate
```

它表示 runtime 侧先提出一个候选记忆。

字段包括：

```text
candidate_kind
subject
scope
proposed_type
statement
source
source_evidence
confidence
sensitivity
retention_policy
ttl_seconds
merge_hint
extensions
```

`candidate_kind` 包括：

```text
assertion
confirmation
correction
derived
```

这非常适合 agent 工作流。

agent 不应该每次都直接写 canonical memory。

更好的流程是：

```text
agent extracts memory candidate
  -> attach confidence and evidence
  -> propose type and scope
  -> send merge hint
  -> policy / gateway decides create, merge, reinforce, dedupe, manual review
```

这对我们的 R&D Agent 极其重要。

比如 quant research 中：

```text
candidate:
  statement: "factor X shows unstable IC after industry neutralization"
  proposed_type: semantic_fact
  source: backtest_run_2026_06_30
  evidence: IC table + chart + code commit
  confidence: 0.72
  sensitivity: internal
  merge_hint:
    strategy: merge
    dedupe_key: factor_x_industry_neutral_ic
```

这比简单写一条 note 强很多。

## Merge 和 Conflict

MGP 的 `gateway/semantics.py` 里有几个关键函数：

```text
memory_from_candidate
locate_existing_memory
merge_memory
build_result_item
```

`merge_memory` 支持：

```text
create
upsert
replace
merge
reinforce
dedupe
manual_review_required
```

如果冲突不能安全解决，就返回：

```text
MGP_CONFLICT_UNRESOLVED
```

这对长期记忆系统很重要。

因为 memory 不只是追加。

它会出现：

```text
旧事实被新事实纠正
用户偏好变化
实验结论被推翻
数据问题导致历史结论 invalid
不同 agent 写入互相矛盾
```

所以必须有 conflict semantics。

我们的 Research OS 以后也应该区分：

```text
reinforce: 又一次证据支持这个结论
merge: 把新的 evidence 加进去
replace: 原结论被新结论替代
manual_review_required: 需要人类 PM 审核
```

这和我们设计 R&D Agent 的人类 PM 审核点完全一致。

## Lifecycle Semantics

MGP 对 lifecycle 的区分很值得学。

| operation | meaning |
| --- | --- |
| `ExpireMemory` | 记忆过期，不再默认搜索，但仍可审计 |
| `RevokeMemory` | 撤回记忆，不再正常使用，但不等于删除 |
| `DeleteMemory` | 软删除，留下 tombstone |
| `PurgeMemory` | 硬删除，从正常存储路径移除 |

这四个不能混用。

在 Research OS 里也一样。

例如：

```text
过期:
  某个 job opening 已经过期

撤回:
  某个 career narrative 不再采用

软删除:
  某条 pitch 内容不应该再被 agent 使用，但保留审计

硬删除:
  某条私密信息需要彻底清除
```

在 Quant Research OS 里：

```text
过期:
  某个市场 regime insight 已不适用

撤回:
  某个因子假设被数据泄漏问题推翻

软删除:
  某个实验结果不再参与汇总，但保留原因

硬删除:
  某些受限制数据不能再存储
```

这就是治理系统和普通笔记系统的区别。

## Adapter Ecosystem

MGP 的 adapter layer 很关键。

它支持：

| adapter | backend | use case |
| --- | --- | --- |
| `memory` | process memory | testing / development |
| `file` | JSON files | file-native workflows |
| `graph` | SQLite | relationship semantics |
| `postgres` | PostgreSQL | production relational backend |
| `oceanbase` | OceanBase / seekdb | MySQL-compatible production backend |
| `lancedb` | LanceDB | vector / hybrid search |
| `mem0` | Mem0 service | managed memory |
| `zep` | Zep service | graph-native memory |

所有 adapter 都实现同一个 `BaseAdapter`：

```text
write
search
get
update
expire
revoke
delete
purge
list_memories
get_manifest
```

这就是 backend portability。

runtime 不应该直接依赖某个 memory vendor。

runtime 应该依赖：

```text
MGP protocol surface
```

backend 可以换：

```text
in-memory -> file -> postgres -> lancedb -> zep
```

只要符合协议，agent runtime 不用重写 memory 逻辑。

对我们做开源 Research OS 也是一样。

早期可以：

```text
file adapter
```

后面可以：

```text
SQLite / Postgres / LanceDB / LightRAG / custom graph
```

但上层 R&D Agent 不应该直接绑死存储实现。

## Compliance Suite

MGP 很重视 compliance。

`compliance/` 里验证：

```text
schema validity
OpenAPI alignment
contract drift
core operations
lifecycle and retention
conflict behavior
access decision behavior
adapter compatibility
search result contract
dedupe / upsert / merge
delete / purge lifecycle
audit correlation fields
batch / export / import / sync
```

默认 CI 矩阵验证：

```text
memory
file
graph
```

还有可选：

```text
postgres
oceanbase
external services
```

这点很有顶会/开源项目启发。

如果我们以后做 `Pengyi Quant Research OS`，也应该有自己的 compliance suite：

```text
schema validation
artifact contract test
backtest report schema test
factor lifecycle test
human review test
leakage diagnosis contract test
publish workflow test
```

不能只靠 demo。

真正可复用的系统必须可验证。

## Python SDK

MGP 提供 Python SDK。

核心类：

```text
MGPClient
AsyncMGPClient
PolicyContextBuilder
SearchQuery
MemoryCandidate
AuditQuery
```

`MGPClient` 支持：

```text
write_memory
write_candidate
search_memory
get_memory
update_memory
expire_memory
revoke_memory
delete_memory
purge_memory
write_batch
export_memories
import_memories
sync_memories
get_capabilities
initialize
query_audit
get_task
cancel_task
wait_for_task
iter_search_pages
iter_search_results
iter_audit_events
```

SDK 里最实用的是 `PolicyContextBuilder`。

它把：

```text
actor_agent
subject_id
tenant_id
task_id
session_id
risk_level
runtime_id
correlation_id
consent_basis
assertion_origin
```

统一打进 `policy_context`。

这对应用开发很重要。

因为如果每次手写 JSON，治理字段很容易缺失。

SDK 应该把正确行为变成默认路径。

## Nanobot Integration

MGP 和 `nanobot` 的关系也很关键。

MGP 不是只停留在协议层。

它专门做了 Nanobot integration。

Nanobot 是一个合适的集成对象，因为：

```text
1. 它是实际 agent runtime。
2. 它已经支持 MCP。
3. 它有自己的 native memory。
4. 它需要 governed memory peer protocol。
```

集成有两条路径：

| path | role |
| --- | --- |
| Path A: `nanobot[mgp]` extra | production path |
| Path B: harness in MGP repo | validation path |

Path B 里有 rollout modes：

```text
off
shadow
primary
```

含义：

```text
off:
  不调用 MGP

shadow:
  调用 MGP，但不把 recall 注入 prompt

primary:
  调用 MGP，并允许 recall 影响 prompt
```

这是非常成熟的上线思路。

不要一上来就让 memory 影响 agent 决策。

应该先 shadow。

先验证：

```text
recall quality
latency
error rate
wrong memory risk
privacy risk
```

再进入 primary。

这对我们以后接入自己的 Research OS memory 也一样。

先让 agent 写 memory candidate。

然后：

```text
shadow mode:
  写入、搜索、审计都跑，但不影响正式决策

primary mode:
  召回结果进入 agent prompt / plan
```

这就是安全上线。

## Sidecar Pattern

MGP 支持 sidecar integration。

sidecar 的职责是：

```text
inject policy_context
generate request_id
forward requests to gateway
normalize errors back to runtime
forward audit metadata
support off / shadow / primary rollout
```

为什么需要 sidecar？

因为有些 runtime 不好直接改内部代码。

sidecar 可以作为低风险集成路径。

它不要求 runtime 一开始完全重构。

这对我们也很有用。

如果我们未来有：

```text
Codex workflow
GitHub Pages website
Quant backtest scripts
personal notes
R&D Agent runtime
```

我们可以先用 sidecar 接入 memory governance，而不是把每个系统都重写。

## Examples

`examples/` 覆盖了一条完整学习路径：

```text
01_write_profile
02_search_episodic
03_ttl_expiry
04_switch_backend
05_sdk_only
06_gateway_plus_postgres
07_sidecar_shadow_mode
08_batch_export_import
09_task_polling
10_audit_query
e2e_demo
```

这组 examples 很适合作为我们之后贡献 PR 的入口。

因为它们把协议从抽象变成可跑流程：

```text
write -> search -> get -> expire -> audit
```

我们未来可以参考这种编号式 examples 方式。

对自己的 project 也这样写：

```text
01_create_project
02_add_source
03_write_research_memory
04_search_decision
05_expire_old_hypothesis
06_audit_factor_run
07_export_project_memory
```

## MGP 的非目标

MGP 也很清楚自己不做什么。

它不是：

```text
memory store
database replacement
retrieval science framework
policy engine
agent framework
hosted service
static export format
MCP sub-layer
```

这点非常好。

一个协议项目最怕边界失控。

MGP 的边界是：

```text
standardize governed memory interaction
```

不负责：

```text
怎么生成记忆
怎么做 embedding
怎么做 ranking
怎么组织 agent workflow
怎么做 full application state
```

这对我们写系统也有启发。

Research OS 不要一开始就什么都包。

每个模块必须知道自己不做什么。

## 和 CatchMe 的关系

`CatchMe` 负责：

```text
capture personal digital footprint
```

MGP 负责：

```text
govern persistent memory
```

它们可以组合：

```text
CatchMe raw trace
  -> memory extraction
  -> MGP memory candidate
  -> policy decision
  -> canonical memory object
  -> adapter storage
  -> audit trail
```

也就是说：

```text
CatchMe = capture layer
MGP     = governance layer
```

没有 MGP，CatchMe 的长期记忆可能只是个人 activity log。

加上 MGP，它可以变成：

```text
可审计、可撤回、可迁移、可策略化的 agent memory system。
```

## 和 SepLLM 的关系

`SepLLM` 解决的是：

```text
long-context memory budget
```

它的抽象是：

```text
initial anchor + separator memory + local window
```

MGP 解决的是：

```text
persistent memory governance
```

它的抽象是：

```text
memory object + policy context + lifecycle + audit
```

两者可以组合：

```text
SepLLM-style compressor:
  decides what should be promoted from raw context to memory candidate

MGP:
  governs whether and how that candidate becomes persistent memory
```

这就是很完整的一条链：

```text
context stream
  -> compression
  -> memory candidate
  -> policy
  -> persistent memory
  -> recall
  -> prompt-safe view
```

对 Research OS 来说，这条链非常实用。

## 和 LightRAG 的关系

`LightRAG` 是知识检索系统。

它重点是：

```text
documents -> graph / vector / key-value storage -> retrieval
```

MGP 不是替代 LightRAG。

MGP 更像上层治理协议。

未来可以有：

```text
MGP adapter -> LightRAG-backed memory
```

或者：

```text
LightRAG as knowledge backend
MGP as governed access protocol
```

区别是：

```text
LightRAG asks:
  how to retrieve knowledge effectively?

MGP asks:
  how to govern persistent memory operations consistently?
```

两者互补。

## 对 Pengyi Research OS 的启发

我们的 `Pengyi Research OS v0` 可以直接借鉴 MGP 的几个概念。

### 1. Research Memory Object

我们可以定义：

```text
research_memory
  id
  subject
  scope
  type
  content
  source
  confidence
  sensitivity
  retention_policy
  evidence
  derived_from
  created_at
  updated_at
```

类型可以是：

```text
project_profile
research_preference
experiment_event
paper_fact
procedure_rule
person_project_relationship
artifact_pointer
report_summary
```

这几乎就是 MGP memory object 的 Research OS 版本。

### 2. Policy Context

每次 agent 写记忆，都必须带：

```text
actor_agent
acting_for_subject
project_id
task_id
session_id
risk_level
channel
data_zone
consent_basis
correlation_id
```

例如：

```text
actor_agent: pengyi-rd-agent/v0
acting_for_subject: pengyi
project_id: quant-research-os
task_id: factor-study-001
risk_level: medium
data_zone: private
requested_action: write
```

这比“随便写个 markdown”强很多。

### 3. Audit Trail

每条关键记忆应该有：

```text
who wrote it
when it was written
which source produced it
which agent used it
which report referenced it
whether it was later revoked
```

这会让 Research OS 具备长期可信度。

### 4. Lifecycle

我们应该支持：

```text
expire
revoke
soft delete
hard purge
```

例如：

```text
一个 RA 申请机会过期 -> expire
一个 pitch narrative 不再采用 -> revoke
一条敏感计划不让 agent 再使用 -> delete
一条必须清除的私人信息 -> purge
```

这比普通 note archive 更专业。

### 5. Return Mode

对不同场景返回不同视图：

```text
public website:
  public artifact_summary only

private pitch repo:
  summary / masked

local personal agent:
  raw allowed

external collaborator:
  metadata_only or sanitized summary
```

这对我们开源时尤其重要。

很多材料可以用于内部规划，但不能 raw public。

MGP 的 return mode 正好解决这个问题。

## 对 Quant Research OS 的启发

量化研究特别需要 memory governance。

因为量化 memory 里可能有：

```text
private factor ideas
data vendor constraints
backtest assumptions
strategy performance
portfolio logic
transaction cost model
broker / exchange details
PM feedback
internal risk notes
```

这些不能混在普通知识库里。

可以设计：

```text
QuantMemoryObject
  memory_id
  subject: factor / strategy / dataset / portfolio / user
  scope: task / agent / org / shared_team
  type: semantic_fact / episodic_event / procedural_rule / artifact_summary
  content:
    factor_name
    hypothesis
    universe
    horizon
    metric
    conclusion
  evidence:
    backtest_id
    chart_path
    commit_sha
    report_path
  sensitivity:
    internal / confidential / restricted
  retention_policy:
    research_active / expired_opportunity / compliance_hold
```

R&D Agent 每轮研究都应该产生 memory candidate：

```text
factor hypothesis generated
implementation completed
backtest completed
bias diagnosed
PM reviewed
next plan generated
```

然后由 MGP-style policy 决定：

```text
write
merge
reinforce
manual_review_required
revoke
expire
```

这样 agent 才能长期迭代，而不是每次从零开始。

## 对职业规划和组织理解的启发

我们之前一直在讲组织里的后台：

```text
财务
法务
人力
```

它们的核心价值不是直接做业务，而是：

```text
治理资金流
治理合同和风险
治理人员和组织关系
```

MGP 在 agent system 里扮演类似角色。

它不是直接做 reasoning。

它不是直接做 trading。

它不是直接写报告。

它治理的是：

```text
memory flow
```

所以可以把它类比成：

```text
agent organization 的 memory legal / compliance / audit layer
```

这正好和我们对组织后台的理解打通。

一个真正大的 AI organization，不可能只有 prompt 和 tools。

它必须有：

```text
tool protocol
memory governance protocol
audit system
policy layer
data lifecycle
permission model
```

这就是从单 agent 走向组织级 agent 的关键。

## 可以快速应用的方向

### 方向 1：Research Memory Schema

先不实现完整 MGP。

我们可以先在自己的 repo 里定义：

```text
research_memory.schema.json
policy_context.schema.json
audit_event.schema.json
```

然后让所有 research notes 和 backtest reports 都能生成 memory candidates。

### 方向 2：Website Publishing Governance

我们的网站内容可以分：

```text
public
internal
confidential
restricted
```

public 才能发布到 GitHub Pages。

internal 可以进 private repo。

confidential 只能本地保留。

restricted 需要明确 purge/加密/不写入。

这就是把 MGP 的 sensitivity / return_mode 用到我们的真实工作流。

### 方向 3：Quant Experiment Audit

每次量化实验写入：

```text
experiment_started
factor_implemented
backtest_run
bias_diagnosed
pm_reviewed
experiment_closed
```

每个 event 绑定：

```text
request_id
actor_agent
commit_sha
dataset_version
config_hash
report_path
decision
```

这会让我们的 Quant Research OS 比普通 notebook 更专业。

### 方向 4：PR Contribution

MGP 可以考虑的贡献点：

```text
1. 增加 research / quant memory examples
2. 增加 README 里的 MCP + MGP runtime diagram 示例
3. 增加 file adapter 的 local-first demo
4. 增加 Research OS memory candidate template
5. 增加 docs 中的 privacy / public artifact publishing example
```

但老原则不变：

```text
先真实使用，再提真实 PR。
```

## 风险和限制

MGP 很强，但也不能误用。

### 1. 协议有成本

如果只是一个小脚本记几条偏好，直接写 JSON 就够了。

MGP 适合：

```text
memory is infrastructure
```

不适合：

```text
memory is a tiny implementation detail
```

### 2. MGP 不负责 memory extraction

MGP 不决定：

```text
什么应该成为记忆
什么时候抽取记忆
摘要质量如何评估
embedding 怎么做
ranking 怎么做
```

这些要由上层 runtime 或 backend 解决。

对我们来说：

```text
CatchMe / LightReasoner / SepLLM / LLM extractor
```

负责产生 memory candidates。

MGP 负责治理。

### 3. Policy hook 是 minimal

reference policy 只是 baseline。

生产系统需要更完整的：

```text
identity
permission
tenant isolation
encryption
data residency
human approval
retention schedule
legal hold
```

MGP 定义接口，不替你完成所有治理。

### 4. Backend capability 不一致

不同 adapter 支持能力不同。

所以需要：

```text
capability declaration
conformance profiles
compliance suite
```

这也是 MGP 强调 manifest 和 compliance 的原因。

## 这篇的核心结论

`HKUDS048 MGP` 对我们最大的启发是：

```text
agent memory 不是一个数据库问题。
agent memory 是一个 governance problem。
```

数据库只回答：

```text
数据放在哪里？
```

MGP 回答：

```text
谁写的？
代表谁写的？
能不能读？
能返回原文还是摘要？
能不能撤回？
能不能过期？
能不能删除？
有没有审计？
换 backend 后 contract 还在不在？
```

MGP 的系统位置是：

```text
Memory Governance Protocol
```

映射到我们的系统：

```text
Pengyi Research OS:
  research memory governance layer

Quant Research OS:
  experiment / factor / artifact memory governance layer

R&D Agent:
  candidate -> policy -> persistent memory -> recall -> audit loop

Public website:
  sensitivity-aware publishing and sanitized artifact layer
```

和前几篇组合起来：

```text
CatchMe:
  capture raw traces

SepLLM:
  compress active context

MGP:
  govern persistent memory

LightRAG:
  retrieve structured knowledge

LightReasoner:
  identify high-signal reasoning points
```

这就是一个越来越完整的 AI Scientist OS memory stack。

## 下一步

如果继续深化 MGP，可以做三个小实验：

```text
1. 跑 file adapter demo：
   write profile -> search -> expire -> audit query

2. 写一个 ResearchMemoryCandidate：
   把一篇 HKUDS 学习笔记变成 MGP-style candidate

3. 设计 QuantMemory schema：
   factor hypothesis / backtest artifact / bias diagnosis / PM review
```

这三个实验都能直接服务我们的长期目标：

```text
AI Scientist OS
Quant Research OS
R&D Agent
public + private research portfolio
```

`HKUDS048 MGP` 的位置非常明确：

```text
它是 Research OS 的 memory governance layer。
```
