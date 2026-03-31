---
name: reddit-miner
description: Turn a Reddit thread into 5 ready-to-develop LinkedIn post ideas using audience pain points. Use this skill when generating content ideas from Reddit — especially when no fresh call transcript is available. Triggers include "mine Reddit for ideas", "use Reddit for content", "Reddit mining for [client]", or "find content on Reddit". Full pipeline: Reddit JSON → pain extraction → hooks → LinkedIn post drafts.
---

# Reddit Content Miner

Reddit is the only platform where your ICP complains honestly. On LinkedIn they perform; on Reddit they vent. The best Reddit threads for content aren't the highest-upvoted — they're the ones where commenters describe their situation before asking for help. That's unfiltered ICP language you can't get from a sales call, because on sales calls people use their "professional" vocabulary. On Reddit they say what they actually mean.

The thread quality matters more than the post itself. A mediocre question with 200 passionate replies is gold. A brilliant question with 3 surface-level answers is worthless. High-upvote comments = validated pain that dozens of people felt strongly enough to click an arrow. Low-upvote comments = noise.

This skill turns a Reddit thread into 5 LinkedIn post ideas grounded in real audience pain — not paraphrased marketing language, but the actual words your ICP uses when nobody's watching.

**Pre-flight:** Run preflight.md. Client: [name provided by user].
**Required:** ICP pain points and content pillars (for mapping extracted angles).

---

## Quality Gate

Insufficient input → stop. State gap, impact, fix option. Never invent or fabricate.

---

## When to Use

- No fresh call transcript is available for a client's content batch
- You want ideas grounded in what the ICP actually says, not what they're supposed to say
- You want to validate a content angle before writing a full post
- You're building new content pillars and need to hear the audience's language

---

## What You Need

- A relevant Reddit post URL (from a subreddit the client's ICP hangs out in)
- Client context: niche, ICP, content pillars, 3–4 example posts that have performed well
- Access to the client's Claude Project (all reference docs are loaded there — use it for post development, not this initial skill)

---

## The Pipeline

### Step 1: Find a relevant Reddit post

Go to Reddit. Find a post that your ICP would write or engage with — a question, frustration, debate, or rant in their niche.

**How to find it — try in this order:**

**Recency note for Reddit:** Reddit threads are sources of ICP pain language — do NOT apply the 7-day recency rule here. Use a 15-day window.

**Attempt 1 — WebSearch with site restriction:**
```
site:reddit.com [niche keyword] after:[date 15 days ago YYYY-MM-DD]
site:reddit.com [subreddit name] [niche keyword]
```
Run 2–3 queries with different keyword angles. Extract any Reddit post URLs from the results.

**Attempt 2 — WebSearch without site restriction (if Attempt 1 returns nothing):**
```
reddit [niche keyword] [subreddit name]
"reddit.com/r/[subreddit]" [niche keyword]
```
Google indexes Reddit even when `site:` filter returns nothing. Often returns the same threads through different query paths.

**Attempt 3 — WebFetch JSON (works in interactive sessions, may be blocked in headless):**
If a Reddit URL was found in Attempts 1–2, append `/.json` and fetch:
```
WebFetch: https://www.reddit.com/r/[subreddit]/comments/[id]/[slug]/.json
```
Or fetch the subreddit top directly:
```
WebFetch: https://www.reddit.com/r/[subreddit]/top.json?limit=10&t=month
```
If WebFetch is blocked: proceed with snippet text from Attempts 1–2.

**If all three attempts return nothing:**
```
METHOD RESULT: Reddit Miner
Status: FAILED
Reason: No Reddit threads found via WebSearch and WebFetch blocked.
```
Return this block and stop. Do NOT substitute industry press, blog posts, news articles, or any non-Reddit source. FAILED is the correct and honest result.

- Look for: posts with active comment sections — engagement matters more than post date
- The thread quality matters more than the post itself — the best content is in the replies

**Good subreddits by client type:**
| Client niche | Where to look |
|---|---|
| B2B SaaS / GTM / sales | r/sales, r/salesforce, r/hubspot, r/startups, r/entrepreneur |
| AI tools / automation | r/artificial, r/MachineLearning, r/ChatGPT, r/nocode |
| Data & analytics | r/dataengineering, r/analytics, r/BusinessIntelligence |
| Founder / B2B growth | r/Entrepreneur, r/smallbusiness, r/marketing, r/growthhacking |
| LinkedIn content | r/linkedin, r/marketing, r/digitalmarketing |

---

### Step 2: Pull the full thread

**If a Reddit URL was found and WebFetch is available:**
Add `/.json` to the end of the URL and fetch it:
```
Before: https://www.reddit.com/r/sales/comments/abc123/cold_email_is_dead/
After:  https://www.reddit.com/r/sales/comments/abc123/cold_email_is_dead/.json
```
The JSON contains the full thread with upvote counts. Higher-upvoted comments = more validated pain.

**If WebFetch is blocked (headless/sandbox mode):**
Work from the search snippet text collected in Step 1. Snippets contain the post title and a fragment of the top comments — enough to identify the core pain and generate angles. Flag the output as PARTIAL — snippet-only, not full thread analysis. Proceed to Step 3 using the snippet text as input instead of full JSON.

---

### Step 3: Extract pain points and generate hooks

Analyze the thread (JSON or snippet) and extract content intelligence:

**Pain extraction rules:**
- Pull 5-8 distinct pain points from comments, prioritizing highest-upvoted
- Use the commenter's actual language — not marketing paraphrases. If they said "our CRM is a dumpster fire," that's the pain point, not "CRM adoption challenges"
- Distinguish between symptoms (what they're complaining about) and root causes (why it's actually happening). The best posts address the root cause while naming the symptom
- Discard generic complaints ("everything sucks") — keep specific situational frustrations ("we spent 3 months migrating and lost half our pipeline data")

**Hook generation from pain points:**
For each usable pain point, generate a 2-line LinkedIn hook:
- Line 1: Bold claim, counter-take, or striking observation drawn from the pain
- Line 2: Set up tension or promise that earns the "see more" click

Match hooks to the client's voice profile and documented hook style. Reference their best-performing posts for rhythm and energy — but the ideas, wording, and angles must be new.

---

### Step 4: Review pain points and select the best hooks

Before moving to post development:

- [ ] Do the extracted pain points reflect something real — or is it generic complaining?
- [ ] Does the pain map to this client's ICP specifically (not just anyone on Reddit)?
- [ ] Are the hooks grounded in one specific pain — or are they vague?
- [ ] Would this client have something genuine to say in response to this pain?
- [ ] Run the **commoditization check**: *"Could someone find this answer by Googling the topic and reading a top agency blog?"* If yes, the angle needs the client's unique take, not just a repackaged explanation.

Pick 1–3 hooks to develop into posts.

---

### Step 5: Develop into LinkedIn posts

Take selected hooks and develop into full post drafts using copy-developer principles:

**Development rules:**
- Start with the selected hook (or a refined version)
- Include 1 clear takeaway + 1 framework (steps, a list, or a before/after)
- Keep it skimmable — one idea per line, white space every 2-3 lines
- End with a CTA that earns the ask — natural, not bolted on
- Ground in the client's real experience — use [INSERT REAL STAT] for any data you don't have
- Do NOT recycle phrases from the client's existing posts — structural patterns yes, exact wording no
- Apply the client's Voice Profile from word one — not as a polish pass after drafting

**The Reddit-to-LinkedIn translation:**
Reddit pain is raw. LinkedIn content is structured. The skill is preserving the emotional truth of the Reddit thread while repackaging it in the client's voice and expertise. Don't sanitize the pain — channel it through the client's authority.

---

### Step 6: Review and hand off

Before taking drafts to the client:

- [ ] Does the post sound like the client — or like a clever person writing about their topic?
- [ ] Is the pain addressed specific enough, or too general?
- [ ] Is any data or stat real or clearly flagged as [INSERT REAL STAT]?
- [ ] Would the client's ICP read this and feel seen?
- [ ] Apply the standard fabrication check: no invented numbers, no made-up case studies

Drafts that pass → move to content-copy-developer for refinement, or send directly to client for review depending on confidence level.

---

## What to Do With the Output

| Output | Goes to |
|---|---|
| Pain point list | Save to client's content notes or idea batch doc |
| Selected hooks | Add to the client's content idea batch |
| Post drafts | Content copy developer → client review |
| Reddit post URL | Note the source in the content idea batch for reference |

---

## Related Skills
- `content-idea-batch` — full idea generation workflow (use after this for formatting and QC)
- `hook-generator` — if you need more hook variations beyond the 5 generated here
- `content-copy-developer` — take developed post drafts through refinement

---

Reddit mining complete — [N] angles found.
Next step → trigger: run hook-generator for [client] with [angle title]
Or bring angles to weekly-idea-session if part of a larger batch.

---

## Verification

**Correctness:**
```
□ Extracted pain points are in the audience's own language — not marketing paraphrases? Yes → pass. No → rewrite using the actual Reddit language.
□ Selected hooks passed the commoditization check: a direct competitor cannot find this angle by Googling the topic? Yes → pass. No → find the client's unique angle.
□ Any placeholder data flagged as [INSERT REAL STAT] — no fabricated numbers in the post drafts? Yes → pass. No → flag or remove the invented data.
```

**Completeness:**
```
□ Pain points extracted (Step 3) before hooks generated — not hooks written from scratch without pain extraction? Yes → pass. No → run the extraction step first.
□ Post drafts include: hook, takeaway, framework (steps/list/before-after), and CTA? Yes → pass. No → add missing elements.
□ Reddit thread URL noted as source reference? Yes → pass. No → add it.
```

**Context-fit:**
```
□ Pain points map to this client's ICP — not just anyone who uses Reddit? Yes → pass. No → filter the pain points against the client's ICP before developing.
□ Post drafts match the client's documented voice — not generic LinkedIn ghostwriter tone? Yes → pass. No → apply the Voice Profile.
□ High-upvote comments prioritized over low-upvote comments in pain extraction? Yes → pass. No → re-weight the extraction.
```

**Consequence:**
If these post ideas are developed and published: what is the most likely reason they underperform with the client's ICP?
→ Answer before delivering. If the answer is "pain is real but too broad — not ICP-specific" or "draft doesn't contain any of the client's actual experience" — fix it before handoff.
