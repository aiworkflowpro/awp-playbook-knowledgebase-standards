# 12 · Research prompt A, without a standard

- **Purpose:** the control prompt of the A/B comparison. The output is long prose with no fixed
  sections (222 lines in the run shown).
- **In the video:** 03 Prompt standard, the A/B demo, card "A · No standard".

## As shown in the video

The video shows a shortened form of the control prompt:

```text
Research how people organize knowledge for AI agents in 2026. Compare CLAUDE.md, Cursor rules, Cline memory bank, Notion AI, and file-based knowledge bases.
```

## Use it in your own project

Run the full control prompt exactly as written, with no working-directory line and no
standard: adding anything would change the control. Start the agent in an empty folder next to your project, not in the project itself: once the
standards are installed, your project's `AGENTS.md` makes the agent read them, and the control
would no longer be a control. For example `mkdir ../no-standards && cd ../no-standards`. When it has answered, send this in the
same session so the answer is saved for the audit in prompt 14:

```text
Save your previous answer, unchanged, to ../{your project}/research/kb-approaches-no-standard.md.
```

The control prompt:

```text
Research how people organize knowledge for AI agents in 2026. Compare different approaches like CLAUDE.md, Cursor rules, Cline memory bank, Notion AI, and file-based knowledge bases. What are the pros and cons of each? Which approach is best for a solo creator who uses multiple AI tools?
```

Example output: [`kb-approaches-no-standard.md`](../examples/prompt-comparison/kb-approaches-no-standard.md).
