---
document_id: awp-meta-authoring-standard/std-content-file-writing
language: en
publication: public
source_revision: 1
title: "Standard Authoring Standard · File Content"
---

# Standard Authoring Standard · File Content

> Sub-file. Covers what goes inside a standard text file. Main text → `std-core-chapter-skeleton.md`.

---

## Text File Skeleton

All rule text files ✅ must organize chapters in this order:

```
[⓪ Preamble] → ① Positioning → ② Design Philosophy → ③ Organizational Framework → ④ Chapter Details → ⑤ Shared Rules → ⑥ Checklist → [⑦ Build Process]
```

| # | Chapter | Question answered | Required |
|---|---------|-------------------|:--------:|
| ⓪ | Preamble (glossary / constraint language declaration) | What special terms does this standard use? How to read constraint levels? | ⚪ |
| ① | Positioning | What does this standard govern? What is the deliverable? | ✅ |
| ② | Design Philosophy | Why is it designed this way? What are the core principles? | ✅ |
| ③ | Organizational Framework | How many parts does the deliverable have? What are their relationships? | ✅ |
| ④ | Chapter Details | What are the specific requirements for each part? | ✅ |
| ⑤ | Shared Rules | What rules apply to all deliverables? | ⚪ |
| ⑥ | Checklist | How to self-check after completion? | ✅ (⚪ for sub-texts in a primary-subordinate multi-file family; see `std-layout-package-organization.md § Sub-text skeleton`) |
| ⑦ | Build Process | How to build a compliant deliverable from scratch? | ⚪ (✅ when targeting external users) |

Complete definition of ⑦ → `std-process-build-package.md`.

⚪ **Preamble (⓪)**: Standards with many terms or that need local constraint-level declarations may add a "Glossary" and "Constraint Language Declaration" before ① Positioning, marked ⓪. Omit if terms are few and start directly at ①. This meta-standard and most sub-standards use ⓪ — it is not a violation of the seven-chapter skeleton but a legitimate optional prefix.

### ① Positioning

The opening two paragraphs must clarify three things:

| What to say | Example |
|-------------|---------|
| What deliverable it governs | "Guides creating style files under the workflow style library" |
| The deliverable's responsibility | "Defines article skeleton, fixed elements, and marking system" |
| One deliverable per file or shared | "Each style has its own independent structure file" |

### ② Design Philosophy

Explains "why it is designed this way." Not execution instructions for the Agent, but design intent for the author.

Good design philosophy answers:

- Why split into these files? (orthogonality)
- Why this framework and not another? (trade-offs)
- What are the core design principles? (hard rules / constraints)

### ③ Organizational Framework

Describes the deliverable's skeleton using a clear structural model. Two recommended patterns:

**N-question framework** (for content-type standards):

```
① What to do → ② How to do it → ③ What to include → ④ How to label → ⑤ What not to do
```

Each question maps to a chapter; all are required.

**Layered architecture** (for system-type standards):

```
Staging layer → Persistent layer → Loading layer
```

Each layer defines rules independently.

Regardless of pattern, ✅ there must be an **overview table** mapping chapters to questions.
