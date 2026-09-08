# KB Approach Comparison Report

*Output of the eight-part prompt `kb-approach-comparison-v1`. Approaches compared: CLAUDE.md, Cursor rules, Cline memory bank, Notion AI, file-based knowledge bases. Context: solo creator who uses multiple AI tools. Sources retrieved 2026-09-06 via web search.*

## Section 1 · Approach summary table

| Approach | What it is | Home tool | How knowledge loads | Who updates it |
|----------|-----------|-----------|---------------------|----------------|
| CLAUDE.md | A Markdown instruction file at the project root that an agent reads every session | Claude Code (Anthropic) | Auto-injected into context at session start | Human edits by hand |
| Cursor rules | Rule files (`.cursor/rules`, legacy `.cursorrules`) that attach to files by glob pattern | Cursor editor | Auto-attached when matching files are opened; `alwaysApply` rules always load | Human, in the repo |
| Cline memory bank | A fixed set of Markdown files (core + context) that build on each other | Cline (and community clones) | Read on demand at task start; updated as the agent works | Agent self-maintains under instruction |
| Notion AI | Q&A / retrieval over a hosted Notion workspace (pages, wikis, databases) | Notion | Query-time retrieval (RAG); coding agents need connectors to reach it | Human, in Notion |
| File-based knowledge base | Your own folders of plain Markdown that agents read through pointers/indexes | Any tool | On-demand file reads driven by pointers, indexes, or imports | Human (or agent under a discipline rule) |

## Section 2 · Six-dimension comparison matrix

**Legend**: rating 5 = strong by design, 1 = weak by design. Every cell: rating — one-sentence justification — source. Sources keyed below the table.

| Approach | Structure control | Agent context loading | Standard composability | Multi-model portability | Knowledge persistence | Multi-agent collaboration |
|----------|-------------------|----------------------|------------------------|-------------------------|------------------------|---------------------------|
| CLAUDE.md | 3/5 — free-form prose with no enforced schema, but conventions like `@imports`, 150-line guidance, and directory-level files add optional order — [BetterClaw] | 5/5 — injected into context automatically at every session start, with the cost that everything in it is always loaded — [HumanLayer] | 4/5 — supports `@imports` and layered directory files, so it composes cleanly with other Markdown standards — [HumanLayer][r/ClaudeCode] | 3/5 — native to Claude Code only; its generic twin AGENTS.md is read by 30+ tools, so the content ports but the file name does not — [BetterClaw] | 3/5 — lives in git so it survives sessions, but it updates only when a human edits it, so it goes stale without maintenance — [Claude Code Best Practices] | 3/5 — many agents read the same file easily, but only Claude Code reads it and there is no protocol for agents to write to it — [BetterClaw] |
| Cursor rules | 4/5 — rules carry frontmatter (description, globs, `alwaysApply`) and a fixed precedence order (Team → Project → User), so application is deterministic — [Cursor Docs] | 4/5 — selective glob attachment keeps most rules out of context until relevant, though many small files can still stack up — [Cursor Docs][Steve Kinney] | 3/5 — rules can point at shared files, and Cursor reads skills from `.claude/skills` and `.agents/skills`, but the rules format itself stays Cursor-flavored — [Steve Kinney] | 2/5 — rules load only inside Cursor; they do not travel to other agents or editors — [Cursor Docs] | 3/5 — version-controlled files in the repo, but human-maintained, and stale rules silently mislead unless pruned — [Steve Kinney] | 2/5 — Team Rules support shared human teams, but the format is not designed for several different AI agents to co-write — [Cursor Docs] |
| Cline memory bank | 5/5 — strongest enforced schema of the five: required core files with a fixed hierarchy (`projectbrief.md` foundation, files that build on each other) — [Cline Docs] | 3/5 — the agent reads core files on demand at task start instead of auto-injection, so loading depends on following the memory-bank instructions — [Cline Docs][cline GitHub] | 3/5 — the files are plain Markdown in the repo so other tools can read them, but the fixed Cline file set is a convention other agents do not know — [cline GitHub] | 2/5 — a Cline/community convention; Claude, GPT, or Gemini agents will not know to read `memory-bank/` unless told — [Cline Docs] | 5/5 — designed to persist and stay current across sessions because the agent rewrites its own files as it works (self-documenting) — [Cline Docs][cline_docs] | 3/5 — files live in a shared repo and the agent writes them, which suits git-based sharing, but multi-agent write conflicts are unconfirmed — [cline GitHub] |
| Notion AI | 4/5 — Notion databases, properties, and templates enforce field-level organization well for humans — [eesel] | 2/5 — AI answers by retrieval at query time, and coding agents cannot read the workspace natively; connectors are read-only and sync-limited — [eesel][Macha] | 2/5 — content is locked inside a closed workspace; the API/connectors reach out read-only (up to 72-hour syncs) instead of composing with open file standards — [eesel] | 2/5 — knowledge is trapped behind Notion access even though Notion AI itself can run several models (community reports Sonnet 4.5 / GPT-5 options) — [r/Notion] | 4/5 — a durable hosted workspace with good ownership and review workflows, though content drifts without designated owners — [eesel] | 2/5 — Notion's own AI is a read-and-answer layer; acting or co-writing across several AI agents requires third-party bridges — [Macha][r/Notion] |
| File-based knowledge base | 2/5 — no enforced structure at all; order comes only from the author's own naming and index conventions — [HumanLayer] | 3/5 — nothing auto-loads, so the agent reads only what pointers say, which is context-efficient but demands hand-built navigation — [HumanLayer][r/ClaudeCode] | 5/5 — plain Markdown composes with every standard, skill, and import mechanism, and converters exist between formats — [BetterClaw] | 5/5 — Markdown and the AGENTS.md convention are read by 30+ tools (Claude Code, Cursor, Copilot, Codex, Gemini CLI), so knowledge travels — [BetterClaw] | 4/5 — version-controlled and permanent, but stays accurate only while someone (human or a disciplined agent) keeps it updated — [HumanLayer] | 4/5 — shared plain files in a repo let many agents read and git arbitrates writes, though nothing prevents blind overwrites — [BetterClaw][cline GitHub] |

**Sources** (retrieved 2026-09-06):
- [Cursor Docs] cursor.com/docs/rules — official Cursor rules documentation.
- [Cline Docs] docs.cline.bot/best-practices/memory-bank — official Cline memory bank guide.
- [cline GitHub] github.com/cline/cline/docs/prompting/cline-memory-bank.mdx — Cline repo documentation.
- [cline_docs] github.com/nickbaumann98/cline_docs — community custom-instructions library (memory bank).
- [HumanLayer] humanlayer.dev/blog/writing-a-good-claude-md — context-engineering guidance for CLAUDE.md.
- [BetterClaw] betterclaw.io/blog/agents-md-best-practices — AGENTS.md research across 2,500+ repositories (2026).
- [Claude Code Best Practices] iwoszapar.com/p/claude-code-best-practices — CLAUDE.md scope and maintenance.
- [Steve Kinney] stevekinney.com/courses/ai-development/cursor-rules — cross-agent skills and rules guidance.
- [eesel] eesel.ai/blog/notion and /notion-ai-knowledge-hub — Notion AI knowledge hub guides (2025).
- [Macha] getmacha.com/blog/macha-notion-integration-turn-wiki-into-agent-knowledge — Notion as live agent knowledge source.
- [r/Notion] reddit.com/r/Notion — community reports on Notion AI quality and models (2024–2026).
- [r/ClaudeCode] reddit.com/r/ClaudeCode — community practice: keep CLAUDE.md minimal and point to files.

## Section 3 · Best-for summary

- **CLAUDE.md** — best when you live inside Claude Code and want zero-setup memory, because it auto-loads every session; keep it short and point to other files so context stays cheap.
- **Cursor rules** — best when you do most work in Cursor on a codebase with clear file boundaries, because glob-attached rules give relevant context exactly when it is needed.
- **Cline memory bank** — best for long, evolving projects where the agent should keep its own state across sessions, because the enforced file set and self-maintenance stop drift — at the cost of a single-tool convention.
- **Notion AI** — best when knowledge must be human-friendly and shared with people first, and agents only need to ask questions over it; it is a retrieval layer, not agent memory.
- **File-based knowledge base** — best for a solo creator jumping between many AI tools, because plain Markdown is the only format every agent reads, and thin per-tool pointers (a short CLAUDE.md, a rules file) can all point back to one source of truth.

For the stated context — a solo creator using multiple AI tools — the matrix favors a **file-based knowledge base as the single source of truth with short tool-specific pointer files**, since it is the only approach that scores top on both portability (5/5) and composability (5/5) while still allowing auto-load where a tool supports it. Cline's memory bank pattern scores equally high overall by keeping knowledge persistent, and can be layered onto the file base as an updating discipline. No single approach is best in every situation; the matrix is the comparison.
