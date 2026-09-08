# Standards

> Leo's rules — naming, writing, thinking, and operating conventions the agent must follow. Episode 3 brought in three standard packages and produced the first standard the agent wrote for itself.

This area holds the rules that keep the knowledge base consistent: how files are named, how content is written, how the agent thinks, and how work is done. Rules live here as single sources of truth, not repeated in every folder.

Two shapes live side by side. **Single files** are short rules that fit on one page. **Packages** — every directory named `awp-*-standard/` — are full standards with their own index, core file, and advanced material; open the package's `CLAUDE.md` first to find the right file inside.

## File Index

| File | Content |
|------|---------|
| `naming-convention.md` | Four-part filename rule — type-object-scope-date, compact dates, no spaces |
| `format.md` | Document format rule — metadata block, title, plain simple English |
| `youtube-script-standard.md` | AWP YouTube script rule — hook in 20s, structure, concise English |

## Package Index

| Package | Content | Trigger words |
|---------|---------|---------------|
| `awp-knowledge-management-standard/` | The standard that built this knowledge base in Episode 2 — naming rules, directory patterns, area skeletons, methodology | naming, directory pattern, area skeleton, methodology, how to build, knowledge management |
| `awp-meta-authoring-standard/` | How to write any standard — the chapter skeleton every standard follows, plus language, lifecycle, and package layout rules | meta-standard, how to write a standard, skeleton, authoring, language style |
| `awp-prompt-writing-standard/` | The eight-part format for reusable prompts, with a blank template and advanced patterns | prompt format, eight-part, reusable prompt, prompt template |
| `awp-skill-development-standard/` | How to build a Skill — declaration, config, workflow steps, references, and thirteen advanced topics | skill, workflow package, SKILL.md, step files, preflight |
| `awp-cognitive-discipline-standard/` | Rules that constrain how the agent thinks on open-ended discovery tasks — seven weaknesses, one rule each, checkable thresholds | cognitive discipline, thinking rules, discovery, product direction, blue ocean, avoid obvious answers |

## Rules

- The four packages that arrived with the episodes are **read-only source material**. Standards the agent writes go into new packages of their own.
- Cite the file when a rule comes from one of these, so the reasoning stays traceable.
- New packages follow the same naming shape: `awp-{topic}-{kind}-standard/`, four segments, three hyphens.

## Subdirectory Index

| Directory | Content | Trigger words |
|-----------|---------|---------------|
| `prompts/` | Reusable prompts written with the eight-part prompt standard — each is an executable tool with its own metadata header | reusable prompt, eight-part prompt, kb-approach-comparison |

## Change Log

| Date | Change |
|------|--------|
| 2026-09-06 | First written prompt added — `prompts/kb-approach-comparison.md` (eight-part format) |
| 2026-09-06 | Cognitive discipline standard written using the meta-standard |
| 2026-09-06 | Episode 3 standards installed — meta-authoring, prompt writing, skill development |
| 2026-08-25 | Knowledge management standard moved into the knowledge base |
