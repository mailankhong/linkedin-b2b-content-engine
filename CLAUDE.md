# LinkedIn Content Engine

A portable, multi-client system for creating authentic, strategic LinkedIn content that builds pipeline over time.

---

## Pipeline Entry Points

Enter at any stage. Bring what you have.

```
ideas → hooks → copy → grade → visual
  ↓        ↓       ↓      ↓       ↓
weekly  hook-   copy-  post-  visual-
idea-   gen-    dev-   grader brief
session erator  eloper
```

Every production skill accepts a direct entry. You do not need to start over.

---

## Skills Reference Map

### FOUNDATION — Run once per client, in this order

| # | Skill | What it does |
|---|-------|-------------|
| 1 | `voice-analyzer` | Build voice profile — 3 scenarios: online research, document input, or research-only audit |
| 2 | `icp-analyzer` | Build ICP document — 6 layers, 3 buyer tiers, 2 scenarios |
| 3 | `content-pillars` | Define 3–5 pillars and generate 30–75 post ideas — 2 scenarios |
| 4 | `profile-optimizer` | Rewrite LinkedIn profile using foundation outputs |
| 5 | `formatting-profile` | (Optional) LinkedIn visual formatting preferences — line breaks, length, structure, CTA style, emoji |

Foundation outputs are written to the client folder using the schema in `context.md`.

Run `formatting-profile` if the client has formatting preferences or a completed style questionnaire. Trigger: run formatting-profile for [client]

---

### WEEKLY PRODUCTION — Full pipeline

| Step | Skill | What it does |
|------|-------|-------------|
| 1 | `weekly-idea-session` | Generate idea batch — spawns 6 parallel research workers, quality gate, ICP tier check |
| 2 | `hook-generator` | Generate 20+ hook variations for an approved idea |
| 3 | `copy-developer` | Develop approved idea into full post — runs hook-generator first, routes lead magnets |
| 4 | `post-grader` | Score post on 5-dimension rubric (/50) — below 38 returns to copy-developer with specific fixes |
| 5 | `visual-brief` | Build structured visual brief for posts needing a designed asset |
| 6 | `lead-magnet-writer` | Write lead magnet post — surfaces at weekly session end and at copy-developer entry |
| 7 | `lead-magnet-builder` | Build the actual resource promised in the lead magnet post |

**Copy agent loop:** copy-developer → post-grader → if below 38/50 → back to copy-developer (max 2 loops).

**Lead magnet surfaces at two points only:**
- End of weekly-idea-session: "Would you like one post to be a lead magnet?"
- Entry to copy-developer: "Is this a standard post or a lead magnet post?"

---

### RESEARCH MINERS — Called by weekly-idea-session OR standalone

| Skill | What it does |
|-------|-------------|
| `reddit-miner` | Reddit thread JSON → pain extraction → hooks → post angles |
| `youtube-miner` | YouTube video → Gemini analysis → frameworks → post angles |
| `viral-visual-miner` | Scenario A: Claude searches proactively / Scenario B: analyze a visual you drop |
| `real-time-signal` | X/Twitter + LinkedIn via Apify → live debates → POV post angles |
| `repurposing-archive` | Client post history via Apify → content gaps and refresh angles |
| `reverse-engineering` | Viral posts via Apify → text templates (Scenario A) + visual frameworks (Scenario B) |

All miners return a standardized METHOD RESULT block when called by weekly-idea-session.

---

### MAINTENANCE — Run on schedule or on demand

| Skill | Cadence | What it does |
|-------|---------|-------------|
| `feedback-capture` | Every session (automatic) | Silently logs what was produced, changed, and rejected — appends to learning-log.md |
| `pattern-recognition` | Every 5 sessions or on demand | Reads learning log — identifies patterns, recommends client folder updates (with confirmation) and skill updates (review only) |
| `content-auditor` | Monthly / quarterly | Full audit — pillar relevance, voice freshness, performance patterns, maintenance report |

---

## Orchestrator — Natural Language Routing

Users shouldn't need to memorize skill names. When a request comes in, match it to the right skill automatically using the intent map below. If the request maps clearly to one skill, run it. If ambiguous, state which skill you'd route to and why — then proceed on confirmation.

| User says something like... | Route to |
|---|---|
| "write a post about X" / "develop this idea" / "turn this into a post" | `copy-developer` |
| "what should [client] post this week" / "generate ideas" / "start the weekly session" | `weekly-idea-session` |
| "grade this" / "is this post good" / "score this" / "review my post" | `post-grader` |
| "give me hooks for X" / "I need opening lines" | `hook-generator` |
| "make a visual brief" / "this needs a visual" / "design for this post" | `visual-brief` |
| "build [client]'s voice profile" / "analyze their voice" | `voice-analyzer` |
| "who is [client]'s audience" / "build the ICP" | `icp-analyzer` |
| "define pillars" / "what should they talk about" | `content-pillars` |
| "mine Reddit for ideas" / [drops Reddit URL] | `reddit-miner` |
| "analyze this video" / [drops YouTube URL] | `youtube-miner` |
| "find viral visuals" / [drops image/screenshot] | `viral-visual-miner` |
| "what's trending" / "live debates in [niche]" | `real-time-signal` |
| "repurpose old posts" / "what worked before" | `repurposing-archive` |
| "reverse-engineer this post" / "what makes this work" | `reverse-engineering` |
| "optimize their profile" / "rewrite their headline" | `profile-optimizer` |
| "lead magnet post" / "gated content post" | `lead-magnet-writer` → `lead-magnet-builder` |
| "run an audit" / "monthly review" | `content-auditor` |
| "check patterns" / "what keeps getting changed" | `pattern-recognition` |
| "repurpose this for other platforms" / "turn this into a thread" | `repurpose-from-linkedin` |
| "pull engagement data" / "how did their posts do" | `engagement-loop` |
| "search transcripts" / "find call recordings" | `fireflies-transcript-miner` |
| "run system check" / "test all connections" | `system-check` |

**Multi-skill sequences** — some requests imply a chain:
- "write and grade a post" → `copy-developer` → `post-grader` (this already happens automatically)
- "full session for [client]" → `weekly-idea-session` → pick ideas → `copy-developer` for each
- "onboard [client]" → `voice-analyzer` → `icp-analyzer` → `content-pillars`

**Fallback:** If the request doesn't match any row, ask: "Which of these sounds closest to what you need?" and list the 2-3 most likely skills with one-line descriptions.

---

## Environment Variables

| Variable | Required | Used by |
|----------|----------|---------|
| `APIFY_API_KEY` | Required | repurposing-archive, reverse-engineering, real-time-signal, youtube-miner (Scenario B) |
| `X_COOKIES` | Optional | real-time-signal — improves X/Twitter results; unauthenticated search attempted if not set |
| `GOOGLE_API_KEY` | Optional | visual-brief — enables in-session visual generation via Google AI Studio (Gemini) |

---

## Skill Design Standards

When creating a new skill or updating an existing one, apply these principles (from the Vibe Skill Creator framework):

**Voice:** Write like a domain expert, not a documentation page. Open every skill with 2-3 sentences of domain expertise — why this process matters, what most people get wrong, what good looks like. "Most LinkedIn hooks fail because they describe the topic instead of earning the click" — not "This skill generates hooks."

**Principles with WHY:** Every rule must explain WHY it exists. "Don't start with 'I'" is weak. "Don't start with 'I' — LinkedIn truncates after 2 lines on mobile, and 'I' signals self-focus before the reader has any reason to care" transfers thinking. Claude can adapt rules with WHY; it can only follow rules without WHY.

**Anti-patterns:** Telling Claude what NOT to do is as important as what to do. Name the specific failure patterns, show wrong vs right examples, and list banned vocabulary/phrases.

**Ruthless focus:** Every section must earn its place. Test: "Does removing this section make the output worse?" If no, remove it. Target under 500 lines per skill. No "just in case" sections.

**No boilerplate bloat:** Quality gate = one compact line ("Insufficient input → stop. State gap, impact, fix option."). Pre-flight = one line. No repeated "You can enter the pipeline here directly" blocks.

**Real examples:** Before/after transformations, not theoretical descriptions. Show the contrast between bad output and good output.

**Self-test:** Before finalizing any new or updated skill, compare output with the skill vs Claude's default output without the skill. If there's no meaningful difference, the skill isn't teaching Claude anything new.

---

## Working Principles

- Before any skill that requires client context, run `preflight.md`. It locates the client folder, validates required fields, extracts keywords and profile data, and surfaces exactly what is missing. Never proceed with incomplete context.
- Never fabricate data, numbers, or results. If something is missing, flag it with [INSERT REAL DATA] and move on.
- Always write in the client's documented voice — from their Voice Profile. If the Voice Profile hasn't been built yet, run `voice-analyzer` first.
- Every content decision should trace back to the ICP. Ask: which tier does this speak to, and why does it matter to them?
- Enter the pipeline at any stage — never force a restart. Every production skill accepts direct entry with whatever context is available.
- Sub-agent spawning is permitted for research methods in weekly-idea-session — run all 6 workers in parallel.
- feedback-capture runs at the end of every production session automatically.
- pattern-recognition recommendations for skill files require explicit human approval before any change is made. They are presented for review only — never applied automatically.
- The client's judgment is the final filter. Generated content is a first draft, not a finished product.
- When in doubt about workflow, reference `guide.md`.
- At session start: pull latest from GitHub before doing anything.
- At session end: push all changes to GitHub automatically.
