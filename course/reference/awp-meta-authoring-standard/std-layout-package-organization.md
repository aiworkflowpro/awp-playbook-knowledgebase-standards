---
document_id: awp-meta-authoring-standard/std-layout-package-organization
language: en
publication: public
source_revision: 1
title: "Standard Authoring Standard · Package Organization"
---

# Standard Authoring Standard · Package Organization

> Sub-file. Covers how standard packages are organized: how to split files and structure directories. Main text → `std-core-chapter-skeleton.md`.

---

## Dual-File System

Every standard consists of at least two files:

```
awp-{domain}-{target}-standard/
├── CLAUDE.md          ← Index file (Agent entry point)
└── {target}.md        ← Rule text (complete rules)
```

Large standards can split into multiple text files in two patterns:

**Parallel multi-file** — each text file is independent and equal in rank:

```
awp-skill-development-standard/
├── CLAUDE.md                            ← Index
├── skill-core-file-declaration.md       ← Text 1 (file structure)
├── skill-core-development-standard.md   ← Text 2 (development workflow)
└── advanced/                            ← Further text files by topic
```

**Primary-subordinate multi-file** — one primary text defines the general framework; subordinate texts add detail by dimension:

```
awp-meta-authoring-standard/
├── CLAUDE.md                          ← Index
├── std-core-chapter-skeleton.md       ← Primary text (shared contract: section skeleton, lifecycle, governance)
├── std-content-file-writing.md        ← Subordinate text (per-section content requirements)
├── std-lifecycle-version-retire.md    ← Subordinate text (standard lifecycle and versioning)
└── ...
```

### Primary-Subordinate Division of Responsibility

| Role | Contains | Does not contain |
|------|----------|-----------------|
| Primary text | General principles, shared framework, cross-subordinate anti-patterns | Specific execution details of a sub-dimension |
| Subordinate text | Chapter requirements for that dimension, deduplication boundaries, checklist | General principles already defined in the primary text (use references instead of repetition) |

✅ The primary text must explicitly declare the relationship with subordinate texts (inheritance diagram or division table).
❌ Subordinate texts must not repeat the primary text's general rules — reference them.

### Subordinate Text Skeleton

Subordinate texts follow a simplified version of the text skeleton — ⑤ Shared Rules is handled by the primary text, so subordinates omit it:

```
① Positioning → ② Design Philosophy → ③ Organizational Framework → ④ Chapter Details → [⑥ Checklist]
```

⚪ ⑥ Checklist is optional in subordinate texts: in a primary-subordinate multi-file family, the primary text carries the unified checklist; subordinate texts add one only when they have dimension-specific self-check items not covered by the primary checklist.
⚪ Subordinate texts may add "Deduplication Principles" and "Anti-Patterns" chapters for dimension-specific boundaries and pitfalls.

### Why Dual-File

- **CLAUDE.md** is the router — the Agent reads it first upon entering a directory and decides whether to go deeper
- **Rule text** is the substance — complete rules are written here, loaded on demand

The two files have orthogonal responsibilities; no overlap.

---

## Package Tier Classification

Standard packages are classified into four tiers by internal complexity. When creating a new standard, determine the tier first and organize accordingly.

| Tier | Form | Minimum structure | Use case | Example |
|------|------|------------------|----------|---------|
| L1 · Single-file | Index + one text | `CLAUDE.md` + `{target}.md` | Narrow constraint scope, low rule volume (fits in one text) | *(not included in this collection)* |
| L2 · Parallel multi-file | Index + multiple peer texts | See § Dual-File System · Parallel | Medium constraint scope, flat by sub-topic | `awp-skill-development-standard` |
| L3 · Primary-subordinate | Primary text + subordinate texts | See § Dual-File System · Primary-subordinate | Has a shared contract, refined by dimension or form | `awp-meta-authoring-standard` (this standard) |
| L4 · Layered suite | Multi-dimension subdirectories + each dimension has its own text family | See below | Extremely broad constraint scope, spans multiple domains, cannot fit in a single file family | `awp-knowledge-management-standard` |

### L4 · Layered Suite

One standard package governs multiple dimensions. Each dimension has its own set of text files. Dimensions are connected through a coverage matrix in the index.

```text
awp-{domain}-{target}-standard/
├── CLAUDE.md                    ← Index: global routing + coverage matrix
├── prompt-term-writing-glossary.md               ← Optional: package-wide glossary
├── {dimensionA}/                ← Dimension directory
│   ├── {topic1}.md           ← Independent text, follows the seven-chapter skeleton
│   ├── {topic2}.md
│   └── ...
├── {dimensionB}/
│   └── ...
└── {dimensionC}/
    └── {sub-dimension}/         ← Maximum two levels of dimension nesting
        └── {topic}.md
```

✅ The index `CLAUDE.md` must have a coverage matrix listing what each dimension governs and its boundaries with other dimensions.
✅ Each dimension directory has its own text files; each independently follows the seven-chapter skeleton.
✅ Dimension subdirectory depth must not exceed two levels (dimension / sub-dimension).
❌ Dimensions must not cross-reference each other's text content — connect only through the index coverage matrix. Cross-dimension shared rules go up to the index or package-level auxiliary files.
❌ Dimension directory names do not use the four-segment package ID format — use short, meaningful English words.

### Tier Promotion

Standard packages can be promoted as they grow: L1 → L2 (when adding a second text) → L3 (when a shared contract needs a primary-subordinate relationship) → L4 (when spanning multiple domains that a single file family cannot contain). When promoting:

1. Update the package tier declaration in the index `CLAUDE.md`.
2. L3 → L4 must also create a coverage matrix.
3. Existing text files change only their directory location, not their content.

---

## File Roles

| Role | Contains | Must satisfy |
|------|----------|-------------|
| Index file | `CLAUDE.md`: entry point, scope, routing, file list | Index four elements |
| Rule text | Verifiable rules, prohibitions, checklists | Text file skeleton (see `std-content-file-writing.md`) |
| Auxiliary file | Dictionaries, GLOSSARY, templates, examples, ledgers, instances, historical proposals, reference implementations, build prompts | Clear purpose, source, and maintenance method; must not masquerade as rule text |

✅ `CLAUDE.md` must distinguish rule texts from auxiliary files in its document index; group by role when there are more than 3 text files.
❌ Auxiliary files must not define new general rules; if an auxiliary file starts writing rules, it must be promoted to rule text or merged into existing rule text.

---

## Naming Conventions

The standards collection targets open-source publication; **paths and text are in English**. Paths are identifiers matched byte-for-byte by machines and cross-vault references — renaming breaks them; text is human-readable content.

### Directory Naming

```text
awp-{domain}-{target}-standard/
```

Four segments: `awp` is the fixed namespace; `{domain}` is the registered domain; `{target}` is a single target word; `standard` is the standard package type. Each segment allows only lowercase ASCII letters and digits. No hyphens or underscores within segments. The directory name must match `^awp-[a-z][a-z0-9]*-[a-z][a-z0-9]*-standard$`.

Domains, targets, and standard package IDs are centrally registered in `{standards_root}naming-map.yaml`. Compound concepts must use a single clear target word — for example, compress `product-development` to `product`; do not stuff a compound into one slot.

### File Naming

| File | Naming |
|------|--------|
| Index file | Fixed `CLAUDE.md` (loaded by name in the ecosystem; no locale suffix) |
| Text file | `{topic}.md` — English kebab-case topic name |
| Auxiliary machine-readable | `{name}.yaml` / `{name}.json` (neutral single copy; no locale suffix) |
| Third-party original source | `{name}.md` — **preserved as-is; no locale suffix** |

✅ **File names must reflect the constraint target** so the agent can route without opening the text: `std-core-chapter-skeleton.md`, `std-style-language-contract.md`, `std-family-alignment-rules.md`.
❌ **Name-content mismatch prohibited**: a file named `identity-business-model` whose text constrains the operations domain should be renamed to something accurate.
❌ **Pinyin and ad-hoc abbreviations prohibited**: file names like `ming-ming` or `nm` do not qualify.

---

## Change Log

> Rolling window, keep the latest 3 entries, each ≤20 words.

| Date | Change |
|------|--------|
| 2026-08-22 | Split from main text: dual-file + L1-4 + index + naming + layout + machine enforcement |
