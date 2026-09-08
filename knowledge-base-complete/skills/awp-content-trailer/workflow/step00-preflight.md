# Step 00: Preflight

> **Executor**: Main Agent (+ shell checks)
> **Input**: Skill folder, `config/default.yaml`, channel name
> **Output**: `-` (writes `preflight.md` into the run folder)

## Execution Instructions

1. Read `config/default.yaml`.
2. Decide the run keyword: the channel name in slug form (lowercase, hyphens), for example `AI Workflow Pro` → `ai-workflow-pro`.
3. Create the run folder: `{output_root}{keyword}-{YYYYMMDD}/`. Stop if the output root is not writable.
4. Check the video generator: when `generator.enabled` is true, probe `generator.host` once. When it does not respond, stop and report. When `generator.enabled` is false, record run mode = **prompts-only**.
5. Check ffmpeg with `ffmpeg -version`. Record found / missing. ffmpeg is required only for full mode (steps 07-08).
6. Check the reference files under `references/` exist.
7. Write `preflight.md` into the run folder with: run mode, generator status, ffmpeg status, and the resolved run folder path.
8. Do not call a paid generation API in this step. Do not write business output yet.

## Validation Checkpoints

| ID | Check | Pass Standard |
|----|-------|---------------|
| 00-a | Output root writable | Run folder was created |
| 00-b | Mode decided | `preflight.md` states `full` or `prompts-only` |
| 00-c | Generator consistent | enabled=true → host responded; enabled=false → prompts-only recorded |
| 00-d | References present | `references/specs/` and `references/rules/` files exist |

## Next Step

→ `Step 01: Intake`
