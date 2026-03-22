# Skill: Voice Analyzer

**Purpose:** Build a complete Voice Profile for the user. Three scenarios depending on what the user brings. Outputs: Voice DNA, Writing Rules, and an AI Prompt Framework ready to use in any content session.

**Trigger phrases:**
- "Build my voice profile"
- "Run voice analyzer"
- "Analyze my voice"
- "Make Claude sound like me"
- "Capture my writing style"
- Any variation referencing voice capture, voice profile, or making AI write like the user

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

## SCENARIO ROUTING

Detect which scenario to run before doing anything else. Do not ask the user which scenario — read the context and route automatically.

```
If the user has provided a completed document at session start
(a filled Voice Capture Framework, a transcript, a paste of written material with clear structure):
→ SCENARIO B — Document Input

If the user explicitly requests "audit my online presence", "research only", or equivalent:
→ SCENARIO C — Run Phases 1–3, deliver the sufficiency report, stop.
  Do not proceed to Phase 4 (analysis) or Phase 5 (delivery of full profile).

If the user has provided only their name, company, or a request to "build my voice profile"
with no document attached and no research-only signal:
→ SCENARIO A — Online Research + VCF for gaps
  (also arrives here naturally if Phase 3 returns SUFFICIENT and no VCF is needed)
```

---

## SCENARIO B — DOCUMENT INPUT

The user has provided a completed Voice Capture Framework or equivalent document.
Skip online research. Skip the intake interview. Analyze the document directly.

### Step 1 — Read and map the document

Read the full document provided. Map its contents against the five voice dimensions:

| Dimension | Covered by | Status |
|---|---|---|
| How they actually talk | VCF Input 1 / raw transcripts / unscripted material | COVERED / PARTIAL / MISSING |
| Who they are outside work | VCF Input 2 / personal segments | COVERED / PARTIAL / MISSING |
| Communication values (what they value) | VCF Input 3A | COVERED / PARTIAL / MISSING |
| Communication values (what they hate) | VCF Input 3B | COVERED / PARTIAL / MISSING |
| Emotional calibration | VCF Input 4 | COVERED / PARTIAL / MISSING |

### Step 2 — Check for critical gaps

Critical sections (without these, the Voice Profile will be too thin to be useful):
- How they actually talk (VCF Input 1 or equivalent unscripted material)
- Communication values — what they hate (VCF Input 3B)

If either critical section is missing or contains only 1–2 sentences with no real substance:
Ask for that specific section only. Do not run the full interview. Use this message:

```
I can see [section X] is missing or too thin to analyze reliably.
Without it, the voice profile won't capture [specific dimension].

Can you add this? [paste the relevant VCF Input section here]

Everything else looks good — I'll proceed as soon as you send this.
```

If non-critical sections are missing: note the gap in the final output, proceed without blocking.

### Step 3 — Proceed to Phase 4 (Voice Analysis)

Run Phase 4 analysis directly on the document content.
Note in the output: "Voice Profile built from provided document. No online sources were analyzed."

If the document contains only written material (no spoken/unscripted content), flag it:
```
⚠️ This profile is based on written material only.
Written content may be more polished or edited than how this person actually speaks.
For stronger voice fidelity, supplement with a voice memo or podcast appearance when available.
```

Then proceed to Phase 5 (Delivery).

---

## SCENARIO A — ONLINE RESEARCH PATH
*(becomes Scenario C if online material is sufficient and no VCF is needed)*

### PHASE 1 — DISCOVERY

Search for publicly available sources featuring the user's authentic voice.

First, ask: "What's your full name and company? Any LinkedIn, YouTube, or podcast links I should start with?"

Then run these searches:

```
"[NAME]" interview podcast 2023 OR 2024 OR 2025
"[NAME]" "[COMPANY]" YouTube talk keynote
"[NAME]" site:youtube.com
"[NAME]" Forbes OR TechCrunch OR Inc interview
"[NAME]" LinkedIn article written
"[NAME]" podcast transcript
```

For each result, note:
- Title, URL, format (YouTube / Podcast / Article / LinkedIn / Talk)
- Approximate date
- Length: Short (<5 min) / Medium / Long (>20 min)
- Authenticity signal: Scripted / Semi-scripted / Unscripted/Conversational

Present findings grouped into two categories:

```
✅ SPOKEN / HIGH-TRUST (unscripted video, podcast, conversational interview)
[list]

⚠️ WRITTEN / NEEDS CONFIRMATION (articles, blog posts, LinkedIn posts — may be AI-assisted)
[list]

Total found: [X]
Gaps noted: [e.g., "No long-form unscripted audio found" / "Most content is from 2022, nothing recent"]
```

Ask the user: "Which of these should I analyze? For spoken sources, I'll process all unless you say otherwise. For written content, tell me which ones you personally wrote without heavy editing."

Wait for confirmation before proceeding.

---

### PHASE 2 — CONTENT EXTRACTION

Extract text from each approved source.

**For YouTube videos:** Use the youtube_transcript_api if available. If not, note the video URL and ask the user: "I can't auto-extract this one — can you paste the transcript or a summary of what you said?"

**For articles and written content:** Fetch the full text. Strip navigation and ads. Keep only the main body.

**For multi-speaker sources (podcasts, interviews):** Extract only the user's lines. Discard host and interviewer lines entirely.

Tag each piece of content:

```
SOURCE: [Title]
FORMAT: [YouTube / Podcast / Article / LinkedIn]
DATE: [Year]
AUTHENTICITY: [Scripted / Unscripted]
---
[Extracted text]
---
```

Report: "Extraction complete. [X] sources processed. ~[X] total words. Ready for analysis."

---

### PHASE 3 — DATA SUFFICIENCY CHECK

Before analyzing, assess whether the collected material covers the five dimensions of voice:

| Dimension | Covered by | Status |
|---|---|---|
| How they actually talk | Long-form unscripted spoken sources | COVERED / PARTIAL / MISSING |
| Who they are outside work | Personal segments in interviews | COVERED / PARTIAL / MISSING |
| Communication values (what they value) | Patterns across written + spoken | COVERED / PARTIAL / MISSING |
| Communication values (what they hate) | Rarely stated online | ALMOST ALWAYS MISSING |
| Emotional calibration | Tone patterns across sources | COVERED / PARTIAL / MISSING |

**Verdict:**
- **SUFFICIENT** → Proceed to Phase 4. No VCF needed. *(This is Scenario C — online material alone was enough.)*
- **PARTIAL** → Proceed to Phase 4 with what exists. Send the user only the missing VCF sections.
- **INSUFFICIENT** → Send the full VCF before analyzing.

If sending VCF sections, list exactly which ones and why. Do not send the whole form if only part is needed.

---

### PHASE 4 — VOICE ANALYSIS

Analyze all collected material and produce the Voice Profile.

#### PART 1: VOICE DNA

**Tone Profile**
- Primary tone (describe the emotional texture — e.g., "dry wit wrapped in genuine curiosity")
- Secondary tones that appear situationally
- What is notably absent from their tone
- Support each claim with 2–3 verbatim quotes from the material

**Emotional Baseline**
- Default register (how do they sound most of the time?)
- When do they get most animated? Quote a moment.
- When do they pull back? Quote a moment.
- What emotions do they avoid expressing?

**Natural Rhythm and Sentence Patterns**
- Do they build to a point, or state it upfront then explain?
- Analogy type: technical / personal / nature / pop culture / other
- Sentence length: short and punchy / long and winding / mixed
- Any verbal tics (in spoken material) or written tics (in articles)
- Do they qualify statements or make them flat?

**Vocabulary Signature**
- Words and phrases they use repeatedly (their language, not industry defaults)
- Words they conspicuously avoid
- How they describe their own work in their own terms
- Technical depth: how deep do they go? Do they explain jargon?
- Humor: frequency and style

---

#### PART 2: WRITING RULES

**DO'S (6–10 specific rules, each with a why and example from their material)**

**DON'TS (6–10 specific rules, each with what it would sound like if broken)**

**Sentence Structure Guide**
- Ideal sentence length
- When to use short sentences
- How to open a post (what signals "this is them")
- How to close (their natural landing style)

**POV Rules**
- When they use "I" vs "we" vs "you"
- How personal they get
- How directly they address the reader

**Storytelling Patterns**
- Story then insight, or insight then story?
- How specific are their examples?
- Where does vulnerability appear — and how do they handle it?

**Words to Use / Words to Avoid**

| Use This | Not This |
|----------|----------|
| [their word] | [the AI/corporate version] |

(List 10–15 rows)

---

#### PART 3: AI PROMPT FRAMEWORK

A custom instructions block ready to paste into any AI session:

```
You are writing as [NAME].

VOICE CORE
[3–4 sentences describing their voice in a way AI can operationalize]

ALWAYS:
[6–8 specific, actionable rules]

NEVER:
[6–8 specific, actionable rules]

SENTENCE STYLE:
[2–3 rules on length, structure, rhythm]

NATURAL VOCABULARY:
[List of their words/phrases to pull from]

BANNED VOCABULARY:
[List of words that immediately break their voice]

LITMUS TEST:
Before outputting: Does this sound like [NAME] speaking out loud?
If it sounds like a B2B content template, rewrite it.
```

**"Sounds Like Them" vs "Doesn't Sound Like Them" examples (3 pairs):**
Pull from their actual material. For each pair:
- Original (from their material — sounds like them)
- AI version of the same idea (sounds generic)
- What makes the original theirs

**Quality checklist for reviewing AI drafts (8–10 checks specific to this person):**
Pull from the voice analysis — not generic checks.

---

### PHASE 5 — DELIVERY

Present the full Voice Profile to the user.

End with:

```
Voice Profile complete.

Scenario: [A — online research + VCF / B — document input / C — online research only]
Top 3 voice signatures:
1. [One-liner]
2. [One-liner]
3. [One-liner]

Data sufficiency: [SUFFICIENT / PARTIAL — sections sent via VCF / DOCUMENT-BASED — see flag if written-only]

How to use this:
- Paste the AI Prompt Framework (Part 3) at the start of any content session
- Reference the Writing Rules when reviewing any AI-generated draft
- Update this profile every 4 weeks or after 5+ new recordings/transcripts

Save this profile somewhere you can easily copy from.
```

---

## VOICE CAPTURE FRAMEWORK (VCF)

Send only the sections flagged as MISSING or PARTIAL in Phase 3 (Scenario A/C) or Step 2 (Scenario B). Include the intro below.

**Intro to send the user:**

```
To complete your Voice Profile, I need your input on a few things I couldn't find from your material.
This takes [X] minutes (based on which sections below apply to you).
Answer as naturally as possible — voice memos are better than typing if you prefer.
```

---

### VCF Input 1: How You Actually Talk (~8 min)

**Option A — Voice Memo (recommended)**
Record 7–8 minutes answering these naturally, like leaving a voice note for a trusted colleague:
- What do you actually do day-to-day? Explain it like I'm a smart friend who doesn't work in your industry.
- Something that's on your mind recently — a client situation, a pattern you're noticing, something frustrating, something you're figuring out.
- When someone asks what you do at a dinner party, what do you actually say?

Filler words and incomplete sentences are fine. Do not perform.

**Option B — Written (if you dislike recording)**
10 minutes. No editing. Answer exactly how you'd say it:
- Tell me about something interesting that happened with a client or in your work recently. What was the situation? What did you say? What stood out?
- What frustrates you about your industry? Something you wish more people understood.
- When someone asks what you do, how do you explain it? The real version, not the LinkedIn version.

---

### VCF Input 2: You Outside of Work (~7 min)

Answer 3 of these 5 questions — pick whichever feel easiest:
1. Describe your morning or commute today. What did you do, notice, think about?
2. Tell me about something mildly annoying that happened this week. How did you react?
3. What made you laugh recently?
4. What are you personally interested in outside work? How do you describe it to people who share that interest?
5. Tell me about a small moment that felt good recently — something that made you feel calm, grateful, or just good.

---

### VCF Input 3: Communication Values (~6 min)

**Part A: What matters to you when you communicate? Pick your top 3 and explain each with a real example.**
- Clear over clever
- Direct over diplomatic
- Real over polished
- Simple over impressive
- Challenging over comfortable
- Story-driven over data-driven
- Data-driven over story-driven
- Warm over sharp
- Sharp over warm
- Humor as connection

**Part B: What do you never want to sound like? Pick 2–3 and explain why each bothers you.**
Options: corporate fluff / bro-marketing / academic / overly humble / fake inspirational / sales-y / guru energy / therapy-speak / safe and boring

---

### VCF Input 4: Emotional Calibration (~4 min)

**Q1: When people read something you wrote, what do you want them to feel? Pick 2–3.**
Smarter / Reassured / Challenged / Energized / Understood / Clearer / Validated / Provoked (in a good way)

**Q2:** A piece of writing or content that moved you recently. What was it? What specifically made it land?

**Q3:** A communicator whose style you genuinely respect. Who are they? What do you admire about how they express themselves?

---

### VCF Input 5: Raw Materials (~5 min)

Send 2–4 of these — whatever is easiest to grab:
- [ ] A voice memo explaining something (work-related or not)
- [ ] Slack or chat messages where you explained something clearly
- [ ] An email you wrote that felt natural
- [ ] A LinkedIn post or comment that actually sounds like you
- [ ] Notes or voice memos you made for yourself

We're looking for unfiltered material — not polished, just real.

---

Voice profile complete — scenario used: [A/B/C]
Quality check: flag any section that felt thin or uncertain with specific missing input.
If thin: state exactly what is missing and what it affects.

Next step → trigger: run icp-analyzer for [client]

---

## Verification

**Correctness:**
```
□ AI Prompt Framework (Part 3) contains a NEVER list specific to this person — not generic AI banned words? Yes → pass. No → add the person-specific banned vocabulary.
□ "Sounds Like Them" vs "Doesn't Sound Like Them" examples pulled from actual material — not invented? Yes → pass. No → pull from real quotes.
□ Voice DNA section includes verbatim quotes from the material as evidence for each claim? Yes → pass. No → add the quotes.
```

**Completeness:**
```
□ All three parts delivered: Voice DNA, Writing Rules, AI Prompt Framework? Yes → pass. No → add the missing part.
□ Words to Use / Words to Avoid table has at least 10 rows? Yes → pass. No → extend it.
□ Data sufficiency verdict stated (SUFFICIENT / PARTIAL / DOCUMENT-BASED)? Yes → pass. No → state it.
```

**Context-fit:**
```
□ Voice rules are specific to this person — not generic "write short sentences" advice? Yes → pass. No → rewrite using examples from their actual material.
□ The AI Prompt Framework passes the litmus test: would a content session using only Part 3 produce output that sounds like this person? Yes → pass. No → add more specificity to the ALWAYS/NEVER lists.
□ Scenario documented (A/B/C) and any data limitations flagged? Yes → pass. No → add the flag.
```

**Consequence:**
If a content session uses this Voice Profile and the output "doesn't sound like me" — what is the most likely missing rule?
→ Name the gap before delivering. If the answer is "no banned vocabulary list" or "no rhythm examples from their actual speech" — fix it.
