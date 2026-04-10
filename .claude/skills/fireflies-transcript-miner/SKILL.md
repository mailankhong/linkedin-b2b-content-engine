---
name: fireflies-transcript-miner
description: Search Fireflies and Google Drive for call transcripts related to a specific client and extract content intelligence — ideas, pain points, wins, and specific requests.
---

# Fireflies Transcript Miner

Call transcripts are the highest-signal content source available. Unlike Reddit or YouTube where you're mining someone else's audience, transcripts capture your client's actual conversations — the way they explain their product, the objections they hear, the wins they celebrate. One 30-minute call typically contains 3-5 post-worthy moments that the client doesn't even realize are content because they're too close to the material.

This skill searches Fireflies and Google Drive for recent transcripts, extracts content intelligence (ideas, pain points, wins, specific requests), and passes them into the content pipeline.

**Pre-flight:** Run preflight.md. Client: [name provided by user or calling skill].
**Required:** Client name, client niche.
**Trigger:** Manual ("run fireflies mining for [client]").

---

## STEP 1 — DETERMINE SEARCH TERMS

Map the client ID to search terms:

| Client ID | Search terms |
|-----------|-------------|
| amir-airtop | "airtop" |
| bharath-datapoem | "datapoem", "bharath" |
| alex-coldiq | "coldiq", "alex" |
| mai-lan-linkedin | "mai-lan", "content call" |

Search patterns to use (substitute client name from mapping above):
- "[client name] content call"
- "[client name] weekly"
- "content call [client name]"
- "weekly [client name]"

**Google Drive search patterns** (Gemini-recorded calls saved to Google Drive):
- "Content [client name]"
- "Mai Lan [client name]"

These two naming conventions cover all call recordings. For Alex, search both "Content Alex" and "Mai Lan Alex".

**Mai Lan's own content (mai-lan-linkedin):** Also search for DWY client calls that contain content material for Mai Lan:
- "Content SEOPROFY"
- "Content PIP"

---

## STEP 2 — SEARCH FIREFLIES + GOOGLE DRIVE (7-day window)

Search **both** Fireflies and Google Drive for transcripts from the **past 7 days** matching the search terms above. Run both searches in parallel.

### 2A — Fireflies

Use the Fireflies MCP `fireflies_search` or `fireflies_get_transcripts` tool.

### 2B — Google Drive

Use `mcp__google-docs__searchGoogleDocs` with `searchIn: "name"` and `modifiedAfter` set to 7 days ago (ISO 8601). Run one search per Google Drive search pattern from Step 1.

Google Drive results are transcription documents — read the document content with `mcp__google-docs__readGoogleDoc` to extract the transcript text for the extraction step.

### Fallback logic

**If no transcripts found in 7 days (across both sources):**
- Automatically expand search to **past 14 days** on both Fireflies and Google Drive
- Flag the expansion in output: "No transcripts found in past 7 days — expanded search to 14 days"
- If still none found: output "No transcripts found for [client] in past 14 days" and end the skill

**If Fireflies MCP call fails:**
- Output: "Fireflies unavailable — [error details]"
- Continue with Google Drive results only

**If Google Drive search fails:**
- Output: "Google Drive unavailable — [error details]"
- Continue with Fireflies results only

**If both fail:**
- Inform the user and end the skill.

---

## STEP 3 — EXTRACTION

For each transcript found, extract across 4 categories:

### 1. Content Ideas
Any topic, angle, or post idea mentioned or discussed. Include:
- Explicit suggestions ("let's post about X")
- Implicit angles (something explained in depth that could become a post)
- Frameworks, processes, or systems the client described

### 2. Pain Points
Frustrations, challenges, or questions the client raised. Include:
- Things they said aren't working
- Questions they're getting from their own clients or prospects
- Objections or friction they mentioned

### 3. Wins and Results
Anything positive worth turning into social proof content. Include:
- Client results mentioned ("a client of mine just...")
- Personal wins ("we closed X", "I hit Y")
- Positive feedback or reactions they described

### 4. Specific Requests
Direct asks. Include:
- "I want to post about X"
- "Can we cover Y this week"
- Topics they said they've been meaning to address
- Anything framed as a request or intention

---

## STEP 4 — OUTPUT

Present the full extraction below, then ask the user how to use it.

```
TRANSCRIPT SEARCH — [CLIENT NAME]
Search window: [7 or 14] days
Sources searched: Fireflies, Google Drive
Transcripts found: [N] ([n] Fireflies, [n] Google Drive)

[If N = 0:]
No transcripts found. Proceeding without transcript insights.

[If N > 0:]
TRANSCRIPTS FOUND
-----------------
[For each transcript:]
- Title: [transcript title]
  Source: [Fireflies / Google Drive]
  Date: [date]
  Duration: [duration]

EXTRACTED INSIGHTS
------------------
Content Ideas:
[Bulleted list, or: None found]

Pain Points:
[Bulleted list, or: None found]

Wins / Results:
[Bulleted list, or: None found]

Specific Requests:
[Bulleted list, or: None found]

STRENGTH FLAG
-------------
Strongest insights for idea generation:
[List 1-3 highest-signal items with reason — "strongest because: [direct quote or clear signal]"]
[Or: All insights are low-signal — flagged for review before use]
```

After presenting, ask:

```
Found [N] transcript(s) for [client]. Insights extracted across [N] categories.

What would you like to do?

A) Use these in idea generation now
   → trigger: run weekly ideas for [client] with transcript insights

B) Save to client folder for later
   → saves as transcripts-[YYYY-MM-DD].md in client folder

C) Both
```

Wait for user response before proceeding.

**If user selects A or C:** pass insights into weekly-idea-session as personal material input.

**If user selects B or C:** write a file to the client folder:
- Path: `clients/[client-id]/transcripts-[YYYY-MM-DD].md`
- Content: full extraction output block above

---

## VERIFICATION

Run before closing.

**Correctness:**
```
□ Did the search target the right client name/terms? Yes → pass. No → flag: wrong client matched — rerun with correct terms.
```

**Completeness:**
```
□ Were all 4 extraction categories covered for each transcript? Yes → pass. No → flag which categories are missing and why (e.g., "no wins mentioned in transcript").
```

**Context-fit:**
```
□ Are extracted ideas relevant to the client's niche and ICP? Yes → pass. No → flag low relevance: "insights extracted but may not fit [client] niche — review before use in idea generation".
```

**Consequence:**
```
□ If these insights go into idea generation now, do they add value or create noise?
  → Add value (specific, on-niche, actionable): pass
  → Noise (vague, off-topic, or low-signal): flag: "REVIEW BEFORE USE — insights are low-signal. Recommend manual review before passing to idea generation."
```

---

## CHANGELOG

v1.0 — March 2026 — initial build
v1.1 — April 2026 — added Google Drive as second transcript source (Gemini-recorded calls); search patterns "Content [client]" and "Mai Lan [client]"; mai-lan-linkedin also searches DWY clients (SEOPROFY, PIP); graceful fallback if either source fails
v1.2 — April 2026 — removed dual-mode (headless/interactive) design after weekly-production-run was retired; skill is always interactive
