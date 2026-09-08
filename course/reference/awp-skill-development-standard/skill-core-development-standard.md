---
document_id: awp-skill-development-standard/skill-core-development-standard
language: en
publication: public
source_revision: 1
title: "AWP Skill Development Standard"
prerequisites: []
see_also: []
---

# AWP Skill Development Standard

> Full-lifecycle development rules for Skills. When creating or changing a Skill, first find the matching standards file, read it in full, and then act.

> **Inherits**: Constraints from your knowledge base's general rules (Markdown layout/file metadata/L0–L4/lifecycle/naming).
> **Overrides**: Skills do not follow the version/change-log rules for "knowledge files"—SKILL.md / `docs/changelog.md` / frontmatter `version` are all ❌ prohibited. git log is the version history (see `skill-core-file-declaration.md` §3.3).

In this standard, ✅ (required), ❌ (prohibited), ⚪ (optional), and ⚠️ (warning) follow the definitions in "Standards Standard § Constraint Language."

---

## Design Philosophy

- **Self-contained first**: Each Skill is the smallest independent unit that can be distributed and does not depend on hidden outside conventions
- **Progressive complexity**: A one-step Skill needs only SKILL.md; add complexity only when needed (`workflow/`→ scripts/ → config/ → credentials/)
- **Separate declarations from runtime entities**: The Skill directory stores declarations, scripts, and documents. Runtime entities such as environments, models, and caches all go under `~/.awp/runtime/`
- **Startup preflight only when needed**: Every Skill with scripts, credentials, Runtime assets, or a multi-step workflow must have a Step00 startup gate—a multi-step Skill uses `workflow/step00-preflight.md`; a one-step Skill with outside dependencies keeps a Step 00 section inside SKILL.md. A one-step Skill that is knowledge-only / Markdown-only and has no scripts, credentials, or Runtime is exempt from Step00. When Step00 exists, it defaults to `auto`: a valid cache skips heavy checks, but the light entry check cannot be bypassed
- **Orthogonal split**: Each standards file focuses on one concern—file structure, step orchestration, context loading, script execution, and platform constraints stay separate. Cross-file knowledge points to one source through explicit references instead of repeating the text
- **Easy for Agents to run**: Every rule is executable by an Agent and does not depend on human judgment
- **Match freedom to task fragility**: The level of constraint in an instruction should match how fragile the task is—turn fixed, error-prone operations into scripts with low freedom, such as format conversion and batch uploads; give open-ended judgment tasks principle-based guidance and let the model decide, such as content evaluation, translation, and polishing. Too much detail traps a capable model, while too little control lets fragile operations drift (source: Anthropic's official Skill authoring practices; for execution rules, see the script vs SubAgent selection principles in `advanced/skill-script-file-standard.md`)
- **❌ Do not hardcode external knowledge**: A Skill must not hardcode content loaded dynamically from the AWP knowledge base—including brand names, style-element names (fixed elements/blockquote labels/technique lists), platform-rule details, and identity information. This content differs across platforms/styles/brands and must be referenced through variables or read from the AWP knowledge base at runtime. A Skill defines "what type of content to load," not "the exact content." Mapping tables such as style_map and platform_dir_map belong in the configuration layer and may be placed under config/
- **Replace when changing; do not patch**: When changing a Skill, replace the wrong step directly with the right step. ❌ Do not add a ⚠️ warning / trap note / fallback explanation / backward-compatibility branch / "X before, Y now" comparison beside the error. A Skill is a living document, and git log is its version history. No user reads historical warnings; they only leave noise that weakens the correct instruction. Before changing it, ask, "If this Skill were written today, would this warning exist?" If not, do not add it. See `advanced/skill-design-pattern-library.md §6.6 change anti-patterns`

---

## Rules for Changes (Required Reading When a Skill Evolves)

| # | Rule | Bad example | Good example |
|---|------|------|------|
| 1 | Replace what is wrong; do not add a warning | Add ⚠️ "Note: this fails here; use the new method" beside the old step | Delete the old step and write the new one |
| 2 | Fix a wrong path; do not leave a fallback | Keep `~/.claude/knowledge-base/...` + a comment saying "see the new path below" | Write only `{skill_dir}/scripts/...` |
| 3 | When a command changes implementation, replace it directly; do not add a branch | `if old venv exists: use A; else: use B` | Write only the new command |
| 4 | Fold lessons from failures into the step itself | Add a long ⚠️ trap explanation | Turn the conclusion into one imperative line in the step |
| 5 | Do not compare "former / historical / old version" behavior | "It was X before; after 2026-04 it is Y" | Write only Y, as if X never existed |
| 6 | Do not keep compatibility code for retired paths | Support both old and new parameter sets | Support only the new parameters |

> Self-check: Read the changed section after editing and pretend "this Skill was written today." Delete every warning / comment / compatibility branch that "would not exist if it were written today."

---

## Shared Glossary

> The whole standards family uses these terms. Child files do not create aliases or repeat their definitions.

| Term | Definition |
|------|------|
| **SubAgent** | An independent Agent instance started through the Agent tool, formerly called Task in Claude Code. Documents always call it SubAgent; pseudocode uses `Task()` |
| **Main Agent** | The Agent that orchestrates steps in the current conversation |
| **Preparation SubAgent** | A SubAgent dedicated to large-scale data preprocessing (see `advanced/skill-step-document-standard.md` §7) |
| **keyword (run identifier)** | A directory-naming keyword extracted from user input for each run (see `advanced/skill-runtime-data-standard.md` §3) |
| **preflight (startup precheck)** | Step00 startup gate check (see `advanced/skill-step-document-standard.md` §2.4) |
| **Cognitive isolation** | The Main Agent consumes only execution instructions distilled by a SubAgent instead of reading a large amount of material directly, keeping its context clean. This term is used everywhere in the standards; do not call it "zero context pollution" |
| **Runtime (runtime entity)** | Runtime entities such as environments, binaries, models, and caches, all stored under `~/.awp/runtime/` (see `advanced/skill-config-parameter-standard.md` §12 and `advanced/skill-runtime-data-standard.md`) |

---

## Boundary with Command

Starting in 2026, users can call a Skill directly with `/name`, so Command is no longer an entry layer for Skill. Their roles are:

| Dimension | Skill | Command |
|------|-------|---------|
| Form | Directory (SKILL.md + supporting files such as workflow/scripts/config) | One `.md` file |
| Use | Multi-step workflows and tasks that need scripts/credentials/reference resources | Zero-dependency, ready-to-run light tasks |
| Call | `/name` or automatic trigger | `/name` |

They share the naming system (`awp-{domain}-{target}-{action}`) and change rules, but evolve independently. For Command rules, see your knowledge base's Command standard.

---

## Structure

Standards files are split into two responsibility layers:

| Layer | Position | Files |
|------|------|------|
| Core standards | Required reading for every Skill | SKILL file + steps + context + scripts + platform + runtime + config + prompts |
| Supporting standards | Read when needed | credentials + docs + troubleshooting + tests + modes |

### Overview

| # | Question | File | Main content |
|---|------|------|---------|
| 1 | How do I write SKILL.md? | skill-core-file-declaration.md | Naming + directory + frontmatter + Runtime Contract + distribution + lifecycle |
| 2 | How do I write step files? | advanced/skill-step-document-standard.md | Step00 preflight + naming + executor + parameter collection + round-based scheduling |
| 3 | How do I load context? | advanced/skill-context-loading-standard.md | 8 dimensions + three-level loading + dimension matrix + cognitive isolation |
| 4 | How do I write scripts? | advanced/skill-script-file-standard.md | Multiple runtimes + dependency declarations + external Runtime entities + HTTP API |
| 5 | What are the platform limits? | advanced/skill-platform-constraint-limits.md | Model matrix + 21 Hooks + Agent Teams + CC toolchain |
| 6 | How do I manage run data? | advanced/skill-runtime-data-standard.md | Runtime runs directory + keyword + preflight cache + progress.json + resume_hint |
| 7 | How do I write configuration/variables/templates? | advanced/skill-config-parameter-standard.md | Three-layer configuration + config/runtime.json + preflight + model asset declarations + variable placeholders (§13) + output templates (§14) |
| 8 | How do I write a Prompt? | advanced/skill-prompt-template-standard.md | SubAgent Prompt + custom Agent integration |
| 9 | How do I manage credentials? | advanced/skill-credential-file-standard.md | credentials/ directory + two-mode fallback + live validation (format contract points to the credentials standard) |
| 10 | How do I write the beginner guide/environment setup? | advanced/skill-docs-authoring-standard.md | guide.md beginner guide + setup.md environment setup (Step00 preflight + doctor/prepare + dependencies + models + credentials) |
| 11 | How do I debug errors? | advanced/skill-error-common-fixes.md | Seven-layer classification + preflight + Runtime assets + five-layer validation + retry |
| 12 | How do I test it? | advanced/skill-testing-process-standard.md | EDD + Step00 preflight + Runtime distribution test + cross-model test + publication checklist |
| 13 | Which design patterns exist? | advanced/skill-design-pattern-library.md | Eleven patterns (P1–P11) + anti-patterns |

---

## Core Standards (Required Reading for Every Skill)

| Standard | File | Notes |
|------|------|------|
| SKILL.md standard | `skill-core-file-declaration.md` | Naming, directory, frontmatter, Runtime Contract, multi-flow patterns, distribution, composition chain, and lifecycle |
| Step documents | `advanced/skill-step-document-standard.md` | Step00 preflight, step naming, executors, parameter collection, autonomous completion, and round-based scheduling |
| Context engineering | `advanced/skill-context-loading-standard.md` | Definitions of 8 dimensions, three-level loading, dimension selection matrix, and cognitive isolation |
| Script standard | `advanced/skill-script-file-standard.md` | Script vs SubAgent, dependency declarations, external Runtime entities, HTTP API, and fully qualified MCP names |
| Platform constraints | `advanced/skill-platform-constraint-limits.md` | Model matrix, 21 Hooks, Agent Teams, Worktree, and CC toolchain |
| Run data | `advanced/skill-runtime-data-standard.md` | Runtime runs directory, keyword, preflight cache, progress.json, and resume_hint |
| Parameter configuration | `advanced/skill-config-parameter-standard.md` | Three-layer configuration, config/runtime.json, preflight, model asset declarations, constant definitions, variable placeholders, and output templates |
| Prompt templates | `advanced/skill-prompt-template-standard.md` | SubAgent Prompt authoring and custom Agent integration |

## Supporting Standards (Read When Needed)

| Standard | File | Notes |
|------|------|------|
| Credential management | `advanced/skill-credential-file-standard.md` | credentials/ directory, two-mode fallback, and live validation (format contract points to the credentials standard) |
| docs documents | `advanced/skill-docs-authoring-standard.md` | guide.md beginner-guide template + setup.md environment setup (Step00 preflight, doctor/prepare, dependencies, model assets, and credential configuration) |
| Troubleshooting | `advanced/skill-error-common-fixes.md` | Seven-layer error classification, preflight, Runtime asset errors, five-layer validation, and retry strategy |
| Test standard | `advanced/skill-testing-process-standard.md` | EDD, Step00 preflight, Runtime distribution testing, cross-model testing, and publication checklist |
| Design patterns | `advanced/skill-design-pattern-library.md` | Eleven patterns (P1–P11) + anti-pattern list |

> Variable placeholders and HTML output templates have moved into `advanced/skill-config-parameter-standard.md` (§13, §14). The standards for docs/guide.md and docs/setup.md are combined in `advanced/skill-docs-authoring-standard.md`.

---

## Reading by Scenario

| Scenario | Required | As needed |
|------|------|------|
| One-step Skill | skill-core-file-declaration.md | - |
| Multi-step workflow | + advanced/skill-step-document-standard.md + advanced/skill-runtime-data-standard.md | advanced/skill-platform-constraint-limits.md |
| Includes scripts | + advanced/skill-script-file-standard.md | advanced/skill-config-parameter-standard.md + advanced/skill-docs-authoring-standard.md |
| Includes local models/OCR/ASR | + advanced/skill-config-parameter-standard.md + advanced/skill-docs-authoring-standard.md + advanced/skill-error-common-fixes.md | advanced/skill-testing-process-standard.md |
| Includes context loading | + advanced/skill-context-loading-standard.md | advanced/skill-prompt-template-standard.md |
| Includes HTML output | + advanced/skill-config-parameter-standard.md §14 | - |
| First publication | + advanced/skill-testing-process-standard.md | advanced/skill-docs-authoring-standard.md |

---

## Checklist

**New Skill**:
- [ ] SKILL.md exists and its frontmatter contains name + description
- [ ] description contains trigger keywords
- [ ] When scripts/credentials/Runtime assets/a multi-step workflow exist, Step00 startup preflight is defined (multi-step uses `workflow/step00-preflight.md`; one-step with dependencies uses a Step 00 section inside SKILL.md). A knowledge-only one-step Skill with no outside dependencies may omit it
- [ ] If Step00 exists: it defaults to `auto`, skips heavy checks when a valid cache hits, and verifies that dependencies are installed and required credentials truly work
- [ ] Directory structure follows the skill-core-file-declaration.md standard
- [ ] The one-step Skill has passed testing in CC

**Multi-Step Workflow Skill**:
- [ ] The `workflow/ directory exists and step files follow the naming rules in advanced/skill-step-document-standard.md`
- [ ] The first row of the workflow table is Step 00 preflight, with `-` as its output
- [ ] Every step has a clear executor (Agent/script/SubAgent)
- [ ] progress.json checkpoint continuation has been verified

**Skill with Scripts**:
- [ ] Scripts under scripts/ can run independently
- [ ] Dependencies are declared through PEP 723 / pyproject.toml / package.json
- [ ] Environment entities, model weights, and provider caches are not written inside the Skill directory
- [ ] Credentials are declared as placeholders through credentials/*.md, with no real key hardcoded

**Skill with Runtime Assets**:
- [ ] config/runtime.json declares environment, binary, model, and cache policies
- [ ] config/runtime.json declares the `preflight` policy (required/mode/cache_ttl_hours/invalidate_on)
- [ ] Required credentials have side-effect-free live validation; if they cannot be validated, stop at Step00
- [ ] docs/setup.md explains the doctor / prepare / repair commands
- [ ] A missing environment/model returns runtime_missing; a damaged model returns runtime_corrupt
- [ ] Distribution testing on a new machine has passed

**Before Publication**:
- [ ] EDD testing in advanced/skill-testing-process-standard.md is complete
- [ ] All four Step00 scenarios pass: first run, cache hit, configuration-change invalidation, and missing-credential failure
- [ ] Constraint language is consistent (✅ ⚪ ❌ ⚠️)
- [ ] Every child-standard file has a compliance statement

**Changed Skill** (required after making a change):
- [ ] The wrong step was **replaced** by the right step instead of receiving a ⚠️ warning beside it
- [ ] No old path / old command / old parameter remains as a compatibility branch
- [ ] No accumulated explanation says "X before, Y now" / "see the old-version method below" / "note: it used to..."
- [ ] Lessons from failures are folded into the step itself as one imperative line, not written as a long ⚠️ explanation
- [ ] The changed section was reread as if the Skill had been written today, and anything that "would not exist if written today" was deleted
- [ ] Passed `../awp-meta-authoring-standard/std-style-language-contract.md`: three-layer language contract / remove jargon / formalize spoken wording / faithfulness, clarity, and grace
