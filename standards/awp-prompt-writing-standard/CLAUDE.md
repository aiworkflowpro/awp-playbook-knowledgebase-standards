---
document_id: awp-prompt-writing-standard/package-index
language: en
publication: public
source_revision: 2
title: "AWP Prompt Standard"
prerequisites: []
see_also: []
---

# AWP Prompt Standard

> Standards, templates, and examples for all prompts in the AWP knowledge base.
> One-sentence intent: **A prompt is not an article outline pasted in reverse. It is a methodology packaged as a tool.**
> **This standard is a prompt-framework container**: it hosts several framework versions, each with a clear scope. **The eight-section format is not universal**—choose a framework by task type first. If none fits, **create a new version and state its scope**. Do not force every task into the eight-section format.

## Framework Version × Scope: Choose by Task Type First ⭐

| Framework version | Suitable task type | File |
|-------------------|--------------------|------|
| **Eight-section format (v1)** | One executable generation prompt—a clear role and one output, such as a report, search, rewrite, or single-turn generation | `prompt-format-eight-part.md` |
| **Multi-file prompt-system framework** | A prompt knowledge system that coordinates several files across methodology rule / persona / vocab / machine-readable registry, such as an explainer PPT or motion flow | `advanced/prompt-system-multifile-framework.md` |
| **Create a version when needed** | When neither version fits the task, such as a pure conversational agent / long-chain reasoning / multi-turn interaction / tool-call orchestration, **create a framework version, state its scope, and register it in this table** | — |

> Selection rules: ① identify the task type first → use this table to choose a framework; ② write from that framework's template; ③ **no fitting version = a gap, so create a version** and state its scope and boundary with existing versions. Do not apply the eight-section format to every task.

## Scope

| Who must follow it | Requirement |
|--------------------|-------------|
| Every reusable prompt written for an LLM, including Claude / GPT / Gemini / Llama | Required |
| Prompt sections embedded in a workflow / Agent / Skill / Command | Required |
| Ready-to-copy prompts given to readers in tutorials / articles | Required |
| One-off conversations, including open questions / simple queries | Not required, but the structure is recommended |

## Document Index

| File | Purpose |
|------|---------|
| **prompt-format-eight-part.md** | Main standard: eight-section structure, design philosophy, forbidden anti-patterns, testing, and acceptance; governs **one executable prompt** |
| **prompt-core-blank-template.md** | Empty structure template, ready to copy |
| `advanced/prompt-system-multifile-framework.md` | Multi-file prompt systems and their single source of truth |
| `advanced/prompt-handoff-session-standard.md` | Two modes for handoff prompts, step-based / goal-based |
| `advanced/prompt-example-material-report.md` | Demo 1: an eight-section rewrite based on a real production prompt |
| `advanced/prompt-example-search-generator.md` | Demo 2: an eight-section rewrite of a complex nested structure |
| `advanced/prompt-term-writing-glossary.md` | Terminology definitions |

## Eight-Section Structure: Core

```
[YAML metadata header]
↓
# Role (+ boundaries)
## Core Task (+ success criteria)
## Information Input (+ placeholder rules + fallback)
## Workflow (+ reasoning process + nested subframeworks)
## Examples / Samples (+ positive example + negative example)
## Output Rules (+ field structure + self-check checklist)
## Refusal Cases
```

## Quick Lookup

| What I need to do | Where to look |
|-------------------|---------------|
| Write a prompt from scratch | Copy `prompt-core-blank-template.md` directly |
| Rewrite an existing prompt made by pasting H2 headings in reverse | `prompt-format-eight-part.md` §1.2 Anti-patterns + §3 Eight-section format |
| Write a prompt for an n8n workflow | `prompt-format-eight-part.md` §2.1 Metadata + §6.5 Placeholders |
| Design a role name | `prompt-format-eight-part.md` §4 Role section, three hard rules |
| Design the output format | `prompt-format-eight-part.md` §9 Output rules |
| Add examples | `prompt-format-eight-part.md` §8 + Demo files |
| Test a prompt | `prompt-format-eight-part.md` §13 Five testing steps |

## Nine Forbidden Actions

1. ❌ Paste an article's H2 list in reverse as the "output structure"
2. ❌ Use a generic role such as "AI side-business coach / senior consultant"
3. ❌ Use empty verbs such as "analyze / help / optimize"
4. ❌ Leave input fields without variables, such as not using `{{ }}` or `___`
5. ❌ List only H2 headings in the output rules without giving a field structure
6. ❌ Omit the example section, including at least one positive and one negative example
7. ❌ Omit the self-check checklist
8. ❌ Omit role boundaries
9. ❌ Leave the word count without a hard target

## Relationship with Other Standards

| Standard | Relationship |
|----------|--------------|
| **Skill development standard** | Prompts inside a Skill must follow this standard |
| **Agent tool standard** | When a CLI / MCP tool calls an LLM, its prompt must follow this standard |
| **Distribution asset standard** | Prompts embedded in externally distributed tutorials must follow this standard |

## Working Rule

Read the main prompt standard first. Then follow the eight-part format for every reusable prompt. Choose the framework version by task type — do not force every task into the eight-section format.

## Trigger Words

prompt, prompt template, prompt structure, prompt standard, prompt framework, framework version, eight-section format, multi-file prompt system, prompt architecture, create prompt framework

## Metadata

| Field | Value |
|-------|-------|
| Status | Active |
| Last updated | 2026-07-16 |
| Reviewed | 2026-07-10 |

## Change Log

Keep the latest 10 entries, each no longer than 20 words.

- 2026-07-10: Added long-context / reasoning / caching practices.
- 2026-07-10: Registered the handoff prompt standard.
- 2026-07-09: Indexed style references.
- 2026-07-09: Added the entry structure.
- 2026-05-26: Upgraded to a multi-framework container: framework version × scope routing.
- 2026-05-26: Added the multi-file prompt-system framework.
- 2026-05-21: Created the eight-section standard family.
