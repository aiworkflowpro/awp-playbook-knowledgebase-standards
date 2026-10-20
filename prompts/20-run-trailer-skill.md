# 20 · Run the Skill

- **Purpose:** run the Skill you built. The run stops at step 02 and waits for you to confirm the
  brand truth sheet, then writes the timeline, the voiceover script and five shot prompts.
- **What this repo runs:** prompts-only mode, which needs no GPU and no API key and stops after
  step 06. The video's full run also rendered the shots with an open-weight video model on a
  local GPU (about 40 minutes for five shots). That generator setup is not part of this repo;
  to render video, paste the shot prompts into any video generator you use, or add your own
  adapter in `config/default.yaml`.
- **In the video:** 04 Skill standard, "run the Skill; it stops at a real confirmation point".

## As shown in the video

```text
Run the awp-content-trailer-generating skill for AI Workflow Pro.
```

## Use it in your own project

Given only "run the Skill", an agent may read the step files, decide it already knows the
answer and skip from the gate straight to the shot prompts. Name the constraints:

```text
Working directory: your project root. Run pwd first and cd there if you are not in it.

Run the awp-content-trailer-generating skill you built in skills/ for {your channel}.

Execute every step in order, step00 through step06, and steps 07 and 08 only if a video
generator is configured. Step 02 is a gate: show me the truth sheet and wait for my
confirmation before step 03. Do not skip a step and do not merge two steps into one answer. Each step writes its own output file into the run folder before you
move on. After each step, tell me which step finished and which file it wrote.

If a step cannot run because a tool is missing, say which step and why, then stop. Do not jump
ahead to a later step you happen to be able to do.
```

When it finishes, the run folder should hold one file for every step from 00 to the last one
that ran. A gap in the numbers means a step was skipped. Without a video generator, the Skill
stops after step 06 and leaves you the script and the shot prompts.
