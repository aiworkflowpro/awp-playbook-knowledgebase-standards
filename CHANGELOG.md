# Changelog

All notable changes that affect people using this repository are recorded here.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the
project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-10-19

First public release, tagged `v1.0.0` together with the video tag
`video-knowledgebase-standards`.

### Added

- `standards/`: three standard packages you install into your own project, the meta-standard
  (`awp-meta-authoring-standard`), the prompt standard (`awp-prompt-writing-standard`) and the
  Skill standard (`awp-skill-development-standard`).
- `template/AGENTS.snippet.md`: a section for your `AGENTS.md` that tells your agent which
  standard to read for which task.
- `prompts/`: every prompt typed on screen, word for word, each with a version for your own
  project. Control runs (prompts 05 and 12) start in an empty folder next to your project so the
  installed standards cannot leak into them.
- `examples/cognitive-discipline/`: the cognitive discipline standard shown on screen and its test.
- `examples/prompt-comparison/`: the A/B run from the video, an eight-part prompt against a
  prompt without the standard, with the audit rubric (expected scores 13/30 and 28/30).
- `examples/channel-trailer-skill/`: the `awp-content-trailer-generating` Skill, configured
  through `config/default.yaml`. Without a video generator it runs in prompts-only mode and stops
  at shot prompts; every run writes to a new `{channel}-{date}-{time}` folder.
- Quick Start and a manual install that are safe to run again: an existing `CLAUDE.md` gets
  `@AGENTS.md` as its first line and an existing `## Standards` section is not duplicated.
- `AGENTS.md` as the single instruction file for agents; `CLAUDE.md` and `GEMINI.md` import it.
- `playbook.yaml` with video, chapter and series metadata.
