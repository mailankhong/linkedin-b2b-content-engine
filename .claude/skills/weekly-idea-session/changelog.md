# Changelog — weekly-idea-session

## v3.0 — 2026-04-17

**Summary of changes:**
Major restructure. Research is now always-on (no opt-in question). New Phase 2.5 (Evergreen Pool) mines 5 ranked ideas from confirmed client materials. Phase 7 output restructured into three sections: Section A (Evergreen Pool, 5 ideas), Section B (Research Pool, all scraped angles ranked), Section C (Recommended Weekly Combination, default 6 ideas mixing 3 evergreen + 3 research). Phase 1 no longer asks questions — defaults to 1 week / 6 ideas / research on, overrides only from explicit user request. Phase 5 adds evergreen-research balance check. Verification updated for three-section output.

**Why:**
When running "weekly ideation," the output should always combine pillar-grounded evergreen content (long shelf life on LinkedIn, compounds over time) with timely research-sourced content (captures live market signals). Without both, the feed is either always chasing news (decays fast) or always recycling the same themes (misses market moments).

**Known edge cases:**
- Client-provided materials (whitepapers, case studies, presentations) are evergreen source material and should be prioritized when fresh.
- Some clients have AI-contaminated post archives (repurposing-archive may need to be skipped per client directive).

**Gotchas:**
- Evergreen pool stays at 5 regardless of multi-week runs — evergreen is reusable.
- Research pool is uncapped — output everything, ranking is what matters.
- Never go all-evergreen or all-research in Section C. Minimum 1 from each pool.

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
