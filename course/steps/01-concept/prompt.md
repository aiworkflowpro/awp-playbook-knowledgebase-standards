# Step 1 · Why Standards Matter

> This step is conceptual — no files to create, no output to compare. Read, understand, then move to Step 2.

---

**Where you are** — repository `awp-playbook-knowledgebase-standards`, Episode 3 · Standards That Make Agents Consistent.

Run `pwd` first. It must end with `awp-playbook-knowledgebase-standards`. If it does not, `cd` into that
folder before doing anything else; every path below is relative to it.

- `course/` is storage — the step prompts, the finished examples, and the standard packages.
  Read from it, never write into it.
- `knowledge-base/` is your workspace. Everything you produce goes there.

## What is a standard?

A standard is a written rule that your AI agent reads and follows every time it works.

Think of it as an employee handbook. A new employee without a handbook guesses how to name files, what format to use, what tone to write in. They guess differently every time. An employee with a handbook produces consistent work from day one.

Your agent is the same. Without standards, it guesses. With standards, it follows your rules.

## Why does this work?

Four data points:

1. **McKinsey 2025**: Teams with prompt standards had 40% fewer AI output failures in production.
2. **arXiv research (SPRIG, 2410.14826)**: Same model, same task — restructuring the prompt raised accuracy from 52% to 85%.
3. **Anthropic official blog**: A configured Claude versus an unconfigured Claude is "night and day."
4. **GitHub internal + AWS Kiro**: After adopting standards, code rewrites dropped 10×. Features that took 40 hours shipped in 8.

## What we'll build in this episode

Three layers, each building on the last:

| Layer | Standard | What it controls |
|-------|----------|-----------------|
| 1 | Meta-standard | How to write any standard (the standard for standards) |
| 2 | Prompt standard | How to write prompts that produce consistent, high-quality output |
| 3 | Skill standard | How to build a reusable workflow package that gets better every run |

## Where the standards live

Step 0 copied all three into `knowledge-base/standards/`, flat, one package each. Every step
from here reads them from there, inside the workspace — your agent never reaches outside it to
find its own rules. In Episode 2 you built the knowledge base; from now on the rules live inside it.

| Layer | Package | The file the step tells your agent to read |
|-------|---------|-------------------------------------------|
| 1 | `knowledge-base/standards/awp-meta-authoring-standard/` | `std-core-chapter-skeleton.md` |
| 2 | `knowledge-base/standards/awp-prompt-writing-standard/` | `prompt-format-eight-part.md` |
| 3 | `knowledge-base/standards/awp-skill-development-standard/` | `skill-core-development-standard.md` |

If those three packages are not inside `knowledge-base/standards/`, Step 0's copy never ran —
go back and run it now. Step 2 will not find the meta-standard otherwise.

When you're ready, move to Step 2.
