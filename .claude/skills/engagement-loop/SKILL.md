---
name: engagement-loop
description: Pull LinkedIn engagement data and feed it back into the learning log so pattern-recognition has real numbers, not session notes. Two scenarios — Scenario A (weekly Apify auto, no impressions) and Scenario B (monthly native export with true impressions and engagement rate). Both compute period-over-period comparison and trigger year-over-year analysis when the data is ambiguous. Triggers include "run engagement loop for [client]", "run weekly engagement loop for [client]", "run monthly engagement loop for [client]", "analyze linkedin performance for [client]", or scheduled execution.
---

# Engagement Loop

Most content systems run forever on session notes and gut feel. Pattern recognition without real engagement data is just opinion with extra steps. This skill closes the loop — it pulls actual post performance, compares it to the client's own baseline, and writes the result back to the system so pattern-recognition is mining numbers, not vibes.

The skill has two scenarios because LinkedIn deliberately hides impressions from non-authenticated traffic. Scenario A (Apify) gives you fast, autonomous engagement counts every week. Scenario B (native export) gives you the truth — impressions, engagement rate, click-through — but requires you to download a file from your LinkedIn dashboard. You run both: weekly for direction, monthly for fidelity.

**Pre-flight:** Run preflight.md. Client: [name].
**Required:** LinkedIn URL (Header block), content-pillars.md, learning-log.md (auto-created if missing), `clients/[name]/analytics/` folder (auto-created if missing).

## Quality Gate

Missing LinkedIn URL → stop. State gap, impact, fix option. Never invent or fabricate engagement numbers. If Apify or the native export is unavailable, the run stops — no fallback to "estimated" data.

---

## Two Scenarios — Pick One Up Front

| Scenario | Frequency | Source | Returns | Effort |
|---|---|---|---|---|
| **A — Apify auto** | Weekly | Apify scraper | reactions, comments, reposts (no impressions) | Zero — runs autonomously |
| **B — Native export** | Monthly | LinkedIn XLSX export | impressions, engagement rate, click-through, full engagement | 5 min — you download the file |

**Trigger routing:**
- `run weekly engagement loop for [client]` → Scenario A
- `run monthly engagement loop for [client]` → Scenario B (asks for the file)
- `run engagement loop for [client]` → ask which one

**Why both:** Weekly Apify keeps pattern-recognition fed with directional signal between monthly fidelity checks. The monthly native export is the source of truth that calibrates what "good" actually means for this client. Apify alone gives you trends without context. Native alone gives you context without frequency. Together they give you a real feedback loop.

---

## Scenario A — Weekly Apify Auto

### Step 1 — Pull post history

```
POST /acts/datadoping~linkedin-profile-posts-scraper/run-sync-get-dataset-items?token={APIFY_API_KEY}
{
  "profiles": ["[LinkedIn URL from client folder Header block]"],
  "maxPosts": 100
}
```

Returns per post: `text, total_reactions, comments, reposts, posted_at`. **No impressions** — LinkedIn does not expose this to public scrapers. The skill always discloses this in its output, no exceptions.

If Apify call fails:
```
Apify call failed for engagement-loop — cannot pull post history.
Method stopped. No engagement data written this run.
Run system-check to diagnose.
```
Do not ask the user to paste posts as a substitute. Stop.

### Step 2 — Segment into periods

- **Current period:** last 7 days from today
- **Previous period:** 8–14 days ago
- **Year-ago period (conditional, only if YoY triggered):** 358–371 days ago, read from `engagement-history.md` — never re-scrape

Skip any post where `posted_at` is missing or unparseable. Note skipped count in the gaps section.

### Step 3 — Score posts

For each post: `score = comments + reposts`. Likes are vanity. Comments and reposts are intent signals — what people did when the post made them stop scrolling. This is the metric pattern-recognition needs.

Compute the **client's trailing 30-day median** from all posts in `engagement-history.md` plus the current Apify pull. If history has < 10 posts, use available count and flag the report:

```
⚠ Baseline computed on [N] posts — low-confidence percentiles. Will stabilize after ~10 weekly runs.
```

Tag each current-period post against this baseline:
- `top decile` — score in top 10% of trailing 30d
- `above median` — above median, below top decile
- `below median` — below median, above bottom decile
- `bottom decile` — bottom 10%

### Step 4 — Period-over-period comparison

Compute aggregate metrics for current vs previous period:

- Total posts in period
- Sum of `comments + reposts`
- Median post score
- Top post score
- Number of `top decile` posts

Then compute deltas: current vs previous, expressed as % change.

### Step 5 — Year-over-year trigger check

Run YoY ONLY if one of these three conditions is true. Otherwise skip — keep the report clean.

1. **Big swing:** median post score changed >30% week-over-week (either direction)
2. **Flat:** median post score changed <5% week-over-week (no movement = needs longer arc to interpret)
3. **Single-post dominance:** one post contributed >60% of total period engagement

If YoY triggered AND `engagement-history.md` has data from 358–371 days ago:
- Pull that period's aggregates from history
- Compute current vs year-ago deltas
- Add a YoY section to the report explaining which condition triggered

If YoY triggered but no year-ago data exists:
```
YoY analysis triggered ([condition]) but engagement-history.md has no data from [date range].
Available history: [oldest_date] → [newest_date]. YoY will become available [date when 12mo accumulates].
```

### Step 6 — Write to engagement-history.md

Append every current-period post to `clients/[name]/analytics/engagement-history.md`. This file is the persistent record — every weekly Apify run and every monthly native export writes here. Pattern-recognition reads from this file directly.

If `engagement-history.md` does not exist, create it with the header block defined in the **Storage Schema** section below.

For each post in the current period, add or update its entry:
- New post (not yet in history) → append a new POST block
- Existing post (already in history, being re-measured) → append a new METRICS line under that post

Never delete or overwrite previous METRICS lines. The history is append-only — that's how trends become visible.

### Step 7 — Backfill learning-log.md (best effort)

For each high-confidence match between an Apify post and a learning-log session entry:
1. Match by date window (post date within ±2 days of session date)
2. Topic match: ≥3 distinctive content words from session "Post topic" line appear in the post text (skip stopwords: the, a, and, of, to, for, on, in, at, with, this, that, is, are)
3. Confidence: high (date + 5+ word match), medium (date + 3–4 word match), low (date only or weak text match)

For high and medium confidence matches, append a new section inside the matching session entry:

```
ENGAGEMENT DATA (T+[N]d, source: apify_auto, match: [confidence]):
- Reactions: [N]
- Comments: [N]
- Reposts: [N]
- Impressions: not available (Apify limitation)
- Score (c+r): [N]
- Percentile vs 30d median: [top decile / above / below / bottom]
```

Low-confidence matches go to the report only, not the log. Flag them for human review.

### Step 8 — Output the weekly report

See **Output Format — Weekly Report** below.

---

## Scenario B — Monthly Native Export

### Step 1 — Ask for the file

If triggered without a file path:

```
Monthly engagement loop for [client] needs the native LinkedIn export.

To get it:
1. Open LinkedIn → Me → Posts & Activity → Analytics → Content
2. Top right: "Export" → choose date range "Last 30 days" (or "Last 365 days" for full history)
3. Save the XLSX as: clients/[client]/analytics/linkedin-export-[YYYY-MM-DD].xlsx
4. Reply with "ready" or paste the file path

If you've already saved the file in clients/[client]/analytics/, I'll auto-pick the most recent one.
```

Then check `clients/[name]/analytics/` for the most recent `linkedin-export-*.xlsx`. If found, proceed. If not, wait for the user.

### Step 2 — Parse the XLSX

Use flexible column detection — LinkedIn renames columns periodically. Match by header substring (case-insensitive):

| Logical field | Header substring to match |
|---|---|
| post_url | "post url" or "url" |
| post_date | "date" or "created" or "publish" |
| post_text | "title" or "content" or "post" |
| impressions | "impression" |
| reactions | "reaction" or "like" |
| comments | "comment" |
| reposts | "repost" or "share" |
| engagement_rate | "engagement rate" or "engagement %" |
| click_through_rate | "click" or "ctr" |

If a required column (impressions, reactions, comments, reposts, post_date) is missing, halt and report:

```
Native export parse failed — column "[field]" not found.
LinkedIn may have changed the export schema. Headers found: [list]
Stopping. Update engagement-loop column mapping before retry.
```

Do not guess. A wrong column mapping silently corrupts the entire history.

### Step 3 — Segment into periods

- **Current period:** last 30 days from export date
- **Previous period:** 31–60 days ago
- **Year-ago period (conditional):** 11–13 months ago

If the export covers <60 days, previous-period comparison is skipped with a note. If <13 months, YoY is skipped with a note.

### Step 4 — Score posts (true engagement rate available)

For each post in the export:
- `score = comments + reposts` (same metric as Scenario A — keeps the two scenarios comparable)
- `engagement_rate` is read directly from the export — this is the metric Scenario A cannot compute
- `impressions` recorded for future analysis

Compute the client's trailing 30-day median from the export's current period.

Tag each post against the baseline (top decile / above / below / bottom) using the score, NOT engagement_rate. Engagement rate is reported as a separate dimension, not used for ranking — because rate punishes long posts that get high absolute engagement, and pattern-recognition needs absolute signals.

### Step 5 — Period-over-period comparison

Same structure as Scenario A, but adds **two extra dimensions** Apify can't provide:

- Median engagement rate (current vs previous)
- Total impressions (current vs previous)
- Top post by impressions (separate from top by score)

### Step 6 — Year-over-year trigger check

Same three conditions as Scenario A. If triggered AND the export covers 13+ months, run YoY directly from the export. Otherwise check `engagement-history.md` for year-ago data. Otherwise skip with a note.

### Step 7 — Write to engagement-history.md

Same append-only logic as Scenario A. Native export METRICS lines include all fields:

```
- 2026-04-15 (T+14d, source: native_export):
  reactions: 47
  comments: 12
  reposts: 3
  impressions: 4823
  engagement_rate: 1.45%
  click_through_rate: 0.21%
  score: 15
```

Native export data is **always** recorded — even for posts that already have Apify METRICS lines. The two sources are stored side by side. This is how you'll eventually calibrate Apify counts against true LinkedIn metrics ("when Apify shows 50 reactions, native export shows ~62 — Apify undercounts by ~20%").

### Step 8 — Backfill learning-log.md

Same matching logic as Scenario A. With native export, the source field is `native_export` and the impressions/engagement_rate fields are populated.

### Step 9 — Output the monthly report

See **Output Format — Monthly Report** below.

---

## Storage Schema — engagement-history.md

Path: `clients/[name]/analytics/engagement-history.md`

```
# Engagement History — [Client Name]
# Append-only. Every weekly Apify run and every monthly native export writes here.
# Pattern-recognition reads from this file as the source of truth.

---
POST: [first 80 chars of post text, or learning-log topic if matched]
URL: [post URL if available, otherwise omit]
Posted: 2026-04-08
First seen: 2026-04-15
Pillar: [pillar name if matched to learning log, otherwise "unmapped"]
Hook type: [hook type if matched, otherwise "unmapped"]
Post type: [post type if matched, otherwise "unmapped"]

METRICS:
- 2026-04-15 (T+7d, source: apify_auto, match: high):
  reactions: 47
  comments: 12
  reposts: 3
  impressions: not_available
  engagement_rate: not_available
  score: 15
  percentile: above_median
- 2026-05-01 (T+23d, source: native_export, match: high):
  reactions: 52
  comments: 14
  reposts: 4
  impressions: 4823
  engagement_rate: 1.45%
  click_through_rate: 0.21%
  score: 18
  percentile: top_decile
---
```

**Why one POST block per post, multiple METRICS lines:** the same post matures over time. T+7d looks different from T+23d. Pattern-recognition needs to see the curve, not just the latest snapshot. Append-only history is the simplest way to preserve that.

---

## Output Format — Weekly Report

```
═══════════════════════════════════════════════════════════
WEEKLY ENGAGEMENT LOOP — [Client Name]
Period: [YYYY-MM-DD] → [YYYY-MM-DD]
Source: Apify auto (no impressions — see limitation note)
═══════════════════════════════════════════════════════════

CURRENT WEEK
- Posts: [N]
- Total comments + reposts: [N]
- Median post score: [N]
- Top post: "[truncated topic]" — score [N] ([percentile])
- Top decile posts: [N]

VS PREVIOUS WEEK
- Posts: [N] → [N] ([+/-]%)
- Median score: [N] → [N] ([+/-]%)
- Top decile posts: [N] → [N]

[YEAR-OVER-YEAR — only if triggered]
YoY triggered: [big swing / flat / single-post dominance]
- Median score this week: [N]
- Median score same week 2025: [N] ([+/-]%)
- Interpretation: [1 sentence — is this seasonal, structural, or noise]

TOP POSTS THIS WEEK
1. "[topic]" — score [N], [percentile], pillar: [name], hook: [type]
2. "[topic]" — score [N], [percentile]
3. "[topic]" — score [N], [percentile]

BOTTOM POSTS THIS WEEK
1. "[topic]" — score [N], [percentile]
2. "[topic]" — score [N], [percentile]

PATTERN NOTES (auto, no recommendations — that's pattern-recognition's job)
- [Hook type X dominated top posts: N of top 3]
- [Pillar Y had no posts above median this week]
- [Post type Z is overrepresented in bottom decile]

LIMITATIONS
- Impressions not available (Apify cannot read this field — LinkedIn limitation)
- Engagement rate not computable without impressions
- [Baseline confidence note if applicable]
- For true impressions and engagement rate, run monthly native export.

WRITTEN
- engagement-history.md: [N] new posts, [N] new metrics lines
- learning-log.md: [N] high-confidence backfills, [N] low-confidence flagged

NEXT
- Next weekly run: Monday [date] 9:00
- Next monthly native export prompt: first Monday of next month — [date]
═══════════════════════════════════════════════════════════
```

---

## Output Format — Monthly Report

Same structure as weekly but adds an **Impressions & Engagement Rate** section between "VS PREVIOUS PERIOD" and "TOP POSTS":

```
IMPRESSIONS & ENGAGEMENT RATE (native export)
- Total impressions this month: [N]
- Avg impressions per post: [N]
- Median engagement rate: [X.XX%]
- Top post by impressions: "[topic]" — [N] impressions
- Top post by engagement rate: "[topic]" — [X.XX%]
- Click-through rate (posts with links): [X.XX%]

VS PREVIOUS MONTH
- Total impressions: [N] → [N] ([+/-]%)
- Median engagement rate: [X.XX%] → [X.XX%] ([+/-] pp)
- Avg impressions per post: [N] → [N] ([+/-]%)
```

The rest of the format is identical. Native export reports **also** include the `LIMITATIONS` section but only mentions baseline confidence — never the impressions caveat (because impressions are present).

---

## Anti-patterns

**Don't fabricate impressions.** If Apify is the source, impressions stay `not_available`. Never estimate, infer, or back-calculate. A made-up number poisons the history forever — pattern-recognition will treat it as real.

**Don't overwrite history.** METRICS lines are append-only. If a post already has a T+7d entry from Apify, the next run adds a T+14d entry — it does not replace.

**Don't rank by engagement rate.** Engagement rate punishes long posts that earn high absolute engagement. Score (comments + reposts) is the ranking metric. Engagement rate is reported as a separate dimension.

**Don't recommend.** This skill reports. Pattern-recognition recommends. Mixing the two creates noise — the engagement loop should be a clean data layer, not an opinion layer.

**Don't skip the limitation banner.** Every Apify report must say impressions are unavailable. The user (and Mai-Lan especially) needs to know what they're looking at. Omitting the banner = inviting wrong conclusions.

**Don't prompt the user mid-run.** Scenario A is fully autonomous (built for cron). Scenario B asks for the file once at Step 1, then runs to completion. No mid-run questions.

---

## Verification

**Correctness:**
```
□ Score = comments + reposts (not likes, not engagement rate)? Yes → pass. No → fix the metric.
□ Impressions field is "not_available" for every Apify-sourced metric line — never a number? Yes → pass. No → remove the fabricated value.
□ engagement-history.md is append-only — no METRICS lines deleted or overwritten? Yes → pass. No → restore from prior state and append correctly.
□ Period boundaries match the cadence (7d for weekly, 30d for monthly)? Yes → pass. No → recompute.
```

**Completeness:**
```
□ All 8/9 steps executed in order, none skipped? Yes → pass. No → run the missing step.
□ engagement-history.md exists or was created? Yes → pass. No → create it.
□ Report includes period comparison (current vs previous)? Yes → pass. No → add it.
□ YoY section present if any of 3 trigger conditions met (or absent if not triggered)? Yes → pass. No → fix.
□ Limitations section present and accurate for the scenario used? Yes → pass. No → add it.
```

**Context-fit:**
```
□ Trailing 30-day median computed from history file (not a generic benchmark or assumed number)? Yes → pass. No → recompute from history.
□ Pattern notes are observations about THIS client's data, not generic LinkedIn advice? Yes → pass. No → rewrite as observations.
□ Backfill matches use date + topic word overlap, not loose text similarity? Yes → pass. No → re-match strictly.
```

**Consequence:**
If pattern-recognition reads engagement-history.md after this run: what is the most likely reason it draws a wrong conclusion?
→ Answer before delivering. If the answer is "low-confidence matches were treated as high-confidence" or "Apify counts compared directly to native export counts" — fix before saving.

---

## When called by a scheduled cron trigger

There is **one** weekly cron per client: `0 9 * * 1` (Mondays at 9 AM local). It always fires Scenario A (Apify auto). The skill then self-detects whether today is also the first Monday of the month and chains the monthly jobs if so.

### First-Monday-of-month detection (deterministic, runs at the start of every weekly cron)

```
today = current date
day_of_week = today.weekday()  # 0 = Monday
day_of_month = today.day

is_monday = (day_of_week == 0)
is_first_monday = is_monday AND (day_of_month <= 7)
```

If `is_first_monday`:
1. Run Scenario A (weekly Apify) to completion as normal.
2. Then run Scenario B (monthly native export) — but in **passive mode**: check `clients/[name]/analytics/` for any `linkedin-export-*.xlsx` modified in the last 14 days. If found, parse it and run the monthly analysis. If not found, surface a single Discord ping: *"Monthly engagement loop for [client] needs the native LinkedIn export. Drop the file in clients/[client]/analytics/ and reply 'run monthly engagement for [client]' when ready."* Do not block.
3. Then run [foundation-refresh-ping](../foundation-refresh-ping/SKILL.md) for the same client.

If NOT `is_first_monday`: only Scenario A runs. Exit cleanly.

### Failure logging (for Mai-Lan to catch missed/failed runs at session start)

Every cron-invoked run writes its outcome to `clients/[name]/analytics/run-log.md` — one line per run, append-only:

```
2026-04-13 09:02 — weekly_apify — SUCCESS — 12 posts analyzed, 8 backfilled
2026-04-20 09:02 — weekly_apify — FAILED — Apify call returned 503; retry manually
2026-05-04 09:02 — weekly_apify — SUCCESS — 14 posts analyzed
2026-05-04 09:04 — first_monday_chain — monthly_native — SKIPPED — no recent export found, ping sent
2026-05-04 09:05 — first_monday_chain — foundation_refresh — SUCCESS — reminder sent
```

If the run fails entirely before it can write to run-log.md (e.g., the skill itself crashed), the cron infrastructure should log to `clients/[name]/analytics/run-log.md` from the cron wrapper instead. The wrapper line in this case looks like:

```
2026-04-20 09:02 — weekly_apify — CRASHED — skill did not return; check cron output
```

**At session start**, Claude should read each client's `run-log.md` and surface any FAILED, CRASHED, or SKIPPED entries from the last 14 days as a top-of-session reminder. This is the "remind me when the system comes back on" behavior — failures live in a file Claude reads automatically.

Never block a cron trigger waiting for input. Either run autonomously or exit with a clean ping and a logged status line.

---

Engagement loop complete — [N] posts analyzed, [N] high-confidence backfills, [N] low-confidence flagged.
Next step → trigger: run pattern-recognition for [client] (recommended every 5 sessions, or after a monthly native export run)
