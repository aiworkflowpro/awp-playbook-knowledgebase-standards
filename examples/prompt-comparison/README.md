# Example: the same research with and without a prompt standard

> Example output from one recorded run. Your result will differ: models, search results and
> scores change from run to run.

This is the A/B comparison from the video ([prompts 12–14](../../prompts/12-research-without-standard.md)).
Both runs research the same question. A uses a plain request; B first writes an eight-part
prompt from the [prompt standard](../../standards/awp-prompt-writing-standard/prompt-format-eight-part.md),
then runs it.

| File | What it is |
|---|---|
| [`prompt-a-no-standard.md`](prompt-a-no-standard.md) | A: the control prompt, one paragraph |
| [`kb-approaches-no-standard.md`](kb-approaches-no-standard.md) | A: the output, 222 lines of prose with no fixed sections |
| [`kb-approach-comparison.md`](kb-approach-comparison.md) | B: the eight-part prompt the agent wrote, 165 lines |
| [`kb-approaches-with-standard.md`](kb-approaches-with-standard.md) | B: the output, 100 lines in fixed sections |
| [`quality-audit-prompt.md`](quality-audit-prompt.md) | The rubric: six dimensions, five points each |

## Scores

The scores in the video are an illustrative example of how the rubric reads:

| Dimension | A · no standard | B · eight-part prompt |
|---|---|---|
| Structure consistency | 3 | 5 |
| Dimension coverage | 2 | 5 |
| Source traceability | 2 | 4 |
| Objectivity | 2 | 5 |
| Actionability | 2 | 4 |
| Completeness | 2 | 5 |
| **Total** | **13/30** | **28/30** |

When you run the audit yourself, expect different numbers. What should hold is the direction:
B scores higher on structure consistency, dimension coverage and source traceability, because
the prompt fixes the sections, the dimensions and the citation rule before the agent starts.
