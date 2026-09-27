# Changelog

All notable changes that affect people using this repository are recorded here.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the
project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

The first public release will be tagged `v1.0.0`, together with the video tag
`video-knowledgebase-standards`, on the day the video is published.

### Fixed

- The repo no longer refers to other videos or repos: no episode number, no previous/next links,
  no "you need Episode 2". Everything here works on its own.
- The trailer Skill reads its brand folder and output folder from `config/default.yaml` everywhere,
  so changing `brand_dir` is enough.
- README says next to the Quick Start that the trailer Skill stops at shot prompts; rendering needs
  your own video generator.
- Prompt 20 names the step 02 gate and runs steps 07 and 08 only with a generator; prompt 19 asks
  the agent to build the Skill from the standard rather than copy the example.
- The trailer Skill starts every run in a new folder (`{channel}-{date}-{time}`) and never overwrites an
  earlier run, and prompt 19 asks the Skill you build for the same, and to run in prompts-only mode when no
  generator is configured; prompts 19 and 20 say to remove the example copy so the Skill you built is the one
  that runs.
- Prompts 05 and 12 run the control in an empty folder next to your project, so the installed
  standards cannot leak into it.
- Prompt 12 has the agent save its control answer for the audit; prompt 14 copies the rubric into
  the project itself. No step asks you to save or copy files by hand.
- Prompt 08 step 3 adds the AGENTS.md row only if the agent has not registered the standard already.
- Prompt 08 asks for the purge rule the video describes: a quarantine list of at least 30 obvious
  directions before any research.
- Prompt 01 shows how to point an agent that does not load Skills on its own at the Skill file.
- The trailer Skill is renamed `awp-content-trailer-generating`, the four-part name the Skill
  standard requires (it was `awp-content-trailer`).
- Manual install prints a reminder when `AGENTS.md` already has a `## Standards` section.
- Prompts 13 and 14 say to create `standards/prompts/` when it is missing.
- Prompts 01 and 19 and the trailer example say where the Skill goes for Claude Code, Codex and
  other agents.
- Manual install no longer skips an existing `CLAUDE.md` (it now adds `@AGENTS.md` as the first
  line) and no longer duplicates an existing `## Standards` section; safe to run again.
- Quick Start keeps the repo cloned next to your project, so every prompt from the video stays
  at hand, as the video describes; the `AGENTS.md` snippet points your agent to it.
- Prompt 03 explains where `competitors.md` comes from and gives a short example.
- FAQ explains why a local run stops at step 06; the trailer example shows how to change its length.
- Prompts 06, 07 and 16 say they run in the same session as the prompt before them.
- Prompt 20 and the trailer Skill example state what this repo runs (prompts-only mode) and that
  rendering video needs a generator you set up yourself.
- The Skill standard no longer points to private home-directory paths; run data and preflight
  state live in the Skill's own run folders.

### Changed

- Restructured from a follow-along course (`course/`, `knowledge-base-starter/`,
  `knowledge-base-complete/`) into a toolkit you install into your own project. The video is an
  animated explainer, so there is no recorded start and end state to follow.
- Paths map as follows:
  - `course/reference/` → `standards/`
  - `course/steps/` → `prompts/` (one file per prompt shown in the video)
  - `course/examples/02-cognitive-discipline.md` →
    `examples/cognitive-discipline/awp-cognitive-discipline-standard/` (the version shown on screen)
  - `course/examples/03-*` → `examples/prompt-comparison/`
  - `knowledge-base-complete/skills/awp-content-trailer/` →
    `examples/channel-trailer-skill/awp-content-trailer-generating/`
  - `knowledge-base/`, `knowledge-base-starter/`, `knowledge-base-complete/` → removed; install
    the standards into your own project instead (README, Quick Start)
- The prompt comparison example now uses the run shown in the video: an eight-part prompt of
  165 lines, outputs of 222 lines (A) and 100 lines (B). The expected scores follow the video's
  illustrative example, 13/30 and 28/30, instead of 17/30 and 29/30.
- The meta-standard now names `standards/` as the standards folder instead of `specs/`.
- The Skill context-loading standard uses `rg` instead of a private knowledge-base CLI.
- The cognitive discipline standard's references point to files that exist in the three
  standard packages.

### Added

- `template/AGENTS.snippet.md`: a section for your `AGENTS.md` that tells your agent which
  standard to read for which task.
- `prompts/`: every prompt typed on screen, word for word, each with a version for your own
  project.
- `AGENTS.md` as the single instruction file for agents; `CLAUDE.md` and `GEMINI.md` import it.
- `playbook.yaml` with video, chapter and series metadata.

### Removed

- `course/steps/00-setup` to `04-skill-spec`, and the three knowledge-base snapshots. The
  Episode 2 knowledge base is no longer required.
