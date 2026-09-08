---
document_id: awp-skill-development-standard/advanced/skill-runtime-data-standard
language: en
publication: public
source_revision: 2
title: "Runtime Data Standard"
purpose: Full standard for Runtime runs and the preflight cache
category: Standard
prerequisites:
  - ../skill-core-file-declaration.md
  - skill-step-document-standard.md
see_also:
  - skill-script-file-standard.md
  - skill-config-parameter-standard.md
  - skill-error-common-fixes.md
---

# Runtime Data Standard

> **output**: Runtime data under the Runtime runs directory (state/, output/, and step directories) and the preflight cache in Runtime inventory.
> **Responsibility**: Define the run-directory structure, startup preflight cache, progress-file format, and interruption-recovery process.

---

## 0. Design Philosophy

| Principle | Description |
|------|------|
| ✅ **Recoverable state** | Save each step result so a run can resume from a checkpoint after interruption without repeating completed work |
| ✅ **Separate data and logic** | Runtime runs stores data and state; workflow/ stores execution logic; do not mix them |
| ✅ **Unique keyword (run ID)** | A keyword (run ID) uniquely identifies each run, and every path is organized around it |
| ✅ **Cache startup preflight** | Run the light part of Step00 every time and reuse heavy-check results from the Runtime inventory cache |

---

## Organization

| # | Question | Section | Main content |
|---|------|------|---------|
| §0 | What principles govern runtime data? | Design Philosophy | Recoverable state, separation of data and logic, unique keyword |
| §1 | What belongs in Runtime runs? | Directory Responsibilities | Purpose and main structure of Runtime runs (state/output/dynamic) |
| §2 | How is Runtime runs organized internally? | runs/ Directory Structure | Directory naming, edge cases, and output-column rules |
| §3 | How is keyword defined? | keyword Rules | keyword source, normalization rules, and source declaration |
| §4 | What do directories for different Skills look like? | Directory Structure Examples for Different Skills | Directory examples for research/multi-mode/content-conversion Skills |
| §5 | How is the state/ directory designed? | state/ Directory Rules | progress.json field definitions and batch/item modes |
| §6 | What state fields exist? | State Field Rules | Four status values (pending/processing/completed/failed) |
| §7 | How is the Step00 cache stored? | preflight cache | Runtime inventory cache location, status, and invalidation rules |
| §8 | How does a run recover after interruption? | resume_hint | Field definitions and process for session-interruption recovery |
| §9 | How is the run directory initialized? | Run Directory Initialization | Initialization process and utility function signatures |
| §10 | How is multi-process mode organized? | Multi-Process Mode | Directory organization distinguished by mode prefixes |
| §11 | When is progress updated? | Progress Update Rules | Update timing and atomic write method |
| §12 | What are the use cases? | Use Cases | Recovery/resume/monitoring/cleanup cases |
| §13 | How are results reported after completion? | Completion Report | Reference formats for success/failure reports |
| §14 | What is prohibited? | Prohibited Items | Limits such as storing runtime data outside Runtime runs |
| §15 | How is old data cleaned up? | Data Cleanup Policy | Keep the latest five runs and use .keep to prevent cleanup |

---

## 1. Directory Responsibilities

**Role**: Store data and state for every run

**Storage location** (choose one; default is inside the Skill):
- **Inside the Skill directory** (default): `{skill-dir}/runs/{keyword}-YYYYMMDD-HHMMSS/` — self-contained and easy to run or distribute independently. `.gitignore` must exclude runtime data, and distribution packages must not include old runs.
- **Central Runtime management** (optional): `~/.awp/runtime/runs/skills/{skill-name}/` — move to this path after the Runtime CLI takes full control.

**Main structure (only two directories are fixed)**:
- `state/` — progress state; required for every Skill
- `output/` — final output; required for every Skill
- `{step-dirs}/` — dynamic directories defined by the workflow table in SKILL.md

---

## 2. Runtime runs Directory Structure

### 2.1 Structure Design Principles

**Fixed directories + dynamic directories**:

```
~/.awp/runtime/runs/skills/{skill-name}/{keyword}-YYYYMMDD-HHMMSS/
├── state/                    # [Fixed] Progress state, required for every Skill
│   └── progress.json
├── output/                   # [Fixed] Final output, required for every Skill
│   └── ...
├── step01-collect/           # [Dynamic] output of step01-collect.md
├── step02-filter/            # [Dynamic] output of step02-filter.md
└── step03-analyze/           # [Dynamic] output of step03-analyze.md
```

**Directory naming format**: `step{NN}-{action}/`

**Required consistency rule**: `{action}` in the directory name must match the workflow filename.

Example workflow table:

```markdown
| Step | Executor | Document | Input | output |
|------|--------|------|------|------|
| 01 | Script | step01-collect.md | URL | `step01-collect/` |
| 02 | SubAgent | step02-filter.md | step01-collect/ | `step02-filter/` |
| 03 | SubAgent | step03-analyze.md | step02-filter/ | `step03-analyze/` |
| 04 | Script | step04-generate.md | step03-analyze/ | `output/` |
```

**Rules**:
- A path wrapped in backticks, such as \`step01-collect/\`, is automatically recognized as an output directory.
- At the start of a run, parse the table and create output directories ([AWP Runtime Library feature]; scripts must create them explicitly when the library is unavailable).
- `state/` and `output/` are fixed directories. They may be declared explicitly in the table or omitted.

### 2.2 Edge Cases

#### 2.2.1 No Directory output

Put `-` in the output column and do not create a directory.

```markdown
| Step | Document | output |
|------|------|------|
| 01 | step01-resolve.md | `-` |
```

#### 2.2.2 Batch Processing

Keep `batches/` as a shared directory.

```markdown
| Step | Document | output |
|------|------|------|
| 03 | step03-split.md | `batches/` |
| 04 | step04-eval.md | `batches/batch_{N}/` |
| 05 | step05-merge.md | `step05-merge/` |
```

#### 2.2.3 Stage-Based Organization (>8 Steps)

Stage directories are allowed.

```markdown
| Step | Document | output |
|------|------|------|
| 01 | ... | `collect/` |
| 02 | ... | `collect/` |
| 03 | ... | `collect/` |
| 04 | ... | `create/` |
| 05 | ... | `create/` |
| 06 | ... | `create/` |
| 07 | ... | `create/` |
| 08 | ... | `create/` |
```

#### 2.2.4 Multiple Modes

Distinguish modes with a runs prefix. Each mode has its own internal structure.

#### 2.2.5 Latest-Pointer Files (⚪)

To avoid variable arithmetic, use "latest-pointer files" as standard references:

- `feedback_latest.md` → latest feedback
- `round_latest_questions.json` → latest questions and answers

They provide stable entry points when a Prompt reads the "previous version/latest."

### 2.3 output Column Rules

| output type | Format | Example |
|----------|------|------|
| Step directory | `step{NN}-{action}/` | `step02-collect/` |
| Final output | `output/` | `output/` |
| No output | `-` | `-` |
| Batch directory | `batches/` | `batches/` |
| Stage directory | `{phase}/` | `collect/` |

### 2.4 Directory Naming Rules (Required)

**Format**: `{keyword}-YYYYMMDD-HHMMSS` or `{mode}-{keyword}-YYYYMMDD-HHMMSS`

| Part | Description | Required |
|------|------|------|
| `{keyword}` | Main runtime parameter (after normalization) | **Required** |
| `{mode}` | Mode name for a multi-mode Skill | Optional |
| `YYYYMMDD` | Date | Required |
| `HHMMSS` | Time in **UTC, 24-hour format** | Required |

| Format | Description | Example |
|------|------|------|
| `{keyword}-YYYYMMDD-HHMMSS` | Single-process mode | `claude-code-20260115-103000` |
| `{mode}-{keyword}-YYYYMMDD-HHMMSS` | Multi-process mode | `analyze-user-a-20260115-103000` |

**Correct example**: `claude-code-20260123-103000`

**Incorrect example**: `20260123-103000` (keyword is missing)

**keyword** is a runtime parameter, such as a search term, username, or article title. It is not a fixed Skill value.

---

## 3. keyword Rules

### 3.1 keyword Source

Extract keyword from the **main parameter in user input for each run**:

| Skill type | keyword source | Example |
|-----------|-------------|------|
| Keyword research | Search keyword | `react-hooks`, `ai-agent`, `mcp-server` |
| Social-account analysis | Username | `username-a`, `username-b` |
| Article translation | Short form of the article title | `react-19`, `api-design-tips` |
| Presentation generation | Topic keyword | `ai-workflow`, `system-design` |
| Data collection | Target name | `product-a`, `competitor-b` |

### 3.2 keyword Normalization Rules (Configurable)

Normalize user input into a directory name:

| Rule | Description | Example |
|------|------|------|
| Words | Keep main words, convert to lowercase, and turn spaces/special characters into hyphens | "React Hooks Guide" → `react-hooks` |
| Non-ASCII input | Keep the ASCII part, or transliterate it | "AI workflow tutorial" → `ai-workflow` |
| URL | Extract the domain or a path keyword | `https://example.com/user123` → `user123` |
| Length | No more than 32 characters | |

> **Note**: These are default policies. They may be adjusted for the business case.

### 3.3 keyword Source Declaration

Official frontmatter does not provide a keyword source field. Mark it in the configuration question step:

```markdown
## Config prompts

| Parameter | description | keyword |
|------|------|---------|
| query | Search keyword | ✅ used for directory naming |
| depth | Research depth | |
```

### 3.4 Run Directory Examples

When the same Skill runs several times, each run has a different keyword:

```
~/.awp/runtime/runs/skills/{skill-name}/
├── react-hooks-20260123-103000/     # First run: research React Hooks
├── ai-agent-20260123-143000/        # Second run: research AI Agent
├── mcp-server-20260123-160000/      # Third run: research MCP Server
└── api-design-20260123-180000/      # Fourth run: research API Design
```

For a multi-mode Skill, prefix the keyword with the mode name:

```
~/.awp/runtime/runs/skills/{skill-name}/
├── analyze-user-a-20260123-103000/   # Analyze mode + username
├── analyze-user-b-20260123-143000/
├── compare-user-a-20260123-160000/   # Compare mode + username
└── compare-user-b-20260123-180000/
```

---

## 4. Directory Structure Examples for Different Skills

### 4.1 Data Research (Single Mode)

User input: "Research React Hooks for me"

```
~/.awp/runtime/runs/skills/{skill-name}/react-hooks-20260123-103000/
├── state/
│   └── progress.json  # keyword: "react-hooks"
├── step01-collect/    # Data collection (dynamic)
├── step02-analyze/    # Analysis results (dynamic)
└── output/            # Final output (fixed)
```

### 4.2 Multi-Mode Skill

User input: "Analyze user-a" (analyze mode)

```
~/.awp/runtime/runs/skills/{skill-name}/analyze-user-a-20260123-103000/
├── state/
│   └── progress.json  # keyword: "user-a", mode: "analyze"
├── step01-collect/    # Data collection (dynamic)
├── step02-filter/     # Data filtering (dynamic)
├── step03-analyze/    # Analysis results (dynamic)
└── output/            # Final output (fixed)
```

### 4.3 Content Conversion

User input: "Translate this article about new React 19 features"

```
~/.awp/runtime/runs/skills/{skill-name}/react-19-20260123-103000/
├── state/
│   └── progress.json  # keyword: "react-19"
├── step01-fetch/      # Source retrieval (dynamic)
├── step02-transform/  # Conversion results (dynamic)
└── output/            # Final output (fixed)
```

---

## 5. state/ Directory Rules

### 5.1 Standard Files

| File | Purpose | Required |
|------|------|------|
| `progress.json` | Batch progress | Required for batched tasks |
| `progress.json.health` | Health status | ⚪ |

### 5.2 Main progress.json Fields

**Main fields**: `keyword`, `keyword_raw`, `mode`, `directories`, and `preflight`

```json
{
  "keyword": "claude-code",
  "keyword_raw": "React Hooks tutorial",
  "mode": null,
  "created_at": "2026-01-23T10:30:00+08:00",
  "updated_at": "2026-01-23T11:45:00+08:00",
  "step": "step02-filter",
  "preflight": {
    "status": "passed",
    "mode": "auto",
    "cache_hit": true,
    "checked_at": "2026-01-23T10:29:00+08:00",
    "inventory": "~/.awp/runtime/inventory/skills/{skill-name}.preflight.json"
  },
  "directories": ["step01-collect", "step02-filter", "step03-analyze"],
  "step_status": {
    "step01-collect": "completed",
    "step02-filter": "in_progress",
    "step03-analyze": "pending",
    "step04-generate": "pending"
  },
  "resume_hint": {
    "executor": "task_agent",
    "workflow_doc": "workflow/step02-filter.md",
    "input_dir": "step01-collect/",
    "output_dir": "step02-filter/"
  }
}
```

**Field descriptions**:

| Field | Description |
|------|------|
| `keyword` | Normalized keyword used in the directory name |
| `keyword_raw` | Raw user input used for display and reports |
| `mode` | Current mode of a multi-mode Skill (may be null) |
| `preflight` | Reference to the Step00 preflight result and cache-hit status used for this run |
| `directories` | List of dynamic directories created for this run |
| `step_status` | Execution status of each step |

> A Skill may add custom fields, such as `batch_metadata` and `custom_state`, but it must not delete standard fields.

### 5.3 Two progress.json Modes

| Mode | Use case | Characteristics |
|------|----------|------|
| **Batch mode** | Process data in batches (evaluation/analysis) | Track by batch; simple and fast |
| **Item mode** | Process items one by one (publishing/uploading) | Track by item for exact control |

### 5.4 Batch Mode (⚪)

```json
{
  "keyword": "user-a",
  "keyword_raw": "user-a",
  "mode": "analyze",
  "step": "evaluate",
  "total_items": 50,
  "batch_size": 15,
  "batches": [
    {"start": 0, "end": 14},
    {"start": 15, "end": 29},
    {"start": 30, "end": 44},
    {"start": 45, "end": 49}
  ],
  "completed_batches": [0, 1],
  "current_batch": 2,
  "updated_at": "2026-01-05T00:30:00+08:00",
  "resume_hint": {
    "executor": "task_agent",
    "workflow_doc": "workflow/analyze/step03-evaluate.md",
    "warning": "Task() must be used to start the Agent"
  }
}
```

### 5.5 Item Mode

```json
{
  "step": "publish",
  "items": [
    {
      "index": 1,
      "status": "completed",
      "retries": 0,
      "completed_at": "2026-01-05T00:30:00+08:00"
    },
    {
      "index": 2,
      "status": "pending",
      "retries": 0,
      "completed_at": null
    }
  ],
  "resume_hint": {...}
}
```

### 5.6 Mode Selection Guide

| Use case | ⚪ Preferred mode | Reason |
|------|----------|------|
| Agent batch analysis | Batch mode | The whole batch succeeds/fails; no need to track every item |
| API publishing one item at a time | Item mode | One failed item does not affect others |
| Batch file processing | Batch mode | Batch-level tracking is enough |
| External service calls | Item mode | The network may be unstable; per-item records are safer |

---

## 6. State Field Rules

### 6.1 status Field

| status | Meaning | Rerun behavior |
|--------|------|---------|
| `pending` | Waiting to be processed | Process |
| `processing` | Processing | Retry (it may have been interrupted) |
| `completed` | Completed | Skip |
| `failed` | Failed | Skip (unless manually reset) |

> These four values are the main states. A Skill may add values such as `skipped` and `rolled_back`, but its rerun logic must remain compatible with the four main states.

### 6.2 Health Status Mapping

| Status | Description | Action |
|------|------|------|
| `ok` | Everything succeeded | Continue to the next step |
| `partial` | Some items failed | Retry failed items |
| `error` | Everything failed | Stop and report an error |

---

## 7. Preflight Cache

Write the full Step00 check result to Runtime inventory, not to the Skill directory or the business directory of one run. This section is the single authoritative source for preflight cache storage, structure, and invalidation rules. `preflight.cache_ttl_hours` in `config/runtime.json` defines the cache TTL (see `skill-config-parameter-standard.md` §12.4).

**Storage location**: `~/.awp/runtime/inventory/skills/{skill-name}.preflight.json`

**Standard structure**:

```json
{
  "schema_version": "1.0",
  "skill": "awp-video-example",
  "profile": "standard",
  "mode": "auto",
  "status": "passed",
  "checked_at": "2026-05-10T08:24:00-07:00",
  "expires_at": "2026-05-17T08:24:00-07:00",
  "fingerprint": {
    "skill": "sha256:...",
    "runtime": "sha256:...",
    "credentials_schema": "sha256:...",
    "credentials_secret": "local-only-sha256:..."
  },
  "checks": {
    "structure": "passed",
    "runtime": "passed",
    "dependencies": "passed",
    "credentials": "passed",
    "credential_live": "passed",
    "binaries": "passed",
    "runs_root": "passed",
    "residue": "passed"
  }
}
```

**Invalidation rules**:

| Condition | Action |
|------|------|
| `expires_at` has passed | Rerun the heavy Step00 checks |
| Fingerprint changes in `SKILL.md` / `workflow/` / `scripts/` / `config/` / `credentials/` / `reference/` | Rerun the heavy Step00 checks |
| Credential secret fingerprint, source level, or file mtime changes | Rerun credential live validation |
| profile or preflight policy changes in `config/runtime.json` | Rerun the heavy Step00 checks |
| Previous status is not `passed` | Do not skip; recheck immediately |
| User selects `strict` | Ignore the cache and run every check again |
| User selects `off` and the cache is valid | Run light checks only and skip heavy checks |

The current run's `progress.json` does not copy the full inventory. It stores only a reference:

```json
{
  "preflight": {
    "status": "passed",
    "mode": "auto",
    "cache_hit": true,
    "inventory": "~/.awp/runtime/inventory/skills/{skill-name}.preflight.json"
  }
}
```

## 8. Resume Hint (resume_hint)

### 8.1 Purpose (Optional)

Used to restore execution state after a session interruption. **Fill it in only when the execution method is not the default path or strict constraints apply**.

### 8.2 Field Definitions (Minimal)

| Field | Type | Description |
|------|------|------|
| `executor` | string | Executor type (fill in only for a non-default executor) |
| `warning` | string | Key constraint (for example, Task must be used) |
| `resume` | string | One-line resume hint (replaces a detailed action list) |

### 8.3 Short Example

```json
{
  "step": "evaluate",
  "current_batch": 3,
  "completed_batches": [0, 1, 2],
  "total_batches": 6,
  "resume_hint": {
    "executor": "task_agent",
    "warning": "Task() must be used to start the Agent,the main Agent must not execute directly",
    "resume": "continue evaluating batches 3–5"
  }
}
```

### 8.4 Recovery Process (Simplified)

```
When resuming a session
    │
    ├─ 1. read progress.json
    │
    ├─ 2. if resume_hint exists, choose the execution method as instructed and follow its constraints
    │
    ├─ 3. check completed_batches
    │
    └─ 4. continue from current_batch
```

### 8.5 Recovery Checklist

| Order | Check | Action |
|------|--------|------|
| 1 | Read progress | `Read state/progress.json` |
| 2 | Check resume_hint | Get the executor and warning |
| 3 | Read the step document | `Read {workflow_doc}` |
| 4 | Confirm execution method | Continue based on executor type |

---

## 9. Run Directory Initialization

### 9.1 Initialization Process

```
Run start
    │
    ├─ 1. determine keyword
    │      └─ extract from user input or frontmatter
    │
    ├─ 2. create the Runtime run directory {run_dir}/
    │
    ├─ 3. create fixed directories
    │      ├─ state/
    │      └─ output/
    │
    ├─ 4. parse the SKILL.md workflow table
    │      └─ extract directory names from every "output" column
    │
    ├─ 5. create the step directory (format: step{NN}-{action}/)
    │      ├─ step01-collect/
    │      ├─ step02-filter/
    │      └─ step03-analyze/
    │
    └─ 6. initialize state/progress.json (including keyword)
```

### 9.2 Utility Functions

> **Note**: The following are example signatures from the [AWP Runtime Library], not official built-in Claude Code features. When the runtime library is not connected, scripts must create directories themselves.

| Function | Signature | Description |
|------|------|------|
| `normalize_keyword` | `(raw: str, max_len=32) → str` | Normalize user input into a directory name (lowercase, drop non-ASCII, add hyphens, truncate) |
| `parse_workflow_dirs` | `(skill_md: Path) → list[str]` | Parse the list of output directories from the workflow table in SKILL.md |
| `init_run_dir` | `(skill_name, keyword, mode=None) → Path` | Create the run directory + state/ + output/ + step directories + progress.json under `~/.awp/runtime/runs/skills/{skill-name}/` |

### 9.3 Usage Examples

```python
# Single-mode Skill
init_run_dir(skill_name, keyword="React Hooks")
#   → ~/.awp/runtime/runs/skills/{skill-name}/react-hooks-20260123-103000/

# Multi-mode Skill
init_run_dir(skill_name, keyword="user-a", mode="analyze")
#   → ~/.awp/runtime/runs/skills/{skill-name}/analyze-user-a-20260123-103000/

# Input containing digits and symbols
init_run_dir(skill_name, keyword="React 19 new-features")
#   → ~/.awp/runtime/runs/skills/{skill-name}/react-19-20260123-103000/
```

### 9.4 Get the Latest Run Directory

| Function | Signature | Description |
|------|------|------|
| `get_latest_run` | `(skill_name: str) → Path` | Sort by directory name under the Runtime runs root and return the latest run directory |
| `get_latest_output` | `(skill_name: str) → Path` | Return the output/ path of the latest run |

---

## 10. Multi-Process Mode

### 10.1 Directory Structure

```
~/.awp/runtime/runs/skills/{skill-name}/
├── analyze-user-a-20260115-103000/    # Analyze mode + keyword
│   ├── state/
│   ├── step01-collect/                # Data collection (dynamic)
│   ├── step02-filter/                 # Data filtering (dynamic)
│   ├── step03-analyze/                # Analysis results (dynamic)
│   └── output/
└── compare-user-a-20260115-143000/    # Compare mode + keyword
    ├── state/
    ├── step02-fetch/                  # Data retrieval (dynamic)
    ├── step03-analyze/                # Analysis results (dynamic)
    └── output/
```

### 10.2 Naming Format

`{mode}-{keyword}-{YYYYMMDD-HHMMSS}`

Examples:
- `analyze-user-a-20260115-103000`
- `compare-user-a-20260115-143000`
- `export-dataset-b-20260116-091500`

---

## 11. Progress Update Rules

### 11.1 Update Timing

| Timing | Update |
|------|---------|
| Batch starts | Set `current_batch` to the current batch |
| Batch completes | Add the batch ID to `completed_batches` |
| Step changes | Update the `step` field |
| Error occurs | Update status to `failed` |

### 11.2 Update Method

Read → merge fields → update `updated_at` → write atomically (write a temporary file first, then rename it).

```python
update_progress(run_dir, current_batch=3, completed_batches=[0, 1, 2])
```

---

## 12. Use Cases

| Use case | Action |
|------|------|
| Restore progress after a session interruption | Read progress.json + resume_hint |
| Resume from a checkpoint after failure | Continue from current_batch |
| Monitor execution status | Check the completed_batches ratio |
| Clean up expired data | Delete old directories by timestamp |

---

## 13. Completion Report (Reference Format)

> The Agent will naturally summarize results after completing the Skill. The following format is a reference, not a requirement.

**Success**: Report statistics + output path + next action.
**Failure**: Report failed stage + error information + recovery method.

---

## 14. Prohibited Items

| Prohibited | Reason |
|------|------|
| ❌ Store runtime data outside Runtime runs | Breaks the data-separation principle |
| ❌ Manually change a run-directory name (keyword-timestamp) | Breaks sorting and recovery logic |
| ❌ Omit state/progress.json | Checkpoint resume becomes impossible |
| ❌ Omit resume_hint for a batch task | A batch task cannot recover after session interruption (a simple linear Skill may omit it) |

---

## 15. Data Cleanup Policy

Clean up old run directories regularly. ⚪ Keep the latest five successful runs and retain failed runs longer. Create a `.keep` file to prevent automatic cleanup.

---

## Checklist

**Directory structure**:
- [ ] Runtime runs includes the fixed state/ and `output/ directories`
- [ ] Dynamic directory names match workflow filenames
- [ ] Directory naming format is `{keyword}-YYYYMMDD-HHMMSS`

**Progress and state**:
- [ ] progress.json records the status of every step
- [ ] progress.json includes a `preflight` reference
- [ ] keyword uniquely identifies this run
- [ ] status uses four values (pending/processing/completed/failed)

**Recovery process**:
- [ ] resume_hint supports checkpoint recovery
- [ ] Progress updates use atomic writes (temporary file first, then rename)

**Step00 cache**:
- [ ] The preflight cache is written to Runtime inventory
- [ ] The cache includes fingerprint, expires_at, and checks
- [ ] checks includes dependencies and credential_live
- [ ] Heavy Step00 checks rerun automatically when the cache becomes invalid
- [ ] Pass `../../awp-meta-authoring-standard/std-style-language-contract.md`: three-layer language contract / remove jargon / formalize speech / fidelity, clarity, and grace
