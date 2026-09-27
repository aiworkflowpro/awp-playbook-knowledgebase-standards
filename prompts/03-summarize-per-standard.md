# 03 · Same standard, different agent

- **Purpose:** hand the same standard to two different agents and models. Both answer in the same
  five sections, because the structure comes from the standard, not from the model.
- **In the video:** 01 Concept, "change the model or the person; the standard stays".

## As shown in the video

```text
Summarize competitors.md per standards/report-format.md
```

## Use it in your own project

Use the `report-format.md` from [prompt 02](02-report-per-standard.md). `competitors.md` is your
own notes about the products you compete with; it is not part of this repo. Save the report
from prompt 02 as `competitors.md`, or create a short file like this one:

```markdown
# Competitors

- Notion AI: notes app with an AI assistant built in. https://www.notion.com/product/ai
- Obsidian: local Markdown notes with plugins. https://obsidian.md
- Mem: AI notes that organize themselves. https://get.mem.ai
```

Then run the prompt in two agents (for example Claude Code and Codex) from the same project
folder and compare the section headings of the two answers.

```text
Working directory: your project root. Run pwd first and cd there if you are not in it.

Summarize competitors.md per standards/report-format.md
```
