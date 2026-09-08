# Step 0 · Setup

> Paste everything below the line into your agent.

---

**Where you are** — nowhere yet. This step creates the working directory for Episode 3
(Standards That Make Agents Consistent). Run it from wherever you keep your projects.

After this step, `awp-playbook-knowledgebase-standards/` is the working directory for every
later step, and two folders inside it do different jobs:

- `course/` is storage — the step prompts, the finished examples, and the standard packages.
  Read from it, never write into it.
- `knowledge-base/` is your workspace. Everything you produce goes there.

## 1 · Clone the repo

```
git clone https://github.com/aiworkflowpro/awp-playbook-knowledgebase-standards
cd awp-playbook-knowledgebase-standards
```

Everything from here is relative to this folder.

## 2 · Fill the workspace

This episode continues the knowledge base from Episode 2. Pick the line that matches you:

```
# Working directory: awp-playbook-knowledgebase-standards/ — run `pwd` first and `cd` there if you are not in it.

# A · You did Episode 2 — bring your own
cp -r ../awp-playbook-knowledgebase-skeleton/knowledge-base/. ./knowledge-base/

# B · You skipped Episode 2 — start from the finished E02 result shipped here
cp -r knowledge-base-starter/. ./knowledge-base/
```

Either way you now have a real knowledge base with eight folders. Step 4 reads
`knowledge-base/brand/` to build the trailer Skill, so this cannot be skipped.

## 3 · Copy the standards into the workspace

The three standards this episode teaches live in `course/reference/`. Copy them in:

```
# Working directory: awp-playbook-knowledgebase-standards/ — run `pwd` first and `cd` there if you are not in it.
cp -r course/reference/* knowledge-base/standards/
```

The `*` matters: it copies the three package folders themselves, so they land **flat** inside
`knowledge-base/standards/` next to what Episode 2 already put there — not nested under a
`reference/` folder.

Then tell your agent:

```
Working directory: awp-playbook-knowledgebase-standards/ — run `pwd` first and `cd` there if you are not in it.
All paths below are relative to that folder.

I copied the three packages from course/reference/ into knowledge-base/standards/:
awp-meta-authoring-standard/, awp-prompt-writing-standard/, and awp-skill-development-standard/.

knowledge-base/standards/CLAUDE.md already exists — Episode 2 wrote it. Update it, do not
overwrite it:

1. Add a row for each of the three new packages to the Subdirectory Index, each with trigger
   words so a future session knows when to load it. Keep every entry that is already there,
   including awp-knowledge-management-standard/.
2. Note in each row that these three are read-only reference material — the agent follows
   them, it does not edit them.
3. Add one changelog line, following the format already in the file.
```

**Why copy at all?** Because `course/` is storage and `knowledge-base/` is where the agent
works. A rule the agent follows every day belongs in the workspace, not in a course folder it
has to reach outside for. From Step 2 on, every prompt reads the standards from
`knowledge-base/standards/`. The originals stay in `course/reference/` — that is the
source this copy came from, and where a fresh copy comes from if you ever need one.

## 4 · Confirm

Print `tree knowledge-base/ -L 2` and tell me what you see. You should have eight folders, and
`standards/` holding four packages side by side at the same level:

```text
knowledge-base/standards/
├── awp-knowledge-management-standard/   ← Episode 2 left this here
├── awp-meta-authoring-standard/         ← new, used by Step 2
├── awp-prompt-writing-standard/                 ← new, used by Step 3
└── awp-skill-development-standard/                  ← new, used by Step 4
```

If you see a `reference/` folder inside `standards/`, the copy went one level too deep — the
packages must sit flat. Fix it before moving on, or Steps 2, 3, and 4 will not find their
standards.

Do not read any standard files yet — just set up and confirm.
