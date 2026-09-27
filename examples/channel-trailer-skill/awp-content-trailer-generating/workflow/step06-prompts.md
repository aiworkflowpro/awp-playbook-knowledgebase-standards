# Step 06: Compile prompts and generate

> **Executor**: Main Agent (+ generator adapter in full mode)
> **Input**: `05-visual-mapping.md`, `config/default.yaml`
> **Output**: `06-prompts/shot-1.txt` … `shot-5.txt`; in full mode, rendered segments

## Execution Instructions

1. Read the visual mapping and the config.
2. Compile **one shot prompt per segment** using the template in `references/specs/prompt-framework.md`. Every prompt carries: subject, action, environment, camera, mood, on-screen text (≤ 4 words), aspect 16:9, and any style anchor from the truth sheet.
3. Keep the five prompts consistent: same channel look, same text style, no invented logo or colors (see truth sheet constraints).
4. Save the five prompts into `06-prompts/shot-1.txt` through `shot-5.txt`.
5. **Submit half** — run only when `generator.enabled` is true:
   - Map the compiled prompts onto the configured adapter's input format
   - Submit each shot, wait for completion, and save the segment files
6. When `generator.enabled` is false (prompts-only mode), **stop here**. Report the script and shot prompts as the deliverables. Do not fabricate a render.

## Input Files

| File | Source | Description |
|------|--------|-------------|
| `{run_folder}/05-visual-mapping.md` | Step 05 output | Concrete visual per line |
| `config/default.yaml` | config | Generator adapter and settings |

## Output Files

| File | Format | Description |
|------|--------|-------------|
| `{run_folder}/06-prompts/shot-{1..5}.txt` | Text | Compiled shot prompts |
| `{run_folder}/segments/*.mp4` | Video | Full mode only: rendered segments |

## Validation Checkpoints

| ID | Check | Pass Standard |
|----|-------|---------------|
| 06-a | Five prompts | shot-1 through shot-5 exist |
| 06-b | Complete fields | Every prompt has subject, action, environment, camera, mood, text |
| 06-c | Consistent | Shots share one look; no invented logo or brand colors |
| 06-d | Mode respected | prompts-only mode stopped after compiling; full mode submitted |

## Next Step

→ `Step 07: Assemble` (full mode only) · prompts-only mode ends here
