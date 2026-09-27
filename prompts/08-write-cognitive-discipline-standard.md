# 08 · Write a standard with the meta-standard

- **Purpose:** use the meta-standard to generate a new standard, here a cognitive discipline
  standard that names the agent's own weaknesses and gives one checkable rule for each. Then test
  it on a real question.
- **In the video:** 02 Meta standard, the generation run (Claude Code with DeepSeek V4 Flash).
  The run reads the meta-standard (919 lines), writes a 318-line standard, then a 224-line test.
  Both files are in [`examples/cognitive-discipline/`](../examples/cognitive-discipline/README.md).

## As shown in the video

On screen the standards sit in `knowledge-base/standards/`. After the Quick Start they are in
`standards/` in your project.

```text
Read knowledge-base/standards/awp-meta-authoring-standard/std-core-chapter-skeleton.md — the meta-standard. Write a cognitive discipline standard: one rule per LLM weakness, each with pass/fail thresholds. Save it to knowledge-base/standards/awp-cognitive-discipline-standard/, then test it on a real product question.
```

## Use it in your own project

**1. Generate the standard**

```text
Working directory: your project root. Run pwd first and cd there if you are not in it.
If standards/awp-meta-authoring-standard/ does not exist, stop and tell me to install the
standards first (README, Quick Start).

Read standards/awp-meta-authoring-standard/std-core-chapter-skeleton.md. It is the
meta-standard: the structure every standard follows.

Write a cognitive discipline standard. It should:
- list the cognitive weaknesses of LLMs that lead to obvious, low-quality answers
- give exactly one rule per weakness that forces the agent past it
- give every rule a pass/fail threshold the agent can check
- start with a purge rule: before any research, the agent writes the obvious ideas into a
  quarantine list of at least 30 directions and may not recommend any of them

Follow the meta-standard skeleton exactly. Save it as a package at
standards/awp-cognitive-discipline-standard/: a CLAUDE.md package index plus the standard
itself, with four-part file names.
```

Check the result: at least five named weaknesses, one rule per weakness, a number in every
threshold, a purge rule with a quarantine list of at least 30 directions, and the same chapter
order as the other standards.

**2. Prove it works**

```text
Working directory: your project root. Run pwd first and cd there if you are not in it.

Read standards/awp-cognitive-discipline-standard/ and follow it strictly.

Find a SaaS product direction for solo AI content creators: one-person businesses that make
YouTube videos, blog posts and newsletters with AI tools.

Save the run, including the discard list and the evidence you found, to
research/cognitive-discipline-test.md.
```

Compare it with the answer from [prompt 05](05-brainstorm-without-standard.md).

**3. Register it**

```text
If the Standards table in AGENTS.md has no row for standards/awp-cognitive-discipline-standard/
yet, add one: what it is for and when to read it. If a row already exists, check it says both
and do not add a second one. Keep every existing row.
```

A file that no index points to is a file the next session will not find.
