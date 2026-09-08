---
document_id: awp-prompt-writing-standard/prompt-format-eight-part
language: en
publication: public
source_revision: 2
title: "AWP Prompt Standard (Eight-Part Format  -  Framework v1)"
prerequisites: []
see_also: []
---

# AWP Prompt Standard (Eight-Part Format  -  Framework v1)

> **This is the "Eight-Part Format" version in the prompt framework container** (see the framework-version table in `CLAUDE.md` for routing).
> **Scope (not universal)**: **one executable generation prompt** with a clear role and one output (report / retrieval / rewrite / single-turn generation / a prompt section embedded in n8n, an API, or a tool).
> **Not for**: a multi-file prompt knowledge system spanning "method/persona/vocabulary/machine-readable data" → use `advanced/prompt-system-multifile-framework.md`; if a task fits neither framework → create a new framework version in the container.
> One-line idea: **A prompt is not an article outline pasted in reverse. It packages a method into an executable tool.**

## 0. Position

### 0.1 Who Must Follow It

| Who | Requirement |
|---------|------|
| Every reusable prompt written for an LLM (Claude / GPT / Gemini / Llama) | Must follow |
| Prompt sections embedded in a workflow / Agent / Skill / Command | Must follow |
| "Copy and use" prompts for readers in tutorials / articles | Must follow |
| One-time conversations (open questions / simple queries) | Not required, but the structure is recommended |

### 0.2 Relationship With Other Standards

| Standard | Relationship |
|------|------|
| **Skill Development Standard** | Prompts in a Skill must follow this standard |
| **Command Development Standard** | Prompts in a Command must follow this standard |
| **Agent Tools Standard** | A prompt Using when a CLI / MCP tool calls an LLM must follow this standard |
| **Distribution Asset Standard** | Prompts embedded in public tutorials / resource packages must follow this standard |

### 0.3 Meta-Standard Requirements

- Every prompt must first meet the **general standards** (Markdown / files / file naming).
- Prompt filenames follow this collection's file naming rules, the two-hyphen pattern: `{purpose}-{target}-{version}.md` (for example, `advanced/prompt-example-material-report.md`).
- A reusable prompt must have a metadata header (see §2).

---

## 1. Design Philosophy

### 1.1 One-Line Idea

**Good prompt = encode the method as a rule set the Agent can execute directly + a concrete delivery checklist.**

### 1.2 Boundary with Writing Methodology (Must Read)

| Scenario | What the prompt should do | What it should not do |
|----------|--------------------------|----------------------|
| **Tool-type** (report fields, retrieval, rewriting checklists, API-embedded) | Lock down output fields, steps, and thresholds | Use a vague role with only "please analyze" |
| **Creative-type** (social posts, long articles, persona-driven writing) | Load persona + current material + style samples; align with Writing Methodology Standard | Use banned-phrase lists or sentence-pattern scanning to "lock down style" |
| **Safety and format** | Prohibited content, word-count caps, and output format may be hard constraints | Rely on regex to judge "whether it sounds human" |

Creative prompts use **thin protocols and dynamic sampling**. Do not pile on "no question endings / no X sentence pattern" machine-checkable bans just to pass detection — this conflicts with the Writing Methodology Standard and produces homogeneous output.

### 1.3 Five Anti-Patterns (Must Avoid)

| # | Anti-pattern | Form | Result |
|---|------|------|------|
| 1 | **Reverse-pasted H2s** | Copy an article's H2 list directly into the prompt's "output structure" | The Agent repeats the article outline instead of applying the method |
| 2 | **Generic consultant role** | "You are an X side-business coach / senior consultant" | The same template is applied across topics, producing repetitive output |
| 3 | **Empty task verbs** | "Analyze / help / improve / suggest" | The Agent does not know what concrete item to produce |
| 4 | **No method encoded** | Role + input + "please analyze" + output structure | The Agent does not know which rules to use for analysis |
| 5 | **Hardcoded creative writing rules** | Banned-phrase lists + requiring code to scan sentence patterns | Conflicts with Writing Methodology Standard; output becomes homogeneous |

### 1.4 Seven Positive Principles

1. **Use a specific role**: "Review-to-SKU Converter" is better than "cross-border e-commerce consultant."
2. **Name the task output**: "Produce three SKUs + a seven-day calendar" is better than "analyze reviews."
3. **Encode the method**: Explicitly put the article's five dimensions / five steps / decision tree / thresholds into the prompt as tables / lists.
4. **Give an output structure**: Use a field-level template instead of an abstract description.
5. **Lock behavior**: Use bold **must / prohibited / strict** language to turn soft advice into hard constraints.
6. **Pair prohibitions with positive replacements**: Saying what to do works better than only saying what not to do. Rewrite "Do not use Markdown" as "Write in clear prose paragraphs." Give a positive form for every key prohibition where possible (source: Anthropic, "Claude prompting best practices").
7. **Keep creative prompts simple**: Persona and material come first; do not pile on style-policing rules. For creative-type prompts, define "who is speaking, what material to use, what counts as passing" rather than listing sentence-level bans.

> **Note**: Principle 7 applies to creative-type prompts only. Tool-type prompts benefit from tight constraints (see §1.2).

### 1.5 Comparison With Industry Practice

| Industry source | Main structure | Matching section in this standard |
|---------|--------|-----------|
| Official Anthropic Claude guidance | Role + Task + Examples + Output + Constraints | § 4-9 |
| Reddit r/PromptEngineering 5-layer | ROLE → TASK → CONSTRAINTS → CONTEXT → FORMAT | § 4-9 |
| PMI 12 elements | Task + Role + Context + Input + Personality + Constraints + Format + Detail + Audience + Style + Options + Self-reflection | Fully covered by § 4-9 |
| Summary of 13 official DeepSeek examples | Role + task + context + examples + output + constraints | § 4-9 |

---

## 2. Metadata Header (Required)

Every standalone prompt file **must** contain a YAML frontmatter metadata header:

```yaml
---
prompt_id: material-need-report-v1
version: v1.0
updated_at: 20260521
author: AWP
target_models: [claude-opus-4-7, gpt-5, gemini-3-pro]  # Or [universal]
tools_required: [nocodb_get_many_rows]  # Or [none]
invocation: n8n  # Or api / cli / dialogue
language: en-US
tested: true
---
```

### 2.1 Field Descriptions

| Field | Required | Description |
|------|:---:|------|
| `prompt_id` | ✅ | Globally unique ID in kebab-case with a version suffix |
| `version` | ✅ | SemVer: v1.0 / v1.1 / v2.0 |
| `updated_at` | ✅ | YYYYMMDD format |
| `author` | ✅ | Author / team |
| `target_models` | ✅ | List of target models, or `[universal]` |
| `tools_required` | ✅ | Tool dependencies (search / NocoDB / browser / file reading); use `[none]` when none |
| `invocation` | ✅ | Call method: n8n / api / cli / dialogue / skill / command |
| `language` | ✅ | Output language: en-US / fr-FR |
| `tested` | ⚠️ | Whether it has been tested; when false, the caller must verify it |

### 2.2 Short Form for an Embedded Prompt

When a prompt is embedded in an article / Skill / workflow, the metadata header may be reduced to a one-line comment:

```text
<!-- prompt_id: xxx-v1 | model: claude-opus-4-7 | tools: none | tested: 2026-05-21 -->
```

---

## 3. Main Eight-Part Structure

```text
[Metadata header]
↓
# Role: [noun-based role + specialty + output artifact name]
[Role boundaries]
↓
## Core Tasks
[A prose core mission + success standard]
↓
## Input Information
[Placeholder conventions + field list + missing-input fallback]
↓
## Workflow
[Numbered steps + reasoning requirements + nested sub-frameworks]
↓
## Examples / Templates
[One positive + one negative]
↓
## Output Standard
[Skeleton + word count + prohibited introduction + self-checklist]
↓
## Refusal Scenarios
[Out of scope / violates ethics / refusal rules when data is insufficient]
```

### 3.1 Why the Sections Use This Order

| Order | Industry basis |
|------|--------|
| Metadata header first | YAML frontmatter standard + tool-call parsing needs |
| Role → task → input | Anthropic / Reddit consensus: instructions come first |
| Workflow in the middle | Put the method inside the execution process, closest to how the Agent acts |
| Examples → output rules | Verified by 13 DeepSeek examples: example-driven works better than spec-driven |
| Refusal cases last | Fallback rules for cases outside the normal path |

### 3.2 Cache-Friendly Split for API / n8n Use

⚪ When an API or automated workflow calls the same prompt repeatedly, put the **static structure** (role, workflow, examples, output rules, refusal cases) in the system message and **dynamic variables** (input fields and source material for this run) in the user message. Prompt caching matches prefixes, so an unchanged static prefix can hit the cache. This greatly reduces cost and delay for frequent calls (source: Anthropic prompt caching documentation). A one-time conversational use does not need this split.

---

## 4. Role Section

### 4.1 Standard Template

```text
# Role: [seniority modifier] [specific role name]

You are [seniority modifier] [specific role name], specializing in [capability 1], [capability 2], [capability 3].
Based on these capabilities, you generate [output artifact name].
[Output artifact purpose and value, one sentence].

**Role boundaries**:
- You only [core action]. Do not do [prohibited action 1], [prohibited action 2].
- Do not invent [data / cases / platform rules]. When information is missing, explicitly write "unconfirmed."
- Do not output prohibited content forms, such as marketing exaggeration, motivational filler, or unsupported legal and medical conclusions.
- Do not make [judgments outside role boundaries].
```

### 4.2 Three Hard Rules for Role Names

1. **Must be a noun**: "Review-to-SKU Converter," "Gate Locator," "Data Snapshot Recorder"
   - ❌ Wrong: "Cross-Border E-Commerce Consultant," "Side-Business Coach"
2. **Must include the output**: The role name hints at what the Agent must produce.
   - ✅ "Review → SKU Converter" (outputs SKUs)
   - ❌ "Review Analyst" (output is unclear)
3. **Avoid empty words such as "expert / consultant / coach"**: They are too broad, so the Agent does not know its boundary.

### 4.3 Four Types of Role-Boundary Prohibitions

| Type | Example prohibition |
|------|--------|
| Fabrication | Do not invent sales / conversion rates / platform rules / legal clauses |
| Out of scope | Do not make legal / medical / investment judgments |
| Wording | Do not use exaggerated marketing words ("miracle tool" / "unbelievable" / "guaranteed hit" / "absolute") |
| Style | Do not write motivational filler / vague advice / decisions on the user's behalf |

---

## 5. Main Task Section

### 5.1 Standard Template

```text
## Core tasks

Using [methods / tools / data sources],[output-focused verb][target].
**Core mission**:[one sentenceartifact-focused mission with critical verbs in bold].
**Success standard**:[one sentencewhat counts as success—can bevalidation].
```

### 5.2 Two Hard Rules for Task Verbs

1. **Must be an output verb**: produce / generate / output / score / classify / decide
   - ❌ Weak verbs: analyze / help / improve / suggest / refine
2. **Must include a verifiable success condition**:
   - ✅ "Produce three SKUs that can be listed immediately + a seven-day test calendar"
   - ❌ "Analyze reviews in depth"

### 5.3 Success Condition Patterns

| Pattern | Example |
|------|------|
| Quantity | "Produce at least three SKUs" |
| Time | "Can be completed in seven days" |
| Decision | "Choose one: continue / adjust / pause" |
| Falsifiable | "If X data is below Y, recommend adding samples" |

---

## 6. Information Input Section

### 6.1 Two Input Modes

A prompt should support **two modes**, chosen by the prompt's call context:

| Mode | Use case | Agent behavior |
|------|--------|--------|
| **One-time form** | n8n / API integration / the user has prepared the material | The user fills `{{ }}` / `___` once, and the Agent executes directly |
| **Interview** ⭐ | Conversation / the user does not know what to fill in / fields depend on each other | The Agent asks only one question at a time and waits for the answer before asking the next |

**Both modes must be declared in the prompt** so the Agent can choose based on whether the user filled in the form.

### 6.2 Standard Template (Including Interview Mode)

```text
## Input information

> **Placeholder conventions**:
> - `{{ variable name }}` = n8n / automation workflow variable (automatically injected)
> - `___` = conversational fill-in (filled once by the user)
> - `[optional]` = optional field
> - mark after a field `[interview]` = this field is proactively requested by the Agent in interview mode

**Field list**:
1. **[field 1]** (required): {{ variable }} — [one-sentence note]
2. **[field 2]** (required): ___ — [one-sentence note]
3. **[field 3]** (optional): [optional]

**Input posture judgment** (Agent's required first step):
- User filled ≥ 70% of required fields → **one-shot fill-in mode**. Mark missing fields as "unconfirmed" and continue execution.
- User filled fewer than 70% of required fields / fields entirely empty → **interview mode**.

**Interview-mode rules** (Agent must follow):
1. Ask only one question at a time. (Prohibited: "I need to know five things..." listing them all at once.)
2. When asking a question, provide 3–5 **options / examples / default values** so users can answer in seconds. (For example: "Target market is US / Europe / Southeast Asia / Other (please specify).")
3. Order fields by importance: required → optional; logical prerequisites → later dependencies.
4. After the user answers, **repeat the confirmation**: "Okay, you selected X. I will continue to the next question."
5. When all required fields are collected → enter the workflow. Optional fields may be asked in one shot before ending the interview.
6. If the user answers "do not know / skip / let AI decide" → apply fallback logic to this field (downgrade / mark as "unconfirmed").

**Missing-input fallback** (used in one-shot fill-in mode):
- [field X] is empty → [downgrade output / proactively switch to interview mode / refuse execution] — choose one of three.
- Insufficient data volume (< N item sample) → [expand the sample / downgrade to Y-level precision].
```

### 6.3 Five Hard Rules for Interview Mode

1. **Ask one question at a time**: Do not say "I need you to answer five questions." The Agent must ask them in sequence like an interviewer.
2. **Give choices**: Provide 3-5 choices / examples / defaults for every question to reduce the user's effort.
3. **Required → optional**: Ask all fields required for execution first, then ask optional fields together at the end.
4. **Repeat for confirmation**: After the user answers a field, the Agent repeats "Okay, you chose X" before asking the next question.
5. **Return to fallback**: If the user says "skip / let AI decide," apply the §6.2 fallback logic to that field and do not ask again.

### 6.4 Interview vs One-Time Form — When to Use Each

| Use case | Recommended mode |
|------|--------|
| n8n / Zapier / API call | One-time form |
| Skill / Command (triggered by the user with `/cmd args`) | One-time form |
| "Copy and use prompt" in a tutorial | One-time form (user replaces `___`) + interview fallback |
| Direct Claude / GPT conversation | Interview |
| First-time user / unfamiliar fields | Interview |
| Fields depend on each other (an earlier answer determines a later question) | Interview |
| User has uploaded a large amount of material | One-time form (use the material directly) |

### 6.5 Placeholder Rules

| Placeholder | Use case | How the value is added |
|--------|--------|--------|
| `{{ variable_name }}` | n8n / workflow | Replaced automatically by the system |
| `{{ $('node_name').item.json['field'] }}` | Complex n8n reference | Standard n8n syntax |
| `___` | Conversation / API | Filled in manually by the user |
| `[optional]` | Optional field | May be left blank after this label |

### 6.6 Three Fallback Policies for Missing Input

| Policy | When to use | Example |
|------|------|------|
| Reduced output | Field is not essential | Competitor data missing → still give a score but mark it "unconfirmed" |
| Ask the user | Field is essential but can be supplied | Sales direction missing → ask the user, then execute |
| Refuse to execute | Missing field would cause fabrication | Original review text missing → refuse to produce SKUs |

### 6.7 Arrangement of Large Source Material (Long-Context Use)

When the input contains a large amount of source material (documents, code, or review sets over about 20,000 tokens), arrange it using Anthropic's tested long-context rules. In this case, **move the source-material section to the front of the structure and keep the other seven sections in the same order**:

1. **Put source material first and instructions after it**: Place long documents at the top of the prompt and task instructions below them. Material first and instructions later can significantly improve performance (Anthropic tests showed gains of up to 30% in multi-document cases).
2. **Wrap source material in XML tags**: Wrap several sources in `<documents>` / `<document>` structures with `<source>` and `<document_content>` child tags. This physically separates "source material" from "instructions" and prevents the model from treating sentences in the material as instructions (it is also the first defense against prompt injection).
3. **Quotes first**: Require the model to extract relevant original text into `<quotes>` tags before completing the task from those quotes. This forces evidence location first and reduces fabrication.

> Source: Anthropic, "Long context prompting tips." Normal short inputs at the form-field level do not need this arrangement; follow the §6.5 placeholder rules.

---

## 7. Workflow Section

### 7.1 Standard Template

```text
## Workflow

1. **[step name 1]**: [specific action + tool calls]. [count / quality threshold].
   **Reasoning requirements**: first organize in `<thinking>` tags [reasoning dimension], then output results.

2. **[step name 2]**: **Important requirement: [prohibited action / required action]**.
   Strictly follow [N] dimensions of analysis:
   - **[dimension 1]** ([English / abbreviation]): [what the author should inspect]
     - *Checks*: [list]
   - **[dimension 2]**: same as above

3. **[Complex steps with nested sub-frameworks]**:
   \`\`\`text
   [sub-framework name]:
   - field A: [how to fill it in]
   - field B: [how to fill it in]
   \`\`\`

4. **[Report generation]**: using the `output-standard` writing rules.
```

### 7.2 Five Hard Rules for the Workflow

1. **Every step includes the trio "action + dimension + warning."**
2. **Encode the method here**: Put the article's five dimensions / five steps / decision tree / thresholds into this section as tables / lists.
3. **Nest a subframework in a complex step**: A step may contain another `\`\`\`text` block (for example, prompt 2 embeds the "Standard Prompt Construction Framework" in Step 5).
4. **Reasoning-process requirement** (strongly marked by Anthropic): Add a `<thinking>` tag requirement to analysis steps.
5. **Bold warnings**: Mark key prohibitions with **Important requirement** or **Important reminder**.

### 7.3 How to Use the `<thinking>` Tag for Reasoning

```text
**Reasoning requirements**: First organize your reasoning inside `<thinking>` tags:
- Dimension 1: Is the current data sufficient?
- Dimension 2: What is the user's real intent?
- Dimension 3: Which type of rule applies?
Then output results.
```

Chain-of-thought asks the model to organize its basis before reaching a conclusion. It significantly improves accuracy in analysis, scoring, and decision tasks (source: Anthropic prompt-engineering document "Let Claude think").

**Choose by model reasoning mode**:

| Target model mode | Method |
|-------------|------|
| Native reasoning is off (normal chat model, API call with reasoning disabled) | Use the `<thinking>` tag above as a manual structure |
| Extended thinking is on (extended-thinking / reasoning model) | ❌ Do not require a manually written `<thinking>` tag. The model has a native reasoning channel, and repeating the structure wastes tokens. Instead, state **when to reason and which dimensions to consider** in the instruction (for example, "After receiving tool results, assess their quality before choosing the next step.") |

When `target_models` includes a reasoning model, state beside the metadata header which mode the prompt uses.

---

## 8. Examples / Sample Section (Industry Standard, Repeatedly Verified by 13 DeepSeek Examples)

### 8.1 Standard Template

```text
## Examples / templates

**Input example**:
- field 1: [specific value]
- field 2: [specific value]

**Expected output (excerpt)**:
\`\`\`
[One short, complete output segment of 3–5 lines]
\`\`\`

**Negative example (what does not qualify)**:
- ❌ [common error 1: too vague / too abstract / invented data]
- ❌ [common error 2: violates role boundaries]
```

### 8.2 Why It Is Required

- **All 13 official DeepSeek prompts contain both USER and sample-output sections.**
- **Anthropic's "Examples" is one of its five elements.**
- **Industry consensus on few-shot prompting**: One good example works better than ten lines describing rules.
- Negative examples help the Agent recognize boundaries.

### 8.3 Example Field Requirements

| Field | Required | Description |
|------|:---:|------|
| Input example | ✅ | At least one full input set |
| Expected output | ✅ | A short excerpt; full length is not needed |
| Negative example | ⚠️ Strongly recommended | At least two "unacceptable" comparisons |

---

## 9. Output Rules Section

### 9.1 Standard Template

```text
## output-standard:"XX"

**Strictly follow this structure.Total word count:[N words].**
**Output directly"XX",without an introduction, closing remarks, or explanations.**
**Prohibited globally**:[content prohibition1],[content prohibition2].

[Complete field-level skeleton — each field contains a subheading, word count, and filling guidance]

**Self-checklist (required before output)**:
- [ ] Word count meets target [N ± 10%]
- [ ] No introduction or closing remarks (must not contain "Okay, I will...")
- [ ] Every field has content (no empty fields)
- [ ] [Topic-specific check 1]
- [ ] [Topic-specific check 2]
- [ ] Role boundaries not exceeded (no invention / making decisions for users)
```

### 9.2 Five Hard Rules for Output Rules

1. **Field-level structure, not an abstract description**:
   - ❌ "Output structure: 1. Give the conclusion first 2. Analyze 3. Suggest"
   - ✅ "▌I. Five-dimension scorecard (with evidence + reasons for deductions) | ▌II. Top three dangerous deductions | ▌III. Three-level conclusion | ▌IV. Seven-day action vs pause list"
2. **Must include a hard word-count target**: "1,200 words total" / "about 150 words per paragraph"
3. **Must include "no introduction"**: Produce the result directly; do not say "Okay, I will..."
4. **Must include "global prohibitions"**: Content prohibitions that apply across fields
5. **Must include a self-check checklist**: The Agent checks its output before sending it (standard PMI Self-reflection element)

### 9.3 Self-Check Checklist Template

General checks (every prompt should include them):
- [ ] Word count meets the target
- [ ] No introduction or closing remarks
- [ ] Every field has content
- [ ] Role boundaries were respected (no fabrication / out-of-scope judgment)

Topic-specific checks (add based on the prompt topic):
- [ ] Every data point has an evidence source
- [ ] Fields involving platform rules say "Use the dashboard on the day of execution"
- [ ] Every SKU is supported by at least three original evidence quotes

---

## 10. Refusal Cases Section

### 10.1 Standard Template

```text
## Refusal scenarios

Refuse execution immediately for the following inputs (do not output a report; state the reason for refusal directly):
- [scenario 1: insufficient input data — below a threshold value]
- [scenario 2: input involves illegal, infringing, or hateful content]
- [scenario 3: input requirement outside role boundaries, for example requesting legal / medical / investment judgment]
- [scenario 4: input fields entirely empty or clearly test data]
```

### 10.2 Four Standard Refusal Types

| Type | Trigger | Refusal wording |
|------|--------|--------|
| Insufficient data | Essential field missing / samples below threshold | "There are fewer than N samples. Add X before calling again." |
| Ethical boundary | Illegal / infringing / hateful / fraudulent content | "This case is outside my capability boundary." |
| Role boundary | Requests specific legal / medical / investment advice | "Consult a licensed professional." |
| Test input | All fields blank / placeholders not replaced | "Unfilled placeholders were detected." |

### 10.3 Refusal vs Fallback

| Dimension | Fallback (§6 Missing Input) | Refusal (§10) |
|------|-----------------|-----------|
| Trigger | Nonessential field missing | Essential field missing / ethics / out of scope |
| Behavior | Reduced output + mark as unconfirmed | No output; give the refusal reason |
| Example | Competitor missing → still score | Reviews missing → refuse to produce SKUs |

---

## 11. Anti-Pattern Prohibitions (All Red Lines)

### 11.1 Nine Prohibited Actions

| # | Prohibition | Result when violated |
|---|------|--------|
| 1 | ❌ Paste an article's H2 list in reverse as the "output structure" | Root cause of repetitive prompts |
| 2 | ❌ Use generic roles such as "AI Side-Business Coach / Senior Consultant" | Same template applied across topics |
| 3 | ❌ Use empty verbs such as "analyze / help / improve" | Agent does not know the output |
| 4 | ❌ Do not turn input fields into variables (no `{{ }}` or `___`) | Cannot be reused in n8n / API |
| 5 | ❌ Output rules list only H2s without a field structure | Agent does not know the exact format |
| 6 | ❌ No example section (missing one positive + one negative example) | No few-shot example |
| 7 | ❌ No self-check checklist | Agent will not check its work |
| 8 | ❌ No role boundary | Agent may go out of scope |
| 9 | ❌ No hard word-count target | Agent output varies unpredictably in length |

### 11.2 Red Lines From Other Standards

Inherited red lines from other standards:
- Do not use exaggerated marketing words ("miracle tool" / "unbelievable" / "absolute") — from the matching workflow style library.
- Give numbers as ranges and label them "Use the dashboard on the day of execution" — from the **Platform Rules Standard**.
- Do not invent nonpublic data — from the matching workflow style library.
- Do not include real names / book titles unless the user explicitly authorizes them — from the matching workflow style library.

---

## 12. Length Rules

### 12.1 Total Length

| Type | Recommended range |
|------|--------|
| Standalone prompt file | 800-2,800 characters |
| Prompt section embedded in an article | 400-2,400 characters |
| Embedded in a Skill / Command | 800-2,400 characters |
| Embedded in a workflow (n8n / Codex App) | 600-2,400 characters |

> Basis for the length limit: Tested production prompts (13 official DeepSeek examples / the user's active source-material request report prompt / information-retrieval prompt) are all 2,000-2,400 characters. The Eight-Part Format + interview mode is naturally 30-50% longer than the v1 five-part format, so a 2,400-character limit is needed for the full structure.

### 12.2 Recommended Section Lengths

| Section | Recommended characters |
|----|--------|
| Metadata header | 100-200 (YAML) |
| Role + boundary | 80-150 |
| Main task | 50-100 |
| Information input | 100-250 (based on field count) |
| Workflow | 300-800 (largest section; encodes the method) |
| Examples / sample | 100-300 |
| Output rules | 200-500 |
| Refusal cases | 50-150 |

### 12.3 What to Do When It Is Too Long

If a prompt is over 2,500 characters, do these three things first:
1. Reduce the encoded method to a table (no long sentences).
2. Move the example section to an external link ("See examples/xxx.md for the full example").
3. Split several uses into several prompts (one responsibility per prompt).

---

## 13. Testing and Acceptance

### 13.1 Five Required Steps Before Release

1. **Peer review** (Anthropic Golden Rule): Give the prompt to someone unfamiliar with the task. If they cannot understand it in three minutes, rewrite it.
2. **Empty-input test**: Leave every `___` blank and see whether the Agent triggers refusal or fallback.
3. **Boundary test**: Deliberately ask the Agent to do something outside its role boundary (for example, ask a review-analysis prompt for legal advice).
4. **Repeat test**: Run the same input three times. The output structure should remain consistent (field order / field names).
5. **Cross-model test**: Run it once on every model in target_models.

### 13.2 Eight Acceptance Checks

- [ ] Metadata header is complete
- [ ] All eight sections are present
- [ ] Role name is a noun
- [ ] Task verb names an output
- [ ] Workflow encodes the method (rule table / dimensions / thresholds)
- [ ] Includes positive + negative examples
- [ ] Includes a self-check checklist + refusal cases
- [ ] Pass `../awp-meta-authoring-standard/std-style-language-contract.md` (in the AWP Standards Standard package): three-layer language contract / remove jargon / formalize speech / fidelity, clarity, and grace

---

## 14. Version Evolution

### 14.1 SemVer Use

| Change type | Version | Example |
|--------|------|------|
| Fix a field error / spelling | v1.0.1 | patch |
| Add / remove fields / change defaults | v1.1 | minor |
| Change role position / main method | v2.0 | major |

### 14.2 Retirement Process (Forward Only)

Overwrite the prompt during iteration and let git keep history. Do not pile retirement labels into the metadata header. When an entire prompt is retired and no caller uses it:

1. Overwrite the old file directly with the new version. ❌ Do not add `deprecated: true` or keep the old version in parallel for 1-2 months.
2. If the old version must be kept, archive it in the caller's own archive directory: `inbox/archive/{YYYYMM}/{YYYYMMDD}-prompt-{name}-replaced/`.
3. Change all caller references (workflow / Skill / API) to the new `prompt_id` at once.

> Follow the meta-standard's "no Deprecated intermediate state" rule: a prompt is either in use or retired and archived. There is no gray area of "deprecated but retained."

---

## 15. Quick Reference

| I want to | Read |
|---------|--------|
| Write a prompt from scratch | Copy the structure directly from `prompt-core-blank-template.md` |
| Rewrite a prompt made by reverse-pasting H2s | §1.3 Anti-Patterns + §3 Eight-Part Structure |
| Decide how strict to make a prompt | §1.2 Boundary with Writing Methodology |
| Write a prompt for an n8n workflow | §2.1 Metadata + §6.5 Placeholder Rules |
| Embed a prompt in a Skill / Command | §2.2 Short Metadata + §3 Eight-Part Structure |
| Design a role name | §4 Role Section (three hard rules) |
| Design the output format | §9 Output Rules (field-level structure) |
| Add examples | §8 Examples / Sample Section (DeepSeek pattern) |
| Work with missing input | §6.6 Fallback vs §10.3 Refusal |
| Test a prompt | §13 Five Testing and Acceptance Steps |

## Change Log

> Rolling window: keep the 10 most recent entries, each no more than 20 words.

| Date | Change |
|------|---------|
| 2026-07-10 | Long-context arrangement + reasoning routing + cache split |
| 2026-07-10 | Retirement process made consistent + language self-check added |
| 2026-05-21 | Added interview mode to information input |
| 2026-05-21 | Created Eight-Part Format Standard v1.0 |
