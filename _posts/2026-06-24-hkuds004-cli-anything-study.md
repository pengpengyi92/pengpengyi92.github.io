---
title: "HKUDS004: CLI-Anything 作为 Agent-Native Software Action Layer"
date: 2026-06-24 00:00:00 +0800
categories: [Learning, Research OS]
tags: [pengyi-hkuds-studymap, hkuds004, hkuds, cli-anything, cli-hub, agent-native, software-action-layer, cli, preview, skill, research-os]
---

这是 `PENGYI_HKUDS_STUDYMAP` 的第五篇。

```text
HKUDS004 -> CLI-Anything
```

到目前为止，HKUDS 第一阶段我们已经看了：

```text
HKUDS000 -> study map
HKUDS001 -> LightRAG
HKUDS002 -> Vibe-Trading
HKUDS003 -> nanobot
```

现在看 `CLI-Anything`。我的定位是：

```text
CLI-Anything = Agent-Native Software Action Layer
```

如果说：

```text
LightRAG     = research memory
Vibe-Trading = quant research workflow
nanobot      = personal agent shell
```

那么：

```text
CLI-Anything = 让 agent 能稳定操作外部软件的工具生态层
```

这非常关键。因为一个 agent 如果只能聊天、读文件、写代码，它还没有真正进入软件世界。真正的 research OS 需要能操作：

```text
Zotero
browser
Obsidian / Joplin
LibreOffice
Blender
GIMP
Kdenlive
QGIS
Ollama
OpenRefine
Firefly III
custom quant tools
```

`CLI-Anything` 的核心想法就是：不要让 agent 去模拟人类点 GUI，而是把软件能力改造成可发现、可调用、可测试、可组合的 CLI 协议。

## Local Snapshot

这次阅读的是本地 HKUDS 工作区里的 `CLI-Anything`。

| Item | Value |
|---|---|
| repo | `CLI-Anything` |
| branch | `main` |
| local head | `bf3cc39` |
| latest local commit | `Feature/cli matrix multi approach (#355)` |
| status | clean |
| license | Apache 2.0 |
| Python | `>=3.10` |

本地规模：

| Metric | Count |
|---|---:|
| total files | 1698 |
| Python files | 1194 |
| Markdown files | 396 |
| top-level directories | 85 |
| `registry.json` harness entries | 76 |
| `public_registry.json` entries | 20 |
| `matrix_registry.json` workflow matrices | 5 |
| canonical root skills | 70 |

项目入口很多，但主结构可以先压成几层：

```text
CLI-Anything/
  README.md
  registry.json
  public_registry.json
  matrix_registry.json
  cli-hub/
  cli-anything-plugin/
  cli-hub-meta-skill/
  codex-skill/
  hermes-skill/
  reasonix-skill/
  skills/
  <software>/agent-harness/
```

它不是一个普通 repo，而是一个 agent-native software ecosystem。

## Core Thesis

README 的标题是：

```text
CLI-Anything: Making ALL Software Agent-Native
```

它背后的判断是：

```text
today's software serves humans
tomorrow's users will be agents
```

这句话对我们做 Research OS 很有启发。现在大部分软件都默认用户是人：

```text
button
menu
dialog
drag and drop
visual inspection
manual export
```

但 agent 更适合的接口是：

```text
command
argument
state
json output
help text
artifact path
testable result
```

所以 `CLI-Anything` 的目标不是替代 GUI 软件，而是把 GUI 软件背后的真实能力转成 agent 可以调用的 command surface。

## Why CLI

README 里对 CLI 的判断很直接：

| Reason | Meaning |
|---|---|
| structured and composable | 命令、参数、管道天然适合 agent 规划 |
| lightweight and universal | 终端比 GUI 自动化更稳定 |
| self-describing | `--help` 可以被 agent 自动读取 |
| agent-first | `--json` 输出避免脆弱文本解析 |
| deterministic | 比截图点击更容易复现和测试 |

这点对工程系统很重要。浏览器 GUI 自动化和屏幕点击在很多场景会很脆：

```text
resolution changes
theme changes
button position changes
modal appears
network delay
OCR error
```

而 CLI 把动作变成稳定协议：

```bash
cli-anything-zotero --json item find "foundation model" --limit 5
cli-anything-libreoffice --project report.json export render report.pdf -p pdf --overwrite
cli-anything-blender --json --project scene.json preview capture --recipe quick
```

这就是 agent 真正需要的接口。

## Two User Paths

`CLI-Anything` 有两条使用路径。

第一条是直接使用已有生态：

```bash
pip install cli-anything-hub
cli-hub list
cli-hub search image
cli-hub info gimp
cli-hub install gimp
cli-hub launch gimp
```

第二条是在 registry 没有覆盖时生成新 harness：

```text
/cli-anything <software-path-or-repo>
```

第一条面向普通用户和 agent runtime，第二条面向 contributor 和系统扩展。

换句话说：

```text
CLI-Hub = use existing agent-native CLIs
CLI-Anything plugin = build new agent-native CLIs
```

这两个路径合起来，才形成生态闭环。

## System Architecture

我把 `CLI-Anything` 的系统架构理解成：

```text
Human / Agent Host
  -> CLI-Hub meta-skill or platform plugin
  -> cli-hub package manager
  -> registry.json / public_registry.json / matrix_registry.json
  -> install selected harness
  -> cli-anything-<software>
  -> real software backend
  -> artifacts / preview bundles / JSON results
  -> agent reads result and continues
```

里面每层的职责不同：

| Layer | Component | Role |
|---|---|---|
| discovery | `registry.json`, `public_registry.json` | 描述有哪些 CLI、怎么安装、入口命令是什么 |
| package manager | `cli-hub` | search、info、install、launch、update、uninstall |
| builder | `cli-anything-plugin/HARNESS.md` | 生成新 harness 的标准流程 |
| agent adapters | `codex-skill`, `hermes-skill`, `reasonix-skill`, `.pi-extension` | 把同一套方法接到不同 agent 平台 |
| harness | `<software>/agent-harness/` | 具体软件的 CLI 包 |
| skill | `skills/cli-anything-<software>/SKILL.md` | 让 agent 知道如何使用某个 CLI |
| preview | `docs/PREVIEW_PROTOCOL.md`, `cli_hub.preview` | 统一中间结果预览协议 |
| matrix | `matrix_registry.json`, `cli-hub-matrix/` | 面向复杂任务的 capability-to-provider 映射 |

这比“写一个 CLI wrapper”成熟很多。它有 registry、package manager、builder SOP、skills、preview、matrix、tests、contribution path。

## CLI-Hub

`cli-hub` 是整个生态的包管理器。

本地核心文件：

```text
cli-hub/
  cli_hub/
    cli.py
    registry.py
    installer.py
    matrix.py
    matrix_skill.py
    preview.py
    analytics.py
  setup.py
  tests/
```

`registry.py` 做几件事：

```text
fetch registry.json
fetch public_registry.json
cache under ~/.cli-hub
merge harness and public CLIs
search by name / description / category
get one CLI by name
list categories
```

`installer.py` 负责安装和状态记录：

```text
~/.cli-hub/installed.json
~/.cli-hub/matrix_state.json
pip install
npm install
uv install
brew / bundled / custom command
update
uninstall
matrix install
doctor
```

`cli.py` 暴露用户命令：

```text
cli-hub list
cli-hub search <query>
cli-hub info <name>
cli-hub install <name>
cli-hub uninstall <name>
cli-hub update <name>
cli-hub launch <name>
cli-hub matrix ...
cli-hub previews ...
```

这让 agent 不需要知道每个软件怎么安装。agent 可以先问 hub：

```text
what tool can handle Zotero?
what tool can handle image editing?
what tool can handle video subtitles?
what capabilities are available for a video workflow?
```

然后按 registry 返回的信息安装和使用。

## Registry Layer

`registry.json` 是 CLI-Anything 自己的 harness catalog。本地有 76 个条目。

每个条目包含：

```json
{
  "name": "zotero",
  "display_name": "Zotero",
  "version": "0.4.1",
  "description": "...",
  "requires": "...",
  "homepage": "...",
  "source_url": null,
  "install_cmd": "...",
  "entry_point": "cli-anything-zotero",
  "skill_md": "skills/cli-anything-zotero/SKILL.md",
  "category": "office",
  "contributors": []
}
```

`public_registry.json` 是外部 public CLI catalog。本地有 20 个条目，比如：

```text
feishu
minimax-cli
wecom
contentful
sanity
shopify
sentry
1password-cli
generate-veo-video
suno
elevenlabs
obsidian-cli
arcgis-pro
```

这说明 `CLI-Anything` 不只收自己生成的 harness，也把外部成熟 CLI 纳入同一个发现和安装体系。

本地 harness 分类大概是：

| Category | Count |
|---|---:|
| ai | 7 |
| graphics | 6 |
| web | 6 |
| devops | 6 |
| video | 5 |
| office | 4 |
| gamedev | 3 |
| image | 3 |
| automation | 3 |
| others | 33 |

这不是只服务 creative tools，它已经覆盖 devops、knowledge、office、finance、science、debugging、database 等方向。

## Matrix Layer

`matrix_registry.json` 是我觉得非常有启发的一层。

单个 CLI 是一个工具。matrix 是一个完整 workflow 的 capability map。

本地有 5 个 matrix：

| Matrix | Category | Capabilities | CLIs |
|---|---|---:|---:|
| `video-creation` | video | 19 | 14 |
| `knowledge-research` | knowledge | 12 | 13 |
| `3d-cad` | 3d | 12 | 6 |
| `game-development` | game | 10 | 9 |
| `image-design` | image | 9 | 7 |

matrix 里的基本对象是：

```text
capability
  -> intent
  -> inputs
  -> outputs
  -> providers
```

provider 又可以是：

```text
harness-cli
public-cli
python
native
api
agent-skill
agent-native
web-search
```

这就从“装工具”升级成“根据任务能力选择工具组合”。

比如 video creation 不是一个 CLI 能做完的，它会拆成：

```text
storyboard planning
video search
video download
music search
music download
screen capture
video generation
voice generation
caption
editing
thumbnail
quality review
```

这对我们做 quant research 有直接启发。未来我们也可以设计：

```text
quant-research matrix
  capability.factor_ideation
  capability.data_ingestion
  capability.alpha_backtest
  capability.bias_diagnosis
  capability.portfolio_analysis
  capability.report_generation
  capability.public_safe_export
```

每个 capability 再绑定不同 provider：

```text
LLMQuant
Vibe-Trading
custom backtest CLI
LightRAG
DuckDB / Polars
broker API
report generator
```

这会比“写一个超级大 agent”更可维护。

## The 7-Phase Harness Pipeline

`cli-anything-plugin/HARNESS.md` 是整个项目的方法论核心。

生成一个新 CLI harness 要走 7 阶段：

| Phase | Name | Output |
|---|---|---|
| 1 | Codebase Analysis | 找 backend engine、API、数据模型、已有 CLI、undo system |
| 2 | CLI Architecture Design | 设计 command groups、state model、output format、REPL/subcommand |
| 3 | Implementation | 实现 data layer、probe/info、mutation、backend wrapper、render/export、session |
| 4 | Test Planning | 先写 `TEST.md`，列 unit/E2E/real workflow 测试计划 |
| 5 | Test Implementation | 写 `test_core.py`、`test_full_e2e.py`、subprocess tests |
| 6 | Test Documentation | 把真实测试结果追加到 `TEST.md` |
| 6.5 | SKILL.md Generation | 生成 agent 可读的 skill |
| 7 | Publishing | setup.py、namespace package、console script、pip install |

这个流程对我们非常有价值。它实际上是在训练 agent 做完整工程：

```text
understand software
design interface
implement wrapper
test real behavior
document proof
publish package
make it discoverable
```

这和我们想训练自己的 R&D Agent 很像。

## Harness Anatomy

一个标准 harness 通常长这样：

```text
<software>/
  agent-harness/
    <SOFTWARE>.md
    setup.py
    cli_anything/
      <software>/
        README.md
        __init__.py
        __main__.py
        <software>_cli.py
        core/
        utils/
          repl_skin.py
          <software>_backend.py
        skills/
          SKILL.md
        tests/
          TEST.md
          test_core.py
          test_full_e2e.py
```

几个关键要求：

```text
use Click
support --json
support REPL by default
support one-shot subcommands
use real software backend
have clear install instructions
have unit tests and real backend E2E tests
generate SKILL.md
package as cli-anything-<software>
```

这就是 agent-native software 的工程标准。

## Real Software, Not Toy Reimplementation

`HARNESS.md` 里最重要的规则是：

```text
Use the real software. Do not reimplement it.
```

也就是说，GIMP CLI 不应该用 Pillow 假装 GIMP；Blender CLI 不应该只生成一个玩具 scene parser；LibreOffice CLI 不应该自己写 PDF renderer。

正确做法是：

```text
manipulate native project/intermediate format
  -> call real backend
  -> verify real output
```

例子：

```text
LibreOffice -> libreoffice --headless --convert-to pdf
Blender     -> blender --background --python script.py
GIMP        -> gimp -i -b ...
Inkscape    -> inkscape --actions ...
Kdenlive    -> melt project.mlt
OBS         -> obs-websocket
Browser     -> DOMShell MCP server
Zotero      -> SQLite + connector + Local API
```

这条规则非常务实。它承认成熟软件的渲染、导出、内部模型很复杂，不要用简化版 Python 逻辑去冒充。

## Rendering Gap

`HARNESS.md` 还特别强调 rendering gap。

问题是：很多 GUI 软件的效果是在 render engine 里应用的。你改了项目文件，不代表导出工具真的会应用这些效果。

典型错误：

```text
CLI modifies timeline filters
  -> export uses naive ffmpeg concat
  -> filters silently disappear
```

正确策略是：

```text
native engine
  -> translated filtergraph
  -> render script
```

并且测试不能只看 exit code。必须验证：

```text
file exists
file size reasonable
magic bytes correct
ZIP / OOXML structure correct
frame brightness / audio RMS / duration correct
```

这对我们做 quant 也有对应关系：不能只看 backtest 跑完，必须验证：

```text
data range correct
universe correct
lookahead avoided
transaction cost applied
benchmark aligned
output metrics credible
```

工程上，这是一种同构问题。

## Preview Protocol

`CLI-Anything` 还有一个很关键的抽象：preview bundle。

`docs/PREVIEW_PROTOCOL.md` 定义了跨 harness 的中间结果预览协议。

一个 preview bundle 结构大概是：

```text
<bundle_dir>/
  manifest.json
  summary.json
  artifacts/
    hero.png
    gallery_01.png
    preview.mp4
    pipeline_diff.json
```

核心规则：

```text
harness produces preview bundle
cli-hub previews reads bundle
agent or human inspects intermediate artifact
real renderer still remains the backend
```

live preview 还会有：

```text
session.json
trajectory.json
immutable bundle directories
```

这非常适合 agent 迭代。agent 不应该盲目连续写 20 个命令，而应该：

```text
run command
capture preview
inspect preview summary
compare trajectory
decide next command
```

这也可以迁移到 quant：

```text
run backtest
capture result bundle
summary.json: Sharpe, turnover, drawdown, IC, coverage
artifacts/: equity curve, drawdown plot, exposure plot
trajectory.json: factor changes and metrics
agent decides next research iteration
```

## Skills

每个 CLI 都应该有 `SKILL.md`。

它的作用不是给人看的 README，而是给 agent 的 operation manual：

```text
when to use this CLI
how to install
what commands exist
how to request JSON output
what errors mean
what workflow examples are safe
what preview commands exist
```

本地根目录 `skills/` 里有 70 个 canonical skills。

同时还有 `cli-hub-meta-skill`，它让 agent 可以先发现工具，再安装工具：

```bash
pip install cli-anything-hub
cli-hub list
cli-hub search image
cli-hub install gimp
cli-hub info gimp
```

这和 `nanobot` 的 skill 系统能直接接起来：

```text
nanobot loads cli-hub-meta-skill
  -> discovers relevant CLI
  -> installs or checks availability
  -> reads CLI-specific SKILL.md
  -> uses CLI through shell/tool layer
```

## Platform Adapters

`CLI-Anything` 不只面向 Claude Code。

本地能看到：

```text
cli-anything-plugin/
.pi-extension/
codex-skill/
hermes-skill/
reasonix-skill/
qoder-plugin/
opencode-commands/
cli-hub-meta-skill/
```

这说明它把核心方法论抽象成：

```text
HARNESS.md
commands/
guides/
scripts/
templates/
```

然后不同 agent 平台只是 adapter。

这对我们很重要。真正好的 Research OS 不应该绑定某个 agent UI。它应该把能力层抽出来：

```text
methodology
tools
skills
state
artifacts
```

然后可以被 Codex、nanobot、Claude Code 或未来的 agent host 调用。

## Representative Harnesses

我读了几个代表性 harness。

### Blender

Blender harness 支持：

```text
scene
object
material
modifier
camera
light
animation
render
preview
session
```

它可以生成 scene JSON，再通过 Blender backend 做真实渲染。它还有 preview 和 live preview：

```bash
cli-anything-blender --json --project scene.json preview capture --recipe quick
cli-anything-blender --json --project scene.json preview live status --recipe quick
cli-hub previews inspect /path/to/bundle-or-session
```

这代表了复杂 artifact 生成类软件的路径。

### Browser

Browser harness 使用 DOMShell MCP，把 Chrome accessibility tree 映射成 filesystem-like interface：

```text
fs ls
fs cd
fs cat
fs grep
act click
act type
page open
page info
```

这很有意思。它不是屏幕点击，而是把网页结构变成可读、可搜索、可行动的 tree。

这比传统 browser automation 更贴近 agent：

```text
agent explores structure
agent reads element
agent clicks by path
agent types by path
```

### Zotero

Zotero harness 明确说自己不重写 Zotero，而是组合真实本地 surface：

```text
SQLite for offline read-only inventory
connector endpoints for official write flows
Local API for citation, bibliography, export, live search
```

这对我们的科研工作流直接有用：

```text
find paper
inspect collection
import RIS / BibTeX / JSON
attach PDF
export BibTeX / CSL JSON
generate LLM context
```

如果以后把 `Zotero + LightRAG + nanobot` 接起来，就可以得到很强的 literature workflow。

## Relation To Previous HKUDS Projects

现在四个项目可以拼起来看：

| Project | System Layer | Role |
|---|---|---|
| `LightRAG` | Research Memory Layer | 结构化知识、图谱检索、source-grounded memory |
| `Vibe-Trading` | Quant Workflow Layer | finance question 到策略、回测、报告、研究证据 |
| `nanobot` | Personal Agent Shell | 多入口、session、memory、tools、cron、MCP、WebUI |
| `CLI-Anything` | Software Action Layer | 把外部软件变成 agent 可发现、可调用、可测试的 CLI |

系统图可以这样画：

```text
Human / PM
  |
  v
nanobot
Personal Agent Shell
  |
  +--> LightRAG
  |    research memory
  |
  +--> Vibe-Trading / LLMQuant
  |    quant research workflow
  |
  +--> CLI-Anything
       external software action layer
       |
       +--> Zotero / Browser / LibreOffice / Obsidian
       +--> Blender / GIMP / Kdenlive / QGIS
       +--> custom quant CLIs
```

`nanobot` 负责 agent runtime，`CLI-Anything` 负责把软件世界变成工具世界。

## Why It Matters For Pengyi Research OS

我们要做的不是一个“聊天机器人网站”。我们要做的是可以持续产出的 AI scientist / quant research operating system。

这要求系统能完成：

```text
read papers
manage references
search web
run code
run backtests
generate figures
write reports
publish website
maintain private notes
produce public-safe artifacts
```

`CLI-Anything` 给我们的启发是：每一个外部软件都应该尽量变成 agent-native CLI。

对我们近期最有用的组合可能是：

| Need | Candidate CLI Layer |
|---|---|
| paper/reference management | Zotero CLI |
| web exploration | Browser CLI |
| notes/knowledge base | Obsidian/Joplin CLI |
| report generation | LibreOffice CLI |
| diagram generation | Draw.io / Mermaid CLI |
| local model management | Ollama CLI |
| data cleaning | OpenRefine CLI |
| public website artifacts | custom site publish CLI |

对 quant 方向，未来可以自己做：

```text
cli-anything-factor-lab
cli-anything-backtest
cli-anything-worldquant-sandbox
cli-anything-report-pack
cli-anything-data-quality
```

这些不一定要进 CLI-Anything 官方 repo，但可以学习它的 harness 标准：

```text
--json
REPL
real backend
TEST.md
SKILL.md
preview bundle
registry entry
```

## Possible Quant Research Matrix

我现在很想把 matrix 思路迁移到量化研究。

可以先设计一个私有的：

```text
pengyi-quant-research matrix
```

capabilities：

```text
literature.search
literature.extract
factor.ideate
data.ingest
data.validate
factor.implement
backtest.run
backtest.diagnose
risk.exposure
report.generate
public.sanitize
memory.writeback
```

providers：

```text
LightRAG
Vibe-Trading
LLMQuant
DuckDB / Polars
custom backtest CLI
Zotero CLI
Browser CLI
LibreOffice CLI
website publish scripts
```

这比直接喊“我要一个超级量化 agent”更具体。因为每个 capability 都可以独立测试、替换、扩展。

## PR Opportunities

如果我们之后想给 `CLI-Anything` 提 PR，比较自然的方向有：

| Direction | Possible PR |
|---|---|
| Windows docs | Windows + PowerShell + Python Scripts PATH + real backend setup notes |
| nanobot integration | `nanobot + cli-hub-meta-skill` 的使用文档 |
| quant matrix proposal | 一个 public-safe `finance-research` 或 `quant-research` matrix draft |
| Zotero workflow | Zotero + LightRAG literature pipeline example |
| Browser workflow | agent-native web research workflow example |
| Preview docs | 把 preview bundle 映射到 research artifacts 的范例 |
| Chinese guide | 中文 quickstart 或 contributor guide |
| Harness improvements | 真实使用某个 harness 后补 bugfix/test/docs |

最好的路径仍然是：

```text
use it
find real gap
open issue
make small scoped PR
include test or doc evidence
```

不要为了 PR 而 PR。

## What It Is Not

也要讲清楚边界。

`CLI-Anything` 不是：

```text
GUI automation magic
market data source
quant engine
universal browser agent
replacement for real software
security sandbox
```

它是：

```text
software-to-CLI conversion methodology
CLI catalog and package manager
agent-readable skill ecosystem
real-backend harness standard
preview/artifact protocol
workflow matrix layer
```

它解决的是“agent 如何可靠使用软件”，不是“业务逻辑本身是什么”。

所以对我们来说，它不会替代 `Vibe-Trading` 或 `LLMQuant`，但它可以让这些系统调用更多外部软件和工具。

## Risks And Cautions

这个方向也有风险。

第一，CLI 一旦能操作真实软件，就有真实副作用：

```text
write files
send requests
modify local databases
install packages
call cloud APIs
delete artifacts
```

所以必须有 workspace boundary、dry-run、confirm、backup、public/private separation。

第二，真实软件依赖会带来环境复杂度：

```text
Blender version
Zotero local API
LibreOffice headless
Chrome extension
Node/npm
Python PATH
Windows path handling
```

第三，registry install command 是一个信任边界。公开使用时要看清楚来源、权限和安装路径。

第四，quant data 和私有策略不能随便接到 public workflow。我们的私有 Research OS 必须明确：

```text
private data stays private
public export must sanitize
agent shell cannot casually publish private notes
```

## My Current Conclusion

`CLI-Anything` 对我们最大的启发是：AI agent 的能力边界，不只取决于模型，也取决于它能否稳定操作真实软件。

一个可持续的 Research OS 应该是：

```text
agent shell
  + memory
  + workflow
  + software action layer
  + artifact protocol
  + public/private boundary
```

放到当前 HKUDS study map：

```text
nanobot      = agent shell
CLI-Anything = software action layer
LightRAG     = research memory
Vibe-Trading = quant workflow
```

这四个项目已经可以拼出一版非常清晰的 `Pengyi Research OS v0` 工程骨架。

下一步可以做 `HKUDS005`：把 `LightRAG + Vibe-Trading + nanobot + CLI-Anything + LLMQuant` 统一成一张系统架构图，并设计一个最小可运行 demo。
