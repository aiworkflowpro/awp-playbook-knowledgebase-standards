---
document_id: awp-agent-cognitive-discipline/cognitive-discipline
language: en
publication: public
source_revision: 6
title: "AWP Cognitive Discipline Standard"
prerequisites: []
see_also: []
---

# AWP Cognitive Discipline Standard

> Rules that stop an agent from answering open-ended questions with the average of its training data. The standard names the agent's own predictable cognitive weaknesses — one weakness per chapter — and pairs each with exactly one rule that defeats it and a threshold the agent can check. A framework standard (weakness → rule → threshold → output gate), not a step-by-step workflow.

---

## ① Position

### Scope

| Question | Answer |
|----------|--------|
| What does this standard govern? | How an agent reasons about **open-ended discovery tasks**: finding a product direction, a market gap, a niche, a creative strategy, or a research question. It constrains thinking, not formatting. |
| What is the deliverable? | A validated answer **plus its evidence trail**: a discard list, an evidence ledger, a kill log, transfer notes, a competitor-scan log, market arithmetic, and a founder-fit matrix. |
| What is it NOT for? | Mechanical execution (renaming, formatting, calculation), tasks with one known-correct answer, and any task a more specific standard already governs. |

**Scope guard**: ⚠️ When the task is out of scope, the agent must say so and must not apply these gates. When the task is in scope, the agent must apply all seven rules — see § ③ for the fixed order. A user may opt out of a gate for one run; that opt-out is a deviation and must be recorded (see § ⑤ Shared Rules).

### Relationship With Other Standards

| Standard | Relationship |
|----------|--------------|
| Prompt Standard | A prompt may bind itself to rules R1–R7 **by ID** without restating their text. See `prompt/prompt.md`. |
| Skill Standard | A Skill's discovery phase may declare this standard as its thinking discipline. See `skill/skill.md`. |
| Meta-standard | This file follows the body skeleton of the meta-standard. Where this file is silent, the meta-standard governs: `meta-authoring/authoring.md § Body File Structure`. |

### Skeleton Compliance

This body follows the meta-standard skeleton one section at a time:

| Meta-standard skeleton role | This file's section |
|----------------------------|---------------------|
| ① Position | § ① Position |
| ② Design Philosophy | § ② Design Philosophy |
| ③ Organization Framework | § ③ Organization Framework (chain model + overview table) |
| ④ Section Details | § ④ Section Details (chapters W1–W7, then the output gate) |
| ⑤ Shared Rules | § ⑤ Shared Rules |
| ⑥ Checklist | § ⑥ Checklist |
| ⑦ Build Procedure | § ⑦ Build Procedure |

---

## ② Design Philosophy

### The Failure This Standard Corrects

An agent asked an open-ended question returns the **mode of its training distribution**, not a considered answer. "Find a SaaS direction" reliably produces "AI writing assistant, AI video editor, AI scheduler" — not because those are good answers, but because they are the most probable strings in the data the agent was trained on. Each one already has mature competitors.

The failure is structural, so exhortation cannot fix it. "Be more creative" or "think harder" changes the wording of the output, not its position in the probability distribution. Only **constraints on the process** — what the agent must search, count, cite, and destroy before it is allowed to answer — move the output away from the average.

### Why the Agent Cannot Police Itself

Three properties of a language model make naive self-checking useless:

| Property | Consequence |
|----------|-------------|
| The agent cannot tell what it *read* from what it *invented* | Pain points it fabricates feel identical to ones it remembers. "Does this pain exist?" has no honest answer inside the model — the question must be answered with a source artifact outside the model |
| The agent cannot tell a novel idea from a forgotten one | Every idea it generates feels fresh to it, because its own memory of the training corpus is not addressable. "Is this obvious?" must be answered by counting, not by feeling |
| The agent argues to keep its own output alive | Once an idea is on the page, the agent's next tokens continue it. It will not volunteer reasons to stop. Reasons to stop must be manufactured as a separate, forced step |

### Principles

| Principle | Why |
|-----------|-----|
| One weakness → one rule → one checkable gate | Traceability. If the answer is still bad, exactly one gate leaked. Blame never falls on "the agent was not creative enough" |
| Evidence over imagination | Every factual claim must carry a source artifact, or it is deleted. The agent may still be wrong about the source, but it can no longer pass off invention as fact |
| Rules over exhortation | A rule an agent can verify beats advice it can only intend. Every gate below ends in a count, a list, or a dated log — things an agent or a person can check |
| The deliverable is the audit trail | A "good idea" with no trail is noncompliant, because the trail is the only way to tell it apart from an obvious idea. The list at the end is a summary of the trail |
| Cheap objective gates stay hard; expensive judgment stays criteria | Counting sources and recording searches costs almost nothing and is unambiguous, so those thresholds are hard numbers. Taste judgments (transfer fit, objection strength) stay criteria. See `meta-authoring/authoring.md § Criteria Over Hard Numbers` |

### Why Seven Weaknesses and Seven Rules

Each weakness below is a distinct, nameable failure with a distinct mechanism. Each got exactly one rule, so a violation maps to one chapter. Adding a rule costs real attention budget, so a proposed eighth rule must first prove that one of the seven does not already cover the failure — see `meta-authoring/authoring.md § Governance`. The rules are also ordered so that no rule depends on output of a later rule.

---

## ③ Organization Framework

### Chain Model

Every in-scope answer runs through one fixed pipeline. Each stage is one weakness that would otherwise corrupt the answer, one rule that defeats it, and one artifact that proves the rule ran:

```
R7 Context     → capture who is asking (founder context + exclusions)
R1 Purge       → quarantine the training-data average (≥ 30 discarded)
R2 Evidence    → build candidates from cited complaints, not imagination
R3 Kill        → force every candidate through an adversarial round (≤ 40% survive)
R4 Transfer    → bring in one mechanism from an unrelated industry
R5 Novelty     → verify with a dated competitor scan; state the strongest rival
R6 Size        → show market arithmetic + dated trend signals
Output gate    → apply the quality criteria; only then write the final answer
```

The chapter numbers W1–W7 name the weaknesses for traceability. The **execution order is fixed** as shown above: R7 runs first even though its chapter is last, because context shapes every later stage. ❌ An agent must not reorder the pipeline to skip a stage.

### Overview Table

| # | Weakness (what corrupts the answer) | Rule (what the agent must do) | Checkable gate (artifact + threshold) |
|---|-------------------------------------|------------------------------|----------------------------------------|
| W1 | **Mode-seeking** — first ideas are the crowd's average | R1 · Purge the obvious | Quarantine list of ≥ 30 directions; zero overlap with the final answer |
| W2 | **Fabricated support** — invented pains and quotes feel real | R2 · Evidence ledger | Ledger: every factual claim has source URL + date + verbatim quote; ≥ 3 sources per direction from ≥ 2 platforms |
| W3 | **Self-confirmation** — the agent defends its own output | R3 · Kill round | Kill log: ≥ 3 attacks per candidate, survival rate ≤ 40% |
| W4 | **In-domain search** — answers come only from the problem's own industry | R4 · Cross-domain transfer | Transfer note: ≥ 1 mechanism adopted from an unrelated industry per direction |
| W5 | **Competitor blindness** — "novel" ideas already exist, funded | R5 · Search-verified novelty | Dated scan log + strongest-rival statement; auto-fail on a funded direct competitor |
| W6 | **Scale blindness** — no sense of how many users or how much money | R6 · Market arithmetic | Arithmetic block to a year-1 SOM + ≥ 2 dated trend signals; default gate 1,000 payers × $50/mo |
| W7 | **User blindness** — the same answer for every person | R7 · Context lock-in | Context table recorded first; founder-fit matrix; LOW fit dropped unless the user overrides |
| Gate | **Unverified output** — a list with no trail | Quality criteria | All criteria pass; artifact appendix present; language gate passed |

---

## ④ Section Details

Each weakness chapter below has the same shape: what the weakness is → why it produces bad answers → the rule that defeats it → the checkable gate → good/bad comparison → boundary.

### W1 · Mode-seeking — R1 · Purge the Obvious

**The weakness.** The agent's first ideas are the statistical center of everything it has read. "AI writing assistant" appears first because thousands of posts, decks, and threads say exactly that. Ideas that arrive without effort are red ocean by definition.

**The rule.** ✅ The agent must dump its obvious ideas on purpose, before doing any research, into a **quarantine list** of at least **30** candidate directions, written in one pass with no filtering and no commentary. ❌ The agent must not present any quarantined item as a recommendation. ⚠️ A quarantined item may re-enter only through the resurrection path: real evidence discovered during R2 must independently re-raise it, after which it must be evidence-backed (R2's gate) and pass R3–R6 like any other candidate.

| Good | Bad | Reason |
|------|-----|--------|
| "Discarded batch (32): AI writing assistant, AI video editor, AI thumbnail generator, AI scheduler, …" listed before research | Agent answers with the first three ideas it thought of | The former proves the agent saw the obvious and refused it; the latter *is* the obvious |
| A quarantined idea re-enters with a source and survives the kill round | A quarantined idea re-enters "because on reflection it could work" | Resurrection without evidence is W1 sneaking back through the front door |

**Checkable gate.** Artifact: the quarantine list. Threshold: ≥ 30 entries, timestamped before the research phase; final directions share **zero** items with it.

**Boundary.** Purging is not ignoring inspiration. It is a refusal to *report* the average — the agent may think anything, but it may only *recommend* what passed the pipeline.

### W2 · Fabricated Support — R2 · Evidence Ledger

**The weakness.** Asked "what problems do people have?", the agent generates plausible-sounding pains from its training data. It cannot distinguish a pain it read about from a pain it invented, and neither can the reader — until the claim is checked against a real source.

**The rule.** ✅ Every factual claim that appears anywhere in the deliverable — a pain, a demand signal, a competitor fact, a market number, a trend — must have an entry in the **evidence ledger**: claim → source URL → platform → retrieval date → verbatim quote → direction ID. ✅ The agent must search for evidence, not generate it. ❌ A claim with no ledger entry must be deleted from the answer before output, even if the agent believes it is true. ⚠️ The agent's own arithmetic and its own reasoning need no citation; the inputs to that arithmetic do.

| Good | Bad | Reason |
|------|-----|--------|
| "Pain: manually re-uploading each video to every platform (source: r/NewTubers thread, URL, retrieved 2026-09-06, quote: …)" | "Creators waste hours re-uploading to every platform" | The former can be verified; the latter may be invented and feels identical when it is |
| "3 sources: Reddit thread, HN comment, G2 review — 2 platforms" | "Creators everywhere complain about this" | The former meets the count; the latter is a vibe, not evidence |

**Checkable gate.** Artifact: the evidence ledger. Threshold: a direction is *evidence-backed* only with ≥ 3 independent sources from ≥ 2 distinct platforms; the ledger is printed in the deliverable appendix.

**Boundary.** The ledger is evidence, not argument. A source proves that a complaint exists; it does not prove the complaint matters or that people pay to fix it — that is R6's job.

### W3 · Self-Confirmation — R3 · Kill Round

**The weakness.** Once a candidate is on the page, the agent's next tokens continue and defend it. Asked "is this good?", it finds reasons to say yes. Nothing in its training rewards the agent for killing its own output.

**The rule.** ✅ Every candidate that survives R2 must enter a **kill round**: at least **three** forced attacks per candidate — (a) a live search for anyone already doing it, (b) the strongest investor objection written out in full, (c) one named, concrete failure mode (why users churn, why it cannot reach users, or its single point of failure). ✅ The result of every attack is recorded in the **kill log**. ✅ The agent must report the survival rate: entered vs. survived. ❌ "I believe this idea is good" is not an allowed verdict; it must be replaced by the attacks above.

| Good | Bad | Reason |
|------|-----|--------|
| "Kill log — Idea 4: (a) found 2 adjacent tools, (b) objection: creators churn after 3 months, (c) failure: depends on one platform's API → died" | "Idea 4 is promising and worth exploring" | The former records why the idea died; the latter records nothing checkable |
| "Survival rate 3/10 (30%) — 7 killed" | "After filtering, the best directions are…" | The former can be audited; the latter hides how many were considered |

**Checkable gate.** Artifact: the kill log. Threshold: survival rate ≤ 40%; each survivor shows all three attacks with outcomes. If no candidate survives, the compliant deliverable is: "No direction passes — here is what failed and what would change that."

**Boundary.** The kill round judges a candidate *as specified*. A candidate that dies because of its distribution model may survive as a different variant; the variant is a new candidate and must be re-killed.

### W4 · In-Domain Search — R4 · Cross-Domain Transfer

**The weakness.** The agent searches for solutions inside the industry the problem lives in. If the pain is in content creation, it looks at other content tools — while the pattern (say, "many small artifacts must be assembled under deadline") may already be solved in logistics, publishing, or e-commerce.

**The rule.** ✅ For every survivor of R3, the agent must abstract the operational pattern ("what is really happening here, in any industry") and search at least **two unrelated industries** for how they handle that pattern. ✅ At least **one** mechanism from an unrelated industry must be adopted into the direction and recorded in a **transfer note**: pattern → industries searched → mechanism adopted → source industry → how it adapts. ❌ A direction whose only justification is "tools already exist for this in this industry" must not be reported as novel.

| Good | Bad | Reason |
|------|-----|--------|
| "Pattern: reviewers must judge 200 submissions weekly. Publishing: magazine fact-check queues. Adopted: batched triage with forced breaks → creator collab review tool" | "It's like Grammarly but for collabs" | The former shows a search outside the industry; the latter never left it |
| Transfer note lists industries searched (logistics, healthcare) | "We took inspiration from other industries" | The former is checkable; the latter is decoration |

**Checkable gate.** Artifact: transfer note per direction. Threshold: ≥ 1 adopted mechanism per direction with a named source industry.

**Boundary.** Transfer is enrichment of a surviving candidate, not a source of new candidates. A mechanism must survive contact with the actual users the candidate serves — transfer notes that fail R6's arithmetic are discarded with the direction.

### W5 · Competitor Blindness — R5 · Search-Verified Novelty

**The weakness.** The agent does not know which products exist, which are funded, and which markets are saturated. It generates in a vacuum and reports "novel" ideas that a funded company already executes.

**The rule.** ✅ Every survivor must pass a **novelty scan**: dated searches with recorded queries and top results, checking both the obvious keyword and the problem-phrasing ("I spend hours doing X" rather than only "X software"). ✅ The deliverable must state the **strongest competitor found** and the specific reason it does not already own the space. ✅ The scan results are logged with the date. ❌ "No competitors found" without a scan log is forbidden.

**Hard bar.** ❌ A direction auto-fails when the scan finds a **direct competitor with institutional funding** (more than $10M raised) or more than ~100,000 users — unless the direction's wedge is a specific, verifiable difference, in which case it must survive a second kill round on that wedge alone.

| Good | Bad | Reason |
|------|-----|--------|
| "Scan 2026-09-06, query 'collaboration review tool creators' — top 5: … Strongest rival: X (Series A, does Y). Reason unoccupied: X requires team accounts; our user is solo" | "No direct competitors exist — this is a blue ocean" | The former states the rival and the wedge; the latter is a claim the model cannot support from memory |
| Second kill round on the wedge recorded | Wedge asserted once and never attacked | A wedge that survives a dedicated attack is a real difference |

**Checkable gate.** Artifact: dated scan log + strongest-rival statement per direction. Threshold: log present with date and queries; strongest rival named; no funded direct competitor, or a wedge that survived its second kill round.

**Boundary.** Novelty is about *occupancy*, not *uniqueness*. A direction may be unoccupied yet still fail R6 (too small) — the stages judge different things.

### W6 · Scale Blindness — R6 · Market Arithmetic

**The weakness.** The agent recommends ideas without knowing whether enough people with enough money exist. An idea can be genuinely novel and still be too small to matter.

**The rule.** ✅ Every direction that reaches this stage must show **market arithmetic**: a chain from evidence to a year-1 serviceable market — number of realistically reachable payers (each assumption labeled as assumption or evidence) × price per month = revenue. ✅ The agent must record at least **two dated trend signals** (search-volume or community-frequency observations) and state whether the pain is rising, stable, or falling. ✅ The default gate for a solo-founder SaaS: **1,000 reachable payers × $50/month** (≈ $600K/year revenue potential). ⚠️ A smaller deliberate niche is allowed only with the user's recorded sign-off; a *falling* pain also requires sign-off. ❌ "Huge market" with no arithmetic is forbidden.

| Good | Bad | Reason |
|------|-----|--------|
| "Reachable: 4 creator communities × ~1,500 active members × 2% conversion ≈ 120 payers… below gate → direction dies" | "Creators are a massive market" | The former can be checked and fails honestly; the latter cannot be checked at all |
| "Trend: 'repurpose podcast' queries up over 12 months (2 dated signals)" | "This pain is growing" | The former cites signals; the latter asserts a trend |

**Checkable gate.** Artifact: arithmetic block + trend signals per direction. Threshold: arithmetic present with assumptions labeled; ≥ 2 dated trend signals; gate met or user sign-off recorded.

**Boundary.** Arithmetic sizes an opportunity; it does not validate it. A direction can pass R6 and still die in the output gate on founder fit (R7) or on missing evidence (R2).

### W7 · User Blindness — R7 · Context Lock-In

**The weakness.** The agent gives the same answer to everyone. A solo bootstrapper with no code skills receives the same directions as a funded team. The agent never asks who is asking.

**The rule.** ✅ Before any research, the agent must capture a **context table** in the user's own words: what the user can build and ship, their available time and budget, who their audience is, what they have already tried and ruled out, and any **exclusions** ("never suggest X"). ✅ Every candidate that survives the pipeline is scored **founder-fit: HIGH / MEDIUM / LOW** against that table. ✅ The fit matrix is part of the deliverable. ❌ A LOW-fit direction must be dropped unless the user explicitly overrides; a direction in an exclusion category must never be recommended. ⚠️ If the user answers "I don't know" to a context question, the agent records the gap and flags fit scores as provisional rather than inventing an answer.

| Good | Bad | Reason |
|------|-----|--------|
| "Context: solo, ships no-code MVPs, 10 h/week, audience = newsletter readers, excluded: anything requiring a mobile app. Fit: Direction A HIGH, B MEDIUM, C LOW (needs native app) → C dropped" | Directions chosen purely by market size | The former serves *this* user; the latter serves a generic one |
| "Fit provisional — user has not stated budget" | "Budget is fine for a solo founder" | The former flags the gap; the latter fabricates context (W2's weakness applied to the user) |

**Checkable gate.** Artifact: context table (recorded before R1) + founder-fit matrix. Threshold: context table present with the five fields; every reported direction has a fit score; no LOW fit and no excluded direction in the answer.

**Boundary.** Context is captured, not guessed. The agent may prompt for context, but every field value comes from the user; a missing field is a flagged gap, never a default.

### Output Gate · Quality Criteria

The final answer is compliant only if **all** criteria pass:

| Criterion | Pass threshold | Fails when |
|-----------|----------------|------------|
| Directions delivered | ≥ 3 for an open market scan; ≥ 1 for a directed question — or the explicit "none passed" answer with reasons | An empty list with no reasons, or a list padded to reach a count |
| No training-data average | Zero final directions appear in the R1 quarantine list | Any overlap |
| Evidence-backed | Every direction: ≥ 3 ledger sources from ≥ 2 platforms; every factual claim cited | A claim without a ledger entry anywhere in the deliverable |
| Survived attack | Every direction appears in the kill log; survival rate ≤ 40% | Survival rate above 40%, or a survivor without all three attacks |
| Cross-domain | ≥ 1 transfer mechanism per direction, source industry named | No transfer note |
| Occupancy checked | Dated scan log + strongest-rival statement per direction | "No competitors" with no scan log |
| Sized | Arithmetic to a year-1 SOM with labeled assumptions; gate met or sign-off recorded; ≥ 2 dated trend signals | Missing arithmetic or trend signals |
| Founder fit | Context table first; fit matrix present; no LOW fit, no excluded direction | Fit matrix absent or a LOW/excluded direction reported |
| Audit trail | Appendix lists every artifact or records each skipped gate as a deviation | An artifact silently missing |
| Language gate | Prose passes `language-style.md` | Jargon, template-speak, or unclear plain English |

---

## ⑤ Shared Rules

The following rules apply to **every** run of this standard and are not repeated in each chapter.

- ⚠️ **Fixed pipeline, recorded skips.** All seven rules run in the order in § ③. A user may opt out of a gate for a single run; the agent must then mark the deviation in the deliverable: `⚠️ Deviation: R{n} / Reason: user request for a fast pass`. ❌ A gate must not be skipped silently.
- ✅ **Artifacts appendix.** The deliverable lists every gate artifact (quarantine list, evidence ledger, kill log, transfer notes, scan logs, arithmetic, fit matrix) — or states that a gate was skipped and why. ❌ An artifact must not be summarized away into prose.
- ✅ **One rule, one location.** Prompts and Skills bind to this standard by rule ID (R1–R7). ❌ They must not restate the rule text; when a rule changes, the copies would drift. See `meta-authoring/authoring.md § One Rule, One Location`.
- ✅ **Language.** Final deliverable prose passes `language-style.md`: three-layer language contract / no jargon / formalized spoken wording / faithfulness, clarity, and grace.
- ⚠️ **Threshold ownership.** The numeric gates (30 discarded, 3 sources, 40%, 1,000 × $50) are defaults for solo-founder discovery work. A user may set different numbers for their own work; the changed numbers are recorded in the context table, not edited into this file.
- ❌ **No standard-content rules here.** Naming, file metadata, and change logs follow the general standard and the meta-standard; this file does not repeat them.

---

## ⑥ Checklist

Use this checklist to verify a completed run.

**Before start**
- [ ] Scope checked: this is an open-ended discovery task; no more specific standard governs it
- [ ] Context table recorded in the user's own words: skills, time, budget, audience, tried-and-ruled-out, exclusions (R7)
- [ ] Quarantine list of ≥ 30 obvious directions written before research, timestamped (R1)

**During the run**
- [ ] Every factual claim has an evidence ledger entry: source URL, platform, retrieval date, verbatim quote (R2)
- [ ] Every candidate attacked ≥ 3 ways in the kill log: competitor search, investor objection, named failure mode (R3)
- [ ] Transfer notes: ≥ 2 unrelated industries searched, ≥ 1 mechanism adopted per survivor (R4)
- [ ] Novelty scan run with recorded queries and date; strongest rival named per survivor (R5)
- [ ] Arithmetic blocks written with assumptions labeled; ≥ 2 dated trend signals (R6)

**Before output**
- [ ] ≥ 3 directions (open scan) or ≥ 1 (directed question) — or the explicit "none passed" answer
- [ ] No final direction overlaps the quarantine list (R1)
- [ ] Survival rate ≤ 40% and reported (entered vs. survived) (R3)
- [ ] Founder-fit matrix present; no LOW fit or excluded direction reported (R7)
- [ ] Artifact appendix lists every gate's artifact or records each skip as a deviation (§ ⑤)
- [ ] Passed `language-style.md`: three-layer language contract / no jargon / formalized spoken wording / faithfulness, clarity, and grace

---

## ⑦ Build Procedure

This section tells the agent how to produce a compliant deliverable from scratch through a guided run.

### Interview Questions (ask one at a time)

| # | Ask | Maps to |
|---|-----|---------|
| 1 | "Who is this for — what can you actually build and ship?" | Context table, fit scoring (R7) |
| 2 | "How much time per week and budget do you have?" | Context table, R6 gate sign-off |
| 3 | "Who is your audience or customer, in your words?" | Context table |
| 4 | "What have you already tried, and what did you rule out?" | Context table exclusions |
| 5 | "Is there any direction you refuse to consider?" | Exclusions (R7) |
| 6 | "This run defaults to a 1,000-payers × $50/month gate — is that right for you?" | R6 arithmetic gate |

### Agent Rules

- Write the context table in the user's own words — never rewrite it into template language.
- If an answer is thin, ask one follow-up, then proceed with what you have and flag the gap as provisional.
- If a question gets "I don't know" or "not yet", record the missing field and continue; fit scores for that run are provisional.
- Run the pipeline in the fixed order R7 → R1 → R2 → R3 → R4 → R5 → R6 → output gate. ❌ Do not reorder.
- If the user opts out of a gate, record it as a deviation in the deliverable.
- Never present a quarantined item, an uncited claim, or an unkilled idea in the final answer.

### Build Verification

- Print the artifact tree: quarantine list, evidence ledger, kill log, transfer notes, scan logs, arithmetic, fit matrix.
- Check the deliverable against § ⑥ Checklist item by item.
- Effect demo: run the same question without this standard, then with it; the gated answer must not repeat the ungated answer's directions, and every claim in it must resolve to a ledger entry.
