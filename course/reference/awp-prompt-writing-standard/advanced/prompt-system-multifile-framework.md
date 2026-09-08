---
document_id: awp-prompt-writing-standard/prompt-system-multifile-framework
language: en
publication: public
source_revision: 1
title: "Prompt Documentation System Framework (Multi-File Prompt Knowledge System)"
purpose: Defines the layers, document types, templates, sources of truth, and indexes when a prompt set is not one file but a knowledge system coordinated across methodology / persona / vocabulary / machine-readable files
category: Standard
prerequisites:
  - ../prompt-format-eight-part.md
see_also: []
---

# Prompt Documentation System Framework (Multi-File Prompt Knowledge System)

> One-line premise: **The eight-section format governs how to write one executable prompt. This framework governs how to organize a prompt knowledge system spread across several files.**
> Use it when a prompt set is not one file, but a group of files for methodology + persona + vocabulary + machine-readable index that work together to generate output at runtime. A typical case is the `shared/libraries/` system for workflows that explain PPT or animated web pages.
> Root problem: These systems easily turn into **patchwork**. The same fact is scattered across files, every file uses its own format, and each new capability is added in several unrelated places. This framework fixes that through layers + fixed templates by type + one source of truth + a knowledge map.

## 1. When to Use This Framework

Use this framework when any one condition is met. Otherwise, use the eight-section format for a single file:
- One prompt set spans **at least two types** of files, such as both methodology and persona files or both persona and vocabulary files.
- There are **several consumers**, such as several steps or workflows reading the same knowledge.
- It needs **long-term evolution**, with repeated additions of styles, capabilities, or source material.

## 2. Four Layers and Four Document Types: Layer Responsibility Equals One Source of Truth

> Strict rule: **Every fact has only one source of truth.** Every other file refers to it without restating or forking it.

| Layer | Document type | Answers | Location | Template basis |
|----|---------|------|------|---------|
| Methodology layer | **rule (methodology)** | Why + criteria, including principles, decision standards, and red lines | `shared/libraries/methods/rules/*.md` | This framework §3.1, aligned with the Agent workflow domain's own Rule template |
| Persona layer | **persona (style persona/director)** | How to generate at runtime: executable persona + output contract | `shared/libraries/styles/{axis}/*.md` | This framework §3.2, eight-section format + frontmatter |
| Material layer | **vocab (vocabulary/pattern library)** | Which composable materials and moves exist: a material menu | `shared/libraries/styles/{axis}/*-patterns.md` | This framework §3.3 |
| Machine-readable layer | **registry (machine-readable index)** | How the machine selects: a mirror of the md files + statistics | `shared/registries/*.json` | This framework §3.4 |

> Relationship: rule defines criteria → persona generates at runtime from those criteria → persona takes and combines materials from vocab → registry is a machine-readable mirror of rule/persona/vocab. **md is always the source of truth. registry is always a mirror.**

## 3. Four Templates

### 3.1 rule (Methodology) Template
```markdown
---
id: {rule-id}
type: rule
owns: [the facts and criteria for which this document is the single source of truth]
consumes: [the documents cited by this document]
status: ready | seeded
updated: YYYYMMDD
---
# {methodology name}
> Source-of-truth statement: this document is the single source of truth for {X}; see {elsewhere} for {Y}.
## 0. Core proposition   (one-sentence thesis + single value criterion)
## 1...N                 (principles / decision framework / criteria as needed)
## Relationship to other axes  (boundaries: what this document does not cover and who owns it)
## Change log
```
rule writes criteria. It does not use role-playing language or one-off examples.

### 3.2 persona (Style Persona/Director) Template
> persona = a role-based wrapper around the eight-section format. Add style frontmatter to that format.
```markdown
---
id: {persona-id}
type: persona
name: {display name}
scene: {applicable scenario}
tone: {tone}
# Parameters specific to this axis, such as narration pace_cps/page_seconds or motion tier/signature
owns: [style facts defined by this persona]
consumes: [the rules and vocabulary it cites]
status: ready
updated: YYYYMMDD
---
# {persona name} (role)
(Use the eight-section format: role and boundaries → rules/workflow → material-selection rules [cite vocab] → output contract [fields and self-check].)
## Output contract    (JSON field skeleton + downstream reconciliation anchors such as beat_id)
## Change log
```
persona is a persona that generates at runtime. It must be executable and include an output contract. Do not restate rule; refer to it.

### 3.3 vocab (Vocabulary/Pattern Library) Template
```markdown
---
id: {vocab-id}
type: vocab
owns: [material and technique entries cataloged in this library]
consumes: [the rules it follows]
status: ready
updated: YYYYMMDD
---
# {vocabulary library name}
> ⚠️ This is a **material library, not a fixed menu**: the persona combines and exceeds materials in context; do not apply an N-of-1 choice mechanically.
## {category}
Every entry uses the same fields: **name — use case  -  recipe (how to use it)  -  backend (implementation)  -  discipline (constraint)**
## Change log
```
vocab is a set of composable materials. Add new items only as entries. Do not change the existing structure.

### 3.4 registry (Machine-Readable Index) Template
```json
{
  "schema_version": "x.y",
  "purpose": "... (declare which Markdown files this machine-readable mirror represents)",
  "truth_source": "shared/libraries/... (points to the source-of-truth Markdown file)",
  "by_*": { "metric": "count" },
  "entries": [ { "id": "...", "...": "...", "ref_md": "corresponding Markdown path" } ]
}
```
registry **only mirrors**. Its field values match the source md. When adding a capability, **change the md first, then sync registry**. Never change only registry.

## 4. Shared Frontmatter Required for Every Knowledge Document

| Field | Meaning |
|------|------|
| `id` | Stable ID in kebab case |
| `type` | `rule` / `persona` / `vocab` / `registry` |
| `owns` | The facts for which this document is the **only source of truth**, which is key to removing duplication |
| `consumes` | The documents this file refers to |
| `truth_source` | If this document is a mirror or derivative, points to the source of truth |
| `status` | `ready` / `seeded` |
| `updated` | Date of the latest update |

## 4.5 File Naming → Governed by the Agent Workflow Naming Standard

This framework governs document **types and internal structure**. These documents live under `workflows/`, so their **file naming belongs to the Agent workflow domain** and follows that domain's own naming convention: use `{type}-{axis}-{slug}.{ext}`, with the strict rule **filename stem = frontmatter `id` = registry entry id**. This framework requires only that the frontmatter `id` match the filename stem. It does not repeat the naming format here.

## 4.6 Language Contract: One Form Per Concept ⭐

> `../../awp-meta-authoring-standard/std-style-language-contract.md` is the source of truth for general writing style. This section applies it to prompt knowledge systems. It is consistent with that standard and does not conflict with it.

> Root problem: prose that mixes bare identifiers into sentences looks messy. The real cause is usually **synonym drift**, where one concept has three names, not the identifiers themselves. Stripping every identifier out of prose breaks code references and makes things worse. The fix is **layers + one form for each concept**.

**Three-layer language contract**:
1. **Identifier layer = never rewritten**: filenames / `id` / field names / enum values / code symbols / APIs / library names / CSS tokens. Keep them exactly as the code spells them, in backticks.
2. **Concept layer = one fixed term, with the identifier in parentheses at first use**: In domain prose, use **only one term** for each concept. At its **first appearance**, add `Term (identifier)` once to help readers align it. After that, use only the term.
3. **Prose layer = natural sentences**: Write judgments, standards, and explanations as full sentences. Do not string identifiers together in place of prose.

**One concept, one form**: Every knowledge system must have a **glossary** at `shared/libraries/GLOSSARY.md` that locks each concept's "prose term + identifier + first-use annotation + deprecated variants." Check the glossary before writing. It is the source of truth for terminology in that system. To add or change a form, update the glossary before the documents.

**Headings**: An identifier annotation such as `## Term (identifier)` is **allowed** to help readers align a concept with its code name. Within one document, either use it consistently or use it only in H1. Do not add it at random.

**Remove jargon**: Prompts are written for readers, whether people or agents, not as notes for the author.
- **A code is not a prose term**: Keep field enum values such as `gen` / `shot` / `lib` in code positions, with backticks or as field values. Use a descriptive phrase in prose, such as "generated assets," not "gen assets."
- **Expand abbreviations**: Expand private abbreviations, such as "image generation" and "assign a director," instead of using internal shorthand.
- **Define a metaphor before using it**: For metaphors such as director / palette / parts, explain the real object in one sentence at first use, and do not put the metaphor in a heading.

**Faithfulness, clarity, and grace; no translation-like writing**:
- **Faithfulness**: Do not drop any instruction or change its meaning. Keep every measured target.
- **Clarity**: One meaning per sentence, with no ambiguity.
- **Grace**: Write natural, fluent, complete sentences. **Do not pile up ` - ` / `/` tag clouds**. Keep tag clouds only where the content is already an enumeration, such as style-property enums. Do not pile up nouns.
- Bad: `Step 3  -  Write image prompts for gen assets (aimed at the "parts", not the picture)`; good: `Step 3  -  Write image prompts for generated assets (\`source=gen\`)`.

> Applies to all prose in this system: the body of `rule` / `persona` / `vocab` / `prompt`, plus `step` / `WORKFLOW` / `docs`. A `type=prompt` document must also use the eight-section format.

## 5. INDEX Knowledge Map Required for Every System

`shared/libraries/INDEX.md`: One table maps **document → type / owns / consumes**, making the whole system visible and auditable at a glance. Adding or deleting a document requires an INDEX update.

## 6. Relationship to Existing Standards Without Repetition

- **Eight-section format (`../prompt-format-eight-part.md`)**: Governs **one executable prompt**. persona and workflow prompts use it.
- **The Agent workflow domain's own multi-file framework**: Governs the **structure** of WORKFLOW/ROUTER/step/manifest/Rule/Prompt/Check/Schema. The rule type in this framework follows its Rule template.
- **This framework**: Governs the layers, types, sources of truth, and index for a **multi-file prompt knowledge system**. It is the higher-level organizational rule above the first two.

## 7. Anti-Patterns: Any Match Is a Violation

- ❌ **Patchwork**: Adding a capability in several scattered places without changing the source of truth → use the one source of truth in §2 + `owns` in §4 to find the one location.
- ❌ **Several truth sources / drift**: The same fact differs between md and registry → registry only mirrors it (§3.4).
- ❌ **persona restates rule/vocab**: A persona copies criteria or entries → always refer to them.
- ❌ **vocab treated as a fixed menu**: Pick one of N and apply it unchanged → it is a material library for combinations and new work.
- ❌ **Every file uses its own format**: Does not follow the §3 template / lacks frontmatter → block it with a linter.
- ❌ **Synonym drift**: The same prose concept has several forms, such as direction/persona/director → violates §4.6. Converge on `GLOSSARY.md`.

## 8. Checklist

- [ ] Every knowledge document has shared frontmatter: `type`/`owns`/`consumes`.
- [ ] Every fact has one source of truth. registry marks `truth_source` and matches the md.
- [ ] rule/persona/vocab/registry each uses its matching template.
- [ ] The system has an `INDEX.md` that matches the actual files.
- [ ] The system has a `GLOSSARY.md`. Its prose follows the three-layer language contract in §4.6, and concept forms match the glossary.
- [ ] Prose removes jargon and follows faithfulness, clarity, and grace: codes are not prose terms, abbreviations are expanded, metaphors are defined first, and natural full sentences replace tag clouds (§4.6).
- [ ] A `type=prompt` document uses the eight-section format.
- [ ] Adding a capability: change the source md + sync registry + update INDEX + update changelog, with no patchwork.

## Change Log

> Rolling window: keep the 10 most recent entries, each no more than 20 words.

| Date | Change |
|------|---------|
| 2026-05-26 | Added jargon removal + writing-quality line to §4.6 |
| 2026-05-26 | Added §4.6 language contract + glossary mechanism |
| 2026-05-26 | Moved file naming to workflow standard |
| 2026-05-26 | First version of the multi-file prompt framework |
