# 13 · Research prompt B, written with the prompt standard

- **Purpose:** the same research, but the agent first writes an eight-part prompt from the
  prompt standard, saves it, then runs it. In the run shown, the prompt is 165 lines and the
  output is 100 lines in fixed sections.
- **In the video:** 03 Prompt standard, the A/B demo, card "B · Eight-part prompt" (new session).

## As shown in the video

```text
Read prompt-format-eight-part.md and write an eight-part prompt for the same research. Save it, then run it.
```

## Use it in your own project

Start a new session so the agent does not reuse the answer from prompt A.

```text
Working directory: your project root. Run pwd first and cd there if you are not in it.

Read standards/awp-prompt-writing-standard/prompt-format-eight-part.md. Write an eight-part
prompt for this research task: compare how people organize knowledge for AI agents in 2026.

The prompt must include:
- Role: Knowledge Architecture Comparison Analyst; its output is a comparison matrix.
- Core task: a structured comparison of five approaches: CLAUDE.md, Cursor rules, Cline memory
  bank, Notion AI, file-based knowledge bases.
- Input: the approaches and the evaluation dimensions, with defaults when left blank.
- Workflow: gather documentation, characterize each approach, rate each one on six fixed
  dimensions, then summarize who each approach suits. Use a search tool if one is available;
  otherwise use training knowledge and mark uncertain claims as unverified.
- Six dimensions: structure control, agent context loading, standard composability,
  multi-model portability, knowledge persistence, multi-agent collaboration.
- Output standard: an approach summary table, a matrix where every cell has a rating, one
  sentence of justification and a source, and one best-for bullet per approach.
- Refusal scenarios: fewer than two approaches, systems that are not knowledge management,
  requests to name a single winner.

Save the prompt to standards/prompts/kb-approach-comparison.md (create standards/prompts/ and
research/ if they are missing), then run it and save the output to
research/kb-approaches-with-standard.md.
```

Example prompt: [`kb-approach-comparison.md`](../examples/prompt-comparison/kb-approach-comparison.md).
Example output: [`kb-approaches-with-standard.md`](../examples/prompt-comparison/kb-approaches-with-standard.md).
