---
document_id: awp-skill-development-standard/advanced/skill-testing-process-standard
language: en
publication: public
source_revision: 1
title: "Skill Testing Standard"
purpose: Standard methods for Skill trigger, function, and performance testing
category: Standard
prerequisites:
  - ../skill-core-file-declaration.md
  - skill-step-document-standard.md
see_also:
  - skill-error-common-fixes.md
  - skill-platform-constraint-limits.md
---

# Skill Testing Standard

> This document governs **Skill testing methods and the release process**: trigger tests, function tests, and performance tests.
> output responsibility: define test cases, acceptance conditions, and the pre-release checklist. This file is a globally shared reference used by every Skill.

---

## 0. Design Philosophy

| Principle | Description |
|------|------|
| ✅ **Evaluation-driven** | Define acceptance conditions before writing rules the Skill, instead of deciding how to test it afterward |
| ✅ **Cross-model verification** | The Skill must pass on Haiku, Sonnet, and Opus |

## Organization

| # | Question | Section | Main content |
|---|------|------|---------|
| §1 | What test types exist? | Test Types | Trigger/function/performance |
| §2 | How are triggers tested? | Trigger Testing | Positive/synonym/negative/debugging |
| §3 | How are functions tested? | Function Testing | Happy Path/edge cases/error recovery |
| §4 | How is performance tested? | Performance Testing | Time/Token/quality/parallel work |
| §5 | What support tool is available? | skill-creator Tool | Draft generation/review/trigger improvement |
| §6 | How are fixes iterated? | Iteration Pattern | Feedback → fix → verification cycle |
| §7 | What is checked before release? | Testing Checklist | Trigger/function/performance/documentation checks |
| §8 | How is EDD done? | Evaluation-Driven Development | Write evaluations before writing rules the Skill |
| §9 | How is A/B development done? | Claude A/B Iteration | Separate author/user roles |
| §10 | What does a full release check? | Full Release Checklist | Main quality/code/testing/cross-model |

## 1. Test Types

| Type | Goal | Requirement |
|------|------|--------|
| **Trigger testing** | Whether description correctly matches user intent | Required |
| **Function testing** | Whether steps execute correctly | Required |
| **Performance testing** | Speed, Token use, and quality | ⚪ Optional |

---

## 2. Trigger Testing

Trigger testing verifies whether the `description` field makes Claude activate the Skill in the right case and remain inactive in unrelated cases.

### 2.1 Positive Trigger

Use an exact phrase from description to trigger it directly and verify the baseline match rate.

```
# Example: description says "Trigger when the user says 'analyze'"
Test input:"help me analyze this item's code quality"
Expected result:Skill is activated
```

### 2.2 Synonym/Paraphrase Trigger

Test the same intent with different wording to cover natural language variations from users.

```
# Same intent, different wording
"Check this project's code"
"Review code quality"
"Help me do code review"
```

### 2.3 Negative Exclusion Test

Verify that unrelated requests do not trigger the Skill and prevent activation in the wrong case.

```
# These should not trigger Code Analyzer Skill
"Help me write some code"
"Check the weather"
"Summarize this article"
```

### 2.4 Debugging Method

When trigger behavior does not match expectations, ask Claude directly:

```
"When would you use the [skill-name] skill?"
```

Claude will quote description content to explain the trigger logic. Use that explanation to decide whether description needs changes.

### 2.5 Trigger Regression (When the Skill Library Changes)

Triggers compete across the entire library: Claude chooses among the name + description of every installed Skill. Adding a Skill or heavily changing a description may take triggers away from an existing Skill or weaken its matches.

| Timing | Action |
|------|------|
| Add a Skill in a field close to an existing Skill | Run §2.1-§2.3 once for each similar Skill and confirm that its triggers were not taken |
| Heavily change a description | Run the full trigger suite for that Skill + negative exclusion tests for the one or two neighboring Skills most likely to be confused with it |
| Two Skills repeatedly trigger for each other's cases | First rewrite descriptions to make cases mutually exclusive (add negative trigger words); consider merging if they still cannot be distinguished |

---

## 3. Function Testing

### 3.1 Happy Path

Use standard input to verify that the full process runs as expected and produces correct output.

| Check | Pass condition |
|--------|---------|
| Each step output file exists | `step{NN}-*/` directory is not empty |
| JSON format is correct | Parses successfully and includes all fields |
| Final output exists | `output/` contains the expected files |
| progress.json is updated | Contains the `resume_hint` field |

### 3.2 Edge Cases

| Case | Test method | Focus |
|------|---------|--------|
| Empty input | Trigger directly without parameters | Whether a clear message appears |
| Invalid format | Enter garbled text or a very long string | Whether it crashes |
| Very large data | Simulate over 1,000 items | Whether batch logic works |
| Repeated trigger | Run again against existing runs/ | Whether a new run directory is created correctly |

### 3.3 Error Recovery

Simulate failure in an intermediate step and verify the Skill's fault tolerance and recovery.

```
# Test case 1: Simulate an API timeout
Manually modify the script to return a timeout error
Expected:retry twice,mark it failed after the limit and continue

# Test case 2: Recover after interruption
At Step 3,force-terminate
Expected:read progress.json,from Step 3 resume from the checkpoint
```

> See `skill-error-common-fixes.md` for the recovery process.

---

## 4. Performance Testing

| Metric | Measurement | Baseline |
|------|---------|------|
| Completion time | From trigger to final output | Compare with manual work time |
| Token use | Total input/output Tokens | Within a sensible range (no strict limit below 1M) |
| output quality | Compare with human results | Matches or exceeds them |
| Parallel efficiency | Agent parallelism vs serial time | Uses parallel work sensibly (no more than 4 per round) |

---

## 5. skill-creator Tool

Claude includes skill-creator, which can support Skill generation and improvement:

| Use | Description |
|------|------|
| Generate a draft | Generate a full Skill structure from a requirements description |
| Review and improve | Analyze an existing Skill and give an improvement plan |
| Improve triggers | Check whether description coverage is sensible |

**How to call it**:

```
"Help me build a skill using skill-creator"
"Review my skill using skill-creator and suggest improvements"
```

---

## 6. Iteration Pattern

Make targeted fixes from test feedback and quickly reach a stable version.

| Feedback | Fix |
|---------|---------|
| Does not trigger | Expand description trigger words to cover more natural-language variations |
| Triggers incorrectly | Add negative trigger words to narrow the match range |
| Unstable quality | Add exact instructions and output examples to reduce ambiguity |
| Runs too slowly | Increase parallelism (no more than 4 per round) and reduce unnecessary serial waiting |
| Skips steps | Use numbered lists and add verification checkpoints to step documents |
| Context overflow | Rare below 1M; check for unusually large data reads |

### Iteration Rhythm

```
Test → discoveryProblem → locate description / step / prompt
    → modify → trigger validation again → after stabilization, preserve history with git log
```

---

## 7. Testing Checklist

Complete these checks before releasing a new Skill:

```
Trigger tests
  [ ] all positive trigger phrases match
  [ ] 3+ paraphrase tests with synonyms pass
  [ ] negative tests do not trigger falsely

Functional tests
  [ ] Happy Path end to endruns end to end
  [ ] each output file passes validation
  [ ] interruption recovery tests pass

Runtime Test
  [ ] doctor can discover missing environment variables/models/binaries
  [ ] can complete a run after prepare
  [ ] when the model is missingdoes not download silently

Performance tests
  [ ] token use is within a reasonable range
  [ ] batching works for large data volumes

Documentation
  [ ] SKILL.md workflow table matches actual steps
  [ ] docs/setup.md overrides Runtime/model/credentialspreparation
```

---

## 8. Evaluation-Driven Development (EDD)

> **Source**: Official Anthropic Skill practices. Main idea: **Write evaluations before writing rules the Skill. Make sure it solves a real problem instead of documenting an imagined need.**

### 8.1 EDD Process

```
1. Identify gaps → run the task without the Skill and record areas where Claude fails
2. Create evaluations → write three test scenarios that cover the gap
3. Establish a baseline → Claude's performance without the Skill
4. Write the minimum instructions → write only the content needed to pass the evaluation
5. Iterate → run evaluations → compare with the baseline → refine
```

### 8.2 Evaluation Structure

```json
{
  "skills": ["my-skill-name"],
  "query": "the user's real request",
  "files": ["test-files/sample.pdf"],
  "expected_behavior": [
    "read the file successfully",
    "extract all content correctly",
    "output format matches expectations"
  ]
}
```

### 8.3 Key Principles

| Principle | Description |
|------|------|
| Evaluation before documentation | First confirm that the problem solved by the Skill exists |
| Minimal instructions | Write only the minimum content needed to pass the evaluation |
| Real cases | Test with real tasks, not invented cases |
| Continuous iteration | Whenever a new problem appears, add an evaluation before fixing the Skill |

---

## 9. Claude A/B Iterative Development

> **Source**: Official Anthropic Skill development pattern.

### 9.1 Role Definitions

| Role | Description | Responsibility |
|------|------|------|
| **Claude A** (author) | Claude instance that helps design the Skill | Analyze requirements → generate SKILL.md → improve instructions |
| **Claude B** (user) | Claude instance that loads the Skill and runs a real task | Find problems with the Skill in actual use |

### 9.2 Development Process

```
1. complete the task without a Skill (using Claude A)
   → note which context you repeatedly provided

2. ask Claude A to package this context into a Skill
   → "Create a Skill that captures this pattern"

3. ask Claude A to check conciseness
   → "Remove explanations Claude already knows"

4. test with Claude B (new instance + Skill)
   → observe whether B finds correct information and applies the rules correctly

5. bring the results back to Claude A
   → "Claude B forgot to filter test accounts,
      maybe the rule isn't prominent enough?"

6. A modify → B test again → loop
```

### 9.3 Observe Claude's Navigation Behavior

During iteration, watch how Claude actually uses the Skill:

| Observation | Possible improvement |
|---------|----------|
| Reads files in an unexpected order | The structure is not clear enough; adjust directory organization |
| Does not follow a reference to an important file | The link is not visible enough; move the reference higher |
| Repeatedly reads the same file | Put that content directly in SKILL.md |
| Never opens a packaged file | The file may be unnecessary, or its reference is too weak |

---

## 10. Full Release Checklist

> **Source**: Official Anthropic Skill practices checklist.

### 10.1 Main Quality

```
[ ] description is specific and contains key trigger words
[ ] description states both "what it does" and "when to use"
[ ] SKILL.md body ≤800 lines(split detailed content into separate files)
[ ] nonetime-sensitive information(or place it in an "old mode" collapsed section)
[ ] all terminology is consistent
[ ] examples are concrete and executable, not abstract
[ ] filereferences remain one level deep
[ ] use progressive disclosure appropriately
[ ] complex workflows have clear steps and checklists
[ ] Step00 preflight Yesstartup entrypoint,and do not generate business artifacts
```

### 10.2 Code and Scripts

```
[ ] script solves the problem itself (solve, do not punt)
[ ] errors are handled clearly and helpfully
[ ] none"magic numbers"(all constants have explanatory comments)
[ ] dependencies are declared and verified available
[ ] Runtime dependencies are declared in config/runtime.json declare
[ ] do not commit .venv / node_modules / modelweights / provider cache
[ ] script has clear documentation
[ ] no Windows backslash paths
[ ] key operations have validation and a feedback loop
```

### 10.3 Runtime and Distribution Testing

```
[ ] Step00 first run:on a cache miss, run all necessary checks
[ ] Step00 cache hit:on a valid cache hit, skip heavy checks
[ ] Step00 cache invalidation:SKILL.md / workflow / scripts / config / credentials / reference recheck automatically after changes
[ ] Step00 missing credentials:return a structured failure and do not enter business steps
[ ] Step00 credentialsvalid:requiredcredentialspass side-effect-free live validation
[ ] Step00 dependencies are installed:env/package/binary/model all available
[ ] clean machine doctor:clearly lists missing env/model/binary/credential
[ ] standard machine prepare:can create a Runtime Environment and verify the model checksum
[ ] missing-model test:return runtime_missing; do not download silently
[ ] corrupt-model test:return runtime_corrupt; allow repair
[ ] portable Test:copy the Skill to another machine; after configuring credentials and running prepare, it can run
[ ] deduplicate the same model:do not download repeatedly when multiple Skills cite the same SHA-256
```

### 10.4 Testing

```
[ ] at least 3 evaluation scenarios
[ ] test Haiku, Sonnet, and Opus separately
[ ] test with real scenarios (not constructed data)
[ ] collect team feedback (where applicable)
```

### 10.5 Cross-Model Focus

| Model | Test focus |
|------|---------|
| **Haiku** | Does the Skill provide enough guidance? |
| **Sonnet** | Are instructions clear and efficient? |
| **Opus** | Does it overexplain what Claude already knows? |

---

## Checklist

**Trigger testing**:
- [ ] Every positive trigger word matches
- [ ] At least three synonym/paraphrase tests pass
- [ ] Negative tests cause no incorrect triggers

**Function testing**:
- [ ] The Happy Path runs from start to finish
- [ ] output-file verification passes for every step
- [ ] Interruption-recovery test passes

**Step00 preflight testing**:
- [ ] Full checks pass on the first run without a cache
- [ ] Heavy checks are skipped when a valid cache entry is found
- [ ] The cache becomes invalid after changes to SKILL.md / workflow / scripts / config / credentials / reference
- [ ] Step00 stops when credentials are missing or invalid, Runtime is missing, dependencies are not installed, or the run directory is not writable
- [ ] Required Step00 credentials pass live validation; when verification is impossible, it returns `credential_live_unverified`
- [ ] Step00 does not call a paid generation API, download models, or create a long-term environment by default

**Runtime testing**:
- [ ] `config/runtime.json` matches actual script dependencies
- [ ] doctor / prepare / repair paths are verified
- [ ] Missing environment, missing model, and corrupt model each return a structured error
- [ ] The portable Skill passes distribution testing on a new machine

**Release process**:
- [ ] EDD (Evaluation-Driven Development) is complete before release
- [ ] At least one Claude A/B iteration is complete
- [ ] Cross-model testing covers Haiku/Sonnet/Opus
- [ ] Every release checklist item passes
- [ ] Pass `../../awp-meta-authoring-standard/std-style-language-contract.md`: three-layer language contract / remove jargon / formalize speech / fidelity, clarity, and grace

> ⚠️ Deviation format: `⚠️ Deviation: {rule} | Reason: {reason}` — valid only for the current output and does not set a precedent.
