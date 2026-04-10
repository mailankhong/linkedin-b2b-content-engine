---
name: system-check
description: Pre-run diagnostic that tests every component of the content machine before a production round. Tests env vars, all Apify actors, Reddit, WebSearch, Discord, Google Docs MCP, Gemini MCP, and Fireflies MCP. Outputs a pass/fail checklist and a GO / NO-GO decision. Run this before any production round. Trigger: "run system check".
---

# System Check

A production round that fails mid-run wastes more time than one that never starts. This skill tests every live dependency — env vars, Apify actors, Reddit, WebSearch, Discord, Google Docs MCP, Gemini MCP, Fireflies MCP — before you commit to a full run. Takes 2-3 minutes. No client context required.

---

## HEADLESS MODE

```
HEADLESS MODE ACTIVE
— Run all tests without asking questions
— Never stop on a failure — test everything and report at the end
— Each test is independent — a failure in one does not skip others
— Output the full checklist at the end, then the GO / NO-GO decision
```

---

## STEP 1 — ENV VAR CHECK

Run this Bash command and read the output:

```bash
echo "APIFY_API_KEY=${APIFY_API_KEY}" && \
echo "GOOGLE_API_KEY=${GOOGLE_API_KEY}" && \
echo "DISCORD_WEBHOOK_URL=${DISCORD_WEBHOOK_URL}" && \
echo "X_COOKIES=${X_COOKIES}" && \
echo "GAMMA_API_KEY=${GAMMA_API_KEY}"
```

For each variable:
- If the value is non-empty → PASS
- If the value is empty → FAIL — note which var is missing

Store the confirmed values. Use them for all subsequent tests — never assume a variable is set without seeing it in the Bash output.

---

## STEP 2 — APIFY: LinkedIn Post Search Scraper

Tests: reverse-engineering, real-time-signal

```bash
curl -s -X POST "https://api.apify.com/v2/acts/datadoping~linkedin-posts-search-scraper/run-sync-get-dataset-items?token=${APIFY_API_KEY}&timeout=60" \
  -H "Content-Type: application/json" \
  -d '{"keywords": ["B2B LinkedIn"], "maxResults": 3}'
```

- Response is a JSON array with 1+ items → PASS
- Response contains `"error"` key → FAIL — log the error message
- Empty array `[]` → FAIL — actor returned no results
- Timeout or network error → FAIL

---

## STEP 3 — APIFY: LinkedIn Profile Posts Scraper

Tests: repurposing-archive

```bash
curl -s -X POST "https://api.apify.com/v2/acts/datadoping~linkedin-profile-posts-scraper/run-sync-get-dataset-items?token=${APIFY_API_KEY}&timeout=60" \
  -H "Content-Type: application/json" \
  -d '{"profiles": ["https://www.linkedin.com/in/mai-lan-khong/"], "maxPosts": 2}'
```

- Response is a JSON array with 1+ items → PASS
- Response contains `"error"` key → FAIL — log the error message
- Empty array `[]` → FAIL
- Timeout or network error → FAIL

---

## STEP 4 — REDDIT DIRECT ACCESS

Tests: reddit-miner

Use WebFetch on: `https://www.reddit.com/r/sales/top.json?limit=3&t=week`

- Response contains `"data"` key with posts → PASS
- Redirect, auth wall, or error response → FAIL
- Empty or no posts returned → PARTIAL — note it, do not block

---

## STEP 5 — WEBSEARCH

Tests: real-time-signal, viral-visual-miner, youtube-miner

Run a WebSearch for: `B2B LinkedIn content strategy after:2026-03-01`

- Returns at least 3 results → PASS
- Returns 0 results or error → FAIL

---

## STEP 6 — DISCORD WEBHOOK

Tests: Discord notifications used by remote-trigger reminders and any skill that pings Discord

```bash
curl -s -o /dev/null -w "%{http_code}" -X POST "${DISCORD_WEBHOOK_URL}" \
  -H "Content-Type: application/json" \
  -d "{\"content\": \"[System Check] Discord webhook test — $(date '+%Y-%m-%d %H:%M'). This is an automated pre-run check, not a production notification.\"}"
```

- HTTP 204 → PASS
- Any other status → FAIL — log the status code
- If DISCORD_WEBHOOK_URL was empty in Step 1 → SKIP (already flagged)

---

## STEP 7 — GOOGLE DOCS MCP

Tests: visual-brief, lead-magnet-builder, fireflies-transcript-miner (Google Drive transcript reads)

Call `mcp__google-docs__getRecentGoogleDocs` or `mcp__google-docs__listSharedDrives` — any call that confirms the MCP is responding.

- MCP responds without error → PASS
- MCP tool unavailable or returns error → FAIL — note the error

---

## STEP 8 — GEMINI MCP

Tests: youtube-miner Scenario A, image generation fallback

Call `mcp__gemini__gemini-query` with prompt: `"Reply with the single word: PASS"`

- Returns a response → PASS
- MCP tool unavailable or returns error → FAIL

---

## STEP 9 — FIREFLIES MCP

Tests: fireflies-transcript-miner

Call `mcp__fireflies__fireflies_get_transcripts` — request last 7 days.

- MCP responds (even if 0 transcripts found) → PASS — the tool is available
- MCP tool unavailable, permission error, or API key error → FAIL — note the error

---

## OUTPUT — SYSTEM CHECK REPORT

Print this checklist after all 9 steps complete:

```
SYSTEM CHECK REPORT — [date and time]
======================================

ENV VARS
  APIFY_API_KEY         [PASS / FAIL]
  GOOGLE_API_KEY        [PASS / FAIL]
  DISCORD_WEBHOOK_URL   [PASS / FAIL]
  X_COOKIES             [PASS / FAIL]
  GAMMA_API_KEY         [PASS / FAIL — note: Gamma API deprecated, key not functional]

APIFY
  Posts search scraper  [PASS / FAIL — reason if failed]
  Profile scraper       [PASS / FAIL — reason if failed]

PLATFORM ACCESS
  Reddit direct JSON    [PASS / PARTIAL / FAIL]
  WebSearch             [PASS / FAIL]
  Discord webhook       [PASS / FAIL / SKIP]

MCP TOOLS
  Google Docs           [PASS / FAIL]
  Gemini                [PASS / FAIL]
  Fireflies             [PASS / FAIL]

======================================
RESULT: [GO / NO-GO]

[GO conditions: all APIFY tests pass, WebSearch passes, Google Docs MCP passes, Discord passes]
[NO-GO conditions: any of the above fail]

[If NO-GO: list exactly which components failed and what to fix]
[If GO with warnings: list PARTIAL or FAIL items that won't block the run but will reduce output quality]
======================================
```

---

## GO / NO-GO LOGIC

**NO-GO — do not start the production run:**
- Either Apify actor returns an error (rate limit, auth, or invalid response)
- Google Docs MCP unavailable — cannot create the output document
- APIFY_API_KEY or GOOGLE_API_KEY missing from env

**GO WITH WARNINGS — run proceeds, note reduced quality:**
- Discord webhook fails — run completes, notification skipped, summary printed to terminal
- Fireflies MCP unavailable — transcript step skipped, run proceeds
- Reddit PARTIAL — fewer results, run proceeds
- X_COOKIES missing — Twitter/X signals limited, run proceeds
- Gemini unavailable — images marked NEEDS MANUAL IMAGE, run proceeds

**Full GO — all systems operational:**
- All 9 tests pass or return PARTIAL at worst

---

## CHANGELOG

v1.0 — March 2026 — initial build
