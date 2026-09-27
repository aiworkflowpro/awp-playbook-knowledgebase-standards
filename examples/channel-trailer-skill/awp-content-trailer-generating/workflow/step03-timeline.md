# Step 03: Timeline

> **Executor**: Main Agent
> **Input**: `02-truth-sheet.md`, `config/default.yaml`
> **Output**: `03-timeline.md` in the run folder

## Execution Instructions

1. Read the confirmed truth sheet and the config beat order: hook → context → value → proof → ask.
2. Split 30 seconds into five segments of ~6 seconds each. Adjust segment lengths only to serve the hook (0-3 s must grab attention) and the ask (last 2-3 s is the end card).
3. For every segment record: time window, beat type, its job in one sentence, and the narration word budget (total 60 words across the five lines; ~12 words per line).
4. Note the rhythm: hook segments move fast (visual change every 2-3 s); proof and ask segments may hold still.
5. Do not write the voiceover lines here. This step plans time and beats only.

## Input Files

| File | Source | Description |
|------|--------|-------------|
| `{run_folder}/02-truth-sheet.md` | Step 02 output | Confirmed five-field message |

## Output Files

| File | Format | Description |
|------|--------|-------------|
| `{run_folder}/03-timeline.md` | Markdown | Five segments with time windows, beats, jobs, word budgets |

## Validation Checkpoints

| ID | Check | Pass Standard |
|----|-------|---------------|
| 03-a | Five segments | Exactly five rows |
| 03-b | Covers 30 s | Windows sum to 30 s with no gaps |
| 03-c | Beat order | hook → context → value → proof → ask |
| 03-d | Word budget | Narration budgets total 60 words or fewer |

## Next Step

→ `Step 04: Script`
