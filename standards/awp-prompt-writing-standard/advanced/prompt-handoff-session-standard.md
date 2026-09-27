---
document_id: awp-prompt-writing-standard/prompt-handoff-session-standard
language: en
publication: public
source_revision: 1
title: "Handoff Prompt Standard"
prerequisites: []
see_also: []
---

# Handoff Prompt Standard

> Method for passing unfinished work to another Agent, window, context, or model.
> The execution workflow and ready-made templates live in the session handoff workflow in your own knowledge base.

## Two Modes

Choose by task state, not by model:

- Step mode: the method is settled and only execution remains.
- Goal mode: the method is not settled and the next Agent must analyze and choose.

## Shared Context

Both modes must include:

- Background.
- Latest user request.
- User decisions and preferences.
- Finished work.
- Current state.
- Key references: absolute file paths, project location, stack, services, and environment.
- Risks, constraints, and known issues.

The modes differ only in the "work to do" section.

## Step Mode

- Number the next actions in run order.
- Name exact commands, files, and edits.
- Give one verification method for every step.
- Require deterministic evidence: command output, tests, real file state, or visible page behavior.
- Compare evidence with the expected result before marking a step done.
- Never accept "should work" or memory as proof.

## Goal Mode

- State what to achieve, what not to do, and where scope ends.
- Remove internal conflicts and vague wording.
- Do not include first-step, second-step instructions.
- Give enough authority to cover the stated boundary, not only the words named by the user.
- Forbid extra abstractions, files, or unused flexibility.
- Require deterministic evidence before reporting completion.

Goal mode needs both forces: do the job fully inside the boundary, and do not design beyond it.

## Why This Split

Capability levels such as basic, intermediate, and advanced imply that one mode is always better.
That is false. Handoff shape describes how settled the task is, not how strong the next model is.
Step versus goal stays useful as models change.

Model-specific warning lists were removed.
Stable lessons were put into both templates: evidence checks, exact boundaries, no contradictions, enough authority, and no extra design.
Fast-changing model settings remain historical research, not daily rules.

## Relationship to Other Rules

- `../prompt-format-eight-part.md` owns the shared prompt structure.
- This file adds the two handoff modes.
- The session handoff workflow in your own knowledge base generates and runs handoffs.
- Project handoffs normally use goal mode unless the route is already settled.
- Language follows the language rules in `../../awp-meta-authoring-standard/std-style-language-contract.md`.

## Checks

- The mode matches whether the method is settled.
- Every shared context block is present.
- Step mode includes ordered actions and proof per step.
- Goal mode includes scope, exclusions, authority, and anti-overdesign rules.
- File paths are absolute.
- Current state and finished work are factual.
- Completion depends on evidence, not confidence.

## Change Log

- 2026-07-10: Fixed the reference to the prompt writing standard.
- 2026-07-04: Created the two-mode method.
