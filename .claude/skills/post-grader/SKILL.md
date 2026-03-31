---
name: post-grader
description: Score any LinkedIn post across 5 dimensions using a clean rubric. Pass threshold is 38/50. Below threshold triggers specific, actionable fixes per failing dimension. Optionally rewrites. Triggers include "grade this post", "review my post", "score this", "is this post good", "rewrite this post", or pasting a post for quality review.
---

# Post Grader

A post can feel good and still flop. Feeling is not a grading system. This skill scores posts across 5 dimensions using a clean rubric — hook strength, clarity, ICP relevance, voice match, and conversion intent. Pass threshold is 38/50. Below threshold triggers specific, actionable fixes per failing dimension — not "improve the hook" but "move line 7 to line 1 and cut the first paragraph."

**Pre-flight:** Run preflight.md. Client: [name provided by user].
**Required:** ICP (Dimension 3) and Voice Profile (Dimension 4).

## Quality Gate

Missing ICP or Voice Profile → stop. State gap, impact, fix option.

---

## Scoring Rubric

Score each dimension 1–10. Total out of 50. Pass threshold: 38/50.

---

### Dimension 1: Hook Strength (1–10)

The only test: would someone stop scrolling to read this?

**8–10:** First 2 lines are specific, unexpected, or create a genuine curiosity gap. Stands out in a feed of AI-generated content.

**5–7:** Competent but not compelling. Describes the topic rather than earning the click. A competitor could write the same opener.

**1–4:** Vague, generic, or could belong to any post on this topic. Starts with "I", a question, or a statement that reads like a blog headline.

---

### Dimension 2: Clarity and Specificity (1–10)

Test: does the post have real substance, or is it advice floating in space?

**8–10:** At least one concrete detail — a number, a name, a scene, a moment, a real result. Feels like it came from experience. Moves from one state to another (problem to insight, confusion to clarity).

**5–7:** Clear point but vague — the claim is real but unanchored. One specific example would fix it.

**1–4:** Pure generic advice. No concrete detail anywhere. The reader cannot picture a real situation this came from.

---

### Dimension 3: ICP Relevance (1–10)

Test: is this post speaking to one specific type of person?

**8–10:** A specific person reads this and thinks "this is about me." Pain points, examples, and language are specific to an audience segment. The right person would save or share this.

**5–7:** Audience identifiable but post speaks to a broad group. Language is slightly more generic than the ICP's actual vocabulary.

**1–4:** Could be about anyone, for anyone. No clear audience segment. The ICP would not recognize their specific situation.

**Connect posts:** Score 8–10 if a specific type of person reads this and feels seen — not because their problem is named, but because their experience is named. "That's exactly how I felt" reaction. Score lower if the story is too personal to generalize or too generic to land.

**Respond posts:** Score 8–10 if the right person thinks "this is exactly what I need to hear about this right now." Score lower if the post reacts to something too niche to matter to the ICP, or too broad to feel relevant to their role.

---

### Dimension 4: Voice Match (1–10)

Test: does this sound like this specific person wrote it?

**8–10:** Clear personality markers — humor, phrasing, opinions, preferences. Sentence length and structure match the Voice Profile. Zero AI vocabulary. A regular reader would recognize the author from the writing alone.

**5–7:** Voice present but diluted — the right person, polished down to a generic version. Minor AI vocabulary slipping through. Correct structure but missing personality markers.

**1–4:** Could have been written by anyone. Classic AI crutches: em dashes, "here's the thing", "the reality is", "let me be honest", "make no mistake". Vocabulary from the banned list present.

---

### Dimension 5: Conversion Intent (1–10)

Test: does this post move the reader closer to trusting, following, or buying?

**8–10:** Author comes across as credible and experienced — demonstrates expertise, doesn't just claim it. Someone would DM or follow after reading. Clear, earned CTA. Post could NOT be written by a direct competitor.

**5–7:** Builds some credibility but conversion path is unclear. CTA feels bolted on rather than earned. Passes the swap test — a competitor could post this with minor edits.

**1–4:** Pure engagement bait with no positioning value. No reason to think about this author specifically. CTA asks for something the post hasn't earned.

**Connect posts:** Measure trust-building, not pipeline signaling. Do not penalize for lacking a promotional CTA — that is correct behavior. Score 8–10 if the post builds genuine human trust: the reader learns something real about who this person is, and is more likely to follow or engage. Score lower if vulnerability feels performed or if the post ends with a hard sell.

**Respond posts:** Measure POV authority, not CTA strength. Score 8–10 if the post shows a faster, sharper read on the industry than average — reader thinks "I want more of this person's takes." Do not penalize for a soft or absent CTA. Score lower if the post just summarizes what happened with no distinct perspective.

---

## Scoring Calibration

- **10:** Exceptional. Would go viral in this niche.
- **8–9:** Very strong. Clear voice, specific, will perform.
- **6–7:** Good but has a fixable weakness. Worth revising.
- **4–5:** Multiple problems. Needs real work before publishing.
- **1–3:** Structural rethink needed.

A post with clear voice, real specificity, and genuine insight starts at 8. Reserve scores below 6 for posts with genuine problems.

**Performance benchmarks (calibration context):**
- 3–5% engagement rate: solid
- 5–8%: strong
- 8–10%: excellent — candidate to repurpose
- 10%+: viral for this niche — study the structure and repeat it
- Meaningful comments: adds perspective, shares experience, asks a follow-up. Emoji reactions and "great post!" do not count.
- Saves: strongest indicator for Teach posts.
- Shares/reposts: highest-value signal for Lead posts.
- Connection requests: best pipeline signal.

---

## Output Format

```
TOTAL SCORE: [N] / 50
VERDICT: [PASS (38+) / NEEDS REVISION (<38)]

SCORES:
Hook Strength:       [N]/10
Clarity/Specificity: [N]/10
ICP Relevance:       [N]/10
Voice Match:         [N]/10
Conversion Intent:   [N]/10
```

**If PASS:**
Strong post. Ready to publish.
[Note any dimension at 7 or below — optional improvement if user wants.]

**If NEEDS REVISION:**
This post scored [N]/50. Threshold is 38/50.

Dimensions below threshold:
- [Dimension]: [Score]/10 — [Specific, actionable fix. Not "improve the hook." Tell them what to change: "The hook describes the topic — replace with a specific number or story from the post body. Example from this draft: use '[specific line from the post]' as the opener instead."]

Return to copy-developer with these notes. Revise, then re-grade.

---

## Rewrite Mode

If user requests a rewrite after grading:

- Target 9/10 on every dimension
- Rewrite the hook completely if it scored below 8
- Add one concrete detail wherever the original was vague — a number, a scene, a name
- Make bold improvements, not light edits
- Keep the author's core message and voice

**Banned in rewrites:**
em dashes / "here's the thing" / "the reality is" / "let me be honest" / "make no mistake" / "let that sink in" / "read that again" / "full stop" / "game-changer" / "next-level" / "deep dive" / "that said" / "in today's landscape" / "in today's fast-paced world" / "it's important to note" / "as we navigate" / "leverage" (as a verb) / "utilize" / "synergies" / "circle back" / "in conclusion" / "groundbreaking" / "transformative" / "cutting-edge" / "robust" / "scalable" / "best practices" / "touch base" / overly perfect parallel structure (three items that begin and end identically — real writing varies)

---

## Verification

**Correctness:**
```
□ Each dimension score is supported by a specific observation from the post (not a general statement)? Yes → pass. No → add the specific observation.
□ The feedback for any failing dimension names an exact line or word to change, not a general direction? Yes → pass. No → rewrite the feedback to be specific.
□ Post-type adjustments applied to Connect/Respond posts in Dimensions 3 and 5? Yes → pass. No → apply them now.
```

**Completeness:**
```
□ All 5 dimensions scored? Yes → pass. No → score the missing ones.
□ Total score calculated correctly (sum of 5 dimensions)? Yes → pass. No → recalculate.
□ PASS or NEEDS REVISION verdict stated? Yes → pass. No → add it.
```

**Context-fit:**
```
□ Dimension 4 (Voice Match) scored using the client's actual Voice Profile, not generic "sounds natural" judgment? Yes → pass. No → read the Voice Profile and re-score.
□ Dimension 3 (ICP Relevance) scored against the client's documented ICP tier, not general audience intuition? Yes → pass. No → read the ICP and re-score.
□ Any score below 6 accompanied by a structural fix, not just a surface edit suggestion? Yes → pass. No → add the structural fix.
```

**Consequence:**
If this post goes live at the current score: what is the single most likely underperformance reason?
→ State it before delivering. If the answer is "the hook won't stop the scroll" or "it reads as AI" — the post is not ready. Fix it or flag it explicitly.

---

[If PASS:]
Post approved — [score]/50
Dimension scores: Hook [N] | Clarity [N] | ICP [N] | Voice [N] | Conversion [N]

Next steps:
A) Post needs a visual → trigger: run visual-brief for [client]
B) Text only, next idea → trigger: run hook-generator for [next idea title]
C) All posts done → trigger: end session for [client]

---

[If NEEDS REVISION:]
Post needs work — [score]/50. Pass threshold: 38/50.
Dimensions that failed: [dimension + score + specific actionable note]
Limitation encountered: [what was missing that caused the fail]
Suggested fix: [specific input that would improve this dimension]

Returning to copy-developer now with these notes.
[loops automatically — max 2 retries]
