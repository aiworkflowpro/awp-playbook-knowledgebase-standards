# AWP Skill Development Standard

> Rules for the full Skill lifecycle: create, change, test, release.
> A Skill is a persistent workflow package — declaration + config + workflow steps + references.

## Scope

All Skills. A Skill packages a multi-step workflow into a reusable tool that gets better every run.

## Document Index

**Core** (start here):

| File | Purpose |
|------|---------|
| **skill-core-development-standard.md** | Main standard: what a Skill is, package structure, design philosophy, the no-patch change rule |
| **skill-core-file-declaration.md** | SKILL.md declaration file: naming, frontmatter, trigger, scope |

**Advanced** (specialist topics in `advanced/`):

| File | Purpose |
|------|---------|
| `advanced/skill-config-parameter-standard.md` | Three-layer config, `config/runtime.json`, model assets, variable-output template |
| `advanced/skill-context-loading-standard.md` | Context-dimension selection, internal CLI search, direct-read priority |
| `advanced/skill-credential-file-standard.md` | Credential folder, two-mode fallback, live validation |
| `advanced/skill-docs-authoring-standard.md` | `docs/guide.md` and `docs/setup.md` templates |
| `advanced/skill-runtime-data-standard.md` | Runtime `runs` folder, preflight cache, progress records, resume |
| `advanced/skill-term-development-glossary.md` | Terminology definitions |
| `advanced/skill-design-pattern-library.md` | Eleven patterns P1–P11, anti-pattern list |
| `advanced/skill-platform-constraint-limits.md` | Claude Code features: model matrix, Hook, Agent Teams, Worktree |
| `advanced/skill-prompt-template-standard.md` | SubAgent prompt writing, custom Agent integration |
| `advanced/skill-script-file-standard.md` | When to use a script or SubAgent, dependency declarations |
| `advanced/skill-step-document-standard.md` | Startup preflight, step naming, executors, parameter collection |
| `advanced/skill-testing-process-standard.md` | EDD, preflight, cross-model tests, release checklist |
| `advanced/skill-error-common-fixes.md` | Seven error layers, five-layer validation, retry rules |

## Working Rule

Read `skill-core-development-standard.md` first. Then route to the specialist file that matches the work.

## Metadata

| Field | Value |
|-------|-------|
| Status | Active |
| Last updated | 2026-08-30 |
