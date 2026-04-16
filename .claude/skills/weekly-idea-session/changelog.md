# Changelog — weekly-idea-session

## v2.1 — 2026-04-17

**Summary of changes:**
Phase 7 output format now requires a sixth field per idea: `Recommended post date: [Month DD] or earlier | Any day (evergreen) | [Month DD] (pre-event) or [Month DD] (same-day reaction)`. Verification completeness check updated to enforce concrete dates (not vague decay windows).

**Why:**
Decay windows like "5-7 day first-mover window" put timing math on the client, who doesn't have the research context. Client picks idea Monday, schedules Thursday, window closed Tuesday. Every idea now carries a concrete post-by date or an explicit evergreen tag so the client can scan and know status at a glance.

**Known edge cases:**
- Calendar-locked events (earnings, conferences, product launches): use the event date for pre-event posts or the next day for same-day reactions.
- Stats tied to refresh cycles (quarterly earnings, annual reports): use refresh date minus 1 as the latest recommended post date.

**Gotchas:**
- Evergreen ideas still get the field filled — write "Any day (evergreen)" for consistency. The client should never wonder why some ideas have a date and others don't.

## v2.0 — 2026-03-22

**Summary of major changes:**
Full directive rewrite; Phase 5 coverage checks updated to include 5-category post-type diversity table (Teach/Lead/Prove/Connect/Respond) with >50% imbalance flag alongside ICP tier check; Phase 7 idea output uses `Category:` not `Post type:`; Verification section added.

**Known edge cases:**

**Gotchas:**
