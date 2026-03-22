---
name: formatting-profile
description: Capture a client's LinkedIn visual formatting preferences — how posts look on the feed. Covers line breaks, sentence length, punctuation, emphasis, post length, structure, CTA style, and emoji. Does NOT cover voice, tone, or content — those live in voice-analyzer. Optional Foundation skill, runs after profile-optimizer. Triggers include "run formatting-profile for [client]", "capture formatting preferences", or when a client has strong formatting opinions.
---

# Skill: Formatting Profile

PRE-FLIGHT REQUIRED
Run preflight.md before proceeding.
Client: [name provided by user]

---

You can enter the pipeline here directly. Bring what you have. If prerequisite context is missing, I will ask for only what I need — I will not force you to start over.

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

**Formatting-specific quality rule:** If any answer is vague, show a concrete example of what each option looks like in a real post. Ask for clarification on that dimension only. Never guess formatting preferences.

---

**Purpose:** Capture only how posts look on the feed — the 8 visual formatting dimensions that shape scannability and rhythm. This skill contains nothing about tone, voice, vocabulary, vulnerability, or content. Those live in voice-analyzer.

**Pipeline position:** Optional. Runs after profile-optimizer. Can also run standalone at any time.

---

## SCENARIO ROUTING

```
If the user has provided a completed questionnaire or document with formatting information:
→ SCENARIO B — Document Analysis

If the user has provided only a request to run this skill with no document:
→ SCENARIO A — Interview
```

---

## SCENARIO A — INTERVIEW

Ask all 8 questions together in a single message:

```
Quick formatting check for [client] — 8 preferences, purely about how posts look on the feed:

1. Line breaks:
   A) Lots of white space — airy and scannable
   B) Moderate spacing
   C) Dense paragraphs

2. Sentence length:
   A) Short and punchy — under 15 words
   B) Mixed short and medium
   C) Flowing and complex

3. Punctuation:
   A) Minimal — periods and commas only
   B) Em dashes for flow
   C) Ellipses for pauses
   D) Mix

4. Emphasis:
   A) Bold key points
   B) ALL CAPS
   C) Arrows and bullets
   D) Clean text only
   E) Mix — specify which

5. Post length default:
   A) Under 100 words
   B) 100–200 words
   C) 200–400 words
   D) 400+ words
   E) Varies by type

6. Structure default:
   A) Numbered lists
   B) Bullet points
   C) Story prose
   D) Problem → Solution → Result
   E) Mix — specify when

7. CTA style:
   A) Direct ask — comment, DM, book a call
   B) Soft question
   C) No CTA — pure value
   D) Varies

8. Emoji:
   A) Never
   B) 1–2 maximum
   C) Moderate
   D) Frequent
```

After answers: proceed directly to OUTPUT. Do not ask follow-up questions unless a specific answer is contradictory or blank — in that case, ask only about that dimension.

---

## SCENARIO B — DOCUMENT ANALYSIS

Parse the provided questionnaire or document.

Extract only the 8 formatting dimensions above.
Ignore all sections about tone, vocabulary, storytelling, vulnerability, inspiration, or any content preferences — those belong in voice-analyzer.

Flag any of the 8 dimensions that are:
- Blank or unanswered → apply documented default, flag it
- Contradictory (e.g., "short sentences" + "flowing and complex") → flag the conflict, suggest the resolution

Proceed to OUTPUT.

---

## OUTPUT

Save as `formatting-profile.md` in the client's folder.

```
FORMATTING PROFILE — [CLIENT NAME]
Built: [date]
Scenario: [A — interview / B — document analysis]

---

VISUAL RULES
Line breaks: [specific rule — e.g., "one blank line between every paragraph; never 3+ lines of continuous text"]
Sentence length: [specific rule with one example — e.g., "under 15 words per sentence; 'This system cuts onboarding time by half.' not 'By implementing this system, you can significantly reduce the time it takes to onboard new team members.'"]
Paragraph max: [lines before a break — e.g., "2 lines maximum before white space"]

---

PUNCTUATION AND EMPHASIS
Use: [list — e.g., periods, commas, dashes]
Never use: [list — e.g., ellipses, semicolons]
Emphasis tools: [list — e.g., bold for key terms only; no ALL CAPS; no bullets in body]

---

POST STRUCTURE
Default format: [specific — e.g., "story prose with one insight at the end"]
Length target: [word range — e.g., "150–250 words"]
List usage: [when yes, when no — e.g., "numbered lists for step-by-step only; not for general points"]

---

CTA AND ENGAGEMENT
Default close: [specific — e.g., "direct question the ICP has a strong opinion on"]
Emoji rule: [specific — e.g., "never" / "one at most, only in CTA line"]

---

GAPS FLAGGED: [any of the 8 dimensions left blank — with default applied and labeled as DEFAULT]
CONFLICTS FLAGGED: [any contradictions — with recommended resolution]
```

---

Formatting profile saved for [client].
copy-developer will apply these rules automatically.
Foundation fully complete.

→ trigger: run weekly ideas for [client]

---

## Verification

**Correctness:**
```
□ All 8 dimensions captured with specific rules — not just option letters? (e.g., "one blank line between every paragraph" not just "A")? Yes → pass. No → convert options to explicit rules.
□ Any contradictions flagged and resolved — not silently applied one way? Yes → pass. No → flag the conflict before saving.
□ Document contains zero voice, tone, vocabulary, or content preferences — those belong in voice-analyzer? Yes → pass. No → remove them.
```

**Completeness:**
```
□ All 8 dimensions present in the output: line breaks, sentence length, punctuation, emphasis, post length, structure, CTA style, emoji? Yes → pass. No → fill the missing ones.
□ Any blank answers applied with a documented default and labeled as DEFAULT? Yes → pass. No → apply the default and label it.
□ Scenario documented (A — interview / B — document analysis)? Yes → pass. No → add it.
```

**Context-fit:**
```
□ Rules are specific enough that copy-developer can apply them without judgment calls? (e.g., "under 15 words per sentence" is specific; "keep it short" is not) Yes → pass. No → add the specific rule.
□ Formatting profile saved to client folder as formatting-profile.md? Yes → pass. No → save it.
□ If this overrides a default in linkedin-framework.md, is the override explicit? Yes → pass. No → note the override.
```

**Consequence:**
If copy-developer applies this formatting profile and the client says "this doesn't look like my posts": what dimension is most likely off?
→ Name it before finalizing. If the answer is "line break rule is too vague" or "sentence length not specified" — fix it.
