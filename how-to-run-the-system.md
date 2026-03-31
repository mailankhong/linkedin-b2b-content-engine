# LinkedIn Content Engine — How to Run the System

---

This is the full system for creating LinkedIn content that builds pipeline over time.
It runs entirely inside Claude Code — no dashboards, no extra apps, no context switching.
Everything lives in one folder. You open it, type `claude`, and the system is ready.

This doc explains what's in it, how to set it up, and how to run it week to week.
Read it once before your first session. You don't need to memorize it — come back to it when you need it.

---

## WHAT'S IN THE SYSTEM

21 skills that cover every stage of content production:

- **Foundation** — extract your voice, define your audience, build your content strategy, rewrite your LinkedIn profile
- **Weekly planning** — 6 parallel research workers pull from Reddit, YouTube, live debates, your post archive, viral post structures, and visual frameworks — merged into a prioritized idea batch
- **Post production** — hook generation → copy development → post grading → revision loop
- **Visual briefs** — structured briefs for carousels, infographics, and comparison graphics
- **Lead magnet pipeline** — write the post and build the actual resource it promises
- **Maintenance** — a learning loop that gets smarter over time

Each skill is self-contained. You enter at any stage. You bring what you have, and the system asks only for what's missing.

---

## BEFORE YOUR FIRST SESSION — SETUP

Most skills need nothing beyond Claude Code. A few need extras depending on how you want to use the system.

---

**1. Claude Code**
Everything runs inside Claude Code. If you don't have it yet, install it from claude.ai/code.

---

**2. youtube-transcript-api (recommended)**
Used by `voice-analyzer` to pull transcripts from YouTube interviews and podcast videos automatically when building your Voice Profile.

```
pip3 install youtube-transcript-api
```

Without it: `voice-analyzer` will ask you to paste YouTube content manually. The skill still works — just more effort.

---

**3. Apify account (required for research miners)**
Used by five research skills: `repurposing-archive`, `reverse-engineering`, `real-time-signal`, `youtube-miner` (Scenario B), and others. These are the skills that pull live data from Reddit, LinkedIn, X/Twitter, and YouTube at scale.

Sign up at apify.com → get your API key → add it to your `.env` file:

```
APIFY_API_KEY=your_key_here
```

Without it: Research miners that rely on Apify will skip or return partial results. `weekly-idea-session` will still run with the methods that don't need it — but you lose your strongest research channels.

---

**4. Optional: Gemini MCP**
Used by `youtube-miner` (Scenario A) to analyze full YouTube videos via Gemini. This is the deeper analysis path — it can work with any video, not just ones with transcripts.

Get your API key at aistudio.google.com → follow the Gemini MCP setup for Claude Code.

Without it: `youtube-miner` falls back to the transcript-only path. Not a blocker.

---

**5. Optional: Google Docs MCP**
Lets Claude save outputs (Voice Profile, ICP, idea batches, post drafts) directly to your Google Drive instead of copy-pasting.

```
claude mcp add google-docs
```

Without it: copy-paste outputs manually. Perfectly fine, just more steps.

---

**6. Optional: Gamma MCP**
Lets Claude generate carousels and framework visuals directly from your visual brief without switching to another tool.

Get your API key at gamma.app → Settings → Integrations → follow Gamma MCP setup.

Without it: take the visual brief into Gamma manually. No real downside.

---

Once setup is done, open your terminal inside the project folder and type:

```
claude
```

All 21 skills load automatically. You're in.

---

## PHASE 1 — FOUNDATION (do this once per client)

Four steps. Do them in order. These build the context that every other skill depends on.

---

### STEP 1: Build Your Voice Profile
**Skill: `voice-analyzer`**

This is the most important thing you'll do. Everything the system writes for you — every post, every hook, every profile section — runs through your Voice Profile. Without it, Claude writes in a generic B2B voice. With it, Claude writes like you.

Three ways to run it:
- **Scenario A (default):** Claude researches your online presence automatically — LinkedIn posts, articles, interviews, podcasts — and builds a draft profile. You fill gaps with a short interview.
- **Scenario B:** You paste in a completed Voice Capture Framework or a batch of raw material (your posts, messages, anything you've written). Claude analyzes it directly.
- **Scenario C:** Research-only audit — useful if you want to see what's findable about you before committing to a full build.

The output has three parts: Voice DNA (your tone, rhythm, emotional baseline), Writing Rules (DOs and DON'Ts with examples from your actual material), and an AI Prompt Framework that gets embedded into every production skill going forward.

Save the output. This is the DNA of the whole system.

---

### STEP 2: Define Your ICP
**Skill: `icp-analyzer`**

Who are you actually writing for? This skill builds a three-tier audience profile:
- Tier 1 — your primary buyer (decision maker, fastest to close)
- Tier 2 — the champion or evaluator (influences the decision)
- Tier 3 — the practitioner or community member (builds audience, rarely converts directly)

Each tier gets a full breakdown: demographics, their exact problems in their own words, what they've tried before, what they actually want, what triggers them to act, what objections they'll raise, and how they talk in different contexts (DMs, sales calls, Reddit, internally).

Every piece of content you publish maps back to one of these tiers. The ICP is what lets you write posts that feel like they were written specifically for one person — because they were.

Save the output.

---

### STEP 3: Define Your Content Pillars + First Idea Bank
**Skill: `content-pillars`**

Based on your ICP and what you know best, this skill defines 3–5 content pillars — the core themes you'll rotate through. Then it generates 10–15 specific post ideas per pillar.

That's 30–75 post ideas before you've written a single word.

Each idea includes: topic, post type (story / framework / how-to / contrarian / social proof / lead magnet), ICP tier targeted, and goal (engagement / authority / conversion / lead gen).

Save the output. This is your standing idea bank. You'll pull from it every week when research comes up thin, or when you want to plan ahead.

---

### STEP 4: Optimize Your LinkedIn Profile
**Skill: `profile-optimizer`**

Run this last. Before you start, the skill loads your Voice Profile, ICP, and Content Pillars from the previous steps. It needs all three to rewrite your profile in your actual voice, speaking directly to your actual audience.

It rewrites:
- Headline
- About section (with hooks, credibility beats, and a clear CTA)
- Experience entries (outcome-focused)
- Featured section (strategic, not random)
- Banner text or concept

Your profile is where people land after they see your content. If the profile doesn't match the content, you lose them. Copy-paste the rewrites directly. Do this before you publish your first post.

---

### OPTIONAL: Formatting Profile
**Skill: `formatting-profile`**

If you have strong opinions about how your posts look on the feed — line break style, sentence length, whether you use bullet points or prose, how you structure CTAs, emoji policy — run this after profile-optimizer.

It captures visual formatting preferences separately from voice. These get stored in your client folder and applied as overrides in every production session.

Trigger: "run formatting-profile for [your name]"

---

## PHASE 2 — WEEKLY IDEA GENERATION

Run once a week, before you write anything. This is how you always have something good to write about.

---

**Skill: `weekly-idea-session`**

Claude asks you two questions before running anything:

**Question 1: How many posts do you want to publish this week?**
Be honest. 2 is fine. 3 puts you in the top 10% of creators. Claude sizes the idea batch to match — you don't get more ideas than you can actually use.

**Question 2: Should I run research for new ideas?**
Say yes, and Claude runs all 6 research methods simultaneously:

- **Reddit** — finds what your audience is frustrated about right now, in their own words, in the forums they actually use
- **YouTube** — breaks down high-performing long-form content in your niche into structured post angles
- **Real-time signals** — finds what the industry is debating this week on X/Twitter and LinkedIn; gives you timely reactive posts
- **Repurposing archive** — pulls your existing posts (if any), identifies your strongest themes, and finds what to go deeper on or revisit
- **Reverse engineering** — finds viral text and visual posts in your niche and extracts the structural templates you can adapt
- **Viral visual miner** — searches for visual posts that are working in your space and maps the formats to your expertise

You don't do anything during this. Claude runs all 6 in parallel, applies a quality filter, checks each idea against your ICP tiers, and comes back with a sized, prioritized batch.

After research, Claude asks one more thing:

> "Do you have any personal material this week? A client conversation, a pattern you noticed, a win, something that frustrated you, an insight from your work — anything real."

Add it if you have it. Personal material is what separates your posts from everyone else's. It gets priority placement in the batch.

---

**At the end of the session, Claude asks:**
> "Would you like one of this week's posts to be a lead magnet — where you give away a free resource (a template, playbook, checklist) in exchange for comments and connections?"

If yes, flag one idea for that path. If no, all ideas go through the standard copy development route.

Don't say yes unless you're ready to build the resource. The post promises something, and you need to deliver it.

---

## PHASE 3 — COPY DEVELOPMENT

Run once per idea you want to turn into a post.

---

### STANDARD PATH (most posts)

**Skill: `copy-developer`**

Pass in the approved idea. Claude routes through four steps automatically:

**Step 1 — Load context**
Claude loads your Voice Profile, ICP, Content Pillars, and formatting preferences before writing a single word. This is why the Foundation phase matters. Without it, every post starts from scratch. With it, context is already loaded.

**Step 2 — Hook first**
Claude runs `hook-generator` before writing the body. You get 20+ hook variations organized by emotional trigger — Desire (makes them want what you're offering), Curiosity (makes them need to know what comes next), Fear (makes them want to avoid a mistake). Claude recommends the top 3–5 with reasoning.

Pick one. Don't rush this. The hook is the most important line you'll write — it determines whether anyone reads the rest.

**Step 3 — Write the post**
With the hook selected, Claude identifies the right post category (Teach / Lead / Prove / Connect / Respond) and selects the matching template from a library of 20+ proven LinkedIn structures. Then it writes the full post in your voice.

Review the draft. If something doesn't sound like you, say so specifically. Tell Claude what feels off. This is a first draft — your judgment is the final filter.

**Step 4 — Grade the post**
Claude runs `post-grader` automatically. The post is scored across 5 dimensions (pass threshold: 38/50):

1. Hook Strength — would someone actually stop scrolling?
2. Clarity and Specificity — is there a real, concrete detail or is it advice floating in space?
3. Audience Targeting — does it speak to one person or no one?
4. Voice Match — does it sound like you or like a B2B content template?
5. CTA — does it earn the ask, or just add one?

Below 38 → Claude identifies the specific failing dimensions, proposes fixes, and returns to copy development. Max 2 loops before it surfaces the score and flags the tradeoffs to you.

---

### LEAD MAGNET PATH

Run when you said yes in Phase 2.

**Skill: `lead-magnet-writer`** → writes the post promoting the free resource. Uses 10 proven templates from high-performing lead magnet posts. The post promises something specific in exchange for a comment.

**Skill: `lead-magnet-builder`** → builds the actual resource you're giving away. Six format options: playbook, guide, template, checklist, swipe file, or system blueprint. Fully structured with every section mapped out.

Then run `post-grader` on the lead magnet post before publishing.

---

## PHASE 4 — VISUAL

Run after copy is finalized. Not every post needs a visual — but consistent visual content on 80%+ of posts is the benchmark for high-performing B2B LinkedIn.

---

### STEP 1: Generate a Visual Brief
**Skill: `visual-brief`**

Pass in the finalized post copy. Claude outputs a structured brief:
- Format (carousel / single-frame infographic / comparison / quote graphic / framework visual)
- Number of slides or sections
- Exact headline and supporting text per section
- Visual tone and direction
- How to apply your brand (colors, fonts, layout principles)

This is what you take into your visual tool.

Before running this for the first time, you need a brand reference — your color palette (with hex codes), typography, visual tone, and any logo elements. Without it, your visuals will look different every week and you lose the recognition that builds over time.

If you don't have a brand reference yet: paste your existing brand assets into Claude and say "Build me a brand guideline reference for LinkedIn visuals." Save the output. Paste it into every visual session going forward (or ask Claude to remember it).

---

### STEP 2: Generate the Visual

**Option A: Gamma (carousels, framework breakdowns, step-by-step)**
Go to gamma.app → paste your visual brief into Gamma's AI generation flow. Strong for structured multi-slide content. Looks polished without any design work. If you've set up the Gamma MCP, Claude generates the visual directly from your session — no window switching.

**Option B: Google AI Studio (infographics, data visuals, single-frame)**
Go to aistudio.google.com → paste your visual brief and specify your brand style. Strong for single-frame visuals, comparisons, and data breakdowns.

---

## MAINTENANCE — KEEP THE SYSTEM SHARP

These three skills run in the background and get smarter over time. You don't have to think about them — but you should know what they do.

---

**`feedback-capture` — runs automatically at the end of every session**
Silently logs what was produced, what was changed and why, what was rejected, and any engagement data you share. Appends a structured entry to your learning log. Never interrupts a production session — end of session only.

You don't need to trigger this. It runs on its own.

---

**`pattern-recognition` — runs every 5 sessions, or on demand**
Reads your full learning log and identifies what's actually happening across sessions: which hook types keep getting rewritten, which post structures are underperforming, where voice drift is creeping in, which ICP tiers you're over- or under-serving.

Outputs two things:
1. Recommended updates to your client folder (Voice Profile refinements, ICP adjustments, pillar shifts) — applied with your confirmation
2. Recommended updates to the skill files themselves — presented for your review only, never applied automatically

Trigger: "run pattern recognition for [your name]"

---

**`content-auditor` — run monthly or quarterly**
Full strategy review. Checks pillar relevance against your current business stage, looks for performance patterns in your learning log, flags if your voice profile needs refreshing, and outputs a prioritized maintenance report with specific recommended updates.

Trigger: "run content audit for [your name]"

---

## WHEN YOU GET STUCK

**"I don't know what to write about"**
→ Pull from your Content Pillars idea bank (Phase 1, Step 3). You have 30–75 ideas already there. Pick one and run copy-developer.

**"I have an idea but can't figure out the angle"**
→ Run hook-generator directly with the raw topic. The 20+ variations will show you which angle is strongest — the one that generates the most interesting hooks is usually the right direction.

**"The post sounds too AI-generated"**
→ Read your Voice Profile. Find which rule is being broken. Tell Claude: "This doesn't sound like me. Here's the rule I think it's violating: [paste it]. Rewrite this section." Be specific. "More casual" doesn't help. "Stop using the word 'leverage'" does.

**"I'm not getting engagement"**
→ Run post-grader on your last 3 posts. Look for patterns. It's almost always Dimension 1 (weak hook) or Dimension 2 (no concrete specifics). Generic advice with no real experience behind it will flatline every time.

**"My ideas are running thin"**
→ Run a standalone research miner. `reddit-miner` is the fastest — paste a subreddit URL and it comes back with post angles in minutes. `viral-visual-miner` is good if you want to try a new format.

**"My posts don't feel like they're building toward anything"**
→ Run pattern-recognition. It will tell you which ICP tiers you've been ignoring, which pillars are underrepresented, and whether your mix is too heavy on one type of content (usually too much How-To, not enough Story or Social Proof).

**"I want to try a visual format I've never done before"**
→ Find a post from a creator you respect, drop it into the session, and say "run viral-visual-miner on this." It will reverse-engineer the format and tell you exactly how to adapt it.

---

## SUMMARY — WEEKLY RHYTHM

```
Start of week       → weekly-idea-session: generate this week's idea batch
Per post            → copy-developer: hook → post → grade → revise if needed
Per post w/ visual  → visual-brief → Gamma or Google AI Studio
When ideas run dry  → pull from Content Pillars idea bank
Every 5 sessions    → pattern-recognition runs automatically
Monthly             → content-auditor: full strategy check
```

That's the whole system.

The research, the structure, the templates, the quality gates — all of that runs automatically. Your job is to bring the real experience behind the words, make the calls when the system surfaces options, and add what only you can add.

---

