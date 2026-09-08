---
document_id: awp-skill-development-standard/advanced/skill-step-document-standard
language: en
publication: public
source_revision: 2
title: "Step Document Standard"
purpose: Full standard for `workflow/step`*.md files: structure, executors, SubAgent calls, and parameter collection through text Q&A
category: Standard
prerequisites:
  - ../skill-core-file-declaration.md
see_also:
  - ../../awp-prompt-writing-standard/prompt-format-eight-part.md
  - skill-runtime-data-standard.md
---

# Step Document Standard

> This document governs the creation and maintenance of `workflow/step*.md` step files.
> A step file defines the executor, input and output, validation checks, and scheduling rules for one step in a Skill workflow.
> Each step has its own file named `stepNN-{action}.md`. A multi-step Skill must use `step00-preflight.md` as its startup gate.

---

## Design Principles

| Principle | Description |
|------|------|
| **A step is a contract** | Each step file is an interface contract between the main Agent and the executor. Input, output, and validation are all required |
| **Verify before startup** | Step00 confirms that structure, Runtime, credentials, runtime folders, and recovery state work before business steps begin |
| **Executors are orthogonal** | Script/MCP/SubAgent/main Agent are decoupled. Changing an executor in one step changes configuration, not the flow |
| **Cognitive isolation** | Give data processing to a SubAgent. The main Agent only coordinates and keeps its context clean. Use "cognitive isolation" consistently across the standards |
| **Infer instead of asking** | Parameter collection relies on independent inference. Text Q&A is the fallback. This reduces interaction and preserves an automated experience |

---

## 0. Terms

> See `../skill-core-development-standard.md` § Shared Glossary for cross-standard terms. This section adds terms specific to step documents.

| Term | Definition | Use |
|------|------|---------|
| **SubAgent** | An independent Agent instance started through the Agent tool | Shared term in documents and standards |
| **Agent tool (Task)** | Official CC tool that starts a SubAgent | Implementation method; pseudocode uses `Task()` |
| **Agent Teams** | Experimental multi-Agent collaboration framework | Very large workflows with 6+ coordinated parallel workers; see `skill-platform-constraint-limits.md` §9 |
| **Main Agent** | Agent in the current conversation that coordinates steps | Reads configuration, coordinates SubAgents, and merges results |

> `Task()` in this document is pseudocode. Actual CC uses the Agent tool. Do not copy it directly as Python code.

---

## Structure

| # | Question | Section | Main Content |
|---|------|------|---------|
| §0 | How are terms kept consistent? | Terms | Definitions of SubAgent, Agent tool, Agent Teams, and main Agent |
| §1 | Where do step files go? | Folder Responsibility | Role of workflow/ and content classes |
| §2 | How are step files named? | File Naming Standard | stepNN-{action}.md format, Step00 preflight, numbering |
| §3 | What does a step document look like? | Step Document Structure | Standard skeleton: execution → validation → next step |
| §4 | How is a SubAgent step different? | Agent Step Document | Dedicated SubAgent template with Prompt section |
| §5 | How is a SubAgent called? | SubAgent Call Standard | Task() parameters, parallel control, return handling |
| §6 | How are several batches scheduled? | Round Scheduling Rules | Manual coordination of several batch rounds |
| §7 | How is a large dataset prepared? | Preparation SubAgent Pattern | Main Agent preprocesses and hands work to SubAgent |
| §8 | What does a SubAgent return? | Minimal Return Format | Field standard for SubAgent return JSON |
| §9 | How are user parameters collected? | Parameter Collection Standard | Text Q&A + independent completion + three classes |
| §10 | How is step execution validated? | Pre-Checks and Post-Validation | Validation before and after execution |
| §11 | How are timeouts and errors handled? | Timeout and Error Handling | Timeout settings and retry policy |
| §12 | How are variables referenced? | Variable Placeholders | Variable-reference rules in step documents |
| §13 | How are steps organized for several flows? | Multi-Flow Pattern | Step organization for multi-mode Skills |
| §14 | How do parallel steps synchronize? | Synchronization Points | Merge and synchronization of parallel steps |
| §15 | How are different types routed? | Type-Based Routing | Route content types to different processing flows |
| §16 | How does a step load knowledge? | Prerequisite Knowledge Loading | Obtain external knowledge needed by a step |
| §17 | How is brand experience protected? | Brand Experience Standard | Brand consistency in output |

---

## 1. Folder Responsibility

**Core role**: Store step-execution documents: process only.

**Content type**:

- `stepNN-{action}.md` - step execution instructions

**Do not put here**:

- Prompt templates → put them in `reference/prompts/`
- Analysis frameworks → put them in `reference/`
- Script code → put it in `scripts/python/`

---

## 2. File Naming Standard

### 2.1 Naming Format

```
stepNN-{action}.md
```

| Part | Standard | Example |
|------|------|------|
| `step` | Fixed prefix | step |
| `NN` | **Two digits**; multi-step Skills start at 00, business steps start at 01 | 00, 01, 02, 10 |
| `{action}` | Action description separated by hyphens; Step00 always uses `preflight` | preflight, resolve, collect, batch-split |

### 2.2 Numbering Standard

| Rule | Description |
|------|------|
| Starting number | ✅ A multi-step Skill must start with `step00-preflight.md`; business steps start with `step01-*.md` |
| Digit count | Two digits for sorting |
| Serial order | 00→01→02→...→N |
| No substeps | Do not use step02a or step02-1 |

### 2.3 Correct vs Wrong Examples

**Correct**:

```
workflow/
├── step00-preflight.md
├── step01-locate-run-dir.md
├── step02-collect.md
├── step03-split-batches.md
├── step04-eval-batches.md
└── step10-final-merge.md
```

**Wrong**:

```
workflow/
├── step00-init.md       # ❌ Step00 handles preflight only, not parameter initialization or business preparation
├── step1-collect.md     # ❌ One digit
├── step02a-prepare.md   # ❌ Substep suffix
└── step02-1-verify.md   # ❌ Substep number
```

### 2.4 Step00 Preflight Standard

A Skill with scripts, credentials, Runtime assets, or a multi-step workflow needs a startup preflight. A multi-step Skill uses `workflow/step00-preflight.md`. A single-step Skill with external dependencies keeps a `## Step 00: Preflight` section inside SKILL.md. A pure knowledge-only single-step Skill with no external dependency is exempt.

Step00 is a startup gate, not a business step.

| Check Layer | Required Checks | Default Behavior |
|--------|----------|----------|
| Structure | SKILL.md, workflow table, step files, old-path residue | Stop on failure |
| Runtime | `config/runtime.json`, env, binary, model, cache, installed dependency state | Call doctor; confirm installed; create no physical resource |
| Credentials | Existence, field format, and real usability of `tools/credentials/*.md` | Live validation is required; do not print real values; do not call a paid generation API by default |
| Runtime folder | Runtime runs root is writable and resume state is readable | Stop on failure |
| Scripts | Dependency declarations, syntax entry points, no `.venv` / `__pycache__` / `*.pyc` | Stop on failure |
| Cache | Whether preflight cache is valid | Skip heavy checks on a hit |

Step00 supports three modes:

| Mode | Use | Rule |
|------|------|------|
| `auto` | Default startup mode | Run light checks every time; valid cache may skip dependency-installation and credential live-validation checks |
| `strict` | New machine, pre-release, troubleshooting | Run full checks every time and do not trust cache; verify installed dependencies and real credential validity |
| `off` | Fast startup on a verified machine | Skip heavy checks only when valid successful cache exists; without cache, fall back to `auto` |

Step00 forbidden actions:

- ❌ Do not call a paid API or write business output by default
- ❌ Do not silently download models or create long-lived environments
- ❌ Do not skip all checks without successful cache
- ❌ Do not write preflight cache into the Skill folder
- ❌ Do not check only that a credential file exists without checking real usability

Use `-` in the Step00 output column. Write check results into Runtime inventory. The current run's `progress.json` records only `preflight.status`, `preflight.cache_hit`, and `preflight.inventory`.

**Step00 document skeleton**:

```markdown
# Step 00: Preflight

> **Executor**: Main Agent + script
> **Input**: Skill folder
> **Output**: `-`

## Execution Instructions

1. Read the `preflight` policy in `config/runtime.json`; default `mode=auto`.
2. Read preflight cache from Runtime inventory.
3. Run light checks: structure, credential templates, runtime-folder permissions, and residue.
4. When cache is absent, expired, fingerprint-changed, not passed, or mode=strict, run heavy checks.
5. Heavy checks must confirm installed dependencies, executable required binaries, and live validation of required credentials.
6. Refresh inventory after success. Stop on failure and do not enter Step01.

## Validation Checkpoints

| ID | Check | Pass Standard |
|------|--------|----------|
| 00-a | Complete structure | SKILL.md, workflow table, and step files agree |
| 00-b | Runtime can be diagnosed | doctor returns passed or explicit missing items |
| 00-c | Dependencies installed | env, package, binary, and model references work |
| 00-d | Credentials really work | Required credentials pass a no-side-effect authentication probe |
| 00-e | Runtime folder writable | Runtime runs root can create a folder |
| 00-f | Correct cache policy | off falls back to auto without valid cache |

## Next Step

→ `Step 01: Init`
```

---

## 3. Step Document Structure

### 3.1 Standard Template

```markdown
# Step NN: {action description}

> **Executor**: Script / SubAgent / Main Agent
> **Input**: `step{N-1}-{prev-action}/` or `user input`
> **Output**: `step{NN}-{action}/` or `output/` or `-`

---

## Execution Instructions

{specific execution steps}

## Input Files

| File | Source | Description |
|------|------|------|
| `step01-collect/data.json` | Step 01 output | {use} |

## Output Files

| File | Format | Description |
|------|------|------|
| `step02-filter/filtered.json` | JSON | {content description} |

## Script Execution (If Applicable)

python scripts/python/{script}.py --run-dir {run_dir}

## Agent Prompt (If Applicable)

→ See `reference/prompts/prompt-{function}.md`

**Scheduling principles**:

- The main Agent **does not read** the Prompt body. It passes only `prompt_path` and variables
- The SubAgent **Reads** the Prompt file and runs it

**Folder-reading rules**:

- When input contains a folder path, **do not Read the folder directly**
- First Glob the file list, then Read each file

**Variable rules**:

- **No variable arithmetic**, such as `{version-1}` or `{round_num-1}`
- For "previous version" or "latest," use a stable pointer file such as `feedback_latest.md` or `round_latest_questions.json`

## Validation Checkpoints

| ID | Check | Pass Standard |
|------|--------|----------|
| NN-a | {check} | {standard} |
| NN-b | {check} | {standard} |

---

## Next Step

→ `Step N+1: {next step}`
```

**Output-path standard**:

| Output Type | Format | Example |
|----------|------|------|
| Step folder | `step{NN}-{action}/` | `step02-filter/` |
| Final output | `output/` | `output/` |
| No output | `-` | `-` |
| Batch folder | `batches/` | `batches/` |

### 3.2 Executor Types

> **Five executors**, consistent with `../skill-core-file-declaration.md` §5.2

| Executor | Description | Best Fit | Context Effect |
|--------|------|----------|-----------|
| **Script** | Python/Shell script | Deterministic operations such as batching, merging, and uploading | None |
| **MCP** | Tools such as Brave/exa | Web retrieval with free selection | Medium |
| **SubAgent** | Independent Agent started by the Task tool | LLM judgment such as evaluation, analysis, and generation | Injected on return when the platform supports TaskOutput |
| **Main Agent** | Runs directly in the main conversation | Simple operations such as reading config and coordinating the flow | Accumulates |
| **Preparation SubAgent** | Data-preparation SubAgent | Large-scale data preprocessing | Released after execution |

**Executor choice**: ✅ Use a script or MCP for deterministic work. Use a SubAgent for LLM judgment.

> **See §7 for Preparation SubAgent**

### 3.3 Validation Checkpoint IDs

Checkpoint ID format: `{step number}{letter}`

```
Step 04 checkpoints:
4a. [ ] step04-eval/batch_{N}.json exists
4b. [ ] JSON format is valid
4c. [ ] Every item contains score and reason
```

---

## 4. Agent Step Document

When a SubAgent executes a step, the step document focuses on **execution-layer** information:

### 4.1 Document Structure

```markdown
# Step NN: {action description}

> **Executor**: SubAgent
> **Input**: {prior-step output path}
> **Output**: {current-step output path}

---

## Execution Conditions

{prerequisites that trigger execution}

## Input Files

| File | Path | Description |
|------|------|------|
| ... | ... | ... |

## Output Files

| File | Path | Description |
|------|------|------|
| ... | ... | ... |

## Execution Parameters

| Parameter | Value | Description |
|------|-----|------|
| subagent_type | "general-purpose" | Fixed value |
| model | "claude-sonnet-4-6" | Use the matching haiku model for a light task |
| run_in_background | true/false | Whether to run in the background |

## Prompt Template

→ `reference/prompts/prompt-{function}.md`

## Validation Checkpoints

| ID | Check | Pass Standard |
|------|--------|----------|
| ... | ... | ... |

## Next Step

→ `Step N+1: {next step}`
```

### 4.2 Responsibility Boundaries

| Concern | skill-step-document-standard.md (This Document) | skill-prompt-template-standard.md |
|--------|------------------|-----------|
| **When to run** | ✅ Execution conditions and trigger time | - |
| **Input and output** | ✅ File paths and data formats | - |
| **Execution parameters** | ✅ Agent tool parameter configuration | - |
| **Validation checks** | ✅ Checkpoint definitions | - |
| **Prompt writing** | → Reference | ✅ Full standard |
| **Forbidden clauses** | → Reference | ✅ Definitions |
| **Return format** | → Reference | ✅ Definitions |

> **See `skill-prompt-template-standard.md` for the Prompt writing standard**

---

## 5. SubAgent Call Standard

### 5.1 Agent Tool Parameters

> See `skill-platform-constraint-limits.md` §7.1.1 for the full parameter matrix, including skills preloading and team_name.
> This section lists only the **parameters most often used in step documents**.

| Parameter | Type | Required | Description |
|------|------|------|------|
| `subagent_type` | string | ✅ | `"general-purpose"` or a custom Agent name; see §5.5 |
| `prompt` | string | ✅ | Full task instructions |
| `model` | string | ❌ | ⚪ Default `"sonnet"`; see the selection guide below |
| `run_in_background` | bool | ❌ | `True`=run in background, `False`=block in foreground |
| `description` | string | ❌ | Short 3-5 word description |
| `isolation` | string | ❌ | `"worktree"` = run in an independent git worktree |

**Model selection guide (checked 2026-06-12)**:

| Task Type | Alias | Reason |
|---------|---------|------|
| Very hard reasoning / complex architecture | `fable` | Mythos level; strongest current reasoning |
| Complex analysis / architecture design | `opus` | Strong reasoning with 1M context |
| Mixed Plan + Execute | `opusplan` | Opus for Plan, Sonnet for Execute |
| Standard batch work / content generation | `sonnet` | Balanced default with 1M context |
| Simple classification / format conversion | `haiku` | Fast and inexpensive with 200K context |

> See `skill-platform-constraint-limits.md` §5.1 for the full model matrix, including Effort levels.

### 5.1.1 Good vs Bad

| Good | Bad | Reason |
|----|-----|------|
| Put full file paths in prompt | Write `"read prior-step output"` | An exact path needs no guess by the SubAgent |
| `run_in_background=True` + `TaskOutput(block=True)` | Wait serially for several foreground Agents | The first form runs in parallel and saves time |
| Return `{"ok": true, "batch": 1}` | Return full processed text | A minimal return prevents context growth |

### 5.2 Start One Agent (Foreground Blocking)

```python
Task(
    subagent_type="general-purpose",
    model="claude-sonnet-4-6",
    prompt="""
You are a data-evaluation specialist. Complete evaluation for batch 1/3.

[Input File]
{run_dir}/batches/batch_1_input.json

[Reference Documents] (read as needed)
- Evaluation criteria: {run_dir}/output/criteria.md
- Scoring standard: {skill_dir}/reference/scoring-rubric.md

[Your Task]
1. Read the evaluation criteria and understand the viewpoints
2. Read the scoring standard and understand the score rules
3. Read the batch data and evaluate every item
4. Write results to {run_dir}/step04-eval/batch_1.json

[Return Format - Minimal]
Success: {"ok": true, "batch": 1, "count": 30}
Failure: {"ok": false, "batch": 1, "err": "short description"}
"""
)
```

### 5.3 Start Several Agents in Parallel (Background)

```python
# Note: Call several Agent tools in the same message

# Agent 1: Batch 1
Task(
    subagent_type="general-purpose",
    model="claude-sonnet-4-6",
    run_in_background=True,
    description="Evaluate batch 1",
    prompt="...full prompt for batch 1..."
)

# Agent 2: Batch 2, in the same message
Task(
    subagent_type="general-purpose",
    model="claude-sonnet-4-6",
    run_in_background=True,
    description="Evaluate batch 2",
    prompt="...full prompt for batch 2..."
)
```

### 5.4 Wait for Background Agents

```python
TaskOutput(task_id="agent_1_id", block=True)
TaskOutput(task_id="agent_2_id", block=True)
```

### 5.5 Custom Agent Types

Besides built-in types such as `general-purpose`, `Explore`, and `Plan`, `subagent_type` may reference a custom Agent under `.claude/agents/`:

```python
# Reference a custom Agent with the security-review Skill preloaded
Task(
    subagent_type="security-reviewer", # → .claude/agents/security-reviewer.md
    prompt="Review code under {run_dir}/step02-code/ for security"
)
```

**Benefits of a custom Agent**:

- Preload domain knowledge through the `skills` field instead of repeating reference paths in prompt
- Control available tools exactly through `tools` / `disallowedTools`
- Prevent endless loops through `maxTurns`
- Run in isolation through `isolation: worktree`

> See `skill-platform-constraint-limits.md` §7.1.2 for detailed Agent frontmatter fields.

### 5.6 Worktree Isolation

When several Agents modify files at the same time, use `isolation: "worktree"` to prevent conflicts:

```python
Task(
    subagent_type="general-purpose",
    isolation="worktree",
    run_in_background=True,
    prompt="Implement feature A on an independent branch"
)
```

After Worktree completion: changes exist → return the branch name; no change → clean up automatically.

---

## 6. Round Scheduling Rules (Manual Coordination)

> Claude Code's built-in `/batch` can coordinate parallel tasks automatically. The manual round scheduling below is for cases that need exact control. For simple parallel work, ✅ use `/batch` directly.

### 6.1 Serial vs Round Selection

| Case | Method | Reason |
|------|----------|------|
| Batch count ≤4 | **All parallel** | Platform parallel limit, no risk |
| Batch count 5-10 | **All parallel**, after checking tokens per batch | Safe below 10K per batch; use serial above 50K per batch |
| Batch count >10 | Rounds, each with ≤4 | Ordered management and no large accumulation |
| Tokens per batch >50k | **Serial** | Control accumulation |

> **1M-era thresholds (2026-03)**: The old standard assumed 200K and was conservative. With 1M, ≤10 × 10K = 100K is only 10%, so full parallel work is easy.

### 6.2 Core Constraints

```
┌─────────────────────────────────────────────────────────────┐
│  Platform hard limit: start ≤4 SubAgents at once             │
│  Batches ≤4 → all parallel                                   │
│  Batches 5-10 → all parallel after checking tokens per batch │
│  Batches >10 → rounds, each ≤4, to preserve order            │
│  TaskOutput injects returns, but accumulation fits within 1M  │
└─────────────────────────────────────────────────────────────┘
```

### 6.3 Scheduling Flow

```
Main Agent:
├─ 1. Read the index and get batch count N
├─ 2. Calculate rounds: round_count = ceil(N / 4)
├─ 3. for round in 1..round_count:
│     │
│     ├─ Start Agents for the current round, at most 4, run_in_background=true
│     │
│     ├─ **Wait for the current round** with TaskOutput × 4, block=true
│     │
│     ├─ **Validate batch files** and retry failures at once
│     │
│     ├─ Update state/progress.json
│     │
│     └─ Continue to the next round
│
└─ 4. All rounds complete → next step
```

### 6.4 Configuration Parameters

| Parameter | Value | Description |
|------|---|------|
| BATCH_SIZE | 30 | Items processed per batch |
| AGENTS_PER_ROUND | 4 | Maximum parallel Agents per round; platform hard limit |
| MAX_RETRIES | 2 | Maximum retries per batch |

---

## 7. Preparation SubAgent Pattern

> Use a script or MCP for deterministic operations such as collection, batching, merging, and uploading.

⚪ For large-volume data processing, use a "Preparation SubAgent" to isolate preparation. This is an optional improvement with a 1M context window:

### 7.1 Three-Phase Execution Pattern

```
┌─────────────────────────────────────────────────────────────┐
│ Phase 0: Preparation SubAgent                                │
├─────────────────────────────────────────────────────────────┤
│ 1. Read data and count items                                 │
│ 2. Calculate the batch index                                 │
│ 3. Write state/progress.json                                 │
│ 4. Return: {"ok": true, "total_batches": N}                  │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│ Phase 1-N: Execution SubAgent, serial or in rounds           │
├─────────────────────────────────────────────────────────────┤
│ Each SubAgent: │
│ 1. Read state/progress.json for the current batch            │
│ 2. Read the matching data slice                              │
│ 3. Read references itself, without the main Agent            │
│ 4. Run the task and write the result file                    │
│ 5. Update state/progress.json                                │
│ 6. Return: {"ok": true, "batch": N}                          │
└─────────────────────────────────────────────────────────────┘
```

### 7.2 Core Principles

| Principle | Description |
|------|------|
| Main Agent reads no data | It does not read raw data; it reads only the batch count from the progress file |
| SubAgent handles data | Preparation and execution SubAgents load their own data independently |
| State file supports resume_hint | Recover progress from progress.json |

### 7.3 Comparison with the Traditional Pattern

| Item | Traditional Pattern | Preparation SubAgent Pattern |
|--------|----------|-------------------|
| Main Agent duty | Read data → calculate batches → start execution | **Reads no data** and only starts SubAgents |
| Batch calculation | Done in main Agent context | Done by the **Preparation SubAgent** |
| Context pollution | Main Agent context grows after reading data | Main Agent stays clean |

---

## 8. Minimal Return Format

### 8.1 Standard Return

**After completion, the Agent returns only**:

```json
{"ok": true, "batch": 1, "count": 30}
```

### 8.2 Optional Extra Fields

| Field | Type | Best Fit | Example |
|------|------|----------|------|
| `batch` | int | Batch task | `1`, `2`, `3` |
| `count` | int | Processed item count | `30`, `15` |
| `dimension` | string | Multi-dimensional analysis | `"thinking"`, `"writing"` |
| `version` | int | Iteration version | `1`, `2`, `3` |
| `line` | int | Single-line task | `42` |
| `topic` | string | Topic identifier | `"ai_agents"` |
| `output_file` | string | Main output file | `"batch_1.json"` |

### 8.3 Return Examples

| Task Type | Return Example |
|----------|----------|
| Batch evaluation | `{"ok": true, "batch": 1, "count": 30}` |
| Dimension analysis | `{"ok": true, "dimension": "thinking", "version": 3}` |
| Single-item processing | `{"ok": true, "line": 42}` |
| Failure | `{"ok": false, "batch": 1, "err": "API timeout"}` |

### 8.4 Return Rules

✅ Write every analysis result to a file and return only minimal status.

⚪ A short summary or key statistic may be returned, such as `{"ok": true, "summary": "Processed 30 items; 8 had high scores"}`. Accumulation is manageable with 1M context.

❌ Do not return full file contents or details for many items.

---

## 9. Parameter Collection Standard

> **Do not use the AskUserQuestion tool.** Collect every parameter through text Q&A: The Agent outputs the full question list directly, and the user replies once in free text.
>
> Reason: AskUserQuestion interrupts automation by breaking Bridge calls and blocking a pipeline. Text Q&A does not.
>
> Note: This rule governs Skill runtime only. In other standards, "user confirmation" also means ordinary text confirmation unless that workflow declares a dedicated confirmation method.

### 9.1 Independent Parameter Completion (Before Asking)

> **Core principle**: Infer instead of asking. When the user gives some parameters, the Agent infers missing ones.

#### 9.1.1 Three Parameter Levels

| Level | Description | Handling | Example |
|------|------|---------|------|
| **L-required** | Cannot be inferred; user must provide it | Collect through text Q&A | URL, keyword, target account |
| **L-inferred** | Can be inferred from context, input, or history | Agent decides independently | Language, style, depth, market |
| **L-default** | Has a reasonable default | Use config/default.json directly | batch_size, timeout, format |

#### 9.1.2 Independent Inference Strategy

```
1. Analyze input → infer from text/URL/files the user supplied
2. Read run history → infer preferences from recent Runtime runs */config.json
3. Analyze task context → infer from the current conversation or file contents
4. Use domain knowledge → Agent makes an independent expert choice
5. Use defaults → config/default.json
```

#### 9.1.3 Record Inferred Results

Mark the source of independently inferred parameters in config.json:

```json
{
  "language": "en",
  "language_source": "inferred_from_input",
  "depth": "deep",
  "depth_source": "history_preference"
}
```

Allowed `*_source` values: `user_input` / `inferred_from_input` / `history_preference` / `domain_knowledge` / `default`

#### 9.1.4 Good vs Bad

| Good | Bad | Reason |
|----|-----|------|
| Infer language from the input URL without asking | Ask the user to choose a language every time | Reduces interaction and keeps automation |
| Record `"language_source": "inferred_from_input"` in `config.json` | Infer without recording the source | Traceable and easier to debug |
| Ask every L-required parameter in one round | Ask one parameter per round | One collection round reduces back-and-forth |

#### 9.1.5 Design Principles

| Principle | Description |
|------|------|
| **Infer instead of asking** | Reduce interaction and preserve a fully automated experience |
| **Analyze every item independently** | Do not apply one template; decide from the actual content each time |
| **Inference is traceable** | Record the source for debugging |
| **Inference can be overridden** | An explicit user value always wins |
| **Ask only when uncertain** | Use text Q&A only when a value truly cannot be inferred |

---

### 9.2 Text Q&A Format Standard

**If the trigger text has enough information, extract it and do not ask. If L-required parameters are missing, output one question list, wait for one reply, then execute directly.**

#### 9.2.1 Question-List Format

```markdown
Provide these parameters in one reply:

**Required**:
1. **Input path** — File or folder to process
2. **Keyword** — Topic keyword

**Optional** (inferred when omitted):
3. **Language** — en-US / fr-FR / ja-JP; inferred from content by default
4. **Depth** — quick / standard / deep; default standard
5. **Style** — professional / casual / friendly; inferred from content by default
6. **Market** — US / CN / JP / Global; inferred from language by default
7. **Output format** — markdown / html / json; default markdown
8. **Batch size** — 10 / 30 / 50; default 30
```

#### 9.2.2 Format Requirements

| Element | Rule |
|------|------|
| Required/optional groups | Required first, optional second |
| Item format | `number. **parameter name** — option list, default XXX` |
| Default label | Every optional item states a default or inference rule |
| Self-explanatory question | Parameter name + options + default are enough; no extra example is needed |
| One collection round | Ask every parameter together and execute after one user reply |

### 9.3 Parsing Tolerance (Natural Agent Ability)

After the user replies, the Agent extracts parameters from free text. **The Skill does not need parsing logic.**

| User Input Form | Agent Behavior |
|-------------|-----------|
| Numbers such as `1 3 5` | Match values for the numbered options |
| Keywords such as `deep ghost` | Match keywords among allowed values |
| Free description such as `use deep mode and publish to Ghost` | Extract parameters by meaning |
| Mixed, such as `~/file.md deep 3` | Parse path + keyword + number together |
| Empty reply | Use all defaults |

The Skill defines only questions and possible values. The Agent parses them. Missing parameters follow independent inference under §9.1.

### 9.4 Required Declaration in step01

Every step01 file must contain:

```markdown
> Do not use the AskUserQuestion tool. Use text Q&A for every question.
```

---

## 10. Pre-Checks and Post-Validation

> ⚪ The validation mechanisms below apply to **critical steps** such as API calls and file writes. A simple step may omit them.

### 10.1 Pre-flight Check

⚪ Before a critical step, validate dependencies and fail fast:

| Check | Description |
|--------|------|
| Input file exists | Prior-step output exists and is not empty |
| Configuration valid | config.json can be parsed |
| Prior step complete | Prior-step status in progress.json is completed |

### 10.2 Post-write Validation

After a step, run five-layer validation to confirm valid output.

> **See `skill-error-common-fixes.md` §11 for the five-layer definition and reference implementation**

---

## 11. Timeout and Error Handling

| Executor | Default Timeout | Maximum Timeout |
|--------|---------|---------|
| Script | 120 seconds | 600 seconds |
| SubAgent | 300 seconds | 600 seconds |
| Batch processing | 600 seconds | 1200 seconds |

**Handling strategy**: retry for a temporary failure → skip a non-critical single batch → stop on a critical step or exhausted resources

> **See `skill-error-common-fixes.md` for detailed L0-L6 error classes, retry policy, and recovery flow**

---

## 12. Variable Placeholders

→ See `skill-config-parameter-standard.md §13`

---

## 13. Multi-Flow Pattern

> See `../skill-core-file-declaration.md` §10 for the **full standard**. This section lists only step-document naming conventions.

- Mode-specific steps: `workflow/{mode}/stepNN-*.md`
- Shared steps: `workflow/stepNN-*.md` or `workflow/shared/`

---

## 14. Synchronization Points

A synchronization point is a constraint between steps. It ensures the prior phase is **fully complete** before the next begins.

### 14.1 Use Cases (Parallel Tasks Only)

| Case | Description |
|------|------|
| Parallel processing of several topics | Merge only after every topic finishes |
| Multi-batch Agent execution | Enter the next phase only after every batch finishes |
| Later step depends on aggregation | Needs prior results before creating final output |

### 14.2 Simplified Rules

- Use an **implicit check** by default: enter the next step only when every parallel item has `status=completed`.
- **No fixed output label is required**, reducing noise.
- A linear flow may omit synchronization-point notes.

---

## 15. Type-Based Routing

Choose a processing flow by data type. Some types may skip certain steps.

### 15.1 Design Pattern

Declare routing rules in Step 01:

```markdown
## Step 01 Routing Logic

| Type | Condition | Processing Flow | Skipped Steps |
|------|---------|---------|---------|
| standalone | `publish_method === "standalone"` | Full flow | None |
| quote | `publish_method === "quote"` | Full flow | None |
| thread | `publish_method === "thread"` | Light flow | Step 02-05 |
```

### 15.2 Record Skipped Steps

A routed item marks why it skipped steps:

```json
{
  "index": 1,
  "type": "thread",
  "skipped_steps": ["step02", "step03", "step04", "step05"],
  "skip_reason": "A Thread does not need illustrations and goes directly to publishing"
}
```

### 15.3 Merge Outputs

The final step merges outputs of every type:

```markdown
## Step N (Final Output)

1. Read output from the full flow
2. Read output from routed items
3. Merge in original order
4. Write the final output file
```

### 15.4 Routing Configuration Example

```json
{
  "routing_rules": [
    {
      "type": "thread",
      "condition": "publish_method === 'thread'",
      "skip_steps": ["step02_research", "step03_illustrate"],
      "reason": "A Thread does not need illustration or deep research"
    },
    {
      "type": "quick_share",
      "condition": "priority === 'high' && length < 100",
      "skip_steps": ["step02_research"],
      "reason": "A quick share does not need deep research"
    }
  ]
}
```

### 15.5 Execution Flow with Synchronization Points

```
Step 01: Classify and route
    ├── thread type → mark step02-05 skipped
    └── other types → full flow
         ↓
Step 02: Deep research
    ↓ [BARRIER: research_complete]
Step 03: Generate content
    ↓ [BARRIER: generation_complete]
Step 04: Refine content
    ↓
Step 05: Generate illustrations
    ↓
Step 06: Final merge
    ├── merge full-flow output
    └── merge routed-item output
```

---

## 16. Prerequisite Knowledge Loading

### 16.1 Knowledge Declaration

A step may declare knowledge resources loaded from config context:

```markdown
## Prerequisite Knowledge

- Read: context.writing_style
- Read: context.writing_structure
- Read: context.red_lines
```

Before executing the step, the Agent Reads the file paths declared in context after variable interpolation.

### 16.2 Loading Rules

| Rule | Description |
|------|------|
| Load on demand | Load only context entries declared by this step, not all entries |
| Entries with context.when=init | Already loaded when the Skill started; the step does not declare them again |
| Entries with context.when=on_demand | Must be declared in every step that needs them |
| Load failure | Check the fallback path; without a fallback, return an error |

### 16.3 Typical Step Template

```markdown
# Step 03: Writing

## Prerequisite Knowledge

- Read: context.writing_style (writing style for the target platform)
- Read: context.writing_structure (article-structure standard)

## Execution

1. Write under the language-style rules in writing_style
2. Organize the article under the structure rules in writing_structure
3. After completion, self-check with the human-tone checklist in writing_style

## Output

- `{run_dir}/step03-draft/draft.md`
```

### 16.4 Relationship to Existing `reference/`

| Source | Use | Priority |
|------|------|--------|
| context (AWP knowledge base) | Identity/style/rules shared across Skills | High (SSOT) |
| reference/ (local) | Skill-specific Prompt templates/scoring standards | Medium |
| Embedded in SKILL.md | Summary prompts, no more than five lines | Low (quick reference) |

When context conflicts with reference/, context from the AWP knowledge base wins because the AWP knowledge base is the single source of truth.

---

## 17. Brand Experience Standard

> A Skill is a direct touchpoint for the AWP brand. Every run should convey a consistent brand impression.

### 17.1 Opening (Required)

After every Skill trigger, output an opening before any operation.

**Format**:

```
AWP  -  {Skill name}

{One sentence explaining what the Skill does}
```

**Rules**:

| Rule | Description |
|------|------|
| Fixed prefix | First line is `AWP  - ` + the Skill name |
| Function description | Second line explains the function to the user in one sentence |
| No pleasantries | No exclamation mark and no "welcome," "hello," or "thank you" |
| Full coverage | A zero-interaction Skill also needs an opening |

**Examples**:

| Type | Opening |
|------|--------|
| Content creation | AWP  -  Long-Form Writing<br>Create a deep article in six steps, from research to polish. |
| Utility | AWP  -  Code Redaction<br>Find sensitive data and replace it with placeholders. |
| Video | AWP  -  Intelligent Remix<br>Remix several assets into a storyline with automatic voice and subtitles. |
| Publishing | AWP  -  Ghost Publishing<br>Publish finished Markdown to the Ghost site automatically. |

### 17.2 Self-Explanatory Questions (Required for Interaction)

When a Skill needs user input through text Q&A, every question must explain itself. List the parameter name, allowed values, and default clearly so the user needs no extra explanation.

**Format**: `number. **parameter name** (required/optional) — option list, default XXX`

**Rules**:

| Rule | Description |
|------|------|
| The question contains the answer | List every valid option so the user can choose one |
| Explicit default | State a default or inference strategy for every optional parameter |
| No example | The question is clear enough and needs no example reply |
| One collection round | Ask every parameter together and execute after one user reply |

### 17.3 Closing (Required)

After Skill execution, output a closing.

**Format**:

```
{completion report/output table}

---
AWP  -  {Skill name} completed.
```

**Rules**:

| Rule | Description |
|------|------|
| Fixed format | Separator + `AWP  -  {name} completed.` |
| Report first | Put the completion report/output table before the separator |
| No pleasantries | Do not use "thank you," "best wishes," or "see you next time" |

### 17.4 Placement

| Skill Type | Opening Location | Closing Location |
|-----------|-----------|-----------|
| Has workflow/ | `## Opening` section at the top of step01 | Final step or report template |
| No workflow/ (single step) | `## Opening` section in the SKILL.md body | End of the SKILL.md execution flow |

### 17.5 Brand Consistency Checklist

| Check | Pass Standard |
|--------|---------|
| Opening exists | Contains the `AWP  - ` prefix |
| Example guidance | Every user-input question has an example |
| Closing exists | Contains `AWP  -  {name} completed.` |
| No pleasantries | Does not contain "welcome," "thank you," "best wishes," or "see you" |

---

## Checklist

**All step files**:

- [ ] Named `stepNN-{action}.md` with two digits and hyphen separation
- [ ] Declares an executor: script / MCP / SubAgent / main Agent
- [ ] Contains input and output declarations
- [ ] Contains validation checkpoints using `{step number}{letter}`
- [ ] Contains a "Next Step" pointer

**Step00 preflight**:

- [ ] A multi-step Skill has `workflow/step00-preflight.md`
- [ ] Step00 checks only structure, Runtime, credentials, runtime folders, scripts, and cache
- [ ] Step00 supports `auto` / `strict` / `off`
- [ ] `off` falls back to `auto` without valid cache
- [ ] Step00 does not call paid APIs, download models, or create long-lived environments by default

**SubAgent steps**:

- [ ] Contains an execution-parameter table: subagent_type / model / run_in_background
- [ ] References the Prompt through `reference/prompts/` instead of embedding it
- [ ] Uses minimal JSON return format: `{"ok": true, ...}`
- [ ] Starts no more than four parallel Agents

**Steps with parameter collection (step01)**:

- [ ] Parameters use three classes: L-required / L-inferred / L-default
- [ ] Text Q&A collects them in one round
- [ ] ❌ AskUserQuestion is not used
- [ ] Contains an opening: `AWP  -  {name}`

**Brand experience**:

- [ ] Opening exists with the fixed prefix
- [ ] Closing exists with the fixed format
- [ ] No pleasantries: welcome/thank you/best wishes/see you
