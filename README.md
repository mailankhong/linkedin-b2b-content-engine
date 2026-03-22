# Universal Content Machine

A portable, multi-client system for creating authentic, strategic LinkedIn content that builds pipeline over time. Built to run entirely inside Claude Code — no external dashboards, no separate apps, no context switching.

Every skill is a self-contained workflow. You enter at any stage, bring what you have, and the system asks only for what is missing.

---

## What it does

- **Foundation build** — extract voice, build ICP, define content pillars, optimize LinkedIn profile
- **Weekly idea generation** — 6 parallel research workers (Reddit, YouTube, viral posts, live debates, post archive, reverse engineering) merge into a prioritized idea batch
- **Full post production** — hook generation → copy development → post grading → revision loop
- **Visual asset briefs** — structured briefs for designed posts (carousels, infographics, comparison graphics)
- **Lead magnet pipeline** — write the post and build the actual resource it promises
- **Maintenance loop** — feedback capture → pattern recognition → content audit keeps strategy fresh over time
- **Multi-client** — each client gets an isolated folder; the same skills run across all of them

---

## Setup

```bash
# 1. Clone the repo
git clone https://github.com/YOUR_USERNAME/universal-content-machine.git

# 2. Copy the environment file
cp .env.example .env

# 3. Add your API keys to .env
# Required: APIFY_API_KEY
# Optional: GOOGLE_API_KEY, X_COOKIES

# 4. Open the folder in Claude Code
# All skills load automatically from .claude/skills/
```

---

## Skills included

See the full Skills Reference Map in [CLAUDE.md](CLAUDE.md) for the complete list, organized by:

- **Foundation** — voice-analyzer, icp-analyzer, content-pillars, profile-optimizer, formatting-profile
- **Weekly production** — weekly-idea-session, hook-generator, copy-developer, post-grader, visual-brief, lead-magnet-writer, lead-magnet-builder
- **Research miners** — reddit-miner, youtube-miner, viral-visual-miner, real-time-signal, repurposing-archive, reverse-engineering
- **Maintenance** — feedback-capture, pattern-recognition, content-auditor

21 skills total.

---

## Requirements

- [Claude Code](https://claude.ai/code) — all skills run inside Claude Code as slash commands
- [Apify account](https://apify.com) — required for research miners (repurposing-archive, reverse-engineering, real-time-signal, youtube-miner Scenario B)
- Google API key — optional; enables in-session visual generation via Gemini (visual-brief)
- X cookies — optional; improves X/Twitter signal quality in real-time-signal

---

## Client folder structure

Each client gets a folder under `.claude/clients/[client-name]/` containing their Voice Profile, ICP document, Content Pillars, and learning log. The `preflight.md` at root validates context before any skill runs.
