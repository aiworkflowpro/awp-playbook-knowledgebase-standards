# Step 05: Visual mapping

> **Executor**: Main Agent
> **Input**: `04-script.md`, `02-truth-sheet.md`
> **Output**: `05-visual-mapping.md` in the run folder

## Execution Instructions

1. Read the script and the confirmed truth sheet.
2. Map **each voiceover line to exactly one visual**. Do not describe "nice footage" — name concrete elements.
3. Follow `references/rules/visual-mapping-rules.md`. Every row needs four required fields:
   - **Subject** — what is in the shot (one named thing, person, or screen)
   - **Action** — what the subject does (an editing verb, not a state)
   - **Environment** — where the shot happens
   - **Camera** — one camera move or shot size from `references/specs/vocabulary.md`
4. Add a mood line per shot (lighting + tone) and any on-screen text (max four words per frame — see the vocabulary spec).
5. Respect the truth sheet's "Needs your call" list: if visual identity is undefined, do not invent colors or a logo; use a plain treatment and flag it.
6. Ensure visual continuity: the five shots feel like one channel, not five random clips.

## Input Files

| File | Source | Description |
|------|--------|-------------|
| `{run_folder}/04-script.md` | Step 04 output | The five timed lines |
| `{run_folder}/02-truth-sheet.md` | Step 02 output | Message and constraints |

## Output Files

| File | Format | Description |
|------|--------|-------------|
| `{run_folder}/05-visual-mapping.md` | Markdown | One row per line: subject, action, environment, camera, mood, text |

## Validation Checkpoints

| ID | Check | Pass Standard |
|----|-------|---------------|
| 05-a | One visual per line | Five rows, matching the five lines |
| 05-b | Concrete | Every row has all four required fields with named elements |
| 05-c | Vocabulary used | Camera and action terms come from the vocabulary spec |
| 05-d | Text limited | No on-screen text over four words per frame |
| 05-e | No invention | Undefined brand visuals are flagged, not invented |

## Next Step

→ `Step 06: Compile prompts and generate`
