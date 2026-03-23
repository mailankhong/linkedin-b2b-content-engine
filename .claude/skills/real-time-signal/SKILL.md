---
name: real-time-signal
description: Find current LinkedIn and X/Twitter debates in a client's niche and surface 2-3 POV post angles grounded in live industry conversation. Uses Apify for both X/Twitter and LinkedIn post search. Part of the weekly research pipeline. Triggers include "run real-time signal for [client]", "what's being debated right now in [niche]", or when called by weekly-idea-session.
---

# Skill: Real-Time Signal

PRE-FLIGHT REQUIRED
Run preflight.md before proceeding.
Client: [name provided by user]
Methods requiring client context: keyword string from Content Pillars (required — hard stop without it).

**Environment variables required:**
- `APIFY_API_KEY` — all Apify calls
- `X_COOKIES` — X/Twitter search may require authenticated cookies for full results. If not set, Apify will attempt unauthenticated search; results may be limited.

---

You can enter the pipeline here directly. Bring what you have. If prerequisite context is missing, I will ask for only what I need — I will not force you to start over.

---

## Quality Gate Rule

If input is insufficient to produce quality output:
1. Stop immediately
2. State exactly what is missing
3. State what the limitation means for output quality
4. Suggest the specific input needed to fix it
5. Ask: Shall I proceed with reduced quality and flag the gaps, or wait for better input?

Never invent, fabricate, assume, or produce work silently from insufficient input.
Always surface the gap, name the impact, suggest the fix.

---

**Purpose:** Surface 2–3 live debate signals in the client's niche across X/Twitter and LinkedIn. For each signal: summarize the debate, name the two sides, propose a clear POV the client could take — grounded in their content pillars.

---

## Source A — X/Twitter Signal via WebSearch

Use WebSearch with the following query format to find live X/Twitter debates:
```
site:x.com OR site:twitter.com "[keyword string from pre-flight]" after:[date 7 days ago YYYY-MM-DD]
```

Also run a supplemental search:
```
"[keyword string from pre-flight]" debate OR "hot take" OR "unpopular opinion" after:[date 7 days ago YYYY-MM-DD]
```

Filter results to posts with high engagement signals in the snippet (replies, retweets mentioned).
Keep only results from the past 7 days — discard anything older.

If WebSearch returns no recent X/Twitter results:
```
X/Twitter WebSearch returned no results for real-time-signal Source A. X/Twitter signals unavailable.
Proceeding with LinkedIn signals only.
```

---

## Source B — Apify LinkedIn Post Search Scraper

```
POST /acts/datadoping~linkedin-posts-search-scraper/run-sync-get-dataset-items?token={APIFY_API_KEY}
{
  "keywords": ["[keyword string from pre-flight]"],
  "maxItems": 20
}
```

Filter returned posts by `comments` descending. Keep top 5 posts with >10 comments.
High comment count on LinkedIn = active debate signal.

If Apify LinkedIn call fails:
```
Apify LinkedIn call failed for real-time-signal Source B. LinkedIn signals unavailable.
```

If both sources fail:
```
Real-time-signal could not run — both Apify sources failed.
```

---

## Processing

Merge results from Sources A and B. Deduplicate signals that are clearly the same debate appearing on both platforms.

For each signal where real conversation is happening, produce:
- **Debate summary:** What is this discussion actually about? (1–2 sentences)
- **Two sides:** Camp A argues [X]. Camp B argues [Y].
- **POV angle:** A clear position the client could take — must connect to one of their content pillars. Not a neutral summary — a stance.
- **Platform signal:** X/Twitter / LinkedIn / Both

Surface 2–3 signals. Quality over volume. A signal with no clear POV the client can credibly take is not worth surfacing.

---

## Output

**When run standalone:** Present signals and POV angles directly. Each signal is a candidate post idea — ready to develop through hook-generator → copy-developer.

**When called by weekly-idea-session**, return this result block:

```
METHOD RESULT: Real-Time Signal
Status: [COMPLETE / PARTIAL — one source failed / FAILED — both sources failed]
Angles found: [N]
Ideas:
- [Signal 1: debate topic + proposed POV in 1 sentence] (Source: [X/LinkedIn/Both])
- [Signal 2: debate topic + proposed POV in 1 sentence] (Source: [X/LinkedIn/Both])
- [Signal 3: debate topic + proposed POV in 1 sentence — if found]
Flags: [failed sources with reason, or "None"]
```

---

## Session Closing

Real-time signal complete — [N] debate angles found.
Sources used: [X/Twitter via Apify / LinkedIn via Apify / both]
Sources that failed: [list with reason, or "None"]
Gaps: [any reduced quality from missing X_COOKIES or partial Apify results]

Next step → trigger: run hook-generator for [client] with [signal title]

---

## Verification

**Correctness:**
```
□ Each POV angle is a clear stance the client can credibly take — not a neutral summary of the debate? Yes → pass. No → push harder on the client's position or discard the signal.
□ Each signal connects to one of the client's documented content pillars — not just "relevant to the niche"? Yes → pass. No → map to a specific pillar or remove the signal.
□ Debate sides are named as two distinct camps (Camp A argues X / Camp B argues Y) — not combined into one vague summary? Yes → pass. No → separate the positions before surfacing.
```

**Completeness:**
```
□ 2–3 signals surfaced — not just 1 (too thin) or 5+ (signal-to-noise problem)? Yes → pass. No → filter or expand.
□ For each signal: debate summary, two sides, POV angle, and platform source all present? Yes → pass. No → add missing fields.
□ If called by weekly-idea-session: METHOD RESULT block returned with Status, Angles found, Ideas, and Flags? Yes → pass. No → format as METHOD RESULT.
```

**Context-fit:**
```
□ Signals are from the past 7 days — not recycled debates from months ago? Yes → pass. No → re-run or flag the age of the signal.
□ POV angle is one the client can speak to from real experience or a documented pillar — not a topic outside their expertise? Yes → pass. No → discard the angle.
□ Apify failure flags surfaced explicitly — not silently omitted from output? Yes → pass. No → add the failure note before delivering.
```

**Consequence:**
If the client publishes a POV post from one of these signals: what is the most likely reason it falls flat?
→ Answer before delivering. If the answer is "the POV is too neutral — reads like a summary, not a stance" — push the angle harder before handoff.
