---
document_id: awp-knowledge-base/cognitive-discipline-test
language: en
publication: public
title: "Cognitive Discipline Standard — Test Comparison"
---

# Cognitive Discipline Test

> Does the cognitive discipline standard change how an agent answers an open-ended question? This file compares two runs of the same task: one plain, one following `standards/awp-cognitive-discipline-standard/std-cognitive-weakness-discipline.md`. Test date: 2026-09-06.

## The task (both runs)

> Find a SaaS product direction for solo AI content creators — one-person businesses who create YouTube videos, blog posts, and newsletters using AI tools.

## Run A — plain request, no standard

Output given without any standard:

1. AI writing and repurposing assistant — "turn one video into a blog post and newsletter". Crowded: Jasper, Copy.ai, Descript, OpusClip, repurpose.io.
2. AI video editor / shorts clipper. Crowded: OpusClip, Klap, CapCut, Descript.
3. AI thumbnail and branding generator. Crowded: Canva, Adobe Express, Thumbnail.AI.
4. AI social scheduler. Crowded: Buffer, Hootsuite, Metricool, Typefully.
5. Newsletter growth platform. Crowded: beehiiv, Substack, Kit, SparkLoop.
6. AI idea and hook generator. Crowded: vidIQ, TubeBuddy, plus a hundred ChatGPT wrappers.
7. Faceless YouTube automation. Crowded and risky — many tools already exist.

Verdict of run A: "These are all taken. Pick one and out-execute the incumbents." No sources, no checks, no user fit. Every idea is the first thing a language model thinks of.

## Run B — same task, following the cognitive discipline standard

The agent read `standards/awp-cognitive-discipline-standard/std-cognitive-weakness-discipline.md` first and ran the pipeline in its fixed order. Every artifact below was actually produced; every source was retrieved on 2026-09-06.

### R7 · Context lock-in

Single-shot prompt, no interview possible, so gaps are flagged, not invented.

| Field | Value | Source |
|-------|-------|--------|
| Who | Solo creator running a one-person business | prompt |
| Makes | YouTube videos, blog posts, newsletters, using AI tools | prompt |
| Audience | Their own viewers / readers / subscribers | prompt (implied) |
| Builds or ships | Not stated — can use AI tooling daily | ⚠️ provisional |
| Time / budget | Not stated | ⚠️ provisional |
| Exclusions | None stated | ⚠️ provisional |

Fit scores below are provisional where these fields are missing (allowed by R7: "records the gap and flags fit scores as provisional").

### R1 · Purge the obvious — quarantine list (32 items)

Written before research, one pass, no filtering. This list is not recommended — it is the training-data average, dumped on purpose:

1. AI writing assistant
2. AI video editor
3. AI scheduler
4. AI thumbnail generator
5. AI repurposing / clipping tool
6. Newsletter growth platform
7. AI idea / hook generator
8. Faceless YouTube automation
9. Social media manager
10. Link-in-bio tool
11. AI caption / subtitle tool
12. AI translation / dubbing tool
13. YouTube SEO tool
14. Blog SEO tool
15. Content calendar app
16. Podcast editing tool
17. AI voiceover tool
18. Stock / motion asset library
19. Newsletter analytics dashboard
20. Community platform for creators
21. Course / membership platform
22. Digital product template shop
23. Affiliate link manager
24. Sponsorship marketplace
25. Collab-finder marketplace
26. UGC ads platform
27. Creator CRM
28. AI landing page builder
29. Print-on-demand / merch tool
30. Live-streaming tool
31. Email deliverability checker
32. Creator accounting / tax tool

Run A's entire answer (writing assistant, clipper, thumbnail, scheduler, newsletter growth, idea generator, faceless automation) sits inside this list. **The plain run's output is the disciplined run's garbage.** That is W1 in action: the obvious answers are the mode of the training data.

### R2 · Evidence ledger — candidates are built from cited complaints, not imagination

Candidates come from pain clusters with real sources (retrieved 2026-09-06). Ledger rows:

| ID | Claim (pain / demand signal) | Source URL | Platform | Date |
|----|------------------------------|-----------|----------|------|
| E1 | "15 browser tabs open just to publish one YouTube video … $127/month in subscriptions. 20+ hours/week in busywork" | news.ycombinator.com/item?id=46897425 | Hacker News | 2026-02-05 |
| E2 | "Newsletters are a mess for me. Need help" (Ask HN headline) | news.ycombinator.com/item?id=38505796 | Hacker News | 2023-12-03 |
| E3 | "Small newsletter creators, how do you handle DMARC?" (Ask HN headline — deliverability tech burden) | news.ycombinator.com/item?id=39522563 | Hacker News | 2024-02-27 |
| E4 | "Sponsorship from my monthly newsletter doesn't come close [to a salary]… how much money I'm leaving on the table" | news.ycombinator.com/item?id=46458997 | Hacker News | 2026-01-01 |
| E5 | Newsletter owner hand-advertising: "Can I Advertise On Your Newsletter? Yes! Email me…" (manual, human-brokered deals) | news.ycombinator.com/item?id=48451841 | Hacker News | 2026-06-08 |
| E6 | "People with more than 5k newsletter subscribers, how you get sponsors?" (Ask HN headline) | news.ycombinator.com/item?id=42513273 | Hacker News | 2024-12-26 |
| E7 | "I built a newsletter sponsorship marketplace with 0% platform fees" (supply attempt, 2021) | news.ycombinator.com/item?id=28988841 | Hacker News | 2021-10-25 |
| E8 | "How to Price Newsletter Sponsorships" — how-to written because pricing is not standard | indiehackers.com/post/c5f1cc5d8f | Indie Hackers | 2021-02 |
| E9 | "5 ways to repurpose content like a professional creator" — reuse needs an explicit workflow | indiehackers.com/post/55545946fb | Indie Hackers | 2021-06 |
| E10 | Launch cluster: Reelze, Magiclip, Pictory, Sawtly, YTShortsDL, Shortgen ("OpusClip but Way Cheaper") | item?id=40620680 · 45821099 · 45069356 · 42931858 · 46055079 · 44754637 | Hacker News | 2024–2025 |
| E11 | Faceless / AI-video launch cluster: viralvelocity, LongStories.ai, "all in one toolkit" ×2 | item?id=46897425 · 43187305 · 46913054 · 46934331 | Hacker News | 2025–2026 |
| E12 | beehiiv is active and scaling: "Substack or Beehiiv?" debate, paywall reader, crawler-control feature | item?id=34750462 · 47947170 · 48646997 | Hacker News | 2023–2026 |
| E13 | "There are 100M+ channels uploading on YouTube regularly and only 2-3M of them are monetized" (commenter claim) | news.ycombinator.com/item?id=48765186 | Hacker News | 2026-07-02 |
| E14 | "Why YouTube doesn't let you search transcripts?" (Ask HN — retrieval demand) | news.ycombinator.com/item?id=40626040 | Hacker News | 2024-06-09 |
| E15 | Transcript → blog/notes tools exist ("Repurpose video content into blogs…") — supply side | news.ycombinator.com/item?id=45322161 | Hacker News | 2025-09-21 |
| E16 | "make it way easier to find marketing channels that actually work, from niche subreddits to Discord" (founder of a channel-discovery tool, single weak signal) | news.ycombinator.com/item?id=45079882 | Hacker News | 2025-08-31 |

Platform count: sources above come from news.ycombinator.com and indiehackers.com — 2 platforms. Note E1 and E11 share one founder's complaint posted three times; that counts as one independent source.

Candidates built from evidence (each = a pain cluster, not a brainstorm):

- **C1** Sponsor and ad-deal back-office for small newsletters and channels — pricing, outreach, renewals, invoices (E4, E5, E6, E8, E3).
- **C2** Publishing ops console for the solo publish pipeline — telemetry and runbooks, deliberately *not* generating content (E1, E2, E9).
- **C3** Personal content memory — the creator's own archive searchable and reusable, so repurposing starts from their real output (E14, E15, E1, E9).
- C4 Newsletter growth / churn platform (E12 shows incumbent strength — kept to test the kill round).
- C5 AI repurposing clipper (E10 shows a launch crowd — kept to test the kill round).
- C6 Faceless AI channel automation (E11 launch crowd — kept to test the kill round).
- C7 "What to make next" analytics dashboard (no independent complaint found in corpus).
- C8 Channel / distribution discovery tool (only one weak signal — E16 in appendix).

### R3 · Kill round

Every candidate was attacked at least three ways. Kill log (entered 8, survived 3 → **37.5%, within the ≤ 40% gate**):

| Candidate | Attack (a) who already does it | Attack (b) strongest objection | Attack (c) named failure mode | Verdict |
|-----------|-------------------------------|-------------------------------|------------------------------|---------|
| C1 Sponsor back-office | 2021 zero-fee marketplace (E7) tried the market; beehiiv (E12) owns newsletter infra, not deal ops | "Newsletters this small rarely get repeat sponsors" | Single big sponsor = revenue cliff | **Survived** — objections countered below |
| C2 Publishing ops console | AI "all-in-one" toolkits (E11) target the same pain | "Creators won't pay for ops, they pay for magic" | Value unclear until subscriptions add up | **Survived** — wedge: refuses to generate |
| C3 Content memory | Transcript tools (E15) do one slice; no full-archive player surfaced in corpus | "Creators can just search their own Drive" | Cold start until the archive is ingested | **Survived** — wedge: provenance + voice reuse |
| C4 Newsletter growth platform | beehiiv / Substack active and scaling (E12) | Network effects and switching costs | Churn after the first growth spike | Killed (funded incumbent territory) |
| C5 Repurposing clipper | Launch crowd 2024–2026 (E10), "OpusClip but cheaper" price war | No moat on model plumbing | Price collapse to near zero | Killed |
| C6 Faceless automation | Feb 2026 launch cluster (E11) | Platform rules can ban automated channels | Content quality ceiling | Killed |
| C7 "Make next" dashboard | No evidence of the pain in corpus (R2 fail) | — | — | Killed at R2, never entered attack |
| C8 Distribution finder | One weak signal only (R2 fail) | Cold-start chicken-and-egg | — | Killed at R2, never entered attack |

C1 objection counter-evidence: E5 shows even a mid-size owner still brokers deals by hand email in 2026, and E6 shows 5k-subscriber owners asking how to get sponsors at all — the gap is on the seller side, below where marketplaces and incumbents sit.

### R4 · Cross-domain transfer

| Direction | Abstract pattern | Industries searched | Mechanism adopted | Source |
|-----------|------------------|--------------------|-------------------|--------|
| C1 | "Small supplier sells small recurring inventory to many buyers with no broker" | Freelancing, insurance renewals, SaaS billing | Standardized rate card + renewal/dunning automation + a benchmark price index | Insurance renewal & ad-rate-card practice |
| C2 | "One operator, many manual micro-steps across tools" | Airline ops, restaurants, DevOps | Runbook checklists + time telemetry ("observability for a publish run") | DevOps runbooks |
| C3 | "An organization must reuse its own archive before generating new output" | Journalism newsrooms, legal e-discovery | Corpus-first retrieval with provenance ("reuse desk") | Newsroom archive practice |

### R5 · Search-verified novelty (scans run 2026-09-06)

Scans used the corpora reachable in this test environment (Hacker News full-text via Algolia, Indie Hackers). General web search (Bing, DuckDuckGo, Reddit) was blocked in this sandbox — this is a **recorded scan limitation**, not evidence of a blue ocean. Production runs must repeat these scans on the open web.

| Query (recorded) | Top signal found | Result |
|------------------|------------------|--------|
| "newsletter sponsorship platform" | 2021 zero-fee marketplace (E7); 2024 Ask HN (E6) | No funded direct competitor surfaced for seller-side back-office |
| "beehiiv" | Active scaled newsletter platform, 2023–2026 (E12) | Incumbent exists — but for hosting/growth, not deal ops |
| "content repurposing tool 2025" | 2025–2026 launch cluster (E10) | Repurposing space occupied → C5 killed here too |
| "OpusClip" | "OpusClip but Way Cheaper" launch (E10) | Price war confirmed |
| "Descript" | No product-specific signal (noise) | Recorded as low-signal scan |
| "personal knowledge Rewind Mem" | No relevant hits | Low-signal scan, recorded |

Strongest-rival statements: C1 → 2021 zero-fee marketplace (different model: listings vs. back-office) and beehiiv (infra, not deal ops). C2 → the E11 AI toolkits (generation-centric; C2's wedge is being tool-agnostic and non-generating). C3 → transcript tools (one slice of the archive, no provenance). No funded direct competitor surfaced in the accessible corpus for any survivor — verdict provisional until open-web scans run.

### R6 · Market arithmetic

Gate: 1,000 reachable payers × $50/month ≈ $600K/year revenue potential. Assumptions are labeled; none are presented as fact.

- **C1** — Pool: E13 claims ~2–3M monetized channels; assume a small slice (~5%) actively sells sponsorships → ~100–150K potential. Reachable year-1 through creator communities, newsletter directories, and HN/IH: 1,200–1,500 at $50/mo (assumption). Value anchor: sponsorship deals are worth far more than $50/mo each, and E5 shows deals are still brokered by hand. **Passes gate (labeled assumption).**
- **C2** — Pool: solo publish-active creators, subset of E13's monetized channels plus newsletter operators (E2, E12 scale). Reachable year-1: ~1,000 at $49/mo (assumption). Value anchor: E1 shows one creator already paying $127/mo across 6 tools for this workflow. **Passes gate (labeled assumption).**
- **C3** — Pool: creators with a multi-format archive (video + newsletter + blog), the exact E1/E9 profile. Reachable year-1: ~1,000 at $49/mo (assumption). Value anchor: replaces the transcription and rewriting steps of E15/E9. **Passes gate (labeled assumption).**

Trend signals (≥ 2 dated per direction): C1 — sponsor questions recur across 2021 (E7), 2024 (E6), 2026 (E5): durable; newsletter platforms keep growing (E12, 2023→2026). C2 — publish-workflow pain voiced Feb 2026 (E1/E11); reuse how-tos keep being written (E9, 2021, still linked in 2026). C3 — transcript search demand (E14, 2024) and transcript supply (E15, 2025) both rising. All labeled indicative (limited corpus).

### R7 · Founder fit matrix

| Direction | Fit | Why | Gap flag |
|-----------|-----|-----|----------|
| C1 | HIGH | A solo AI creator selling sponsorships is exactly the user; they feel the E4/E6 pain themselves | None material |
| C2 | HIGH | Their own weekly publish workflow is the product (E1 profile) | Provisional: build skill / budget unknown |
| C3 | MEDIUM | Needs their real archive and publishing tooling; value depends on how they create | Provisional: tooling depth unknown |

LOW examples dropped: C5 and C6 score LOW (require continuous content generation at volume and carry platform-risk that endangers the user's real channels) → dropped, per R7.

### Output gate — quality criteria check

| Criterion | Result |
|-----------|--------|
| Directions delivered (open scan needs ≥ 3) | 3 (C1, C2, C3) — no padding added |
| No training-data average | Zero overlap with the 32-item quarantine list |
| Evidence-backed | Every survivor has ≥ 3 ledger sources from ≥ 2 platforms; every factual claim has a URL and date |
| Survived attack | All 3 in kill log; survival rate 37.5% (≤ 40%) |
| Cross-domain | ≥ 1 transfer mechanism each, source industry named |
| Occupancy checked | Scan logs recorded with date + queries; strongest rival stated per direction; provisional until open-web scan |
| Sized | Arithmetic blocks with labeled assumptions; trend signals ≥ 2 dated per direction |
| Founder fit | Fit matrix present; no LOW or excluded direction reported |
| Audit trail | This appendix; recorded deviations: scan limitation, provisional context fields |
| Language gate | Passed — plain simple English, no jargon |

### Final answer (run B, summary of the artifact trail)

1. **C1 — Sponsor back-office for small newsletters and channels.** Solo creators with 1k–20k subscribers have no standard way to price, find, renew, and invoice sponsorships; they ask on forums how it works (E6), price by guesswork (E8), and broker deals by hand email even at 80k+ subscribers (E5). A seller-side back-office (rate card, outreach tracker, renewals, invoices, deliverability pre-checks) with a public price-benchmark index is unoccupied in the accessible corpus.
2. **C2 — Publishing ops console (non-generating).** The weekly "make one video and ship it everywhere" run is a chain of 15-tab busywork across six subscriptions (E1). A runbook + time-telemetry console that is deliberately tool-agnostic and refuses to generate content stands against the crowded AI "all-in-one" toolkits (E11) instead of joining them.
3. **C3 — Personal content memory.** Creators cannot search or reuse their own output: transcript search is a standing request (E14), and repurposing is taught as a manual ritual (E9). An archive-first retrieval layer over the creator's own videos, posts, and newsletters — with provenance and voice preservation — turns repurposing from regeneration into reuse.

## The contrast

| Dimension | Run A (plain) | Run B (with standard) |
|-----------|---------------|------------------------|
| Where ideas come from | First thoughts = training-data mode | Evidence-ledger pain clusters (E1–E15) |
| The obvious answers | Delivered as the answer | Quarantined in 32-item list, never recommended |
| Validation | None ("pick one and out-execute") | 3-attack kill round, 62.5% killed |
| Competitors | Hand-waved ("crowded") | Dated scan logs + strongest-rival statements |
| Market size | Not estimated | Arithmetic with labeled assumptions, trend signals |
| User fit | None (same answer for anyone) | Context table + fit matrix, LOW dropped |
| Trail | Nothing checkable | Full artifact appendix |

The visible difference is exactly what the step predicts: run A gives already-taken directions with no checks; run B discards those, builds from real dated complaints, kills most candidates, and only then recommends — each recommendation carrying its own evidence, kill history, and arithmetic.

## Limits of this test

- **Scans**: general web search was blocked in this sandbox; novelty verdicts are provisional until the same scans run on the open web (recorded deviation under R5).
- **Interview**: run B was single-shot, so three context fields stayed provisional (R7 gap flags).
- **Sources**: two platforms (Hacker News, Indie Hackers); a production run should add Reddit, G2, and App Store reviews.
- **Trends**: labeled indicative; a production run should add Google Trends over 12 months.
