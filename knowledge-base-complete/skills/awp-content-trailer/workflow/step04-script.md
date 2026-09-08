# Step 04: Script

> **Executor**: Main Agent
> **Input**: `03-timeline.md`, `02-truth-sheet.md`
> **Output**: `04-script.md` in the run folder

## Execution Instructions

1. Read the confirmed truth sheet and the timeline.
2. Write **one voiceover line per segment** — five lines total, one line per segment, one sentence per line.
3. Follow `references/rules/voiceover-rules.md`:
   - Total ~60 words (hard ceiling 75); per-line budget from the timeline
   - A line must read in its segment window at ~2.5 words per second
   - Plain brand voice from the truth sheet; no hype, no filler
   - Line 1 (hook) states the channel's promise in the first 3 seconds
   - Line 5 (ask) ends with the call to action
   - Fresh phrasing every run — never reuse a line from an earlier run
4. Keep the five-field message flowing through the five lines: category and promise in the hook, proof and difference in the middle, ask at the end.
5. Write each line with its segment number, time window, word count, and a read-time check.

## Input Files

| File | Source | Description |
|------|--------|-------------|
| `{run_folder}/03-timeline.md` | Step 03 output | Time windows and word budgets |
| `{run_folder}/02-truth-sheet.md` | Step 02 output | Confirmed message and voice |

## Output Files

| File | Format | Description |
|------|--------|-------------|
| `{run_folder}/04-script.md` | Markdown | Five timed voiceover lines with word counts |

## Validation Checkpoints

| ID | Check | Pass Standard |
|----|-------|---------------|
| 04-a | Five lines | One per segment, all segments covered |
| 04-b | Fits 30 s | Every line reads within its window at 2.5 words/s |
| 04-c | Budget kept | Total words ≤ 75; per-line budget respected |
| 04-d | Brand voice | No hype or marketing filler; matches truth sheet voice |
| 04-e | Message complete | Category, promise, proof, difference, ask all appear |

## Next Step

→ `Step 05: Visual mapping`
