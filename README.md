# Pengyi Personal Website and Research Blog

GitHub Pages repository for Pengyi's public portfolio and learning log.

Public site target:

```text
https://pengpengyi92.github.io/
```

## Purpose

This site is the public index for:

- AI scientist / quant research engineer positioning
- Quant R&D Agent and Research OS projects
- Jekyll/Chirpy technical blog posts
- RA, PhD, research engineer, and quant research application narrative
- Public learning log and output ledger
- Selected achievements, technical writing, and open-source direction

## File Map

| File | Purpose |
|---|---|
| `index.html` | Main personal portfolio homepage |
| `learning.html` | Learning log, output ledger, and public research asset plan |
| `_posts/` | Markdown technical blog posts |
| `_tabs/` | Chirpy tabs: categories, tags, archives, about |
| `_config.yml` | Jekyll/Chirpy site configuration |
| `.github/workflows/pages-deploy.yml` | GitHub Actions Pages build and deploy workflow |
| `version_A_academic.html` | Archived academic-facing draft |
| `version_B_industry.html` | Archived industry-facing draft |
| `version_C_universal.html` | Archived universal draft |

## Public Safety Rules

Publish:

- Sanitized open-source code and demos
- Public research notes
- Technical reports and benchmark summaries
- Public CV/application narratives

Do not publish:

- Employer confidential data or internal documents
- Private factor libraries without sanitization
- Client names, internal processes, or non-public metrics
- Anything that violates contract, IP, or compliance obligations

## Update Workflow

```powershell
git status --short
git add index.html learning.html README.md _posts _tabs _config.yml Gemfile .github/workflows/pages-deploy.yml
git commit -m "Update personal website"
git push
```

## New Post Template

Create a Markdown file under `_posts/`:

```text
_posts/YYYY-MM-DD-short-slug.md
```

Example front matter:

```yaml
---
title: "Post Title"
date: 2026-06-22 15:30:00 +0800
categories: [AI Scientist, Quant Research]
tags: [llm-agent, quant, research-os]
---
```

Then write normal Markdown below the front matter.

GitHub Actions builds the Jekyll/Chirpy site and deploys it to GitHub Pages.
