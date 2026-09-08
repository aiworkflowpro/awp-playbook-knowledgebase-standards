---
document_id: awp-skill-development-standard/advanced/skill-docs-authoring-standard
language: en
publication: public
source_revision: 2
title: "docs Documentation Standard (guide.md + setup.md)"
purpose: Full standard for the two files docs/guide.md (Beginner Guide) and docs/setup.md (Environment Setup)
category: Standard
prerequisites:
  - ../skill-core-file-declaration.md
see_also:
  - skill-credential-file-standard.md
  - skill-testing-process-standard.md
---

# docs Documentation Standard (guide.md + setup.md)

> This document defines the two user-facing files under a Skill's `docs/` directory. Each Skill may have at most one of each:
> - `docs/guide.md` (Beginner Guide): teaches end users how to use the Skill—what it does, how to call it, and its inputs and outputs (see "Part One: guide.md").
> - `docs/setup.md` (Environment Setup): teaches technical users how to fix the environment—Step00 preflight, Runtime diagnosis, dependencies, model assets, credentials, and troubleshooting (see "Part Two: setup.md").
>
> The dividing line: guide.md teaches use (how to call it and where output goes); setup.md fixes problems (error handling and environment setup).

---

## Part One: guide.md (Beginner Guide) Standard

### 0. Design Philosophy

| Principle | Description |
|------|------|
| ✅ **For end users** | Assume readers know nothing about the Skill's internals and care only about how to use it |
| ✅ **Start in five minutes** | From trigger phrase to visible output in no more than five minutes |

### Structure

| # | Question | Section | Core Content |
|---|------|------|---------|
| §1 | What does guide.md cover? | File Role | Position, target reader, and distinction from setup.md |
| §2 | When is it needed? | Need Test | Workflow with ≥3 steps / >300 lines / nontechnical users |
| §3 | What is the standard structure? | Standard Template | Full guide.md template |
| §4 | How should each section be written? | Section Rules | Feature description / call methods / inputs and outputs / usage flow |
| §5 | What should and should not be done? | Rule List | Positive list + negative list |
| §6 | What are the fixed writing rules? | Writing Rules | Consistent terms and runnable examples |

### 1. File Role

guide.md is a getting-started document for beginners. Its target reader is an ordinary user seeing the Skill for the first time. They may not know the technical details. They only want to know, "How do I use this?"

Its core content has three parts: what the Skill can do, how to call it, and where its inputs and outputs are.

The distinction from setup.md: guide.md teaches use (how to call it and where output goes); setup.md fixes problems (error handling and environment setup).

### 2. Need Test

| Condition | Write guide.md? |
|------|------------------|
| One-step Skill | ❌ No (SKILL.md is enough) |
| Workflow with ≥3 steps | ⚪ Optional |
| SKILL.md >300 lines | ⚪ Optional |
| Complex parameters | ⚪ Optional |
| For nontechnical users | ✅ **Required** |
| Interactive flow | ⚪ Optional |

Quick test: if SKILL.md is >300 lines, write it; otherwise, if the workflow has ≥3 steps, write it; otherwise, if the target users are nontechnical, write it; if none apply, it is not needed.

### 3. Standard Template

```markdown
# Beginner Guide

---

## What Does This Skill Do?

{Describe its function and value in 2–3 sentences}

**For example**: {Specific use case}

---

## How Do I Call It?

**Slash command** (✅ preferred):

/skill-name

**Natural language**:

Say "trigger phrase" to start

**With parameters**:

/skill-name --url https://example.com

| Parameter | Required | Description |
|------|------|------|
| `--url` | Yes | Target link |

---

## Inputs and Outputs

| Type | Path | Format | Description |
|------|------|------|------|
| Input | `{run_dir}/step01-collect/` | Markdown | Data to process |
| Output | `{run_dir}/output/` | JSON | Processing results |

---

## Usage Flow

1. Prepare the input file
2. Run `/skill-name`
3. View the results in `{run_dir}/output/`

---

## Next Step

- Full documentation: `SKILL.md`
- If you see an error: `docs/setup.md`
```

### 4. Section Rules

**What Does This Skill Do?**: Explain the Skill's function and value in 2–3 sentences. Write from the user's point of view. Focus on "what you get," not the technical implementation. It must include "For example" and use one concrete case to help the user understand.

**How Do I Call It?**: This is the most important section in guide.md. It must explain three call methods. The slash command is preferred, in the form `/skill-name`; the natural-language method tells users what to say to trigger it; a call with parameters must include a parameter table with purpose, whether each parameter is required, and an example value.

**Inputs and Outputs**: Tell users what they need to prepare and what they will get. The table must have four columns: type (input/output), path, format, and description. The format column tells users whether a file is Markdown, JSON, or another format.

**Usage Flow**: Give beginners clear numbered steps. Each step does one thing. Cover all three stages: prepare → run → view.

### 5. Rule List

**Do**: Write in language a beginner can understand, give concrete examples, state the `/skill` call command, write the usage flow as steps, and point error questions to setup.md.

**Do not**: Do not assume users understand technology, write only abstract descriptions, omit call methods, reduce the flow to one paragraph, or put environment setup in guide.md.

### 6. Writing Rules

**Consistent terms**: Use "Skill" throughout. Do not mix it with "capability" or "plugin." Use "call" throughout. Do not mix it with "execute" or "run."

**Runnable examples**: Put commands in code blocks so users can copy them directly. Give real example values for parameters, not only placeholders.

### guide.md Checklist

- [ ] Includes a "What Does This Skill Do?" section (2–3 sentences + an example)
- [ ] Includes a "How Do I Call It?" section (slash command + natural language + parameters)
- [ ] Includes an "Inputs and Outputs" table (type, path, format, and description)
- [ ] Includes a "Usage Flow" (numbered steps)
- [ ] Written for end users (not from a developer's point of view)
- [ ] Uses consistent terms throughout (Skill/call)

---

## Part Two: setup.md (Environment Setup) Standard

### 0. Design Philosophy

| Principle | Description |
|------|------|
| ✅ **Idempotent execution** | Running the setup script again causes no side effects |
| ✅ **Environment isolation** | Environment entities live under `~/.awp/runtime/` and do not pollute the system Python |
| ✅ **Declaration first** | Read `config/runtime.json` before running doctor / prepare |
| ✅ **Auditable models** | Every model download must have a source, license, checksum, and explicit command |
| ✅ **Explainable preflight** | setup.md must say which doctor/prepare/credential command to run when Step00 fails |

### Structure

| # | Question | Section | Core Content |
|---|------|------|---------|
| §1 | What does setup.md cover? | File Role | Position, target reader, and distinction from guide.md |
| §2 | When is it needed? | Need Test | Write it only for dependencies / credentials / special environments |
| §3 | What is the standard structure? | Standard Template | Full setup.md template |
| §4 | How should each section be written? | Section Rules | Step00 / Runtime / runtime / models / credentials / troubleshooting tables |
| §5 | What are the fixed writing rules? | Writing Principles | Problem-led, one-step fixes, and runtime-neutral |
| §6 | What is forbidden? | Prohibited Items | No tutorials, pip, or hard-coded values |
| §7 | How should install locations be written? | Install Location Rules | Three install levels + priority |
| §8 | How should the diagnostic flow be written? | Diagnostic Flow | Quick checks + layered diagnosis + recovery check |
| §9 | How are multiple runtimes configured? | Multi-Runtime Templates | Python / Node.js / Bash / mixed |
| §10 | How are model assets prepared? | Runtime Asset Template | Model preparation for OCR / ASR / vision / embedding and more |

### 1. File Role

**Position**: Environment repair for technical users. **Target reader**: A user facing a technical error.

**Core content**: Step00 preflight + Runtime diagnosis + runtime configuration + dependency installation + model asset preparation + credential configuration + troubleshooting.

**Distinction from guide.md**: setup.md = fix the machine (ModuleNotFoundError, API 401); guide.md = teach use (how to trigger it and where output goes).

### 2. Need Test

| Condition | Need setup.md? |
|------|-------------------|
| No external dependencies | No |
| Script dependencies | Yes |
| API credentials required | Yes |
| Special environment requirements | Yes |
| Local model assets required | Yes |
| System binaries required | Yes |

### 3. Standard Template

````markdown
# Environment Setup

Use this document to configure the environment when you see an error. See `guide.md` for usage instructions.

---

## Install Location

<!-- Fill this in for the actual case -->

| Level | Path | Priority |
|------|------|--------|
| Enterprise | See skill-platform-constraint-limits.md | Highest |
| Project | `.claude/skills/<skill-name>/` | Medium |
| User | `~/.claude/skills/<skill-name>/` | Lowest |

> **Priority rule**: Enterprise > Project > User (a higher priority overrides a lower priority)

---

## Runtime Environment

| Requirement | Value |
|------|-----|
| Runtime | <!-- Python / Node.js / Bash --> |
| Version | >= <!-- X.Y --> |
| Package manager | <!-- uv / pnpm / npm --> |
| Environment entity | `~/.awp/runtime/envs/skills/<skill-name>/` |

---

## Runtime Diagnosis and Preparation

By default, Step00 preflight runs a light check at every start. Use strict mode on the first run, on a new machine, after a config change, and before release. strict must confirm that dependencies are installed, required binaries can run, and required credentials pass side-effect-free live validation.

```bash
# Full preflight; do not call paid APIs or create an environment
python scripts/doctor.py --deep

# Diagnose only; do not create an environment or download models
python scripts/doctor.py

# Prepare the environment and model assets
python scripts/prepare.py --profile standard
```

---

## Dependency Installation

<!-- Fill in installation commands for the runtime -->

---

## Credential Configuration

| Service | File | Required | How to Get It | Format Example |
|------|------|------|---------|---------|
| <!-- Service name --> | `tools/credentials/xxx.md` | <!-- Yes/No --> | <!-- URL --> | <!-- Field name --> |

---

## Model Assets

| Role | Type | Model | Required | Preparation Method |
|------|------|------|:---:|----------|
| <!-- asr-default --> | <!-- asr --> | <!-- whisper-cpp/ggml-base --> | Yes | `python scripts/prepare.py --profile standard` |

---

## Troubleshooting

| Class | Symptom | Possible Cause | Fix | Validation |
|------|---------|---------|---------|---------|
| Runtime asset layer | <!-- Error message --> | <!-- Cause --> | <!-- Command --> | <!-- Validation command --> |
````

### 4. Section Rules

**4.1 Runtime configuration table rules**: The runtime environment table in setup.md must contain these fields—runtime (Python / Node.js / Deno / Bash and more, required), version (minimum version in the form `>= X.Y`, required), package manager (uv / pnpm / npm / cargo and more, required), and environment entity (`~/.awp/runtime/envs/skills/{skill-name}/...`, required).

**4.2 Dependency installation command rules**: The dependency installation section must include Runtime diagnosis (`python scripts/doctor.py`, required), Runtime preparation (`python scripts/prepare.py --profile standard`, required), dependency sync (only as a development and debugging command when Runtime is unavailable, optional), and a run command (standard run-command template, required). Format requirement: use code blocks and give every command a comment.

**4.3 Credential configuration table rules (Markdown-only)**: Fields include service (API service identifier), file (relative path under `tools/credentials/*.md`), required (yes/no), how to get it (official link or short explanation), and format example (Markdown table field name, such as `API Key` / `Base URL`, optional).

**4.4 Model asset table rules**: When `config/runtime.json` contains `models`, setup.md must contain a model asset table. Its fields include role (`asr-default` / `ocr-default` and more), type (`asr` / `ocr` / `vision` / `embedding` and more), model (`models[].id`), required (whether the model is required), and preparation method (Runtime prepare command or manual import instructions).

**4.5 Troubleshooting table rules**: **The focus is the format, not a list of specific errors.** Every error entry must include class, symptom, possible cause, and fix (required), plus validation (optional). Error classes are defined below:

| Class | Use Case |
|------|----------|
| Runtime asset layer | Local model, Runtime env, or provider cache is missing or damaged |
| Dependency layer | Package/module missing or version conflict |
| Credential layer | API authentication failure or wrong key format |
| Network layer | Connection timeout, request rate limit, or proxy issue |
| Runtime layer | Incompatible version or missing environment variable |
| Progress layer | Lost state or failed resume_hint |
| Path layer | File not found or insufficient permissions |

### 5. Writing Principles

| Principle | Description |
|------|------|
| Problem-led | Organize by error class, not by steps |
| Precise identification | Give the exact error message so users can search for it |
| One-step fix | Give the shortest fix command for each problem |
| Link outside resources | Provide links for outside resources such as credential access |
| Runtime-neutral | Support several runtimes instead of binding to one language |
| External entities | Manage environment and model entities only through Runtime |

### 6. Prohibited Items

| Prohibited | Reason |
|------|------|
| ❌ Write a usage tutorial | Put it in guide.md |
| ❌ Write workflow logic | Put it in SKILL.md |
| ❌ Use pip/poetry ([AWP only]) | Use uv for all Python work |
| ❌ Hard-code paths | Use relative paths or variables |
| ❌ List unrelated errors | Include only errors the Skill may actually encounter |
| ❌ Ask users to copy `.venv`/`node_modules`/model weights | Runtime must materialize entities |
| ❌ Let a script silently download a missing model | Use Runtime prepare so the action is auditable and recoverable |

### 7. Install Location Rules

setup.md should contain an install location table. It explains the install levels supported by the Skill in three columns: level, path, and priority. Standard levels, matching skill-platform-constraint-limits.md:

| Level | Typical Path | Description |
|------|----------|------|
| Enterprise | See Section 1.3 of skill-platform-constraint-limits.md | Deployed by an organization administrator; highest priority |
| Project | `.claude/skills/<skill-name>/` | Shared within a project; medium priority |
| User | `~/.claude/skills/<skill-name>/` | Personal configuration; lowest priority |

> **Priority rule**: `Enterprise > Project > User`. A same-named Skill at a higher priority overrides one at a lower priority.

### 8. Diagnostic Flow

setup.md may contain a diagnostic decision flow to help users find a problem quickly.

**8.1 Quick-check list template**:

````markdown
## Quick Checks

Run these in order. The first failed item is where the problem lies:

- [ ] Runtime version check: `xxx --version`
- [ ] Dependency integrity check: `xxx check`
- [ ] Credential validity check: `xxx validate`
- [ ] Network connectivity check: `curl -I https://api.xxx.com`
````

**8.2 Layered diagnostic order** (from lower to higher): Runtime asset layer (environment entity, local model, provider cache) → runtime layer (version, environment variables) → dependency layer (package installation, version conflicts) → credential layer (valid key, correct format) → network layer (connectivity, proxy, rate limits) → progress layer (state file, resume_hint).

**8.3 Recovery validation template**: Give a validation command and its expected output to confirm that the problem is fixed.

### 9. Multi-Runtime Configuration Templates

Choose the matching template for the runtimes used by the Skill.

**9.1 Python Template**:

````markdown
## Runtime Environment

| Requirement | Value |
|------|-----|
| Runtime | Python |
| Version | >= 3.9 |
| Package manager | uv |
| Environment entity | `~/.awp/runtime/envs/skills/<skill-name>/...` |
| Script directory | scripts/python/ |

## Runtime Diagnosis and Preparation

python scripts/doctor.py
python scripts/prepare.py --profile standard

## Development and Debugging Commands

uv run --project scripts/python python xxx.py
````

**9.2 Node.js Template**: Runtime: Node.js (version >= 18, package manager: pnpm, no virtual environment); use `python scripts/prepare.py --profile standard` for dependency installation, and `pnpm exec node scripts/xxx.js` as the development and debugging command.

**9.3 Bash Template**: Runtime: Bash (version >= 4.0, dependencies: curl and jq); check dependencies with `command -v curl` / `command -v jq`; use `brew install curl jq` on macOS and `apt-get install curl jq` on Ubuntu; run the script with `bash scripts/xxx.sh`.

**9.4 Mixed Multi-Runtime Template**: When a Skill uses several runtimes, declare them in a four-column component-runtime-version-package manager table (for example, core script: Python >= 3.9, uv; frontend build: Node.js >= 18, pnpm; helper tool: Bash >= 4.0). Diagnose Runtime first, then prepare Runtime dependencies.

### 10. Runtime Asset Templates

**10.1 Model Asset Template**:

````markdown
## Model Assets

This Skill declares its model assets in `config/runtime.json`. The entities live under `~/.awp/runtime/models/` and do not sync with the Skill directory.

| Role | Type | Model | Required | Size | Preparation Method |
|------|------|------|:---:|------|----------|
| asr-default | asr | whisper-cpp/ggml-base | Yes | 142 MB | `python scripts/prepare.py --profile standard` |

## Manual Import (When the License Restricts Downloading)

If the model license does not allow automatic downloads:

1. Download the file from the model's official page
2. Put it in a temporary local import directory
3. Run:

```bash
python scripts/prepare.py import-model --id whisper-cpp/ggml-base --file /path/to/model.bin
python scripts/doctor.py
```
````

**10.2 Runtime Failure Handling Template**:

| Error | Meaning | Fix |
|------|------|------|
| `runtime_missing` | The environment or model has not been prepared | `python scripts/prepare.py --profile standard` |
| `runtime_corrupt` | Model checksum or environment validation failed | `python scripts/prepare.py --repair --profile standard` |
| `dependency` | A system binary is missing | Install the binary named by doctor, then run doctor again |

### setup.md Checklist

**setup.md file**:
- [ ] Contains a runtime environment table (runtime, version, package manager)
- [ ] Contains the Step00 preflight explanation and strict diagnostic command
- [ ] Explains live validation for required credentials
- [ ] Contains Runtime doctor / prepare commands
- [ ] Dependency preparation commands can be copied and run directly
- [ ] The setup script can run repeatedly (idempotent)
- [ ] The Python environment is managed with uv/Runtime
- [ ] Dependencies are declared in PEP 723 or pyproject.toml; requirements.txt is forbidden
- [ ] States clearly that `.venv`, `node_modules`, and model weights do not belong in the Skill directory

**Credentials section**:
- [ ] The credential configuration table includes service, file, required, and how to get it
- [ ] The credential format is Markdown-only

**Model assets section**:
- [ ] If `config/runtime.json` contains models, setup.md includes a model asset table
- [ ] Models are prepared through Runtime prepare or manual import
- [ ] license, checksum, and source are traceable in runtime.json

**Troubleshooting section**:
- [ ] Errors use seven layers (Runtime assets / runtime / dependencies / credentials / network / paths / progress)
- [ ] Every error includes a symptom, cause, and fix
- [ ] Passes `../../awp-meta-authoring-standard/std-style-language-contract.md`: three-layer language contract / remove jargon / formalize speech / fidelity, clarity, and grace

> ⚠️ Format deviation: `⚠️ Deviation: {rule} | Reason: {reason}`—valid only for the current deliverable and does not set a precedent.
