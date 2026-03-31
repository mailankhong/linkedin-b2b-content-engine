# Visual Brief Generator

Most LinkedIn visuals are pretty but pointless — they look designed but don't communicate a single clear idea at scroll speed. The visual brief isn't about aesthetics. It's about information architecture: what's the one thing someone should understand in 2 seconds of looking at this image? If you can't answer that, the visual needs restructuring before it needs design.

**Pre-flight:** Run preflight.md. Client: [name provided by user].
**Required:** Voice Profile (for brand tone in copy elements), content pillars (for visual positioning).

## Quality Gate

Insufficient input → stop. State gap, impact, fix option. Never invent or fabricate.

---

**Purpose:** Generate a structured visual brief for a finalized LinkedIn post. The brief specifies format, layout, text content, visual direction, and brand application — ready to take into any visual creation tool.

**Trigger phrases:**
- "Generate a visual brief for this post"
- "Visual brief"
- "Make a visual for this"
- "Create a brief for the designer"
- Any variation referencing visual production after a post is finalized

---

## What You Need

- The finalized post copy (approved, after any revisions)
- The user's brand reference (colors, typography, visual tone). If they don't have one yet, prompt them to define it — see the Brand Reference section below.

---

## STEP 0 — BRAND REFERENCE CHECK

Before generating any brief, confirm the user has a brand reference.

If they don't, ask:

```
Before I write the brief, I need your brand reference. This takes 2 minutes to set up once and you'll use it every time.

Please tell me:
1. Primary color (hex code or describe it — e.g., "deep navy blue")
2. Secondary/accent color (hex code or describe)
3. Any additional brand colors
4. Font preference (if you have one — or just say "default clean sans-serif")
5. Visual tone: clean and minimal / bold and high-contrast / technical / warm and human

Once you give me this, I'll save it as your brand reference block for all future briefs.
```

Once provided, save the brand reference as a block the user can reuse:

```
BRAND REFERENCE — [USER NAME / COMPANY]
Primary: [hex]
Accent: [hex]
Additional: [hex if any]
Font: [preference]
Visual tone: [descriptor]
Logo placement: bottom right
```

---

## STEP 1 — FORMAT RECOMMENDATION

Analyze the post content and recommend the best visual format:

| Format | Best for |
|---|---|
| **Carousel** (3–8 slides) | Step-by-step processes, numbered frameworks, before/after, multi-point breakdowns |
| **Single infographic** | One concept, comparison, single framework, data visualization |
| **One-pager / reference card** | Cheat sheets, checklists, reference frameworks meant to be saved |
| **Quote card** | A single strong line from the post that deserves visual emphasis |

State your recommendation and the reason in one sentence.

**Dimensions (always):**
- Portrait / vertical format — LinkedIn is mobile-first
- Single image, carousel, infographic: 1080 × 1350 px (4:5)
- If designing for square: 1080 × 1080 px

---

## STEP 2 — WRITE THE BRIEF

### SECTION 1: VISUAL CONCEPT

- Visual idea: [Describe the main concept in 1–2 sentences — what is this showing?]
- Format: [Carousel / Single infographic / One-pager / Quote card]
- Number of slides or sections: [X]
- Content type: [Educational / Thought leadership / Social proof / Framework / Story]
- Strategic goal: [What should this visual achieve? e.g., "make this framework easy to scan and save"]
- Content pillar: [Which of the user's pillars this maps to — if known]
- Target audience: [Tier 1 / Tier 2 / Tier 3 — and why this format resonates with them]

---

### SECTION 2: TEXT ELEMENTS

For each slide or section, specify:

```
SLIDE [N] / SECTION [N]:
Headline: [Max 8–10 words. Bold. This is what the eye sees first.]
Subtext: [One line of supporting context — optional]
Body: [Main content — concise bullets or 1–2 short sentences]
Visual note: [What to show visually on this slide — icon, diagram, data, photo, etc.]
```

Repeat for each slide. For a single infographic, break into logical visual zones instead of slides.

---

### SECTION 3: STRATEGIC NOTES

- Key message: The ONE thing this visual must communicate
- Proof point to feature: [The most specific number or result from the post — use it prominently]
- Why someone would save this: [What makes it reference-worthy?]
- What to avoid: [Any visual clichés or patterns that would feel off for this person's brand]

---

### SECTION 4: BRANDING

- Primary color: [from brand reference] — use for headlines and key accents
- Accent color: [from brand reference] — use for supporting elements
- Background: [white / light neutral / dark — based on visual tone]
- Font: [from brand reference]
- Logo: bottom right, small
- Tone: [paste the visual tone from their brand reference]

---

### SECTION 5: VISUAL HIERARCHY

- What the eye sees first: [headline / key stat / visual element]
- What the eye sees second: [supporting text / framework label]
- What the eye sees third: [body content / CTA]
- Reading flow: [left to right / top to bottom / circular — describe the intended path]

---

## STEP 3 — DELIVERY NOTE

After the brief, add:

```
Take this brief into your visual tool:
— For carousels and frameworks: AI presentation tools (Gamma, Canva AI, or similar)
— For infographics: AI image generation with a structured prompt (Google AI Studio, similar)
— For a designer: share this brief directly — it has everything they need

Paste your brand reference block at the start of every generation prompt.
```

---

## STEP 4 — OPTIONAL: GENERATE WITH GOOGLE AI STUDIO

**Requires:** `GOOGLE_API_KEY` environment variable.

If set, offer to generate the visual in-session using Gemini with structured JSON prompting.

### 4A — Build the structured image prompt

Do NOT send a plain-text prompt. Build a structured JSON prompt from the brief sections. Pull brand values directly from the client's `brand-guidelines.md` (or `visual-style.md`).

```json
{
  "subject": "[Visual concept from Section 1 — what this image shows]",
  "text_content": {
    "headline": "[Main headline from Section 2, slide 1 or primary zone]",
    "subtext": "[Supporting text — keep short]",
    "body_points": ["[bullet 1]", "[bullet 2]", "[bullet 3]"]
  },
  "style": "[From client brand guidelines — e.g., 'Dark corporate infographic, near-black background, electric blue accents']",
  "colors": {
    "background": "[hex from brand-guidelines.md]",
    "primary_accent": "[hex]",
    "text_color": "[hex]",
    "secondary_accents": ["[hex]", "[hex]"]
  },
  "typography": "[From brand-guidelines.md — e.g., 'Bold geometric sans-serif headlines, clean sans-serif body']",
  "composition": "[From Section 5 Visual Hierarchy — reading flow, what eye sees first/second/third]",
  "format": "portrait, 1080x1350px",
  "negative_prompt": "[From Section 3 'What to avoid' + generic: 'blurry, low resolution, clip art, stock photo aesthetic, warped text, misspelled words']",
  "visual_elements": "[From Section 2 visual notes — icons, diagrams, data viz, photos, etc.]"
}
```

### 4B — Generate the image

Convert the JSON into a single detailed prompt string and send to Gemini:

```
POST https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash-exp:generateContent?key={GOOGLE_API_KEY}
{
  "contents": [{
    "parts": [{
      "text": "Generate a LinkedIn visual image.\n\nSUBJECT: [subject]\nFORMAT: [format]\nSTYLE: [style]\nCOLORS: Background [background], primary accent [primary_accent], text [text_color], secondary accents [secondary_accents]\nTYPOGRAPHY: [typography]\nCOMPOSITION: [composition]\nTEXT CONTENT:\n- Headline: [headline]\n- Subtext: [subtext]\n- Body: [body_points joined]\nVISUAL ELEMENTS: [visual_elements]\nAVOID: [negative_prompt]\n\nThe image must render all text clearly and legibly. Portrait orientation, 1080x1350px."
    }]
  }],
  "generationConfig": {"responseModalities": ["IMAGE", "TEXT"]}
}
```

### 4C — Evaluate and iterate

After generation, check:
- Is all text legible and correctly spelled?
- Does the color palette match the client's brand guidelines?
- Is the visual hierarchy correct (eye path matches Section 5)?
- Does it avoid everything listed in the negative prompt?

If any check fails, adjust the prompt and regenerate (max 2 attempts). Note what was adjusted so the feedback loop improves future generations.

If `GOOGLE_API_KEY` is not set: present the brief only. Note where to take it.

---

Visual brief complete.
Ready-to-build spec: framework type, headline, layout, color assignment.

Generate visual now?
A) Yes, use Google AI Studio → trigger: generate visual for [client] from this brief
B) No, take brief to Canva / Gamma / designer

After visual is complete:
→ trigger: run hook-generator for [next idea]
→ or: end session for [client]

---

## Verification

**Correctness:**
```
□ Format recommendation (Carousel / Infographic / One-pager / Quote card) justified by the post content type? Yes → pass. No → reconsider the format recommendation.
□ Each slide/section has a headline under 10 words — not a sentence? Yes → pass. No → shorten.
□ The single most specific number or result from the post is prominently featured in the brief? Yes → pass. No → add it to Section 3 (Strategic Notes) and a key slide.
```

**Completeness:**
```
□ All 5 sections present: Visual Concept, Text Elements, Strategic Notes, Branding, Visual Hierarchy? Yes → pass. No → complete the missing section.
□ Brand reference block confirmed or built before brief was written? Yes → pass. No → stop and build it.
□ Dimensions stated (portrait 1080×1350px or square 1080×1080px)? Yes → pass. No → add.
```

**Context-fit:**
```
□ Brief maps to the correct content pillar from the client's strategy? Yes → pass. No → update Section 1.
□ Target audience (ICP tier) stated and the visual format matches how that tier consumes content? Yes → pass. No → adjust.
□ "What to avoid" field in Section 3 is specific to this client's brand — not generic design advice? Yes → pass. No → make it client-specific.
```

**Consequence:**
If a designer builds this visual from the brief and it fails to stop the scroll: what is the most likely structural reason?
→ Answer before delivering. If the answer is "entry point is unclear" or "key stat isn't prominent enough" — fix the brief first.
