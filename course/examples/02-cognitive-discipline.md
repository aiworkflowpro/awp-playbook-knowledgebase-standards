---
document_id: awp-agent-cognitive-discipline/cognitive-discipline
language: en
publication: public
source_revision: 5
title: "Cognitive Discipline Standard"
prerequisites: []
see_also: []
---

# Cognitive Discipline Standard

> Rules that force an AI agent to think beyond its training-data average. This standard defines the agent's cognitive weaknesses and the constraints that overcome them. It does not prescribe a step-by-step workflow — it defines what the agent **must and must not do** when thinking about open-ended problems like product ideation, market analysis, or opportunity discovery.

---

## ① Position

### Scope

| Question | Answer |
|----------|--------|
| What does this standard govern? | How an agent reasons about open-ended discovery tasks — product directions, market gaps, niche analysis, creative strategy |
| What is the deliverable? | Validated directions backed by real evidence, not a list of ideas generated from training data |
| What is it NOT for? | Mechanical execution (formatting, calculation, file operations) or tasks with known correct answers |

### Relationship With Other Standards

| Standard | Relationship |
|----------|-------------|
| Prompt Standard | A prompt may reference this standard's rules as its workflow constraints |
| Skill Standard | A Skill may embed this standard as the thinking discipline for its discovery phase |
| Workflow Standard | A workflow implementing this standard's rules is one possible execution path, not the only one |

---

## ② The Problem: Seven Cognitive Weaknesses of Language Models

An agent asked to "find me a product direction" will produce plausible-sounding ideas that are almost always obvious, unvalidated, and already occupied. This is not a bug — it is the nature of how language models work. These seven weaknesses are predictable and correctable.

### W1: Training-Data Average

The agent's first ideas are the statistical center of everything it has read. "Build an AI writing assistant" appears first because thousands of blog posts, pitch decks, and Reddit threads say exactly that. These ideas are red ocean by definition.

**Rule**: Any idea that comes to the agent immediately — without effort, without searching — must be treated as the training-data average and discarded. The agent must generate and discard its first 30 directions before doing real work.

### W2: Imagination as Evidence

When asked "what problems do people have?", the agent invents plausible-sounding pain points from its training data. These pain points may or may not exist in reality. The agent cannot distinguish between a pain it read about and a pain it fabricated.

**Rule**: Pain points must come from real, citable complaints — Reddit threads, G2 reviews, Upwork job posts, App Store reviews. The agent must search for evidence, not generate it. Every pain point must include a source URL or citation.

### W3: Self-Validation Bias

The agent naturally wants to confirm its own ideas. When asked "is this idea good?", it finds reasons to say yes. It does not instinctively try to kill its own ideas.

**Rule**: The agent must actively try to disprove every idea it generates. It must search for existing competitors, invent investor objections, and find reasons the idea will fail. Only ideas that survive active attack are valid.

### W4: Single-Domain Blindness

The agent searches for solutions within the same industry the problem lives in. If the problem is in content creation, the agent looks at other content tools. It does not think to look at manufacturing, logistics, healthcare, or agriculture for solution patterns.

**Rule**: For every problem, the agent must identify the abstract operational pattern (e.g., "reviewing many items under time pressure") and search for how other industries solve that same pattern. Blue ocean opportunities live at the intersection of industries, not within one.

### W5: No Market Awareness

The agent does not know which products exist, which are well-funded, and which markets are saturated. It generates ideas in a vacuum and presents them as novel when a $100M company already does exactly that.

**Rule**: Every idea must be validated against real market data. The agent must search for competitors and verify that no well-funded direct competitor exists. A direction is only blue ocean if a web search confirms it.

### W6: No Sense of Scale

The agent recommends ideas without knowing if the market is large enough to sustain a business. An idea might be genuinely novel but only 50 people in the world need it.

**Rule**: Every direction must pass a market size test. For solo founders, the minimum is: can you find 1,000 users willing to pay $50/month? (1,000 × $50 = $50K MRR = $600K ARR). Below this, the market is too small.

### W7: No Founder Context

The agent gives the same recommendations regardless of who is asking. A developer gets the same ideas as a designer. A solo bootstrapper gets the same ideas as a funded team. The agent does not factor in the user's strengths, constraints, or situation.

**Rule**: Before generating any directions, the agent must understand the user's domain, target audience, personal strengths, constraints (solo/team, budget, timeline), and directions to exclude. All subsequent thinking must be filtered through this context.

---

## ③ The Standard: Nine Rules

These nine rules are the specification. An agent following this standard must obey all nine. The order of application is flexible — the agent may apply them in whatever sequence fits the task. But none may be skipped.

| # | Rule | Overcomes | Constraint |
|---|------|-----------|-----------|
| R1 | **Discard the obvious** | W1 | Generate and discard the first 30 directions before doing real work |
| R2 | **Evidence only** | W2 | Every pain point must cite a real source (URL, review, data point). No imagined pain points |
| R3 | **Kill before build** | W3 | Actively try to disprove every idea. Search for competitors, invent objections, find failure modes |
| R4 | **Cross-domain search** | W4 | For every problem, find how an unrelated industry solves the same abstract pattern |
| R5 | **Verify blue ocean** | W5 | Every final direction must include a web search proving no direct competitor exists |
| R6 | **Quantify the market** | W6 | Every direction must pass: SOM ≥ $100K ARR, 1000×$50 test |
| R7 | **Know the founder** | W7 | Collect the user's domain, strengths, constraints, and exclusions before generating anything |
| R8 | **Measure daily friction** | All | A direction is only worth pursuing if the pain is daily, measurable, and people would pay to fix it |
| R9 | **Trend-aware scoring** | W1+W6 | Favor growing pains over shrinking ones. A pain that is increasing in frequency is a better opportunity than a static one |

---

## ④ Quality Criteria

An output that follows this standard meets all of the following:

| Criterion | Threshold |
|-----------|-----------|
| Directions delivered | ≥ 3 validated directions |
| Each direction has evidence | ≥ 3 independent real complaints cited with sources |
| Gap between demand and supply | Gap Score ≥ 7/10 (demand signals ÷ supply level) |
| Blue ocean verified | Web search confirms no direct well-funded competitor |
| Market size sufficient | SOM ≥ $100K ARR, passes 1000×$50 test |
| Friction is sharp | Friction Score ≥ 18/25 (frequency + duration + workaround ugliness + articulability + willingness to pay) |
| Founder fit assessed | Each direction rated HIGH / MEDIUM / LOW against user's stated strengths |
| Validation ready | Each direction includes 5 Mom Test interview questions (ask about past behavior, not hypotheticals) |
| No training-data average | Final directions do not overlap with the discarded first-batch list |

---

## ⑤ Anti-Patterns

| Without this standard | With this standard |
|---|---|
| "Build an AI writing assistant" (obvious, crowded) | Discarded by R1 — training-data average |
| Agent invents pain points that sound real but aren't | Blocked by R2 — must cite real sources |
| Agent says "this is a great idea" without checking competitors | Blocked by R3 + R5 — must try to kill it and verify blue ocean |
| All ideas come from the same industry | Blocked by R4 — must search other industries |
| Interesting idea but 50 people need it | Blocked by R6 — SOM < $100K |
| Same recommendations for a developer and a designer | Blocked by R7 — must know the founder first |
| "Would you use this?" validation (hypothetical) | Blocked by R8 — Mom Test asks about past behavior |
| Agent recommends a direction in a declining market | Blocked by R9 — trend weight penalizes shrinking pain |

---

## ⑥ Scoring Reference

### Gap Score (R2 + R5)

```
Gap Score = (Demand signals normalized to 10) ÷ (Supply score) × 10
```

| Supply level | Score |
|-------------|-------|
| No tool exists | 1 |
| One niche tool, poorly rated | 3 |
| 2-3 tools, mixed reviews | 5 |
| Multiple well-funded tools | 7 |
| Dominant market leader | 10 |

### Friction Score (R8)

| Dimension | 5 | 3 | 1 |
|-----------|---|---|---|
| Frequency | Daily | Weekly | Monthly |
| Duration per occurrence | >30 min | 10-30 min | <10 min |
| Workaround ugliness | No workaround | Spreadsheet/manual | Partial tool exists |
| Articulability | User describes clearly | Vaguely | Cannot describe |
| Willingness to pay | "Shut up and take my money" | "Maybe" | "Only if free" |

Threshold: ≥ 18/25.

### Trend Weight (R9)

```
Adjusted Gap Score = Gap Score × Trend Multiplier
```

| Google Trends (past 12 months) | Multiplier |
|-------------------------------|-----------|
| Rising | ×1.3 |
| Stable | ×1.0 |
| Declining | ×0.7 |

### Mom Test Questions Template (R8)

1. "Last time you [did the painful task], walk me through what happened step by step."
2. "How much time did that take? How often does it happen?"
3. "What did you try before you settled on [current workaround]?"
4. "What would you do if [current workaround] stopped working tomorrow?"
5. "Have you ever paid someone to handle [this task] for you? How much?"

---

## ⑦ Checklist

**Before starting**:
- [ ] User's domain, target audience, strengths, constraints, and exclusions collected (R7)
- [ ] First batch of obvious ideas generated and discarded (R1)

**During analysis**:
- [ ] Every pain point cites a real source with URL (R2)
- [ ] Every idea has been actively attacked — competitors searched, objections raised (R3)
- [ ] Cross-domain solution patterns explored for every pain cluster (R4)
- [ ] Google Trends checked for trend direction (R9)

**Before output**:
- [ ] ≥ 3 directions surviving
- [ ] Each direction: Gap Score ≥ 7, Friction ≥ 18, SOM ≥ $100K
- [ ] Each direction: blue ocean verified via web search (R5)
- [ ] Each direction: founder fit assessed (R7)
- [ ] Each direction: 5 Mom Test questions written (R8)
- [ ] No direction overlaps with the discarded first-batch list (R1)

---

## ⑧ Testing

| Test | Method | Pass condition |
|------|--------|---------------|
| Obviousness filter | Run with a generic domain ("AI tools"). Check if discarded list covers well-known categories | Top-10 ProductHunt AI categories appear in the discarded list |
| Evidence quality | Spot-check 5 cited sources — do the URLs exist and say what the report claims? | 4/5 sources verify |
| Kill effectiveness | Count ideas entered vs. survived. Survival rate should be < 40% | ≤ 40% survival |
| Blue ocean verification | For each final direction, independently search the same terms. Confirm no direct competitor | 0 direct competitors for "confirmed" directions |
| Cross-model consistency | Run the same task on two different models. Both should pass all quality criteria | Both runs produce ≥ 3 directions passing all thresholds |

---

## ⑨ Sources

| Methodology | Source | Used in |
|------------|--------|---------|
| Inversion / discard first batch | Charlie Munger, *Poor Charlie's Almanack* | R1 |
| Complaint data mining | BigIdeasDB (1M+ complaints across Reddit, G2, Capterra, Upwork) | R2 |
| Gap Score (demand ÷ supply) | Superframeworks niche scoring | R2, R5 |
| Blue Ocean Strategy | W. Chan Kim & Renée Mauborgne | R4, R5 |
| Vertical SaaS thesis | SaaS & Systems Journal 2026 | R4 |
| BUILD/KILL/PIVOT adversarial | Open-source GitHub methodology | R3 |
| TAM/SAM/SOM | Y Combinator, standard MBA framework | R6 |
| 1000×$50 formula | IndieHacker community | R6 |
| Mom Test | Rob Fitzpatrick, *The Mom Test* (2013) | R8 |
| Friction scoring | Product-market fit evaluation (frequency × intensity) | R8 |
| Google Trends for opportunity timing | Standard market research practice | R9 |

---

## Change Log

| Date | Change |
|------|--------|
| 2026-08-26 | V4: Rewrite as true standard (7 weaknesses → 9 rules → quality criteria). Moved workflow to archive |
| 2026-08-26 | V3: Added Phase 0 interview, trend weight, moat, founder fit, MVP estimate, expansion, iteration |
| 2026-08-26 | V2: Rewrite with 7 real methodologies, quantitative thresholds, blue ocean verification |
| 2026-08-26 | V1: Initial 5-phase brainstorming protocol |
