# AGENTS.md

This repository is a toolkit, not a project to work in. It holds three writing standards for AI
agents, the prompts shown in the AI Workflow Pro video "Why Your AI Agent Gives Different Answers
(3-File Fix)", and example outputs. The user's goal is to use the standards
in their own project.

## What is here

All paths are relative to the repository root.

```text
awp-playbook-knowledgebase-standards/
├── standards/   the three standard packages; the part that gets installed
├── template/    AGENTS.snippet.md, appended to the user's AGENTS.md on install
├── prompts/     every prompt from the video, plus a version for the user's own project
└── examples/    example outputs from the recorded run; read-only reference
```

| Package | Entry file | Read it when the task is to |
|---|---|---|
| `standards/awp-meta-authoring-standard/` | `std-core-chapter-skeleton.md` | write or change a standard |
| `standards/awp-prompt-writing-standard/` | `prompt-format-eight-part.md` | write a reusable prompt |
| `standards/awp-skill-development-standard/` | `skill-core-development-standard.md` | build or change a Skill |

Each package has a `CLAUDE.md` index listing its files. Files in `advanced/` are supporting
detail; read them only when the entry file points there.

## Install into the user's project

Do this when the user asks to install or set up the standards. "The project" is the folder the
user is working in, not this repository.

1. Confirm the project root with the user (`pwd`). Never install into this repository.
2. Clone this repository next to the project as `../awp-playbook-knowledgebase-standards` (run `git pull` there if it already
   exists), or reuse the copy you are in.
3. Copy `standards/` into the project as `standards/`. If a package with the same name already
   exists, ask before overwriting it. Keep every other file in the project's `standards/`.
4. Append `template/AGENTS.snippet.md` to the project's `AGENTS.md`; create the file if it does
   not exist. If a `## Standards` section already exists, merge the table rows instead.
5. Claude Code reads `CLAUDE.md`, not `AGENTS.md`. If the project has no `CLAUDE.md`, create
   one containing the single line `@AGENTS.md`. If it has one that does not import
   `AGENTS.md`, add `@AGENTS.md` as its first line.
6. Keep the clone: its `prompts/` folder holds every prompt from the video. Show the user
   `standards/` (one level deep), the new `## Standards` section and the list of prompts, and
   confirm the three entry files exist.

## Using the standards

- Read the entry file for the task before starting, and check the result against the
  standard's checklist before answering.
- A new standard follows the meta-standard: its own package under `standards/`, a `CLAUDE.md`
  index, four-part names (`awp-{topic}-{class}-standard/`, files `{a}-{b}-{c}-{d}.md`).
- After adding a standard, prompt or Skill, register it in the `## Standards` table of the
  project's `AGENTS.md`.
- `prompts/NN-*.md`: the "As shown in the video" block is kept word for word. Use the "Use it in
  your own project" block when the user wants to run a prompt.
- `examples/`: compare structure, not wording. Never copy an example over the user's own work
  without asking.

## Rules for this repository

- Do not write into this repository while helping the user. Their work goes into their project.
- The video was recorded with an older layout (`course/`, `knowledge-base-starter/`,
  `knowledge-base/standards/`). If the user quotes one of those paths, map it using
  `prompts/04-setup-as-filmed.md` and `CHANGELOG.md`.
