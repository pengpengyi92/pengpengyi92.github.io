---
title: "GitHub Pages Public Profile OS: 把个人主页变成研究资产入口"
date: 2026-06-22 12:30:00 +0800
categories: [AI Scientist, Portfolio]
tags: [github-pages, jekyll, chirpy, public-profile, research-os]
---

个人主页不是一个装饰页面。对我来说，它应该成为一个 public profile OS：

```text
主页
  -> 说明我是谁、做什么、正在寻找什么机会

博客
  -> 记录学习、项目、paper reading、技术报告和申请叙事

项目索引
  -> 把 GitHub repo、实验、demo、报告连接起来

输出 ledger
  -> 让每周的学习和工程进展留下公开痕迹
```

这次升级的目标很清楚：

- 保留一个干净的个人门面，服务 RA / PhD / research engineer / quant research 机会。
- 新增技术博客能力，用 Markdown 写文章。
- 用分类、标签、归档，把学习路径沉淀成可检索资产。
- 所有内容必须 public-safe，不写雇主内部材料、不写非公开数据、不写未脱敏因子。

## 为什么用 GitHub Pages

GitHub Pages 足够简单：

```text
Markdown / HTML
  -> Jekyll 构建
  -> GitHub Pages 发布
  -> https://pengpengyi92.github.io/
```

它没有后端数据库，也不需要自己维护服务器。对研究型个人网站来说，这反而是优点：低维护成本、版本可追溯、每次更新都是 commit。

## 为什么用 Chirpy

Chirpy 是一个偏技术写作的 Jekyll theme。它适合长期记录：

- posts
- categories
- tags
- archives
- table of contents
- code highlighting
- search-friendly static pages

也就是说，它更像一个 technical writing system，而不只是一个 blog skin。

## 后续更新节奏

这个网站的价值来自持续更新，不来自一次性装修。

我希望形成一个很具体的节奏：

```text
每周至少 1 篇学习/项目文章
每个核心项目至少 1 篇 technical note
每个 RA/PhD 方向至少 1 篇 research-fit note
每次重要项目迭代都留下 public changelog
```

最终目标是让网站本身成为证明：

```text
我不仅有想法。
我能把想法变成 repo、实验、文章、申请材料和长期可维护的系统。
```
