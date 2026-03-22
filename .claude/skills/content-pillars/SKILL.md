---
name: content-pillars
description: Define 3-5 strategic content pillars and generate dozens of specific post ideas within each pillar. Use this skill when the user wants to figure out what to post about, needs content ideas, wants to define their content strategy, or is running out of topics. Triggers include "what should I post about", "give me post ideas", "define my content pillars", "I don't know what to write", or when building a content strategy from scratch. Generates 10-15 ideas per pillar with draft hooks.
---

# Content Pillar + Post Ideas Generator

Define 3-5 content pillars based on your expertise, offer, and ICP. Then generate dozens of specific post ideas within each pillar that you can rotate for months without repeating yourself.

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

Detect which scenario to run before doing anything else. Do not ask the user which scenario — read the context and route automatically.

```
If the user has provided a document at session start
(a strategy brief, brand guide, existing pillar list, positioning document, or any
structured document describing what they do, who they serve, and what they want to be known for):
→ SCENARIO B — Document Input

If the user has provided only a request to "define my pillars" or "give me post ideas"
with no document attached:
→ SCENARIO A — Interview
```

---

## SCENARIO B — DOCUMENT INPUT

The user has provided an existing strategy document, brand guide, or pillar list.
Skip the interview. Map the document against the pillar schema and generate directly.

### Step 1 — Read and map the document

Read the full document. Map its contents against the five pillar dimensions:

| Dimension | What to look for | Status |
|---|---|---|
| Offer / expertise | What they do, what they're known for | COVERED / PARTIAL / MISSING |
| Audience | Who they serve, ICP role or niche | COVERED / PARTIAL / MISSING |
| Authority angle | Why they're credible on their topics | COVERED / PARTIAL / MISSING |
| Conversion intent | What action content should drive | COVERED / PARTIAL / MISSING |
| Contrarian / POV | Beliefs they hold, what they disagree with | COVERED / PARTIAL / MISSING |

### Step 2 — Check for critical gaps

Critical dimensions (without these, pillars will be too generic to be useful):
- Offer / expertise — what they actually do and get paid for
- Audience — who they serve (even a rough ICP description is enough)

If either critical dimension is missing, ask for it specifically:

```
I can see [dimension] isn't covered in the document.
Without it, [specific consequence — e.g. "I can't tie the pillars to a clear buyer"].

Can you add: [single targeted question for only that dimension]

Everything else is covered — I'll proceed as soon as you send this.
```

Ask no more than one gap question at a time. Do not run the full interview.
Non-critical gaps: infer from document context, flag as *(inferred — confirm with client)*, proceed.

### Step 3 — Proceed to Phase 2 (Define Content Pillars)

Build pillars from the document contents using the same pillar framework as Scenario A.
Note at the top of the output: "Pillars built from provided document."
List any inferred dimensions clearly at the end of the output.

---

## SCENARIO A — INTERVIEW

### How It Works

1. **Input Phase** - Understand the user's business, expertise, and audience
2. **Pillar Definition** - Build 3-5 strategic content pillars
3. **Idea Generation** - Generate 10-15 post ideas per pillar with hooks

---

## Phase 1: Input

Ask the user:

1. What do you do? (service, offer, product)
2. Who do you serve? (or reference the ICP profile if available)
3. What are you most known for? (or want to be known for)
4. What topics could you talk about for hours without notes?
5. What mistakes do you see your audience making constantly?
6. What's a belief you hold that most people in your space disagree with?
7. Do you have client stories you can reference? (results, transformations, lessons)
8. What questions do prospects ask on sales calls?

---

## Phase 2: Define Content Pillars

Build 3-5 pillars using this framework:

### Pillar Structure

Each pillar needs:
- **Name**: 2-4 word label (e.g., "LinkedIn Strategy", "Client Stories", "AI for Content")
- **Why it works**: How it connects to the ICP's pain or desire
- **Authority angle**: Why you're credible on this topic
- **Conversion path**: How this pillar moves someone closer to buying
- **Content ratio**: What percentage of posts should come from this pillar

### Pillar Categories to Consider

**1. Expertise Pillar (30-40% of content)**
Your core knowledge area. The thing you get paid to do. Frameworks, how-tos, breakdowns, strategies.
- Positions you as the go-to expert
- Gives people a reason to follow you
- Example: "LinkedIn Content Strategy" for a ghostwriter

**2. Proof Pillar (20-25% of content)**
Case studies, client results, behind-the-scenes, real numbers. Shows you actually do the thing.
- Builds trust faster than any advice post
- Converts better than educational content
- Example: "Client Transformations" with real results and stories

**3. Personal Pillar (15-20% of content)**
Your journey, lessons, opinions, failures, personality. Makes people like you as a human.
- Creates emotional connection
- Makes you memorable and relatable
- Example: "Ghostwriting Lessons" from 5+ years in the trenches

**4. Industry/Trend Pillar (10-15% of content)**
Hot takes, trend analysis, predictions, tool recommendations. Shows you're paying attention.
- Keeps content fresh and timely
- Positions you as someone plugged in
- Example: "AI for Content Creation" covering tools, trends, what's working now

**5. Lead Magnet Pillar (10-15% of content)**
Posts specifically designed to capture leads. Free resources, tools, templates, systems.
- Directly generates connections and DMs
- Builds your email list or audience
- Example: "Free Systems and Resources" with comment-to-get CTAs

### Output Format for Pillars

```
CONTENT PILLAR 1: [Name]
Why: [1 sentence on why this connects to ICP]
Authority: [Why you're credible here]
Conversion: [How this moves them toward buying]
Ratio: [X% of total content]

CONTENT PILLAR 2: [Name]
...

CONTENT PILLAR 3: [Name]
...
```

---

## Phase 3: Generate Post Ideas

For each pillar, generate 10-15 specific post ideas. Each idea includes:

- **Topic**: One sentence description
- **Post type**: Story / framework / case study / listicle / contrarian / lead magnet / how-to
- **Goal**: Engagement / authority / conversion / lead gen

> **Hooks are not generated here.** When any of these ideas move to copy development, run the hook-generator skill at that stage — before writing the body. Hook-generator produces 10-15 variations with proper emotional trigger analysis; inline hook drafts here would be lower quality and get replaced anyway.

### Idea Generation Frameworks

Use these to generate diverse ideas within each pillar:

**The Mistake Framework**
"Most [audience] do [wrong thing]. Here's what to do instead."
Generate 5 common mistakes in their niche, each becomes a post.

**The Process Framework**
"Here's exactly how I [do thing], step by step."
Break their core process into individual post-worthy steps.

**The Story Framework**
"A client came to me with [problem]. Here's what happened."
Turn every client interaction into a potential story post.

**The Contrarian Framework**
"Everyone says [common advice]. I disagree. Here's why."
List beliefs in their niche they can challenge with evidence.

**The Comparison Framework**
"[Old way] vs [new way]. Here's the difference."
Compare outdated approaches with what actually works.

**The Lesson Framework**
"After [years/clients/projects], here's what I learned about [topic]."
Extract lessons from experience and turn each into a post.

**The Tool/Resource Framework**
"[Number] tools I use every day for [outcome]."
Share the tools, templates, or resources behind their work.

**The Timeline Framework**
"How I went from [starting point] to [result] in [time]."
Map out a transformation journey across multiple posts.

---

## Output Format

```
CONTENT PILLARS + POST IDEAS

PILLAR 1: [Name] ([X]% of content)
Why: [connection to ICP]

Post Ideas:
1. [Topic] | [Type] | Goal: [goal]
2. [Topic] | [Type] | Goal: [goal]
[...10-15 ideas per pillar]

PILLAR 2: [Name] ([X]% of content)
...

[Repeat for all pillars]

TOTAL IDEAS: [number]
ESTIMATED COVERAGE: [X weeks/months at Y posts per week]

→ When any idea moves to copy development: run hook-generator first (before writing the body).
```

---

## Quality Check

Before delivering:
- Does each pillar serve a different strategic purpose? (not overlapping)
- Are the post ideas specific enough to write from? (not vague like "post about marketing")
- Is there a healthy mix of post types? (not all listicles or all stories)
- Would the ICP actually care about these topics?
- Can the user look at this list and immediately know what to write this week?
- Do the hooks sound like something a real person would say, not an AI?

---

Content pillars defined — scenario used: [A/B]
Quality check: flag any pillar missing a Why sentence.
Note: 30–75 post ideas generated and available as starting pool.

Next step → trigger: run profile-optimizer for [client]

---

## Verification

**Correctness:**
```
□ Each pillar serves a different strategic purpose — no two pillars overlap in content type? Yes → pass. No → merge or differentiate.
□ Each pillar has a conversion path — how it moves a reader toward buying? Yes → pass. No → add it.
□ Post ideas are specific enough to write from immediately (not "post about marketing") ? Yes → pass. No → make them more specific.
```

**Completeness:**
```
□ 3–5 pillars defined with name, Why, Authority, Conversion, and Ratio? Yes → pass. No → complete the missing fields.
□ At least 10 post ideas per pillar? Yes → pass. No → generate more.
□ Scenario documented (A — interview / B — document)? Yes → pass. No → add it.
```

**Context-fit:**
```
□ Pillars map to the client's documented ICP — would the ICP care about every pillar? Yes → pass. No → replace the non-relevant pillar.
□ At least one Prove/Social Proof pillar present (required for pipeline)? Yes → pass. No → flag the gap.
□ Post idea mix includes multiple post types (not all listicles, not all stories)? Yes → pass. No → diversify.
```

**Consequence:**
If the client posts against these pillars for 90 days: what is the most likely strategic gap?
→ Answer before delivering. If the answer is "no proof content — no pipeline signal" or "no contrarian angle — no audience growth" — flag it explicitly.
