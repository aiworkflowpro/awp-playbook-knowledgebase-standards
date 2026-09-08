# Expected Output Excerpt

When you run the **with-standard prompt** (eight-part format), your output should have three sections:

## Section 1: Approach Summary Table

| Approach | Core design | Knowledge format | Loading method |
|----------|-------------|------------------|----------------|
| Claude CLAUDE.md | Per-directory config files | Markdown in git | Agent reads on startup |
| Cursor Rules | Single .cursorrules at root | Plaintext rules | Editor injects into context |
| Cline Memory Bank | Six auto-generated files | Markdown, agent-managed | Agent rebuilds on session start |
| Notion AI Workspace | Hosted wiki + relational DB | Notion pages | API / MCP connector |
| File-system KB | Plain folders + Markdown | Files in git | Agent reads on demand via CLAUDE.md pointers |

## Section 2: Six-Dimension Comparison Matrix

Each cell should have a rating (Strong / Partial / Weak / None) plus a one-sentence justification with a source citation.

Example cell: `**Strong** — Per-directory CLAUDE.md files create a hierarchy that mirrors the knowledge base structure (getunblocked.com).`

## Section 3: Best-For Summary

Five bullets, one per approach. No overall recommendation — the matrix speaks for itself.

Example: `File-system KB is best for multi-model, long-term solo work because the open AGENTS.md standard plus git versioning gives portability and persistence with no vendor lock-in.`

## Quality Audit Score

When you run the **quality-audit prompt** to compare no-standard vs with-standard output:

- No-standard output: expect ~17/30
- With-standard output: expect ~29/30
- Biggest gaps: structure consistency, source traceability, completeness
