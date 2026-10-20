# 02 · Write a report against a standard

- **Purpose:** a standard works as a checklist. The agent reads it, writes the draft, then checks
  the draft against every item before it answers.
- **In the video:** 01 Concept, "a checklist is a chain of visible actions".

## As shown in the video

```text
Write the competitor report per standards/report-format.md. Check your draft against it before you output.
```

## Use it in your own project

`standards/report-format.md` is a one-page demo standard; it is not part of this repo. Create
your own before you run the prompt. A version close to the one in the video:

```markdown
# Report format

A competitor report covers exactly five aspects, in this order:
1. What the product is
2. Who it is for
3. Pricing
4. Strengths
5. Weaknesses

- Format: Markdown, one H2 per aspect.
- Required content: at least one source link per aspect.
- Length: 200 words or fewer.

Before you output, check the draft against every line above and list the result.
```

```text
Working directory: your project root. Run pwd first and cd there if you are not in it.

Write the competitor report per standards/report-format.md for {competitor}.
Check your draft against it before you output.
```
