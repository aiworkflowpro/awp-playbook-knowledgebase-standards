---
name: awp-content-trailer
description: Turns a channel name into a complete 30-second channel trailer package. Reads the brand facts from the knowledge base brand/ area on its own, then produces a brand truth sheet (with a confirmation gate), a 30-second timeline, a five-line voiceover script, a visual mapping, and shot prompts ready for any video generator. Triggers on "trailer", "channel trailer", "promo video", "brand video", "30-second trailer".
---

# awp-content-trailer

Generates a 30-second channel trailer package (1920×1080, 16:9) for a media brand from one input — the channel name. Brand facts are read from `knowledge-base/brand/` automatically; nothing about the brand is hardcoded. Runs in two modes:

- **Prompts-only mode (default)**: no video generator is configured. Produces the full package through step06: truth sheet, timeline, voiceover script, visual mapping, and compiled shot prompts. The prompts can go straight into any video generator (MiniMax, Seedance, Veo, Kling, and similar).
- **Full mode**: a generator adapter is configured in `config/default.yaml`. The package runs end to end: prompts are submitted, segments render, step07 stitches them with ffmpeg, and step08 adds music, subtitles, and loudness normalization.

## Execution rules

1. Read before doing: open `workflow/stepNN-*.md` before executing step NN.
2. No skipped steps: step00 → step01 → step02 (gate) → step03 → step04 → step05 → step06. Full mode continues to step07 → step08; prompts-only mode stops after step06.
3. Each step ends only when its validation checkpoints pass. Fix one problem category per iteration, maximum three iterations.
4. The step02 truth sheet is a gate: present it and wait for explicit confirmation before any creative step.

## Workflow

| Step | What it does | Gate? |
|------|-------------|:-----:|
| step00-preflight | Check output folder, ffmpeg, video generator, references; decide run mode (full or prompts-only) | |
| step01-intake | Channel name → read `brand/` facts → intake record | |
| step02-truth-sheet | One-page brand truth sheet + five-field message, built only from what was read; user confirms | ✅ |
| step03-timeline | 30 s split into five segments, each with a duration and beat type | |
| step04-script | One voiceover line per segment, fits 30 seconds | |
| step05-visual-mapping | Each voiceover line → one concrete visual (subject, action, environment, camera) | |
| step06-prompts | Compile shot prompts in the generator format; submit only in full mode | |
| step07-assemble | Stitch segments with transitions (full mode only) | |
| step08-deliver | Music, subtitles, loudness, export, manifest (full mode only) | |

## Hard rules

- **Never invent brand facts.** The truth sheet is built only from files read in step01. Facts marked draft or undefined in the knowledge base stay marked in the truth sheet; creative steps must not fill them in silently.
- **No hype.** The voiceover uses the brand voice found in the knowledge base. Marketing filler and superlatives are prohibited.
- **Fresh output every run.** Script lines, visuals, and prompts are generated fresh. Never reuse phrasing from an earlier run.
- **One line, one visual.** Every voiceover line maps to exactly one concrete shot in step05. "Nice footage" is not a visual.

## Input

| Mode | What the user provides |
|------|----------------------|
| Channel name | The channel name (for example "AI Workflow Pro"). Everything else is read from `knowledge-base/brand/`. |

## Output (prompts-only mode)

```
{business/youtube/channel-trailer/{keyword}-{YYYYMMDD}}/
├── 01-intake.md
├── 02-truth-sheet.md          (the gate artifact)
├── 03-timeline.md
├── 04-script.md               (five lines, ~30 s)
├── 05-visual-mapping.md       (one concrete visual per line)
├── 06-prompts/
│   └── shot-1..5.txt          (compiled shot prompts)
└── preflight.md
```

Full mode adds rendered segments, the assembled video, subtitles, and the delivery manifest in step08.

## What prompts-only mode is worth

The default mode needs no GPU, no model weights, no local service — the Skill is Markdown instructions end to end. Hardware only matters at the moment step06 submits a prompt somewhere.

Stopping at step06 leaves you with a script, a timeline, and five shot prompts. Paste them into any hosted generator, hand them to a videographer as a shot list, film them yourself, or keep the voiceover alone as a 30-second script. Deciding what the trailer says and what each second shows is the expensive part, and that is what steps 01–06 produce.

Adapters for specific generators, plus a worked example of folding a shot block into a single-paragraph prompt → `references/specs/prompt-framework.md`.

## References

| File | Contains |
|------|---------|
| `references/rules/voiceover-rules.md` | Word budgets, pace, five-line structure, brand voice rules |
| `references/rules/visual-mapping-rules.md` | Four required visual fields, concreteness rules, continuity, text overlays |
| `references/specs/prompt-framework.md` | Generic shot-prompt template and how to adapt it to a generator |
| `references/specs/vocabulary.md` | Camera moves, shot sizes, transitions, mood words |
