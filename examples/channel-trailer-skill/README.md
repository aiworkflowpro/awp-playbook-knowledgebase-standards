# Example: a channel trailer Skill built from the Skill standard

> Example output from one recorded run. Your result will differ: your Skill describes your own
> steps in your own words. Compare structure, not wording.

This is the Skill that [prompt 19](../../prompts/19-build-trailer-skill.md) built in the video, as
it came out of the recording: 15 files.

| Path | What it is |
|---|---|
| [`awp-content-trailer-generating/SKILL.md`](awp-content-trailer-generating/SKILL.md) | Entry point: what the Skill does, the nine steps, hard rules, input and output |
| [`awp-content-trailer-generating/config/default.yaml`](awp-content-trailer-generating/config/default.yaml) | Fixed settings: duration, format, where brand notes live, where runs are saved |
| [`awp-content-trailer-generating/workflow/step00-preflight.md`](awp-content-trailer-generating/workflow/step00-preflight.md) … [`step08-deliver.md`](awp-content-trailer-generating/workflow/step08-deliver.md) | One file per step; step 02 is a gate that waits for your confirmation |
| [`awp-content-trailer-generating/references/`](awp-content-trailer-generating/references/specs/prompt-framework.md) | Voiceover and visual-mapping rules, the shot-prompt framework, a camera vocabulary |

## Run it in your own project

1. Copy `awp-content-trailer-generating/` into your agent's skills folder
   (Claude Code: `.claude/skills/`; Codex: `.agents/skills/`; any other agent: keep it in `skills/` and name `skills/awp-content-trailer-generating/SKILL.md` in your prompt).
2. **Point it at your brand notes.** The defaults assume a knowledge base with a
   `knowledge-base/brand/` folder. Without one, create a `brand/` folder with a few short Markdown
   files (who the channel is for, what it covers, how it sounds), then in `config/default.yaml`
   set `brand_dir: brand/` and `output_root: runs/channel-trailer/`. The Skill only uses facts it
   reads there; anything missing stays marked as missing in the truth sheet.
3. Pick the length. `trailer.duration_seconds` in `config/default.yaml` is 30; set 15 for a short
   or 60 for a one-minute cut, and adjust `segments` and `narration_words_total` to match.
4. Run it with [prompt 20](../../prompts/20-run-trailer-skill.md).

Everything in the package is Markdown and YAML. No GPU or API key is needed: the Skill runs in
prompts-only mode and stops after step 06 with a truth sheet, a timeline, a voiceover script and
five shot prompts.

**Rendering video is not included.** Full mode (steps 07–08) needs a video generator adapter,
an audio source and ffmpeg, and this repo ships no adapter. In the video, the shots were rendered
with an open-weight video model on a local GPU, set up separately. To get video from your run,
paste the five shot prompts into the generator you already use, then cut the clips to the
script.
