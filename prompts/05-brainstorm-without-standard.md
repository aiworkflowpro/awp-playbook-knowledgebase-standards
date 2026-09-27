# 05 · Brainstorm without a standard

- **Purpose:** the control. With no standard, the agent answers an open question with the
  average of its training data: an AI writing assistant, an AI video clipper, an AI social media
  scheduler. Every one of them already has mature competitors.
- **In the video:** 02 Meta standard, "from a rule framework to the usual answers".

## As shown in the video

```text
Help me brainstorm and identify a viable niche for a new software startup idea, so I can build a website to generate profit and revenue.
```

## Use it in your own project

Run it exactly as written, in a fresh session with no standards loaded, and save the answer.
Start the agent in an empty folder next to your project, not in the project itself: once the
standards are installed, your project's `AGENTS.md` makes the agent read them, and the control
would no longer be a control. For example `mkdir ../no-standards && cd ../no-standards`.
Then run [prompt 08](08-write-cognitive-discipline-standard.md) and ask the same question again.
