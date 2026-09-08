---
document_id: awp-skill-development-standard/advanced/skill-credential-file-standard
language: en
publication: public
source_revision: 2
title: "Credential File Standard"
purpose: Dual-mode loading and security isolation for a Skill's credentials/ directory
category: Standards
prerequisites:
  - ../skill-core-file-declaration.md
see_also:
  - skill-script-file-standard.md
  - skill-docs-authoring-standard.md
---

# Credential File Standard

> This document defines dual-mode loading and security isolation for a Skill's `credentials/` directory.
> For the Markdown format, section structure, and field names of credential files, follow your knowledge base's credential-file format rules.

> **Source-of-truth boundary**: The authoritative credential access rules—the format contract (§C1 parsing protocol), tool integration contract (§C3), and four-level fallback chain (L0 → L1a → L1b → L2 in §C2)—live in your knowledge base's credential standard, not in this file. This file covers only Skill-specific differences: the Skill view of dual-mode fallback, portable distribution, and Step00 live validation. It does not repeat shared rules from the credential standard.

---

## 0. Design Principles

| Principle | Description |
|------|------|
| ✅ **One Markdown format** | Use the same format as 66 knowledge-base credentials and CLI tools, removing the JSON/MD bridge |
| ✅ **Dual-mode loading** | Distribution mode contains self-contained placeholders; AWP mode automatically falls back to knowledge-base credentials |
| ✅ **Security isolation** | .gitignore excludes credential files from version control |
| ✅ **Real usability checks** | Step00 must run live validation for required credentials. Checking only that a file exists is not enough |

## Structure

| # | Question | Section | Core content |
|---|------|------|---------|
| §1 | What belongs in credentials/? | Directory responsibility | API keys, OAuth tokens, and service account credentials |
| §2 | What format do credential files use? | Credential file format | → `AWP credentials` |
| §3 | How are credentials loaded? | Dual-mode credentials | L0 environment variables → L1 inside the Skill → L2 AWP knowledge base |
| §4 | How do scripts read credentials? | Script credential access | One entry point: `shared/_cred.py:load_cred()` |
| §5 | How are files organized and protected from leaks? | Directory and security | credentials/ structure + .gitignore + setup.md |
| §6 | How do we confirm credentials really work? | live validation | Side-effect-free authentication probes, caching, and failure handling |

## 1. Directory Responsibility

**Purpose**: Store API credentials and keys

**Location**: `credentials/`

**Typical content**: API keys, OAuth tokens, and service account credentials

---

## 2. Credential File Format

> **Full format standard**: follow your knowledge base's credential-file format rules.
>
> Includes: the five-section skeleton (title → authentication information → common configuration → management entry point → usage rules), the standard field-name list, four authentication templates, placeholder rules, and file-naming rules.

**Key points**:
- Use one `*.md` format. Do not use `.json`, `.env`, or `.yaml`
- Use the table `| Item | Value |` and wrap the value in backticks
- Parsing regex: `\|\s*{key}\s*\|\s*` `` ` `` `([^` `` ` `` `]+)` `` ` ``
- Placeholder: `YOUR_*` prefix

---

## 3. Dual-Mode Credentials ([AWP Organization Policy])

### 3.1 Dual-Mode Fallback (Skill View)

> The authoritative definition of the full four-level fallback chain (L0 → L1a → L1b → L2) lives in your knowledge base's credential standard. It is the only source of truth for the whole library. This section explains only how that chain applies to a Skill.

**Skill-specific difference**: For a CLI/MCP, L1a (placeholder template in the package) and L1b (the user's real value under `~/.awp-{tool}/`) are two paths. In a Skill, both **use the same path**, `tools/credentials/{service}.md`. The placeholder template and the user's real value are two filled states of the same file. The Skill view therefore simplifies them into one L1 level.

| Level | Path | Available to | Description |
|------|------|------|------|
| L0 | Environment variable `{SERVICE}_{KEY}` | Everyone | CI / Docker / temporary override |
| L1 | `tools/credentials/{service}.md` | **Members** | A placeholder template at distribution time (L1a); becomes L1b after the member fills in the real value |
| L2 | `{knowledge_base_root}/credentials/{service}.md` | **AWP** | Built-in knowledge-base credentials, absent from member environments |

At runtime, search in the order L0 → L1 → L2. Return as soon as a level matches and do not search lower levels. **Safety follows naturally**: a member's machine has no AWP knowledge base → the L2 directory does not exist → skip it silently with no error. See your knowledge base's credential standard for placeholder-level meaning and configured-value checks.

### 3.2 Core Rules

| Rule | Description |
|------|------|
| **Self-contained distribution mode** | A Skill distributed to members includes placeholder `.md` files under `credentials/` |
| **Automatic knowledge-base reuse** | In project owner's environment, `_cred.py` automatically falls back to knowledge-base credentials |
| **One entry point** | Read every credential through `shared/_cred.py:load_cred()` |
| Do not hard-code paths | Do not write knowledge-base paths directly in scripts |
| Do not commit real Keys | Do not commit credential files to version control |
| Do not redact during review | When reviewing a Skill under this standard, do not change real keys the user has already configured |

### 3.3 Why Use Two Modes?

| Problem | Risk with L1 only | Dual-mode solution |
|------|---------------|-----------|
| Repeated work for AWP | Manually fill Keys for every Skill (66 credentials × N Skills) | L2 automatically reuses the AWP knowledge base |
| Portability for members | — | L1 placeholders + clear errors that explain setup |
| Environment differences | Paths differ across machines | Skip silently when the L2 directory does not exist |

### 3.4 Naming Convention

`load_cred(service)` uses the same `service` argument to build the L1 and L2 paths. For example, `load_cred("search-example-shared-api")` maps to:
- **L1**: `tools/credentials/search-example-shared-api.md`
- **L2**: `{knowledge_base_root}/credentials/search-example-shared-api.md`

**Rule**: `service` is the file name without the `.md` suffix. L1 and L2 use the same `service`, so their file names must match. Knowledge-base credential files follow your knowledge base's four-part naming rule (`{domain}-{service}-{owner}-{purpose}.md`). The file name under the Skill's `credentials/` must match it so fallback can find it.

### 3.5 Placeholder Detection

> Full placeholder standard: see §2.6 of your knowledge base's credential standard.

Treat a value matching `YOUR_*` / `REPLACE_*` / `CHANGEME` / empty as a placeholder, then fall back to the next level.

### 3.6 Correct Pattern

```python
# ✅ Load through _cred.py (automatic three-level fallback)
from shared._cred import load_cred
token = load_cred("example-api")
base_url = load_cred("example-api", "Base URL")
```

### 3.7 Wrong Patterns

```python
# ❌ Hard-coded knowledge-base path
cred_path = Path("{english_vault_root}/tools/credentials/search-example-shared-api.md")

# ❌ Reads only inside the Skill and does not support fallback
cred_path = skill_dir / "credentials" / "example-api.md"

# ❌ Uses JSON
cred_path = skill_dir / "credentials" / "example-api.json"
```

---

## 4. Reading Credentials in Scripts

### 4.1 One Entry Point

Load through `shared/_cred.py:load_cred()`. See skill-script-file-standard.md §2.

### 4.2 `_cred.py` Reference Implementation

**Location**: `scripts/python/shared/_cred.py` (copy-per-Skill, the same pattern as `workflow.py`)

```python
"""Skill credentials dual-mode loader (unified Markdown format)"""
import os, re
from pathlib import Path

_KB_CRED = Path.home() / "knowledge-base/tools/credentials"  # Adjust to your KB layout
_PLACEHOLDER_RE = re.compile(r"^(YOUR_|REPLACE_|CHANGEME$)", re.IGNORECASE)


def _is_placeholder(v: str) -> bool:
    return not v or not v.strip() or bool(_PLACEHOLDER_RE.match(v.strip()))


def _read_md(path: Path, key: str) -> str | None:
    """extract a value from a Markdown table row | key | `val` |"""
    if not path.exists():
        return None
    try:
        content = path.read_text(encoding="utf-8-sig")
    except UnicodeDecodeError:
        return None
    m = re.search(rf"\|\s*{re.escape(key)}\s*\|\s*`([^`]+)`", content)
    return m.group(1).strip() if m and not _is_placeholder(m.group(1)) else None


def load_cred(service: str, key: str = "API Key",
              *, skill_dir: Path | None = None) -> str:
    """
    load credentials with three fallback layers.

    Args:
        service: service name (for example, "example-api"), also used for L1/L2 filenames
        key: table field name (default "API Key")
        skill_dir: Skill root directory; infer automatically when None
    """
    # L0: Environment variable
    env_key = f"{service.upper().replace('-', '_')}_{key.upper().replace(' ', '_')}"
    env_val = os.environ.get(env_key, "")
    if env_val and not _is_placeholder(env_val):
        return env_val

    # L1: MD inside the Skill
    sd = skill_dir or Path(__file__).resolve().parent.parent.parent.parent
    l1 = _read_md(sd / "credentials" / f"{service}.md", key)
    if l1:
        return l1

    # L2: Knowledge-base MD
    l2 = _read_md(_KB_CRED / f"{service}.md", key)
    if l2:
        return l2

    raise FileNotFoundError(
        f"credentials not configured: {service}\n"
        f"  → edit credentials/{service}.md enter {key}\n"
        f"  → or set environment variable {env_key}"
    )
```

**Usage examples**:

```python
from shared._cred import load_cred

token = load_cred("example-api")                    # Reads "API Key" by default
base_url = load_cred("example-api", "Base URL")    # Reads the specified field
bearer = load_cred("twitter-api", "Bearer Token")  # OAuth1
client_id = load_cred("github", "Client ID")       # OAuth2
```

---

## 5. Directory and Security

### 5.1 Directory Structure

```
credentials/
├── example-api.md      # Example API credentials
├── openai.md           # OpenAI API credentials
├── ghost.md            # Ghost API credentials
└── .gitignore          # Ignores credential files
```

### 5.2 .gitignore Configuration

```gitignore
credentials/*.md
!credentials/.gitkeep
```

**Note**: A credential file containing placeholders only may be committed as a template.

### 5.3 Credential Guide in setup.md

Include an acquisition guide in `docs/setup.md`:

```markdown
## credentialsconfig

### Example API

1. Visit the provider's official site
2. register an account and obtain an API Key
3. Edit `tools/credentials/example-api.md`,replace `YOUR_API_KEY` with your key
```

### 5.4 Security Red Lines

| Red line | Level |
|------|:----:|
| Real Keys never enter version control | ❌ |
| Scripts do not print Keys | ❌ |
| Errors do not expose full paths | ❌ |
| Do not hard-code Keys | ❌ |
| Rotate regularly | ⚪ |

---

## 6. Live Validation

Step00 must confirm that required credentials really work, not only that `tools/credentials/*.md` exists.

### 6.1 Validation Strategy

| Scenario | Default strategy | Description |
|------|----------|------|
| The service has a side-effect-free authentication endpoint | Must call it | Such as account/me, models/list, balance, or token introspection |
| The service has only paid generation endpoints | Do not call by default | Unless `allow_paid_probe=true` or the user explicitly enters strict smoke |
| OAuth / cookie credentials | Must check session validity | Do not submit, publish, delete, or perform other writes |
| Cannot validate | Fail with `credential_live_unverified` | Do not enter business steps |

### 6.2 Cache Rules

The live validation result may be written to the preflight cache in the Runtime inventory. The cache must stay private to the local machine. It does not enter the Skill directory and is not synced.

Cache hit conditions:

- The last state was `passed`
- The credential source level has not changed
- The credential file mtime or local secret fingerprint has not changed
- The cache has not expired
- The current preflight mode is not `strict`

### 6.3 Failure Handling

| Error | Handling |
|------|------|
| Placeholder not replaced | Stop and tell the user to fill in the credential |
| 401/403 | Stop and tell the user to reset the Key or check permissions |
| Insufficient balance / quota | Stop and tell the user to add funds or switch credentials |
| Cannot validate | Stop and require an `auth_check` or explicit permission for a paid probe |

---

## Checklist

**Credential files (credentials/*.md)**:
- [ ] Use Markdown format (❌ no .json/.env/.yaml)
- [ ] Follow the five-section skeleton in `AWP credentials`
- [ ] Use the `YOUR_*` prefix for placeholders
- [ ] Required credentials have a Step00 live validation plan

**Dual-mode credentials**:
- [ ] Scripts load through `shared/_cred.py:load_cred()`
- [ ] The `credentials/` directory contains distributable placeholder `.md` files
- [ ] No direct references to knowledge-base paths
- [ ] Skip silently when the L2 directory does not exist

**Security isolation**:
- [ ] .gitignore contains `tools/credentials/*.md`
- [ ] Scripts do not print keys to stdout/stderr
- [ ] docs/setup.md contains a credential acquisition guide
- [ ] Pass `../../awp-meta-authoring-standard/std-style-language-contract.md`: three-layer language contract / remove jargon / formalize spoken language / faithfulness, readability, and fit
