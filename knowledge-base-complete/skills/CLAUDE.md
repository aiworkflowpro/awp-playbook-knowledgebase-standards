# Skills

> Leo's tools — reusable workflow packages that turn one input into a finished deliverable. First skill: the 30-second channel trailer generator.

This area holds Skills: self-contained packages (declaration, config, workflow steps, references) that any agent can run by name. Each Skill is a directory named `awp-*-*/` with a `SKILL.md` entry point. Follow the skill development standard in `standards/` when adding or changing a Skill.

## Skill Index

| Skill | Content | Trigger words |
|-------|---------|---------------|
| `awp-content-trailer/` | Turns a channel name into a 30-second trailer package — reads brand facts from `brand/`, then produces a truth sheet (with a gate), timeline, voiceover script, visual mapping, and shot prompts | trailer, channel trailer, promo video, brand video, 30-second trailer |

## Rules

- Skills live here, not in the course repo — this folder is Leo's to keep.
- Read the Skill's `SKILL.md` first, then the step file for the step you are on.
- Never edit a Skill while it is running a step.

## Change Log

| Date | Change |
|------|--------|
| 2026-09-06 | First Skill added — `awp-content-trailer/`, the 30-second channel trailer generator |
