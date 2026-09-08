# Step 07: Assemble

> **Executor**: Main Agent + ffmpeg
> **Input**: `segments/` from step06, `config/default.yaml`
> **Output**: `assembled.mp4` in the run folder
> **Runs only in full mode.** In prompts-only mode this step is not reached.

## Execution Instructions

1. Confirm `generator.enabled` is true and segments exist in `segments/`. Otherwise stop — this step has nothing to assemble.
2. Check ffmpeg is installed (`ffmpeg -version`). Stop if missing.
3. Stitch the segments in order (shot-1 first, shot-5 last) with a crossfade of `assemble.transition_seconds` (0.5 s default) between adjacent segments.
4. Encode to `assembled.mp4` at `trailer.resolution` (1920×1080) and `trailer.fps` (30).
5. Verify the output duration is 30 s ± 1 s (crossfades consume overlap time — check, and trim the end card if needed).

## Input Files

| File | Source | Description |
|------|--------|-------------|
| `{run_folder}/segments/*.mp4` | Step 06 output | Rendered segments |

## Output Files

| File | Format | Description |
|------|--------|-------------|
| `{run_folder}/assembled.mp4` | MP4 (1920×1080) | Segments stitched with transitions |

## Validation Checkpoints

| ID | Check | Pass Standard |
|----|-------|---------------|
| 07-a | Segments present | Five segment files exist before stitching |
| 07-b | Stitched in order | shot-1 … shot-5 sequence in the final file |
| 07-c | Duration | 30 s ± 1 s |
| 07-d | Resolution | 1920×1080 at 30 fps |

## Next Step

→ `Step 08: Music, post and delivery`
