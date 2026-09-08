---
document_id: awp-meta-authoring-standard/std-lifecycle-version-retire
language: en
publication: public
source_revision: 1
title: "Standard Authoring Standard · Lifecycle"
---

# Standard Authoring Standard · Lifecycle

> Sub-file. Covers the complete process from creating to retiring a standard. Main text → `std-core-chapter-skeleton.md`.

---

## Multilingual and Publication

### Translation Guidelines

If you maintain translations of your standards, follow these principles:

- Keep a single source of truth and translate outward. Do not edit translations directly — always edit the source first.
- When translating, maintain a glossary; use one translation per concept across the entire collection.
- Preserve path references, `{placeholders}`, frontmatter keys, code blocks, and table structures as-is; translate only the descriptive text.
- Third-party originals under `reference/` keep their source language and are not translated.

---

## Deviation Mechanism

When a ⚠️ warning rule needs an exception, follow this procedure:

### Deviation Conditions

| Condition | Description |
|-----------|-------------|
| ✅ Must be explicitly marked | Write `⚠️ Deviation: {rule}  --  Reason: {rationale}` in the deliverable |
| ✅ Must state the reason | The rationale must be specific — "special circumstances" is not a valid reason |
| ❌ Deviations do not propagate | Valid only for the current deliverable; does not set precedent |
| ❌ Cannot deviate from ✅ and ❌ | Must and must-not rules have no exceptions — to change them, go through the standard change process |

### Good vs Bad

| Good | Bad | Why |
|------|-----|-----|
| `⚠️ Deviation: Checklist  --  Reason: Exploratory draft, will complete after finalization` | Silently omitting the checklist | The former is traceable |
| Marking deviation in the deliverable | Saying "this time is special" verbally | The former has a record |

---

## Lifecycle

### Status Definitions

Every standard under `{standards_root}` has exactly one status:

| Status | Meaning | Marked in |
|--------|---------|-----------|
| **Draft** | Under development; must not be cited as a compliance basis | Index file metadata |
| **Active** | In effect (default); all deliverables must comply | Index file metadata |

❌ No Deprecated/Retired intermediate state. Standards have no external consumers pinning old versions; "deprecated but retained" only becomes another dead rule everyone ignores — either Active or archived out. No middle ground.

### Status Flow

```
Draft → Active → Retired (moved out of {standards_root})
          ↑
   Active (can revert to Draft for skeleton-level rewrites)
```
