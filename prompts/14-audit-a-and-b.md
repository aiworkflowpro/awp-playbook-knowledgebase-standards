# 14 · Score A and B with one rubric

- **Purpose:** turn "B looks better" into a number. Both outputs are scored on six dimensions,
  five points each, 30 in total.
- **In the video:** 03 Prompt standard, "read the difference across six dimensions". The scores
  on screen, 13/30 for A and 28/30 for B, are an illustrative example. Your scores will differ
  from run to run; the gap should show up in structure consistency, dimension coverage and
  source traceability.

## As shown in the video

```text
Audit A and B with the same rubric. Score each out of 30.
```

## Use it in your own project

The rubric is a prompt of its own:
[`quality-audit-prompt.md`](../examples/prompt-comparison/quality-audit-prompt.md). The prompt
below copies it into your project from the clone next to it.

```text
Working directory: your project root. Run pwd first and cd there if you are not in it.

If standards/prompts/quality-audit-prompt.md is missing, copy it there from
../awp-playbook-knowledgebase-standards/examples/prompt-comparison/quality-audit-prompt.md.
Then read standards/prompts/quality-audit-prompt.md and run it. Score these two files on its six
dimensions:

- Output A (no standard):   research/kb-approaches-no-standard.md
- Output B (with standard): research/kb-approaches-with-standard.md

Give me the per-dimension scores, the gap analysis, and both totals out of 30.
```

For a fairer score, run the audit in a new session or with a different model than the one that
wrote A and B. An agent grading its own work tends to be generous (see
[prompt 06](06-self-critique.md)).
