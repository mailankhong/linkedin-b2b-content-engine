# context.md — Machine-Level Output Schema

This file defines the standard schema that Foundation Phase skills write to client folders.
It is a template, not a filled document.

Each client folder contains their own filled version of this schema.
Skills read from the client folder version — they never read from this file directly.

When onboarding a new client, copy this schema into their client folder and fill each section
using the outputs of voice-analyzer, icp-analyzer, and content-pillars in that order.

---

## SECTION 1 — VOICE PROFILE
*Produced by: voice-analyzer*
*Required by: copy-developer, hook-generator, post-grader, profile-optimizer, lead-magnet-writer, lead-magnet-builder, visual-brief*

```
═══════════════════════════════════════
VOICE PROFILE — [CLIENT FULL NAME]
═══════════════════════════════════════

## PART 1: VOICE DNA

Tone Profile:
- Primary tone: [describe the emotional texture]
- Secondary tones: [situational tones]
- What is notably absent: [what they never sound like]

Emotional Baseline:
- Default register: [how they sound most of the time]
- When most animated: [quote a moment]
- When they pull back: [quote a moment]
- Emotions they avoid: [list]

Natural Rhythm and Sentence Patterns:
- Structure: [builds to a point / states it upfront]
- Analogy type: [technical / personal / pop culture / other]
- Sentence length: [short and punchy / long and winding / mixed]
- Written tics: [recurring patterns]
- Qualification style: [qualifies statements / makes them flat]

Vocabulary Signature:
- Words and phrases used repeatedly: [their language, not industry defaults]
- Words conspicuously avoided: [list]
- How they describe their own work: [in their terms]
- Technical depth: [how deep they go]
- Humor: [frequency and style]

---

## PART 2: WRITING RULES

DO'S:
1. [Rule] — Why: [reason] — Example: [from their material]
2. [Rule] — Why: [reason] — Example: [from their material]
3. [Rule] — Why: [reason] — Example: [from their material]
4. [Rule] — Why: [reason] — Example: [from their material]
5. [Rule] — Why: [reason] — Example: [from their material]
6. [Rule] — Why: [reason] — Example: [from their material]

DON'TS:
1. [Rule] — What it sounds like if broken: [example]
2. [Rule] — What it sounds like if broken: [example]
3. [Rule] — What it sounds like if broken: [example]
4. [Rule] — What it sounds like if broken: [example]
5. [Rule] — What it sounds like if broken: [example]
6. [Rule] — What it sounds like if broken: [example]

Words to Use / Words to Avoid:
| Use This         | Not This               |
|------------------|------------------------|
| [their word]     | [the AI/corporate version] |
| [their word]     | [the AI/corporate version] |
| [their word]     | [the AI/corporate version] |
[10–15 rows]

---

## PART 3: AI PROMPT FRAMEWORK

You are writing as [NAME].

VOICE CORE
[3–4 sentences describing their voice in a way AI can operationalize]

ALWAYS:
- [Rule 1]
- [Rule 2]
- [Rule 3]
- [Rule 4]
- [Rule 5]
- [Rule 6]

NEVER:
- [Rule 1]
- [Rule 2]
- [Rule 3]
- [Rule 4]
- [Rule 5]
- [Rule 6]

SENTENCE STYLE:
- [Rule on length]
- [Rule on structure]
- [Rule on rhythm]

NATURAL VOCABULARY:
[List of their words/phrases to pull from]

BANNED VOCABULARY:
[List of words that immediately break their voice]

LITMUS TEST:
Before outputting: Does this sound like [NAME] speaking out loud?
If it sounds like a B2B content template, rewrite it.
```

---

## SECTION 2 — ICP
*Produced by: icp-analyzer*
*Required by: weekly-idea-session, reddit-miner, youtube-miner, viral-visual-miner, hook-generator, copy-developer, post-grader, profile-optimizer, lead-magnet-writer, lead-magnet-builder*

```
═══════════════════════════════════════
ICP PROFILE — [CLIENT FULL NAME]'s Ideal Buyer
═══════════════════════════════════════

WHO THEY ARE:
[2–3 sentence description of the exact person]

## TIER 1 — Primary Buyer (decision maker, fastest to close)

DEMOGRAPHICS:
- Role: [specific title]
- Company: [size, stage, revenue range]
- Industry: [if niche-specific]
- Team size: [number]
- Years in business / role: [range]

THEIR BIGGEST PROBLEMS (in their words):
1. "[exact language they'd use]"
2. "[exact language they'd use]"
3. "[exact language they'd use]"
4. "[exact language they'd use]"
5. "[exact language they'd use]"

WHAT THEY'VE TRIED:
- [previous solutions that didn't work]
- [why those failed]

WHAT THEY ACTUALLY WANT:
- [desired outcome in their language]
- [what success looks like to them in 6–12 months]

BUYING TRIGGERS:
- [event or moment that pushes them to act]
- [event or moment that pushes them to act]

OBJECTIONS:
- [objection] → [how to address it]
- [objection] → [how to address it]

LANGUAGE MAP:
- DMs: "[how they describe their problem when reaching out]"
- Sales calls: "[how they explain it verbally]"
- Google: "[what they search when looking for solutions]"
- Community: "[how they talk about it in Slack groups, Reddit, forums]"
- Internally: "[how they think about it privately]"

CONTENT THAT GRABS THEM:
- [type of post / angle that makes them stop]
- [type of post / angle that makes them stop]

NOT YOUR ICP:
- [who to repel — role, mindset, or behavior]
- [who to repel — role, mindset, or behavior]

---

## TIER 2 — Champion / Evaluator (influences the decision, longer path)

[Same structure as Tier 1]

---

## TIER 3 — Community / Practitioner (builds audience, rarely converts directly)

[Same structure as Tier 1 — condensed version acceptable]
```

---

## SECTION 3 — CONTENT PILLARS
*Produced by: content-pillars*
*Required by: weekly-idea-session, reddit-miner, youtube-miner, viral-visual-miner, copy-developer, profile-optimizer, visual-brief*

```
═══════════════════════════════════════
CONTENT PILLARS — [CLIENT FULL NAME]
═══════════════════════════════════════

PILLAR 1: [Name — 2–4 words]
Why: [1 sentence — how this connects to ICP pain or desire]
Authority: [why this client is credible on this topic]
Conversion path: [how this moves someone closer to buying]
Content ratio: [X% of total posts]

Keywords: [2–3 search terms this pillar maps to]

Post Ideas:
1. [Topic] | [Type: Story / Framework / How-to / Contrarian / Social proof / Lead magnet] | Goal: [Engagement / Authority / Conversion / Lead gen]
2. [Topic] | [Type] | Goal: [goal]
3. [Topic] | [Type] | Goal: [goal]
[10–15 ideas per pillar]

---

PILLAR 2: [Name — 2–4 words]
Why: [1 sentence]
Authority: [why credible]
Conversion path: [how it moves buyers]
Content ratio: [X% of total posts]

Keywords: [2–3 search terms]

Post Ideas:
[10–15 ideas]

---

PILLAR 3: [Name — 2–4 words]
[Same structure]

---

PILLAR 4: [Name — 2–4 words] (if applicable)
[Same structure]

---

PILLAR 5: [Name — 2–4 words] (if applicable)
[Same structure]

---

TOTAL IDEAS: [number]
ESTIMATED COVERAGE: [X weeks at Y posts per week]
```
