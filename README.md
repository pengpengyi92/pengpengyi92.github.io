# Pengyi Personal Website

GitHub Pages repository for Pengyi's public portfolio and learning log.

Public site target:

```text
https://pengpengyi92.github.io/
```

## Purpose

This site is the public index for:

- AI scientist / quant research engineer positioning
- Quant R&D Agent and Research OS projects
- RA, PhD, research engineer, and quant research application narrative
- Public learning log and output ledger
- Selected achievements, technical writing, and open-source direction

## File Map

| File | Purpose |
|---|---|
| `index.html` | Main personal portfolio homepage |
| `learning.html` | Learning log, output ledger, and public research asset plan |
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
git add index.html learning.html README.md
git commit -m "Update personal website"
git push
```

If GitHub Pages is not enabled, enable it from repository settings or via GitHub CLI after confirming the repository is public.
