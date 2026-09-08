---
document_id: awp-skill-development-standard/advanced/skill-platform-constraint-limits
language: en
publication: public
source_revision: 2
title: "Platform Constraints and Tool Limits"
purpose: Hard Claude Code tool limits and Skill installation path rules
category: Standard
prerequisites: []
see_also:
  - skill-script-file-standard.md
  - skill-step-document-standard.md
---

# Platform Constraints and Tool Limits

> **Output**: The platform adaptation layer for a Skill: installation paths, tool calls, parallel strategy, and runtime choice. Each Skill has one platform configuration.
> **Responsibility**: Define hard Claude Code tool limits, Skill installation path rules, and platform boundaries.

---

## 0. Design Principles

**Why platform constraints are needed**: A Skill runs on Claude Code and is limited by Claude Code tools, the context window, and the execution model. Platform constraints help a Skill run reliably inside these boundaries. They do not limit creativity.

| Principle | Description |
|------|------|
| ✅ **Constraints create freedom** | Know exactly what the platform does not support and avoid wasted trial and error |
| ✅ **Hard limits cannot be bypassed** | Tool parallelism, context windows, and Hook events are base Claude Code constraints |
| ✅ **Soft limits can adapt** | Model choice, parallel count, and token budget can change with the Skill |

---

## Structure

| # | Question | Section | Core Content |
|---|------|------|---------|
| §0 | Why are platform constraints needed? | Design principles | Constraints create freedom, hard limits cannot be bypassed, soft limits can adapt |
| §1 | Where is a Skill installed? | Skill installation paths | User / project / enterprise paths and precedence |
| §2 | What hard limits do CC tools have? | Hard Claude Code tool limits | Hard constraints for Read/Write/Bash/Glob/Grep/Agent tools |
| §3 | Which constraints must Skill developers know? | Required Skill development constraints | No Agent nesting, Bash output limits, and other development constraints |
| §4 | How is the SKILL.md file named? | SKILL.md filename | Filename case and path rules |
| §5 | How is Token context managed? | Token context limits | Token budget management with a 1M context window |
| §6 | Which tools are forbidden or restricted? | Tool blocklist and restrictions | Forbidden tools and tools allowed only under conditions |
| §7 | How is SubAgent parallelism controlled? | Parallelism details | Limits on SubAgent count and batch size |
| §8 | Which runtimes are supported? | Supported runtimes | Support state for Python/Node/Deno/Bash/Bun |
| §9 | How are Agent Teams used? | Agent Teams | Multi-Agent collaboration framework, experimental |
| §10 | How is Worktree isolation used? | Worktree isolation | Isolated runtime through git worktree |
| §11 | How is Skill safety protected? | Skill safety | Credential isolation, path limits, and other safety rules |
| §12 | What differs across platforms? | Cross-platform limits | Differences among macOS/Linux/Windows |
| §13 | How do Rules relate to Skills? | Relationship between Rules and Skills | How Rules files and Skills work together |
| §14 | How does the CC toolchain work with a Skill? | CC toolchain and Skills | Hook, MCP, and Agent tool integration |

---

## 1. Skill Installation Path Rules

### 1.1 Default Installation Path (User Level)

**Default installation location**: `~/.claude/skills/<skill-name>/`

> **Important**: Install every Skill in the user-level directory by default so it works across all projects.

| System | Expanded Path |
|------|---------|
| macOS | `/Users/<username>/.claude/skills/` |
| Linux | `/home/<username>/.claude/skills/` |
| Windows | `C:\Users\<username>\.claude\skills\` |

### 1.2 Other Scopes (Optional)

| Scope | Path | Use |
|--------|------|---------|
| **User level (default)** | `~/.claude/skills/<name>/` | Personal, shared across all projects |
| **Project level** | `.claude/skills/<name>/` | Shared by a team in one repository |
| **Enterprise level** | See the table below | Deployed by an organization administrator |

### 1.3 Enterprise Paths by Operating System

| System | Path |
|------|------|
| macOS | `/Library/Application Support/ClaudeCode/` |
| Linux/WSL | `/etc/claude-code/` |
| Windows | `C:\Program Files\ClaudeCode\` |

### 1.4 Precedence

```
Enterprise (managed) > Project (.claude/) > User (~/.claude/) > Plugin
```

When Skills share a name, the higher-precedence Skill replaces the lower-precedence one.

### 1.5 Command Paths

| Scope | Path |
|--------|------|
| User level | `~/.claude/commands/<name>.md` |
| Project level | `.claude/commands/<name>.md` |

---

## 2. Hard Claude Code Tool Limits

### 2.1 Read Tool

| Limit | Default | Can Be Changed |
|------|--------|--------|
| Line truncation | 2,000 lines | `offset` + `limit` arguments |
| Single-line truncation | 2,000 characters | No |

**Note**: Read large files in parts with the `offset` and `limit` arguments. Images, PDFs, and Jupyter Notebooks are supported.

### 2.2 Edit Tool

| Rule | Description |
|------|------|
| **Read first** | An unread file cannot be edited and causes an error |
| Uniqueness | `old_string` must be unique in the file |
| Permission | User approval is required |

### 2.3 Write Tool

| Rule | Description |
|------|------|
| **Read an existing file first** | Read a file before overwriting it |
| Permission | User approval is required |

### 2.4 Bash Tool

| Limit | Default | Environment Variable |
|------|--------|----------|
| Output truncation | **30,000 characters** | `BASH_MAX_OUTPUT_LENGTH` |
| Truncation method | Middle truncation, keeping the start and end | — |
| Default timeout | 120,000ms (2 minutes) | `BASH_DEFAULT_TIMEOUT_MS` |
| Maximum timeout | 600,000ms (10 minutes) | `BASH_MAX_TIMEOUT_MS` |

**Truncation**: Beyond 30K characters, the start and end stay and the middle is removed. Full output is saved in the `tool-results/` directory.

### 2.5 MCP Tools

| Limit | Default | Environment Variable |
|------|--------|----------|
| Output ceiling | **25,000 tokens** | `MAX_MCP_OUTPUT_TOKENS` |
| Warning threshold | 10,000 tokens | — |
| Startup timeout | — | `MCP_TIMEOUT` |

### 2.6 Task Tool (SubAgent)

| Limit | Description |
|------|------|
| Concurrency | ⚪ No more than 4 per turn; a 1M context leaves enough room |
| Returned content | TaskOutput results enter the main conversation context |
| Nesting | A SubAgent cannot call Task again |

> **1M context**: After 1M became GA, four parallel Agents returning 10K tokens each add 40K. This is easy to control within a 1M context.

### 2.7 Skill Tool

| Limit | Default | Environment Variable |
|------|--------|----------|
| Character budget | **2% of the context window** | `SLASH_COMMAND_TOOL_CHAR_BUDGET` |
| Fallback value | 16,000 characters | — |

**Explanation**: The total character budget for Skill descriptions loaded into context scales with the window. When it is exceeded, some Skill descriptions are left out. Use `/context` to see the warning. An environment variable can override the budget.

---

## 3. Required Skill Development Constraints

### 3.1 File Operation Practices

| Case | Correct | Wrong |
|------|---------|---------|
| Edit a file | Read, then Edit | Edit directly |
| Overwrite a file | Read, then Write | Write directly |
| Large file | Read in parts with offset+limit | Read it all at once |
| Search content | Prefer the Grep tool when available; otherwise use Bash `rg`/`grep` | Ignore existing tools and refuse Bash rg/grep |

### 3.2 SubAgent Prompt Constraints

A SubAgent follows the same tool limits:
- For a large data batch, keep each batch below 25k tokens.
- A SubAgent cannot nest a Task call.

### 3.3 Deep-Thinking Mode (ultrathink)

Include the keyword `ultrathink` in SKILL.md to enable Claude deep-thinking mode.

**How to trigger it**:

```markdown
---
name: awp-complex-analysis
description: Complex data analysis.
---

# Complex Analysis Skill

> This Skill enables ultrathink deep-thinking mode to protect analysis quality.

## Analysis Steps
...
```

**Use cases**:

| Case | Guidance |
|------|------|
| Simple data processing | Does not need ultrathink |
| Complex logical analysis | ⚪ Enable |
| Code review | ⚪ Enable |
| Architecture design | ⚪ Enable |

### 3.4 Advanced Features (Officially Supported)

The following features are officially supported by Claude Code. See `../skill-core-file-declaration.md` for full configuration:

| Feature | Use | Configuration Location |
|------|------|----------|
| **hooks field** | Skill lifecycle hooks: PreToolUse / PostToolUse / Stop | Frontmatter `hooks:` |
| **HTTP Hooks** | POST JSON to a URL instead of a Shell command | Frontmatter `hooks:` |
| **PostCompact Hook** | Fires after Context Compaction finishes | Global hooks configuration |
| **Dynamic context injection** | The `` !`command` `` syntax runs a shell command at load time | SKILL.md body |
| **Variable replacement** | `$ARGUMENTS` / `$ARGUMENTS[N]` / `${CLAUDE_SESSION_ID}` / `${CLAUDE_SKILL_DIR}` | SKILL.md body |

**Hook types**:
- **Shell Hooks**: Run local Shell commands; stable.
- **HTTP Hooks** (⚪ for connecting remote services): POST JSON to a URL.

**Full Hook event matrix (21 events, checked 2026-06-12)**:

| Category | Event | When It Fires | Available in Skill |
|------|------|---------|:---:|
| Lifecycle | `SessionStart` | Session starts/resumes | ❌ |
| Lifecycle | `SessionEnd` | Session ends | ❌ |
| Input | `UserPromptSubmit` | User submits a prompt | ❌ |
| Tool | `PreToolUse` | Before a tool call; may block it | ✅ |
| Tool | `PostToolUse` | After a tool call | ✅ |
| Tool | `PostToolUseFailure` | A tool fails | ✅ |
| Permission | `PermissionRequest` | Permission dialog appears | ❌ |
| Notification | `Notification` | A notification is sent | ❌ |
| Agent | `SubagentStart` | SubAgent starts | ✅ |
| Agent | `SubagentStop` | SubAgent finishes | ✅ |
| Agent | `Stop` | Main Agent finishes its response | ✅ |
| Teams | `TeammateIdle` | A teammate is about to become idle; experimental | ❌ |
| Teams | `TaskCompleted` | A task is marked complete; experimental | ❌ |
| Context | `InstructionsLoaded` | CLAUDE.md/rules load | ❌ |
| Context | `ConfigChange` | A configuration file changes | ❌ |
| Worktree | `WorktreeCreate` | A Worktree is created | ❌ |
| Worktree | `WorktreeRemove` | A Worktree is removed | ❌ |
| Compaction | `PreCompact` | Before context compaction | ❌ |
| Compaction | `PostCompact` | After context compaction | ❌ |
| MCP | `Elicitation` | MCP asks the user for input | ❌ |
| MCP | `ElicitationResult` | Response to MCP input | ❌ |

> **Skill-level hooks**: Only events marked ✅ may be configured under `hooks:` in SKILL.md frontmatter. Configure global events in `settings.json`.

**Hook exit codes**:
| Exit Code | Meaning |
|--------|------|
| 0 | Continue, with no feedback |
| 1 | Error; record it in the log |
| 2 | Block the action and inject stderr into the conversation as feedback |

---

## 4. SKILL.md Filename

| Rule | Description |
|------|------|
| Filename | Must be `SKILL.md` in uppercase |
| Case-sensitive | `../skill-core-development-standard.md` is not recognized |
| Location | Must be in the Skill root directory |

---

## 5. Token Context Limits

### 5.1 Context Window

> **Model matrix checked 2026-06-12**. Models change quickly. Before use, treat the options shown by Claude Code `/model` and the official documentation as current.

| Model | Alias | Context | Maximum Output | Description |
|------|------|--------|---------|------|
| Claude Fable 5 | `fable` | **1M** | **128k** | Mythos class, strongest reasoning at present |
| Claude Opus 4.8 | `opus` | **1M** | **128k** | Strong reasoning, first choice for coding |
| Claude Sonnet 4.6 | `sonnet` | **1M** | **64k** | Balanced first choice |
| Claude Haiku 4.5 | `haiku` | **200k** | **8k** | Lightweight and fast |
| Explicit Opus 1M | `opus[1m]` | **1M** | **128k** | Explicitly enables the 1M window |
| Explicit Sonnet 1M | `sonnet[1m]` | **1M** | **64k** | Explicitly enables the 1M window |
| Mixed OpusPlan | `opusplan` | **1M** | **128k** | Opus in Plan, Sonnet in Execute |

**Effort levels**, set through `/effort` or together with frontmatter `model`:

| Level | Description | Use |
|------|------|---------|
| `low` | Fastest, least reasoning | Simple format conversion |
| `medium` | Balanced, default | Daily tasks |
| `high` | Deep reasoning | Complex analysis, architecture design |
| `max` | Strongest reasoning; Opus only, current session | Very hard problems |

**Model choice in a Skill**: The frontmatter `model` field may use an alias or full ID. The SubAgent `model` argument works the same way.

> **1M is GA**: The 1M-token context window for Opus and Sonnet has been released for general availability, not beta, with no price premium over standard pricing.
>
> **Practical effect**: With a 1M context, Compaction events fall by about 15%. An Agent can run for hours with little risk of context overflow. Fine-grained token micromanagement is no longer needed.

### 5.2 Token Use Reference

| Source | Estimated Use | Description |
|------|---------|------|
| System Prompt | ~5k tokens | Fixed |
| SKILL.md | ~2–5k tokens | Loaded when triggered |
| Step document | ~1–3k tokens/step | Loaded as needed (⚪) |
| File read | ~0.1–25k tokens/read | Large files can be read in parts |
| SubAgent return | Accumulates | Minimal return (⚪ optional) |
| Conversation history | Accumulates | A 1M window leaves enough room |

> **The 1M change**: There is no longer a need to count every token. Keep good practices such as progressive loading and short returns because they make code clearer, not because they are required to prevent overflow.

### 5.3 Context Awareness (Model Self-Awareness)

Claude 4.x and Fable 5 models can sense how much context remains.

| Context Remaining | Model Behavior |
|-----------|---------|
| > 30% | Runs normally |
| 10–30% | May shorten output on its own |
| < 10% | May refuse long output to avoid overflow |

**Practical effect with 1M**: At 1M, 10% is 100K tokens, far more than most complete Skill runs use. Automatic shortening is rarely triggered in practice.

---

## 6. Tool Blocklist and Restrictions

### 6.1 Forbidden Operations

The following operations are **forbidden** in a Skill:

| Tool/Operation | Reason | Alternative |
|-----------|------|---------|
| Nested `Task` call | A SubAgent cannot call Task again | Use a flat design |
| Interactive `Bash` command | Interactive input cannot be handled | Use noninteractive arguments |
| Long-running `Bash` | Timeout limit is 10 minutes | Run in the background with `run_in_background` + poll with TaskOutput |
| Large `MCP` return | 25k-token limit | Pagination + limit |

### 6.2 MCP Tool Limits

| MCP Tool | Limit | Solution |
|----------|------|---------|
| Every MCP | 25k-token output ceiling | Return pages |
| Search tools | Result-count limit | Set the `limit` argument |
| Fetch tools | Page-size limit | Fetch only the needed content |

---

## 7. Parallelism Details

### 7.1 SubAgent Parallel Constraints

> Platform hard limits are defined here. For exact multi-turn and serial scheduling, see `skill-step-document-standard.md` §6, the only source.

| Constraint | Value | Description |
|------|-----|------|
| **⚪ Standard parallel count** | **≤4** | With a 1M context, up to four SubAgents may run in one turn |
| **Context accumulation** | N × return | Returns from N Agents accumulate and stay easy to control within 1M |

> **Update for the 1M era**: The old standard of ≤2 was designed for a 200K context. With 1M, four Agents returning 10K each use only 40K, or 4% of the context.

### 7.1.1 SubAgent (Task) Capability Matrix

| Argument | Description | Default |
|------|------|--------|
| `subagent_type` | Agent type: general-purpose / Explore / Plan, and others | Required |
| `model` | Model choice: sonnet / opus / haiku | Inherits from parent |
| `run_in_background` | Runs in the background without blocking the main Agent | `false` |
| `max_turns` | Maximum API turns, preventing an infinite loop | Unlimited |
| `resume` | Pass an agent ID to resume its earlier execution context | — |
| `isolation` | Set to `"worktree"` to run in a separate git worktree | — |
| `mode` | Permission mode: acceptEdits / bypassPermissions / plan, and others | `"default"` |
| `team_name` | Join a named Team | — |

> **Agent Teams**: For multi-Agent collaboration, combine `TeamCreate` + `SendMessage` + `TaskList`. See §9.

### 7.1.2 Custom Subagent Definition (.claude/agents/)

Define a custom Agent type in `.claude/agents/{name}.md`. A Skill can refer to it through `context: fork` + `agent:`, or the Task tool can use it through `subagent_type`.

**Frontmatter fields**:

| Field | Required | Description |
|------|:---:|------|
| `name` | ✅ | Unique identifier: lowercase + hyphens |
| `description` | ✅ | When to delegate to this Agent |
| `tools` | ❌ | Available tools; inherits from the parent when omitted |
| `disallowedTools` | ❌ | Tools removed from the inherited list |
| `model` | ❌ | sonnet / opus / haiku / full ID |
| `permissionMode` | ❌ | Permission handling |
| `maxTurns` | ❌ | Maximum API turns, preventing an infinite loop |
| **`skills`** | ❌ | **Skills to preload**, with full content injected at startup |
| `mcpServers` | ❌ | Dedicated MCP servers |
| `hooks` | ❌ | Agent-level lifecycle hooks |
| `memory` | ❌ | Persistent memory scope |
| `background` | ❌ | Always run in the background: `true` |
| `isolation` | ❌ | Set to `worktree` to run in a separate git worktree |

**Skill preloading**:

```yaml
---
name: security-reviewer
description: Security review specialist
model: opus
skills:
  - owasp-rules
  - code-patterns
---

You are a security review specialist. Use the preloaded OWASP rules and code patterns...
```

> **Difference from `context: fork`**:
> - `context: fork` on the Skill side: Skill content becomes the Agent task, and the Agent type supplies the system prompt.
> - The `skills` field on the Agent side: Full Skill content enters the Agent context, and the Agent decides when to use it.

### 7.2 Multi-Turn Execution Strategy

**When multi-turn execution is needed**:

With a 1M context, six Agents returning 10K each use only 60K tokens, or 6%. This is easy to control. Multi-turn execution has changed from "required to prevent overflow" to "an optional efficiency choice."

```
Parallel strategy with a 1M context:
┌──────────────────────────────────────────────────────────────┐
│ Batches ≤10 → all parallel, up to 4 per turn                  │
│ Batches >10 → several turns, 4 per turn, in order             │
│ Tokens per batch >50K → serial across turns, limit buildup    │
└──────────────────────────────────────────────────────────────┘
```

### 7.3 Parallelism by Scenario

> **Matches the multi-turn scheduling rules in skill-step-document-standard.md §6**

| Case | Batches | Tokens per Batch | Parallelism | Reason |
|------|-------|-----------|--------|------|
| Light evaluation | ≤5 | <5k | **All parallel** | No pressure with 1M |
| Standard batch processing | 6–10 | 5–10k | **4** | Four per turn moves work forward quickly |
| Large batch processing | >10 | 5–10k | **4** | Four per turn across several turns |
| High-token task | Any | >50k | **Serial** | Control accumulation |
| Very large reference document | Any | >50k reference | **2** | Each SubAgent loads its own copy |

### 7.4 Parallel Execution Notes

| Note | Description |
|---------|------|
| Several Tasks in one message | Parallel Agents must be called in **the same message** |
| Independence | Parallel Agents cannot depend on each other's data |
| Failure isolation | One Agent failure does not affect the other Agents |
| Result collection | Collect TaskOutput one by one when present; otherwise a script or the main Agent combines results |
| State update | Update progress.json once after collection finishes |

---

## 8. Supported Runtimes

### 8.1 Runtime Support Matrix

| Runtime | State | Minimum Version | Reference Version | Package Manager |
|--------|------|---------|---------|---------|
| **Python** | Supported | 3.9 | 3.11+ | uv |
| **Node.js** | Supported | 18 | 20+ | pnpm |
| **Deno** | Supported | 1.40 | Latest | Built in |
| **Bash** | Supported | 4.0 | 5.0+ | - |
| **Bun** | Experimental | 1.0 | Latest | bun |

### 8.2 Runtime Selection Guide

| Runtime | Use | Advantage |
|--------|---------|------|
| **Python** | API calls, data processing, file operations | Large ecosystem, type safety |
| **Node.js** | Frontend builds, npm ecosystem tools | Async IO, npm ecosystem |
| **Deno** | TypeScript scripts, secure sandbox | Built-in tools, security model |
| **Bash** | System commands, simple scripts | Available by default, no installation |

### 8.3 Package Manager Rules (AWP Only)

| Runtime | Package Manager | Install Command | Run Command |
|--------|---------|---------|---------|
| Python | **uv** | `uv sync` | `uv run python` |
| Node.js | **pnpm** | `pnpm install` | `pnpm exec` |
| Deno | Built in | - | `deno run` |
| Bash | - | - | `bash` |

**Forbidden (AWP Only)**:

| Forbidden | Reason | Use Instead |
|------|------|------|
| pip | No lockfile, slow | uv |
| poetry | Complex, slow | uv |
| npm | Slow, bloated node_modules | pnpm |
| yarn | Not the main choice | pnpm |

### 8.4 Cross-Runtime Compatibility

When a Skill needs several runtimes, declare runtime requirements in SKILL.md with a table: component / runtime / version / purpose.

---

## 9. Agent Teams (Experimental)

> **State**: Research preview. Enable it with the environment variable `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`.

### 9.1 Difference from SubAgent

| Dimension | SubAgent (Task Tool) | Agent Teams |
|------|---------------------|-------------|
| Communication | Reports only to the main Agent | Teammates message each other directly |
| Coordination | Main Agent manages all work | Shared task list supports self-coordination |
| Context | Shares main-session context | Each has an independent context window |
| Token cost | Low, with result summaries | High, because each is a full Claude instance |
| Use | Batch data processing, parallel tasks | Cross-module work, complex multi-role tasks |

### 9.2 Architecture

- **Team Lead**: Main Session. Creates the team, assigns tasks, and coordinates work.
- **Teammates**: Independent Claude Code instances. Claim tasks and communicate with each other.
- **Task List**: Shared at `~/.claude/tasks/{team-name}/`.
- **Team Config**: `~/.claude/teams/{team-name}/config.json`.

### 9.3 Uses in a Skill

| Case | ⚪ Preferred Method | Reason |
|------|---------|------|
| Batch data processing | **SubAgent** | Lightweight, low cost |
| Multi-module feature development | **Agent Teams** | Independent contexts and cross-file collaboration |
| Parallel work inside one step | **SubAgent** | Simple and direct |
| Multi-Skill orchestration | **Agent Teams** | Each teammate loads a different Skill |

### 9.4 Limits (2026-03)

- Best team size: 3–5 people, with 5–6 tasks each.
- An in-process teammate cannot be resumed.
- Only one team is supported, with no nesting.
- The Lead is fixed and cannot be transferred.

---

## 10. Worktree Isolation

The Agent tool supports the `isolation: "worktree"` argument and runs a SubAgent in a separate git worktree.

### 10.1 Uses

| Case | Description |
|------|------|
| Parallel code changes | Several Agents edit different files at the same time without conflicts |
| Experimental changes | An Agent tries changes in isolation without affecting the main branch |
| Built-in `/batch` Skill | Creates a worktree for each subtask automatically |

### 10.2 Usage

```python
Task(
    subagent_type="general-purpose",
    isolation="worktree",
    prompt="..."
)
```

The Worktree is created automatically in `.claude/worktrees/`. If changes exist when work finishes, the branch name is returned. If there are no changes, it is cleaned up automatically.

---

## 11. Skill Safety

> **Source**: Anthropic's official Agent Skills safety guide.

### 11.1 Basic Rule

**Use Skills only from trusted sources**: Skills you created or Skills from Anthropic. A Skill gives Claude new abilities through instructions and code. A malicious Skill may cause data exposure, unauthorized access, or other security risks.

### 11.2 Audit Checklist

| Check | Description |
|--------|------|
| Review every file | Check SKILL.md, scripts, and resources for unusual patterns |
| External URL calls | A Skill that fetches external data has higher risk because the content may be changed |
| Tool misuse | Check for unexpected file operations or Bash commands |
| Data exposure | Check whether sensitive information could be sent outside |
| Treat a Skill like installed software | Audit it fully before using it in a production system |

### 11.3 AWP Skill Audit Standard

→ **AWP Only**: AWP uses a dedicated security audit Skill (100-point security score + four-level decision). Replace with your own security review process.

---

## 12. Cross-Platform Limits

Skills **do not sync across platforms**:

| Platform | Skill Type | Sync Scope |
|------|-----------|---------|
| **Claude Code** | Filesystem Skill | Personal (`~/.claude/skills/`) or project (`.claude/skills/`) |
| **Claude.ai** | Uploaded Skill (zip) | Personal account, not shared across teams |
| **Claude API** | Uploaded Skill (/v1/skills) | Organization-wide sharing |
| **Agent SDK** | Filesystem Skill | Same as Claude Code |

> Claude Code Skills are fully separate from Claude.ai/API Skills. To use one on several platforms, upload or configure it separately on each platform.

---

## 13. Relationship Between the Rules System and Skills

`.claude/rules/*.md` supports automatic instruction loading by path match and works alongside Skills:

| Mechanism | Loading | Use |
|------|---------|---------|
| **CLAUDE.md** | Always loaded | Project-wide rules |
| **Rules** | Loaded when a file-path glob matches | Path-specific rules, such as React rules for `src/**` |
| **Skills** | Loaded as needed when a task matches | Task-specific abilities and workflows |

**How they work together**: Rules say "which standard applies under this path." Skills say "which method applies to this task."

---

## 14. CC Toolchain and Skills

The following built-in CC commands can work with a Skill during development and runtime:

### 14.1 Development

| Command | Use |
|------|------|
| `/plan` | Enter plan mode and design the Skill structure before coding |
| `/fork [name]` | Fork the conversation and test a Skill change without affecting the main session |
| `/rewind` | Return to a checkpoint after a Skill run fails |
| `/context` | View context use and monitor Skill token consumption |
| `/diff` | View uncommitted Skill file changes |
| `/security-review` | Review the security of code generated by the Skill |

### 14.2 Runtime

| Command | Use |
|------|------|
| `/effort [level]` | Adjust reasoning depth; use high/max for a complex Skill |
| `/fast` | Switch to fast mode and speed up a simple Skill by 2.5x |
| `/compact [focus]` | Compact context by hand during a long Skill run |
| `/btw <question>` | Ask a side question during a Skill run without interrupting the main flow |
| `/loop [interval] /skill` | Run a Skill repeatedly on a schedule; see `skill-design-pattern-library.md` P8 |
| `/batch <instruction>` | Orchestrate parallel tasks automatically instead of splitting them into turns by hand |

### 14.3 Debugging

| Command | Use |
|------|------|
| `/debug [description]` | Read session logs and diagnose a Skill problem |
| `/cost` | View token use for a Skill run |
| `/export [file]` | Export the full conversation from a Skill run |
| `/skills` | List every loaded Skill |

### 14.4 Model Strategy in Skill Frontmatter

```yaml
---
name: complex-analysis
model: opus           # Use Opus for complex tasks
---

---
name: quick-format
model: haiku          # Use Haiku for simple format conversion
---
```

> **Tip**: When the `model` field is omitted, the Skill uses the current session model. The user can adjust it with `/model` or `/effort`.

---

## Checklist

**Installation and paths**:
- [ ] The Skill installation path is ~/.claude/skills/.
- [ ] The SKILL.md filename is uppercase and in the root directory.

**Tools and models**:
- [ ] The model choice is in the supported matrix.
- [ ] Hook events use the correct exit code: 0/1/2.
- [ ] MCP output stays below 25k tokens.

**Parallelism and runtimes**:
- [ ] No more than four SubAgents run in parallel.
- [ ] Agent Teams use is marked experimental.
- [ ] Runtime package managers follow the rule: Python→uv, Node→pnpm.
- [ ] It passes `../../awp-meta-authoring-standard/std-style-language-contract.md`: three-layer language contract / remove jargon / formalize speech / fidelity, clarity, and grace.
