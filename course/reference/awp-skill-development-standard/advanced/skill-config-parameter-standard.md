---
document_id: awp-skill-development-standard/advanced/skill-config-parameter-standard
language: en
publication: public
source_revision: 2
title: "Parameter Configuration Standard"
purpose: Full standard for config/default.json, config/runtime.json, variable placeholders, and output templates
category: Standard
prerequisites:
  - ../skill-core-file-declaration.md
see_also:
  - skill-step-document-standard.md
  - skill-context-loading-standard.md
  - ../../awp-prompt-writing-standard/prompt-format-eight-part.md
---

# Parameter Configuration Standard

> This document is the full standard for `config/default.json`, `config/runtime.json`, and related configuration files.
> output responsibility: define the structure, storage location, and reading method for Skill runtime parameters, Runtime environment declarations, and model asset declarations.
> Each Skill has its own `config/default.json`.
> This document also contains the **variable placeholder** (§13) and **output template** (§14) standards. It is the shared entry point for the configuration layer and static assets.

---

## 0. Design Philosophy

| Principle | Description |
|------|------|
| ✅ **Separate three layers** | Separate the interaction/configuration/preset layers so a Skill can work in different deployment environments |
| ✅ **Do not mix user configuration** | Do not store user runtime parameters (L1) together with Skill defaults (L2) |
| ✅ **Sensible defaults must run** | L2 parameters must support a full run without changes |
| ✅ **Separate declarations from files** | `config/runtime.json` only declares environments/models; their files go under `~/.awp/runtime/` |
| ✅ **State the preflight policy** | Write Step00 required/mode/cache/invalidation rules in `config/runtime.json` |

---

## Organization

| # | Question | Section | Main content |
|---|------|------|---------|
| §0 | Why use these layers? | Design Philosophy | Separate three layers, do not mix user configuration, and run with sensible defaults |
| §1 | What is the role of the configuration file? | File Responsibilities | The role of config/default.json and how it differs from other configuration |
| §2 | How many parameter layers are there? | Three-Layer Configuration Model | Structure and priority of the L1 interaction/L2 configuration/L3 preset layers |
| §3 | How are parameters collected at runtime? | L1 Interaction Layer | Collect main parameters through runtime text questions and fill in inferred values |
| §4 | How are defaults organized? | L2 Configuration Layer | Grouping in default.json and how scripts read it |
| §5 | How is business-logic configuration written? | Domain Configuration | Business-logic configuration in config/domain.json (⚪ optional) |
| §7 | How are parameters declared publicly? | SKILL.md Parameter Declaration | Declare configurable parameters in SKILL.md |
| §8 | What are good configuration practices? | Good Practices | Ask fewer questions, use sensible defaults, and organize by group |
| §9 | What is prohibited? | Prohibited Items | Limits on deep nesting, sensitive data, runtime data, and more |
| §10 | How is option data stored? | Preset Data | Rules for option data sources under reference/presets/ |
| §11 | How are enum constants defined? | Constant Definitions | Rules for enum definitions under reference/definitions/ |
| §12 | How are environments and models declared? | Runtime Contract | Environment/binary/model/preflight declarations in config/runtime.json |
| §13 | How are variable placeholders used? | Variable Placeholders | Single definition for workflow variables + official CC variables + keyword generation |
| §14 | How are output templates written? | output Templates | HTML/CSS template structure and placeholders under reference/templates/ |

---

## 1. File Responsibilities

**Role**: Store defaults for advanced Skill runtime parameters

**Location**: `config/default.json`

**How it differs from other configuration**:
- config/default.json = runtime parameter defaults (batch_size, timeout)
- config/runtime.json = declarations for environments, system binaries, local models, cache policy, and Step00 preflight policy
- reference/presets/ = option data sources (provides choices for text questions)
- reference/definitions/ = constant definitions (format, tone)

---

## 2. Three-Layer Configuration Model

Skill parameters use a three-layer structure:

```
┌─────────────────────────────────────────────────────────┐
│  L1 Interaction layer(asked at runtime)                                   │
│  → core parameters confirmed on every run                              │
│  → Storage location:{run_dir}/config.json                       │
├─────────────────────────────────────────────────────────┤
│  L2 config layer(global defaults)                                   │
│  → advanced parameters edited manually by the user                                  │
│  → Storage location:config/default.json                         │
├─────────────────────────────────────────────────────────┤
│  L3 Preset layer(static data)                                     │
│  → predefined option set, rarely changed                                │
│  → Storage location:reference/presets/                          │
└─────────────────────────────────────────────────────────┘
```

### 2.1 Responsibilities of Each Layer

| Layer | Storage location | Purpose | Change frequency |
|------|---------|------|---------|
| **L1 interaction layer** | `{run_dir}/config.json` | Main parameters confirmed at runtime | Every run |
| **L2 configuration layer** | `config/default.json` | Defaults for advanced parameters | As needed |
| **L3 preset layer** | `reference/presets/` | Predefined option sets | Rarely |

### 2.2 Parameter Priority

```
{run_dir}/config.json(L1 at run time)
    ↓ overrides
config/default.json(globaldefault L2)
    ↓ overrides
script built-in default value
```

---

## 3. L1 Interaction Layer

Main parameters collected once through **text questions** at runtime.

**Design principles**:

| Principle | Description |
|------|------|
| Ask fewer questions | Ask only for main parameters that cannot be inferred (L-required level) |
| **Fill in inferred values** | **The Agent decides inferable parameters without asking the user (L-inferred level)** |
| Preset choices | Provide choices from the L3 preset layer |
| Optional skip | Optional parameters may be skipped |
| ⚪ Recommended label | Add the `(Recommended)` suffix to the first option |

**Typical parameter types**:

| Type | Example | Question method |
|------|------|---------|
| Main input | keyword, topic, url | Text input |
| Preset choice | market, depth, mode | Single choice (from presets) |
| Optional detail | brand, target_user | May be skipped |

**Storage format** (`{run_dir}/config.json`):

```json
{
  "keyword": "react-hooks",
  "location": "United States",
  "language": "English",
  "limit": 50,
  "depth": 50,
  "brand": null,
  "run_dir": "{run_dir}",
  "created_at": "2026-01-13T14:30:00Z"
}
```

### 3.1 L1 Interaction Flow (Including Inferred Values)

```
1. Analyze user input and extract supplied parameters
2. Classify missing parameters(L-required / L-inferred / L-default)
3. L-required → collect with one conversational prompt(see skill-step-document-standard.md §9)
4. L-inferred → Agent infer and complete autonomously(see skill-step-document-standard.md §9.6)
5. L-default → read from config/default.json
6. Merge:userinput > inferredValue > defaultValue
7. write {run_dir}/config.json(containing a *_source annotation)
```

> **See skill-step-document-standard.md §9.6 for the three parameter classes**. Main idea: do not ask when a value can be inferred. Analyze the content independently on every run.

---

## 4. L2 Configuration Layer (default.json)

### 4.1 Design Principles

| Principle | Description |
|------|------|
| Grouped organization | Group by function module (api/scraper/report) |
| Sensible defaults | Runs without changes |
| Shallow structure | Allow only one level of nesting |

### 4.2 Structure Template

```json
{
  "{module1}": {
    "{param1}": {default_value},
    "{param2}": {default_value}
  },
  "{module2}": {
    "{param1}": {default_value}
  }
}
```

### 4.3 Full Example

```json
{
  "api": {
    "timeout": 30,
    "retry": 3,
    "interval": 2
  },
  "processing": {
    "batch_size": 30,
    "max_items": 100
  },
  "output": {
    "language": "en-US",
    "format": "markdown",
    "include_html": true
  }
}
```

### 4.4 How Scripts Read It

**Shell**:

```bash
CONFIG="$SKILL_DIR/config/default.json"
TIMEOUT=$(jq -r '.api.timeout // 30' "$CONFIG")
INTERVAL=$(jq -r '.api.interval // 2' "$CONFIG")
LANGUAGE=$(jq -r '.output.language // "en-US"' "$CONFIG")
```

**Python**:

```python
import json
from pathlib import Path

config_path = Path(__file__).parent.parent / "config/default.json"
config = json.loads(config_path.read_text())

timeout = config.get("api", {}).get("timeout", 30)
interval = config.get("api", {}).get("interval", 2)
```

---

## 5. Domain Configuration (Optional)

When a Skill needs business-logic configuration, it may add `config/domain.json`.

### 5.1 Difference From default.json

| Configuration type | File | Purpose | Example parameter |
|----------|------|------|----------|
| **Skill parameters** | `config/default.json` | Control Skill runtime behavior | hours, batch_size |
| **Domain configuration** | `config/domain.json` | Control business logic | target_total, ratio |

### 5.2 General Structure

```json
{
  "target_total": 100,
  "sources": {
    "{source_a}": {
      "ratio": 0.6,
      "priority_weights": {
        "{priority_1}": {"weight": 0.5, "limit": 10},
        "{priority_2}": {"weight": 0.5, "limit": 10}
      }
    },
    "{source_b}": {
      "ratio": 0.4,
      "priority_weights": {}
    }
  }
}
```

### 5.3 Field Descriptions

| Field | Description |
|------|------|
| `target_total` | Maximum total number of items to process |
| `sources` | Data-source configuration |
| `ratio` | This source's share of the total (the ratios for all sources must add up to 1) |
| `priority_weights` | Distribution of priority weights within the source |

### 5.4 Quota Calculation

Quota = `target_total × source.ratio`. The ratios for all sources add up to 1.

### 5.5 Domain-Specific Parameters

Group domain.json by business need: `collection` (collection policy), `analysis` (analysis parameters), `output` (output format), and `routing` (conditional routing). Define the structure as needed, but keep only one level of nesting.

---

## 7. SKILL.md Parameter Declaration

Add a "Parameters" section to SKILL.md:

```markdown
## parameters

### Interactive parameters(Asked on every run)

| parameters | type | description |
|------|------|------|
| `keyword` | string | Primary keyword |
| `market` | preset | Target market(US-en/JP-ja/...) |
| `depth` | preset | Search depth(quick/standard/deep) |

### Advanced parameters(config/default.json)

| Module | parameters | defaultValue | description |
|------|------|--------|------|
| api | `timeout` | 30 | Request timeout(seconds) |
| api | `interval` | 2 | Request interval(seconds) |
| output | `language` | en-US | output language |

> How to modify: edit `config/default.json`
```

---

## 8. Good Practices

| Practice | Description |
|------|------|
| **Ask fewer questions** | Ask only 3-4 main parameters in L1; put the rest in L2 |
| **Sensible defaults** | L2 parameters run without changes |
| **Grouped organization** | Group L2 by function module so values are easy to find |
| **Reuse presets** | L3 presets can be shared across several Skills |
| **Read with defaults** | Scripts always provide a default when reading a parameter |
| **Document parameters** | Explain configurable parameters in SKILL.md |

---

## 9. Prohibited Items

| Prohibited | Reason |
|------|------|
| ❌ Deep nesting (more than two levels) | Hard to read and maintain |
| ❌ Sensitive data | Put API keys in credentials/ |
| ❌ Runtime data | Put runtime data in runs/ |

---

## 10. Preset Data (reference/presets/)

**Location**: `reference/presets/`, which provides option data sources for parameter collection.

**Standard structure**:

```json
{
  "version": "1.0",
  "items": [
    {"id": "us-en", "name": "US English", "description": "US market", "default": true},
    {"id": "jp-ja", "name": "Japanese", "description": "Japanese market"}
  ]
}
```

**Writing principles**: ✅ unique ID, ✅ at least one `default: true` item, ⚪ preferably 3-6 items, and ✅ a description for every item.

---

## 11. Constant Definitions (reference/definitions/)

**Location**: `reference/definitions/`, which defines domain constants such as format, tone, and score.

**Standard structure**:

```json
{
  "type": "format",
  "items": [
    {"id": "json", "name": "JSON", "description": "structured format", "default": true},
    {"id": "markdown", "name": "Markdown", "description": "document format"}
  ]
}
```

**Difference from presets**: preset = an option data source chosen by the user; constant = an enum definition referenced by business logic. They have the same structure but different purposes.

---

## 12. Runtime Contract (config/runtime.json)

When a Skill needs a script environment, system binary, local model, or large provider cache, it must add `config/runtime.json`. This file stores only **declarations**. It does not store `.venv`, `node_modules`, model weights, or actual cache files.

### 12.1 Storage Boundaries

| Type | Storage location | Synced |
|------|----------|:---:|
| Runtime declaration | `config/runtime.json` | ✅ |
| Python/Node lock | `uv.lock` / `pnpm-lock.yaml` | ✅ |
| Python virtual environment | `~/.awp/runtime/envs/skills/{skill-name}/...` | ❌ |
| Installed Node dependencies | `~/.awp/runtime/envs/skills/{skill-name}/...` | ❌ |
| Model weights | `~/.awp/runtime/models/store/sha256/{sha256}/...` | ❌ |
| Skill model reference | `~/.awp/runtime/models/skills/{skill-name}/...` | ❌ |
| Large provider cache | `~/.awp/runtime/cache/providers/{provider}/...` | ❌ |

### 12.2 Standard Structure

```json
{
  "schema_version": "1.0",
  "skill": "awp-video-youtube-subtitling",
  "profiles": ["standard", "full"],
  "envs": [
    {
      "id": "python-main",
      "kind": "python",
      "version": "3.12",
      "manager": "uv",
      "lockfile": "scripts/python/uv.lock"
    }
  ],
  "binaries": [
    {
      "name": "ffmpeg",
      "required": true,
      "install_hint": "brew install ffmpeg / apt-get install ffmpeg"
    }
  ],
  "models": [
    {
      "id": "whisper-cpp/ggml-base",
      "role": "asr-default",
      "kind": "asr",
      "required": true,
      "size_mb": 142,
      "sha256": "Enter the real sha256",
      "license": "fill inmodel license",
      "source": {
        "type": "url",
        "url": "https://example.com/model.bin"
      }
    }
  ],
  "cache_policy": {
    "root": "runtime",
    "sync": false,
    "allow_provider_cache": true
  },
  "preflight": {
    "required": true,
    "mode": "auto",
    "cache_ttl_hours": 168,
    "skip_heavy_checks_if_cache_valid": true,
    "dependency_validation": {
      "required": true,
      "check_installed": true
    },
    "credential_validation": {
      "required": true,
      "mode": "live",
      "allow_paid_probe": false,
      "cache_ttl_hours": 24
    },
    "invalidate_on": [
      "SKILL.md",
      "workflow/",
      "scripts/",
      "config/",
      "credentials/",
      "reference/"
    ]
  }
}
```

### 12.3 Field Rules

| Field | Required | Description |
|------|:---:|------|
| `schema_version` | ✅ | Runtime contract version |
| `skill` | ✅ | Skill name; must match the directory/frontmatter |
| `profiles` | ⚪ | Supported machine profiles, such as full/standard/worker/minimal |
| `envs` | ⚪ | Runtime environments such as Python/Node/Deno/Bun |
| `binaries` | ⚪ | System binaries such as ffmpeg, tesseract, poppler, and imagemagick |
| `models` | ⚪ | Local model assets such as OCR/ASR/VAD/vision/embedding/rerank |
| `cache_policy` | ✅ | Cache root, whether to sync, and provider cache policy |
| `preflight` | ✅ | Step00 startup preflight policy |

The allowed values for `models[].kind` are **not maintained in this file**. Their single authoritative source is the `kind` definition in your tool manifest standard's Model (`models`) section. The Skill side and tool side share the same enum. Add a new type only once in the authoritative source; do not copy the list into this file.

### 12.4 preflight Field Rules

| Field | Required | Description |
|------|:---:|------|
| `required` | ✅ | Must be `true`; Step00 cannot be removed |
| `mode` | ✅ | `auto` / `strict` / `off`; default is `auto` |
| `cache_ttl_hours` | ✅ | Valid duration of the preflight cache; default is 168 hours |
| `skip_heavy_checks_if_cache_valid` | ✅ | Whether to skip heavy checks when a valid cache entry is found |
| `dependency_validation` | ✅ | Policy for checking whether dependencies are installed |
| `credential_validation` | ✅ | Policy for verifying that credentials actually work |
| `invalidate_on` | ✅ | List of paths included in fingerprint calculation |

`off` only means skipping heavy checks. It does not mean bypassing Step00. If no valid successful cache exists, it must fall back to `auto` automatically.

`credential_validation.mode=live` means Step00 must call a side-effect-free authentication probe for the service to confirm that the credentials work. The default is `allow_paid_probe=false`. Do not use a paid generation endpoint as a startup probe. If no side-effect-free probe exists, return `credential_live_unverified`, or let the user explicitly enter a paid smoke test.

> This section only declares the preflight fields in `config/runtime.json`. For the preflight cache location, field structure, and full invalidation rules (fingerprint / TTL / credential mtime), see `skill-runtime-data-standard.md` §7, the single authoritative source.

### 12.5 Reading and Execution

Skill scripts must not fix a Runtime directory directly in code. The main Agent or script must get paths through the Runtime resolver:

```bash
python {skill_dir}/scripts/doctor.py
python {skill_dir}/scripts/prepare.py --profile standard
python scripts/prepare.py resolve-model --role asr-default
```

`doctor` only returns a diagnosis. It does not create an environment or download models. `prepare` creates the environment, downloads models, checks the checksum, creates the logical Skill reference, and updates the inventory.

### 12.6 Prohibited Items

| Prohibited | Reason |
|------|------|
| Write a local absolute path in `config/runtime.json` | The path breaks after distribution |
| Put `.venv` or `node_modules` in the Skill directory | It does not work across platforms and pollutes the synced directory |
| Put model weights in `assets/` or `reference/` | Large files are synced repeatedly and license boundaries are unclear |
| Download silently when a model is missing | It cannot be controlled or audited; `prepare` must do it |
| Fall back to system Python when the environment is missing | Production runs cannot be reproduced |
| Delete or bypass `preflight.required` | Errors appear late in a business step |

### 12.7 Field Mapping to the Tool manifest runtime Object

A Skill's `config/runtime.json` and a tool's `manifest.runtime` object use **two locations with the same meaning**: the former is in the Skill directory, while the latter is inline in each tool's `manifest.json` inside the tool repository. Their field names evolved separately and do not match, **but neither side should change**. Renaming fields would break configuration files for active Skills and tools. Use this table when comparing the two sides:

| Meaning | Skill `config/runtime.json` | Tool `manifest.runtime` |
|------|------------------------------|--------------------------|
| Environment type | `envs[].kind` (`python` / `node`) | `envs[].type` (`python` / `node` / `system`) |
| Environment logical name | `envs[].id` | `envs[].name` |
| Dependency lockfile | `envs[].lockfile` | `envs[].lock` |
| Package manager | `envs[].manager` (such as `uv`) | Implied by the lockfile |
| Binary installation hint | `binaries[].install_hint` | `binaries[].mac` / `binaries[].linux` |
| Model type | `models[].kind` | `models[].kind` (same name; see your tool manifest standard for the authoritative enum) |
| Model size | `models[].size_mb` (number) | `models[].size` (string, such as `142MB`) |
| Model source | `models[].source` (object `{type, url}`) | `models[].source` (string: `runtime-registry` / URL / provider) |
| Cache policy | `cache_policy` (object `{root, sync, ...}`) | `cache_policy` (enum string: `runtime-cache` / `tool-cache` / `none`) |

> Meanings match one to one across both sides. Use this table for the mapping. For the full tool-side definition, see your tool manifest standard's `runtime` field.

---

## 13. Variable Placeholders

> This section is the single authoritative definition for all variable placeholders. Variable names, replacement rules, and usage constraints in workflow documents and Prompt templates follow this section. Other files reference it without repeating it.

### 13.1 Workflow Variables (AWP Standard)

| Variable | Description | Example |
|------|------|------|
| `{user_cwd}` | Working directory when the user triggers the Skill | `~/project/` |
| `{workspace}` | Root of the Skill workspace (same as `{skill_dir}`; historical alias) | `~/.claude/skills/awp-xxx/` |
| `{run_dir}` | Current run directory | `~/.awp/runtime/runs/skills/{skill-name}/keyword-20251228-160000/` |
| `{skill_dir}` | Skill installation directory (same as `{workspace}`) | `~/.claude/skills/awp-xxx/` |
| `{prompt_path}` | Path to the SubAgent Prompt file | `{skill_dir}/reference/prompts/prompt-research.md` |
| `{batch_id}` | Batch number (starts at 1) | `1`, `2`, `3` |
| `{batch_count}` | Total number of batches | `6` |
| `{count}` | Number of items in this batch | `30` |
| `{round_num}` | Round number | `1`, `2`, `3` |
| `{version}` | Version number | `1`, `2`, `3` |
| `{prev}` | Previous version number | `1` |
| `{dimension}` | Analysis dimension name | `thinking`, `writing rules` |
| `{token_limit}` | Current Token limit | `4000`, `5000` |
| `{base_limit}` | Baseline Token limit | `3000` |
| `{current_limit}` | Current Token limit (dynamic) | `3600` |
| `{growth_pct}` | Token growth percentage | `20` |
| `{timestamp}` | Timestamp | `2025-12-28T16:00:00Z` |
| `{input_path}` | Input file path for the current task | `{run_dir}/step01-collect/data.json` |
| `{progress_path}` | Progress file path | `{run_dir}/state/progress.json` |
| `{output_path}` | output file path for the current task | `{run_dir}/output/result.json` |
| `{mode}` | Mode name for a multi-mode Skill | `clone`, `timeline` |
| `{phase}` | Stage directory name | `collect`, `create` |
| `{brand}` | Current brand directory name | `AWP` (default value) |

### 13.2 Official Claude Code Variables

| Variable | Description | Example |
|------|------|------|
| `$ARGUMENTS` | Full parameter string Using when calling the Skill | `/skill-name arg1 arg2` → `arg1 arg2` |
| `$ARGUMENTS[N]` | Nth parameter (starts at 0) | `/skill-name foo bar` → `$ARGUMENTS[0]` = `foo` |
| `$N` | Short form of the Nth parameter | `$0` = first parameter |
| `${CLAUDE_SESSION_ID}` | Current session ID | `abc123-def456-...` |
| `${CLAUDE_SKILL_DIR}` | Absolute path to the Skill installation directory | `/Users/xx/.claude/skills/awp-xxx/` |

### 13.3 Documentation Example Placeholders (Not Runtime Variables)

The `{xxx}` placeholders used in documentation examples, such as `{action}`, `{NN}`, and `{script}`, only explain naming rules. The system does not add their values. Use meaningful names whose meanings can be inferred from context.

### 13.4 Variable Replacement Rules

| Rule | Description |
|------|------|
| Braced variable | `{variable}` is used in workflow documents and Prompt templates |
| Dollar-sign variable | `$ARGUMENTS` is used in SKILL.md frontmatter |
| Path variable | Always use an absolute path so `~/` does not remain unexpanded in a SubAgent |

### 13.5 keyword Generation Rules

`{keyword}` is the main variable used to name the run directory. It is extracted from user input on every run. **See `skill-runtime-data-standard.md` §3 for the full rules**.

| Stage | Description | Example |
|------|------|------|
| **Raw input** | Key parameter provided by the user | `React Hooks tutorial`, `user-123`, `https://example.com/topic` |
| **Normalization** | Convert to lowercase and remove special characters | `react-hooks`, `user-123`, `topic` |
| **Directory name** | `{keyword}-YYYYMMDD-HHMMSS` | `react-hooks-20260123-103000` |

Normalization rules: convert everything to lowercase; turn spaces/symbols into hyphens; keep only ASCII letters and numbers; extract the key part of a URL; limit length to 32 characters. See `skill-runtime-data-standard.md` §8.2 for the full `normalize_keyword()` implementation. Official frontmatter has no keyword source field. Runtime parameters or the configuration question step determine the keyword.

### 13.6 AWP knowledge base Variables


**`{brand}`**: Brand directory name. This workflow variable resolves at runtime and defaults to `AWP`. It routes to the matching brand identity/audience/visual assets/competitors. Resolution priority: `brand=xxx` explicitly Using in the Skill call > the `brand` field in `## Parameter section` of the source MD > default value `AWP`.


```json
{
  "context": {
  }
}
```

### 13.7 Prohibited Variable Uses

| Prohibited | Reason |
|------|------|
| ❌ Pass content in `{xxx_content}` form | Pass a path and let the SubAgent read it |
| ❌ Fix paths directly in code | Use variable placeholders |
| ❌ Include spaces in variables | May cause parsing errors |
| ❌ Variable arithmetic, such as `{version-1}` | Take variable names only from the §13.1 definition table; do not calculate in variable names |

---

## 14. output Templates

> This section governs **output template files** (HTML and CSS) under `reference/templates/`. It defines the visual structure and placeholder system for Skill output. Each template has its own file.

### 14.1 Directory Responsibilities and Structure

Location: `reference/templates/`. Typical content includes HTML templates, shared CSS, email templates, and report templates.

```
reference/templates/
├── report.html           # Report template
├── email.html            # Email template
├── card.html             # Card template
└── shared/               # Shared resources
    ├── base.css          # Base styles
    └── components.css    # Component styles
```

### 14.2 HTML Template Rules

Basic structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{title}}</title>
    <style>
        /* Inline styles or reference shared/*.css */
    </style>
</head>
<body>
    <div class="container">
        {{content}}
    </div>
</body>
</html>
```

Variable placeholders use double braces: `{{variable_name}}`.

| Variable format | Description | Example |
|----------|------|------|
| `{{name}}` | Simple variable replacement | `{{title}}` |
| `{{item.field}}` | Nested field | `{{user.name}}` |
| `{{#items}}...{{/items}}` | List loop | Iterate over an array and render each item |
| `{{#if condition}}...{{/if}}` | Conditional rendering | Render when the condition is met |

List-loop and conditional-rendering example:

```html
<ul>
{{#items}}
    <li><span class="title">{{title}}</span><span class="score">{{score}}</span></li>
{{/items}}
</ul>

{{#if is_premium}}<div class="premium-badge">Premium</div>{{/if}}
```

### 14.3 Template Types and Shared Styles

| Template | Purpose | Typical structure |
|------|------|---------|
| `report.html` | Analysis report | Title + summary + section details + data table |
| `card.html` | Card component | Title + score + description + labels |
| `email.html` | Email notification | Inline styles + simple layout |

`shared/` stores shared CSS: `base.css` (basic reset, container, and typography) and `components.css` (UI components such as cards, badges, tables, and labels).

### 14.4 Usage and Writing Principles

Scripts render templates in two ways: string replacement (simple templates without loops/conditions) or Jinja2 (templates that need loops/conditional rendering). Template path: `skill_dir / "reference" / "templates" / "{name}.html"`.

| Principle | Description |
|------|------|
| Prefer inline styles | Email templates must use inline styles for email-client compatibility |
| Responsive design | Support mobile display |
| Clear variable names | Use snake_case |
| Reuse shared styles | Put general styles in shared/ |

### 14.5 Prohibited Template Uses

| Prohibited | Reason |
|------|------|
| ❌ External CDN links | They may be unavailable offline |
| ❌ JavaScript logic | Templates only present content; logic belongs in scripts |
| ❌ Sensitive data fixed directly in code | Pass it through variables during rendering |
| ❌ Absolute-path references | Use relative paths or variables |

---

## Checklist

**config/default.json**:
- [ ] Configuration layers are correct (L1 interaction layer/L2 configuration layer/L3 preset layer)
- [ ] Only one level of nesting (grouped by function module)
- [ ] No sensitive data (API keys are in credentials/)
- [ ] No runtime data (runtime data is in runs/)
- [ ] Format is JSON

**config/runtime.json**:
- [ ] This file exists when an environment/binary/model is needed
- [ ] It stores declarations only, with no local absolute paths or installed asset files
- [ ] It includes `preflight.required=true`
- [ ] `preflight.mode` is `auto` / `strict` / `off`
- [ ] `preflight.dependency_validation.required=true`
- [ ] `preflight.credential_validation.mode=live`
- [ ] `preflight.credential_validation.allow_paid_probe=false`, unless the Skill clearly needs a paid smoke test
- [ ] `preflight.invalidate_on` covers SKILL.md, workflow, scripts, config, credentials, and reference
- [ ] Model declarations include kind, role, required, sha256, license, and source
- [ ] `cache_policy.sync` is `false`
- [ ] doctor / prepare commands have matching explanations in docs/setup.md

**reference/presets/**:
- [ ] Preset data is stored in the reference/presets/ directory
- [ ] Every preset has version and items fields
- [ ] At least one item is marked `default: true`

**reference/definitions/**:
- [ ] Constants have clear type and items fields
- [ ] Every item has id, name, and description

**SKILL.md parameter declaration**:
- [ ] Includes an interactive-parameter table (parameters asked on every run)
- [ ] Includes an advanced-parameter table (parameters in config/default.json)

**Variable placeholders (§13)**:
- [ ] Workflow variables use the `{var_name}` brace format, and names come from the §13.1 definition table (no invented variables)
- [ ] Path variables use absolute paths (so `~/` does not remain unexpanded in a SubAgent)
- [ ] No variable arithmetic, such as `{version-1}`
- [ ] Official CC variables use the correct names ($ARGUMENTS, ${CLAUDE_SKILL_DIR}, ${CLAUDE_SESSION_ID})

**output templates (§14, reference/templates/)**:
- [ ] Variable placeholders use the standard `{{variable}}` format
- [ ] Templates can be previewed on their own (without external resources)
- [ ] No external CDN links and no JavaScript logic
- [ ] Email templates use inline styles
- [ ] General styles are moved to `shared/` for reuse, and variable names use snake_case
- [ ] Pass `../../awp-meta-authoring-standard/std-style-language-contract.md`: three-layer language contract / remove jargon / formalize speech / fidelity, clarity, and grace
