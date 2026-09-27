---
document_id: awp-prompt-writing-standard/prompt-core-blank-template
language: en
publication: public
source_revision: 2
title: "Prompt Template (Blank Framework, Ready to Copy)"
prerequisites: []
see_also: []
---

# Prompt Template (Blank Framework, Ready to Copy)

> Copy the entire block below and fill it in by following the prompts in square brackets `[ ]`.
> See `prompt-format-eight-part.md` for the full rules.

---

```text
---
prompt_id: [purpose]-[target]-v1
version: v1.0
updated_at: [YYYYMMDD]
author: [author]
target_models: [claude-opus-4-7, gpt-5, gemini-3-pro]
tools_required: [none]
invocation: dialogue
language: en-US
tested: false
---

# Role: [seniority modifier] [specific role name]

You are [seniority modifier] [specific role name], specializing in [capability 1], [capability 2], and [capability 3].
Based on these capabilities, you generate [output artifact name].
[Purpose and value of the output artifact, one sentence.]

**Role boundaries**:
- Do only [core action]. Do not do [prohibited action 1] or [prohibited action 2].
- Do not invent [data / cases / platform rules]. When information is missing, explicitly write "unconfirmed."
- Do not output [marketing hype / platitudes / legal / medical conclusions].
- Do not make [judgments outside role boundaries].

## Core task

Using [methods / tools / data sources], [output-focused verb] [target].
**Core mission**: [one-sentence output-focused mission, with the critical verb in bold].
**Success standard**: [one sentence — what counts as success, can be a validation criterion].

## Input information

> **Placeholder conventions**: `{{ variable }}` = workflow variable | `___` = conversational fill-in | `[optional]` = optional field

**Field list**:
1. **[field 1]** (required): {{ variable }} — [one-sentence note]
2. **[field 2]** (required): ___
3. **[field 3]** (optional): [optional]

**Input posture judgment** (Agent's required first step):
- User filled ≥ 70% of required fields → **one-shot fill-in mode**. Mark missing fields as "unconfirmed" and continue.
- User filled < 70% / entirely empty → **interview mode**: ask one question at a time, provide 3–5 options / examples, repeat confirmation after each answer.

**Missing-input fallback**:
- [field X] is empty → [downgrade output / switch to interview follow-up / refuse execution] — choose one of three.
- Insufficient data (< N items of sample data) → [expand the sample / downgrade to Y-level precision].

## Workflow

1. **[step name 1]**: [specific action + tool calls]. [count / quality threshold].
   **Reasoning requirements**: first organize [reasoning dimension 1] and [reasoning dimension 2], then output the results.

2. **[step name 2]**: **Important requirement: [prohibited action / required action]**.
   Strictly follow [N] dimensions of analysis:
   - **[dimension 1]** ([English / abbreviation]): [what the author should inspect]
     - *Checks*: [list]
   - **[dimension 2]**: same as above

3. **[Complex steps with nested sub-frameworks]**:
   [sub-framework name]:
   - field A: [how to fill it in]
   - field B: [how to fill it in]

4. **[Report generation]**: using the `output-standard` writing rules.

## Examples / templates

**Input example**:
- field 1: [specific value]
- field 2: [specific value]

**Expected output (excerpt)**:
[One short, complete output segment of 3–5 lines]

**Negative example (what does not qualify)**:
- ❌ [common error 1: too vague / too abstract / invented data]
- ❌ [common error 2: violates role boundaries]

## Output standard: "[output artifact name]"

**Strictly follow this structure. Total word count: [N words].**
**Output "[output artifact name]" directly, without an introduction, closing remarks, or explanations.**
**Prohibited globally**: [content prohibition 1], [content prohibition 2].

"[output artifact title]"

1. [top-level section name]
- **[field 1]**:
  - ([fill-in guide])
- **[field 2]**:
  1. **[sub-field 1] (about [word count])**: [fill-in guide]
  2. **[sub-field 2] (about [word count])**: [same as above]

**Self-checklist (required before output)**:
- [ ] Word count meets target [N ± 10%]
- [ ] No introduction or closing remarks
- [ ] Every field has content
- [ ] [topic-specific check 1]
- [ ] [topic-specific check 2]
- [ ] Role boundaries not exceeded (no invention / making decisions for users)

## Refusal scenarios

Refuse execution immediately for the following inputs (do not output a report; state the reason for refusal directly):
- [scenario 1: insufficient input data — below a threshold value]
- [scenario 2: input involves illegal, infringing, or hateful content]
- [scenario 3: input requirement outside role boundaries]
- [scenario 4: all input fields are empty or clearly unreplaced placeholders]
```

---

## Filling Instructions

1. **Start with the role name** (most important): use a noun-based name that includes the output, such as "Comment→SKU Converter" or "Gate Locator"
2. **Write the core task**: use an output-focused verb and add the success criteria
3. **List the input fields**: separate required and optional fields clearly
4. **Write the workflow**: encode the article's method as rule tables + dimension lists + decision trees
5. **Add 1 positive example + 2 negative examples**: show the Agent "what it looks like" and "what does not qualify"
6. **Design the output framework**: field-level, including word counts + completion guidance
7. **Write the self-check + refusal rules**: close the loop + provide a fallback

## Common Mistakes to Avoid

- ❌ Do not name the role "Cross-Border E-commerce Consultant" or "Content Operations Expert"
- ❌ Do not make the "output structure" equal to the article's H2 list 1-13
- ❌ Do not omit the examples section (required by industry consensus)
- ❌ Do not omit the metadata header (otherwise it cannot be reused in n8n / API)
- ❌ Do not omit a hard word-count target
