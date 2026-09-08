# awp-playbook-knowledgebase-standards

> Episode 3 playbook: standards that make AI agents consistent and high-quality.
> Continues the knowledge base built in [Episode 2](https://github.com/aiworkflowpro/awp-playbook-knowledgebase-skeleton).

## Where you are

**Start your agent in this folder.** Every path in every step prompt is relative to this
repository root, so `course/reference/awp-prompt-writing-standard/prompt-format-eight-part.md` means the file right here, not somewhere
in the user's home directory. If the agent is running one level up, `cd` into this folder
first — otherwise it will not find the standards it is told to read.

```text
awp-playbook-knowledgebase-standards/   ← your agent works here
├── course/                     STORAGE — read from it, never write into it
│   ├── reference/              the three standard packages, copied into the workspace at Step 0
│   ├── steps/                  five steps, each with the full prompt.md
│   └── examples/               finished version of steps 2, 3, and 4, for comparison
├── knowledge-base/             WORKSPACE — everything happens here
├── knowledge-base-starter/     the finished Episode 2 result — copy source, do not edit
├── knowledge-base-complete/    what it looks like after all four steps — reference only
├── README.md
└── CLAUDE.md                   this file
```

`course/` has exactly three subdirectories: `reference/`, `steps/`, `examples/`. There is no
`course/standard/` — if a document mentions it, that document is stale.

`knowledge-base-complete/` is the recording's real output, captured from the machine the
episode was filmed on: 163 files against the starter's 105. Read from it to check your own
work; never write into it.

**The one rule that matters**: `course/` is storage — the full step prompts, the finished
examples, and the standard packages. `knowledge-base/` is the workspace, where everything
actually gets done. Every file a step produces belongs in `knowledge-base/` — a new standard
goes to `knowledge-base/standards/`, a Skill goes to `knowledge-base/skills/`, research output
goes to `knowledge-base/research/`. Never write into `course/`, never leave output at the repo root.

Two rules that follow from it:

- **The standards get copied in at Step 0**, from `course/reference/` to
  `knowledge-base/standards/`. After that, every step reads them from the
  workspace. The agent never reaches outside the workspace for its own rules. The originals
  stay in `course/reference/` — that is the source the copy comes from.
- **Register what you build.** After a step adds a file or a folder, update that directory's
  `CLAUDE.md` index and add a changelog line. This is the rule Episode 2 established: a file
  that no router points at is a file the next agent will never find.

## Where the knowledge base comes from

`knowledge-base/` starts empty. It gets filled one of two ways during setup, and the
distinction matters because step 04 reads `knowledge-base/brand/` to build the trailer Skill:

| The user | What lands in `knowledge-base/` |
|---|---|
| Followed E02 and built their own | Their own knowledge base, copied over from the E02 repo |
| Came straight to E03 | A copy of `knowledge-base-starter/` — the finished E02 result |

`knowledge-base-starter/` is a snapshot, not a working directory. Read it, copy from it,
never write into it. Everything the steps produce goes into `knowledge-base/`.

If `knowledge-base/` is still empty when a step needs it, stop and tell the user to run
`course/steps/00-setup/prompt.md` — do not invent brand facts to fill the gap.

The same test applies to the standards. `knowledge-base/standards/` already exists the moment
the workspace is filled, so its presence proves nothing — check for the **packages**. If
`awp-meta-authoring-standard/`, `awp-prompt-writing-standard/`, or `awp-skill-development-standard/` is not inside
it, Step 0's copy never ran. Stop and send the user back to Step 0 rather than reading the
originals out of `course/reference/`.

## What's here

- `course/reference/` — the three standards this episode teaches, copied into the workspace at Step 0. Core files at package root; `awp-prompt-writing-standard/` and `awp-skill-development-standard/` also carry an `advanced/` subdirectory
- `course/steps/` — five steps (`00-setup`, `01-concept`, `02-meta-spec`, `03-prompt-spec`, `04-skill-spec`), each holding one `prompt.md`
- `course/examples/` — the finished version of steps 2, 3, and 4. Steps 0 and 1 produce no document, so they have no example
- `knowledge-base/` — the user's KB, filled during setup
- `knowledge-base-starter/` — finished E02 knowledge base, 104 Markdown files, used when the user skipped E02

After Step 0 runs `cp -r course/reference/* knowledge-base/standards/`, the packages sit **flat**
inside `knowledge-base/standards/` — not nested under a `reference/` folder:

```text
knowledge-base/standards/
├── awp-knowledge-management-standard/   from Episode 2
├── awp-meta-authoring-standard/         entry file: std-core-chapter-skeleton.md
├── awp-prompt-writing-standard/                 entry file: prompt-format-eight-part.md
└── awp-skill-development-standard/                  entry file: skill-core-development-standard.md
```

Those three entry files are what steps 2, 3, and 4 tell the agent to read. Step 2 writes its
output alongside them, as a package at `knowledge-base/standards/awp-cognitive-discipline-standard/`.

## Steps

| Step | What it does | Needs the knowledge base? |
|------|-------------|---|
| 00-setup | Clone, fill `knowledge-base/`, copy the three standard packages into `standards/` | Creates it |
| 01-concept | Understand what standards are and why they work (four data points) | No |
| 02-meta-spec | Generate a cognitive discipline standard using the meta-standard, then validate it | Writes into it |
| 03-prompt-spec | Write a knowledge base comparison prompt using the eight-part format (17/30 vs 29/30) | Writes into it |
| 04-skill-spec | Design nine steps and build a channel trailer Skill from scratch | **Yes — reads `brand/`** |

## Demo tasks

| Layer | Task | Output |
|-------|------|--------|
| Meta-spec | Generate a cognitive discipline standard → run SaaS blue ocean validation | 7 weaknesses + 9 rules → 3 validated blue ocean directions |
| Prompt | Compare five AI agent knowledge base approaches (six dimensions) | Structured comparison matrix, scored 29/30 vs 17/30 without standard |
| Skill | Build + run a channel trailer generator (nine steps) | Voiceover script + shot prompts → trailer video |
