---
document_id: awp-skill-development-standard/advanced/skill-prompt-template-standard
language: en
publication: public
source_revision: 2
title: "Prompt Template Standard"
purpose: The single authoritative guide for writing SubAgent Prompts
category: Standard
prerequisites:
  - skill-step-document-standard.md
  - skill-runtime-data-standard.md
see_also:
  - ../skill-core-file-declaration.md
---

# Prompt Template Standard

> This document is the full standard for every SubAgent Prompt template under `reference/prompts/`.
> For the eight-part prompt format, see `../../awp-prompt-writing-standard/prompt-format-eight-part.md`.
> Output responsibility: define the SubAgent's cognitive boundary—role, task, input/output, allowed scope, and return format.
> Each SubAgent task has its own `prompt-{function}.md` file.

---

## 0. Design Philosophy

| Principle | Notes |
|------|------|
| ✅ **A Prompt is a cognitive boundary** | A Prompt is the SubAgent's full cognitive boundary. It defines everything the SubAgent can see and should do |
| ✅ **Work independently without asking back** | A good Prompt gives the SubAgent enough context to complete the task alone, without asking the main Agent |
| ✅ **Paths before content** | Pass paths instead of content to keep the main conversation context clean |

---

## Organization

| # | Question | Section | Core content |
|---|------|------|---------|
| §0 | Why design it this way? | Design Philosophy | A Prompt is the SubAgent's cognitive boundary; work independently without asking back |
| §1 | Where does the Prompt file live? | File Responsibility | Purpose and naming format of the reference/prompts/ directory |
| §2 | How is the file named? | Naming Standard | Choosing function prefixes (context-/batch-/eval-, and so on) |
| §3 | What is the internal template structure? | Template Document Structure | Standard foundation for prompt-{function}.md |
| §4 | What can a SubAgent do? | Allowed Scope | Selection strategy for default allowed scope vs. strict prohibitions |
| §5 | How are code blocks nested? | Code Block Standard | Format for nesting code blocks inside a Prompt |
| §6 | How is data passed to a SubAgent? | Path-First Principle | Pass paths, not content; directory-reading rules; variable rules |
| §7 | How do you write a Prompt for an autonomous task? | Autonomous Task Prompt Template | General template for evaluation/classification/generation/analysis tasks |
| §8 | How do you design iterative tasks? | Iterative Prompt Design | Initial generation → incremental iteration → Token budget management |
| §9 | How strict should instructions be? | Instruction Freedom Design | Choosing high/medium/low freedom and assessing fragility |
| §10 | What are the core hard design rules? | Core Design Principles | Task boundaries, measurable standards, and traceable output |
| §11 | How does a custom Agent simplify a Prompt? | Relationship Between Custom Agents and Prompts | Preloaded Agent skills simplify Prompts |
| §12 | How are variables written? | Variable Placeholders | Refer to skill-config-parameter-standard.md §13 |

---

## 1. File Responsibility

**Position**: stores SubAgent Prompt templates

**Location**: `reference/prompts/`

**Naming format**: `prompt-{function}.md`

---

## 2. Naming Standard

### 2.1 Choosing a function Prefix

| Prefix | Purpose | Example |
|------|------|------|
| `context-` | Context-loading SubAgent (see `skill-context-loading-standard.md`) | `prompt-context-style.md` |
| `batch-` | Batch-processing task | `prompt-batch-analysis.md` |
| `init-` | Initial generation | `prompt-init-persona.md` |
| `iterate-` | Incremental iteration | `prompt-iterate-merge.md` |
| `final-` | Final output | `prompt-final-report.md` |
| `eval-` | Evaluation and scoring | `prompt-eval-quality.md` |
| `merge-` | Merge processing | `prompt-merge-results.md` |
| `prepare-` | Preparation stage | `prompt-prepare-data.md` |

> The naming format for the `context-` prefix is `prompt-context-{dimension-id}.md` (such as `prompt-context-style.md` and `prompt-context-reference.md`) or `prompt-context-custom-{label}.md` (a Skill-private dimension).

### 2.2 Directory Example

```
reference/prompts/
├── prompt-context-style.md        # Context: style and standards
├── prompt-context-reference.md    # Context: references and inspiration
├── prompt-context-custom-source.md # Context: private dimension (source-text analysis)
├── prompt-batch-analysis.md       # Batch-analysis Prompt
├── prompt-init-persona.md         # Initial persona-generation Prompt
├── prompt-iterate-merge.md        # Incremental-iteration Prompt
└── prompt-final-report.md         # Final-report generation Prompt
```

---

## 3. Template Document Structure

Every `prompt-{function}.md` must contain this structure:

````markdown
# Prompt: {function_name}

> **Purpose**: {one-sentence description}
> **Applicable step**: Step NN

---

## Prompt Template

Use these parameters to start the SubAgent from the main conversation (full parameter definitions are in `skill-step-document-standard.md` §5.1):

| Parameter | Value |
|------|-----|
| subagent_type | "general-purpose" |
| model | "claude-sonnet-4-6" |
| run_in_background | true |

> **Note**: `Task()` is a pseudo-representation of the Agent tool, **not Python code**. The actual call passes parameters through the CC Agent tool. Standard format and parameter definitions are in `skill-step-document-standard.md` §0 glossary + §5.1 parameter table.

```
Task(
  subagent_type: "general-purpose",
  model: "claude-sonnet-4-6",
  run_in_background: true,
  prompt: "
    You are an expert in XX. Please complete the task for batch {batch_id}/{batch_count}.

    [Input file]
    {run_dir}/batches/batch_{batch_id}_input.json

    [Reference documents] (read as needed)
    - {run_dir}/`output/persona.md`
    - {skill_dir}/reference/xxx.md

    [Your task]
    1. Read the reference documents to understand the task background
    2. Read the batch data and process it item by item
    3. Write the results to {run_dir}/`output/batch_{batch_id}.json`

    [Return format - minimal]
    Success: {\"ok\": true, \"batch\": {batch_id}, \"count\": N}
    Failure: {\"ok\": false, \"batch\": {batch_id}, \"err\": \"short description\"}

    Note: all results have been written to files; no need to repeat them in the return.
  "
)
```

---

## Variable Description

| Variable | Type | Description |
|------|------|------|
| `{run_dir}` | Path | Current run directory |
| `{skill_dir}` | Path | Current Skill installation directory |
| `{batch_id}` | int | Batch number (starting from 1) |
| `{batch_count}` | int | Total batch count |
| `{count}` | int | Item count in this batch |
| `{input_path}` | Path | Current task input file path |
| `{progress_path}` | Path | Progress file path |
| `{output_path}` | Path | Current task output file path |

**Model naming rules**:
- Task's `model` must match SKILL.md's `model`
- If aliases are needed, declare mappings explicitly within the project (to avoid confusion)

---

## Output Format

{expected output JSON structure}

---

## Usage Example

{complete invocation example}
---
````

## 4. Allowed Scope (✅ Default)

SubAgents use "**allowed scope**" by default instead of "strict prohibitions," so they can finish the task.
Use strict prohibitions only in high-risk or tightly constrained cases.

```markdown
## Allowed Scope (Default)

1. **Allowed tools**: Read / Write / Glob / Bash (trim to task needs)
2. **Allowed code**: only the minimum code needed to complete the task (such as script snippets or format conversion)
3. **Allowed information requests**: only necessary clarification questions

The Agent should focus on the task goal and flexibly choose the operations needed to complete it.
```

**Placement**: at the start of the Prompt or after the task description

**Purpose**:
- Let the SubAgent read and process all material needed
- Avoid hidden blocks caused by "Read/Write only" (cannot traverse directories or find files)
- Keep the task boundary and prevent drift

### 4.1 Strict Prohibitions (Optional)

Use only in high-risk cases or where tight constraints are required:

```markdown
## Strict Prohibitions (Optional)

1. **Do not generate any code** (Python/JavaScript/Shell, etc.)
2. **Do not call any tools** (except Read/Write)
3. **Do not request more information**
4. Complete the task directly
```

### 4.2 Prompt-Level Constraints vs. Platform-Level Constraints

| Constraint type | Source | Adjustable | Example |
|---------|------|--------|------|
| **Prompt level** | Defined in this document | ✅ Based on task needs | No code generation and no requests for information |
| **Platform level** | Defined in skill-platform-constraint-limits.md | ❌ Hard limit | 25k-token output and Task cannot be nested |

**Prompt-level constraints** (adjust as needed):
- Tool scope (Read/Write/Glob/Bash, and so on)
- Code-generation permission (minimum, only when needed)
- Interaction limits

**Platform-level constraints** (must not be broken) → see `skill-platform-constraint-limits.md`:
- A SubAgent cannot call Task recursively
- Bash output has a 30K-character limit (truncated in the middle)
- MCP output has a 25K-token limit

---

## 5. Code Block Standard

> Code-format rules inside Prompt templates. Claude can handle many kinds of nested code blocks correctly.

| ⚪ Optional practice | Notes |
|------|------|
| Use ```` ```` ```` outside | The whole Prompt template may be wrapped in four backticks |
| Inner code | Use three backticks normally; Claude parses it correctly |

---

## 6. Path-First Principle

| Comparison | Pass content | ✅ Pass paths |
|--------|--------|--------|
| Main conversation context | Tokens build up with every batch | Stays clean |
| SubAgent flexibility | Receives passively | Reads what it needs |
| Maintainability | Template and content are coupled | Fully separated |

> **Note for the 1M era**: Passing paths remains the ✅ preferred practice (clearer separation), but passing a reasonable amount of content is no longer a serious problem with 1M context.

---

### 6.1 Directory Reading Rules (Required)

When the input path is a directory:
- **Do not Read the directory directly** (it triggers EISDIR)
- **Glob the file list first**, then Read important files one by one

---

### 6.2 Input Priority (⚪ Optional)

When several source layers exist, the SubAgent should read from "raw" to "processed":

1. Raw interview / raw input
2. Round Q&A / detail additions
3. Summary / brief / outline
4. Study report / secondary summary

Purpose: prevent a summary from replacing original intent and keep information accurate.

---

### 6.3 Variable Rules (Required)

- **No variable arithmetic** (such as `{version-1}` and `{round_num-1}`)
- When "previous version" or "latest" is needed, use a stable pointer file, such as:
  - `feedback_latest.md`
  - `round_latest_questions.json`

---

### 6.4 Minimal Return (Optional Improvement)

The SubAgent return format ⚪ should be minimal:
`{"ok": true}`
Write everything else to files.

> **1M-era update**: Returning a short amount of summary information (such as key statistics) is acceptable. Full content still ✅ must be written to files.

---

## 7. Autonomous Task Prompt Template

### 7.1 Applicable Task Types

| Task type | Example | Core capability |
|----------|------|----------|
| Evaluation/scoring | Content-quality evaluation and priority scoring | Multidimensional judgment + reasons |
| Classification/labeling | Sentiment analysis, topic classification, and tag generation | Category judgment + confidence |
| Generation/creation | Summary generation, translation, and rewriting | Content output + quality control |
| Analysis/insight | Trend analysis, anomaly detection, and comparative analysis | Deep understanding + distilled conclusions |
| Extraction/conversion | Information extraction, format conversion, and structuring | Exact extraction + format rules |

### 7.2 General Template Structure

```markdown
# Task: {task_name} - Batch {batch_id}/{batch_count}

## Allowed Scope (Default)

1. **Allowed tools**: Read / Write / Glob / Bash (trim to task needs)
2. **Allowed code**: only the minimum code needed to complete the task
3. **Allowed information requests**: only necessary clarification questions

The Agent should focus on the task goal and flexibly choose the operations needed to complete it.

## Run Paths

- RUN_DIR: {run_dir}
- SKILL_DIR: {skill_dir}
- Reference documents: {skill_dir}/reference/xxx.md (if any)
- Input data: {input_path}
- Progress file: {progress_path}
- Output file: {output_path}

## Execution Steps

1. Read progress.json and confirm the current batch = {batch_id}
2. Read the input data and extract the items for this batch
3. Read reference documents (if any) to understand task standards
4. Execute the task on each piece of data
5. Append results to the output file
6. Update progress.json (mark the batch complete)

## Task Standards

{task-specific judgment standards/dimension definitions/classification rules}

## Output Format

{task-specific output JSON structure}

## Return After Completion

Return only one line of JSON:
{"ok": true, "batch": {batch_id}, "count": {count}}
```

### 7.3 Output Formats by Task Type

| Task type | Core fields | Output example |
|----------|----------|----------|
| **Evaluation/scoring** | `scores`, `total`, `action` | `{"scores": {"dim1": 8}, "total": 8.0, "action": "high_priority"}` |
| **Classification/labeling** | `category`, `confidence`, `tags` | `{"category": "cat_a", "confidence": 0.85, "tags": ["tag1"]}` |
| **Generation/conversion** | `summary`, `key_points` | `{"summary": "...", "key_points": ["key point 1"]}` |
| **Analysis/insight** | `similarities`, `differences`, `insight` | `{"insight": "core insight", "confidence": 0.8}` |

---

## 8. Iterative Prompt Design

### 8.1 Initial Generation (Round 1)

```
Task(
  prompt: "
    You are an expert in {dimension} analysis. Analyze the batch content and generate the first draft archive.

    [Input file]
    {run_dir}/batches/batch_1/{dimension}.md

    [Token limit] {base_limit}

    [Output file]
    {run_dir}/step03-analyze/iterations/{dimension}/v1.md

    [Return format]
    {\"ok\": true, \"dimension\": \"{dimension}\", \"version\": 1}
  "
)
```

### 8.2 Incremental Iteration (Round 2+)

```
Task(
  prompt: "
    You are an expert in {dimension} analysis. Please merge the new batch content into the current version.

    [Current base version]
    {run_dir}/step03-analyze/iterations/{dimension}/v{prev}.md

    [New batch]
    {run_dir}/batches/batch_{round_num}/{dimension}.md

    [Merge priority]
    1. New findings > reinforcing consensus
    2. Core points > edge details
    3. High engagement > ordinary
    4. Unique expressions > common expressions

    [Content trimming rules]
    - Evidence trimming: multiple → 1-2 representative examples
    - Expression condensation: long sentences → phrase summaries
    - Duplicate merging: similar points → merged expression
    - Low-frequency deletion: non-core items appearing only once → delete

    [Token limit] {current_limit} (base +{growth_pct}%)

    [Output file]
    {run_dir}/step03-analyze/iterations/{dimension}/v{round_num}.md

    [Return format]
    {\"ok\": true, \"dimension\": \"{dimension}\", \"version\": {round_num}}
  "
)
```

### 8.3 Token Budget

> **Simplified for the 1M era**: A complex elastic Token-budget formula is no longer needed. Give the task enough room.

| Task type | ⚪ Reference Token limit | Notes |
|---------|---------------|------|
| Single-dimension analysis | 8,000–15,000 | Give it enough room; no need to count every token |
| Multi-dimension merge | 15,000–30,000 | Set by dimension complexity |
| Deep report | 30,000–60,000 | Use the space available in 1M context |

The Token limit may grow naturally between iteration rounds. No formula is required.

---

## 9. Instruction Freedom Design

Choose the instruction style based on task fragility:

| Freedom | Use case | Instruction style | Example |
|--------|---------|---------|------|
| High | Several approaches can work | Give a direction | "Analyze the user's writing style" |
| Medium | There is a preferred pattern | Framework + room to vary | "Analyze with this framework: wording, sentence form, and tone" |
| Low | The operation is fragile | Exact command | "Run `uv run python scripts/python/x.py`" |

**Selection rules**:
- Data collection/file operations → low freedom (exact commands)
- Semantic analysis/evaluation scoring → medium freedom (framework constraints)
- Creative generation/style writing → high freedom (give direction)

### 9.1 Fragility Assessment Checklist

Assess task fragility to choose instruction freedom:

| Check | High fragility (low freedom) | Low fragility (high freedom) |
|--------|------------------|------------------|
| Output format | Strict JSON/fixed fields | Free text/Markdown |
| Path operations | Exact paths must be correct | No path dependency |
| Downstream consumption | A downstream flow depends on it closely | Human review |
| Data integrity | Data loss is unacceptable | Some missing data is tolerable |
| Retry cost | Expensive to retry | Can retry at any time |

**Scoring rule**: ≥3 high-fragility items → low freedom; ≤1 high-fragility item → high freedom

**Case comparison**:

| Task | Fragility assessment | Freedom | Instruction style |
|------|-----------|--------|---------|
| API data collection | Path✓ format✓ integrity✓ | **Low** | Exact command + arguments |
| Content-quality evaluation | Format✓ consumption✓ | **Medium** | Framework + scoring dimensions |
| Creative copy generation | All low fragility | **High** | Style direction + references |

---

## 10. Core Design Principles

| Principle | Notes |
|------|------|
| Define the task boundary | Prohibitions keep the SubAgent from drifting away from the task |
| Make standards measurable | Scoring/classification standards must be clear and actionable |
| Make output traceable | Every result must include a reason or basis |
| Use a processable format | JSON structures are easy for downstream flows to consume |
| Keep returns minimal | Return only status; write results to files |
| Support resume | Recover interrupted tasks through progress.json |

---

## 11. Relationship Between Custom Agents and Prompts

When a SubAgent uses a custom Agent type (`.claude/agents/*.md`):

| Component | Source | Role |
|------|------|------|
| System prompt | Markdown body in the Agent definition | Agent identity and capability boundary |
| Preloaded knowledge | Skill referenced by the Agent's `skills` field | Domain knowledge (injected at startup) |
| Task instructions | Prompt in the Skill workflow | Specific task for the current batch |

**Simplification**: When the Agent has already preloaded reference documents (through the `skills` field), the Prompt template can omit reference-document paths and describe the task directly.

```
# Traditional pattern (Prompt passes a path)
Task(
    prompt: "Read {skill_dir}/reference/rules.md before performing review..."
)

# Custom Agent pattern (knowledge is already preloaded)
Task(
    subagent_type: "security-reviewer", # rules is already preloaded
    prompt: "Review the code in {run_dir}/step02-code/"
)
```

---

## 12. Variable Placeholders

→ See `skill-config-parameter-standard.md §13`.

---

## 13. Knowledge-Base Path Variables



At runtime, it resolves to the local machine's knowledge-base root path (with no hard-coded path).

### 13.2 Referencing Knowledge in a Prompt

When a step needs knowledge-base content, reference it by context name in the Prompt:

```
Task(
  prompt="""
  [Identity] read context.identity
  [Style] read context.writing_style + context.writing_structure

  Complete the writing task according to the above identity and style:
  {specific task description}
  """
)
```

Before running the Prompt, the Agent first Reads the files declared by context and injects their content into context.

### 13.3 Prohibitions

| Do not | Correct approach |
|------|---------|
| ❌ Hard-code an absolute knowledge-base path in a Prompt | ✅ Get it directly with Read/Glob, or declare it with `context.identity` |
| ❌ Embed 100 lines of style instructions in a Prompt | ✅ Get them as needed directly with Read/Glob |

---

## Checklist

**prompt-{function}.md file**:
- [ ] Includes a role definition, task description, and output format
- [ ] Filename follows the `prompt-{prefix}-{name}.md` format
- [ ] Does not hard-code data in the Prompt (references reference/ or passes a path)
- [ ] Does not hard-code an absolute knowledge-base path in the Prompt
- [ ] Includes an allowed-scope or strict-prohibition declaration
- [ ] Return format is minimal JSON

**Custom Agent integration**:
- [ ] Agent definition includes the required frontmatter fields
- [ ] Preloaded knowledge is referenced through the skills field
