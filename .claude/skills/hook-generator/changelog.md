# Changelog — hook-generator

## v2.2 — 2026-04-20

**Summary of major changes:**
Added `references/EXAMPLES.md` — 131 annotated hook examples from 21 working LinkedIn creators, categorized by the four format shapes (44 Dense, 76 Punchy+Context, 3 Single-Line Bomb, 8 Stacked). Each example includes mobile width scores and cites which rule(s) it demonstrates. SKILL.md Phase 5 now instructs Claude to load this file before generating hooks, so format pattern-matching works from real calibration data instead of principles alone. Sourced from the Distinctiva LinkedIn Hook Writer package.

**Known edge cases:**
- EXAMPLES.md cites rules by number (Rule 2, Rule 8, etc.) from the Distinctiva skill's original numbering. Our SKILL.md has different rule numbering, so treat the rule citations in examples as directional, not lookup references.
- Character limits in SKILL.md always override any length seen in EXAMPLES.md.

## v2.1 — 2026-04-20

**Summary of major changes:**
Added Mobile Truncation Model (pixel width, not character count; per-media hook budget). New Phase 5 Format Shape with 4 structural formats (Dense, Punchy+Context, Single-Line Bomb, Stacked) and sub-variants, each with hard character rules. Added 6 craft rules to Customization (compression, load-bearing period, parenthetical self-correction, result credential + constraint, soft warnings, mirror hook). Added register rule (lowercase only when voice profile specifies). New Banned Phrases section (AI clichés, LinkedIn clichés, manufactured drama, banned rhetorical structures, banned storytelling patterns, em dashes). New mandatory Pre-output Validation gate (character count, line breaks, banned phrases, em dashes, swap test). Merged from Distinctiva LinkedIn Hook Writer skill, Apr 2026.

**Known edge cases:**
- Stacked Data-Question Opener is the only format where `?` is allowed — requires a chart/graph attached.
- Register rule defers to Voice Profile — if a client's voice is lowercase (personal brand/creator), lowercase hooks are allowed; otherwise banned.

**Gotchas:**
- "Rewrite before relabel" is a hard rule — never ship a hook with a format label that doesn't match its actual length.
- Character count thresholds are not suggestions. A Dense hook at 135 chars must be expanded to ≥140 or reclassified; a Punchy line at 55 chars must be compressed to ≤50.

## v2.0 — 2026-03-22

**Summary of major changes:**
Full directive rewrite; category (Teach/Lead/Prove/Connect/Respond) established as primary axis with emotional trigger (Desire/Curiosity/Fear) as sub-logic within each category; 4 formulas per category × 5 categories = 20 total; B2B guidance block retained; Verification section added.

**Known edge cases:**

**Gotchas:**
