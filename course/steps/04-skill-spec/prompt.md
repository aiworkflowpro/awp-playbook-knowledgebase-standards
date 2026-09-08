# Step 4 · Build a Channel Trailer Skill from Scratch

> Paste everything below the line into your agent.

---

**Where you are** — repository `awp-playbook-knowledgebase-standards`, Episode 3 · Standards That Make Agents Consistent.

Run `pwd` first. It must end with `awp-playbook-knowledgebase-standards`. If it does not, `cd` into that
folder before doing anything else; every path below is relative to it.

- `course/` is storage — the step prompts, the finished examples, and the standard packages.
  Read from it, never write into it.
- `knowledge-base/` is your workspace. Everything you produce goes there.
- This step reads the standards from `knowledge-base/standards/`. If
  `awp-skill-development-standard/` is not in there, run
  [Step 0](../00-setup/prompt.md) first — it copies the packages in from `course/reference/`.
  Do not read them out of `course/reference/` instead; the workspace copy is the point.

Read `knowledge-base/standards/awp-skill-development-standard/skill-core-development-standard.md` — this is the Skill standard that defines how to build a complete workflow package with declaration, config, workflow steps, and references.

We're going to build a Skill that generates a 30-second channel trailer — from brand info to voiceover script to video prompts, all in one run.

## Think about the steps first

Before you ask the agent to build the Skill, think about what steps a trailer needs:

0. **Preflight** — check the tools are there before starting: the video generator responds, `ffmpeg` exists, the output folder is writable.
1. **Intake** — channel name, positioning, audience, differentiation, duration, aspect ratio. Read from the knowledge base automatically.
2. **Brand truth sheet** — the agent summarizes what it read into one document, you confirm before continuing. This is a gate.
3. **Plan timeline** — 30 seconds split into segments, each with a duration and rhythm.
4. **Write voiceover script** — pick the story spine, one line per segment, controlled word count, fits in 30 seconds.
5. **Map visuals** — each voiceover line maps to a specific visual: subject, action, environment, camera movement. Not "nice footage" — concrete descriptions.
6. **Compile prompts and generate** — translate the visual mapping into the format your video tool reads, then submit it.
7. **Assemble** — stitch the segments with transitions.
8. **Music, post and delivery** — background track, subtitles, loudness, export.

That is nine files, numbered `step00` to `step08`. Steps 7 and 8 are separate because
stitching and finishing use different tools — keeping them apart lets you rerun either
one without redoing the other.

## Build the Skill

Tell your agent:

```
Working directory: awp-playbook-knowledgebase-standards/ — run `pwd` first and `cd` there if you are not in it.
All paths below are relative to that folder.

Read knowledge-base/standards/awp-skill-development-standard/skill-core-development-standard.md — this is the Skill standard.

I need a channel trailer generator Skill. Input: a channel name. Output: a 30-second promotional video package.

I've designed nine steps: preflight, intake, brand truth sheet (gate), plan timeline, write script, map visuals, compile prompts and generate, assemble, music and delivery.

Follow the Skill standard to generate a complete Skill package — SKILL.md, config/, workflow/ (step00 through step08), and references/.

Build it inside my knowledge base, at knowledge-base/skills/awp-content-trailer/ — not in this repo. This repo is the course material; the Skill is mine to keep.
```

Look at what landed in knowledge-base/skills/awp-content-trailer/. It should have:
- A `SKILL.md` declaration — what the Skill does, what it takes as input, how many steps
- A `config/` directory — defaults that don't change (duration, style)
- A `workflow/` directory — nine step files, `step00` through `step08`, one per step you designed
- A `references/` directory — supporting material (prompt writing rules, vocabulary)

File naming is uniform, directory structure is uniform, declaration fields are uniform — because the Skill standard handles the form. The agent filled in your nine steps.

Open `workflow/step04-script.md` and check: does it have input, output, concrete instructions, and quality criteria?

## Register the new folder

Episode 2 built eight top-level folders. The Skill just added a ninth — `knowledge-base/skills/`. This is the first folder you add on your own, and a new folder needs two things before it counts as part of the knowledge base.

```
Working directory: awp-playbook-knowledgebase-standards/ — run `pwd` first and `cd` there if you are not in it.
All paths below are relative to that folder.

knowledge-base/skills/ is a new top-level folder in my knowledge base. Two steps:

1. Write knowledge-base/skills/CLAUDE.md — a router for this folder, following the same
   pattern as the other eight: a one-line positioning quote, what lives here, and an index
   of the Skills inside with their trigger words.
2. Update knowledge-base/CLAUDE.md — add a skills/ row to the Subdirectory Index table,
   and add a changelog line.
```

The folder exists on disk either way. It becomes part of the knowledge base when the root router points at it — that is what makes the next agent, in the next session, able to find and run this Skill without being told it exists.

## Run it

```
Working directory: awp-playbook-knowledgebase-standards/ — run `pwd` first and `cd` there if you are not in it.
All paths below are relative to that folder.

Run the channel trailer Skill with input: AI Workflow Pro

Execute every step in order — step00 through step08, no skipping, no merging two steps
into one answer. Each step writes its own output file into the run folder before you move
to the next one. After each step, tell me which step just finished and what file it wrote.

If a step cannot run because a tool is missing, say which step and why, then stop. Do not
jump ahead to a later step you happen to be able to do.
```

**That "no skipping" instruction is not boilerplate.** Given only "run the Skill", an agent will
read the step files, decide it already knows the answer, and jump straight from the brand gate
to compiling shot prompts — skipping the timeline, the script, and the visual mapping. The
output looks plausible and the run folder is half empty. Naming the constraint is what keeps a
nine-step Skill from collapsing into three.

Watch it execute step by step:

| Step | What it does | What lands in the run folder |
|------|--------------|------------------------------|
| 00 | Preflight — checks tools before starting | `preflight.md` |
| 01 | Reads your `knowledge-base/brand/` directory | `01-intake.md` |
| 02 | Brand summary — **stops and waits for your confirmation** | `02-truth-sheet.md` |
| 03 | Splits 30 seconds into timed beats | `03-timeline.md` |
| 04 | Writes the voiceover, one line per beat | `04-script.md` |
| 05 | Maps each line to a concrete shot | `05-visual-mapping.md` |
| 06 | Compiles the shot prompts | `06-prompts/` |
| 07 | Stitches the segments | assembled video |
| 08 | Music, subtitles, export | final deliverable |

When it finishes, check the run folder. **Every step from 00 through the last one you ran must
have left a file.** A gap in that sequence means the Skill skipped a step, and whatever came
after it was built on nothing.

The shot prompts can go directly to a video generation tool.

**No GPU? No problem.** Steps 1–5 (brand info → script → visual mapping) run on any agent with no special hardware, and step 6 writes the shot prompts before it submits anything. Only the submit half of step 6, plus steps 7 and 8, need a video generation tool. You can use any tool: MiniMax H3 locally, Seedance via API, or stop after the prompts are compiled and keep the script + shot prompts as your deliverables.

## See the example

Check [`knowledge-base-complete/skills/awp-content-trailer/`](../../../knowledge-base-complete/skills/awp-content-trailer/) — the Skill exactly as it came out of the episode recording. Fifteen files: `SKILL.md`, `config/`, nine step files in `workflow/`, and four rule and spec files in `references/`.

It is captured from the machine, not written by hand, so it is what you should actually end up with. Compare structure, not wording — your step files will describe your nine steps in your words.

**Everything in it is plain text.** No GPU, no model weights, no local service: the Skill package itself is a set of Markdown instructions. Hardware only enters the picture when a step submits a prompt to a video generator, and even then you can stop at step 6 and keep the script plus shot prompts as your deliverable.

**Where a Skill goes from here.** The author's production copy of this Skill has been running for months: sixty files, thirty-six visual style presets, five rule documents, wired to a local video model on a 24 GB GPU. It started as roughly what you just built. Each run that went wrong turned into one more line in a step file or one more preset — that accumulation is the whole point.

You do not need any of that hardware to have a working Skill. You need the structure, and one run to start learning from.

Wait for my go-ahead.
