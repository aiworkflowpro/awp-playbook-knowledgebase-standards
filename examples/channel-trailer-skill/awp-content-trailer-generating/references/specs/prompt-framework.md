# Shot Prompt Framework

How step06 compiles one shot prompt per segment, and how to adapt it to a specific video generator.

## Generic shot-prompt template

Each of the five prompts is a self-contained block. Video generators do not share context between shots, so every block repeats the look.

```text
SHOT {N} — {time window}, 16:9

STYLE ANCHOR:
{one-sentence look of the channel; from the truth sheet; skip any undefined
visual identity rather than inventing one}

SUBJECT:
{subject from the visual mapping}

ACTION:
{action from the visual mapping}

ENVIRONMENT:
{environment from the visual mapping}

CAMERA:
{camera move or shot size from the vocabulary}

MOOD:
{lighting + tone from the visual mapping}

ON-SCREEN TEXT (max 4 words):
{short overlay, or "none"}

NEGATIVE:
{what to avoid: invented logo, brand colors, long text, camera drift}
```

## Compile rules

- Fill every block from `05-visual-mapping.md`. Do not rewrite the mapping in looser words — the mapping is already concrete.
- Repeat the style anchor in every block so the five shots match.
- When the truth sheet flagged an undefined visual identity, the negative line says "no logo, no fixed brand colors" and the style anchor stays plain.

## If you have no video generator

Prompts-only mode is the default, and its output is a real deliverable — not a half-finished run.

What you hold at the end of step06: a truth sheet your brand facts were checked against, a 30-second timeline, five voiceover lines that fit it, five concrete visuals, and five compiled shot prompts. Four things you can do with that, none needing a GPU:

| Use it for | How |
|---|---|
| **Paste into any hosted generator** | Runway, Kling, Pika, Luma, Veo — one block per shot, in order. Adapters below |
| **Hand to someone else** | A videographer or motion designer reads subject / action / environment / camera and knows what to shoot. That is a shot list |
| **Shoot it yourself** | The five visuals are described concretely enough to film on a phone |
| **Keep the script alone** | The voiceover plus timeline is a usable 30-second script even if no picture ever gets made |

The expensive part of a trailer is deciding what it says and what each second shows. That decision work is what steps 01 through 06 produce, and it is hardware-free.

## Generator adapters

Prompts-only mode stops after compiling. Full mode maps each block onto the configured generator's input format:

| Generator | What to map |
|-----------|-------------|
| MiniMax / Veo / Kling / Runway style tools | Shot list or scene list format; one entry per shot, keep the four fields |
| Seedance / API text-to-video | Single text prompt per shot: fold style anchor + four fields + mood into one paragraph, negatives at the end |
| Storyboard tools | Keep the blocks as scene cards |

Rules for any adapter:
- The four fields (subject, action, environment, camera) survive the mapping verbatim.
- Text rendering stays at max four words.
- Confirm the adapter accepts 16:9 and ~6 s per shot before submitting.

### Worked example: block to single-paragraph prompt

Tools that take one text box per shot need the block folded into a paragraph. Order matters — style first so it colours everything after it, negatives last.

Block form:

```text
SHOT 2 — 00:06–00:12, 16:9
STYLE ANCHOR: flat editorial illustration, two-colour, generous white space
SUBJECT: a single folder icon splitting into eight labelled folders
ACTION: the split happens once, cleanly, then holds
ENVIRONMENT: empty off-white field, no desk, no room
CAMERA: locked wide, no movement
MOOD: even daylight, calm
ON-SCREEN TEXT: eight folders
NEGATIVE: no logo, no brand colours, no long text, no camera drift
```

Folded form:

```text
Flat editorial illustration, two-colour, generous white space. A single folder
icon splits into eight labelled folders — the split happens once, cleanly, then
holds. Empty off-white field, no desk, no room. Locked wide shot, no camera
movement. Even daylight, calm. Small caption reads "eight folders".
Avoid: logos, brand colours, long text, camera drift. 16:9, 6 seconds.
```

Nothing was dropped. The four fields are all still there, just as prose.

## Quality checklist (before output)

- [ ] Five blocks, one per segment, in order
- [ ] Every block has style anchor + four fields + mood + text + negatives
- [ ] Blocks share one look
- [ ] No invented logo, colors, or claims
- [ ] On-screen text ≤ 4 words everywhere
