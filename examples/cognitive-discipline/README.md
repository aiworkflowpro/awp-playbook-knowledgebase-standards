# Example: a standard written with the meta-standard

> Example output from one recorded run. Your result will differ: the meta-standard fixes the
> shape of a standard, not its content.

This is what [prompt 08](../../prompts/08-write-cognitive-discipline-standard.md) produced in the
video: a cognitive discipline standard that names seven predictable weaknesses of language models
and gives one rule and one checkable threshold for each, followed by a test of the standard on a
real product question.

| File | What it is |
|---|---|
| [`awp-cognitive-discipline-standard/CLAUDE.md`](awp-cognitive-discipline-standard/CLAUDE.md) | Package index |
| [`awp-cognitive-discipline-standard/std-cognitive-weakness-discipline.md`](awp-cognitive-discipline-standard/std-cognitive-weakness-discipline.md) | The standard (318 lines): weaknesses W1–W7, rules R1–R7, thresholds, quality criteria, checklist |
| [`cognitive-discipline-test.md`](cognitive-discipline-test.md) | The test (224 lines): the same question answered without and with the standard; 8 candidates entered the kill round, 3 survived |

How to compare your own result: does every rule point at exactly one weakness, does every
threshold contain a number you could check, and does the file follow the same chapter order as
the three standards in [`standards/`](../../standards/awp-meta-authoring-standard/CLAUDE.md)?

To use this standard as it is, copy the `awp-cognitive-discipline-standard/` folder into your
project's `standards/` folder and add a row for it to your `AGENTS.md`.
