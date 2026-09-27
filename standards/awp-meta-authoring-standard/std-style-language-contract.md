---
document_id: awp-meta-authoring-standard/std-style-language-contract
language: en
publication: public
source_revision: 2
title: "Standards Writing Style"
prerequisites: []
see_also: []
---

# Standards Writing Style

> One writing style for every standard, and usable across all AWP knowledge-base documents: **professional, consistent English with no insider jargon, written with faithfulness, clarity, and grace**.
> This is a child standard of the meta-standard. Every standard must follow its language requirements.
> Inheritance: As a .md file, this document follows your knowledge base's top-level standards index under its General Standards section (Markdown layout / file metadata / change log).

---

## Glossary

| Term | Definition |
|------|------|
| **Insider jargon** | Wording only the author understands: internal codes used as words, private abbreviations, undefined metaphors, and compressed label clouds |
| **Three-layer language contract** | Identifier layer / concept layer / prose layer, each with its own job |
| **Label cloud** | Telegraphic compression that forces several unlike pieces of information into one place with ` - ` or `/` |
| **Glossary (GLOSSARY)** | The source-of-truth table that locks "one form per concept" within a system; a working instance of this standard |

---

## Constraint Language

Follow the Standards Standard: ✅ Required | ⚪ Optional | ❌ Prohibited | ⚠️ Warning. Any deviation must be marked explicitly.

---

## 1. Purpose

| Question | Answer |
|--------|------|
| What does it govern? | The **language layer** of every standard: wording, sentences, headings, and terminology consistency |
| What must the output do? | Make standards professional and consistent, so readers, human or Agent, understand them on the first read |
| One standard or several? | **One main standard followed by every standard**; each system's `GLOSSARY.md` is a terminology instance under it |

---

## 2. Design Principles

- **Documents are for readers, not the author's shorthand.** Insider jargon saves the author a few words by moving the cost to every reader.
- **Identifiers and prose have different jobs.** Models are not confused by identifiers in prose. They are confused when one concept has three names. Stripping every identifier out of prose damages code references and makes things worse.
- **Be trustworthy and maintainable.** Insider jargon, casual speech, and terminology drift reduce professional trust and increase the mental cost of later maintenance.
- **This reduces disorder; it does not add a position.** This standard requires only clear and consistent wording. It adds no content or opinion dimension.

---

## 3. Structure

The writing style consists of a **three-layer language contract** plus **four quality lines**:

| # | Layer / line | Core requirement |
|---|---------|---------|
| Contract 1 | **Identifier layer** | Never rewritten |
| Contract 2 | **Concept layer** | One fixed term, with the identifier in parentheses on first use |
| Contract 3 | **Prose layer** | Natural, professional English sentences |
| Quality line 1 | **Remove insider jargon** | Codes are not words / expand abbreviations / define metaphors before use / do not stack label clouds |
| Quality line 2 | **Casual → professional** | Change slang and casual wording into natural formal language; keep standard idioms and terms |
| Quality line 3 | **Faithfulness, clarity, and grace** | Faithfulness: keep the meaning / clarity: one idea per sentence / grace: full sentences without translationese |
| Quality line 4 | **Standardization** | One form per concept / `Term (identifier)` headings / respect enum boundaries |

---

## 4. Detailed Rules

### Three-Layer Language Contract

| Layer | What to use | Example |
|----|--------|----|
| Identifier layer | **Never rewritten**: file names / `id` / field names / enum values / code symbols / APIs / library names / CSS tokens | `concept_type`, `gsap.timeline`, `oklch` |
| Concept layer | **One fixed term, with the identifier on first use**: Use one term in prose. On first use, align it once as `Term (identifier)`; use only the term afterward | motion design (`motion`), window chrome (`shell`) |
| Prose layer | **Natural, professional English**: Write judgments, standards, and explanations as full sentences. Do not string identifiers together in place of prose | — |

### Quality Line 1  -  Remove Insider Jargon

- ✅ **Keep codes in code positions**: Wrap field enum values such as `gen` / `shot` in backticks and use them as code values. ❌ Do not use them bare as adjectives in prose.
- ✅ **Expand abbreviations**: Expand private shorthand, such as "img-gen" → "image generation" and "assign director" → "assign a director."
- ✅ **Define metaphors before use**: For metaphors such as director / palette / parts, explain the real thing in one sentence on first use and keep them out of headings.
- ✅ **Do not stack label clouds**: Use ` - ` / `/` only for real parallel attribute lists, such as "warm-dark  -  serif  -  sharp." ❌ Do not force an identifier + granularity + constraint into one parenthesis.

| Bad jargon example | Revised |
|----------|------|
| `image-generation prompt for gen material` | Write an image prompt for generated material (`source=gen`) |
| `(style_fingerprint  -  one for the whole piece)` | `style_fingerprint` — move "one for the whole piece" into the prose |

### Quality Line 2  -  Casual → Professional

✅ Change slang and casual wording into natural formal English. ❌ But **keep standard idioms and terms**. Do not overcorrect them into stiff language.

| Casual | Revised |  | Standard idiom (keep) |
|------|------|---|---|
| wing it | improvise |  | back to square one |
| nitpick endlessly | work it through fully |  | going in circles |
| crash and burn | fail |  | a single blocking objection |
| not broken | no error |  | — |

### Quality Line 3  -  Faithfulness, Clarity, and Grace  -  Remove Translationese

- **Faithfulness**: Keep every criterion, number, threshold, and instruction. Do not change the meaning;
- **Clarity**: Express one idea per sentence, with no ambiguity. Use active voice whenever it makes the actor clear;
- **Grace**: Use full, natural English sentences. Avoid literal structure from another language, noun piles, and label clouds.

### Quality Line 4  -  Standardization

- ✅ **One form per concept**: Each system creates one `GLOSSARY.md` that locks the English name, identifier, and deprecated variants for every concept. Check it before writing.
- ✅ **Headings name the concept**: `## Term (identifier)`. Use a bare identifier as a heading only when it is a proper name, such as a book or product. Bad: `## 3. agent_workflow_id` → Good: `## 3. Workflow Stable ID (agent_workflow_id)`.
- ✅ **Respect enum boundaries**: Short parallel lists may use ` / `, such as "light / standard / quality-sensitive." Do not use a label cloud to force unlike information into one place.

### Allowlist (Never Change)

The following items are not insider jargon. **Do not damage them while "removing jargon"**:

- **Standard idioms**: back to square one, going in circles, a single blocking objection, and similar phrases;
- **Technical terms**: Common industry terms such as `build` / `console` / `tests` / `diff` / `token` / `API`;
- **Code identifiers**: Field names / enum values / paths / CSS tokens / commands;
- **Valid short enums**: Parallel lists in the form `A / B / C`;
- **Proper names**: English book, product, and module names such as `Design` / `Motion` / `Narration`;
- **Structural areas**: frontmatter, historical change-log rows, and code blocks;
- **Third-party material**: External downloads under `_sources/` and similar directories.

---

## 5. Agent Writing Discipline (A Systematic Defense Against AI Jargon)

> When an AI Agent (Claude / Codex / any LLM) writes standards, documents, or workflow steps, it naturally tends to use bare English terms, stack abstract ideas, and replace natural language with code-style names. The following rules counter those recurring errors.

### 5.1 Common Agent Violations

| Violation | What it looks like | Root cause | Fix |
|---------|------|------|---------|
| **Bare identifiers used as prose** | Prose drops in workflow, artifact, manifest, credential with no explanation | The Agent copies the code vocabulary straight into the sentence | Use the GLOSSARY term in prose and wrap the identifier in backticks |
| **Directory/file names without backticks** | "put into shared" | The Agent does not separate "talking about a directory" from "writing a path" | Directory names must include backticks and a slash: `shared/`, `runs/` |
| **Field names used as prose** | "declare executor type", "write status field" | The Agent mixes YAML field names directly into sentences | Wrap the field name in backticks, such as `executor`, or use the plain concept "executor" |
| **Abstract stacking** | "Implement a pluggable criterion-spec-driven consistency assurance system" | The Agent adds modifiers to appear "precise," but raises the reading cost | Split it into plain sentences: "Write criteria in a structured file. Read them from the file for every judgment to stay consistent." |
| **Invented compounds** | "cross-subflow consumer bus", "runtime responsibility center" | The Agent compresses several ideas into a new term | Split it apart, for example "output index across child workflows" |

### 5.2 Decision Tree for English Words in Prose

When an Agent meets an English word in prose, decide in this order:

```
What is this English word?
  → Path / file name / command → add backticks, do not translate (`manifest.yaml`, `runs/`)
  → YAML/JSON field name / enum value → add backticks (`executor`, `categorical`)
  → Allowlist term (Agent / API / CLI / MCP / SaaS / SEO, etc.) → keep as is
  → Translatable technical concept → check the GLOSSARY mapping
    → Has an English form → use it (identifier on first use)
    → No English form → add it to the GLOSSARY, then use it
```

### 5.3 General Translation Dictionary + System GLOSSARY (Two-Level Dictionary)

**Level 1: Plain-language replacement dictionary** (see the full AWP standards collection). Shared by the whole knowledge base. It says how to replace insider jargon with plain English, such as artifact→output, land→write, and pass through→pass directly. Check it before writing any document. If a term is marked "must replace," always use the replacement. This is the cross-system floor.

**Level 2: System GLOSSARY.** Each standards system, such as Agent Workflow, Brand, or Business, keeps one `GLOSSARY.md`. It fixes the one written form each term takes within that system, such as MRR→monthly recurring revenue. When a new concept appears, check the GLOSSARY first. Use the listed term if it exists. Otherwise, add an entry before using it.

The relationship between the two levels: The replacement dictionary governs "words that should not have been invented." A system GLOSSARY governs "how a term should be expressed." Before writing, an Agent checks them in this order: replacement dictionary (jargon found? replace it) → system GLOSSARY (standard expression found? use it) → neither? Add an entry before use.

### 5.4 Three Post-Writing Checks

After completing any document, an Agent performs these checks:

1. **Scan for bare identifiers**: Find every code symbol, field name, and enum value used in prose outside code blocks, backticks, frontmatter, and the allowlist. Each one must either be in backticks or be replaced by its plain-English term. Otherwise it violates this standard
2. **Check GLOSSARY**: Compare against the GLOSSARY for consistent concept names. Check whether the same concept appears in two forms in the file
3. **Read the prose**: Hide everything inside backticks and read what is left. If it is hard to follow, the prose is too heavy

---

## 6. Shared Rules (Relationship to Existing Standards)

> Former §5, moved to §6 after adding Agent Writing Discipline.

This standard is the **only authoritative source of truth** for writing style. Other files cite it and do not repeat it:

| Document | Role |
|------|------|
| "No insider jargon" in global `~/.claude/CLAUDE.md` | **Trigger rule** pointing to this standard |
| This standard | **Authoritative details** (source of truth) |
| Each system's `GLOSSARY.md` | **Terminology instance** using the mechanism defined here |
| The prompt-system document framework (see the full AWP standards collection) | Application in the prompt knowledge system; cites this standard |

---

## 7. Checklist

> This standard is the language gate itself, so its checklist does not include the self-referential item "pass `std-style-language-contract.md`." The meta-standard requires that item in every standard, but this file is its target and is exempt. The items below expand that language check.

When writing or changing any standard/document, check each item:

Three-layer contract:
- [ ] Identifiers keep their exact spelling; each concept includes its identifier on first use; prose is full sentences.
- [ ] Paths/file names/field names/enum values have backticks in prose, such as `shared/` and `executor`.

Four quality lines:
- [ ] No internal code is used as a word, no private abbreviation remains, metaphors are defined before use, and no label clouds are stacked.
- [ ] No word from your brand's banned-word list appears (create your own brand redlines document if you have one).
- [ ] No slang or casual wording remains, such as wing it / nitpick endlessly / crash and burn; standard idioms and terms remain.
- [ ] Faithfulness, clarity, and grace: criteria and numbers are preserved, each sentence has one idea, and full sentences have no translationese.
- [ ] One form per concept, checked against the system's `GLOSSARY.md`; headings follow Term (identifier).

Agent writing discipline:
- [ ] No bare concept words remain in prose; handle them with the §5.2 decision tree.
- [ ] No invented compounds remain; split compressed terms such as "cross-subflow consumer bus."
- [ ] No abstract stacking remains. If every abstract word can become something concrete, and deleting the sentence leaves the reader knowing no less, it is empty and should be deleted.
- [ ] The system's `GLOSSARY.md` covers every recurring concept in the document.

Allowlist:
- [ ] The allowlist is unharmed: standard idioms / technical terms / code identifiers / valid enums / proper names / structural areas / third-party material.

Beginner-readable report output, only for human-readable reports/daily reports/diagnostic cards/decision files and similar output:
- [ ] A specialist term has an explanation on first use. Use only plain English afterward.
- [ ] A product or platform name has a one-sentence explanation on first use.
- [ ] An industry concept is explained on first use with "In plain terms."
- [ ] The report ends with a terminology table when it contains at least five terms.
- [ ] A smart reader with no technical background can understand it in one read.

> See the full AWP standards collection for the full report checklist (its report-writing standard, § 6 Quality Self-Check List).

---

## 8. Non-Negotiable Beginner Readability for Reports

> Scope: All human-readable analytical reports, daily reports, diagnostic cards, audit reports, decision files, handoff reports, operating statements, interpretation cards, and similar document output. It does not apply to code, configuration files, or script comments.
>
> This section applies the three-layer language contract to report output. The three-layer contract sets the general rule: identifiers use English, concepts use English, and prose is English only. This section adds requirements for nontechnical readers because a report reader may not know technical terms.

### 8.1 Handling English Terms

- ✅ **Explain on first use**: When a specialist term—abbreviation, proper name, or industry term—first appears, immediately add a one-sentence explanation. Format: `Term (what it is)`. Use only plain English afterward.
- ✅ **Use plain English directly when it is clear**: Do not include needless jargon. "Monthly churn rate" is better than "churn rate" alone when the reader is nontechnical.
- ✅ **Keep product and platform names as written but explain them**: Add one sentence on first use. For example, "Reddit (the largest English-language forum platform)" and "ProductHunt (a community for launching and voting on new products)."
- ❌ **Do not use bare abbreviations**: Readers do not know what "TAM/SAM/SOM" means without an explanation. Write the full term first, then add the abbreviation.

### 8.2 Explaining Industry Concepts

- ✅ **Use "In plain terms" on first use of a professional concept**: Explain gross margin, customer acquisition cost, customer lifetime value, monthly churn rate, and similar terms in one plain sentence.
- ✅ **Add a meaning column for metrics in tables**: If a table has columns such as "gross margin" or "monthly churn rate," add a "meaning" column that explains each metric in plain language.
- ✅ **Add a terminology table at the end**: If a report contains five or more terms that need explanation, its appendix must include a "terminology table" in order of first appearance.

### 8.3 Test

After writing a report, run this check:

> Give the report to a **smart reader who knows no technology**. Can they understand it in one read?
>
> If they need a dictionary or another person to understand a word, that word was not handled well.

### 8.4 How Other Standards Cite This Rule

A standard that produces reports adds this line to its output format or quality requirements:

```markdown
> The report language must follow `../awp-meta-authoring-standard/std-style-language-contract.md § 8 Non-Negotiable Beginner Readability for Reports`.
```

---

## Change Log

> Rolling window: keep the 10 most recent entries, each no more than 20 words.

| Date | Change |
|------|---------|
| 2026-06-23 | Added a pointer to the report checklist in §7 |
| 2026-06-23 | Added §8 beginner-readable report rules |
| 2026-06-22 | Added Agent writing discipline |
| 2026-06-12 | Added the checklist's self-reference exemption |
| 2026-05-27 | First version of the writing style |

