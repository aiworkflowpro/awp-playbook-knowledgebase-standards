---
document_id: awp-skill-development-standard/advanced/skill-context-loading-standard
language: en
publication: public
source_revision: 1
title: "Context Engineering Standard"
purpose: Shared architecture for Skill context loading: direct reading first, KB CLI search, SubAgent isolation, and execution-directive output
category: Standard
prerequisites:
  - ../skill-core-file-declaration.md
  - skill-step-document-standard.md
see_also:
  - skill-config-parameter-standard.md
  - ../../awp-prompt-writing-standard/prompt-format-eight-part.md
---

# Context Engineering Standard

> This standard governs context-loading steps in Skills. AWP context comes from the local file system and `awp-kb` (KB CLI — `KB` in this document refers to this CLI tool, not an abbreviation for "knowledge base").
> Every Skill that needs AWP context must use a separate integer-numbered step for context loading. It produces `step{N}-context/context.md` and any needed dimension files.
> The output must contain execution directives. It must not be a pile of source material, a research report, or a full-text copy.

## Structure

| # | Question | Section | Main Content |
|---|------|------|---------|
| §1 | What are the basic context-loading constraints? | Core Principles | Separate step, direct reading first, cognitive isolation, execution-directive output |
| §2 | Which dimensions can KB provide? | Context Dimensions | Identity, style, standards, references, published work, experience, business facts, credential pointers |
| §3 | How is context obtained? | Retrieval Modes | Direct reading, KB CLI search, fixed-path scanning, mixed loading |
| §4 | Which dimensions fit each Skill? | Dimension Matrix | Choose the minimum context needed by Skill type |
| §5 | How should output files be written? | Output Standard | Formats for context.md and dimension files |
| §6 | How are failures handled? | Error Handling | Stop on a required failure; mark optional failures SKIP |
| §7 | How is a SubAgent prompt written? | Prompt Template | Isolated extraction into execution directives |
| §9 | Which Skills need a context step? | Scope | Content production, publishing, migration, audits, and more |

## 1. Core Principles

### 1.1 Separate Step

Context loading must be a separate integer-numbered step, such as `Step 03 Context`. Do not mix context loading into the main creation step.

The separate step must produce at least:

- `step{N}-context/context.md`
- `step{N}-context/sources.json`
- When needed, dimension files such as `identity.md`, `style.md`, `reference.md`, and `published.md`

### 1.2 Direct Reading First

Read deterministic rules from their original files. Do not ask a SubAgent to extract them.

Content suited for direct reading:

- `brand/{brand}/positioning.md`
- `brand/{brand}/expression-style.md`
- Hard rules under `standards/`
- The ten-file DNA in a workflow style library
- `CLAUDE.md` in the current folder and its parent folders

Reason: Rule files are usually a manageable size. Extraction loses tables, fields, red lines, and reading order.

### 1.3 Cognitive Isolation

Let a SubAgent read and select from large source sets, published work, hit examples, and industry data. The main Agent consumes only the extracted execution directives.

Content suited for a SubAgent:

- Duplicate checks against published work
- Extraction from hit examples
- Cross-comparison of industry data
- Summary of historical experience across several files
- Pattern extraction from similar projects

### 1.4 Output Is an Execution Directive

The context step does not output "source excerpts." It tells the main Agent how to handle the current task.

| Case | Wrong Output | Correct Output |
|------|----------|----------|
| Long-form writing | Copy summaries of 10 reference articles | Use a "problem-demo-delivery" structure and avoid exaggerated promises in the title |
| Terminology | List several possible forms of a term | Use "agent" throughout and explain it on first use |
| Pre-publish review | Repeat platform rules | Delete the price promise in paragraph 3 and add `canonical_url` to frontmatter |

## 2. Context Dimensions

| Dimension ID | Content | Source | Retrieval Method |
|---------|------|------|----------|
| `identity` | Position, red lines, expression boundaries | `brand/{brand}/` | Direct read |
| `style` | Platform style, ten-file DNA | Matching workflow style library | Direct-read path list |
| `rules` | Markdown, SEO, platform, and publishing standards | `standards/` | Direct read |
| `business` | Products, courses, prices, links, and operating facts | `business/` | Direct read + `rg` |
| `reference` | Frameworks, data, hits, cases, terms, and citations | `{reference_root}` or `research/` | KB CLI search + selected full-file reads |
| `published` | Published work, historical titles, and internal links | `brand/ + publishing workflows (`workflows/`) content outputs under `dashboard/output/` and similar locations | KB CLI search + `rg` |
| `experience` | Practices, lessons, and migration records | `tools/best-practices/`, workflow `lessons.md` | KB CLI search + direct read |
| `credentials` | Credential-file location and retrieval method | your knowledge base's credentials index | Pointer only; never output keys |

`{reference_root}` defaults to `commerce/3-build-and-sell/reference-assets`. Do not create the old `brand/{brand}/reference/`.

## 3. Retrieval Modes

### 3.1 Direct-Read Mode

Use this when the path is known, the content is short, and the rules are strict.

Steps:

1. Confirm routing from `CLAUDE.md` in the current folder and its parents.
2. Write `paths.json` with the files that must be read.
3. The execution step reads and follows the original text directly.

### 3.2 KB CLI Search Mode

Use this when target files are not predictable and material must be recalled by topic.

Standard commands:

```bash
```

> See §8.1 for the `KB` alias.

Rules:

- Start with short keywords. Do not put a long question into one search.
- When the folder can be limited, combine path terms with `rg` for a second filter.
- For each dimension, choose at most 3-8 highly relevant files and read them in full.
- When results are weak, change the terms and search again. Do not invent missing facts.

### 3.3 Exact `rg` / `find` Mode

Use this to check paths, titles, fields, old links, and old terms.

Common commands:

```bash
rg -n "keyword" {kb_root}/target-folder --glob '*.md' --glob '!**/.stversions/**'
find {kb_root}/target-folder -name CLAUDE.md -print
```

> `{kb_root}` is the global variable for the knowledge-base root, defined in the global `CLAUDE.md`. Do not hard-code a machine-specific absolute path inside a Skill.

### 3.4 Mixed Mode

Read the framework first, then search for material.

Typical examples:

- Read `CLAUDE.md` and the ten-file DNA in a workflow style library, then search published work.
- Read your knowledge base's reference-material standard, then search `{reference_root}/viral-posts/`.
- Read the product definition under `commerce/3-build-and-sell/ + `business/industries/{trade}/``, then search historical website articles.

## 4. Dimension Selection Matrix

| Skill Type | Required Dimensions | Optional Dimensions |
|------------|----------|----------|
| Long-form creation | `identity`, `style`, `rules`, `reference` | `published`, `business` |
| Short-form/social | `identity`, `style`, `rules` | `reference`, `published` |
| Translation/English adaptation | `identity`, `style`, `rules`, `business` | `reference` |
| Platform publishing | `rules`, `business`, `credentials` | `identity` |
| Final-draft review | `rules`, `style` | `published` |
| Tool development | `rules`, `experience` | `credentials` pointer |
| Knowledge-base maintenance | `rules`, `experience` | `reference` |

Principle: Load only the dimensions needed for this task. Do not load the entire AWP knowledge base because something "might be useful."

## 5. Output Standard

### 5.1 `context.md`

It must contain:

```markdown
# Context

## Load Status

| Dimension | Status | Source Count | Notes |
|-----------|--------|--------------|-------|
| identity | OK | 2 | Position + expression style |
| style | OK | 10 | Ten-file DNA |
| reference | SKIP | 0 | Not needed for this task |

## Execution Directives

1. ...

## Source Files

- `{kb_root}/...`
```

### 5.2 Dimension Files

Name dimension files `{dimension}-{label}.md`.

Requirements:

- Keep each file near 2,500 tokens or less.
- Write only judgments needed for the current task.
- Every key conclusion must trace back to `Source Files`.
- Do not output keys, accounts, tokens, or private source text.

## 6. Error Handling

| Condition | Handling |
|------|------|
| Required identity file is missing | FAIL. Stop the task and require the brand identity first |
| Required standard is missing | FAIL. Stop the task and repair the standard entry point first |
| Optional references are weak | SKIP. Record changed terms and search results |
| Search returns too much | Filter again by `rg`, folder, and filename |
| Paths conflict | Follow the nearest `CLAUDE.md` and the global variables in the root `CLAUDE.md` |
| Credentials must be read | Read only credential instructions and retrieval methods. Do not write keys into outputs |

## 7. SubAgent Prompt Template

```markdown
You are responsible for extracting `{dimension}` context for the main Agent.

Task:
- Read only the files or search results listed below.
- Output execution directives, not a research report.
- Every recommendation must serve `{task}`.
- Do not invent missing facts.

Input:
- task: {task}
- brand: {brand}
- platform: {platform}
- source_files:
  - {file1}
  - {file2}

Output:
1. The 5-12 directives this task must follow
2. Red lines: what must not be written or done
3. A list of material that may be cited
4. Source Files
```

## 8. KB CLI Tool Standard

### 8.1 Tool Entry Point

`KB` is the alias for the knowledge-base CLI. It is an installed entry point on `PATH`, so there is no interpreter and no root variable to substitute — the CLI finds the vault itself through the nearest `.awp-vault.toml`:

```bash
KB = awp-kb
```

Common commands:

```bash
```

### 8.2 Search Strategy

| Case | Recommended Method |
|------|----------|
| Find an old path | `rg -n "old-path" {kb_root} --glob '*.md'` |
| Find CLAUDE | `find ... -name CLAUDE.md` |

### 8.3 Forbidden Actions

- Do not hard-code large blocks of AWP knowledge-base content inside a Skill.
- Do not skip the local CLI and guess the folder structure.
- Do not write historical archive paths as active routes.

## 9. Scope

| Class | Requirement | Typical Cases |
|------|------|----------|
| Context step required | Creation from scratch, deep rewriting, complex migration | Long-form work, courses, English adaptation, competitor improvement |
| Context step recommended | Pre-publish review, cross-platform adaptation | Ghost, Substack, X, YouTube |
| Light context step | Tool documentation, README, simple script notes | Tool publishing, configuration sync |
| Not needed | Pure system operation, small single-file fix | Change a path, run tests, inspect logs |

## Checklist

**Context step**

- [ ] It is a separate integer-numbered step.
- [ ] `CLAUDE.md` in the current folder and parent folders has been read.
- [ ] Required dimensions are complete, and failure stops the task.
- [ ] Deterministic rules use direct reading, without a second summary.
- [ ] A SubAgent extracts large source sets.

**context.md**

- [ ] It has Load Status.
- [ ] It has Execution Directives.
- [ ] It has Source Files.
- [ ] Total size stays near 10K tokens or less.
- [ ] It contains no keys, accounts, or tokens.

**Retrieval**

- [ ] Use KB CLI first, then `rg` / `find` for exact checks.
- [ ] Search terms are short and specific.
- [ ] Read only highly relevant files in full.
- [ ] Old paths, historical archives, and cold backups are clearly marked.
- [ ] Passed `../../awp-meta-authoring-standard/std-style-language-contract.md`: three-layer language contract / remove jargon / formalize speech / accurate, clear, and natural.
