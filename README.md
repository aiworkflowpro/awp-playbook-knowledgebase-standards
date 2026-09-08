# AWP Playbook · Standards That Make Agents Consistent

Companion repo for the video **I Wrote 5 Rules. My AI Agent Stopped Making the Same Mistakes.**

You write three standards — meta-standard, prompt standard, and Skill standard — that make your AI agent produce stable, high-quality output every time. Five steps, every prompt is in this repo, and the standards are included.

## What you need

- Any AI agent that can read and write files: Claude Code, ChatGPT desktop, Cursor, OpenCode, Codex, Windsurf, or others.
- A knowledge base with `owner/`, `brand/`, `business/`, `standards/`, `workflows/`, `research/`, `dashboard/`, `inbox/`. **You do not have to build one first** — if you skipped [Episode 2](https://github.com/aiworkflowpro/awp-playbook-knowledgebase-skeleton), this repo ships a finished one in `knowledge-base-starter/`.

## Setup

Everything is in [`course/steps/00-setup`](course/steps/00-setup/) — paste that prompt into
your agent and it clones the repo, fills `knowledge-base/`, and copies the standards into it.
If you would rather run the commands yourself:

```bash
git clone https://github.com/aiworkflowpro/awp-playbook-knowledgebase-standards
cd awp-playbook-knowledgebase-standards

# A · You did Episode 2 — bring your own knowledge base
cp -r ../awp-playbook-knowledgebase-skeleton/knowledge-base/. ./knowledge-base/
# B · You skipped Episode 2 — start from the finished E02 result shipped here
cp -r knowledge-base-starter/. ./knowledge-base/

# then, either way — copy the standards into the workspace
cp -r course/reference/* knowledge-base/standards/
```

**Run your agent from this folder.** Every path in the step prompts is relative to the
repository root. Each prompt states the working directory at the top, so a fresh agent
window always knows where it is — but an agent started one directory up still will not
find the files.

## How to follow

Open `course/steps/`, go into each step folder, and paste that folder's `prompt.md` into your agent. Each step builds on the previous one.

[`course/examples/`](course/examples/) holds the finished version of every step that produces
one — steps 2, 3, and 4. Compare yours against it as you go. (Steps 0 and 1 have no example:
setup produces no document, and step 1 is conceptual.)

The prompts in each step are short task descriptions. The agent reads the full standard and follows it — **you paste a simple instruction; the agent reads the detailed spec.**

Where it reads them from matters: `course/` is storage, `knowledge-base/` is the workspace. Step 0 copies the standards from `course/reference/` into `knowledge-base/standards/`, and from Step 2 on, every prompt reads them there. Your agent never reaches outside its own workspace to find its own rules. The originals stay in `course/reference/` as the source the copy came from.

## The steps

| Step | Folder | What it builds |
|------|--------|----------------|
| 0 | [`00-setup`](course/steps/00-setup/) | Clone, fill the workspace, copy the standards into it |
| 1 | [`01-concept`](course/steps/01-concept/) | Understand why standards work (four data points) |
| 2 | [`02-meta-spec`](course/steps/02-meta-spec/) | Generate a cognitive discipline standard using the meta-standard |
| 3 | [`03-prompt-spec`](course/steps/03-prompt-spec/) | Write a structured prompt and score it 29/30 vs 17/30 |
| 4 | [`04-skill-spec`](course/steps/04-skill-spec/) | Build a channel trailer Skill from scratch (nine steps) |

## What's inside

```text
awp-playbook-knowledgebase-standards/
├── course/                     ← STORAGE. read from it, never write into it
│   ├── reference/              the three standard packages, copied into the workspace at Step 0
│   ├── steps/                  five steps, each with the full prompt.md
│   └── examples/               the finished version of every step that produces one
├── knowledge-base/             ← WORKSPACE. everything you build lands here
├── knowledge-base-starter/     ← where you start: the finished Episode 2 result, 105 files
├── knowledge-base-complete/    ← where you end up: the recording's real output, 163 files
├── CLAUDE.md · README.md
└── LICENSE · LICENSE-CODE
```

The split is deliberate: **`course/` is storage — prompts, examples, and the standard packages.
`knowledge-base/` is the workspace, where all the work actually happens.**
Copy `knowledge-base-starter/` into `knowledge-base/`, work there, then compare what you produce
against `course/examples/`.

## Checking your work

`knowledge-base-complete/` is what the knowledge base looked like when the episode finished
recording — captured from the machine, not written by hand, so it matches what you see on
screen. Yours will differ in wording, because every file is built from your answers rather than
the demo's. Use it to check structure and depth:

```text
knowledge-base/
├── standards/
│   ├── awp-cognitive-discipline-standard/   ← step 2 wrote this
│   ├── awp-meta-authoring-standard/         ← the three packages, installed at step 0
│   ├── awp-prompt-writing-standard/
│   ├── awp-skill-development-standard/
│   └── prompts/kb-approach-comparison.md    ← step 3 wrote this
├── skills/awp-content-trailer/              ← step 4 built this: 15 files
├── research/
│   ├── cognitive-discipline-test.md         ← step 2 output
│   ├── kb-approaches-no-standard.md         ← step 3, the run without a standard
│   └── kb-approaches-with-standard.md       ← step 3, the run with one
└── business/youtube/channel-trailer/        ← the Skill's own run record
```

It is also the starting point for the next episode.

## What you end up with

After completing all five steps, your knowledge base gains:

```text
knowledge-base/
├── standards/
│   ├── awp-knowledge-management-standard/  ← already there from Episode 2
│   ├── awp-meta-authoring-standard/        ← copied in at Step 0: the meta-standard
│   ├── awp-prompt-writing-standard/                ← copied in at Step 0: eight-part prompt format
│   ├── awp-skill-development-standard/                 ← copied in at Step 0: Skill package structure
│   ├── awp-cognitive-discipline-standard/  ← you generate this in step 2
│   ├── prompts/
│   │   └── kb-approach-comparison.md       ← written in step 3
│   └── CLAUDE.md                           ← index of all standards
├── skills/
│   └── awp-content-trailer/                ← built in step 4
├── research/
│   ├── cognitive-discipline-test.md        ← step 2 output
│   ├── kb-approaches-no-standard.md        ← step 3 output (the control)
│   └── kb-approaches-with-standard.md      ← step 3 output
└── (everything from E02 stays)
```

Every standard package sits flat inside `standards/` — no `reference/` layer in the workspace.
After Step 0 that is four packages: the one Episode 2 left there plus the three copied in.
`course/reference/` is only the shelf the copy came from.

Every one of those lands inside `knowledge-base/`, and every step ends by registering what it
built in the matching `CLAUDE.md` index — a file no router points at is a file your next
session will never find.

## The real system behind this

The author runs 800,000+ files across 32 standards. This playbook teaches the same method, starting from the knowledge base you built in E02.

## All playbooks

→ [awp-playbook-series-hub](https://github.com/aiworkflowpro/awp-playbook-series-hub)

## License

Content: [CC BY 4.0](LICENSE). Code: [MIT](LICENSE-CODE).
