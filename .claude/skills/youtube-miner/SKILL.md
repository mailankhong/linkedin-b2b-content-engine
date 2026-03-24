---
name: youtube-miner
description: Two scenarios. Scenario A (default): Gemini analyzes a specific YouTube video to extract frameworks, insights, and audience pain points. Scenario B (trending-first): Apify finds what is actually trending in the client's niche right now, Claude selects the most relevant video and runs extraction. Triggers include "mine YouTube for ideas", "analyze this YouTube video for [client]", "find trending videos for [client]", "what is blowing up in [niche]", or "YouTube mining for [client]".
---

# Skill: YouTube Miner

PRE-FLIGHT REQUIRED
Run preflight.md before proceeding.
Client: [name provided by user]
Methods requiring client context: ICP and content pillars (for mapping extracted insights to relevant angles).

**Tool dependency:** Scenario A requires the Gemini MCP (`mcp__gemini__gemini-youtube` and `mcp__gemini__gemini-youtube-summary`).
**Environment variable:** `APIFY_API_KEY` required for Scenario B.

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

## SCENARIO ROUTING

```
If user says "find trending", "what is blowing up", or "trending-first":
→ SCENARIO B — Apify finds trending video → extraction

Default (no specific trigger, or user provides a URL):
→ SCENARIO A — Gemini analyzes a specific video
```

---

## SCENARIO A — GEMINI VIDEO ANALYSIS (default)

Turn a specific YouTube video into 3–5 LinkedIn post ideas grounded in real frameworks and audience insight.

### Step 1: Find or receive a video

Either the user shares a URL, or find one.

**IMPORTANT — YouTube access rule:** Never use WebFetch on YouTube URLs directly. YouTube serves a consent redirect page in headless mode that blocks content. Use WebSearch with `site:youtube.com` to find video URLs via Google's index — this bypasses the consent wall. Once you have a URL, pass it directly to Gemini MCP tools which handle YouTube natively without browser auth.

**How to find relevant videos in headless mode:**
- Use WebSearch: `site:youtube.com [niche keyword] [problem or topic] after:[date 30 days ago YYYY-MM-DD]`
- Examples: `site:youtube.com "cold email" B2B 2026`, `site:youtube.com linkedin content strategy ghostwriter`
- From search results: extract the YouTube URL from the result link (format: `youtube.com/watch?v=...`)
- Look for: high view count signals in the snippet, educational titles, posted in the last 4 weeks
- Strong candidates: "how to", "why X doesn't work", "X things about Y", frameworks, case study breakdowns
- Avoid: generic motivational content, brand videos with no substance

### Step 2: Get a structured summary

Use `gemini-youtube-summary`:
- Style: `detailed` for unknown videos; `bullet-points` for confirmed relevant content

What to look for: frameworks the presenter teaches, problems named explicitly, counter-intuitive claims, before/after structures.

### Step 3: Extract pain points and frameworks

Use `gemini-youtube` with 2–3 targeted questions:

```
What problems or frustrations does this video name explicitly?
List them as the audience would phrase them — not the presenter's solution, just the pain.
```

```
What frameworks, systems, or step-by-step processes does this video teach?
Name each one and describe it in 2–3 sentences.
```

```
What counter-intuitive or surprising claims does the presenter make?
List each claim in one sentence — the kind of statement that would stop someone scrolling.
```

```
What does this video assume the viewer already knows or believes?
What would a skeptic push back on?
```

### Step 4: Map to client angle

For each extracted insight: "What does [client] actually know about this from their own experience, clients, or results?"

The goal is not to repackage the video. Use it as a signal for what the ICP cares about, then build angles grounded in the client's real expertise.

**Angle filter:**
- Does the client have a real opinion — not just agreement or a summary?
- Is there a specific story, result, or data point from their work that connects?
- Can the client go further, deeper, or more specific than the video?
- **Commoditization check:** Could someone find this by Googling and reading an agency blog? If yes, find the client's unique angle.

### Step 5: Generate ideas

For each angle that passes the filter:
- Hook (2 lines)
- Post angle (1–2 sentences grounded in the client's experience)
- Format suggestion (text-only / list / short story / visual)
- Why this resonates with the ICP (1 sentence)

Rules: no fabricated data — flag as [INSERT REAL STAT]. No generic takes — every idea must connect to something the client actually knows.

---

## SCENARIO B — APIFY TRENDING-FIRST MODE

Find what is actually trending in the client's niche right now, then extract from the most relevant result.

### Step 1: Find trending videos

Use WebSearch to find what is trending right now — this is more reliable than Apify for YouTube in headless mode:

```
WebSearch: site:youtube.com "[keyword string from pre-flight]" after:[date 7 days ago YYYY-MM-DD]
WebSearch: site:youtube.com "[niche keyword]" views after:[date 14 days ago YYYY-MM-DD]
```

Run 2–3 search queries with different keyword angles. From results, collect 5–10 YouTube URLs with their titles and any view count signals visible in the Google snippet.

**Filtering for trending signal:**
- Prefer videos with view counts mentioned in snippet (e.g., "1.2M views")
- Prefer recently uploaded (title or snippet mentions date/week)
- Prefer educational titles over brand/promo content

If WebSearch returns fewer than 3 relevant YouTube videos:
```
WebSearch fallback limited — proceeding to Scenario A with best available URL or manual search.
```

### Step 2: Select the most relevant video

From returned results:
- Filter for: educational content (not just podcasts or vlogs), published in the last 7 days, highest view count among relevant results
- Select the 1 video most relevant to the client's ICP and content pillars
- Note the view velocity signal: trending = high views in short time

### Step 3: Proceed with Scenario A extraction logic

Take the selected video URL and run it through Scenario A: Steps 2 → 5.
Report at top of output: "Scenario B — trending video selected: [title, URL, view count]"

---

## Output

**When run standalone:** Present 3–5 post ideas with hooks, angles, format suggestions, and ICP relevance notes directly.

**When called by weekly-idea-session**, return this result block:

```
METHOD RESULT: YouTube Miner
Status: [COMPLETE / PARTIAL / FAILED]
Scenario: [A — specific video / B — trending-first]
Angles found: [N]
Ideas:
- [Angle 1: framework or insight mapped to client — 1 sentence]
- [Angle 2]
- [Angle 3]
Flags: [any fabrication-placeholder flags, Gemini unavailability, Apify fallback, or "None"]
```

---

## Session Closing

YouTube mining complete — [N] angles extracted.
Scenario used: [A — Gemini / B — Trending via Apify → Gemini]
Source: [video title / URL]
Gaps: [any limitations — thin transcript, missing client angle, [INSERT REAL STAT] placeholders, etc.]

Next step → trigger: run hook-generator for [client] with [angle title]

---

## Verification

**Correctness:**
```
□ Ideas are grounded in the client's actual experience — not just a repackage of the video's content? Yes → pass. No → find the specific experience, result, or data point from the client that connects.
□ Every angle passed the commoditization check: a direct competitor cannot find this by Googling the topic? Yes → pass. No → find the client's unique angle before surfacing the idea.
□ Any missing data flagged as [INSERT REAL STAT] — no invented numbers? Yes → pass. No → flag or remove.
```

**Completeness:**
```
□ For each idea: hook (2 lines), post angle (1–2 sentences), format suggestion, and ICP relevance note? Yes → pass. No → add missing elements.
□ Scenario documented (A — specific video / B — trending-first)? Yes → pass. No → add it.
□ If called by weekly-idea-session: METHOD RESULT block returned with Status, Angles found, Ideas, and Flags? Yes → pass. No → format as METHOD RESULT.
```

**Context-fit:**
```
□ Ideas mapped to the client's content pillars — not just the video's topics? Yes → pass. No → map each idea to a specific pillar.
□ Client has a genuine opinion on each angle — not just agreement with the video presenter? Yes → pass. No → push for the client's specific POV or discard the angle.
□ Scenario B: video selected is the most relevant to the client's ICP — not just the highest view count? Yes → pass. No → re-select.
```

**Consequence:**
If these ideas are developed and published without adding the client's specific experience: what is the most likely outcome?
→ Answer before delivering. If the answer is "post reads like a video summary — not a perspective piece" — ensure each idea names the specific client experience that grounds it.
