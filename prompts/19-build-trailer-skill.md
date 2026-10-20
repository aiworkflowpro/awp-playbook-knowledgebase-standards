# 19 · Build the channel trailer Skill

- **Purpose:** build a complete Skill package from the Skill standard: `SKILL.md`, `config/`,
  `workflow/` with nine step files, and `references/`.
- **In the video:** 04 Skill standard, "run the Skill; it stops at a real confirmation point".
  The finished Skill from the recorded run is in
  [`examples/channel-trailer-skill/`](../examples/channel-trailer-skill/README.md).

## As shown in the video

On screen the standards sit in `knowledge-base/standards/`. After the Quick Start they are in
`standards/` in your project.

```text
Read knowledge-base/standards/awp-skill-development-standard/skill-core-development-standard.md. Build a channel-trailer Skill at knowledge-base/skills/awp-content-trailer-generating/ — nine steps, step00 preflight to step08 delivery.
```

## Use it in your own project

Think through the steps before you ask. The nine steps used in the video:

| Step | What it does |
|---|---|
| 00 Preflight | Check the tools first: video generator, `ffmpeg`, a writable output folder |
| 01 Intake | Read channel name, positioning, audience and voice from your brand notes |
| 02 Brand truth sheet | Summarize what was read into one page. **Gate:** you confirm before it continues |
| 03 Timeline | Split 30 seconds into segments, each with a duration and a beat |
| 04 Script | One voiceover line per segment, within the word budget |
| 05 Visual mapping | Each line becomes one concrete shot: subject, action, environment, camera |
| 06 Prompts | Compile shot prompts for your video tool; submit only if one is configured |
| 07 Assemble | Stitch the segments with transitions |
| 08 Deliver | Music, subtitles, loudness, export |

```text
Working directory: your project root. Run pwd first and cd there if you are not in it.

Read standards/awp-skill-development-standard/skill-core-development-standard.md. It is the
Skill standard.

Build a channel trailer generator Skill. Input: a channel name. Output: a 30-second
promotional video package. Nine steps: preflight, intake, brand truth sheet (a gate that waits
for my confirmation), timeline, script, visual mapping, compile prompts and generate, assemble,
music and delivery.

Read my brand facts from {folder with your brand notes}. Never invent a brand fact that is not
written there.

Every run writes to a new run folder named with the date and time; never reuse or overwrite
an earlier run.

With no video generator configured, the Skill runs in prompts-only mode: steps 00 to 06 still
run, step 06 writes the shot prompts instead of submitting them, and a missing generator or
generator credential is not a preflight failure.

Build it yourself from the Skill standard; do not copy examples/channel-trailer-skill/ from
the cloned repository (compare against it afterwards if you like).

Follow the Skill standard to produce the full package at skills/awp-content-trailer-generating/:
SKILL.md, config/, workflow/ (step00 to step08) and references/. Then add a row for the Skill
to AGENTS.md: what it does and its trigger words.
```

Check that `workflow/step04-script.md` has an input, an output, concrete instructions and
quality criteria. To let your agent start the Skill by name, copy the folder into its skills folder
(Claude Code: `.claude/skills/`; Codex: `.agents/skills/`; any other agent: keep it in `skills/` and name `skills/awp-content-trailer-generating/SKILL.md` in your prompt).

If you installed the example Skill for prompt 01, delete that copy first (for example
`.agents/skills/awp-content-trailer-generating/`): two Skills with one name leave your agent
guessing which to run in prompt 20.

The name follows the Skill standard's four-part form `awp-{domain}-{target}-{action}`:
domain `content`, target `trailer`, action `generating`.
