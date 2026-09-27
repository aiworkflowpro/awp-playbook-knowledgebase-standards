# Step 02: Brand truth sheet (gate)

> **Executor**: Main Agent
> **Input**: `01-intake.md`
> **Output**: `02-truth-sheet.md` in the run folder — the gate artifact

## Execution Instructions

1. Summarize everything read in step01 into **one page**: one document, plain language, no marketing wording.
2. Structure the truth sheet as the five-field message:
   - **Category** — what the channel is
   - **Promise** — what the viewer gets
   - **Proof** — real evidence from the brand facts (hands-on experience, long-term experiments, and similar)
   - **Difference** — what sets it apart
   - **Ask** — the call to action for a 30-second trailer
3. Every claim traces to a source file from the intake record, or is marked `unconfirmed` / `draft`.
4. Add a short "Needs your call" list: every draft, unconfirmed, or undefined fact that affects the creative steps. For example, an undefined visual identity means colors and logo must not be invented — say so here.
5. Present the truth sheet to the user and **stop**. Do not start any creative step (03 and later) until the user confirms. Record the confirmation in the truth sheet.

## Input Files

| File | Source | Description |
|------|--------|-------------|
| `{run_folder}/01-intake.md` | Step 01 output | Brand facts with sources |

## Output Files

| File | Format | Description |
|------|--------|-------------|
| `{run_folder}/02-truth-sheet.md` | Markdown | One-page five-field truth sheet + "Needs your call" list |

## Validation Checkpoints

| ID | Check | Pass Standard |
|----|-------|---------------|
| 02-a | One page | Truth sheet fits one document, five fields present |
| 02-b | Traceable | Every claim has a source or says `unconfirmed` |
| 02-c | Gaps surfaced | "Needs your call" lists every draft or undefined fact |
| 02-d | Gate respected | No creative step ran before user confirmation |

## Next Step

→ `Step 03: Timeline` (only after user confirms the truth sheet)
