---
name: headless-weekly-content-production
description: Fully autonomous weekly LinkedIn content pipeline for one client. Runs end-to-end without any human input. Searches transcripts, generates 6 ideas, develops 6 copies, grades each, builds visual briefs, attempts Gemini + Gamma image generation, creates a Google Doc in the client's Drive folder, and prints a summary in chat. Trigger phrases include "run headless weekly production for [client]" or "run the headless pipeline for [client]".
---

# Skill: Headless Weekly Content Production

Most "automated" content pipelines fail in the same place: they hit a decision gate, freeze, and wait. The human finds the stalled run three hours later and has to restart half the steps. A true headless pipeline does not pause. It makes every decision with a pre-baked rule, flags what's weak instead of stopping, and delivers a complete output that the human can review at their pace — not the pipeline's pace.

This skill is the orchestrator for one full weekly run: 6 ideas, 6 copies, 6 visuals, 1 Google Doc. Every interactive gate in every downstream skill has an override documented here. If something breaks, it logs and continues. The only stop condition is a missing client folder.

PRE-FLIGHT REQUIRED
Run preflight.md before proceeding. Client: [name provided by user]. Required fields: all standard client context files + `Google Drive Folder ID` in `client-profile.md`. If the Google Drive Folder ID is missing, flag it in the delivery summary, skip Step 5 Google Doc creation, and continue everything else.

---

## HEADLESS MODE

This skill runs without asking questions. Read this before starting.

```
HEADLESS MODE ACTIVE
— Never ask questions
— Never wait for approval
— Make all decisions using the rules in this skill file
— When a downstream skill has an interactive gate, apply the override listed in this file
— Flag issues in the delivery summary instead of stopping
— If any step fails: log the failure, continue to the next step
— The only exception: if preflight cannot locate the client folder at all, stop and ask
```

---

## Environment Variables

| Variable | Required | Used by |
|----------|----------|---------|
| `APIFY_API_KEY` | Required | Step 1 research workers |
| `GOOGLE_API_KEY` | Optional | Step 4 Gemini image generation |

If `APIFY_API_KEY` is missing, Apify-based research workers will fail — flag them in the summary and continue with the workers that don't need it (Reddit, YouTube, WebSearch).
If `GOOGLE_API_KEY` is missing, mark all posts `NEEDS MANUAL IMAGE` and skip Step 4 Gemini calls.

---

## ENV VAR VERIFICATION

Before Step 0, confirm env var values from the shell:

```bash
echo "APIFY_API_KEY=${APIFY_API_KEY:+SET}" && echo "GOOGLE_API_KEY=${GOOGLE_API_KEY:+SET}"
```

Store the confirmed values and use them throughout. Never guess or substitute `{APIFY_API_KEY}` from uncertain context.

---

## STEP 0 — TRANSCRIPT SEARCH (Fireflies + Google Drive)

Search both sources in parallel for call transcripts from the last 14 days that match the client. Transcripts feed Step 1 as personal material — they are what separates the client's real voice from generic research output.

### Build the title match rule per client

Before the first run for any client, define a title-match rule. The rule determines which Fireflies and Google Drive transcripts count as that client's personal material. Save the rule in `clients/[client-id]/client-profile.md` under a `Transcript Title Match:` field so every future run applies it automatically.

**Default rule:** title contains company name OR founder first name (any match).

**Override the default when:**
- Multiple people from the client's company appear in titled calls, and only one person's voice is the content source → use the specific person's name only, not the company. (A team of 17 with only one "voice" means a company-name match pulls mostly wrong speakers.)
- The client's work spans many other clients (e.g., an in-house content strategist, agency operator, or fractional CMO) → add broad catch-all terms that match their cross-client work, such as `"Content"` if all their calls share that keyword.
- The company name has multiple spellings in practice (e.g., `"DataPoem"` vs `"Data Poem"`, `"Ready Set"` vs `"ReadySet"`) → include both.

Example `client-profile.md` field:

```
Transcript Title Match: "Airtop" OR "Amir"
```

If the field is missing, fall back to the default rule and flag in the delivery summary: `NEEDS TRANSCRIPT RULE — add Transcript Title Match to client-profile.md`.

### Search both sources

1. Fireflies: call `mcp__fireflies__fireflies_search_transcripts` once per title term from the client's match rule. Limit 10 per call.
2. Google Drive: call `mcp__google-docs__searchGoogleDocs` with `modifiedAfter` = 14 days ago, `searchIn: "name"`. One call per title term.

### Extract from each transcript found

- Content ideas mentioned — any topic, angle, or post idea discussed
- Pain points expressed — frustrations, challenges, questions raised
- Wins or results mentioned — anything positive worth turning into social proof
- Specific requests — "I want to post about X" or "can we cover Y"

### Output from this step

- List of transcripts found (title, date, source)
- Extracted insights organized by: ideas / pain points / wins / requests
- Pass the full extracted block to Step 1 as personal material

### Failure handling

- If both sources return zero: log "No transcripts found for [client] in past 14 days — Step 1 runs without personal material." Continue.
- If Fireflies fails: use Google Drive only. Continue.
- If both fail: continue without transcripts. Never block.

---

## STEP 1 — IDEA GENERATION (weekly-idea-session)

Run `weekly-idea-session` with headless overrides. Generate **6 ideas** for every client regardless of what the skill's defaults say.

### Override map (auto-answer every gate)

| Gate in weekly-idea-session | Override |
|-----------------------------|----------|
| Phase 1 — Duration? | **1 week** |
| Phase 1 — Number of ideas? | **6** |
| Phase 1 — Run research? | **Yes. All 6 workers in parallel.** |
| Phase 3 — Personal material? | **Use transcript insights from Step 0. If empty, note "no personal material — no transcripts found" and proceed.** |
| Phase 5 — Tier 1 coverage warning | **Note gap in summary. Do not ask. Continue.** |
| Phase 5 — Category diversity warning | **Note in summary. Do not ask. Continue.** |
| Phase 5 — Evergreen vs research balance | **Accept as-is. Do not swap. Continue.** |
| Phase 6 — Lead magnet prompt | **No. Skip. Do not ask.** |

### Output required from Step 1

- Full batch of 6 ideas, each with: title, category (Teach/Lead/Prove/Connect/Respond), ICP Tier (1/2/3), source/research signal
- All 6 METHOD RESULT blocks from the research workers
- List of methods that failed or returned no results (if any)

---

## STEP 2 — COPY DEVELOPMENT (×6 ideas, 1:1)

No filter. No selection. Every idea from Step 1 becomes one post. Run `copy-developer` six times with the overrides below.

### Override map (auto-answer every gate)

| Gate in copy-developer / hook-generator | Override |
|-----------------------------------------|----------|
| copy-developer entry — Standard or lead magnet? | **Standard. Always.** |
| hook-generator Phase 1 — 5 questions | **Auto-fill from idea metadata: topic=idea title, category=idea category, goal=client CTA strategy from `cta-templates.md`, numbers=from research signal if present else "no number", target reader=idea ICP tier, media=yes (visual-brief runs in Step 3).** |
| Step 0 — Hook selection | **Auto-pick the hook with highest specificity: contains a number, named result, or concrete scene. Tie-break: most concrete noun.** |
| Step 1B — Structure selection (2–3 options) | **Accept copy-developer's top recommendation. Do not present options.** |
| Step 2B — Confidence checkpoint (2+ empty boxes) | **Do not stop. Fill missing data with `[INSERT REAL DATA — flag for client review]`. Continue.** |
| `[INSERT REAL DATA]` placeholders | **Never fabricate. Leave the flag. Note in summary.** |

### Grading loop (post-grader, runs automatically after each draft)

1. **Threshold source** — check for `clients/[client-id]/grading-profile.md`. If present, use its weighted scoring + dimension floors. If absent, use default flat 38/50.
2. **On first fail** — auto-loop once. Feed the grader's specific dimension feedback back to copy-developer. Rewrite only the failing dimensions. Do not rebuild from scratch.
3. **On second fail** — accept the attempt. Flag as `NEEDS REVIEW` with the grader's notes. Do not attempt a third rewrite.
4. **Max attempts** — 2 total per post.

### Log per post

```
POST [N]: [Title]
Status: ACCEPTED / NEEDS REVIEW
Score: [X]/50 (attempt 1) | [X]/50 (attempt 2, if run)
Dimension breakdown: [scores per dimension]
Flags: [INSERT REAL DATA count, if any]
```

---

## STEP 3 — VISUAL BRIEF (×6 posts)

Run `visual-brief` for every accepted post (including `NEEDS REVIEW` posts — the flag doesn't block visuals).

### Override map

| Gate in visual-brief | Override |
|----------------------|----------|
| Step 0 — Brand reference check | **Load from `clients/[client-id]/brand-guidelines.md`. If missing, fall back to `client-profile.md`. If neither has brand data, use neutral defaults (charcoal on white, clean sans-serif) and flag `NEEDS BRAND REFERENCE` in the summary.** |
| Step 1 — Format recommendation | **Auto-pick: carousel for frameworks/numbered lists/step-by-step, infographic for single concepts with data, quote card for short punchy posts, comparison graphic for before/after or A vs B. Do not ask.** |
| Step 4 — Generate visual now? | **Yes. Proceed directly to Step 4 image generation below.** |

### Output required

- Full 5-section visual brief for each post (format, layout, text content, visual direction, brand application)
- Format chosen (carousel / infographic / quote card / comparison / other)

---

## STEP 4 — IMAGE GENERATION (×6 posts)

**CRITICAL: use the Gemini MCP tool, not the raw REST endpoint.** The `visual-brief` skill documents a hardcoded call to `gemini-2.0-flash-exp`, which is deprecated. The MCP tool `mcp__gemini__gemini-generate-image` routes to the current `gemini-2.5-flash-image` model. Always use the MCP path.

### 4A — Gemini (primary, non-carousel posts)

For every post where visual-brief recommended infographic, quote card, comparison graphic, or other single-frame format:

Call `mcp__gemini__gemini-generate-image` with:
- `prompt` = the structured prompt built from the visual brief (subject + style + colors + typography + composition + text content + visual elements + negative prompt: "blurry, low resolution, clip art, stock photo aesthetic, warped text, misspelled words")
- `aspectRatio` = `"4:5"` (LinkedIn portrait)
- `imageSize` = `"2K"` (production resolution)

On success: attach image to the post output, mark **IMAGE GENERATED**.
On failure: retry once with a simplified prompt (drop secondary accents, simplify composition). If second attempt fails, mark **NEEDS MANUAL IMAGE**.

### 4B — Gamma (carousel posts only)

For every post where visual-brief recommended carousel:

Call `mcp__claude_ai_Gamma__generate` with:
- `inputText` = full post copy + visual brief Section 2 (slide-by-slide content)
- `format` = `"presentation"`
- `numCards` = number of slides from visual brief
- `themeId` = client-specific theme (default to Gamma's default if no custom theme documented for the client)
- `cardOptions.dimensions` = `"4x5"`

On success: attach `gammaUrl` to the post output, mark **GAMMA GENERATED**.
On failure: mark **NEEDS GAMMA**.

### 4C — No Canva / no Claude Design

Canva MCP requires OAuth (not wired up). Claude Design is a standalone web product (no MCP). Neither is in the headless flow. If a post fails both Gemini and Gamma, the final mark is **NEEDS MANUAL IMAGE** — the human handles it post-run.

---

## STEP 5 — GOOGLE DOC CREATION

Create one Google Doc in the client's Drive folder. This is the final deliverable.

### Doc metadata

- **Title:** `Weekly Content — [Client Display Name] — Week of [Monday of current week, formatted as Month D, YYYY]`
- **Folder ID:** read from `Google Drive Folder ID:` field in `client-profile.md`
- **Formatting:** follow `google-docs-formatting.md`. Never pass markdown syntax to `createDocument` or `appendToGoogleDoc` — strip markdown, then apply native styles via `applyParagraphStyle` (headings) and `applyTextStyle` (bold/italic).

### Structure

Preferred: tabs (Ideas tab + one tab per post, tab name = truncated post title).
Fallback: single document with clear section dividers if tabs are unsupported. At the top, add: "Multi-tab layout pending — sections divided below."

### IDEAS SECTION

```
WEEKLY IDEAS — [CLIENT NAME] — Week of [Date]

Full idea batch (6 generated)
[All 6 ideas with category, ICP tier, source, research signal]

Research sources used
[All 6 METHOD RESULT blocks from Step 1]

Methods that failed or returned no results
[List or: None]

Transcripts used for personal material
[List of transcript titles from Step 0, or: No transcripts found in past 14 days]
```

### PER-POST SECTION (×6)

```
POST [N]: [Post Title]

Category: [Teach / Lead / Prove / Connect / Respond]
ICP Tier: [1 / 2 / 3]
Status: [ACCEPTED / NEEDS REVIEW]

FULL POST COPY
[Complete post, formatted as it appears on LinkedIn, with line breaks preserved]

POST GRADE
Overall score: [X]/50
Dimension scores: [breakdown]
[If NEEDS REVIEW: grader notes + recommended fix]

VISUAL BRIEF
[Full 5-section brief]

VISUAL ASSETS
Image: [IMAGE GENERATED — (description/link) / NEEDS MANUAL IMAGE]
Gamma: [GAMMA GENERATED — (link) / NEEDS GAMMA / N/A — not a carousel post]
```

---

## STEP 6 — DELIVERY SUMMARY (in chat)

No Discord. No webhook. Print the summary directly in the chat where the run was triggered.

### Summary format

```
Weekly content ready for [Client Display Name]
Week of [Monday date, e.g. April 20, 2026]

Google Doc: [URL]

Ideas generated: 6
Posts developed: 6
Posts accepted: [N] / 6
Posts needing review: [N] / 6
Images generated: [N] / 6
Gamma carousels: [N] generated / [N] total carousel posts

Items needing attention:
— NEEDS REVIEW: [post titles, or: none]
— NEEDS MANUAL IMAGE: [post titles, or: none]
— NEEDS GAMMA: [post titles, or: none]
— NEEDS BRAND REFERENCE: [yes/no]
— [INSERT REAL DATA] flags: [count across all posts]

Step 0 note: [transcripts found count, or: no transcripts found in past 14 days]
```

---

## VERIFICATION

Run before closing the session. All checks are binary.

**Correctness**
- [ ] All 6 ideas from Step 1 have a matching post section in the Google Doc
- [ ] Every post has a grade score recorded
- [ ] Posts scoring below threshold after two attempts are marked NEEDS REVIEW with grader notes

**Completeness**
- [ ] Google Doc contains an Ideas section and 6 post sections
- [ ] Every post section contains: copy, grade, visual brief, asset status
- [ ] Summary block printed in chat with Google Doc URL

**Context-fit**
- [ ] Voice Profile was loaded during preflight and applied by copy-developer
- [ ] Every post maps to a client content pillar
- [ ] Category mix across the 6 posts covers at least 3 of 5 categories (flag in summary if not)
- [ ] Brand guidelines were loaded before visual-brief ran

**Consequence test**
If the client opens this Google Doc tomorrow and wants to publish all 6 posts this week — is everything readable, complete, and usable without clarification? If no, identify what is missing and add it before closing.

---

## CHANGELOG

v2.0 — April 2026 — rebuild of retired `weekly-production-run` skill.
- Renamed to `headless-weekly-content-production`
- Step 0 now searches BOTH Fireflies AND Google Drive in parallel (was Fireflies only)
- Step 0 title match is now per-client and lives in `client-profile.md` under `Transcript Title Match:`. Default rule: company name OR founder first name. Overrides documented for teams where only one voice is the content source, for cross-client operators, and for companies with multiple name spellings.
- Output fixed at 6 ideas → 6 copies for every client (was 8–10 with 4–5 selected)
- No idea selection step — every idea becomes one post (1:1)
- Image generation uses Gemini MCP tool (`mcp__gemini__gemini-generate-image`), not the deprecated `gemini-2.0-flash-exp` REST endpoint in visual-brief
- Gamma added for carousel-format posts
- Canva and Claude Design explicitly excluded (Canva needs OAuth, Claude Design has no MCP)
- Discord notification step removed — delivery is the Google Doc URL + summary printed in chat
- All interactive gates in downstream skills now have explicit override rules documented in this file
