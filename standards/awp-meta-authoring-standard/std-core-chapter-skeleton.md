---
document_id: awp-meta-authoring-standard/std-core-chapter-skeleton
language: en
publication: public
source_revision: 3
title: "AWP Standards Standard"
prerequisites: []
see_also: []
---

# AWP Standards Standard

> The standard for standards. It defines one structure, section rules, and maintenance policy for every standard in the AWP knowledge base.
> This is a **guiding framework**, not a collection of examples. It answers "what should a standard look like," not "how do I write one specific standard."

> **Inheritance**: As an .md file, this meta-standard also follows every constraint in your knowledge base's general documentation standard, including Markdown layout, file metadata, and change logs. This standard defines "what a standard looks like," its formal structure. The general standard defines "what the content follows," including layout, structure, and language. They act on different dimensions. Only a real conflict is settled through the priority chain declared in your knowledge base's top-level standards index; see § Relationships Between Standards in this document.

---

## How This Standard Organizes Itself

This standard **defines** the structure. It is not an ordinary instance of that structure. It defines the "seven-section body structure," described in § Body File Structure, but organizes itself flat by decision dimension. It must explain concepts and constraint levels before it can define the structure.

⚠️ Deviation: seven-section body structure | Reason: this meta-standard defines the structure itself. The six sections are its product, not its container. The table below maps this standard's sections to the seven section roles. It proves full coverage with no structural duty omitted:

| Seven-Section Role | Matching Section in This Standard |
|-----------|--------------|
| ① Position | Opening quote block + What a Standard Is |
| ② Design Philosophy | What a Standard Is § Role + Two-File System § Why Two Files |
| ③ Organization Framework | Index File Structure + Body File Structure |
| ④ Section Details | Item-by-item expansion of each structural section |
| ⑤ Shared Rules | Section Authoring Rules + Naming Standard + Deviation Process + Governance |
| ⑥ Checklist | New-Standard Checklist + Existing-Standard Review Checklist |
| ⑦ Build Procedure | Not included in this meta-standard (internal standard; maintainers read ①–⑥ directly) |

---

## Glossary

| Term | Definition |
|------|------|
| **Standard** | A set of verifiable constraints that states what a class of deliverables "must satisfy, should follow, and must not contain" |
| **Deliverable** | A target file governed by a standard, such as a style file, Skill file, or CLAUDE.md |
| **Structure** | The section structure defined by this meta-standard: one shared framework for every standard |
| **Framework** | An organization pattern that a domain standard defines inside the structure, such as a five-question framework or layered architecture |
| **Index file** | The fixed-name `CLAUDE.md`; the Agent entry route, which does not store rule text |
| **Rule body** | The physical file that carries all rules and is loaded when needed |
| **Supporting file** | Supporting material such as a dictionary, glossary, template, example, ledger, decision record, or instance of platform facts; it does not have to follow the full rule-body structure |
| **Compliant** | Satisfies every ✅ item and violates no ❌ item |

---

## Constraint Language Declaration

Following the constraint levels in RFC 2119 and ISO/IEC directives, this standard and all child standards use these markers:

| Marker | Meaning | Use |
|------|------|---------|
| ✅ Required | Omitting it makes the result noncompliant | Core constraint |
| ⚪ Optional | Better when present, but not wrong when absent | Advisory guidance |
| ❌ Forbidden | Doing it makes the result noncompliant | Red line |
| ⚠️ Warning | Strong guidance followed by default. It may say "do not do this by default" or "do this by default." A deviation in either direction must use the deviation process | Exception condition or default |

**Compliance test**: A result is compliant if it satisfies every ✅ item and violates no ❌ item. A deviation from a ⚠️ item must follow the "Deviation Process."

---

## What a Standard Is

### Definition

A standard, or specification, is a set of **verifiable constraints** that says what a class of deliverables "must satisfy, should follow, and must not contain."

A standard is not:

| A Standard Is Not | Difference |
|---------|------|
| Tutorial | A tutorial teaches "how to do it"; a standard defines "what the result must be" |
| Example | An example shows one possibility; a standard defines the boundary of all possibilities |
| Guide | A guide suggests "this is preferable"; a standard requires "do this" |
| Documentation | Documentation describes the current state; a standard defines the target |

> This distinction matches the Diátaxis framework used in technical documentation. A standard maps to reference, a tutorial to tutorial, a guide to how-to, and design philosophy to explanation. The four content types serve different reading goals. Mixing them in one file weakens each of them. This is also the theoretical source for "§ System Health Tests  -  Do Not Mix Types."

### Traits of a Good Standard

| Trait | Test |
|------|---------|
| **Verifiable** | After reading the standard, a reader can decide whether a deliverable complies |
| **Unambiguous** | Two people read the same rule and reach the same conclusion |
| **Complete** | Covers every required decision in the domain, with no "it depends" gray area |
| **Minimal** | Governs only what must be governed and says nothing about matters outside its scope |
| **Agent-facing** | An Agent can act alone after reading it, without asking a person |

### Role of Standards in the AWP knowledge base

```
standard defines the standard → Agent executes according to the standard → The output object is verified according to the standard → Discovery deviation compensates for standard```

A standard is the Agent's **behavior contract**. The Agent does not need to understand "why the rule exists." It only needs to know "what the rule is." The author of the standard must know why, so a standard must state its design philosophy.

### Create a Standard, or Build It Into a Workflow?

Not every constraint should become a standard under `standards/`. Use these tests:

| Create a Standard | Build It Into a Workflow |
|---------|--------------|
| A long-lived constraint reused across several deliverables | A test unique to one workflow that changes with that workflow |
| Repository-wide rules such as formal structure, naming, and metadata | A writing preference for one brand, platform, or format |
| Low retirement cost because it rarely changes | A test that changes often with the business |

> Counterexample: writing style and quality standards once existed as standalone standards. They were retired because each served only its own workflows and changed with them. Their tests moved into workflow `library/`. Before creating a standard, ask: will anything outside the workflow use this constraint? If not, build it in. Do not create a standard.

### Dynamic Dual-Track: When to Change the Standard vs. the Implementation

> Authoritative source: your knowledge base's top-level standards index, under its Standard-Implementation Alignment section.

**The direction depends on the intent of the current task**, not a fixed rule that always favors the standard or always favors the implementation:

| Current Intent | Source of Truth | Action |
|----------------|----------------|--------|
| The knowledge base owner changed or confirmed the implementation/architecture and wants the standard to catch up | Implementation | **Change the standard** |
| The knowledge base owner asks to bring the implementation in line with the standard | Standard | **Change the implementation** |
| No direction specified — a gap was simply discovered | See below | Outdated/wrong standard → change the standard; lazy/incomplete implementation → change the implementation |
| Disagreement over whether a threshold should be strict or lenient | Knowledge base owner | **Wait for a decision** |

❌ Do not default to one direction. ❌ Do not lower a standard's threshold simply because the implementation has not caught up yet.
✅ When the intent is unclear, ask first. Record the decision in the decision table.

---

## Writing Standards for Agents

> Standards are read primarily by Agents. As models grow more capable, the value of adding one more hard rule goes down while its cost goes up. This section defines the new default writing style.

### Templates Over Descriptions

Before writing or revising a standard, open an existing standard of the same type and follow its pattern. **A real template takes priority over the prose in this meta-standard.** When the two conflict, follow the template and update this meta-standard. A template is a working standard — it cannot drift from reality the way prose can.

| Standard Type | Test | Template | What It Gets Right |
|---------------|------|----------|-------------------|
| **Meta-standard** | Governs other standards | `awp-meta-authoring-standard/` | Follows its own rules |
| **Main-child multi-file** | One main + several child standards | `awp-meta-authoring-standard/` | Main body defines shared contract; child bodies add only their differences |
| **Numbered-control** | Many controls requiring traceability | *(not included in this collection)* | Stable IDs (`WEB-002`), profile tags, script-generated index |
| **Technical overlay** | Stacks on top of a product standard | *(not included in this collection)* | Baseline in the overview; each layer only adds implementation constraints |
| **Single-file** | Index + one body covers everything | *(not included in this collection)* | Position section draws a clear "governs / does not govern" boundary |

**Numbered-control is worth a special note.** Give each control a stable ID (`WEB-002`) and tag it with a profile (`profiles: base`). Generate the index by script — the index only registers `ID | level | profile | file` and does not copy rule text. This is "encode the intent into the interface" by example: the ID itself is a reference anchor, the profile itself is an activation condition. No extra prose needed. ⚪ Recommended for standards with more than 100 controls.

### Criteria Over Hard Numbers

When a single sentence that the Agent can self-verify covers the case, do not hard-code a number. A hard number improves results in most scenarios but hurts in a few. A criterion works in both directions.

| Instead of This | Write This |
|-----------------|-----------|
| A single file must not exceed 200 lines | A file covers one topic; when it starts covering a second topic, split it |
| The description column must have at most 3 paragraphs | The description column is readable in one glance — the reader does not need to count paragraphs |
| No comments by default; one line maximum | Written code should read like the code around it |

**Three exceptions — still hard-code these**:

| Type | Why |
|------|-----|
| Security boundaries | These constrain permissions, not taste. One violation is irreversible |
| Physical constraints | Sync latency, API rate limits, and other objective facts that judgment cannot determine |
| Owner preferences | An explicit preference stated by the knowledge base owner, which the model cannot infer |

Test: **ask "is the cost of violating this rule reversible?"** Reversible → criterion. Irreversible → hard constraint.

### Checklists: Only What Machines Cannot Check

A checklist is not a second copy of the rules. After a constraint enters the rule body, ✅ it qualifies for the checklist only if all of the following are true:

- Machine checks cannot cover it (anything machine-checkable goes to the linter; neither the rule body nor the checklist restates it)
- The rule body does not already say the same thing word for word (restating is saying one thing twice)
- It is verifiable ("suitability has been assessed" and "monitoring is in place" are catch-alls that ❌ do not count — an item that cannot be verified will be checked off, not acted on)

A standard ⚪ may have no checklist at all.

### One Rule, One Location

Cross-standard repetition is the most common source of bloat. The test is simple: **where is the authoritative copy of this constraint? Every other location gets a pointer.**

- ❌ Do not restate the meaning of ✅❌⚪⚠️ in each standard — § Constraint Language Declaration in this meta-standard applies repository-wide
- ❌ Do not restate "pass `std-style-language-contract.md`" in each standard — it is a resident output style, already in effect for every session
- ❌ Do not copy the same checklist into N platform/instance files — consolidate it into the entry file of that family; instances only add their own thresholds

### Interface Design Over Examples

Examples lock the Agent into the small space the examples carve out.

- ✅ **Format templates** stay — they are interface definitions that tell the Agent what the output looks like
- ⚪ **One positive example** may stay to resolve ambiguity
- ❌ **Paired good/bad samples** should be removed where possible — a one-sentence criterion that describes the boundary is more precise than two samples that bracket it

### Reference Libraries Belong in the Standard Directory

Each standard's `reference/`, `assets/`, and similar directories hold third-party material and real samples (design-system originals from major companies, open-source project READMEs, case screenshot records). They are **rich reference material** — using real artifacts as specifications is more precise than describing specifications in prose.

✅ Keep the reference library next to the standard it serves. ✅ Do not move it out of `standards/`. The rule body references it with a relative path so the reader does not search across directories.

⚠️ But keep the distinction clear: a reference library is **material**, not **rules**. The rule body ❌ must not cite third-party content from the reference library as a binding constraint — it is there to look at, not to obey.

---

## Directory Naming

Every standard lives in one directory under `standards/`. Its name has exactly four physical parts:

```
awp-{domain}-{target}-standard/
```

| Slot | Rule |
|------|------|
| `awp` | ✅ Mandatory brand prefix. Every standard directory carries it |
| `{domain}` | ✅ Registered area, such as `agent`, `dev`, `doc`, or `meta` |
| `{target}` | ✅ One lowercase alphanumeric word. It contains no hyphen or underscore |
| `standard` | ✅ Fixed package kind for every directory under `standards/` |

Examples: `awp-knowledge-management-standard/`, `awp-meta-authoring-standard/`, `awp-prompt-writing-standard/`, `awp-skill-development-standard/`.

The complete name must match `^awp-[a-z][a-z0-9]*-[a-z][a-z0-9]*-standard$`. Register each package in your knowledge base's package-naming registry. Do not put a compound slug inside one part.

❌ Do not omit the `awp` prefix. ❌ Do not invent another kind suffix. ❌ Do not use a hyphen or underscore inside `domain` or `target`.


The pattern is declared in your knowledge base's directory-pattern registry and registered in its naming-convention document. Both must stay consistent with this section.

---

## Two-File System

Every standard has at least two files:

```
awp-{domain}-{target}-standard/
├── CLAUDE.md               ← Index file (Agent entry)
└── {body}.md               ← Rule body (all rules)
```

A large standard may split into several body files in either of two patterns:

**Parallel files**—each body file is independent and has equal standing:

```
awp-skill-development-standard/
├── CLAUDE.md                            ← Index
├── skill-core-file-declaration.md       ← Body 1 (file structure)
├── skill-core-development-standard.md   ← Body 2 (development workflow)
└── advanced/                            ← Further body files by topic
```

**Main-and-child files**—one main body defines the shared framework, and child bodies add detail by dimension within that framework:

```
awp-meta-authoring-standard/
├── CLAUDE.md                          ← Index
├── std-core-chapter-skeleton.md       ← Main body (shared principles: section skeleton, lifecycle, governance)
├── std-content-file-writing.md        ← Child body (per-section content requirements)
├── std-lifecycle-version-retire.md    ← Child body (standard lifecycle and versioning)
└── ...
```

### Division Between Main and Child Files

| Role | What It Contains | What It Does Not Contain |
|------|-------|---------|
| Main body | Shared principles, shared framework, and anti-patterns across child files | Execution details for one child dimension |
| Child body | Section requirements, deduplication boundaries, and checklist for that dimension | Shared principles already defined in the main body; refer to them instead of repeating them |

✅ The main body must state its relationship with child bodies through an inheritance diagram or division table.
❌ A child body must not repeat shared rules from the main body. Refer to them.

### Child-Body Structure

A child body uses a shorter version of the body structure. The main body owns ⑤ Shared Rules, so a child omits it:

```
① Positioning → ② Design philosophy → ③ Organizational framework → ④ Chapter-by-chapter explanation → ⑥ Checklist```

⚪ A child body may add "Deduplication Principles" and "Anti-Patterns" sections for boundaries and traps unique to that dimension.

---

## Package Complexity Levels

Spec packages are classified into four levels by internal complexity. Determine the level before creating a new package, and organize it accordingly.

| Level | Form | Minimum Structure | Use Case | Example |
|-------|------|------------------|----------|---------|
| L1 · Single file | Index + one body | `CLAUDE.md` + `{target}.md` | Narrow scope, few rules (one body suffices) | *(not included in this collection)* |
| L2 · Parallel multi-file | Index + multiple peer bodies | See § Two-File System · Parallel | Medium scope, topics laid out flat | `awp-skill-development-standard` |
| L3 · Main-child multi-file | Main body + child bodies | See § Two-File System · Main-child | Shared contract with dimension- or form-specific details | `awp-meta-authoring-standard` |
| L4 · Layered suite | Multi-dimension subdirectories, each with its own body files | See below | Extremely wide scope, spans multiple domains, cannot fit in a single file family | `awp-knowledge-management-standard` |

### L4 · Layered Suite

One spec package governs multiple dimensions. Each dimension has its own set of body files. Dimensions are linked through the coverage matrix in the index.

```text
awp-{domain}-{target}-standard/
├── CLAUDE.md                    ← Index: global routing + coverage matrix
├── prompt-term-writing-glossary.md                  ← Optional: package-wide glossary
├── {dimensionA}/                ← Dimension directory
│   ├── {topic1}.md              ← Independent body, follows seven-section structure
│   ├── {topic2}.md
│   └── ...
├── {dimensionB}/
│   └── ...
└── {dimensionC}/
    └── {subdimension}/          ← At most two levels of dimension nesting
        └── {topic}.md
```

✅ The index `CLAUDE.md` must have a coverage matrix listing which deliverables each dimension governs and the boundaries between dimensions.
✅ Each dimension directory has its own body files; each independently follows the seven-section structure.
✅ Dimension subdirectory depth must not exceed two levels (dimension / subdimension).
❌ Dimensions must not cross-reference each other's body content — link only through the index coverage matrix. Shared rules go in the index or package-level supporting files.
❌ Dimension directory names do not use the four-segment package ID format — use short, meaningful English words.

### Level Progression

A spec package may advance in level as it grows: L1 → L2 (when adding a second body) → L3 (when a shared contract creates a main-child relationship) → L4 (when the scope spans multiple domains and a single file family can no longer contain it). When advancing:

1. Update the package form declaration in the index `CLAUDE.md`.
2. L3 → L4 must also create a coverage matrix.
3. Existing body files only change directory location; their content is not rewritten.

---

### File Roles

| Role | What It Contains | Must Satisfy |
|------|-------|----------|
| Index file | `CLAUDE.md`: entry, scope, routes, and file list | Four index elements |
| Rule body | Verifiable rules, forbidden items, and checklist | Body file structure |
| Supporting file | Dictionary, GLOSSARY, template, example, ledger, instance, historical proposal, reference implementation, or build prompt | Clear purpose, source, and maintenance method; does not pose as a rule body |

✅ `CLAUDE.md` must distinguish rule bodies from supporting files in its document index. Group body files by role when there are more than three.
❌ A supporting file must not define a new shared rule. If it starts to state rules, promote it to a rule body or merge it into an existing rule body.

### Metadata for a Standard Body (Decision)

A standard's identity is carried by two files. Metadata appears in one place and is not repeated:

| Location | What It Carries |
|------|---------|
| Index `CLAUDE.md` | Standard status (Draft / Active) + last-updated date; the **only source of truth** for metadata |
| Rule body | ✅ May omit a `## Metadata` table; its parent `CLAUDE.md` carries status and date |

⚪ A body file may use frontmatter such as `title` / `category` / `audit` for machine labels. This is **not a metadata violation**.
⚪ If a body adds its own `## Metadata` table, its date must match the parent `CLAUDE.md` and must not conflict with the index.
❌ Do not write status / date separately in the body and `CLAUDE.md` with conflicting values. Two sources will drift.

> This decision removes the rule conflict among three metadata forms: frontmatter / metadata table / no metadata. All three are valid if they do not conflict with the index source of truth.

### Why Two Files

- **CLAUDE.md** is the router. The Agent reads it first on entering the directory and decides whether to go deeper
- **Rule body** is the substance. All rules live there and load only when needed

The two files have separate duties and do not repeat each other.

---

## Index File Structure (`CLAUDE.md`)

Every standard's CLAUDE.md ✅ must contain this structure:

```markdown
# AWP {domain} Writing Standard
> {One-line positioning: what this standard governs}
## Scope of application
{Clearly state which files/directories this standard governs}
## Document index
| Documentation | Description |
|------|------|
| {filename}.md | {One-line summary: one sentence or ≤7 core words; ❌ do not pile up long "+" tag clouds} |
## Meta information
| field | value ||------|-----|
| Status | Draft / Active (retired standard moved out of `standards/` and is no longer a status value, see § Lifecycle) || Last updated | YYYY-MM-DD || Review | YYYY-MM-DD (⚪ optional, last_reviewed semantics, see below) |```

### Four Index Elements

| Element | Required | Description |
|------|:----:|------|
| One-line position | ✅ | A `>` quote block that says what it governs |
| Scope | ✅ | Exact boundary: which files it governs and which it does not |
| Document index | ✅ | A table listing every body file + one-line summary: one sentence or ≤7 words, not a tag cloud |
| Metadata | ✅ | Status + last-updated date; review date may be added ⚪ |

⚪ **Review field (`last_reviewed`)**: Metadata may add a `review | YYYY-MM-DD` row. It means "the date a person last confirmed that the content was still accurate," unlike the editing meaning of "last updated." If the content did not change but was checked again, change only the review date. Reference review windows: 90 days for tool / workflow standards; one year for meta-standards. Stale detection warns when a review expires (use your audit tool).

### Index Extension for a Large Standard

When there are more than three body files, ✅ group them. Two grouping methods:

**By importance** (for parallel files):

```markdown
### Core standard (N pieces)
| documentation | description ||------|------|

### Auxiliary standard (M pieces)
| documentation | description ||------|------|
```

**By main and child roles** (for main-and-child files):

```markdown
### Main standard
| documentation | description ||------|------|

### {dimension} standard (N pieces)
| Documentation | Scope of application | description ||------|---------|------|
```

⚪ A main-and-child index may add a **quick-reference table** that shows the main differences among all child bodies in one view.

---

## Body File Structure

Every rule-body file ✅ must organize sections in this order:

```
[⓪ Preface] → ① Positioning → ② Design Philosophy → ③ Organizational Framework → ④ Chapter-by-Chapter Detailed Explanation → ⑤ Sharing Rules → ⑥ Checklist → [⑦ Build Procedure]```

| # | Section | Question It Answers | Required |
|---|------|-----------|:----:|
| ⓪ | Front matter (glossary / constraint language declaration) | Which domain terms does this standard use? How should constraint levels be read? | ⚪ |
| ① | Position | What does this standard govern? What is the deliverable? | ✅ |
| ② | Design Philosophy | Why is it designed this way? What are the main principles? | ✅ |
| ③ | Organization Framework | How many parts does the deliverable have? How do they relate? | ✅ |
| ④ | Section Details | What does each part require? | ✅ |
| ⑤ | Shared Rules | Which rules apply to every deliverable? | ⚪ |
| ⑥ | Checklist | How do you check the finished work? | ✅ |
| ⑦ | Build Procedure | How do you build a compliant deliverable from scratch? | ⚪ (✅ for standards published to external users) |

⚪ **Front section (⓪)**: A standard with many terms or one that needs a local constraint-level declaration may add "Glossary" and "Constraint Language Declaration" before ① Position and mark them ⓪. Omit them when there are few terms and start at ①. This meta-standard and most child standards use ⓪. It is a valid optional prefix to the structure, not a violation outside the seven sections.

### ① Position

Use the first two paragraphs to state three things:

| What to State | Example |
|--------|------|
| Deliverable governed | "Guides creation of style files under the workflow style library" |
| Duty of the deliverable | "Defines the article structure, fixed elements, and marking system" |
| One deliverable per item or shared by several | "Each style has its own structure file" |

### ② Design Philosophy

Explain "why it is designed this way." This is design intent for the author, not an execution instruction for the Agent.

Good design philosophy answers:

- Why split it into these files? Orthogonality
- Why use this framework instead of another? Tradeoffs
- What are the main design principles? Hard rules / constraints

### ③ Organization Framework

Describe the deliverable structure with one clear structural model. Two recommended patterns:

**N-question framework** (for content standards):

```
① What to do → ② How to do it → ③ What to put → ④ How to label → ⑤ What not to do```

Each question maps to one section. Every section is required.

**Layered architecture** (for system standards):

```
Staging layer → persistence layer → loading layer```

Define rules for each layer separately.

Whichever pattern you use, ✅ include an **overview table** that maps sections to questions:

```markdown
| # | Question | Chapter | Core content ||---|------|------|---------|
```

### ④ Section Details

Expand every framework section into its own `##` section and cover these items in order:

- **What it must contain**: list required items in a table
- **How to format it**: give a template, not an example
- **Where its boundary lies**: what belongs in this section and what does not

| Good | Bad | Reason |
|----|-----|------|
| List required items in a table + give a template | Long prose description | The former is scannable and machine-readable |
| "Title ≤15 characters" | "Keep the title fairly short" | The former is verifiable |
| Give a template code block | Give only an example screenshot | A template can be copied; an example cannot |

### ⑤ Shared Rules

Extract rules shared among several deliverables into this section so they are not repeated in every deliverable.

State: "The following rules apply to every {deliverable type} and are not repeated in each file."

### ⑦ Build Procedure

Answers "how to build a compliant deliverable from scratch." Sections ① through ⑥ govern what the deliverable must look like at runtime. Section ⑦ governs how to create it from zero through a guided discussion.

⚪ Internal-only standards may omit ⑦ — maintainers read ①–⑥ directly.
✅ Standards published to external users must include ⑦ — so anyone who clones the standard can build compliant output through an interview-driven process.

⑦ has a fixed three-part internal structure:

| Part | Name | Content |
|------|------|---------|
| 1 | Interview Questions | A question table: `#` / what to ask / which file or directory it maps to. The Agent asks each question in turn; the user answers; the Agent writes files from the answers |
| 2 | Agent Rules | How to write after getting answers: use the user's own words (never template language), ask one follow-up if an answer is thin then write, skip conditions, language-style constraints |
| 3 | Build Verification | How to confirm quality after writing: print the file tree, run an effect demo (e.g., a brand-voice test), check against § Checklist |

Template format:

```markdown
## ⑦ Build Procedure

### Interview Questions (ask one at a time)

| # | Ask | Maps to |
|---|-----|---------|
| 1 | ... | which file or directory |
| 2 | ... | ... |

### Agent Rules

- Write in the user's own words — never rewrite into template language
- If an answer is too thin, ask one follow-up, then write what you have
- If a question gets "I don't know" or "not yet", skip the corresponding file and note it in `CLAUDE.md`
- After writing, print the file tree and wait for the user's go-ahead

### Build Verification

- Print the full file tree
- Run an effect demo (have the Agent complete a small task using what was just written, verifying output quality)
- Check against § Checklist item by item
```

### Short Structure for Supporting Files

A supporting file does not have to use the seven-section structure, but ✅ it must let the Agent decide its purpose and boundary:

| Supporting Type | Required Information | Must Not Contain |
|---------|----------|----------|
| Dictionary / GLOSSARY | Scope, item structure, and maintenance method | New process rules |
| Template / example | Purpose, placeholder notes, and entry for use | Unmarked real data |
| Ledger / list | Counting method, update time, and single-source relationship | Rules that can become a global standard |
| Platform fact instance | Last verification date, fact source, and applicable platform | Private brand voice and strategy |
| Historical proposal / decision record | Context, conclusion, and alternatives | Active rule entry |
| Reference implementation | A complete sample built according to the standard (e.g., three people's knowledge base skeletons), for users to compare against | Unverified drafts |
| Build prompt | An executable prompt file that references ⑦ Build Procedure; users paste it to an Agent to trigger the build | Instructions independent of ⑦ |

The supporting file's entry in the `CLAUDE.md` index must say "supporting file" or name its exact type, so it is not mistaken for a rule body.

### Supporting Directory Convention

Supporting files under a standard directory go into the standard subdirectory that matches their nature. Name and substance must agree:

| Directory | What Goes There | Nature | Maintenance |
|------|--------|------|---------|
| `reference/` | Long-lived support such as ledgers, original third-party material, templates, and quick-reference cards | Serves execution of the standard; loaded by the Agent when needed | Living document; a ledger carries a verification date and is reviewed on schedule |
| `templates/` | Skeleton files and sample deliverables ready to copy | A starting point for use, no judgment needed | Updated in sync with the rule body; when the body changes, templates must follow |
| `assets/` | Case screenshots, reference samples, and open-source material records | Real artifacts used as specifications | Append-only; each item carries its source and collection date |
| `glossary/` | Terminology mapping tables, word lists, and enumerated truth tables | Locks "one concept, one spelling" | New concepts enter the glossary before entering the body text |
| `examples/` | Complete reference implementations built according to the standard | Real output used as a model for comparison | Updated when the standard changes; the model must match the current rules |
| `prompts/` | Executable prompt files that reference ⑦ Build Procedure | Users paste them to an Agent to trigger a guided build | Updated in sync with ⑦ |

- ✅ Preserve original third-party material unchanged in one subdirectory per source, such as `reference/{corpus}/{vendor}/`. Add a notes file that records the source URL, fetch date, and type, and states that the material does not enter any external distribution asset.
- ✅ Mark the type of `reference/` content in the `CLAUDE.md` document index.
- ✅ Put one-off research reports, such as selection analysis, competitor breakdowns, and technical comparisons, under `historical research archive (not mirrored in this vault){YYYYMM}/`. ❌ Do not create a `research-archive/` subdirectory inside a standard directory. Move existing `research-archive/` directories in a later batch.
- ❌ Do not put a living reference ledger in historical research archive (not mirrored in this vault); ledgers belong in each standard's `reference/`. Do not put a one-off research report in `reference/`. A mismatch between name and substance starts decay in the supporting area.
- ❌ Process material with no matching rule, such as session drafts, abandoned proposals, or build-period plan files, does not enter a standard directory. Work in progress goes to historical research archive (not mirrored in this vault); completed material goes to `inbox/archive/`.

### ⑥ Checklist

The end of a rule body ✅ must contain a checklist for quick self-review by an Agent or person. Use one `- [ ]` list format.

Group by deliverable type:

```markdown
## Create a new checklist
**{output item type A}**:- [ ] Check item 1- [ ] Check item 2
**{output item type B}**:- [ ] Check item 1```

**Required item in every standard**: The checklist must include one language self-check:
```markdown
- [ ] Passed `std-style-language-contract.md`: three-layer language contract / no jargon / formalized spoken wording / faithfulness, clarity, and grace```
Language Style is a general standard declared in your knowledge base's top-level standards index. It applies globally, so each standard need not repeat its details. The checklist must still check this item explicitly, so no standard misses the language gate.

---

## Section Authoring Rules

### Using Constraint Language

Every rule ✅ must be decidable. After reading it, a reader can answer clearly: "Is this satisfied?"

❌ No vague wording:

| Good | Bad | Reason |
|----|-----|------|
| "Title ≤15 characters" | "Keep it short where possible" | The former can be counted and verified |
| "✅ Must include a Scope section" | "It is better to explain the scope" | The former has an explicit constraint level |
| "❌ Do not put rule text in the index file" | "The index file should not be too long" | The former is a hard red line |

### Conditional Rule Syntax (Based on EARS)

For a rule with a trigger condition, ⚪ use the fixed clause order from EARS: Easy Approach to Requirements Syntax, proposed by Rolls-Royce and published at IEEE RE'09. Put the **condition first, subject in the middle, and behavior last**. One rule contains one subject and one clear response:

| Pattern | Syntax | Example |
|------|------|------|
| General rule | {subject} must {behavior} | Error output must include the `error_type` class |
| State trigger | While in {state}, {subject} must {behavior} | While in `Draft`, a standard must not be cited as a basis for compliance |
| Event trigger | Once {event}, {subject} must {behavior} | Once there are more than three body files, the index must group them |
| Exception handling | If {exception}, then {subject} must {behavior} | If the number of successful branches is below `min_success`, this step must be treated as failed |

Value: A fixed clause order forces the author to state the trigger, prior state, and expected behavior one by one. Ambiguity and "it depends" have nowhere to hide, and each rule directly yields a test case. ❌ Counterexample: "Handle conflicts properly when necessary" has no trigger, subject, or verifiable behavior.

### Prefer Tables

If a rule can use a table, do not write it as prose. Tables help because:

- Agents parse them efficiently
- Column headings define the dimensions and create structure
- They use fewer lines and carry more information

### Positive and Negative Comparison

Give a positive / negative comparison for a key rule. The Agent learns the boundary through pattern matching:

```markdown
| Good | Bad | Reason ||----|-----|------|
| The title should be controlled within 15 words | The title should be concise | The former can be verified |```

### Refer Instead of Repeating

Define a rule in only one place. Refer to it everywhere else:

```markdown
> Series related rules are not managed in this file, see `{other-standard}.md````

### Section Numbers

Use circled numbers (① ② ③ ...) for section positions inside the framework. Define their mapping in the overview table.

---

## Naming Standard

### Directory Names

```
awp-{domain}-{target}-standard/
```

Use only package IDs registered in your knowledge base's package-naming registry. Display titles stay in `CLAUDE.md`; they do not change the directory ID.

### File Names

| File | Naming Rule |
|------|---------|
| Index file | Fixed `CLAUDE.md` |
| Body file | `{domain}-standard.md` for one file or `{subtopic}.md` for several files |

---

## Deviation Process

When a ⚠️ warning rule needs an exception, follow this process:

### Deviation Conditions

| Condition | Description |
|------|------|
| ✅ Must be marked explicitly | Write `⚠️ Deviation: {rule} / Reason: {reason}` in the deliverable. Use a slash separator; an ASCII pipe would split a table cell |
| ✅ Must state the reason | Give a specific reason. "Special case" is not valid |
| ❌ A deviation does not spread | It applies only to this deliverable and does not create a precedent |
| ❌ No deviation from ✅ or ❌ | Required and forbidden rules have no exceptions. To change one, use the standard change process |

### Positive and Negative Comparison

| Good | Bad | Reason |
|----|-----|------|
| `⚠️ Deviation: checklist / Reason: exploratory draft; complete it after finalization` | Omit the checklist without a note | The former can be traced |
| Mark the deviation in the deliverable | Say verbally, "this one is special" | The former leaves a record |

---

## Lifecycle

### Status Definitions

Every standard under `standards/` has exactly one status:

| Status | Meaning | Marked In |
|------|------|---------|
| **Draft** | Being written; must not be cited as a basis for compliance | Index-file metadata |
| **Active** | In force by default; every deliverable must follow it | Index-file metadata |

❌ Do not create a Deprecated / Retired intermediate state. No external consumer pins an old standard version. "Deprecated but retained" only becomes another dead rule people must remember to ignore. A standard is either Active or retired and archived, with nothing between.

### Status Transitions

```
Draft → Active → Retire (move out of standard/)          ↑
Active (can go back to Draft to make major skeleton-level changes)```

### Retirement Process (Instead of Deprecated)

When a standard no longer applies because its tests moved into a workflow, it was merged, or it is fully outdated, retire it in one step. Do not pile deprecation markers into the original file:

| ✅ Step | Description |
|--------|------|
| Move it out of `standards/` | Move the item to `inbox/archive/{YYYYMM}/{date}-standard-{name}-retired/` |
| Register it in the retired table | Add one row to your knowledge base's top-level standards index, under its retired-standards table: retirement reason + destination of the built-in rules + archive location |
| Mark trigger words | Add the standard's keywords to the trigger-word row in the retired table. A match then routes to the archive for reference, not to a live standard |
| Sync upper indexes | Remove live references from the coverage matrix / standard hierarchy / quick lookup / subdirectory index in your knowledge base's top-level standards index |

> For historical instances, see the retired-standards table in your knowledge base's top-level standards index. Writing Style, Quality Standard, AWP knowledge base Reference Manual, and Operations Center Standard were all retired this way.

---

## Governance

### Change Principle (Forward Only; No Patches)

A standard is a living document. git log is its version history, and no downstream consumer pins an old version of a standard. When changing any standard:

| ✅ Right Approach | ❌ Anti-Pattern |
|-----------|----------|
| Replace a wrong rule directly with the new rule | Add ⚠️ "no longer recommended; see the new approach below" beside the old rule |
| Delete a field / section directly | Keep the old field + a "deprecated" comment |
| Rewrite the whole passage | Accumulate comparison notes such as "X was required before; now use Y" |
| Fold a lesson from a failure into the rule itself | Add a long ⚠️ trap / historical-background section |
| Update the index so it points only to the new rule | Point the index to both old and new rule sets |

The only exception: when retiring a standard, add one row to your knowledge base's top-level standards index, under its retired-standards table, with the reason + destination of built-in rules + archive location. This is retirement navigation, not a compatibility patch piled into the original file.

> **Meta-principle**: Once a standard contains a pile of ⚠️ warnings, the next editor tends to add another ⚠️. It soon becomes a warning pile instead of a standard. Reject the first ⚠️.

### Change Process

| Change Type | Process |
|---------|------|
| Correction (typo, wording improvement) | Edit directly and update the metadata date |
| Addition (new section / rule) | After editing, update the date in index-file metadata |
| Restructure (structural change) | First change status to Draft → edit → return it to Active |
| Revision (fix a wrong rule) | **Overwrite directly** with the right rule; do not keep the old rule + warning |
| Retirement (no longer applies) | Move it to inbox archive + register it in your knowledge base's top-level standards index, under its retired-standards table; see § Lifecycle |

### Review Rhythm

| Frequency | Action | Carrier |
|------|------|------|
| Triggered (main) | When deliverables remain noncompliant, check whether the standard itself needs revision | Edit when found |
| Metadata inspection | For a standard whose index metadata "last updated" is more than 90 days old, confirm whether it is still accurate | Your audit tool, which lists stale documents by metadata date |

> Do not create a fixed quarterly task. Real needs trigger standard changes; an idle periodic review becomes ceremony. Leave stale detection to your audit tool, then review a match manually.

### System Health Tests

The readers are Agents and the system has one maintainer. Judge health through these five tests during normal work:

| Test | Requirement |
|------|------|
| Rule budget | Resident context has a hard cap. Before adding one resident rule, delete one. The root `CLAUDE.md` contains only direction and routes |
| Lint share | Keep moving objective rules, such as naming, link validity, forbidden words, and term consistency, from prose into script checks. This share only rises |
| Retirement rhythm | A standard can be born and can die; once dead, it does not return. If nothing retires for six months, the system is decaying |
| Do not mix types | Separate rules, steps, and design philosophy. Design philosophy is for people and does not mix with executable rules |
| Killer items | Every standard set marks 3–5 red lines explicitly instead of burying them among ordinary clauses |

### Change Log

> Rolling window: keep the 10 most recent entries, each no more than 20 words.

A large standard with several files or external references ✅ must maintain its change log at the end of the index file. Child bodies do not keep separate change logs.
A small standard with one file and no more than 200 lines may do so ⚪.

The upstream "File Standard §1.1 Change Log Constraints" defines shared rules for format, rolling window, allowlist and denylist, and hard ceiling. A standard inherits them as a knowledge file. This section does not repeat them.

✅ A standard change log must follow the File Standard's rolling window of **no more than 10 entries** and single-line limit of **no more than 20 words**.
✅ Reference: your knowledge base's file-structure-and-metadata standard §1.1 change-log constraints
❌ Do not rewrite the numbers in this standard or a child standard. One-point maintenance prevents two sources from disagreeing.

---

## Relationships Between Standards

### Priority Chain

The **authoritative definition** of the priority chain is in the "Priority Chain" section of your knowledge base's top-level standards index: four levels, Standards Standard > domain standard > general standard > deliverable's own declaration. This document does not repeat numeric priorities. It only states this standard's position in the chain:

- This file sits at the top. No lower standard may override its structure or constraint language
- Rules a domain standard defines inside the framework cannot be overridden by a deliverable
- A deliverable may declare an exception only to a ⚠️-level rule through the deviation process

### Inheritance

```
standard writes standard (this file)├── Constrains all standard skeleton and chapter rules└── Standard in each field customizes the framework within this skeleton├── business standard → 6×6 two-dimensional architecture├── research standard → three-layer architecture (original/understanding/cognitive)├── SEO-GEO standard → main + 7 stages sub-standard├── CLAUDE.md standard → Three Iron Laws + Six-Layer System       └── ...
```

This standard defines the **structure**: index layout, body-section order, and constraint language. Each domain standard defines its **framework**, the exact pattern inside the Organization Framework section.

### Cross-Reference Principles

- Connect neighboring standards with explicit references
- **Reference direction must be decidable**: ✅ refer upward to a general standard; ✅ refer to another domain standard's public entry, meaning its `CLAUDE.md` or main-standard name. ❌ Do not deep-link to an internal child-section number in another domain standard. Cross-standard section numbers are most likely to break when the other file is restructured. Move a horizontally shared rule up into a general standard, then refer to it from each domain
- Reference format: `See awp-{domain}-{target}-standard/{file}.md`
- **Refer by name, not number or count**: A cross-file reference uses "`{file}.md § section name`". ❌ Do not use a numbered section such as "§15.2," which breaks when a section is inserted. ❌ Do not use a count such as "the four defined checkpoint types," which becomes false when a type is added. Section numbers are allowed only within one file.
- **Do not give one structure twice in full**: A JSON/YAML structure, template, or checklist may have one full definition in one named source. Everywhere else, reduce it to a thin reference + an incremental fragment at most. Two full examples will drift. This is the most common form of decay in a standards system.

---

## Version Management

Standards are knowledge files and **❌ do not carry version numbers**.

Reason: A standard has no external consumer and no case where anyone pins it to vX.Y.Z. At this layer, a version number becomes only an edit counter. Detailed policy → "Version Number Standard §1 Layering Strategy."

### Change Log Location

Put it at the end of the index file (CLAUDE.md) or the body file. A large standard always puts it in the index file; child bodies do not keep separate change logs.
Use the two-column format `| Date | Change content |`, a rolling window of **no more than 10 entries**, and a single-line limit of **no more than 20 words**. See "File Standard §1.1 Change Log Constraints."

---

## New-Standard Checklist

**Index file**:
- [ ] Register the domain, target, and four-part package ID in your knowledge base's package-naming registry
- [ ] Create the directory and two files: CLAUDE.md + body.md
- [ ] CLAUDE.md contains the four elements: position, scope, document index, and metadata
- [ ] Metadata contains status (Draft/Active) and last-updated date

**Rule body**:
- [ ] Organize it in the six-section order: position → philosophy → framework → details → shared rules → checklist. It may start with optional ⓪ front matter: glossary / constraint declaration
- [ ] The framework section has an overview table
- [ ] Every rule is verifiable, with no vague wording
- [ ] Key rules have positive and negative comparisons
- [ ] The checklist is complete and includes the language self-check: pass `std-style-language-contract.md` for three-layer contract / no jargon / formalized spoken wording / faithfulness, clarity, and grace
- [ ] Constraint language is consistent: ✅ ⚪ ❌ ⚠️
- [ ] Change log has no more than **10** entries, each a single line of no more than **20** characters (`filestandard §1.1`)

**Supporting files**:
- [ ] Marked in the `CLAUDE.md` index as a dictionary, template, example, ledger, instance, or decision record.
- [ ] Purpose, source, and maintenance method are clear.
- [ ] Defines no new shared rule. Content that needs to become a rule has entered a rule body.
- [ ] Supporting subdirectories follow the "Supporting Directory Convention": long-lived references go to `reference/`; one-off research goes to historical research archive (not mirrored in this vault); name and substance agree.

**Integration** (update all four registration points in your knowledge base's top-level standards index):
- [ ] Added a row to the coverage matrix: general standards table or domain-standard → area mapping
- [ ] Added a node to the standard hierarchy tree
- [ ] Added a "what I want to do → which standard to read" row to quick lookup
- [ ] Added the directory + trigger words to the subdirectory index

## Existing-Standard Review Checklist

- [ ] Does the index file have a one-line position, scope, document index, and metadata?
- [ ] Has the metadata date gone more than 90 days without an update?
- [ ] Does the rule body begin with a Position section?
- [ ] Does it have an organization framework with an overview table?
- [ ] Is every rule verifiable?
- [ ] Do key rules have positive and negative comparisons?
- [ ] Is there a checklist?
- [ ] Does the checklist contain the language self-check, passing `std-style-language-contract.md`?
- [ ] Are supporting files marked by type, and do none pose as rule bodies?
- [ ] Is constraint language consistent?
- [ ] Does each ⚠️ rule explain the deviation process?
- [ ] Is the standard status accurate (Draft / Active)? If it no longer applies, has it completed the retirement process?
- [ ] Does the change log have no more than **10** entries, each a single line of no more than **20** characters? (`filestandard §1.1`)

## Presentation Defaults Every Standard Writes To

These are the defaults an active standard assumes without restating them. A standard that departs from one must say so and why.

- **System**: AWP. **Owner**: the knowledge base owner. **Root**: `{english_vault_root}`. **Standards root**: `{english_vault_root}/standards/`.
- **Language**: English for prose, headings, metadata, and examples, unless a quoted source or a required identifier uses another language.
- **Runtime output root**: `{english_vault_root}/dashboard/output/` when a standard refers to workflow execution state.
- When a rule needs a brand parameter, write `{brand}` or the explicit `AWP` brand. Never a legacy brand name.

| Label | Use |
|---|---|
| AWP | System, standard, workflow, brand-standard family, and cockpit identity |
| the knowledge base owner | Owner identity in active prose |
| `{english_vault_root}` | Filesystem root |

### Names on Disk Carry the Brand

A path is read far more often than the document at the end of it, and it is what a viewer sees in a terminal. Directory and file names are part of the presentation, not exempt from it.

- `standards/` sub-directories: `awp-{domain}-{target}-standard/`. Each name has exactly four physical parts and appears in your knowledge base's package-naming registry.
- Your tool repository: the entry point and `manifest.name` use `awp-{target}`. `domain` and `form` classify the manifest only. Workspace packages are `{target}/`; service directories are `services/{target}/`.
- `workflows/`: `{brand}-{domain}-{action}-{detail}/`, brand being `awp` or `common`.

An earlier boundary document exempted directory names from renaming. That exception is withdrawn. It was written during the translation pass, when renaming risked breaking references nobody had mapped yet - then it outlived its reason and left 37 standards directories carrying no brand while the rule governing them said otherwise.

**Rename when the reference count is known and can be updated in the same pass. Do not rename a name that something outside this vault resolves.**

### The Only Exempt Names

1. Commands, API names, package names, field names, enum values, and other identifiers an external contract resolves. A name that only something inside this vault reads is not one of these.
2. Your own CLI names, which stay as internal runtime identifiers. In prose, introduce them as such: "the internal CLI `{your-prefix}-sync`."
3. Historical change-log entries, archive titles, third-party quotations, and source material. Do not rewrite history to change presentation.
4. Explicit brand references in a rule that compares, migrates, or isolates brands.
5. `tools/credentials/` and credential values, which are outside every presentation pass.

### Review Rule

A new or edited standard uses AWP, the knowledge base owner, English, and `{english_vault_root}` by default. If it keeps a non-default term, the document makes the exception clear through context or an inline identifier marker.
