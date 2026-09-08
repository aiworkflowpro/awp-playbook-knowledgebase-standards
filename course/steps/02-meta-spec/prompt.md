# Step 2 · Write Your First Standard Using the Meta-Standard

> Paste everything below the line into your agent.

---

**Where you are** — repository `awp-playbook-knowledgebase-standards`, Episode 3 · Standards That Make Agents Consistent.

Run `pwd` first. It must end with `awp-playbook-knowledgebase-standards`. If it does not, `cd` into that
folder before doing anything else; every path below is relative to it.

- `course/` is storage — the step prompts, the finished examples, and the standard packages.
  Read from it, never write into it.
- `knowledge-base/` is your workspace. Everything you produce goes there.
- This step reads the standards from `knowledge-base/standards/`. If
  `awp-meta-authoring-standard/` is not in there, run
  [Step 0](../00-setup/prompt.md) first — it copies the packages in from `course/reference/`.
  Do not read them out of `course/reference/` instead; the workspace copy is the point.

Read `knowledge-base/standards/awp-meta-authoring-standard/std-core-chapter-skeleton.md` — this is the meta-standard that defines how every standard should be written. Every standard follows the same chapter skeleton.

We're going to use the meta-standard to generate a **cognitive discipline standard** — a set of rules that constrains how your agent thinks, not just what it outputs.

## The problem

Ask your agent to find a SaaS product direction for solo AI creators. It will give you obvious answers — AI writing assistant, AI video editor, AI scheduler. You search each one and find mature competitors everywhere.

This is not the agent being dumb. LLM output averages the training data. The ideas you think of first are the ones everyone already built.

## Generate the standard

Tell your agent:

```
Working directory: awp-playbook-knowledgebase-standards/ — run `pwd` first and `cd` there if you are not in it.
All paths below are relative to that folder.

Read knowledge-base/standards/awp-meta-authoring-standard/std-core-chapter-skeleton.md — this is the meta-standard.

I need a cognitive discipline standard. It should:
- List the cognitive weaknesses of LLMs that cause them to give obvious, low-quality answers
- Define rules that force the agent to overcome each weakness
- Include quality criteria with pass/fail thresholds

The standard should follow the meta-standard skeleton exactly. Save it as a package at
knowledge-base/standards/awp-cognitive-discipline-standard/ — a CLAUDE.md package index plus the
standard itself. It belongs in my knowledge base, not in this repo.
```

Look at the output. It should have:
- A list of specific cognitive weaknesses (at least 5)
- A matching set of rules — each rule addresses one weakness
- Quality criteria with quantifiable thresholds
- The same chapter structure as every other standard

## See the example

[`course/examples/02-cognitive-discipline.md`](../../examples/02-cognitive-discipline.md) is the
finished version of this step: seven named weaknesses (W1–W7), nine rules (R1–R9) each mapped
to the weakness it overcomes, and the nine-chapter skeleton the meta-standard prescribes.

Yours will not match it word for word — the meta-standard fixes the shape, not the content.
Compare the shape: does every rule point at a weakness, and does every quality criterion have a
number you could actually check?

## Now prove it works

Use the cognitive discipline standard you just generated:

```
Working directory: awp-playbook-knowledgebase-standards/ — run `pwd` first and `cd` there if you are not in it.
All paths below are relative to that folder.

Read knowledge-base/standards/awp-cognitive-discipline-standard/.

Find a SaaS product direction for solo AI content creators — one-person businesses who create YouTube videos, blog posts, and newsletters using AI tools. Follow the cognitive discipline standard strictly.
```

Compare this output to a plain "find me a SaaS direction" request. The difference should be visible:
- The plain request gives you obvious, already-taken directions
- The standard-driven request drops the obvious directions, finds real pain from user complaints, and validates through adversarial rounds

Save the comparison to `knowledge-base/research/cognitive-discipline-test.md`.

## Register what you built

Your knowledge base carries a rule from Episode 2: a file is not part of the knowledge base until the index says it is. Two files just landed, so two routers need a line.

```
Working directory: awp-playbook-knowledgebase-standards/ — run `pwd` first and `cd` there if you are not in it.
All paths below are relative to that folder.

Two things went into my knowledge base. Update the routers:

1. knowledge-base/standards/CLAUDE.md — register the awp-cognitive-discipline-standard/ package in the
   Subdirectory Index, with trigger words so the agent knows when to load it.
2. knowledge-base/research/CLAUDE.md — register cognitive-discipline-test.md so the topic
   is findable.

Add a changelog line to each, following the format already in the file.
```

Skip this and the file still sits on disk, but the next agent reads the router, does not see it, and works as if the standard never existed. An unindexed file is an invisible file.

Wait for my go-ahead before moving to Step 3.
