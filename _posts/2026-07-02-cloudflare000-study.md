---
title: "CLOUDFLARE000: Cloudflare 网站部署与全栈应用总地图"
date: 2026-07-02 00:00:00 +0800
categories: [Learning, Web Engineering]
tags: [pengyi-cloudflare-map, cloudflare000, cloudflare, cloudflare-pages, workers, pages-functions, d1, kv, r2, full-stack, deployment]
---

这是一个新的工程学习系列：

```text
PENGYI_CLOUDFLARE_MAP
```

这一篇是：

```text
CLOUDFLARE000 -> Cloudflare 网站部署与全栈应用总地图
```

先统一拼写：

```text
Cloudflare
```

我们以后可以把它作为“面向用户的网站和交互型产品”的默认部署平台候选。

## 一句话定义

我现在对 Cloudflare 的理解是：

```text
Cloudflare = global edge network + frontend hosting + serverless backend + storage/database + domain/security tooling.
```

中文：

```text
Cloudflare 不只是 CDN。
它可以承接前端网站、后端 API、边缘函数、数据库、对象存储、域名、缓存、安全和部署流水线。
```

对我们最关键的是这条建站链路：

```text
GitHub repo
  -> Cloudflare Pages 部署前端
  -> Pages Functions / Workers 提供后端 API
  -> D1 / KV / R2 提供数据和文件存储
  -> custom domain
  -> 面向用户的网站或小产品
```

这就是我们以后做：

```text
个人网站
项目展示页
AI / Quant demo site
互动网站
前后端网站
交互型商用网站
用户服务器网站
```

时要优先考虑的基础设施。

## 官方组件地图

先把核心组件压成一张表。

| 组件 | 作用 | 我们怎么用 |
|---|---|---|
| Cloudflare Pages | 部署前端和全栈应用 | 个人主页、项目 landing page、React/Vite/Next/Astro 网站 |
| Pages Functions | 给 Pages 项目加服务端代码 | 表单、登录回调、轻量 API、交互逻辑 |
| Cloudflare Workers | 独立 serverless 后端平台 | API 服务、webhook、AI/Quant demo backend、任务接口 |
| D1 | Cloudflare serverless SQL database | 用户、项目记录、实验 metadata、轻量业务数据 |
| Workers KV | global key-value storage | 配置、偏好、缓存、feature flags |
| R2 | object storage | 图片、PDF、报告、数据文件、模型/实验 artifacts |
| Custom Domains / DNS | 域名接入 | 把项目接到自己的域名 |
| Observability / Logs | 监控和调试 | 看部署、请求、错误、性能 |

核心判断：

```text
Pages 负责网站入口。
Functions / Workers 负责后端逻辑。
D1 / KV / R2 负责不同类型的数据。
DNS / domain / security 负责生产化。
```

## 为什么不是只用 GitHub Pages

GitHub Pages 很适合：

```text
静态博客
学习日志
个人展示页
Jekyll / Markdown 输出
不需要后端 API 的 public site
```

我们现在的 `pengpengyi92.github.io` 用 GitHub Pages 是合理的。

但一旦进入：

```text
用户登录
表单提交
数据库
文件上传
后台管理
实时交互
AI API 调用
付费/商用产品
自定义 API
```

GitHub Pages 就不够了。

这时要用：

```text
Cloudflare Pages + Workers / Pages Functions + D1 / KV / R2
```

这就是从“个人静态展示”走向“用户型网站 / 产品型网站”的分界线。

## Cloudflare Pages

Cloudflare Pages 是前端和全栈应用部署入口。

官方定位是：

```text
Create full-stack applications that are instantly deployed to the Cloudflare global network.
```

它支持几种部署方式：

```text
connect Git provider
direct upload
C3 command line
```

对我们最自然的是：

```text
GitHub repo -> Cloudflare Pages
```

每次 push 后自动 build/deploy。

适合：

```text
personal academic homepage
portfolio
project landing page
docs site
React / Vite app
Astro site
Next.js static site
AI demo frontend
Quant dashboard frontend
```

## Pages Functions

Pages Functions 是 Pages 项目里的服务端代码。

官方文档说，它可以在 Cloudflare network 上执行代码，并让 Pages 应用获得动态能力，不需要运行 dedicated server。

适合：

```text
form submission
API route
auth callback
middleware
A/B testing
contact form
simple backend logic
```

典型结构：

```text
my-site/
  src/
  public/
  functions/
    api/
      hello.ts
      contact.ts
```

直觉：

```text
前端页面由 Pages 托管。
后端 endpoint 由 functions/ 目录下的代码处理。
```

这很适合我们做交互型网站的第一步。

## Workers

Cloudflare Workers 是更独立、更通用的 serverless backend。

官方定位是：

```text
A serverless platform for building, deploying, and scaling apps across Cloudflare's global network.
```

它适合：

```text
standalone API
backend service
webhook receiver
auth gateway
AI inference proxy
scheduled jobs
data processing endpoint
serverless product backend
```

Pages Functions 更像：

```text
跟 Pages 项目绑定的轻量后端。
```

Workers 更像：

```text
独立后端服务。
```

我们的默认判断：

```text
一个网站内部的简单 API -> Pages Functions
多个项目复用的后端能力 -> Workers
复杂 API / webhook / scheduled job -> Workers
```

## D1

D1 是 Cloudflare 的 serverless SQL database。

官方文档说明它是 managed, serverless database，使用 SQLite 的 SQL 语义，可以从 Workers 和 Pages 项目查询。

适合：

```text
users
projects
posts metadata
submissions
feedback
orders lite
experiment records
dashboard tables
```

我们可以把 D1 理解成：

```text
Cloudflare 里的轻量 SQL 数据库。
```

适合小产品、个人网站、AI demo、Quant demo 的结构化数据。

不适合一开始就承担非常复杂的企业级数据库需求。
但对我们的起步阶段非常实用。

## Workers KV

Workers KV 是 global, low-latency key-value storage。

官方文档给出的例子包括：

```text
caching API responses
storing user configurations / preferences
storing user authentication details
```

适合：

```text
feature flags
site config
cache
user preference
temporary lookup
public metadata
rate-limit counters, with caution
```

KV 的直觉：

```text
key -> value
```

例如：

```text
theme:user_123 -> dark
latest_report -> cloudflare000
project_config:quant_demo -> {...}
```

如果你需要关系查询、join、事务，更应该考虑 D1。
如果你只是要快速读写简单 key-value，KV 更自然。

## R2

R2 是 Cloudflare 的 object storage。

官方文档说它用于存储大量 unstructured data，并且适合 web content、data lakes、large batch outputs、ML model artifacts or datasets 等场景。

适合：

```text
images
PDF reports
CV files
research notes exports
CSV / parquet / json artifacts
AI generated assets
Quant backtest reports
demo screenshots
model outputs
```

R2 的直觉：

```text
bucket + object key -> file
```

它不是数据库。
它是文件和对象存储。

我们以后做 AI / Quant 项目展示时，R2 很适合放：

```text
report PDFs
experiment artifacts
dataset samples
screenshots
demo assets
```

## 三种数据层怎么选

```text
D1
  结构化数据
  SQL 查询
  users / projects / submissions / experiments

KV
  key-value
  配置 / 偏好 / 缓存 / flags
  高频读取

R2
  文件 / 图片 / PDF / 大对象
  artifacts / datasets / reports
```

一句话：

```text
表格数据用 D1。
配置缓存用 KV。
文件对象用 R2。
```

不要混用。
一开始就把数据类型分清楚，后面系统会稳很多。

## 我们未来网站的几种形态

### 1. 个人网站

目标：

```text
展示身份、研究方向、项目、文章、CV、联系方式。
```

架构：

```text
Cloudflare Pages
  + static site generator
  + custom domain
```

是否需要后端：

```text
通常不需要。
```

除非要加：

```text
contact form
newsletter
private dashboard
analytics API
```

这时再加 Pages Functions。

### 2. 项目展示页

目标：

```text
给某个 AI / Quant / Harness 项目一个 polished landing page。
```

架构：

```text
Cloudflare Pages
  + project demo
  + docs
  + screenshots
  + GitHub links
  + optional API routes
```

适合：

```text
Pengyi Quant Research OS
DeepSeek Coding Agent Harness proposal
X2Strategy / QuantMind study integration
AI Scientist demo
```

### 3. 交互型网站

目标：

```text
用户可以点击、提交、查询、生成、保存。
```

架构：

```text
Frontend:
  Cloudflare Pages

Backend:
  Pages Functions or Workers

Data:
  D1 for structured records
  KV for config/cache
  R2 for uploaded/generated files
```

例子：

```text
用户提交因子想法 -> API 保存到 D1 -> 后台生成 report -> report 文件放到 R2 -> 页面展示结果
```

### 4. 商用小产品

目标：

```text
真实用户、真实交互、稳定域名、可监控、可迭代。
```

架构：

```text
Cloudflare Pages
  + Workers APIs
  + D1
  + R2
  + KV
  + custom domain
  + observability
  + auth / access layer
```

需要额外注意：

```text
auth
permission
data backup
rate limiting
error logging
privacy
billing
terms
monitoring
```

Cloudflare 能承担很多基础设施，但产品纪律仍然要自己建立。

## 前端后端怎么分工

一个最小 full-stack Cloudflare 项目可以这样拆：

```text
Frontend
  React / Vite / Astro / Next / plain HTML
  页面、按钮、表单、交互、展示

Backend
  Pages Functions / Workers
  API route、业务逻辑、鉴权、读写数据库

Database
  D1
  用户、提交记录、项目 metadata

KV
  配置、缓存、开关

R2
  文件、图片、报告、导出结果
```

前端不要直接操作数据库。
标准链路应该是：

```text
browser
  -> fetch('/api/...')
  -> Pages Function / Worker
  -> D1 / KV / R2
  -> response
  -> UI update
```

这就是正常的前后端分层。

## 最小项目结构

可以先记这个结构：

```text
my-cloudflare-app/
  src/
    App.tsx
    main.tsx
  public/
  functions/
    api/
      hello.ts
      submit.ts
  package.json
  wrangler.toml
```

前端：

```text
src/
```

静态资源：

```text
public/
```

后端 API：

```text
functions/api/
```

配置：

```text
wrangler.toml
```

真实项目里结构会根据框架变化，但心智模型不变。

## 一个最小 API 例子

Pages Functions 的 API 可以这样想：

```ts
export async function onRequestGet() {
  return Response.json({
    ok: true,
    message: "hello from Cloudflare Pages Functions",
  });
}
```

如果文件是：

```text
functions/api/hello.ts
```

那么访问路径就是：

```text
/api/hello
```

这就是最小后端。

## 一个交互流程例子

假设我们做一个：

```text
AI Quant Idea Collector
```

用户提交一个策略想法。

前端：

```text
textarea
submit button
result card
```

API：

```text
POST /api/ideas
```

后端逻辑：

```text
1. 校验输入长度
2. 写入 D1
3. 返回 idea_id
4. 可选：触发分析任务
```

数据：

```text
D1 table: ideas
  id
  user_id
  content
  created_at
  status
```

文件：

```text
R2: generated_reports/idea_id.pdf
```

缓存：

```text
KV: feature flag / prompt version / public config
```

这就是一个完整 full-stack 小产品。

## 部署流程

标准流程：

```text
1. GitHub 建 repo
2. 本地开发前端
3. 加 functions/api
4. 加 wrangler.toml
5. Cloudflare Pages 连接 GitHub repo
6. 配 build command 和 output directory
7. 设置 environment variables / bindings
8. push 到 main
9. Cloudflare 自动 build/deploy
10. 绑定 custom domain
```

本地开发可以用：

```text
Wrangler
```

Wrangler 是 Cloudflare 的 CLI。
它负责：

```text
create
dev
deploy
manage bindings
manage D1 / KV / R2 resources
```

## 环境变量和 secrets

真实项目一定会有 secrets：

```text
API keys
database bindings
auth secret
third-party service token
```

原则：

```text
不要把 secret 写进 GitHub repo。
不要写进前端代码。
通过 Cloudflare dashboard / wrangler secrets 管理。
```

前端可以拿到的是 public config。
后端才能拿 secret。

典型：

```text
PUBLIC_SITE_NAME
  前端可见

OPENAI_API_KEY
  只能后端 Worker / Function 使用
```

## 域名和生产化

Cloudflare 的强项之一是域名和网络层。

我们以后要做项目站时，应该考虑：

```text
custom domain
HTTPS
redirect www / apex
preview deployment
rollback
analytics
logs
security headers
rate limiting
```

个人学习站可以简单。
用户型网站不能太随意。

## 什么时候用 Cloudflare Pages

用 Pages：

```text
静态站
个人主页
项目展示页
前端 SPA
文档站
轻量全栈站
```

原因：

```text
GitHub 集成简单
部署快
适合前端
支持 Functions
支持 preview deployments / rollback
```

## 什么时候用 Workers

用 Workers：

```text
独立 API
多个前端共用的 backend
webhook
scheduled job
AI inference proxy
边缘服务
需要更强后端控制的项目
```

原因：

```text
它是更通用的 serverless application platform。
```

## 什么时候不用 Cloudflare

Cloudflare 很强，但不是所有东西都要上它。

如果项目需要：

```text
复杂长连接状态
传统后端框架强依赖
大型关系数据库
复杂事务
GPU training
heavy backend compute
特殊企业内网部署
```

那就要评估：

```text
VPS
traditional cloud
container platform
managed database
specialized AI infrastructure
```

我们的原则不是迷信平台。
原则是：

```text
选择最小可行、可部署、可维护、可增长的架构。
```

## 我们自己的默认规则

以后按这个判断：

```text
简单 public learning log
  -> GitHub Pages

polished personal / academic homepage
  -> Cloudflare Pages

项目 landing page
  -> Cloudflare Pages

交互型前端网站
  -> Cloudflare Pages + Pages Functions

用户型 full-stack 小产品
  -> Cloudflare Pages + Workers + D1 / KV / R2

AI / Quant demo site
  -> Pages frontend + Workers API + D1 metadata + R2 artifacts

复杂商业系统
  -> 先评估 Cloudflare-first，再决定是否需要传统云架构
```

一句话：

```text
从现在开始，用户型网站默认 Cloudflare-first。
```

## 对 Pengyi Credit OS 的意义

Cloudflare 对我们不是单纯部署工具。
它会进入我们的 credit system。

因为它让我们能把：

```text
repo
demo
technical report
project landing page
interactive product
custom domain
```

连成一套可验证输出。

比如：

```text
GitHub repo:
  source code

Cloudflare site:
  live demo

Website article:
  technical explanation

Private OS:
  roadmap and strategy

Public README:
  setup and architecture
```

这就是从“我做过”变成：

```text
别人可以打开、理解、试用、验证。
```

## 对 AI / Quant 项目的意义

以后我们可以做这些站：

```text
Pengyi Quant Research OS Demo
  idea -> factor -> backtest report -> diagnosis

AI Harness Demo
  task -> agent run -> logs -> score

Research OS Demo
  paper -> hypothesis -> experiment plan -> report

NeetCode Agent Benchmark Demo
  problem -> agent solution -> tests -> analysis

Credit OS Public Showcase
  projects -> reports -> GitHub links -> timeline
```

这些都不是纯静态展示。
它们需要交互。

所以 Cloudflare-first 很合理。

## 学习路线

后续可以这样拆：

```text
CLOUDFLARE000: 总地图
CLOUDFLARE001: Cloudflare Pages 部署个人网站
CLOUDFLARE002: Pages Functions 做 API routes
CLOUDFLARE003: Workers 独立后端服务
CLOUDFLARE004: D1 / KV / R2 数据层
CLOUDFLARE005: Custom Domain / DNS / HTTPS / Redirects
CLOUDFLARE006: AI / Quant Demo Site 实战
CLOUDFLARE007: 商用小产品部署 checklist
```

000 的任务是先建立地图。
后面每篇都可以配一个真正 demo。

## 最小实战计划

第一期实战不要贪大。
做一个：

```text
Cloudflare Hello Full-stack Site
```

功能：

```text
1. 一个首页
2. 一个 /api/hello
3. 一个表单
4. 表单提交到 /api/submit
5. 数据写入 D1
6. 生成一个结果页面
7. 部署到 Cloudflare Pages
```

这个 demo 打通后，我们就有了：

```text
frontend
backend
database
deployment
domain
logs
```

这比空学文档有用。

## 当前结论

`CLOUDFLARE000` 的核心结论：

```text
Cloudflare = 我们未来用户型网站和交互型产品的默认部署平台候选。
```

具体拆法：

```text
Cloudflare Pages
  -> frontend / site deploy

Pages Functions
  -> Pages 内部轻量 API

Workers
  -> 独立 serverless backend

D1
  -> structured SQL data

KV
  -> key-value config / cache

R2
  -> files / reports / artifacts
```

我们现在的规则：

```text
静态学习日志继续 GitHub Pages。
以后有前端后端、用户交互、商用可能、AI/Quant demo 的网站，默认 Cloudflare-first。
```

## Sources

```text
Cloudflare Pages:
https://developers.cloudflare.com/pages/

Cloudflare Pages Functions:
https://developers.cloudflare.com/pages/functions/

Cloudflare Workers:
https://developers.cloudflare.com/workers/

Cloudflare D1:
https://developers.cloudflare.com/d1/

Cloudflare Workers KV:
https://developers.cloudflare.com/kv/

Cloudflare R2:
https://developers.cloudflare.com/r2/
```
