---
document_id: awp-skill-development-standard/skill-core-file-declaration
language: en
publication: public
source_revision: 2
title: "SKILL.md File Standard"
purpose: Full standard for SKILL.md files: naming, folders, frontmatter, and workflow definitions
category: Standard
prerequisites: []
see_also:
  - advanced/skill-step-document-standard.md
  - advanced/skill-runtime-data-standard.md
---

# SKILL.md File Standard

> Scope: creation and maintenance of `~/.claude/skills/*/SKILL.md` files.
> Output responsibility: define Skill entry metadata, workflow coordination, folder structure, and distribution rules.
> Each Skill has one independent SKILL.md.

## Contents

- §0 Concise is Key
- §1 Naming Standard: four-level naming system
- §2 Full Folder Template, including scope independence, Knowledge Binding, and Runtime Contract
- §3 Frontmatter Standard: field definitions, hooks, and dynamic context injection
- §4 Document Size Guidance: three-level loading mechanism
- §5 Workflow Definition Standard: workflow table, executor, trigger conditions, and flattening
- §6 Progressive Disclosure Process: step-by-step reading and standard step01-init content
- §7 Complexity Growth: single step → short workflow → multi-step workflow
- §8 Full Template
- §9 Core Concepts: points to `advanced/skill-step-document-standard.md` §6-§8
- §10 Writing a Multi-Mode Skill
- §11 Content Types: Reference vs Task
- §12 Skill Distribution: personal / project / Plugin
- §13 Preloading Skills into a SubAgent
- §14 Skill Composition and Chained Calls
- §15 Skill Lifecycle
- Checklist

---

## 0. Concise is Key

> **Source**: Core principle from Anthropic's official Skill practices.

**Default assumption**: Claude is already very capable. Add only context Claude does not have.

Run three checks before adding any paragraph:

| Check | Question |
|------|------|
| Necessity | "Does Claude truly need this explanation?" |
| Prior knowledge | "Can I assume Claude already knows this?" |
| Token value | "Is this text worth its token cost?" |

**Good example** (~50 tokens):

```markdown
## Extract PDF Text
Use pdfplumber:
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

**Bad example** (~150 tokens):

```markdown
## Extract PDF Text
PDF (Portable Document Format) is a common file format that contains text, images,
and other content. To extract text from a PDF, you need to use a library. Many
libraries are available, but pdfplumber was chosen because it is easy to use...
```

> **description matters more than instructions**. description is the only basis Claude uses to choose from more than 100 Skills. Spend more time refining description than piling up instructions.

### 0.1 Cross-Model Compatibility Guide

A Skill should work with Haiku / Sonnet / Opus. Different models need different levels of guidance:

| Model | Traits | Writing Guidance |
|------|------|---------|
| **Haiku** | Fast, but needs more guidance | Use numbered steps, give specific examples, and avoid implicit assumptions |
| **Sonnet** | Balanced | Clear, direct instructions are enough |
| **Opus** | Strong, but may over-explain | Avoid repeated instructions and do not restate concepts Claude knows |

**Shared form** that works with all three models:

- Use **numbered steps** for critical operations because Haiku needs them
- Make each step do one thing because Haiku needs this
- Do not explain concepts Claude already knows because Opus does not need them
- Mark critical constraints in **bold** because every model benefits
- Give 1-2 concrete examples because Haiku needs them and they do not hurt Opus

---

## Structure

| # | Question | Section | Main Content |
|---|------|------|---------|
| §0 | What is the core principle for writing SKILL.md? | Concise is Key | Assume Claude already knows; use three checks for token value |
| §1 | How is a Skill named? | Naming Standard | Four-level naming system: awp-domain-target-action |
| §2 | How are folders organized? | Full Folder Template | Folder structure, scope independence, Knowledge Binding |
| §3 | How is metadata written? | Frontmatter Standard | Field definitions, hooks, dynamic context injection |
| §4 | How large should the file be? | Document Size Guidance | Three-level loading mechanism and line-count reference |
| §5 | How is a workflow defined? | Workflow Definition Standard | Workflow table, executor, trigger conditions, flattening |
| §6 | How is content loaded during execution? | Progressive Disclosure Process | Step-by-step reading and standard step01-init content |
| §7 | How does a Skill grow from simple to complex? | Complexity Growth | Single step → short workflow → multi-step workflow |
| §8 | Is there a reusable template? | Full Template | SKILL.md skeleton template |
| §9 | Where are the core concepts? | Core Concepts | Points to `advanced/skill-step-document-standard.md` §6-§8 |
| §10 | How is a multi-mode Skill written? | Multi-Mode Skill | Mode declaration, triggers, folder structure, routing |
| §11 | Reference vs Task? | Content Types | Knowledge type vs task type and trigger choice |
| §12 | How is a Skill distributed? | Skill Distribution | Personal / project / Plugin |
| §13 | How is a Skill injected into a SubAgent? | Skill Preloading | The skills field in a custom Agent |
| §14 | How are several Skills chained? | Composition and Chained Calls | Data-handoff conventions and loose coupling |
| §15 | How is the lifecycle managed? | Skill Lifecycle | Create → test → publish → iterate → retire |

---

## 1. Naming Standard

### 1.1 Four-Level Naming System

```
awp-{domain}-{target}-{action}
   │        │       │        │
   │        │       │        └── Action: create/collect/sync/convert...
   │        │       └────────── Target: twitter/github/ppt/md...
   │        └────────────────── Domain: dev/doc/social/content...
```

**Examples**:

- `awp-social-twitter-creating` = social + Twitter + creation
- `awp-dev-feature-designing` = development + feature + design
- `awp-ghost-github-saving` = Ghost + GitHub + save

### 1.2 Domain Definitions

> **Namespace note**:
>
> - **Official Skills**: Use the `awp-` prefix and are maintained by AWP
> - **User extensions**: May use a custom namespace such as `mycompany-`, following the four-level `{namespace}-{domain}-{target}-{action}` structure
> - The list below contains officially supported domains. Users may add custom domains as needed

**General domains** (⚪ preferred):

| Domain | Prefix | Description | Typical Functions |
|------|------|------|---------|
| Development | `dev` | Full code-development flow | Feature design/development/review, build checks, error fixes |
| Documentation | `doc` | Document management | Markdown, tutorials, AWP knowledge bases, technical manuals |
| Social | `social` | Social-platform publishing | X, LinkedIn, Bluesky, and others |
| Content | `content` | Content creation | PPT, video, articles, chapters |
| Collection | `collect` | Information collection | Keywords, prompts, multi-source research |
| Utility | `util` | General utilities | Git, sessions, conversion, system operations |
| Skill | `skill` | Skill management | Review, publishing, cloning |
| Video | `video` | Video creation | Product videos, AI editing |
| SEO | `seo` | SEO improvement | Keyword research, competitor analysis |
| CMS | `cms` | CMS platform integration | WordPress, Ghost, and other content publishing |

**Platform-integration domains** (extend as needed; examples):

| Domain | Prefix | Description |
|------|------|------|
| GitHub | `github` | GitHub operations: repositories, Issues, and PRs |
| Automation | `automation` | Workflow-engine integration: n8n, Make, and others |
| Notes | `notes` | Notes and knowledge-base platforms: Notion, Obsidian, and others |

> Add platform-integration domains for third-party services that are actually used. The table does not limit the list.

### 1.3 Action Suffixes (Use -ing Form)

> These are common suffixes, not a closed list. Any gerund ending in -ing may be used when its meaning is clear.

| Suffix | Meaning | Example |
|------|------|------|
| `-designing` | Design | `feature-designing` |
| `-building` | Build/develop | `feature-building` |
| `-reviewing` | Review | `code-reviewing` |
| `-creating` | Create | `twitter-creating` |
| `-collecting` | Collect | `twitter-collecting` |
| `-publishing` | Publish | `wordpress-publishing` |
| `-syncing` | Sync | `calendar-syncing` |
| `-converting` | Convert | `bmc-converting` |
| `-finding` | Find | `tutorial-finding` |
| `-saving` | Save | `github-saving` |
| `-hunting` | Hunt/discover | `xhs-hunting` |
| `-fixing` | Fix | `error-fixing` |
| `-cloning` | Clone | `twitter-cloning` |
| `-generating` | Generate | `ppt-generating` |
| `-illustrating` | Illustrate | `twitter-illustrating` |

---

## 2. Full Folder Template

```
{skill-name}/
├── SKILL.md                      # Required - entry document
│
├── docs/                         # Optional - Skill tutorial documents
│   ├── guide.md                  # Beginner guide
│   └── setup.md                  # Environment setup
│   # ❌ Do not create changelog.md — Skills have no version number; see §3.3 red lines. git log is the version history
│
├── workflow/                     # Optional - step execution documents
│   ├── step00-preflight.md       # Required - startup preflight for a multi-step Skill
│   ├── step01-init.md
│   ├── step02-collect.md
│   ├── step03-process.md
│   └── step04-output.md
│
├── reference/                    # Optional - runtime reference resources, all in subfolders
│   │
│   ├── specs/                    # Framework/architecture definitions
│   │   └── *.md                  # Detection frameworks, exit conditions, process definitions
│   │
│   ├── rules/                    # Detection/validation rules
│   │   └── *.md                  # Specific detection rules
│   │
│   ├── patterns/                 # Repair patterns/practices
│   │   └── *.md                  # Common problem-repair strategies
│   │
│   ├── prompts/                  # Prompt templates
│   │   ├── README.md             # Prompt index
│   │   └── prompt-*.md           # Agent Prompt
│   │
│   ├── presets/                  # Preset data
│   │   ├── *.json                # Preset data
│   │   └── *.md                  # Preset notes
│   │
│   ├── templates/                # Output templates → see advanced/skill-config-parameter-standard.md §14 for the standard
│   │   ├── *.html                # HTML templates
│   │   ├── *.json                # JSON Schema templates
│   │   └── shared/               # Shared resources
│   │       └── *.css
│   │
│   └── definitions/              # Constant definitions
│       └── *-definitions.json    # Format, tone, status, and other definitions
│
├── scripts/                      # Optional - executable scripts
│   ├── python/                   # Python runtime, primary
│   │   ├── *.py
│   │   ├── shared/               # Shared modules, optional
│   │   │   ├── __init__.py
│   │   │   └── workflow.py
│   │   ├── pyproject.toml
│   │   └── uv.lock               # Optional - reproducible dependency lock
│   │
│   └── node/                     # Node.js runtime, optional
│       ├── package.json
│       ├── *.mjs
│       └── pnpm-lock.yaml         # Optional - reproducible dependency lock
│
├── config/                       # Optional - workflow configuration
│   ├── default.json
│   └── runtime.json              # Optional - environment/binary/model-asset declaration
│
├── credentials/                  # Optional - API credentials
│   └── *.md                      # Placeholder credentials; real Keys do not enter distribution packages
│
├── runs/                         # Optional - runtime data; excluded by .gitignore and not included in distributed run history
│   └── .gitignore
│
└── temp/                         # Optional - temporary files
```

Runtime data goes to `runs/` inside the Skill folder by default. This is self-contained and supports independent use and distribution. Exclude runtime data with `.gitignore`; distribution packages do not contain historical runs. After Runtime CLI takes full control, it may move to `~/.awp/runtime/runs/skills/{skill-name}/`. Preflight cache goes to `~/.awp/runtime/inventory/skills/{skill-name}.preflight.json`.

### 2.0 Skill Independence Principle (scope Levels)

> **Core concept**: A Skill declares its independence level through `scope`, which decides whether it may reference external knowledge resources.

#### scope Definitions

| scope | Meaning | External Dependencies | Distribution |
|-------|------|---------|--------|
| `personal` | Private Skill bound to a personal AWP knowledge base | May reference the AWP knowledge base through Knowledge Binding | Local machine only |
| `portable` | Distributable and fully self-contained Skill | No external dependency is allowed | Can be copied directly to any machine |

**Default**: Treat an undeclared Skill as `personal`.

**Frontmatter declaration**:

```yaml
---
name: awp-social-twitter-creating
scope: portable
---
```

#### Self-Contained Rules

The following rules are **required** for a `portable` Skill and **⚪ optional** for a `personal` Skill:

| Rule | portable | personal | Description |
|------|----------|----------|------|
| **Self-contained credential placeholders** | ✅ Required | ⚪ | Keep `tools/credentials/*.md` inside the Skill folder. Describe fields and retrieval only. Do not write real keys |
| **Self-contained scripts** | ✅ Required | ⚪ | Scripts under `scripts/` do not import modules outside the Skill |
| **Self-contained references** | ✅ Required | ⚪ | Files under `reference/` do not cite paths outside the Skill |
| **Self-contained dependency declarations** | ✅ Required | ⚪ | Keep `pyproject.toml` / `package.json` inside the Skill |
| **Self-contained Runtime declaration** | ✅ Required | ⚪ | If an environment, binary, or model is needed, provide `config/runtime.json` |
| **No hard-coded external paths** | Required | Not applicable | Do not use absolute paths such as `~/Downloads/`; personal Skills reference them declaratively through Knowledge Binding |

**Distribution test** for portable: Copy the Skill folder to another machine. `doctor` must clearly report missing items. After credentials are configured and Runtime is prepared, the Skill must run in full.

**Correct form** under portable rules:

```python
# ✅ Relative path inside the Skill
cred_path = Path(__file__).parent.parent / "credentials" / "api.md"
ref_path = skill_dir / "reference" / "rules.md"
```

**Wrong form** under portable rules:

```python
# ❌ Depends on an external path
cred_path = Path("~/.claude/knowledge base/credentials/api.md")
ref_path = Path("~/Downloads/shared-rules/rules.md")
```

#### scope Selection Guide

| Case | ⚪ Preferred scope | Reason |
|------|-----------|------|
| Personal content creation: articles and social posts | `personal` | Needs knowledge-base resources such as identity and style |
| Team-shared utility: code review or format conversion | `portable` | Has no personal knowledge dependency and must be shared across people |
| Open-source community Plugin | `portable` | Must be self-contained |
| Workflow bound to a personal knowledge system | `personal` | The AWP knowledge base is a core dependency |

---

#### Runtime Contract (Environment and Model Assets)

A self-contained Skill is **self-contained in declaration**, not **self-contained in physical files**.

**The Skill folder must contain**:

- `config/runtime.json`: declares runtime environments, system binaries, local models, cache policy, and preflight policy
- `docs/setup.md`: explains doctor / prepare / credential live-validation configuration
- Dependency lock files: `uv.lock`, `pnpm-lock.yaml`, PEP 723 inline metadata, and similar files
- Small static assets: templates, examples, small lookup tables, and small test samples

**The Skill folder must not contain**:

- `.venv`, `node_modules`
- OCR / ASR / VAD / diarization / vision / embedding / rerank / segmentation model weights
- Hugging Face cache or provider SDK cache
- Large `runs/` output, partial downloads, or temporary caches
- Real API Keys / tokens / cookies

**Store Runtime files in one place**:

```text
~/.awp/runtime/
├── envs/skills/{skill-name}/...
├── models/store/sha256/{sha256}/...
├── models/skills/{skill-name}/...
├── cache/skills/{skill-name}/...
├── runs/skills/{skill-name}/...
├── locks/
└── inventory/skills/{skill-name}.preflight.json
```

**Management command convention**:

```bash
python {skill_dir}/scripts/doctor.py
python {skill_dir}/scripts/prepare.py --profile standard
```

`doctor` diagnoses only and downloads nothing. Only `prepare` may create an environment and download models. Model storage is deduplicated by sha256 content. Store one copy of the same weight under `models/store/`; a Skill creates a logical reference through `models/skills/{skill-name}/`.

---

### 2.1 Add Only When Needed

| Folder | When to Add |
|------|----------|
| `SKILL.md` | Required |
| `docs/` | A beginner guide or environment configuration is needed |
| `docs/guide.md` | The workflow has several steps or needs beginner guidance |
| `docs/setup.md` | Scripts have dependencies or API credentials are needed |
| ~~`docs/changelog.md`~~ | ❌ **Do not create** — A Skill has no version number. git log is the history |
| `workflow/` | There are 4+ steps or one step document exceeds 50 lines |
| `reference/` | Runtime reference resources are needed |
| `reference/specs/` | Framework definitions or process standards exist |
| `reference/rules/` | Detection rules or validation logic exists |
| `reference/patterns/` | Repair patterns or practices exist |
| `reference/prompts/` | Agent Prompt templates exist |
| `reference/presets/` | Preset configuration such as personas or keywords exists |
| `reference/templates/` | Output templates such as HTML/CSS/JSON exist |
| `reference/definitions/` | Constant-definition files exist |
| `scripts/` | Script execution is needed |
| `scripts/python/` | The Python runtime is used |
| `scripts/python/shared/` | Two or more Python scripts share code |
| `scripts/node/` | The Node.js runtime is needed |
| `config/` | Parameter configuration is needed |
| `config/runtime.json` | A runtime environment, system binary, or local model asset is needed |
| `credentials/` | API credentials are needed |
| `temp/` | Test scripts or retired code exists |
| `runs/` | Create it when runtime data exists; exclude it with `.gitignore`; distribution packages contain no historical runs |

> **Create on demand**: Create only the subfolders that are actually needed. A simple Skill may need only `reference/prompts/`.

### 2.2 `reference/` Subfolder Decision Table

| Subfolder | When to Create | Typical Content | When It Does Not Apply |
|--------|---------|---------|-----------|
| `specs/` | The Skill has multi-stage process definitions or exit conditions | Process standards, architecture definitions | Single-step Skill |
| `rules/` | Detection/validation logic must be reused | Detection rules, validation conditions | Rules can stay inside the Prompt |
| `patterns/` | Common repair patterns exist | Repair strategies, practices | No reusable pattern is needed |
| `prompts/` | The Skill has SubAgent tasks | Prompt templates | No SubAgent step |
| `presets/` | Text question options need external data | Market lists, persona files | Options can be hard-coded |
| `templates/` | The Skill generates HTML or rich-text output | HTML templates, CSS | Plain JSON/Markdown output |
| `definitions/` | Enum constants must be reused | Format definitions, status codes | Fewer than 10 constants |

**Quick decisions**:

- Only a SubAgent → only `prompts/` is needed
- Has an output template → add `templates/`
- Has dynamic options → add `presets/`
- Has several complex stages → add `specs/`, `rules/`, and `patterns/` as needed

### 2.3 Knowledge Binding

> Applies to Skills with `scope: personal`. A Skill with `scope: portable` must not use it.

A Skill can obtain external context from the AWP knowledge base in several ways. **See `advanced/skill-context-loading-standard.md` for the full standard**. This section is only an overview.

#### Recommended Method: Local KB CLI + Direct Reading

The current knowledge-base context comes from the local file system and `awp-kb`. A Skill declares only the dimensions it must load. Do not hard-code specific AWP knowledge-base content in SKILL.md.

Standard capabilities; see `advanced/skill-context-loading-standard.md` §8:

- **Direct read**: identity, standards, workflow style libraries, and red-line lists
- **Exact verification**: `rg` / `find` checks old paths, fields, and indexes

#### Passive Defense: `!`command``

Use `!`command`` inside SKILL.md to inject light content such as lessons and negative examples automatically.

#### config context Declaration (Optional)

Declare dependent knowledge files in `config/default.json` as documentation:

```json
{
  "context": {
    "identity": {
      "source": "knowledge-base",
      "when": "init"
    },
    "writing_style": {
      "source": "knowledge-base",
      "when": "on_demand"
    }
  }
}
```


#### Common Knowledge Paths (General Classes)

> The AWP folder structure decides exact paths. The following are standard knowledge-dimension classes.

| Knowledge Dimension | Content | Retrieval Method |
|---------|------|---------|
| Identity and position | Brand position, values, red lines | CLAUDE.md preload or direct Read |
| Style guide | Language style, article structure | Direct read from the workflow style library |
| Lessons | Failure records and practices | KB CLI search + direct Read |
| Negative examples | Historical error patterns | KB CLI search + `rg` |
| Standard red lines | Platform limits and compliance requirements | Direct Read |
| Published work | Avoid repetition and cite internal links | KB CLI search + `rg` |
| Reference cases | Data and competitor analysis | Search under `{reference_root}` / `research/`, then choose files to Read |

#### Use in a Step

Declare prerequisite knowledge in `workflow/step*.md`:

```markdown
## Prerequisite Knowledge
- Read: context.writing_style
- Read: context.writing_structure
```

At runtime, the Agent Reads the matching files before executing the step.

#### Forbidden Actions

| Forbidden | Reason |
|------|------|
| Hard-code knowledge-base paths in the SKILL.md body | Declare them with config context |
| Point a knowledge-base source to a non-AWP knowledge-base path | The AWP knowledge base is the only external knowledge source |
| Let a portable Skill use a knowledge-base source without a fallback | It will not run after distribution |

---

## 3. Frontmatter Standard

> Based on the official Claude Code standard: https://code.claude.com/docs/en/skills

### 3.0 Field Naming Rules (Official vs SDK)

**Decision**: SKILL.md frontmatter uses **kebab-case**. SDK/code configuration uses **snake_case**.

| Case | Naming Style | Example |
|------|----------|------|
| SKILL.md frontmatter | kebab-case | `allowed-tools`, `disable-model-invocation` |
| SDK/code configuration | snake_case | `allowed_tools`, `disable_model_invocation` |

### 3.1 Required Fields

| Field | Rule | Description |
|------|------|------|
| `name` | ≤64 characters, lowercase letters/numbers/hyphens only, with reserved words anthropic and claude forbidden | Unique Skill identifier; should match the folder name |

### 3.2 ⚪ Standard Fields

| Field | Rule | Description |
|------|------|------|
| `description` | ≤1024 characters, third person | What it does + when it triggers; when omitted, the first Markdown paragraph is used |

### 3.3 Optional Fields

> Fields supported as of Claude Code v2026-03. New versions may add fields. Follow the official documentation.

| Field | Description | Example |
|------|------|------|
| `allowed-tools` | Tool allowlist while the Skill is active; see details below | `"Read Write Bash"` |
| `model` | Model used while the Skill is active | `claude-sonnet-4-6` |
| `context` | Set to `fork` to run in an isolated subagent context | `fork` |
| `agent` | Agent type when `context: fork` | `general-purpose`, `Explore`, `Plan` |
| `hooks` | Skill lifecycle hooks; see §3.5 | `PreToolUse`, `PostToolUse`, `Stop` |
| `user-invocable` | Whether it appears in the slash-command menu; default `true` | `false` |
| `disable-model-invocation` | Prevent Claude from triggering the Skill automatically; user calls only | `true` |
| `argument-hint` | Argument hint shown in autocomplete | `[issue-number]`, `<url>` |
| `license` | License identifier | `MIT` |
| `compatibility` | Compatibility marker | `claude-code>=2.1` |

> **❌ Forbidden field**: Do not use `version` in frontmatter. Do not put a "Change Log" table or "vX→vY changes" table in the SKILL.md body. Do not put a vN.M marker in description. git log is the version history. Use `git log --follow` for history.

> **argument-hint syntax limit**: It accepts only a plain string. YAML sequence syntax, either `[a, b]` or `- a\n- b`, causes a React UI crash (#25826).
>
> | Syntax | Valid | Example |
> |------|------|------|
> | Plain string | ✅ | `argument-hint: "[issue-number]"` |
> | String with spaces | ✅ | `argument-hint: "<repo-url> [target-dir]"` |
> | YAML sequence | ❌ **Forbidden** | `argument-hint: ["arg1", "arg2"]` |

> **allowed-tools details**: Limits the tools available while the Skill is active. It supports glob patterns. When unset, every tool is available.
>
> | Format | Example | Description |
> |------|------|------|
> | Space-separated | `"Read Write Bash"` | Standard form |
> | Bash glob | `"Bash(python:*) Bash(npm:*)"` | Limits Bash to command prefixes |
> | MCP wildcard | `"mcp__github__*"` | Allows all tools from one MCP |
> | Mixed | `"Bash(python:*) Read Write WebFetch mcp__github__*"` | Combines forms |

### 3.4 name Field Rules

| Rule | Description |
|------|------|
| Length | ≤64 characters |
| Characters | Lowercase letters, numbers, and hyphens only |
| Forbidden | XML tags and reserved words anthropic and claude |
| ✅ Use gerund form | -ing form: `processing-pdfs`, `analyzing-data` |

### 3.5 description Field Rules

| Rule | Description |
|------|------|
| Length | Non-empty and ≤1024 characters |
| Person | Third person; do not use "I can help" or "You can use" |
| Content | What it does + when it triggers |
| Format | `{core action description}. {trigger explanation}` |

**Examples**:

```yaml
---
name: awp-simple-tool
description: A simple utility. Triggers when the user says "tool" or "convert."
---
```

```yaml
---
name: awp-pdf-processing
description: Extracts text and tables from PDF files. Triggers when the user needs to process a PDF file.
allowed-tools:
  - Read
  - Bash
model: claude-sonnet-4-6
---
```

**Trigger improvement (search mindset)**: Claude treats description as a "search index" that matches user intent.

| Method | Description | Example |
|------|------|------|
| **Positive triggers** | Natural language the user may say | `Triggers when the user says "clone," "copy," or "recreate"` |
| **Negative triggers** | Cases that do not apply | `Not for simple data browsing; use data-viz skill` |
| **Synonym coverage** | Full terms, abbreviations, and casual forms | `PDF/document/paper` |
| **File-type declaration** | State specific file types when relevant | `Processes .csv and .xlsx files` |
| **Proactive coverage** | List cases that apply even without the exact word | `Triggers when the user mentions data visualization or internal metrics, even without saying "dashboard"` |

> **Write broad, not narrow**: Claude currently tends to **miss triggers**, meaning it does not use a Skill when it should, more often than it triggers wrongly. A description should list applicable cases broadly instead of stating only the most typical use. Source: the official anthropics/skills skill-creator. After writing it, run both directions of `advanced/skill-testing-process-standard.md §2 Trigger Testing`.

### 3.6 hooks Field Details

A Skill can define lifecycle hooks in `hooks` that run shell commands at specific events.

**Hook events available at Skill level**:

| Hook | Trigger Time | Use |
|------|---------|------|
| `PreToolUse` | Before a tool call; may block | Validate parameters and stop dangerous operations |
| `PostToolUse` | After a tool call | Process results, format, and log |
| `PostToolUseFailure` | After tool execution fails | Error handling and fallback logic |
| `SubagentStart` | When a SubAgent starts | Logging and resource preparation |
| `SubagentStop` | When a SubAgent finishes | Result validation and cleanup |
| `Stop` | When Skill execution ends | Clean temporary files and send notifications |

> See `advanced/skill-platform-constraint-limits.md` §3.4 for the **full event matrix**, including 21 global events. Skill-level hooks support only the six tool/Agent events above.

**Configuration example**:

```yaml
---
name: awp-data-processor
description: Data-processing workflow.
hooks:
  PreToolUse: echo "Tool starting: $TOOL_NAME"
  PostToolUse: echo "Tool completed: $TOOL_NAME"
  Stop: rm -rf /tmp/skill-cache-*
---
```

**Hook environment variables**:

| Variable | Description |
|------|------|
| `$TOOL_NAME` | Current tool name |
| `$TOOL_INPUT` | Tool input parameters as JSON |
| `$TOOL_OUTPUT` | Tool output; PostToolUse only |

### 3.7 Dynamic Context Injection

Use `!`command`` syntax to run a shell command **before** the Skill is sent to Claude and inject the output into the document.

**Syntax**:

```markdown
!`shell-command`
```

**Example**:

```yaml
---
name: awp-pr-reviewer
description: Reviews PR code. Triggers when the user says "review PR."
context: fork
agent: Explore
---

## PR Context

Current PR changes:

!`gh pr diff`

Changed files:

!`gh pr diff --name-only`

Recent commits:

!`git log --oneline -5`
```

**Execution flow**:

```
User triggers /awp-pr-reviewer
    ↓
System executes every !`command` statement
    ↓
Command output replaces each placeholder
    ↓
Full document is sent to Claude
```

**Use cases**:

| Case | Example Command |
|------|---------|
| PR review | `!`gh pr diff`` |
| Environment data | `!`uname -a`` |
| Git status | `!`git status --short`` |
| Dependency version | `!`node --version`` |
| Skill folder | `${CLAUDE_SKILL_DIR}`; replaced automatically with the Skill installation path |

**Notes**:

- Commands run on the user's machine, so confirm they are available
- Command output counts toward the Skill document size
- Avoid commands that take too long
- Do not inject sensitive information such as keys this way

---

## 4. Document Size Guidance

> These are official guidelines based on Claude Code progressive loading, not hard limits.

### 4.1 Three-Level Loading Mechanism

| Level | Content | When Loaded | Reference Size |
|------|------|----------|----------|
| Level 1 | Frontmatter: name + description | Always | ~100 tokens/Skill |
| Level 2 | SKILL.md body | When triggered | ≤800 lines; may be moderately larger with 1M |
| Level 3+ | Referenced files under workflow/ and reference/ | On demand | **No limit** |

> **1M context note**: The Level 2 guideline rises to ≤800 lines. SKILL.md size is no longer the main bottleneck with a 1M context window, but concise writing still helps Claude understand intent quickly.

### 4.2 SKILL.md Reference Values

| Rule | Description |
|------|------|
| ≤800 lines | May be moderately larger with 1M; content beyond this should still move to reference/ |
| Reference depth ≤2 hops | A step→prompt one-hop reference is allowed; avoid nested chains beyond two hops |
| Add TOC above 100 lines | Helps Claude preview and locate content |

### 4.3 Step/Reference Documents

| Rule | Description |
|------|------|
| No hard limit | Progressive loading at Level 3+ uses context only when accessed |
| Add TOC above 100 lines | Helps Claude preview and locate content |
| Files above 10k words | Provide grep search patterns in SKILL.md |

---

## 5. Workflow Definition Standard

### 5.1 Workflow Table

**Required fields**: Step / Responsibility / Executor / Document / Input / Output

```markdown
## Workflow (Step 00 + N Steps)

| Step | Responsibility | Executor | Document | Input | Output |
|------|------|--------|------|------|------|
| 00 | Startup preflight | Main Agent + script | `workflow/step00-preflight.md` | Skill folder | `-` |
| 01 | Initialize | Main Agent | `workflow/step01-init.md` | User trigger | `state/` |
| 02 | Collect data | Script | `workflow/step02-collect.md` | User parameters | `step02-collect/` |
| 03 | Filter data | Script | `workflow/step03-filter.md` | step02-collect/ | `step03-filter/` |
| 04 | Analyze | SubAgent | `workflow/step04-analyze.md` | step03-filter/ | `step04-analyze/` |
| 05 | Generate output | Script | `workflow/step05-output.md` | step04-analyze/ | `output/` |
```

**Responsibility-column standard**:

| Rule | Description |
|------|------|
| 2-4 words | Describe the step function briefly |
| Verb-object structure | Examples: data collection, log analysis, output generation |
| No backticks needed | Responsibility is descriptive text, not a path |

**Output-column standard**:

| Output Type | Format | Example |
|----------|------|------|
| Step folder | `step{NN}-{action}/` | `step02-collect/` |
| Final output | `output/` | `output/` |
| No output | `-` | `-` |
| Batch folder | `batches/` | `batches/` |
| Phase folder | `{phase}/` | `collect/` |

**Output-column rules**:

| Rule | Description |
|------|------|
| Wrap in backticks | Define an output folder as \`step01-collect/\` |
| Exact alignment | The folder-name `{action}` must match the workflow filename |
| Automatic creation | At run start, parse the table and create output folders; this is an AWP runtime-library feature. Without the runtime library, a script creates them explicitly |
| Fixed folders | `state/` and `output/` are fixed and may be declared or omitted in the table |
| No-output marker | Use `-` when a step produces no folder output |

**Folder-generation example**:

The \`step01-collect/\`, \`step02-filter/\`, and \`step03-analyze/\` rows in the workflow table are created automatically during Step01 runtime-folder initialization. Step00 uses `-`, so it creates no business folder:

```
{run_dir}/
├── state/           # Fixed
├── output/          # Fixed
├── step01-collect/  # Parsed from the table
├── step02-filter/   # Parsed from the table
└── step03-analyze/  # Parsed from the table
```

> **AWP runtime-library implementation**: See the `parse_workflow_dirs()` and `init_run_dir()` functions in `advanced/skill-runtime-data-standard.md` for folder parsing and creation.

### 5.2 Executor Types

→ See `advanced/skill-step-document-standard.md` §3.2 for five executor definitions and selection rules.

### 5.3 Trigger-Condition Table

**Must include**: keyword → action mapping

```markdown
## Trigger Conditions

| Keyword | Action |
|--------|------|
| "keyword1" "keyword2" | Run the full flow |
| "keyword3" | View results / run part of the flow |
```

### 5.4 Step-Flattening Standard

**Core principle**: Organize steps under `workflow/ in a`✅ flat structure.

| Rule | Description |
|------|------|
| ✅ Flat structure | Each `stepNN-*.md` is one complete independent step |
| Internal organization | Use **bold**, tables, or subheadings inside the step |
| Complex step | When a step has complex logic, use internal subheadings such as §8 Data Collection / §8 Data Cleaning, but do not split them into separate step files |

**Correct example**:

```
workflow/
├── step01-collect.md     # Data collection
├── step02-evaluate.md    # Evaluation and scoring
├── step03-merge.md       # Merge results
└── step04-report.md      # Generate report
```

**⚠️ Avoid, except in complex workflows**:

> ⚠️ Deviation condition: Substeps may be split when they have strong data dependencies and fully independent logic. Mark the file header with `⚠️ Deviation: flattening rule | Reason: {specific reason}`.

```
workflow/
├── step08a-batch-split.md   # ⚠️ Avoid; prefer subheadings inside the step
├── step08b-batch-agent.md   # ⚠️ May split when dependencies are strong and logic is independent
├── step08-1-verify.md       # ⚠️ Avoid; unclear naming
└── step08.1-merge.md        # ⚠️ Avoid; unclear naming
```

---

## 6. Progressive Disclosure Process

### 6.1 Standard Execution Flow

```
User triggers /skill-name
    ↓
Read SKILL.md: only the workflow table, to understand the step overview
    ↓
Read workflow/step00-preflight.md
    ↓
Run Step 00 preflight: auto/strict/off; skip heavy checks on a cache hit
    ↓
Read workflow/step01-init.md
    ↓
Collect parameters in one text Q&A; see skill-step-document-standard.md §9
    ↓
Run Step 01 logic: initialize runtime folder
    ↓
Read workflow/step02-*.md → run Step 2
    ↓
...read and run step by step...
    ↓
Complete
```

### 6.2 Standard Content for step01-init.md

> Every multi-step Skill ✅ includes this. A single-step Skill may keep only parameter collection and omit keyword/runs items.
> Step00 handles startup preflight only and does not collect business parameters. Step01 remains the sole entry point for parameter collection and runtime-folder initialization.

| Content | Description |
|------|------|
| **Collect L-required parameters** | Collect core parameters that cannot be inferred in one text Q&A; see skill-step-document-standard.md §9.2 |
| **Infer L-inferred parameters independently** | Fill missing parameters from input, history, or domain knowledge; see skill-step-document-standard.md §9.6 |
| **Extract keyword** | Extract a folder-naming keyword from user input |
| Initialize runtime folder | Call `init_run_dir(skill_name, keyword)` to create the folder structure under the Runtime runs root |
| Write config.json | Merge user parameters + inferred parameters + defaults and mark each source |
| Initialize progress.json | Include `keyword`, `keyword_raw`, `preflight`, `directories`, and `resume_hint` |

**keyword extraction**: → See `advanced/skill-runtime-data-standard.md` §3 for full rules. Example: `"Claude Code tutorial" → "claude-code"`.

### 6.3 Notes

| Rule | Level | Reason |
|------|------|------|
| ⚠️ Avoid reading all workflow/*.md immediately after triggering | **⚪** | Step-by-step loading stays clearer and easier to understand. On deviation, mark `⚠️ Deviation: progressive disclosure | Reason: {reason}` |
| Put multi-step Skill parameter logic in step01-init.md | ✅ Standard | A single-step Skill without workflow/ may put it directly in SKILL.md |
| ❌ Execute without reading the step document | **Forbidden** | Critical logic may be missed |
| ⚠️ Avoid pre-reading reference documents for later steps | **⚪** | On-demand loading stays clearer. On deviation, mark `⚠️ Deviation: on-demand loading | Reason: {reason}` |

> **1M-era update**: Progressive disclosure changes from "needed to prevent overflow" to "a practice that keeps code clear." Reading every step document at once will not overflow a 1M context window, but step-by-step loading still helps Claude focus on the current step.

---

## 7. Complexity Growth

Start with the simplest form and expand as needed:

```
Only SKILL.md is needed
    ↓ needs several steps
Add workflow/
    ↓ needs an external API
Add scripts/ + credentials/
    ↓ needs reference documents
Add reference/
    ↓ needs configuration
Add config/
```

### 7.1 Single-Step Workflow

```
awp-util-format-converter/
└── SKILL.md
```

### 7.2 Short Workflow (2-3 Steps)

```
awp-collect-weather-api/
├── SKILL.md
├── scripts/
│   └── fetch_weather.py
└── credentials/
    └── weather-api.md
```

### 7.3 Multi-Step Workflow (4+ Steps)

```
awp-social-twitter-collecting/
├── SKILL.md
├── workflow/
│   ├── step00-preflight.md
│   ├── step01-resolve.md
│   ├── step02-collect.md
│   ├── step03-batch-split.md
│   └── step04-batch-agent.md
├── reference/
│   └── prompts/
│       └── prompt-batch-analysis.md
├── scripts/
│   └── fetch_tweets.py
├── config/
│   └── default.json
├── credentials/
│   └── twitter-x.md
```

---

## 8. Full Template

````markdown
---
name: awp-skill-name
description: What it does + when it triggers. Triggers when the user says "keyword."
---

# Skill Title

## Trigger Conditions
| Keyword | Action |
|--------|------|
| "keyword" | Run the full flow |

## Execution Rules
1. Read before doing: Before Step N, Read workflow/stepN-*.md
2. Do not skip steps: Run in 00→01→02→...→N order
3. Persist progress: Maintain state/progress.json

## Workflow (Step 00 + N Steps)
| Step | Responsibility | Executor | Document | Input | Output |
|------|------|--------|------|------|------|
| 00 | Startup preflight | Main Agent + script | `workflow/step00-preflight.md` | Skill folder | `-` |
| 01 | Initialize | Main Agent | `workflow/step01-init.md` | User trigger | `state/` |
| ... | ... | ... | ... | ... | ... |

## Credentials
Follow `tools/credentials/*.md` for local key sources. Put real keys in the local credential area or system keychain, not in the Skill folder.
````

---

## 9. Core Concepts

> Authoritative definitions for batching, round-based execution, iterative merging, progress persistence, minimal returns, and other core concepts are in `advanced/skill-step-document-standard.md` §6-§8. They are not repeated here.

---

## 10. Writing a Multi-Mode Skill

When a Skill supports several execution modes, follow these rules.

### 10.1 Multi-Mode Frontmatter Declaration

```yaml
---
name: awp-social-twitter-cloning
description: |
  Collects Twitter creator data in depth and generates an AI persona prompt.
  Supports two modes:
  - Clone mode: collect from the creator profile
  - Timeline mode: collect from a timeline
  Triggers when the user says "Twitter clone," "persona copy," or "clone."
---
```

**Key points**:

- List every supported mode clearly in description
- Give a short description of when each mode applies

### 10.2 Trigger-Word Design Standard

**Multi-mode trigger table**:

```markdown
## Trigger Conditions

| Keyword | Mode | Action |
|--------|------|------|
| "clone" "copy" "recreate" | Clone | Collect from a creator profile and generate a persona |
| "timeline" | Timeline | Generate a persona from existing timeline data |
| "view results" "result" | View | Show the most recent persona file |
```

**Trigger-word design principles**:

| Principle | Description |
|------|------|
| Mutual exclusion | Trigger words for different modes should not overlap |
| Natural language | Use ordinary user expressions |
| Synonyms | Support several phrasings of the same trigger |
| Default mode | Use the default when no mode is stated clearly |

### 10.3 Execution-Mode Table Standard

Add an execution-mode table to SKILL.md:

```markdown
## Execution Modes

| Mode | Description | Best Fit | Step Count |
|------|------|---------|--------|
| **Clone (Recommended)** | Collect deeply from a creator profile | New creator analysis, full clone | 6 steps |
| **Timeline** | Analyze timeline data | Existing data, fast analysis | 4 steps |
| **View** | View generated results | View, export | 1 step |

> **Note**: Use Clone by default when no mode is specified.
```

### 10.4 Multi-Mode Folder Structure

```
awp-social-twitter-cloning/
├── SKILL.md
├── workflow/
│   ├── clone/                    # Steps for Clone mode
│   │   ├── step01-resolve.md
│   │   ├── step02-collect.md
│   │   ├── step03-batch-split.md
│   │   ├── step04-batch-eval.md
│   │   ├── step05-merge.md
│   │   └── step06-generate.md
│   │
│   ├── timeline/                 # Steps for Timeline mode
│   │   ├── step01-locate.md
│   │   ├── step02-fetch.md
│   │   ├── step03-analyze.md
│   │   └── step04-generate.md
│   │
│   └── shared/                   # Shared steps, optional
│       └── step-output.md        # Shared output step
│
├── reference/
│   └── prompts/
│       ├── prompt-clone-eval.md  # Clone-mode Prompt
│       └── prompt-timeline.md    # Timeline-mode Prompt
└── ...
```

### 10.5 Workflow Tables (Multi-Mode)

```markdown
## Workflow

### Clone Mode (6 Steps)

| Step | Responsibility | Executor | Document | Input | Output |
|------|------|--------|------|------|------|
| 01 | Parse URL | Main Agent | `workflow/clone/step01-resolve.md` | Creator URL | Parsed username |
| 02 | Collect data | Script | `workflow/clone/step02-collect.md` | Username | `step02-collect/` |
| 03 | Split batches | Script | `workflow/clone/step03-batch-split.md` | Raw data | `batches/` |
| 04 | Evaluate batches | SubAgent | `workflow/clone/step04-batch-eval.md` | Batch data | `step04-eval/` |
| 05 | Merge results | Script | `workflow/clone/step05-merge.md` | Evaluation results | `step05-merge/` |
| 06 | Generate persona | SubAgent | `workflow/clone/step06-generate.md` | Merged data | `output/persona.md` |

### Timeline Mode (4 Steps)

| Step | Responsibility | Executor | Document | Input | Output |
|------|------|--------|------|------|------|
| 01 | Locate data | Main Agent | `workflow/timeline/step01-locate.md` | Runtime-folder path | Located data |
| 02 | Get timeline | Script | `workflow/timeline/step02-fetch.md` | Timeline path | `step02-fetch/` |
| 03 | Analyze content | SubAgent | `workflow/timeline/step03-analyze.md` | Raw data | `step03-analyze/` |
| 04 | Generate persona | SubAgent | `workflow/timeline/step04-generate.md` | Analysis results | `output/persona.md` |
```

### 10.6 Mode-Selection Logic

Implement mode selection in step01-init.md:

```markdown
## Step 01: Mode Selection and Initialization

### Decide the Mode

1. **Explicit choice**: The user's trigger words identify the mode
2. **Text Q&A**: If it cannot be decided, ask all questions in one text Q&A; do not use AskUserQuestion
3. **Default mode**: Use Clone when the user does not specify

### Text Q&A Mode Selection

When the trigger is unclear, output this question list and wait for one reply:

```markdown
Choose an execution mode:

1. **Clone mode** (default) — Collect deeply from a creator profile and generate a persona
2. **Timeline mode** — Generate a persona quickly from existing timeline data
3. **View results** — View the most recently generated persona file
```

### Mode Routing

\`\`\`python
mode = determine_mode(user_input)

if mode == "clone":
    # Read workflow/clone/step01-resolve.md
    # Run the Clone-mode flow
elif mode == "timeline":
    # Read workflow/timeline/step01-locate.md
    # Run the Timeline-mode flow
elif mode == "view":
    # Read and show output/persona.md directly
\`\`\`
```

### 10.7 Progress File (Multi-Mode)

→ See `advanced/skill-runtime-data-standard.md` §5 for the progress.json format. A multi-mode Skill adds the `mode` field to progress.json.

---

## 11. Content Types

Skill content has two types, which affect triggering and execution context:

### 11.1 Reference Content

Adds knowledge Claude applies while working: conventions, patterns, style guides, and domain knowledge. The content runs inline beside the conversation context.

```yaml
---
name: api-conventions
description: API design standard. Applies automatically when writing API endpoints.
---

When writing API endpoints:
- Use RESTful naming
- Return a consistent error format
- Include request validation
```

**Trait**: Claude decides automatically when to load it. No user trigger is needed.

### 11.2 Task Content

Gives Claude steps for a specific action such as deployment, commit, or code generation. Usually use `disable-model-invocation: true` to stop Claude from triggering it automatically.

```yaml
---
name: deploy
description: Deploys the application to production.
context: fork
disable-model-invocation: true
---

Deployment flow:
1. Run tests
2. Build the application
3. Push to the deployment target
```

**Trait**: Only the user triggers it manually through `/deploy`.

### 11.3 Selection Guide

| Condition | Type | frontmatter |
|---------|------|------------|
| Is it knowledge, a convention, or a style? | Reference | Default |
| Does it have side effects such as deployment or sending? | Task | `disable-model-invocation: true` |
| Is it background knowledge users should not call directly? | Reference | `user-invocable: false` |

---

## 12. Skill Distribution

### 12.1 Three Distribution Methods

| Method | Path | Scope | Skill Name |
|------|------|---------|-----------|
| **Personal** | `~/.claude/skills/<name>/` | All projects | `/name` |
| **Project** | `.claude/skills/<name>/` | Current project | `/name` |
| **Plugin** | `plugin/.claude-plugin/plugin.json` | Projects where installed | `/plugin:name` |

**Priority**: Enterprise > personal > project > Plugin

### 12.2 Plugin Distribution (Optional)

When a Skill must be shared across teams or communities, package it as a Plugin:

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json       # {"name": "my-plugin", "version": "1.0.0"}
├── skills/
│   └── my-skill/
│       └── SKILL.md
└── agents/               # Optional: custom Agent
```

- The Skill receives a namespace automatically: `/my-plugin:my-skill`
- Install through Marketplace: `/plugin install`
- Test locally: `claude --plugin-dir ./my-plugin`
- Hot reload: `/reload-plugins`

> **AWP workflow**: Personal Skills can use `~/.claude/skills/` directly and do not need Plugin packaging. Use a Plugin for external distribution.

---

## 13. Preloading Skills into a SubAgent

### 13.1 Preload through a Custom Agent

A custom Agent defined under `.claude/agents/` can preload Skills through the `skills` field:

```yaml
---
name: security-reviewer
description: Security review specialist
skills:
  - owasp-rules
  - code-patterns
---

You are a security review specialist...
```

**Effect**: The **full contents** of `owasp-rules` and `code-patterns` are injected into context when the Agent starts.

### 13.2 Comparison with `context: fork`

| Method | Initiator | Where Skill Content Goes | Best Fit |
|------|--------|-------------|---------|
| `context: fork` + `agent:` | Skill side | SKILL.md becomes the Agent task | Skill-driven; Agent provides capability |
| Agent `skills` field | Agent side | Full Skill content is injected into Agent context | Agent-driven; Skill provides knowledge |

### 13.3 Reference a Custom Agent in workflow

```python
Task(
    subagent_type="security-reviewer", # References .claude/agents/security-reviewer.md
    prompt="Review code security under {run_dir}/step02-code/"
)
```

---

## 14. Skill Composition and Chained Calls

### 14.1 Composition Pattern

Several Skills can form an end-to-end workflow. Each Skill keeps one responsibility:

```
/long-writing → /content-boosting → /content-polishing → /content-imaging → -publishing
   write long form     improve hit potential       polish             illustrate              publish
```

### 14.2 Data-Handoff Conventions

Skills pass data through **file paths**, not memory state:

| Method | Description | Best Fit |
|------|------|---------|
| **Output becomes input** | The upstream Skill's `output/` path is the downstream input | Same-folder workflow |
| **Conventional filename** | Upstream and downstream use a fixed filename such as `article.md` | Loosely coupled Skills |
| **$ARGUMENTS handoff** | `/next-skill /path/to/output.md` | Manual chaining |

### 14.3 Design Principles

| Principle | Description |
|------|------|
| **Single responsibility** | Each Skill does one thing; composition creates complex flows |
| **Clear input and output** | SKILL.md declares expected input and output formats |
| **Loose coupling** | A Skill does not hard-code upstream or downstream Skill names; file conventions connect them |
| **Independent operation** | Every Skill can be used alone; composition is optional |
| **Idempotency** | Repeated execution with the same input produces the same output |

### 14.4 Declare the Composition Interface in SKILL.md

```markdown
## Input and Output

| Type | Format | Description |
|------|------|------|
| Input | Markdown file | Article to process (.md) |
| Output | `{run_dir}/output/article.md` | Processed article |

## Upstream and Downstream Skills (Optional Composition)

| Position | Skill | Description |
|------|-------|------|
| Upstream | `/long-writing` | Provides a draft |
| Downstream | `/ghost-publishing` | Publishes to the Ghost site |
```

### 14.5 Chained-Call Trigger Methods

| Method | Description | Example |
|------|------|------|
| **Manual chain** | The user triggers Skills one by one | `/skill-a` → complete → `/skill-b output.md` |
| **Call inside a workflow step** | A step document directs a call to another Skill | `Step 05: Call -publishing to publish` |
| **Automatic chain** | After completion, the Skill suggests another available Skill | Completion report lists "Next: /next-skill" |

---

## 15. Skill Lifecycle

`Create → test → publish → iterate → stabilize → retire`

On retirement: Mark description with "Deprecated. Use /new-skill instead" and set `disable-model-invocation: true`.

---

## Checklist

**All Skills**:

- [ ] frontmatter contains name and description
- [ ] description format: `{core action}. Triggers when the user says "trigger."`
- [ ] name follows the four-level form: awp-domain-target-action
- [ ] name contains only lowercase letters/numbers/hyphens and is ≤64 characters
- [ ] SKILL.md exists at the folder root
- [ ] SKILL.md is ≤800 lines
- [ ] No deeply nested folders; ≤2 levels
- [ ] scope is declared as personal or portable
- [ ] Step00 is defined when scripts, credentials, Runtime assets, or a multi-step workflow exist: multi-step uses `workflow/step00-preflight.md`; a single step with dependencies uses a Step 00 section in SKILL.md. A pure knowledge-only single-step Skill with no external dependency is exempt

**Multi-Step Skill (Additional)**:

- [ ] Contains workflow/
- [ ] The workflow table has six columns: Step/Responsibility/Executor/Document/Input/Output
- [ ] The first workflow-table row is Step 00 preflight with output `-`
- [ ] Output folder names match workflow filenames
- [ ] step01-init.md handles parameter collection and keyword extraction
- [ ] The trigger table maps keyword → action

**Multi-Mode Skill (Additional)**:

- [ ] description lists every supported mode
- [ ] Trigger words are mutually exclusive across modes
- [ ] An execution-mode table exists with Mode/Description/Best Fit/Step Count
- [ ] A default mode exists

**portable Skill (Additional)**:

- [ ] Credentials/scripts/references/dependencies are all self-contained
- [ ] When an environment or model is needed, `config/runtime.json` and `docs/setup.md` exist
- [ ] `config/runtime.json` contains a `preflight` policy
- [ ] `.venv`, `node_modules`, model weights, provider cache, and real Keys are not distributed
- [ ] No external path is hard-coded
- [ ] Distribution testing passes: doctor reports missing items clearly on a new machine, and the Skill runs after prepare
