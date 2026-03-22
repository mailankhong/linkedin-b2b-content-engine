# The LinkedIn Content Engine — Full Guide

The complete system guide is in Google Docs:
https://docs.google.com/document/d/12LpbwHpI8dxp3aIKK0NlSP5_VLrFHyVzN88LFOcBFBE/edit

Read it before your first session. It explains every phase, every skill, and how they connect.

---

## Setup — What You Need to Install

Most skills in this system need nothing beyond Claude Code itself. A few skills need additional setup depending on how you want to use them.

---

### Tier 1 — Required for the full system to work

**1. youtube-transcript-api (Python package)**
Needed by: `voice-analyzer` — to automatically extract transcripts from YouTube interviews and podcast videos when building your Voice Profile.

```
pip3 install youtube-transcript-api
```

Without it: voice-analyzer will ask you to paste transcripts manually for YouTube sources. The skill still works — just more manual effort.

---

**2. Gemini MCP**
Needed by: `youtube-miner` — this skill runs entirely through Gemini's video analysis. It will not work without this MCP.

Setup: Go to [aistudio.google.com](https://aistudio.google.com) → get your API key → follow the Gemini MCP setup instructions for your AI assistant.

Without it: `youtube-miner` won't run. The `weekly-idea-session` will automatically skip Method 2 (YouTube mining) and use the other 3 research methods instead. Not a blocker, but you lose one research channel.

---

### Tier 2 — Optional but recommended

**3. Google Docs MCP**
Needed by: nothing (optional for all skills) — lets Claude save outputs (Voice Profile, ICP, idea batches) directly to your Google Drive instead of copy-pasting.

```
claude mcp add google-docs
```

Without it: copy-paste outputs manually into your own documents. Perfectly fine, just more steps.

---

**4. Gamma MCP**
Needed by: nothing (optional for `visual-brief`) — lets Claude generate carousels and framework visuals directly from a visual brief without switching to another tool.

Setup: Go to gamma.app → Settings → Integrations → get your API key → follow Gamma MCP setup.

Without it: take the visual brief output into Gamma manually. No real downside — just one extra window.

---

### Tier 3 — Fallback only (only matters if Tier 1 setup fails)

**5. yt-dlp + Whisper**
Needed by: `voice-analyzer` — only as a fallback when a YouTube video has no auto-captions and the transcript API fails.

```
pip3 install yt-dlp
pip install openai-whisper
```

Without it: voice-analyzer will skip videos without captions and ask you to paste the content manually. This rarely comes up.

---

### Skills that need nothing

These 12 skills run on pure conversation — no installation required:

| Skill | What it does |
|---|---|
| icp-analyzer | Defines your ICP through conversation |
| content-pillars | Defines pillars and generates idea bank |
| profile-optimizer | Rewrites your LinkedIn profile |
| weekly-idea-session | Weekly idea planning session |
| reddit-miner | Mines Reddit via built-in web fetch |
| viral-visual-miner | Searches for viral visual posts via built-in web search |
| hook-generator | Generates hook variations |
| copy-developer | Writes post drafts |
| post-grader | Scores posts |
| lead-magnet-writer | Writes lead magnet posts |
| lead-magnet-builder | Builds the resource |
| visual-brief | Generates visual briefs |
| visual-analyzer | Analyzes any visual you paste or screenshot |

---

### Quick install summary (copy and run once)

```bash
# Required
pip3 install youtube-transcript-api

# Optional fallback
pip3 install yt-dlp
pip install openai-whisper

# MCP integrations (run in your terminal, one at a time)
# Gemini MCP — see aistudio.google.com for API key
# Google Docs MCP:
claude mcp add google-docs
# Gemini MCP: follow setup at aistudio.google.com
```

---

## Quick Reference — Skill Order

### Phase 1 — Foundation (once)
1. voice-analyzer → Voice Profile
2. icp-analyzer → ICP (3 tiers)
3. content-pillars → Pillars + 30–75 ideas
4. profile-optimizer → LinkedIn profile rewrite (run last)

### Phase 2 — Every Week
- weekly-idea-session → idea batch
- hook-generator → hooks per idea
- copy-developer → full post draft
- post-grader → score before publishing
- visual-brief → brief for visual tool
- lead-magnet-writer + lead-magnet-builder → 1x/month

### On Demand
- reddit-miner, youtube-miner, viral-visual-miner → more ideas
- visual-analyzer → reverse-engineer any visual you admire
