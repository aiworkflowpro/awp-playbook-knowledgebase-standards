# 04 · Setup, as filmed

- **Purpose:** installs the three standards. **Superseded:** the video was made with an earlier
  layout of this repo (`course/`, `knowledge-base-starter/`). That layout no longer exists, so
  this prompt will not work as written.
- **In the video:** 01 Concept, the setup demo in Pi with DeepSeek V4 Flash.

## As shown in the video

```text
Run Step 0 of the course: clone github.com/aiworkflowpro/awp-playbook-knowledgebase-standards, fill knowledge-base/ from the Episode 2 starter, copy the three standards into knowledge-base/standards/, then show tree -L 2.
```

## Use this instead

Use the [Quick Start prompt in the README](../README.md#quick-start). It copies `standards/`
into the project you are working in and adds a short section to your `AGENTS.md` that tells
your agent when to read each standard. You do not need any other video, repo or starter knowledge base.

| In the video | In this repo now |
|---|---|
| `course/reference/*` | `standards/` |
| `course/steps/` | `prompts/` |
| `knowledge-base-starter/`, `knowledge-base/` | removed; you install into your own project |
| `knowledge-base-complete/skills/awp-content-trailer/` | `examples/channel-trailer-skill/awp-content-trailer-generating/` |
