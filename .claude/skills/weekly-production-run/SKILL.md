---
name: weekly-production-run
description: Fully autonomous weekly content production pipeline for one client. Runs end to end without human input. Generates ideas, selects top ideas, develops copy, grades posts, runs visual briefs, attempts image and Gamma generation, creates a Google Doc in the client's Drive folder, and sends a Discord notification when complete. Trigger: "run headless weekly production for [client]".
---

# Skill: Weekly Production Run

PRE-FLIGHT REQUIRED
Run preflight.md before proceeding.
Client: [name provided by user]
Required fields: all standard client context files + `Google Drive Folder ID` in client-profile.md.
If Google Drive Folder ID is missing: flag in Discord message, skip Google Doc creation, continue all other steps.

---

## HEADLESS MODE

This skill runs without asking questions. Read this before starting.

```
HEADLESS MODE ACTIVE
— Never ask questions
— Never wait for approval
— Make all decisions using the rules in this skill file
— Flag issues in the output document instead of stopping
— If any step fails: log the failure, continue to the next step
— The only exception: if preflight cannot locate the client folder at all, stop and ask
```

---

## Environment Variables

| Variable | Required | Used by |
|----------|----------|---------|
| `DISCORD_WEBHOOK_URL` | Required for Step 7 | Discord notification |
| `GOOGLE_API_KEY` | Optional | Image generation in Step 5 |
| `APIFY_API_KEY` | Required | Research workers in Step 1 |

If `DISCORD_WEBHOOK_URL` is not set: complete all steps, then note at the end: "DISCORD_WEBHOOK_URL not set — notification not sent. Copy the summary block above to Discord manually."

---

## ENV VAR VERIFICATION (run before Step 0)

Before any steps run, read and confirm env var values from the shell:

```bash
echo "APIFY_API_KEY=${APIFY_API_KEY}" && echo "DISCORD_WEBHOOK_URL=${DISCORD_WEBHOOK_URL}" && echo "GOOGLE_API_KEY=${GOOGLE_API_KEY}"
```

Store the confirmed values from this Bash output. Use these confirmed values for all subsequent env var references in this run — never attempt to substitute `{APIFY_API_KEY}` or similar from uncertain context.

- If `APIFY_API_KEY` is empty in the Bash output: log "APIFY_API_KEY not in shell environment — Apify research workers will fail. Key must be in ~/.claude/settings.json (global, not project-level)." Continue — other research methods (Reddit, YouTube, WebSearch) can still run.
- If `DISCORD_WEBHOOK_URL` is empty: note it — notification will be skipped after final step.
- If `GOOGLE_API_KEY` is empty: mark all posts NEEDS MANUAL IMAGE, skip Step 5A.

---

## STEP 0 — FIREFLIES TRANSCRIPT SEARCH (runs automatically, never blocks)

Before idea generation, search Fireflies for call transcripts from the past 7 days related to this client.

**Search patterns** (replace [client] with actual client name):
- "[client name] content call"
- "[client name] weekly"
- "content call [client name]"
- "weekly [client name]"

**Client name mapping:**
- amir-airtop → search for "airtop"
- bharath-datapoem → search for "datapoem" or "bharath"
- alex-coldiq → search for "coldiq" or "alex"
- mai-lan-linkedin → search for "mai-lan" or "content call"

**For each transcript found, extract:**
1. Content ideas mentioned — any topic, angle, or post idea discussed
2. Pain points the client expressed — frustrations, challenges, questions they raised
3. Wins or results mentioned — anything positive worth turning into social proof content
4. Any specific requests — "I want to post about X" or "can we cover Y"

**Output from this step:**
- List of transcripts found (title, date, duration)
- Extracted insights organized by: ideas / pain points / wins / requests
- Flag: "Transcript insights added to personal material pool"

**If no transcripts found:**
- Skip silently
- Add note in output: "No Fireflies transcripts found for [client] in past 7 days"
- Proceed directly to Step 1

**If Fireflies MCP call fails:**
- Flag it: "Fireflies unavailable — transcript step skipped"
- Proceed to Step 1. Never block the run.

**Pass extracted insights into Step 1 as personal material input** — treat them exactly like personal material provided by the user. This enriches idea generation with real client voice and current context.

---

## STEP 1 — IDEA GENERATION

Run weekly-idea-session in headless mode.

**Batch sizes by client:**
- alex-coldiq: generate 10 ideas
- amir-airtop: generate 8 ideas
- bharath-datapoem: generate 8 ideas
- mai-lan-linkedin: generate 8 ideas

**Headless overrides for weekly-idea-session:**
- Phase 1 questions → answer autonomously: batch size = client batch size above; research = Yes
- Phase 3 personal material → use transcript insights from Step 0 if found. If no transcripts found, skip and note in output: "No personal material — no Fireflies transcripts found. Recommend adding personal material in next session."
- Phase 5 Tier 1 flag → do not stop. Note the gap, continue.
- Phase 6 lead magnet prompt → skip. No lead magnets in headless run.
- Run all 6 research workers in parallel.

**Output required:**
- Full idea batch with all METHOD RESULT blocks
- List of methods that failed or returned no results with reason

---

## STEP 2 — IDEA SELECTION

Autonomously select top ideas for development.

**Selection counts:**
- alex-coldiq: select top 5 from 10
- all others: select top 4 from 8

**Selection criteria (apply in this order):**

1. **Research signal strength** — ideas backed by real data, live debates, or specific Reddit/YouTube evidence score higher than ideas derived only from pillar keywords
2. **Pillar coverage** — prefer selections that span more pillars over those that concentrate in one
3. **Category variety** — selections must cover at least 3 of 5 categories: Teach / Lead / Prove / Connect / Respond. If the top ideas by signal strength all fall in 2 categories or fewer, swap in the next-best idea from an underrepresented category.
4. **ICP tier** — ensure at least 2 Tier 1 ideas are included if available in the pool. If not available, note it.

**Output for each selected idea:**
```
SELECTED — IDEA [N]: [Title]
Reason: [1 sentence — why this beat competing ideas]
Category: [Teach / Lead / Prove / Connect / Respond]
ICP Tier: [1 / 2 / 3]
Research signal: [source that anchors this idea]
```

---

## STEP 3 — COPY DEVELOPMENT

For each selected idea, run the full copy development loop.

**Loop per idea:**

1. Run copy-developer in headless mode
   - Hook selection → autonomously select the hook with highest specificity (has a number, a named result, or a scene) from the hook-generator output
   - Do not present hooks for selection — pick the best one and proceed
   - Fill any [INSERT REAL DATA] placeholders with: `[INSERT REAL DATA — flag for client review]`

2. Run post-grader automatically on the completed draft

3. Scoring decision:
   - Score 38/50 or above → accept the post
   - Score below 38 → retry once:
     - Feed the grader's specific dimension feedback back to copy-developer
     - Rewrite the failing dimensions only — do not rebuild from scratch
     - Accept the second attempt regardless of score
   - If the second attempt scores below 38: flag the post as **NEEDS REVIEW** in the output

4. Log per post:
   ```
   POST [N]: [Title]
   Status: ACCEPTED / NEEDS REVIEW
   Score: [X]/50 (attempt 1) | [X]/50 (attempt 2, if run)
   Dimension breakdown: [scores per dimension]
   ```

---

## STEP 4 — VISUAL BRIEF

For each accepted post, run visual-brief in headless mode.

**Headless overrides for visual-brief:**
- Brand reference check → read from client's `client-profile.md` if present. If no brand reference in the client folder: proceed with a placeholder brand reference block and flag it as **NEEDS BRAND REFERENCE — apply before production**
- Format recommendation → make autonomously based on post content type (carousel for frameworks and numbered lists, single infographic for single concepts, quote card for short punchy posts)
- Do not ask for confirmation — generate the full brief and proceed

Output: complete 5-section visual brief for each post.

---

## STEP 5 — IMAGE GENERATION

For each visual brief, attempt image and Gamma generation.

### 5A — Image Generation (Google AI Studio)

Requires `GOOGLE_API_KEY`.

If set:
```
POST https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-image:generateContent?key={GOOGLE_API_KEY}
{
  "contents": [{
    "parts": [{
      "text": "[Visual concept from Section 1 of the brief. Full text content from Section 2. Branding from Section 4. Format: portrait, 1080x1350px. Style: [visual tone from brief].]"
    }]
  }],
  "generationConfig": {"responseModalities": ["IMAGE", "TEXT"]}
}
```

- Success → attach image to output, mark: **IMAGE GENERATED**
- Failure → save brief only, mark: **NEEDS MANUAL IMAGE**

If `GOOGLE_API_KEY` not set → mark all posts: **NEEDS MANUAL IMAGE**

### 5B — Gamma Generation (carousel posts only)

For posts where the visual brief recommends Carousel format:
- Attempt Gamma presentation generation using the brief spec
- Success → attach Gamma link, mark: **GAMMA GENERATED**
- Failure → save brief only, mark: **NEEDS GAMMA**

For non-carousel posts: skip Gamma, no flag needed.

---

## STEP 6 — GOOGLE DOC CREATION

Create a new Google Doc in the client's Google Drive folder.

**Document title:** `Weekly Content — [Client Display Name] — Week of [Monday of current week, formatted as Month D, YYYY]`

**Folder ID:** read from `Google Drive Folder ID:` field in client-profile.md

### Document structure

First, attempt to create the document with tabs:
- Tab 1: IDEAS
- Tab 2 onwards: one tab per post (tab name = post title, truncated to 30 characters)

If tab creation fails or is unsupported by the Google Docs MCP:
- Use one document with clear section dividers
- Add this note at the top of the document:
  ```
  Multi-tab layout pending — sections divided below
  ```

---

### IDEAS SECTION

```
===============================================
WEEKLY IDEAS — [CLIENT NAME] — Week of [Date]
===============================================

FULL IDEA BATCH ([N] ideas generated)
--------------------------------------
[All ideas from Step 1, formatted with ICP Tier, Category, Source, Specificity, Quality gate score]

SELECTED IDEAS ([N] selected for development)
----------------------------------------------
[Each selected idea with selection reason from Step 2]

RESEARCH SOURCES USED
----------------------
[All METHOD RESULT blocks from Step 1 — one block per worker]

METHODS THAT FAILED OR RETURNED NO RESULTS
--------------------------------------------
[List each failed method with reason, or: None]

NOTE: No personal material in this run — headless mode. Recommend adding personal material in next session for stronger Prove and Connect posts.
```

---

### PER POST SECTION (repeat for each post)

```
===============================================
POST [N]: [Post Title]
===============================================

Category: [Teach / Lead / Prove / Connect / Respond]
ICP Tier: [1 / 2 / 3]
Status: [ACCEPTED / NEEDS REVIEW]

--------------------------------------
FULL POST COPY
--------------------------------------
[Complete post draft — formatted as it appears on LinkedIn, with line breaks]

--------------------------------------
POST GRADE
--------------------------------------
Overall score: [X]/50
Dimension scores:
  [Dimension 1]: [X]/[max]
  [Dimension 2]: [X]/[max]
  [Dimension 3]: [X]/[max]
  [Dimension 4]: [X]/[max]
  [Dimension 5]: [X]/[max]

[If NEEDS REVIEW:]
NEEDS REVIEW — scored below 38/50 after two attempts
Grader notes: [specific dimension feedback]
Recommended fix: [what the client should address before publishing]

--------------------------------------
VISUAL BRIEF
--------------------------------------
[Full 5-section visual brief from Step 4]

--------------------------------------
VISUAL ASSETS
--------------------------------------
Image: [IMAGE GENERATED — [description] / NEEDS MANUAL IMAGE]
Gamma: [GAMMA GENERATED — [link] / NEEDS GAMMA / N/A — not a carousel post]
```

---

## STEP 7 — DISCORD NOTIFICATION

Send a Discord message via webhook when all steps are complete.

**Webhook:** `DISCORD_WEBHOOK_URL` environment variable

**Message format:**
```
Good morning! Weekly content ready for [Client Display Name]
Week of [Monday date, e.g. March 23, 2026]

Ideas generated: [N]
Posts developed: [N]
Images generated: [N generated] / [N total posts]
Google Doc: [document URL]

Items needing review:
[List each NEEDS REVIEW post by title, or: None]
[List each NEEDS MANUAL IMAGE post by title, or: —]
[List each NEEDS GAMMA post by title, or: —]
[List any missing data flags: NEEDS BRAND REFERENCE, or: —]
```

**Send via Bash curl (use the confirmed DISCORD_WEBHOOK_URL value from the ENV VAR VERIFICATION step):**
```bash
curl -s -o /dev/null -w "%{http_code}" -X POST "${DISCORD_WEBHOOK_URL}" \
  -H "Content-Type: application/json" \
  -d "{\"content\": \"[message text — escape quotes]\"}"
```

A 204 response = success. Any other code = failure.

If the webhook call fails: log the failure in the session. Print the full message to the terminal so it can be copied manually.

---

## VERIFICATION

Run before closing the session. All checks are binary.

**Correctness:**
```
□ Every selected idea from Step 2 has a corresponding post section in the Google Doc? Yes → pass. No → flag the missing post by name.
□ Every post has a grade score recorded? Yes → pass. No → run post-grader on the missing post.
□ Posts scoring below 38 after two attempts are marked NEEDS REVIEW? Yes → pass. No → add the flag.
```

**Completeness:**
```
□ Google Doc contains an IDEAS section and one section per post? Yes → pass. No → add the missing sections.
□ Every post section contains: copy, grade, visual brief, and asset status? Yes → pass. No → complete the missing fields.
□ Discord notification sent (or failure logged with message printed)? Yes → pass. No → send now.
```

**Context-fit:**
```
□ Voice Profile was loaded during preflight and passed to copy-developer? Yes → pass. No → flag: NEEDS VOICE REVIEW — copy may not match client voice.
□ All posts map to a client content pillar? Yes → pass. No → flag unmapped posts.
□ Category mix across all posts covers at least 3 of 5 categories? Yes → pass. No → flag the imbalance in the Discord message.
```

**Consequence:**
If the client opens this Google Doc tomorrow morning and wants to publish all posts this week — is everything readable, complete, and usable without any clarification from the content team?
→ Answer before closing. If the answer is no: identify exactly what is missing and add it.

---

## CHANGELOG

v1.0 — March 2026 — initial build
v1.1 — March 2026 — added Step 0 Fireflies transcript search; transcript insights now passed as personal material into Step 1
