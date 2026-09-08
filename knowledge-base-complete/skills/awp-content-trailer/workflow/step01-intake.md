# Step 01: Intake

> **Executor**: Main Agent
> **Input**: channel name (user), `brand_dir` files
> **Output**: `01-intake.md` in the run folder

## Execution Instructions

1. Take the channel name from the user input.
2. List the files in `brand_dir` (config). If the folder has a subfolder whose slug matches the channel name, use that subfolder instead.
3. Read every file found — never read the folder directly; list first, then read each file.
4. Record the source list (file names) in the intake record.
5. Extract and record:
   - Channel name (from input)
   - What the brand is (category)
   - Who it is for (audience)
   - What the brand voice sounds like
   - Any facts marked draft, "to confirm", or "not defined" — keep their status; do not invent
   - Positioning and differentiation if present
6. Fill the fixed fields from config: duration 30 s, 5 segments, 16:9, 1920×1080.
7. Missing information is written as **unconfirmed** in the intake record. Do not invent it.

## Input Files

| File | Source | Description |
|------|--------|-------------|
| `knowledge-base/brand/*.md` | knowledge base | The channel's brand facts |

## Output Files

| File | Format | Description |
|------|--------|-------------|
| `{run_folder}/01-intake.md` | Markdown | Channel name, brand facts, source list, fixed trailer fields |

## Validation Checkpoints

| ID | Check | Pass Standard |
|----|-------|---------------|
| 01-a | Channel name present | Intake record starts with the channel name |
| 01-b | Sources read | Every brand file is listed in the source list |
| 01-c | Gaps flagged | Draft or missing facts say `unconfirmed`; nothing invented |
| 01-d | Fixed fields set | Duration, segments, aspect, resolution present |

## Next Step

→ `Step 02: Brand truth sheet (gate)`
