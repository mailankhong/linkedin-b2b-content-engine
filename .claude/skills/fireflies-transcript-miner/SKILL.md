---
name: fireflies-transcript-miner
description: Search Fireflies for call transcripts related to a specific client and extract content intelligence — ideas, pain points, wins, and specific requests. Can run standalone or be called automatically by weekly-production-run.
---

# Skill: Fireflies Transcript Miner

PRE-FLIGHT REQUIRED
Run preflight.md before proceeding.
Client: [name provided by user or calling skill]
Required fields: client name, client niche (for relevance check in verification).

**Trigger:** "run fireflies mining for [client]" OR called automatically by weekly-production-run

---

## MODE DETECTION

Determine mode before starting:

- **INTERACTIVE MODE** — triggered manually by user
- **HEADLESS MODE** — called by weekly-production-run

Headless mode is active if the calling skill is weekly-production-run. Otherwise default to interactive.

---

## HEADLESS MODE

```
HEADLESS MODE ACTIVE
— Never ask questions
— Extract silently and pass insights forward
— If transcript quality is poor or unclear: flag it in output and include what was found anyway
— Never block execution
— Log all failures and continue
```

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

---

## STEP 2 — SEARCH FIREFLIES (7-day window)

Search Fireflies for transcripts from the **past 7 days** matching the search terms above.

Use the Fireflies MCP `fireflies_search` or `fireflies_get_transcripts` tool.

**If no transcripts found in 7 days:**
- Automatically expand search to **past 14 days**
- Flag the expansion in output: "No transcripts found in past 7 days — expanded search to 14 days"
- If still none found: output "No Fireflies transcripts found for [client] in past 14 days" and end the skill

**If Fireflies MCP call fails:**
- Output: "Fireflies unavailable — [error details]"
- In headless mode: end skill silently, pass empty insights to calling skill
- In interactive mode: inform the user and end the skill

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

### HEADLESS OUTPUT (passed to calling skill)

```
FIREFLIES TRANSCRIPT SEARCH — [CLIENT NAME]
Search window: [7 or 14] days
Transcripts found: [N]

[If N = 0:]
No transcripts found. Proceeding without transcript insights.

[If N > 0:]
TRANSCRIPTS FOUND
-----------------
[For each transcript:]
- Title: [transcript title]
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

STATUS: Transcript insights added to personal material pool
```

---

### INTERACTIVE OUTPUT (presented to user)

Present the full extraction above, then ask:

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
