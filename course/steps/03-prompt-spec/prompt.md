# Step 3 · Write a Structured Prompt Using the Eight-Part Format

> Paste everything below the line into your agent.

---

**Where you are** — repository `awp-playbook-knowledgebase-standards`, Episode 3 · Standards That Make Agents Consistent.

Run `pwd` first. It must end with `awp-playbook-knowledgebase-standards`. If it does not, `cd` into that
folder before doing anything else; every path below is relative to it.

- `course/` is storage — the step prompts, the finished examples, and the standard packages.
  Read from it, never write into it.
- `knowledge-base/` is your workspace. Everything you produce goes there.
- This step reads the standards from `knowledge-base/standards/`. If
  `awp-prompt-writing-standard/` is not in there, run
  [Step 0](../00-setup/prompt.md) first — it copies the packages in from `course/reference/`.
  Do not read them out of `course/reference/` instead; the workspace copy is the point.

Read `knowledge-base/standards/awp-prompt-writing-standard/prompt-format-eight-part.md` — this is the prompt standard that defines the eight-part format for reusable prompts.

We're going to write a research prompt that compares different ways to organize knowledge for AI agents.

## First: the bad version (no standard)

Run this prompt exactly as written and save the output to `knowledge-base/research/kb-approaches-no-standard.md`:

```
Research how people organize knowledge for AI agents in 2026. Compare different approaches like CLAUDE.md, Cursor rules, Cline memory bank, Notion AI, and file-based knowledge bases. What are the pros and cons of each? Which approach is best for a solo creator who uses multiple AI tools?
```

## Then: the good version (with standard)

Now read `knowledge-base/standards/awp-prompt-writing-standard/prompt-format-eight-part.md` and write a proper eight-part prompt for the same task. Save the prompt itself to `knowledge-base/standards/prompts/kb-approach-comparison.md`.

The prompt must include:
- **Role**: Knowledge Architecture Comparison Analyst (noun-based, outputs a comparison matrix)
- **Task**: Produce a structured comparison matrix covering five AI agent knowledge management approaches
- **Input**: List of approaches to compare + evaluation dimensions (with defaults if the user leaves them blank)
- **Workflow**: Gather documentation → characterize each approach → evaluate on six fixed dimensions → summarize best-for scenarios. If a search tool is available, use it; if not, fall back to training knowledge and mark uncertain claims.
- **Output spec**: Three sections — approach summary table, six-dimension comparison matrix (each cell has a rating + one-sentence justification + source), best-for summary (one bullet per approach)
- **Six evaluation dimensions**: Structure control, Agent context loading, Standard composability, Multi-model portability, Knowledge persistence, Multi-agent collaboration
- **Refusal scenarios**: Fewer than two approaches, non-knowledge-management systems, requests to pick a "winner"

Run the eight-part prompt and save the output to `knowledge-base/research/kb-approaches-with-standard.md`.

## Compare

Show me both outputs side by side. Point out the differences in:
1. **Structure consistency** — does the output follow a fixed format, or does the agent invent its own?
2. **Dimension coverage** — are all approaches evaluated on the same dimensions?
3. **Source citations** — can you verify the claims?
4. **Objectivity** — does it recommend one approach, or let the matrix speak for itself?

## Score it

"Better" is an opinion until you put a number on it. Run the audit prompt over both outputs:

```
Working directory: awp-playbook-knowledgebase-standards/ — run `pwd` first and `cd` there if you are not in it.
All paths below are relative to that folder.

Read course/examples/03-quality-audit-prompt.md and run it, scoring these two files on its six
dimensions:

- Output A (no standard):   knowledge-base/research/kb-approaches-no-standard.md
- Output B (with standard): knowledge-base/research/kb-approaches-with-standard.md

Give me the per-dimension scores, the gap analysis, and both totals out of 30.
```

Expect roughly **17/30 for the no-standard output and 29/30 for the with-standard one**. The
biggest gaps show up in structure consistency, source traceability, and completeness.

## See the example

`course/examples/` holds the finished version of every piece of this step:

| File | What it is |
|------|-----------|
| [`03-no-standard-prompt.md`](../../examples/03-no-standard-prompt.md) | The control prompt, exactly as pasted above |
| [`03-with-standard-prompt.md`](../../examples/03-with-standard-prompt.md) | The finished eight-part prompt — compare against what your agent wrote |
| [`03-quality-audit-prompt.md`](../../examples/03-quality-audit-prompt.md) | The scoring prompt used above |
| [`03-expected-output-excerpt.md`](../../examples/03-expected-output-excerpt.md) | What the three output sections should look like, plus the expected scores |

## Register what you built

Same rule as Step 2 — index what you added.

```
Working directory: awp-playbook-knowledgebase-standards/ — run `pwd` first and `cd` there if you are not in it.
All paths below are relative to that folder.

Three new files went into my knowledge base. Update the routers:

1. knowledge-base/standards/CLAUDE.md — the prompt landed in a new prompts/ subdirectory.
   Register the subdirectory and the prompt inside it.
2. knowledge-base/research/CLAUDE.md — register both comparison outputs
   (kb-approaches-no-standard.md and kb-approaches-with-standard.md).

Add a changelog line to each.
```

Wait for my go-ahead before moving to Step 4.
