# Visual Mapping Rules

Rules for mapping each voiceover line to one concrete visual (used by step05).

## The four required fields

Every shot must name all four. If a field cannot be filled, the shot is not ready.

| Field | Means | Good | Bad |
|-------|-------|------|-----|
| **Subject** | One named thing, person, or screen in the shot | "a person's hands typing at a laptop" | "a creator" |
| **Action** | What the subject does — an editing verb | "typing, then the cursor races across the screen" | "working on content" |
| **Environment** | Where the shot happens | "a tidy home office at night, monitor glow only" | "a nice setting" |
| **Camera** | One camera move or shot size from the vocabulary spec | "slow push-in from a wide shot" | "dynamic footage" |

## Concreteness rules

- **"Nice footage" is not a visual.** Every element must be something a video generator can draw without guessing.
- Name materials, light, and objects. "Monitor glow" and "paper notes pinned on the wall" render; "vibe" does not.
- One action per shot. Two actions split the shot; keep the second action for its own segment.
- Mood is one extra line (lighting + tone), not a replacement for any of the four fields.

## Continuity rules

- The five shots must look like one channel: same treatment, same text style, consistent subject handling.
- On-screen text: max four words per frame. Most video models cannot render long or small text. One word, two at most, is safest for titles.
- Do not invent brand visuals. If the truth sheet says the visual identity is undefined or "Needs your call", use a plain, neutral treatment (clean office, screen, hands-on work) and keep the flag in the shot notes.

## Text overlay rule

- Overlays carry the message word, not the sentence. The voiceover says the sentence; the frame shows one short word or short phrase.
- Never ask a generator to render code, URLs, or more than four words.
