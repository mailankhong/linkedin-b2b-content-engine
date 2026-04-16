---
name: weekly-idea-session
description: Self-serve weekly content planning session. Spawns 6 parallel research workers, merges results, runs quality gate and ICP tier check, adds personal material, outputs a sized and prioritized idea batch. Triggers include "start my weekly session", "generate ideas for this week", "weekly idea batch", "what should I post this week".
---

# Weekly Idea Session

The hardest part of LinkedIn consistency isn't writing — it's knowing what to write. Most creators stare at a blank page and default to whatever's on their mind, which produces random content that doesn't compound. This skill replaces randomness with a system: 6 parallel research workers mine different sources, results get merged and quality-gated, personal material gets layered in, and you end up with a sized, prioritized batch of ideas that are ICP-targeted and ready to develop.

**Pre-flight:** Run preflight.md. Client: [name provided by user].
**Required:** Content pillars must be present — hard stop without them.

## Quality Gate

Missing content pillars → stop. State gap, impact, fix option. Never invent or fabricate.

---

## PHASE 1 — INTAKE

Ask both questions. Do not proceed until both are answered.

```
Before we build this week's ideas, two quick questions:

1. How many posts do you want to publish this week?
   Be honest. 2 is fine. 3 puts you in the top 10% of LinkedIn creators.

2. Should I run research to find new ideas?
   I'll pull from 6 sources in parallel — audience conversations, long-form content,
   live industry debates, your post archive, high-performing post structures,
   and viral visual frameworks.
   Yes / No
```

---

## PHASE 2 — RESEARCH (if user said Yes)

State which workers are running. Spawn all 6 simultaneously.

```
Running 6 research methods in parallel — this takes a moment.
```

**Workers (run simultaneously):**

1. **reddit-miner** — ICP communities and forums → real pain language and post angles
2. **youtube-miner** — Long-form video in niche → frameworks and insight angles
3. **real-time-signal** — Grok/X + Apify → live debates and POV angles
4. **repurposing-archive** — Client post history via Apify → gaps and refresh angles
5. **reverse-engineering** — Viral text + visual posts via Apify → structural templates and angles
6. **viral-visual-miner** — Proactive visual search → visual framework ideas

Each worker receives: client name, keyword string from pre-flight, content pillars.
Each worker returns a standardized METHOD RESULT block.

**RECENCY RULE:**
- **real-time-signal, reverse-engineering, viral-visual-miner:** All WebSearch queries must include `after:[date 7 days ago YYYY-MM-DD]`. Discard LinkedIn posts and live debates older than 14 days. This is a B2B AI/tech niche — 2 weeks is already old news, 4 weeks is history.
- **All Apify LinkedIn searches:** filter to past week only.
- **reddit-miner:** Use a 15-day window. Reddit threads are ICP pain sources, not breaking news. Do not apply the 7-day rule here.
- **youtube-miner:** Use a 30-day window. YouTube videos are framework sources. Do not apply the 14-day rule here.
- **repurposing-archive:** No recency filter — pulls client's full post history by design.
- If a worker finds nothing after exhausting its window, flag FAILED. Do not substitute industry press, blogs, or news articles for any worker.

After all workers complete:
- Collect all METHOD RESULT blocks
- Note any workers that returned FAILED or PARTIAL status
- Deduplicate angles that are substantially the same idea from different sources
- Pool all passing angles for Phase 3

**Model routing note:** Phase 2 workers are data-gathering tasks — fetching from APIs, extracting pain points from threads, parsing video transcripts. These run fine on standard models. Phases 3–6 (personal material extraction, quality gate, coverage checks, prioritization) require judgment: evaluating ICP fit, spotting generic angles, balancing the batch. If using plan mode or model switching, the split point is here — research below this line, synthesis above.

---

## PHASE 3 — PERSONAL MATERIAL

Present a brief count, then ask:

```
Research complete — [N] angles found across [X] methods.
[Note any methods that could not run and why.]

Before I size the batch: do you have any personal material this week?
A client conversation, a pattern you noticed, a win, something frustrating,
an insight from your work — anything real.

If yes, paste it and I'll extract angles and add them to the pool.
If no, I'll build the batch from research.
```

If personal material is provided, extract:
1. Specific stories or moments → standalone post candidates
2. Concrete data points, numbers, or results → post anchors
3. Contrarian takes or strong opinions → challenge posts
4. Client pain points or objections → audience-resonant angles
5. Personal experiences or lessons → Connect post material

For each extracted angle: 1-sentence summary / ICP tier / category / specificity rating (HIGH = has real number or named story / MEDIUM / LOW).

Add to pool. Proceed to Phase 4.

---

## PHASE 4 — QUALITY GATE

Apply to every angle in the pool:

**Q1: Is there a real experience, specific number, or concrete story behind this?**
YES → pass. NO → flag "needs enrichment" or discard.

**Q2: Could only this person write this — or could anyone in their industry write it?**
YES (distinctive) → pass. NO (generic) → flag for refinement.

**Q3: Does this speak to a named pain point of Tier 1 or Tier 2 ICP?**
YES → pass. NO → move to Tier 3 bucket or discard.

**Decision:**
- 3/3 → include in batch
- 2/3 → include with note on weak dimension
- 1/3 → return to user: "This needs more before developing — here's why: [specific gap]"
- 0/3 → discard quietly

---

## PHASE 5 — COVERAGE CHECKS

### ICP Tier Coverage

| Tier | Target | Current count |
|------|--------|---------------|
| Tier 1 (Decision Makers) | At least 2 | [count] |
| Tier 2 (Champions / Evaluators) | At least 1 | [count] |
| Tier 3 (Community / Practitioners) | 0–1 flexible | [count] |

Tier 1 under 2 → flag:
```
We have [X] Tier 1 posts. For pipeline impact, we need at least 2.
I can pull one more from [research method] — or do you have a client story
or outcome you haven't mentioned yet?
```

Do not silently add ideas to fill the quota.

### Post-Type Diversity Check

Recommended weekly mix: 30–40% Teach, 20–25% Lead, 20–25% Prove, 15–20% Connect, 5–10% Respond.

| Category | Target % | Current count |
|----------|----------|---------------|
| Teach/Educational | 30–40% | [count] |
| Lead/Thought Leadership | 20–25% | [count] |
| Prove/Social Proof | 20–25% | [count] |
| Connect/Relationship | 15–20% | [count] |
| Respond/Amplify | 5–10% | [count] |

Batch represents fewer than 3 of 5 categories → flag and ask.
More than 50% of the batch falls in a single category → flag:

```
This batch is heavily weighted toward [category] posts ([N] of [total]).
A balanced week signals range to the algorithm and to your audience.
Want me to suggest a swap — or is this intentional for this week?
```

Do not force a rebalance. Flag and ask.

---

## PHASE 6 — LEAD MAGNET PROMPT

Ask once after confirming the idea batch:

```
One more option: would you like one post this week to be a lead magnet?
That means giving away a free resource — a template, checklist, playbook, or system —
in exchange for comments and connections.

Say yes only if you're ready to build and deliver the resource.
The post creates a public promise. You need to follow through.

Yes / No / Maybe next week
```

Yes → flag that idea for the lead magnet path → lead-magnet-writer + lead-magnet-builder instead of the standard copy path.

---

## PHASE 7 — OUTPUT

Present the batch sized to the user's weekly post count from Phase 1.

Format for each idea:

```
IDEA [N]: [Title / angle in one sentence]

ICP Tier: [Tier 1 / Tier 2 / Tier 3]
Category: [Teach / Lead / Prove / Connect / Respond]
Source: [Personal material / Reddit / YouTube / Real-time signal / Archive repurpose / Reverse-engineered / Visual anchor]
Specificity: [HIGH / MEDIUM — with note if enrichment needed]
Quality gate: [3/3 / 2/3 — with note on weak dimension if applicable]
Recommended post date: [Month DD] or earlier | Any day (evergreen) | [Month DD] (pre-event) or [Month DD] (same-day reaction)
```

**Why this field is required:** News-reactive ideas decay at different rates. A "5-7 day first-mover window from April 15" means nothing to a client scheduling across a full week — they'll pick it Monday and post it Thursday when the window closed Tuesday. The skill has the research context; the client does not. Translate every decay window into a concrete post-by date. For ideas with no decay, state "Any day (evergreen)" — consistency means the client can scan the field and know status at a glance.

**How to set the date:**
- News hook with a stated decay window: today + decay days = latest recommended post date. Subtract 1-2 days for margin.
- Calendar-locked event (earnings, conference, product launch): use the event date itself for pre-event posts, or the next day for same-day reactions.
- Stat that will be superseded by a scheduled refresh: use the refresh date minus 1 as the latest.
- No decay / pillar-grounded: "Any day (evergreen)".

End with summary:

```
This week's batch: [N] ideas
Tier coverage: [X] Tier 1 | [Y] Tier 2 | [Z] Tier 3
Lead magnet: [Yes — [idea title] / No]
Methods that could not run: [list with reason, or "None"]

Next step: Take your first idea to hook-generator, then copy-developer.
Or bring them all — develop one at a time.
```

---

## Verification

**Correctness:**
```
□ Every idea in the batch passed Phase 4 quality gate at 2/3 or higher? Yes → pass. No → remove or flag the failing ones.
□ Tier 1 ICP count is at least 2? Yes → pass. No → flag before delivering.
□ Batch size matches the post count the user stated in Phase 1? Yes → pass. No → resize.
```

**Completeness:**
```
□ Each idea has all 6 output fields: ICP Tier, Category, Source, Specificity, Quality gate, Recommended post date? Yes → pass. No → fill the missing fields.
□ Every idea's Recommended post date is a concrete date or "Any day (evergreen)" — never a vague decay window? Yes → pass. No → resolve the date before delivering.
□ Summary block includes tier coverage, lead magnet decision, and methods that failed? Yes → pass. No → add it.
□ Any workers that returned FAILED or PARTIAL have a note explaining why? Yes → pass. No → add the note.
```

**Context-fit:**
```
□ At least one idea comes from personal material (client conversation, real result, specific experience) — not purely from research? Yes → pass. No → ask for personal material again before delivering.
□ Every idea is mapped to a content pillar from the client's documented pillars? Yes → pass. No → map or discard the unmapped idea.
□ Category mix represents at least 3 of 5 categories? Yes → pass. No → flag the imbalance before delivering.
```

**Consequence:**
If the client develops all ideas in this batch and publishes them this week: what is the most likely gap?
→ Answer before delivering. If the answer is "no Prove posts, so no pipeline signal this week" or "all research angles, nothing personal — will feel generic" — flag it explicitly.

---

Idea batch ready — [N] ideas this week.
Methods that ran: [list]
Methods that failed: [list with reason and what was missing]
Methods with thin results: [list with specific gap and suggested fix]

Lead magnet decision — point 1:
Would you like any of these ideas as a lead magnet this week?
A post that gives away a free resource in exchange for comments.
Only say yes if you are ready to build the resource.

Yes → trigger: run lead magnet flow for [idea title]
No → trigger: run hook-generator for [idea title]
