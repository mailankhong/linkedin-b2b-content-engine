---
name: pattern-recognition
description: Reads the full learning log for a client and identifies patterns across sessions — what consistently gets changed, which hook types get rewritten, which post types underperform, and where voice drift is occurring. Outputs two things: recommended client folder updates (applies with confirmation) and recommended skill file updates (presents for review only, never applies automatically). Triggers after every 5 sessions automatically, or manually with "run pattern recognition for [client]".
---

# Skill: Pattern Recognition

PRE-FLIGHT REQUIRED
Run preflight.md before proceeding.
Client: [name provided by user]
Methods requiring client context: learning-log.md (required — this skill cannot run without it).

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

**Purpose:** Find what the learning log reveals about this client's content patterns. Translate recurring observations into specific, actionable improvements — to the client folder and, when warranted, to the skill files themselves.

**Run cadence:** Automatically after every 5 sessions logged in learning-log.md. Or manually on demand.

**Critical rule:** Recommended client folder updates apply with user confirmation. Recommended skill file updates are presented for human review only — never applied automatically.

---

## STEP 1 — READ THE FULL LEARNING LOG

Read every entry in `learning-log.md` for this client.

If fewer than 3 sessions are logged:
```
Only [N] sessions logged for [client]. Patterns require at least 3 sessions to be reliable.
I'll surface what I can see, but treat these as early signals rather than confirmed patterns.
```

Proceed regardless.

---

## STEP 2 — IDENTIFY PATTERNS

Analyze all log entries across these five dimensions:

**A. What consistently gets changed**
- What fields are modified in almost every session? (hooks, structure, specific words, CTAs?)
- Is there a recurring edit the user always makes? What is the common element?
- Pattern threshold: appears in 3+ sessions = flag as a pattern

**B. Hook types that get rewritten**
- Which emotional triggers (desire / curiosity / fear / mixed) consistently get selected and then revised?
- Which hook templates get chosen but then modified significantly?
- Are there hook patterns that get selected and published without changes? (These are working — note them)

**C. Post types that underperform**
- Cross-reference post types with engagement data where available
- Are framework posts, story posts, or contrarian posts consistently getting lower engagement?
- Are certain ICP tiers consistently outperforming others?

**D. Voice drift**
- Are the voice edits the user makes moving the output toward or away from the documented voice profile rules?
- Is there a banned word or phrase appearing repeatedly in drafts that shouldn't be there?
- Is there a voice rule in the profile that seems to produce output the user consistently revises away from?

**E. Research method signal quality**
- Are ideas from certain research methods (Reddit, YouTube, real-time signal, etc.) consistently making it to final batch?
- Are ideas from certain methods consistently getting rejected? This signals a signal quality problem with that method for this client.

---

## STEP 3 — OUTPUT: PATTERN REPORT

```
PATTERN RECOGNITION REPORT — [CLIENT NAME]
Date: [today's date]
Sessions analyzed: [N]
Date range: [first entry date] to [last entry date]

---

PATTERNS IDENTIFIED:

[For each pattern found:]

PATTERN: [Short name, e.g., "Hook rewrites on curiosity templates"]
Observed in: [N] of [total] sessions
What happens: [Specific description — e.g., "Curiosity hooks from templates 11-15 are selected but consistently rewritten before final draft. User typically adds a specific number or personal reference."]
Signal: [What this suggests — e.g., "Curiosity templates need to be more specific out of the gate. The gap is the lack of concrete detail in the initial generation."]

[Repeat for each pattern]

---

PATTERNS NOT YET CONFIRMED (early signals — 1-2 occurrences):
[List anything that appeared twice but isn't yet a pattern — worth watching]

---

NO PATTERNS FOUND IN:
[List any dimensions where the data showed no consistent signal]
```

---

## STEP 4 — RECOMMENDED UPDATES

### Client Folder Updates (applies with your confirmation)

For each pattern that points to a specific fix in the client folder:

```
RECOMMENDED CLIENT FOLDER UPDATE [N]:
What to change: [field name in client folder]
Current value: "[current text]"
Proposed value: "[new text]"
Why: [which pattern this addresses, how many sessions it appeared in]

Confirm to apply? Yes / No
```

Wait for explicit confirmation before writing each change. Do not batch-apply.

Examples of client folder updates:
- Adding a voice rule to the profile that catches a recurring drift
- Adding a word to the banned vocabulary list that keeps appearing in drafts
- Updating a content pillar description that's producing consistently weak ideas
- Flagging a research method as low-signal for this client

---

### Skill File Updates (presents for review only — never applies automatically)

For each pattern that suggests a skill file needs to change:

```
RECOMMENDED SKILL FILE UPDATE [N]:
Skill: [skill name]
What to change: [describe the specific rule, instruction, or section]
Why: [which pattern this addresses across how many sessions and clients]
Proposed change: [exact text — show the before and after]

STATUS: Presented for your review only.
I will not apply this change. To apply: review the skill file and edit it directly,
or instruct me to apply it explicitly in a separate session.
```

Examples of skill file updates:
- A hook template that consistently produces output that gets rewritten
- A voice application rule in copy-developer that isn't working
- A quality gate filter in weekly-idea-session that's blocking good ideas
- A post structure in copy-developer that consistently underperforms for this type of content

---

## STEP 5 — LOG THE PATTERN RUN

Append to `learning-log.md`:

```
---
PATTERN RECOGNITION RUN: [Date]
Sessions analyzed: [N]
Patterns identified: [N]
Client folder updates applied: [N — list what was changed]
Skill file updates surfaced: [N — list what was recommended, not applied]
---
```

---

Pattern recognition complete.
Client folder updates applied: [N — list]
Skill file updates surfaced for review: [N — list]

→ Next run: after 5 more sessions, or manually with "run pattern recognition for [client]"

---

## Verification

**Correctness:**
```
□ Every pattern listed appeared in 3+ sessions — not inferred from 1-2 data points? Yes → pass. No → move it to "Early Signals" section.
□ Recommended client folder updates show exact before/after text — not just "update the voice profile"? Yes → pass. No → add the specific proposed value.
□ Skill file updates presented for review only — not applied? Yes → pass. No → do not apply them. Present only.
```

**Completeness:**
```
□ All 5 dimensions analyzed: what gets changed, hook types rewritten, post types underperforming, voice drift, research method quality? Yes → pass. No → analyze the missing dimensions.
□ Pattern run logged to learning-log.md? Yes → pass. No → append the log entry.
□ "No patterns found" sections stated explicitly for dimensions with no signal? Yes → pass. No → add them.
```

**Context-fit:**
```
□ Patterns are specific to this client — not generic observations about LinkedIn content? Yes → pass. No → remove generic observations.
□ Early signals (1-2 occurrences) clearly separated from confirmed patterns (3+)? Yes → pass. No → separate them.
□ Research method quality analysis cross-references accepted vs. rejected ideas — not just volume? Yes → pass. No → cross-reference before concluding.
```

**Consequence:**
If client folder updates are applied based on these patterns: what is the most likely unintended side effect?
→ Answer before presenting recommendations. If the answer is "removing a voice rule that was working well most of the time" — flag it before recommending the change.
