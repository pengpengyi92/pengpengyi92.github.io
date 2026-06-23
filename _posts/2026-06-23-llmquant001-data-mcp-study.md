---
title: "LLMQUANT001: data-mcp 作为数据工具层"
date: 2026-06-23 00:00:00 +0800
categories: [Learning, Quant Research]
tags: [pengyi-llmquant-studymap, llmquant001, data-mcp, mcp, quant-research, research-os]
---

这是 `PENGYI_LLMQUANT_STUDYMAP` 的第二篇：

```text
LLMQUANT001 -> data-mcp
```

`LLMQUANT000` 做的是总地图。

这一篇开始深入拆具体项目。第一个项目选 `data-mcp`，因为它是 LLMQuant 生态里最基础、也最适合接入我自己 Quant Research OS 的一层。

一句话理解：

```text
data-mcp = finance agent 的数据工具层
```

它不是回测框架，不是知识图谱，也不是策略生成器。

它的角色更底层：

```text
让 AI agent 能够以工具调用的方式访问金融数据和研究上下文。
```

## 为什么 data-mcp 重要

金融研究不能只靠语言模型记忆。

一个真正可用的 finance agent，至少要能回答：

- 股票价格历史是什么？
- 10-K / 10-Q / 8-K 里面有哪些关键段落？
- 某个机构的 13F 持仓是什么？
- 某个 ticker 有哪些机构持有人？
- 一个 ETF 的底层持仓是什么？
- 某个宏观指标最新值和历史序列是什么？
- 相关论文和 Quant Wiki 条目在哪里？

如果 agent 只能生成文字，它就很容易变成“金融术语作文机”。

但如果 agent 有稳定数据工具，它就可以进入更严肃的研究流程：

```text
research question
  -> tool call
  -> source-grounded result
  -> structured evidence
  -> hypothesis
  -> experiment
  -> diagnosis
```

所以 `data-mcp` 对我来说是 Research OS 的第一块地基。

## 项目定位

本地 `package.json` 显示：

| Item | Value |
|---|---|
| package | `@llmquant/data-mcp` |
| version | `0.3.4` |
| language | TypeScript |
| runtime | Node.js `>=20` |
| MCP framework | `fastmcp` |
| schema | `zod` |
| build | `tsup` |
| test | Node test runner + `tsx` |
| binary | `llmquant-data-mcp` |

它的核心链路是：

```text
MCP-enabled agent
  -> @llmquant/data-mcp
  -> tool schema
  -> LLMQuant Data API
  -> structured JSON result
  -> downstream finance workflow
```

从工程上看，它不是一个 demo，而是一个真正 npm package 形态的 MCP server。

## 工具清单

README 里列出的 MCP tools 包括：

| Tool | 用途 |
|---|---|
| `wiki_search` | 语义搜索 Quant Wiki 条目 |
| `wiki_read` | 根据 ID 读取 wiki article |
| `paper_search` | 语义搜索研究论文 |
| `paper_read` | 读取论文 sections |
| `crypto_historical_klines` | crypto OHLCV K 线数据 |
| `crypto_snapshot` | crypto 当前价格和 24h 统计 |
| `equity_historical_prices` | 美股日频 OHLCV、股息、拆股 |
| `macro_indicator_search` | 浏览宏观指标 catalog |
| `macro_indicator_history` | 宏观指标历史序列 |
| `macro_indicator_snapshot` | 宏观指标最新值 |
| `sec_filing_browse` | 浏览 SEC 10-K / 10-Q / 8-K 元数据 |
| `sec_filing_read` | 读取 SEC filing section text |
| `sec_13f_list_manager_holdings` | 查询机构 manager 13F 持仓 |
| `sec_13f_list_ticker_holders` | 查询某 ticker 的机构持有人 |
| `sec_13f_list_top_managers` | 查询 top 13F managers |
| `etf_lookup` | ETF identity 和 top holdings summary |
| `etf_holdings` | ETF N-PORT regulatory holdings |

这已经覆盖一个早期 finance research agent 很核心的数据面：

```text
knowledge:
  wiki + papers

market data:
  equity + crypto

macro:
  macro catalog + history + snapshot

company filings:
  SEC filings + 13F

fund exposure:
  ETF lookup + ETF holdings
```

对我自己的系统来说，这一层可以先承担“证据入口”的角色。

## 源码总架构

源码的主链路很清楚：

```text
src/index.ts
  -> getEnv()
  -> createServer(env)
  -> new FastMCP(...)
  -> new LlmquantApiClientProvider(env)
  -> registerLlmquantDataTools(server, api)
  -> server.start(stdio or httpStream)
```

关键文件：

| File | 作用 |
|---|---|
| `src/index.ts` | 程序入口，决定 stdio / httpStream 启动方式 |
| `src/env.ts` | 环境变量、`.env`、transport config |
| `src/server.ts` | 创建 FastMCP server，注入 API client provider |
| `src/register-tools.ts` | 集中注册所有 MCP tools |
| `src/tools/registry.ts` | 定义最小 tool registry interface |
| `src/client/api-provider.ts` | 根据本地/远程 context 选择 API client |
| `src/client/web-api.ts` | LLMQuant Data API client |
| `src/shared/schemas.ts` | 复用 Zod 输入 schema |
| `src/shared/errors.ts` | API / transport 错误类型 |
| `src/shared/result.ts` | tool result JSON 格式化 |
| `src/remote/*` | remote MCP principal、path-token proxy、rate limit |

这套结构非常适合学习。

它把 MCP server 拆成了几个清楚边界：

```text
transport
  env
  server
  tool registry
  schemas
  API client
  errors
  result envelope
```

我自己的工具系统也应该这么拆。

## Server 启动逻辑

`src/index.ts` 做的事情很少，但它是主入口：

```text
getEnv()
createServer(env)
if transport == httpStream:
  start httpStream
  maybe start path-token proxy
else:
  start stdio
```

这说明项目支持两种使用场景：

| Transport | 场景 |
|---|---|
| `stdio` | 本地 Claude / Cursor / Codex / Gemini 等 agent 调用 |
| `httpStream` | hosted remote MCP connector |

本地默认是 `stdio`。

这很合理：大部分个人研究者会先从本地 MCP 开始。

## Tool 注册逻辑

`src/register-tools.ts` 是很重要的中控文件。

它显式注册所有工具：

```text
registerSearchWikiTool
registerReadWikiTool
registerSearchPaperTool
registerReadPaperTool
registerCryptoHistoricalTool
registerCryptoSnapshotTool
registerEquityHistoricalTool
registerMacroIndicatorSearchTool
registerMacroIndicatorHistoryTool
registerMacroIndicatorSnapshotTool
registerSecFilingBrowseTool
registerSecFilingReadTool
registerSec13fByManagerTool
registerSec13fByTickerTool
registerSec13fListTopManagersTool
registerEtfLookupTool
registerEtfHoldingsTool
```

这个文件的价值是：

```text
一眼知道 server 暴露了哪些能力
```

对 agent 系统来说，这很重要。

工具不能散落在各处，必须有一个可审计入口。

我自己的 Research OS 后面也应该有类似文件：

```text
registerResearchTools()
registerDataTools()
registerBacktestTools()
registerReportTools()
```

## 单个 Tool 的模式

每个工具基本都遵循同一个模式：

```text
registerXTool(server, api)
  -> server.addTool({
       name,
       description,
       parameters: z.object(...),
       execute: async (args, context) => {
         const response = await getApiClient(api, context).method(args)
         return formatToolResult({...})
       }
     })
```

以 `wiki_search` 为例：

```text
input:
  query
  topK

api:
  searchWiki({ query, topK })

output:
  summary
  items
  meta
```

这个模式非常重要。

它说明一个好的 agent tool 至少要有：

- 清楚的 name；
- 明确的 description；
- 严格的 parameters；
- 单一 execution boundary；
- 结构化 output；
- 可读的 error。

这比只写 prompt 强很多。

结论：

```text
strict tool contracts beat clever prompts
```

## Zod Schema 是质量控制

`src/shared/schemas.ts` 是我最想学习的文件之一。

它定义了所有复用输入约束：

| Schema | 约束 |
|---|---|
| `searchQuerySchema` | query 非空，最多 2000 chars |
| `topKSchema` | 1 到 10 |
| `paperCardIdSchema` | UUID |
| `tickerSchema` | crypto ticker 必须是 `BASE-QUOTE`，如 `BTC-USD` |
| `equityTickerSchema` | 美股 ticker 格式 |
| `equityLimitSchema` | 1 到 200 |
| `macroLimitSchema` | 1 到 500 |
| `secFilingTypeSchema` | 只能是 `10-K`、`10-Q`、`8-K` |
| `secItemsSchema` | SEC items 最多 25 个 |
| `sec13f*LimitSchema` | 13F 查询数量限制 |
| `etfTickerSchema` | ETF ticker 格式 |
| `asOfDateSchema` | 有效 `YYYY-MM-DD` 日期 |

金融 agent 的输入不能太松。

太松会带来很多问题：

- ticker 错；
- filing type 错；
- 日期错；
- result size 无边界；
- 8-K 定位错误；
- `accession_number` 和 year/quarter 混用；
- agent 把 unsupported coverage 当成真实数据。

所以 schema 不是小事。

它是研究质量控制的一部分。

## Progressive Disclosure

SEC filing 工具展示了一个很好的 pattern：

```text
sec_filing_browse
  -> 先返回 filing metadata 和 section_keys

sec_filing_read
  -> 再读取具体 section text
```

这叫 progressive disclosure。

agent 不应该一上来读完整 filing。

正确流程是：

```text
browse
  -> inspect metadata
  -> select accession_number / section
  -> read targeted text
  -> reason with evidence
```

这个模式可以泛化到很多研究任务：

```text
paper_search -> paper_read selected sections
filing_browse -> filing_read selected sections
dataset_catalog -> dataset_slice
factor_library_index -> factor_card_read
```

对我的 Research OS 来说，这是一条重要原则：

```text
先发现，再选择，再读取。
不要默认拉满上下文。
```

这能减少 token 浪费、数据费用和证据混乱。

## API Client 层

`src/client/web-api.ts` 是项目里最大的接口文件。

它负责：

1. 构造 API URL；
2. 设置 auth header；
3. 发起 fetch；
4. 设置 timeout；
5. 解析 JSON；
6. 处理非 2xx response；
7. 把 API 返回映射成更适合 tool 层消费的对象。

一个细节是，它大量把 snake_case 转成 camelCase：

```text
wiki_item_id        -> wikiItemId
paper_card_id       -> paperCardId
available_sections  -> availableSections
adjusted_close      -> adjustedClose
source_updated_at   -> sourceUpdatedAt
```

这体现了 API boundary 的价值。

后端 API 可以有自己的字段风格，agent tool 层应该暴露统一、可读、稳定的数据结构。

## Result Envelope

`src/shared/result.ts` 很简单：

```ts
formatToolResult(payload) {
  return JSON.stringify(payload, null, 2)
}
```

但工具输出通常都会组织成：

```text
summary
item / items
meta
```

这很适合 agent：

- `summary` 给快速方向；
- `item/items` 给结构化数据；
- `meta` 给 query、count、credit、coverage、source 等审计信息。

我自己的 Research OS 也应该采用类似 envelope：

```text
summary
data
evidence
meta
warnings
next_actions
```

特别是在金融研究里，`meta` 不是装饰。

它是 auditability 的一部分。

## Error Contract

`src/shared/errors.ts` 定义了两个核心错误：

| Error | 含义 |
|---|---|
| `LlmquantApiError` | API 返回非 2xx |
| `LlmquantTransportError` | 网络、credential、timeout、JSON decode 等失败 |

工具里统一：

```text
catch error
  -> describeToolError(error)
  -> throw readable Error
```

这对 agent 很重要。

一个 finance agent 如果拿不到数据，不能假装拿到了。

它必须知道失败原因：

- credential missing；
- API rejected；
- network failed；
- timeout；
- unsupported coverage；
- no data found；
- invalid input。

否则后续推理就会变成幻觉。

## Local stdio 与 Remote MCP

`data-mcp` 支持两种 transport：

```text
stdio
httpStream
```

### stdio

本地默认模式。

需要：

```text
LLMQUANT_API_KEY
```

典型启动：

```text
npx -y @llmquant/data-mcp
```

Claude Code、Cursor、Codex CLI、Gemini CLI 都可以按 MCP config 接。

### httpStream

远程 hosted connector 模式。

相关配置：

```text
LLMQUANT_INTERNAL_API_SECRET
LLMQUANT_MCP_HOST
LLMQUANT_MCP_PORT
LLMQUANT_MCP_INTERNAL_PORT
LLMQUANT_MCP_ENDPOINT
LLMQUANT_MCP_ALLOWED_ORIGINS
LLMQUANT_MCP_RATE_LIMIT_WINDOW_MS
LLMQUANT_MCP_RATE_LIMIT_MAX
```

`src/remote/principal.ts` 做 principal resolution。

`src/remote/path-token-proxy.ts` 做 path token rewrite、rate limit、health probe、upstream proxy。

这说明它不只是 local tool，而是已经考虑 hosted MCP connector 的生产化问题。

关键设计：

```text
local stdio 和 hosted httpStream 共享同一套 tools
只在 auth 和 transport 上分叉
```

这是干净的系统设计。

## 项目强点

### 1. MCP 边界清楚

每个工具都有明确：

- name；
- description；
- schema；
- execution；
- result envelope；
- error handling。

这让 agent 工具不再是 prompt 附属品，而是真实接口。

### 2. 金融数据面覆盖够用

早期 Research OS 需要的很多数据面都已经有：

- wiki；
- papers；
- equity prices；
- crypto；
- macro；
- SEC filings；
- 13F；
- ETF holdings。

### 3. Progressive Disclosure 做得好

特别是 SEC filing browse/read。

这个模式值得我迁移到 paper、report、dataset、factor library。

### 4. Remote MCP 思维成熟

它已经考虑 hosted connector、principal、scope、path token、rate limit。

这说明项目不是只面向个人本地用法。

### 5. 有测试面

本地文件里能看到 tests 覆盖：

- env；
- api provider；
- web api client；
- wiki / paper / macro / equity / crypto / SEC / ETF tools；
- remote principal；
- remote http stream integration。

我当前机器没有 `node` / `npm`，所以没有实际跑 `npm test`。

但从文件结构看，测试面是有意识设计的。

## 需要注意的边界

### 1. data-mcp 不是 memory

它负责取数据，不负责长期研究记忆。

memory 应该属于：

```text
QuantMind
Research OS artifact store
notes
website posts
private workspace
```

### 2. data-mcp 不是 backtest engine

它不负责策略执行和回测。

后续应该接：

```text
Magents
WorldQuant-style factor lab
custom research runner
Research OS run manifest
```

### 3. 数据覆盖和 freshness 要写入 meta

例如 ETF holdings 明确是 SEC N-PORT regulatory snapshot，不是实时 issuer holdings。

agent 不能把它说成“当前实时持仓”。

所以在 Research OS 里必须记录：

```text
source
as_of_date
fetched_at
coverage_status
coverage_notice
stale
creditsUsed
```

### 4. Credit 也是规划变量

README 里每个工具都有 credit model。

后续 agent planning 需要考虑：

```text
哪些 call 免费
哪些 call 花 credit
哪些 call 可以 batch
哪些结果应该 cache
哪些证据必须实时更新
```

否则 autonomous loop 可能会浪费大量数据调用。

## 怎么接入 Pengyi Quant Research OS

我希望未来的 Research OS 这样使用 `data-mcp`：

```text
research question
  -> define required evidence
  -> call data-mcp tools
  -> save raw tool result
  -> extract structured evidence
  -> create hypothesis card
  -> create experiment contract
  -> run backtest
  -> diagnose bias
  -> write research report
```

例如一个问题：

```text
机构持仓变化是否能解释 post-earnings drift？
```

可能需要：

```text
sec_13f_list_ticker_holders
sec_13f_list_manager_holdings
sec_filing_browse
sec_filing_read
equity_historical_prices
paper_search
wiki_search
```

然后 Research OS 应该生成：

```text
hypothesis card
data requirement card
raw evidence manifest
factor spec
backtest protocol
bias checklist
research verdict
```

这就是 `data-mcp` 在我系统里的位置：

```text
data-mcp = evidence access layer
QuantMind = knowledge structure layer
skills = workflow routing layer
Research OS = experiment contract layer
R&D Agent = hypothesis and iteration layer
```

## 我想复用的 Patterns

| Pattern | 复用到哪里 |
|---|---|
| central `register-tools.ts` | 我的 Research OS tool registry |
| reusable `schemas.ts` | 因子、实验、数据工具输入验证 |
| `summary + item/items + meta` | 研究工具结果 envelope |
| browse/read progressive disclosure | 论文、filing、dataset、factor library |
| provider-based API client | local / remote / future multi-user execution |
| transport-aware env parsing | 本地研究和 hosted service 双模式 |
| credit metadata | agent planning 和 caching |
| principal + scopes | 未来多用户 private Research OS |

核心 takeaway：

```text
好的 agent 系统首先是好的接口系统。
```

## LLMQUANT001 后续实践

读完这个项目，我应该做三个实际动作。

### 1. 做一个 data-mcp usage playbook

例如：

```text
Equity research:
  wiki_search
  paper_search
  sec_filing_browse
  sec_filing_read
  equity_historical_prices

Macro research:
  macro_indicator_search
  macro_indicator_snapshot
  macro_indicator_history
  paper_search

ETF exposure research:
  etf_lookup
  etf_holdings
  equity_historical_prices

Smart money research:
  sec_13f_list_top_managers
  sec_13f_list_manager_holdings
  sec_13f_list_ticker_holders
```

### 2. 设计 Research OS Data Contract

每一次 data-mcp 调用都应该记录：

```text
tool_name
input_args
timestamp
data_source
result_meta
credits_used
coverage_notice
raw_result_path
derived_evidence_path
```

这会把 tool call 变成 research lineage。

### 3. 做一个 public-safe mini demo

一个安全 demo：

```text
one public company
  -> browse latest 10-K
  -> read risk factors + MD&A
  -> get 90 days equity prices
  -> search related wiki concepts
  -> produce a source-grounded company research note
```

这个 demo 不涉及私密因子，不涉及非公开数据，适合放网站上。

## 当前结论

`data-mcp` 不是整个 LLMQuant。

但它是 LLMQuant 很关键的一层：

```text
agent intent
  -> tool schema
  -> financial data API
  -> structured result
  -> downstream research workflow
```

对我来说，它应该成为 Pengyi Quant Research OS 的默认 evidence access layer。

下一篇应该读：

```text
LLMQUANT002 -> skills
```

因为当 agent 能拿到数据以后，下一步就是：

```text
不同金融任务应该走什么 workflow？
```

## References

- LLMQuant Data MCP: <https://github.com/LLMQuant/data-mcp>
- LLMQuant Data: <https://llmquantdata.com>
- Model Context Protocol: <https://modelcontextprotocol.io>
- FastMCP: <https://github.com/punkpeye/fastmcp>
- LLMQuant Skills: <https://github.com/LLMQuant/skills>
