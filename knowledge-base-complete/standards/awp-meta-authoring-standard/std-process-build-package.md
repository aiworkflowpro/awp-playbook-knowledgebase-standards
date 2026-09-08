---
document_id: awp-meta-authoring-standard/std-process-build-package
language: en
publication: public
source_revision: 2
title: "Standard Authoring Standard · Build Process"
---

# Standard Authoring Standard · Build Process

> Sub-file. Covers how to build a compliant deliverable from scratch. Main text → `std-core-chapter-skeleton.md`.

## Positioning

⑦ Build Process is the seventh chapter of the seven-chapter text skeleton. Chapters ① through ⑥ govern what the deliverable looks like (runtime constraints); ⑦ governs how to build one from scratch (build-time guidance).

⚪ Internally used standards may omit ⑦ — maintainers can execute by reading ①–⑥ directly.
✅ Standards published for external users must include ⑦ — enabling anyone to build compliant deliverables through a guided process.

## Eight Build Forms

The build process is not just interviews. Based on information source and operation style, there are eight forms. The first five are for initial construction; the last three are for continuous evolution.

### Initial Construction (five forms)

| # | Form | Code | Information source | What the Agent does |
|---|------|------|-------------------|---------------------|
| 1 | Interview | `interview` | User verbal input | Ask questions one by one → user answers → write files from answers |
| 2 | Document analysis | `analysis` | User-provided existing files | Read files → extract key points → write in standard format |
| 3 | Information gathering | `research` | Web search, open-source projects, best practices, industry materials | Search → filter → organize → write to knowledge base |
| 4 | Hands-on verification | `verify` | Output from prior steps | Execute a task → compare results → prove the rules work |
| 5 | Standard-driven generation | `generate` | The standard itself | Structure is determined → build directly per standard → no user input needed |

### Continuous Evolution (three forms)

| # | Form | Code | Information source | What the Agent does |
|---|------|------|-------------------|---------------------|
| 6 | System migration | `migrate` | User's old system (Notion, Obsidian, Google Drive, etc.) | Understand old structure → map to new structure → bulk import |
| 7 | Reflective iteration | `iterate` | Run records, lessons learned | Review output → find issues → adjust structure or add to standard |
| 8 | Benchmark comparison | `benchmark` | Others' knowledge bases, open-source repositories | Pull → analyze structure → adapt usable parts to own framework |

### Form Combinations

Most areas' ⑦ uses not a single form but a chain of multiple forms. The opening of ⑦ declares the combination order using `Build forms:`, and the Agent executes in sequence.

Examples:
- `Build forms: interview` — pure interview
- `Build forms: interview → verify` — interview first, then verify
- `Build forms: interview → research → analysis` — ask for direction, search materials, then analyze
- `Build forms: generate` — pure standard-driven, no user input needed
