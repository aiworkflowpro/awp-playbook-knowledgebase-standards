---
document_id: awp-skill-development-standard/advanced/skill-design-pattern-library
language: en
publication: public
source_revision: 1
title: "Skill Design Pattern Library"
purpose: Three core principles and five reusable patterns for Skill architecture design
category: Standard
prerequisites:
  - ../skill-core-file-declaration.md
  - skill-step-document-standard.md
  - skill-script-file-standard.md
see_also:
  - ../../awp-prompt-writing-standard/prompt-format-eight-part.md
  - skill-config-parameter-standard.md
---

# Skill Design Pattern Library

> This document defines **architecture design patterns** for Skills: three core principles, eleven reusable patterns, and an anti-pattern list.
> Output responsibility: provide decision guidance and implementation templates for choosing the right Skill architecture pattern. This is a global shared reference. All Skills use the same copy.
>
> The three principles are design constraints. The patterns are reusable templates. Most complex Skills combine several patterns.

---

## 0. Design Philosophy

> The three principles below form the foundation of pattern design. Every pattern serves these principles.

## Structure

| # | Question | Section | Main Content |
|---|------|------|---------|
| §1 | What are the core principles? | Three Principles | Progressive disclosure / composability / portability |
| §2 | Which patterns exist? | Pattern Quick Reference | P1-P11 quick-reference table |
| §3 | Which design viewpoint should be used? | Two Design Viewpoints | Problem-first / Tool-first |
| §4 | How is each pattern used? | Design Patterns (11) | Structure, key methods, matching standards |
| §5 | How should a pattern be chosen? | Pattern Selection Decision Tree | Route by core need |
| §6 | What must not be done? | Anti-Pattern List | Description / content / structure / scripts / tests / **changes (no patches)** |

## 1. Three Principles

### 1.1 Progressive Disclosure

Use a three-layer loading model. Load only what is needed and keep the code clear:

| Layer | Content | When Loaded |
|------|------|---------|
| L1: YAML frontmatter | `name`, `description` | Every conversation, when Claude decides whether to activate it |
| L2: SKILL.md body | Core instructions and workflow | Only when the Skill is activated |
| L3: Linked files | `reference/`, `scripts/`, `assets/` | Only when a specific step needs them |

**Design rule**: Keep SKILL.md concise, at no more than 800 lines. Move details to `reference/`. With a 1M context window, progressive disclosure shifts from "prevent overflow" to a practice that "keeps things clear."

---

### 1.2 Composability

Each Skill does one thing. Combine Skills to build complex flows:

- Skill + an MCP Server the user already has
- Skill + other Skills that cover different workflow stages

Matching standard: the single-responsibility requirement in `../skill-core-file-declaration.md`.

---

### 1.3 Portability

A Skill should run across environments: Claude.ai / Claude Code / API.

**Design rule**: Do not assume a runtime environment when writing instructions. Do not hard-code platform-specific paths or tools.

---

## 2. Pattern Quick Reference

| Pattern | Core Structure | Typical Use | Key Files |
|------|----------|---------|---------|
| P1 Sequential Workflow | Step 1 → 2 → 3 | Report generation, data processing | `skill-step-document-standard.md`, `progress.json` |
| P2 Multi-MCP Coordination | Phase A → B → C | Cross-platform publishing, multi-source collection | `skill-script-file-standard.md`, intermediate JSON |
| P3 Iterative Refinement | Draft → Validate → Fix | Content creation, quality improvement | `skill-prompt-template-standard.md`, validation scripts |
| P4 Context-Aware Selection | Input → decision tree → path | Multi-format processing, conditional routing | `skill-config-parameter-standard.md` §11, routing logic |
| P5 Domain Intelligence | Input → rules → execution | Compliance checks, expert packaging | `reference/`, audit logs |
| P6 Agent Teams Collaboration | Lead → assign → teammates execute | Cross-module development, multi-role collaboration | `TeamCreate`, `SendMessage` |
| P7 Plugin Distribution | Skill → package → Marketplace | Share a Skill across teams or communities | `.claude-plugin/`, `plugin.json` |
| P8 Scheduled Execution | /loop → Skill → repeat | Monitoring, inspection, polling | `/loop`, `CronCreate` |
| P9 Visual Output | Skill → script → HTML | Charts, reports, dashboards | `scripts/`, `webbrowser` |
| P10 Chained Composition | Skill A → Skill B → Skill C | End-to-end workflows | File conventions, $ARGUMENTS |
| P11 Knowledge Binding | Embedded workflow + referenced knowledge | Creation that needs identity, style, or rules | `skill-context-loading-standard.md`, config context |

---

## 3. Two Design Viewpoints

| Viewpoint | Starting Point | Best Fit |
|------|------|---------|
| **Problem-first** | What result does the user need? → the Skill coordinates tools | Workflow automation, multi-step tasks |
| **Tool-first** | The user has an MCP → the Skill teaches its proper use | MCP enhancement, expert knowledge packaging |

---

## 4. Design Patterns (11)

### P1: Sequential Workflow

**Use when**: A process has several steps, with dependencies between them.

```
Step 1 → Step 2 → Step 3 → Step 4
       ↑ data handoff  ↑ validation check
```

Matching standard: linear flows and validation checkpoints in `skill-step-document-standard.md`.

| Key Method | Description |
|----------|------|
| Explicit step order | Use `stepNN-{action}.md` names to make order clear |
| Data handoff between steps | Pass file paths, not file contents |
| Validate every step | Write to `progress.json` when each step ends |
| Roll back on failure | Return to the previous step or stop when a failure is found |

---

### P2: Multi-MCP Coordination

**Use when**: A workflow crosses several services.

```
Phase 1 (MCP-A) → Phase 2 (MCP-B) → Phase 3 (MCP-C) → Phase 4 (MCP-D)
```

Matching standard: collection-operation rules in `skill-script-file-standard.md` §13.

| Key Method | Description |
|----------|------|
| Isolate phases | Each Phase calls only one MCP |
| Pass data across MCPs | Decouple services with intermediate files such as JSON or MD |
| Validate between phases | Confirm that the prior Phase output exists before continuing |
| Centralize error handling | Catch errors from every Phase in the main conversation layer |

---

### P3: Iterative Refinement

**Use when**: Output quality needs several rounds of improvement.

```
Draft → Validate → Fix Issues → Re-validate → (repeat until pass) → Finalize
```

Matching standard: iterative Prompt design in `skill-prompt-template-standard.md` §8.

| Key Method | Description |
|----------|------|
| Explicit quality standard | Define measurable acceptance conditions in `reference/` |
| Validation scripts | Prefer code validation to language instructions because it is more dependable |
| Exit condition | Set a maximum number of iterations to prevent an endless loop |
| Flexible Token budget | The Token limit may grow with each round; see `skill-prompt-template-standard.md` §8.3 |

---

### P4: Context-Aware Selection

**Use when**: The same goal needs a different tool or path based on context.

```
Input → decision tree → Path A (Tool X)
                      → Path B (Tool Y)
                      → Path C (Tool Z)
```

Matching standard: type-based routing in `skill-step-document-standard.md` §15.

| Key Method | Description |
|----------|------|
| Clear conditions | Route on objective facts such as a file extension or field value |
| Fallback option | Provide a fallback when no path matches |
| Transparent decision | Record which path was chosen and why |

---

### P5: Domain Intelligence

**Use when**: The main value of the Skill is expert knowledge, not tool calls.

```
Input → domain-rule check → compliant? → execute → audit record
                                  → not compliant → flag → human review
```

Matching standard: the `reference/` folder mechanism and constants in `skill-config-parameter-standard.md` §11.

| Key Method | Description |
|----------|------|
| Put domain knowledge in logic | Write rules in `reference/` and cite them from step documents |
| Check compliance before work | Finish validation in Step 1 to avoid useless execution |
| Full audit trail | Write every decision to a log file |

---

## 5. Pattern Selection Decision Tree

```
What is the core of your Skill?
├── Multi-step process → P1 Sequential Workflow
│   └── Crosses several MCPs? → P2 Multi-MCP Coordination
├── Output quality is critical → P3 Iterative Refinement
├── Several paths lead to the same goal → P4 Context-Aware Selection
├── Expert knowledge package → P5 Domain Intelligence
├── Cross-module, multi-role collaboration → P6 Agent Teams (experimental)
├── Must be distributed to others → P7 Plugin Distribution
├── Must run repeatedly on a schedule → P8 Scheduled Execution
├── Needs interactive visualization → P9 Visual Output
└── Several Skills work in sequence → P10 Chained Composition
```

> Most complex Skills combine patterns. For example, P1 + P3 means a sequential flow with iterative refinement.

---

### P6: Agent Teams Collaboration (Experimental)

**Use when**: A multi-role task needs cross-module work in separate contexts.

```
Team Lead → create tasks → Teammate A (Module 1)
                         → Teammate B (Module 2)
                         → Teammate C (Testing)
             ← report done ← merge results
```

> **Status**: Research preview. Enable it with `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`. See skill-platform-constraint-limits.md §9.

| Key Method | Description |
|----------|------|
| Task size | Give each teammate 5-6 tasks; use teams of 3-5 people |
| Communication mode | Direct message vs broadcast; broadcasts cost more, so use them with care |
| Task dependencies | Set dependencies with `addBlockedBy` |
| Difference from SubAgent | Use SubAgent for bulk data processing and Teams for cross-module work |

---

### P7: Plugin Distribution (Skill Packaging)

**Use when**: Package a Skill as an installable Plugin for distribution across teams or communities.

```
Skill development → Plugin packaging → Marketplace publishing → user installation
```

Matching standard: Skill distribution in `../skill-core-file-declaration.md` §12.

| Key Method | Description |
|----------|------|
| Namespace | A Skill inside a Plugin gets a prefix automatically: `plugin:skill` |
| Manifest file | `.claude-plugin/plugin.json` defines name, version, and description |
| Local test | `claude --plugin-dir ./my-plugin` |
| Hot reload | `/reload-plugins` needs no restart |
| Version management | Follow SemVer |

> **When to use a Plugin**: Use one when other people need the Skill. For personal use, put it directly in `~/.claude/skills/`.

---

### P10: Chained Skill Composition

**Use when**: Several single-responsibility Skills form an end-to-end workflow.

```
Skill A (create) → output/article.md
    ↓ file handoff
Skill B (improve) → output/article.md
    ↓ file handoff
Skill C (publish) → publishing succeeds
```

Matching standard: Skill composition and chained calls in `../skill-core-file-declaration.md` §14.

| Key Method | Description |
|----------|------|
| Single responsibility | Each Skill does one thing; composition creates a complex flow |
| File convention | Upstream output/ is downstream input; pass the path |
| Loose coupling | Do not hard-code upstream or downstream Skill names |
| Independent operation | Every Skill can be used on its own |
| Completion note | List optional downstream Skills in the completion report |

**Example chain**:

```
/content-crafting → /content-optimizing → /content-polishing
    → /content-illustrating → /cms-publishing
```

---

### P8: Scheduled Execution (Skill + /loop)

**Use when**: Monitoring, inspection, or polling needs to run repeatedly.

```
/loop 20m /review-pr 1234       # Review the PR every 20 minutes
/loop 5m /monitor-deploy         # Check deployment every 5 minutes
/loop 1h /check-metrics          # Check metrics every hour
```

| Key Method | Description |
|----------|------|
| Fits Task-type Skills | A Skill with `disable-model-invocation: true` can be scheduled by /loop |
| Interval syntax | `5m`/`1h`/`30s`; the default is 10 minutes |
| Session scope | A task lives only in the current session and disappears when it ends |
| Three-day expiry | A loop task expires automatically after three days |
| Nested composition | `/loop` can call any Skill, such as `/loop 30m /simplify` |

**Use cases**:

| Case | Schedule |
|------|---------|
| PR review inspection | `/loop 20m /review-pr {number}` |
| Deployment status monitoring | `/loop 5m check deployment status` |
| Data quality check | `/loop 1h /data-quality-check` |
| Continuous integration monitoring | `/loop 10m check if CI passed` |

---

### P9: Visual Output (Skill + Script → HTML)

**Use when**: Generate an interactive visualization, such as a chart, report, or dashboard, and open it in a browser.

```
Skill → call packaging script → generate HTML → open in browser
```

**Folder structure**:

```
codebase-visualizer/
├── SKILL.md              # Instructions: run the script
└── scripts/
    └── visualize.py      # Generate HTML and open the browser
```

| Key Method | Description |
|----------|------|
| Scripts do the heavy work | Python/Node generates HTML; Claude only coordinates |
| Self-contained HTML | Inline CSS/JS with no external dependency |
| Open in browser | `webbrowser.open(f'file://{path}')` |
| allowed-tools | `Bash(python *)` limits execution to scripts |

**Use cases**: codebase visualization, dependency graphs, test-coverage reports, API documentation, and database Schema diagrams.

---

## 6. Anti-Pattern List

> **Source**: Anthropic official practices. Avoid these common errors when writing a Skill.

### 6.1 Description Anti-Patterns

| Anti-Pattern | Correct Approach |
|--------|---------|
| ❌ Vague description: "Helps with documents" | ✅ Specific description: "Extract text and tables from PDF files" |
| ❌ Ask the user for each missing parameter | ✅ Infer what can be inferred; ask only what cannot be inferred (`skill-step-document-standard.md` §9.6) |
| ❌ First person: "I can help you..." | ✅ Third person: "Extracts text from PDFs" |
| ❌ No trigger cases | ✅ Include a "Use when..." trigger description |

### 6.2 Content Anti-Patterns

| Anti-Pattern | Correct Approach |
|--------|---------|
| ❌ Explain concepts Claude already knows | ✅ Provide only context Claude does not have |
| ❌ Give Claude too many options to choose from | ✅ Give a default plus an escape hatch: "Use X. For Y scenario, use Z instead" |
| ❌ Time-sensitive facts: "Use the old API before August 2025" | ✅ Put old behavior in a collapsed `<details>` block |
| ❌ Inconsistent terms, such as endpoint/URL/route | ✅ Use one term throughout |

### 6.3 Structure Anti-Patterns

| Anti-Pattern | Correct Approach |
|--------|---------|
| ❌ Deep nested references: SKILL→ref1→ref2→ref3 | ✅ Link every reference directly from SKILL.md, one level deep |
| ❌ Windows backslash: `scripts\helper.py` | ✅ Forward slash: `scripts/helper.py` |
| ❌ Vague filenames: `doc2.md`, `file1.md` | ✅ Meaningful filenames: `form_validation_rules.md` |
| ❌ SKILL.md exceeds 800 lines without a split | ✅ Split it into separate files under reference/ |

### 6.4 Script Anti-Patterns

| Anti-Pattern | Correct Approach |
|--------|---------|
| ❌ Leave errors to Claude: `open(path).read()` fails directly | ✅ Handle errors in the script and provide a fallback |
| ❌ Magic number: `TIMEOUT = 47` | ✅ Explain it in a comment: `TIMEOUT = 30  # Typical HTTP timeout` |
| ❌ Assume a tool is installed: "Use the pdf library" | ✅ Declare it explicitly: `pip install pypdf` |
| ❌ MCP tool lacks the service name: `bigquery_schema` | ✅ Fully qualified name: `BigQuery:bigquery_schema` |
| ❌ Reference a path outside the Skill: `~/.claude/knowledge-base/credentials/` | ✅ Internal Skill path: `tools/credentials/api.json` |
| ❌ Import a module outside the Skill | ✅ Keep scripts self-contained or use `scripts/python/shared/` |

### 6.5 Test Anti-Patterns

| Anti-Pattern | Correct Approach |
|--------|---------|
| ❌ Test only one model | ✅ Test Haiku, Sonnet, and Opus separately |
| ❌ Test with invented data | ✅ Test with real cases |
| ❌ Write documentation before evaluation | ✅ Write evaluation before the Skill: EDD |

### 6.6 Change Anti-Patterns (A Skill Is a Living Document; Never Patch It)

> A Skill has no version number and no change log. The git log is its version history. Any historical trace kept "to explain the change" is noise. It weakens correct instructions and misleads future Agents.
> The only action when changing a Skill is **REPLACE**: overwrite the wrong step with the right one and remove every old trace.

| Anti-Pattern | Correct Approach |
|--------|---------|
| ❌ Add ⚠️ beside a wrong step: "Warning: this fails; use ..." | ✅ Delete the wrong step and write the right one; remove the warning too |
| ❌ Keep the old path with a note: "See the new path below" | ✅ Keep only the new path; delete the full old block |
| ❌ Add fallback or if-else compatibility for old behavior | ✅ Keep only the new behavior; delete the old branch |
| ❌ Accumulate comparisons: "Before X, now Y; note the difference" | ✅ Write only Y and do not mention X; readers do not need the history |
| ❌ Turn a lesson into a long ⚠️ warning box | ✅ Put the conclusion into the step itself as one imperative line |
| ❌ Write "2026-04-XX fixed XX bug" in SKILL.md or a step file | ✅ Put it in the git commit message; do not put dated patches in the document |
| ❌ Support both old and new parameters or commands | ✅ Keep the new form only and remove the old one fully |
| ❌ Add "⚠️ Important change: ..." at the top after editing | ✅ A Skill has no announcement area; the commit message is the announcement |

**Three self-check questions** to ask after editing and before committing:

1. Pretend this Skill was written today. Would this ⚠️ warning, comment, or compatibility branch exist? If not, delete it.
2. Who is the reader of this historical comparison? If there is no reader, delete it.
3. If this warning is deleted, will a future Agent repeat the mistake? If not, delete it; the conclusion is already part of the step.

> **Meta-principle**: Patches multiply. After the first ⚠️ appears, the next editor tends to add another one. The document soon becomes a pile of warnings. Reject the first ⚠️.

---

### P11: Knowledge Binding

**Use when**: A Skill needs shared identity, writing style, industry rules, or other knowledge from the Agent AWP knowledge base.

**Core idea**: Embed the workflow, Reference the style. Put workflow steps inside the Skill. Reference style, identity, and rules outside it.

**Structure**:

```
config/default.json
  └── context: { identity, voice, writing_style, red_lines, persona }
           │              │                │              │           │
           │   source:kb  │   source:kb    │  source:kb   │ source:local
           ▼              ▼                ▼              ▼           ▼
    {kb}/identity/    {kb}/styles/       {kb}/styles/{platform}/   {kb}/rules/        reference/presets/
    (init)         (init)       (on_demand)    (on_demand)   (on_demand)
```

**Three knowledge-loading tiers**:

| Tier | Content | Token Cost (Estimate; Adjust to Actual Use) | When Loaded |
|----|------|------|---------|
| Tier 1 | Global identity, injected automatically by CLAUDE.md | ~100 | Every conversation |
| Tier 2 | Identity + language style | ~500 | Skill start: `!`command`` or MCP |
| Tier 3 | Writing style + structure + red lines | ~2000+ | On demand by a step through MCP |

**Applicability**:

| Condition | Use P11 |
|------|---------|
| The Skill needs an identity or persona | ✅ |
| The Skill needs a writing style | ✅ |
| The Skill needs industry rules or red lines | ✅ |
| Several Skills share the same knowledge | ✅ |
| The Skill is portable and will be distributed | ❌ Use local + fallback |

**Difference from P5 Domain Intelligence**:

| | P5 Domain Intelligence | P11 Knowledge Binding |
|---|------------|------------|
| Knowledge source | Internal Skill reference/ | External AWP knowledge base |
| Sharing | Used by one Skill | Shared across Skills |
| Update | Edit a file inside the Skill | Edit the AWP knowledge base; every Skill receives the change |

**Direct Read vs Extraction Decision** (experience from 2026-03):

| Condition | Method | Reason |
|------|------|------|
| Deterministic rules such as style, standards, identity, or red lines, ≤ 5K tokens | **Direct read** — the execution step reads the original KB text | Extraction loses structured formats such as tables, levels, and fixed-element definitions. With a 1M context window, about 2K tokens do not affect attention |
| Needs creative judgment, such as published work, hit examples, or industry data | **SubAgent extraction** — the context step turns it into execution instructions | Large source sets need selection and creative synthesis. Isolated SubAgent processing is more stable |
| Deterministic rules larger than 10K tokens | **SubAgent extraction** | Very large direct reads can affect attention |

> **Field lesson**: A SubAgent in the long-writing Skill reduced a 700-line style file to a 54-line style-rules.md. It lost the three-part structure required for the one-click prompt field, so the output broke the rules. Direct reading removed the loss.

**Direct-read implementation**: The context step generates only `style-paths.json`, a list of paths. The execution step reads the path list and then reads each original KB file. See `skill-context-loading-standard.md` §1.2.1.

**Common combinations**: P11 + P1 (Sequential Workflow), P11 + P3 (Iterative Refinement), P11 + P10 (Chained Composition)

---

## Checklist

**Pattern selection**:

- [ ] The chosen design pattern matches the Skill need under the §5 decision tree
- [ ] A complex Skill states its pattern combination, such as P1 + P3

**Pattern implementation**:

- [ ] The pattern implementation follows the matching standard files
- [ ] It uses none of the anti-patterns in §6
- [ ] It follows the three principles: progressive disclosure / composability / portability

> ⚠️ Deviation format: `⚠️ Deviation: {rule} | Reason: {reason}`. It applies only to the current artifact and does not set a precedent.

