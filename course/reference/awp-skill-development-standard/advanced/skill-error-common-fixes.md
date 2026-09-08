---
document_id: awp-skill-development-standard/advanced/skill-error-common-fixes
language: en
publication: public
source_revision: 2
title: "Common Errors and Fixes"
purpose: A guide to diagnosing and fixing errors across seven layers
category: Tool Support
prerequisites: []
see_also:
  - skill-credential-file-standard.md
  - skill-runtime-data-standard.md
  - skill-platform-constraint-limits.md
  - skill-docs-authoring-standard.md
---

# Common Errors and Fixes

> This document governs **error diagnosis and repair** during Skill execution.
> Output responsibility: provide a lookup table organized into seven layers, from error sign to cause to fix. This is one shared global reference for every Skill.
>
> **Example note**: service names in this document, such as `example-api` and Ghost, are placeholder or illustrative services. They may be replaced with any similar API service.

---

## 0. Design Philosophy

| Principle | Description |
|------|------|
| ✅ **Locate by layer** | Step00 blocks startup errors first; the seven-layer system then checks from Runtime assets through progress, one layer at a time |
| ✅ **Verify before fixing** | Confirm the root cause before making a change; do not make speculative edits |
| ✅ **Prefer recoverable behavior** | If an environment or model is missing, report a structured error; do not download or fall back silently |

## Organization Framework

| # | Question | Section | Main Content |
|---|------|------|---------|
| §1 | How are errors classified? | Step00 + seven error layers | preflight + L0–L6 definitions |
| §2 | What failed in Runtime assets? | L0 Runtime asset errors | Environment entities, models, and provider cache |
| §3 | What failed at runtime? | L1 runtime errors | Versions, environment variables, and context overflow |
| §4 | What failed in dependencies? | L2 dependency errors | Missing modules and version conflicts |
| §5 | What failed in credentials? | L3 credential errors | 401/403, format, and Token |
| §6 | What failed on the network? | L4 network errors | Timeouts, rate limits, and pagination |
| §7 | What failed in paths? | L5 path errors | Missing files, permissions, and cross-platform behavior |
| §8 | What failed in progress state? | L6 progress errors | Stuck state, damaged files, and duplicate processing |
| §9 | How do I locate it quickly? | Quick diagnostic flow | Decision tree that checks layers in order |
| §10 | How do I prevent it? | Prevention | Startup validation, batches, and timeout control |
| §11 | How do I validate output? | Five-layer validation pattern | Exists → format → fields → values → business |
| §12 | How do I retry? | Retry policy | Exponential backoff and batch timeouts |
| §13 | How do I recover after interruption? | Cross-step recovery flow | Decision tree, context rebuilding, and checklist |

## 1. Step00 + Seven Error Layers

Run the Step00 preflight at startup. If Step00 fails, do not enter a business step. After Step00 passes, follow the seven layers defined by setup.md from the lowest layer upward:

| Layer | Class | Applies To |
|------|------|----------|
| Preflight | **Startup preflight layer** | Structure, Runtime, credentials, run directory, script entry point, and cache state |
| L0 | **Runtime asset layer** | Missing or damaged Runtime env, local model, or provider cache |
| L1 | **Runtime layer** | Incompatible versions or missing environment variables |
| L2 | **Dependency layer** | Missing package/module or version conflict |
| L3 | **Credential layer** | API authentication failure or malformed secret |
| L4 | **Network layer** | Connection timeout, rate limit, or proxy problem |
| L5 | **Path layer** | Missing file or insufficient permission |
| L6 | **Progress layer** | Lost state or failed resume_hint |

---

### 1.1 Preflight Startup Errors

| Error Sign | Cause | Fix |
|----------|------|----------|
| `preflight_failed` | Any required Step00 check failed | Read the checks details, fix structure / Runtime / credentials / directory, then rerun |
| `preflight_cache_stale` | The cache expired or its last state was not `passed` | Rerun `python {skill_dir}/scripts/doctor.py --deep` |
| `preflight_cache_mismatch` | The fingerprint changed for SKILL.md, workflow, scripts, config, credentials, or reference | Rerun the heavy Step00 checks and refresh inventory |
| `preflight_skip_denied` | The user requested `off`, but no valid successful cache exists | Fall back to `auto` and run light checks plus any needed heavy checks |
| `credential_live_unverified` | A required credential has no usable authentication probe, or the probe did not pass | Add `auth_check`, replace the credential, or explicitly allow a paid smoke test |
| `dependency_not_installed` | Runtime env / package / binary / model is not installed or executable | Run `prepare`, or install as doctor suggests, then rerun |

A preflight error cannot be solved by skipping a business step. Step00 does not create a long-lived environment, download a model, or call a paid generation API by default. When an entity needs preparation, run `prepare` as the output suggests.

## 2. L0 Runtime Asset Errors

| Error Sign | Cause | Fix |
|----------|------|----------|
| `runtime_missing` | The environment or model declared in `config/runtime.json` has not been prepared | `python {skill_dir}/scripts/prepare.py --profile standard` |
| `runtime_corrupt` | Model checksum mismatch or failed environment validation | `python {skill_dir}/scripts/prepare.py --repair --profile standard` |
| `dependency` | A system binary such as ffmpeg, tesseract, or poppler is missing | Install it as `doctor` says, then rerun `skill doctor` |
| Model license requires a manual download | Automatic download is prohibited | Follow the manual import process in setup.md and run `model import` |
| Polluted provider cache | The SDK put its default cache in a global directory | Remove the wrong cache, set the Runtime cache root, then rerun |

**Structured error example**:

```json
{
  "ok": false,
  "error_type": "runtime_missing",
  "message": "model whisper-cpp/ggml-base is not installed",
  "fix": "python /path/to/skill/scripts/prepare.py --profile standard"
}
```

Do not solve a Runtime asset error by downloading silently inside a script, switching to system Python, or writing temporary data to `/tmp`.

---

## 3. L1 Runtime Errors

| Error Sign | Cause | Fix |
|----------|------|----------|
| Context overflow | Rare with 1M; usually caused by reading an unusually large amount of data | Check for needless full-file reads |
| Agent is stuck and does not respond | No timeout control | `TaskOutput(timeout=300000)` |
| Incompatible version | Python/Node version is too old | Upgrade the runtime version |
| Missing environment variable | .env was not loaded | Use `python-dotenv` or `source .env` (credentials must still use JSON) |

---

## 4. L2 Dependency Errors

| Error Sign | Cause | Fix |
|----------|------|----------|
| ModuleNotFoundError | A dependency is not installed, or the Runtime environment was bypassed | Run `python {skill_dir}/scripts/doctor.py` first |
| Version conflict | Dependency versions are incompatible | Check pyproject.toml/package.json |
| Missing batch output | Output was not validated | Five-layer validation + retry logic |
| Schema validation failure | Output format does not match | JSON Schema validation + repair prompt |

---

## 5. L3 Credential Errors

| Error Sign | Cause | Fix |
|----------|------|----------|
| 401 Unauthorized | API Key is invalid or expired | Check `tools/credentials/*.md` or environment variables, then generate it again |
| Malformed credential | A Markdown table field is missing or misnamed | Validate fields such as `API Key` / `Base URL` |
| Bearer Token spelling | Missing space or wrong capitalization | Standard form: `Bearer {token}` |
| 403 Forbidden | Insufficient permission | Check the API Key permission scope |

---

## 6. L4 Network Errors

| Error Sign | Cause | Fix |
|----------|------|----------|
| 5xx still fails after retry | Service is temporarily unavailable | Add retries and lengthen backoff |
| Retry does not help a 4xx | The request itself is wrong | Check parameters; do not retry a 4xx |
| Frequent timeout | Timeout is too short | Adjust `timeout=120` |
| Garbled URL parameters | Special characters were not encoded | `urllib.parse.urlencode()` |
| Cursor pagination loses data | Cursor handling is wrong | Save the cursor to state and resume from the checkpoint |
| Rate Limit (429) | Requests are too fast | Read the `Retry-After` header, wait, then retry |

---

## 7. L5 Path Errors

| Error Sign | Cause | Fix |
|----------|------|----------|
| File not found | Hard-coded path | Use variable placeholders such as `{run_dir}` |
| `~/` path does not resolve | The Agent does not expand the tilde | Use an absolute path or `Path.expanduser()` |
| Broken ordering | Single-digit step number | Use the two-digit prefix `stepNN-*.md` |
| Cross-platform path fails | Windows backslashes | Use `Path` or forward slashes everywhere |
| Insufficient permission | File/directory permissions | Adjust permissions with `chmod` |
| Hard-coded Runtime path | Internal path such as `~/.awp/runtime/...` is fixed in code | Get the env/model/cache path through the Runtime resolver |

---

## 8. L6 Progress Errors

| Error Sign | Cause | Fix |
|----------|------|----------|
| `processing` state is stuck | Interruption was not cleaned up | On rerun, retry `processing` items automatically |
| Damaged progress file | Write was interrupted | Use an atomic write: temporary file first, then rename |
| Batch index out of range | `current_batch` is outside the range | Validate `current_batch < len(batches)` |
| Lost progress | No checkpoint information after interruption | Recover from `state/progress.json` |
| Completed item runs again | State was not checked | Check `completed_batches` or `items[].status` |

---

## 9. Quick Diagnostic Flow

```
An error occurs
    ↓
1. Check the error class (in layer order)
   - Runtime asset layer → section 2
   - Runtime layer → section 3
   - Dependency layer → section 4
   - Credential layer → section 5
   - Network layer → section 6
   - Path layer → section 7
   - Progress layer → section 8
    ↓
2. Read logs
   - state/progress.json (progress)
   - Console output
    ↓
3. Find the root cause
   - Which step failed?
   - Which batch failed?
   - What is the exact error?
    ↓
4. Apply the fix
```

---

## 10. Prevention

| Measure | Description |
|------|------|
| Step00 preflight | At each startup, check structure, Runtime, credentials, run directory, script entry point, and cache state |
| Runtime doctor | Before execution, check env/model/binary/cache |
| Run in batches | Avoid processing too much data at once |
| Persist progress | Update progress.json after every batch |
| Timeout control | Set a timeout on every Agent/HTTP call |
| Atomic operation | Write files through a temporary file + rename |

---

## 11. Five-Layer Validation Pattern

Run five-layer validation on key output, aligned with the checkpoint in skill-step-document-standard.md:

| Layer | Check | On Failure |
|------|--------|---------|
| 1. File exists | File exists and is not empty | Retry |
| 2. Format is correct | JSON/MD parses | Retry |
| 3. Fields are complete | Required fields exist | Retry |
| 4. Value range is valid | Field values are inside the valid range | Mark abnormal |
| 5. Business logic | Business rules are met | Mark failed |

Check in layer order. If any layer fails, return the error information. Failures in layers 1–3 may retry. Failures in layers 4–5 are marked abnormal/failed.

---

## 12. Retry Policy

### 12.1 HTTP Request Retry

Exponential backoff: `delay = base_delay * (2 ** attempt)`, with 3 retries by default. See skill-script-file-standard.md §8.3.

### 12.2 Batch Retry

| Parameter | ✅ Default | Range |
|------|--------|------|
| Timeout | 5 minutes | 3–10 minutes |
| Retry count | 2 | 1–3 |

---

## 13. Cross-Step Recovery Flow

### 13.1 Recovery Decision Tree

```
Read progress.json
    ↓
Check step_status
    ↓
├─ Every step completed → workflow complete
├─ Current step in_progress → continue from the current step
├─ Current step failed → decide from error_type
│   ├─ Retryable → run the current step again
│   └─ Not retryable → roll back to the previous step
└─ Prerequisite step failed → start from the failed step
```

### 13.2 Rebuilding Context During Recovery

| Step | Action |
|------|------|
| 1 | Read `state/progress.json` |
| 2 | Read `resume_hint` to get the executor type |
| 3 | Read the matching `workflow/stepNN-*.md` |
| 4 | Verify that prerequisite output files exist |
| 5 | Continue from the checkpoint |

Run recovery logic through the decision tree in §13.1. Retryable error types: `timeout`, `rate_limit`, and `service_unavailable`. For a Runtime asset error, run doctor/prepare/repair first, then resume the workflow.

### 13.3 Recovery Checklist

| Order | Check | Action |
|------|--------|------|
| 1 | Read progress | `Read state/progress.json` |
| 2 | Read resume_hint | Get the executor and warnings |
| 3 | Read the step document | `Read {workflow_doc}` |
| 4 | Validate prerequisite output | Check that input files exist |
| 5 | Confirm execution method | Continue according to the executor type |

---

## Checklist

**Error Classes and Fix Table**:
- [ ] Errors are covered by Step00 + seven layers (Preflight + L0–L6)
- [ ] Preflight covers `preflight_failed`, `preflight_cache_stale`, `preflight_cache_mismatch`, and `preflight_skip_denied`
- [ ] Credential live validation covers `credential_live_unverified`
- [ ] Dependency-installation check covers `dependency_not_installed`
- [ ] Runtime asset layer covers `runtime_missing`, `runtime_corrupt`, and `dependency`
- [ ] Every error has three columns: sign, cause, and fix
- [ ] Retry policy has a maximum attempt limit (≤3)

**Validation and Recovery**:
- [ ] The five-layer validation flow is complete: exists → format → fields → values → business
- [ ] Cross-step recovery has a decision tree and checklist
- [ ] Recovery logic separates retryable from nonretryable error types
- [ ] Pass `../../awp-meta-authoring-standard/std-style-language-contract.md`: three-layer language contract / remove jargon / conversational formalization / fidelity, clarity, and grace

> ⚠️ Deviation format: `⚠️ Deviation: {rule} | Reason: {reason}`—valid only for the current output and does not set a precedent.
