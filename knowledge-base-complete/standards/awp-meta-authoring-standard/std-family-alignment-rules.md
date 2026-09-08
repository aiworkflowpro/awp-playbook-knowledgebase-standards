---
document_id: awp-meta-authoring-standard/std-family-alignment-rules
language: en
publication: public
source_revision: 3
title: "Standards Family Alignment"
prerequisites: []
see_also: []
---

# Standards Family Alignment

> **Purpose:** Align every package under `{standards_root}` with the standard-authoring and language-style rules. Remove compressed jargon, inconsistent frames, and term drift.
> **Reference package:** a content-writing standard from the full AWP standards collection, which is readable, has a glossary, and uses a clear six-role frame.
> **Principle:** Align form and language. Do not rewrite every technical detail. Keep body constraints as small as their purpose allows.

## 1. Scope

| Property | Definition |
|----------|------------|
| Controls | Package acceptance criteria, execution order, and completion definition |
| Does not control | Domain rules such as API implementation or VPS configuration; it only requires clear language, a valid frame, and stable terms |
| Readers | Agents and decision-makers who edit standards |

## 2. Reasons for Alignment

| Problem | Effect |
|---------|--------|
| Inconsistent numbering after repeated edits | Rules are hard to locate |
| Unexplained English jargon in prose | Human readers cannot interpret requirements consistently |
| No glossary and several terms for one concept | One term gains several meanings |
| Rules repeated in body, checklist, and machine check | Text expands and versions conflict |
| Writing taste converted into code-scanned rules | Output becomes uniform and conflicts with dynamic writing |

## 3. Completion Definition

A package is aligned only when it satisfies A through D.

### A. Entry and Source Files

- A1: A `CLAUDE.md` index routes readers and does not contain the full standard.
- A2: One or more clear normative body files exist.
- A3: The index states scope, exclusions, document index, and change log.

### B. Body Frame

Cover the six roles in `std-core-chapter-skeleton.md`; equivalent headings are allowed:

| Role | Required Content |
|------|------------------|
| Purpose | Scope, exclusions, and one-sentence rule |
| Design philosophy | Short explanation of why the rule exists |
| Organization | A table or diagram connecting concepts |
| Operation | Steps or controls an Agent can execute |
| Shared rules | Adjacent ownership and conflict priority |
| Checklist | Optional; include only checks that machine validation cannot cover |

A numbered technical control layer can keep its control-item form but still needs purpose, philosophy, and index routing.

### C. Language and Terms

- Use the package's target prose language. Keep identifiers and code in their original form.
- Use one term for one concept. A first occurrence can identify an equivalent term in parentheses.
- Do not use internal aliases, private abbreviations, undefined metaphors, or tag clouds as prose.
- Provide a glossary in the body or as `../awp-prompt-writing-standard/advanced/prompt-term-writing-glossary.md`.
- Replace casual phrases with precise professional language.

### D. Extra Rules for Writing and Creation

- Align with the dynamic-writing standard from the full AWP standards collection.
- Do not make sentence-pattern code scanning the primary taste gate.
- Platform length and prohibited content can be hard controls. Human feel and readability require Agent judgment.

Technical standards still follow A through C but do not inherit D's content position.

## 4. Execution Order

1. Read the package `CLAUDE.md` and main body.
2. Check A through D.
3. Fix the glossary, purpose, opening principle, and obvious jargon first. Leave correct details unchanged.
4. Update the package change log.
5. Never remove a valid safety or physical constraint only to make the package look aligned.

## 5. Common Replacements

| Old Pattern | Current Pattern |
|-------------|-----------------|
| Unexplained level or voice labels | Plain section names and defined terms |
| An untranslated belief-to-desire chain | A defined viewpoint loop with identifiers only where needed |
| Internal jokes used as requirements | Explicit prohibited metric, sentence menu, or concise rule |
| Mixed numbering systems | One continuous section system |
| No glossary | Body glossary or `../awp-prompt-writing-standard/advanced/prompt-term-writing-glossary.md` |
| Regular expressions as writing review | Writing-method pointer; machine checks only for measurable format and safety |
| Mechanical alignment banner | One natural inheritance sentence or no banner |
| Style library as a creation authority | Writing method plus workflow exemplars |

## 6. Checklist

- [ ] A1–A3 provide a clear package entry.
- [ ] The body has purpose, philosophy, and executable controls.
- [ ] A glossary exists and does not replace a richer existing glossary.
- [ ] Prose has no unexplained jargon, tag clouds, or casual shorthand.
- [ ] Creation packages use dynamic writing and do not make code-scanned style the main route.
- [ ] The package change log is updated.
- [ ] The responsible editor opened and reviewed each changed document instead of using an unreviewed bulk rewrite.

## Change Log

| Date | Change |
|------|--------|
| 2026-08-08 | Removed historical execution ledgers and internal roles |
| 2026-07-29 | Aligned package entries and scope boundaries |
| 2026-07-29 | Created the repository-wide alignment standard |
