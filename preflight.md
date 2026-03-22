# Pre-Flight: Client Context Extraction

Every skill that requires client context runs this pre-flight before doing any work.
The trigger is always a client name. Everything else is derived from the client folder.

---

## How Skills Invoke This

At the start of any skill that requires client context, the skill states:

```
PRE-FLIGHT REQUIRED
Client: [name provided by user]
Run preflight.md — Steps 1 through 5 — before proceeding.
```

The skill does not begin its own work until all required fields for its methods are confirmed present.

---

## Step 1 — Locate the Client Folder

Search every accessible location in this order:

1. Current working directory and all subdirectories → fuzzy match on client name
2. Parent directory and sibling directories (one level up, one level sideways)
3. Connected document integrations — if Google Docs MCP is active, search by client name; if Notion MCP is active, search by client name; if other document MCPs are connected, search there too
4. If nothing found across steps 1–3 → stop. Ask exactly this:

```
I can't locate a folder or document for [client name] in any accessible location.
Where is [client]'s context stored?

A) Local folder — paste the path
B) Google Doc or Notion page — share the link
C) Not set up yet — run Foundation Phase first:
   voice-analyzer → icp-analyzer → content-pillars
```

Wait for the user's response. Do not proceed until the folder is located and confirmed.

If multiple matches are found across locations: list them and ask "Which of these is [client]'s folder?"

---

## Step 2 — Confirm the Right Client

Read the Header block first. Confirm the `Name:` field matches the client name provided.

The Header block is the machine-readable anchor at the top of every client folder:

```
Name:           [Full name as used on LinkedIn]
LinkedIn URL:   [https://www.linkedin.com/in/handle]
Niche:          [One line — what they do and who for]
Onboarded:      [Date]
```

If the Name field does not clearly match the client name provided: flag it. Do not proceed on a partial match without user confirmation.

```
I found a folder but the name inside reads "[folder name]" — is this [client]'s folder?
```

---

## Step 3 — Validate Required Fields

Only validate what the current method needs. Do not run a full schema check on every session.

### Hard stops — block the specific method, not the whole session

| Condition | Method blocked | Message to show |
|-----------|---------------|-----------------|
| Content Pillars missing | Methods 3, 5, 5.1, and any method using niche keywords | See below |
| LinkedIn URL missing | Method 4 only | See below |

**MISSING: Content Pillars**
```
I found [client]'s folder but there are no content pillars defined yet.
Run content-pillars to generate them, or paste them here directly.
I cannot run [method name] without this — it is the source of all keyword extraction.
```

**MISSING: LinkedIn URL**
```
I found [client]'s folder but there is no LinkedIn URL in the Header block.
What is [client]'s LinkedIn profile URL?
Format: https://www.linkedin.com/in/handle
Method 4 (Repurposing Archive) cannot run without it.
```

---

### Soft warnings — proceed with a note, no blocking

| Condition | Message to show, then proceed |
|-----------|-------------------------------|
| ICP Language Map missing | See below |
| Voice Profile missing | See below |
| Past Posts section missing (Method 4) | See below |

**MISSING: ICP Language Map**
```
ICP Language Map not found in [client]'s folder.
I'll run with pillar keywords only — results will be less targeted toward how the ICP searches.
Proceed with reduced keyword quality, or paste the Language Map section here to improve it?
```
Proceed either way. Do not wait indefinitely.

**MISSING: Voice Profile**
```
No Voice Profile found for [client] yet.
I can generate post ideas and run all research methods now.
I won't be able to write copy or hooks in [client]'s voice until it exists.
Run voice-analyzer when you're ready. Proceeding with idea generation.
```

**MISSING: Past Posts (Method 4)**
```
No past posts found in [client]'s folder.
I'll pull post history via Apify. If the Apify call is incomplete,
pasting posts directly into the folder is the reliable fallback.
```

---

## Step 4 — Extract Required Fields

Run silently. Do not narrate the reading process to the user.

### Keyword extraction — Methods 3, 5, 5.1

**From Content Pillars:**
- Read each pillar name (2–4 word labels) → these are primary keywords
- Read each pillar "Why" sentence → extract the dominant noun phrase → secondary keyword candidate
- Result: 3–5 primary terms

**From ICP Language Map → Google field (if present):**
- Extract 2–3 phrases the ICP uses when searching for solutions
- These represent pain-language — how the audience talks, not how the expert talks
- Use as secondary keywords only

**Keyword string construction:**
- Take top 3 primary keywords from pillars
- Add up to 2 ICP pain phrases from the Language Map (if available)
- Format: `"[keyword1] OR [keyword2] OR [keyword3] OR [pain phrase]"`
- Hard cap: 5 OR-joined terms. More than 5 dilutes Apify results.
- If fewer than 3 pillars exist: supplement with ICP Demographics (Industry + Role terms) to reach 3

The keyword string is constructed silently and passed directly to the Apify call.
It is not shown to the user unless Apify returns 0 results — then surface it for debugging.

### LinkedIn URL extraction — Method 4

- Read Header block → `LinkedIn URL:` field
- Validate format: must start with `https://www.linkedin.com/in/`
- If malformed or missing the `/in/` path: ask the user to verify before running

### Niche context — all methods

- Read Header block → `Niche:` field
- Used as a relevance filter when Claude evaluates returned results
- Not passed to Apify as a search parameter
- If missing: derive from ICP Demographics (Industry + Role) — this is the one case where derivation is acceptable, because Niche is a summary field, not a structured one

---

## Step 5 — Auto-Update on User-Provided Information

When a user provides missing information in response to a gap question (Steps 1–3 above):

1. Before writing anything, confirm:
   ```
   I'll add [field name] to [client]'s folder — confirm?
   ```
2. Wait for explicit confirmation ("yes", "go ahead", "do it", or equivalent)
3. Write the information to the correct section of the client folder
4. Confirm completion:
   ```
   Added to [client]'s folder. Proceeding.
   ```

**If the folder is in a format that cannot be written to** (e.g. a read-only Google Doc, a linked file without write access): say so and ask the user to add it manually.

```
I can read [client]'s folder but I don't have write access to update it.
Please add this manually:

  [Field]: [Value]

Once added, I'll proceed.
```

Never modify a client folder silently. Always confirm first.

---

## Step 6 — Hard Constraints

These are non-negotiable. They apply to every skill, every method, every session.

```
NEVER invent keywords if Content Pillars are missing → ask
NEVER assume or guess a LinkedIn URL → ask
NEVER fill a missing field with inferred or fabricated data → surface the gap explicitly
NEVER block the whole session because one method's field is missing → block that method only
NEVER proceed on a partial client name match without confirmation → ask
NEVER re-ask for information already present in the folder → trust what's there
NEVER modify a client folder without user confirmation → always confirm first
```

---

## Standard Client Folder Schema

This is the contract. Every client folder must contain these sections.

```
═══════════════════════════════════════════════
CLIENT PROFILE — [CLIENT FULL NAME]
═══════════════════════════════════════════════

Name:           [Full name as used on LinkedIn]
LinkedIn URL:   [https://www.linkedin.com/in/handle]
Niche:          [One line — what they do and who for]
Onboarded:      [Date]

---

## ICP DOCUMENT
[Full output from icp-analyzer]

Required fields for pre-flight extraction:
- DEMOGRAPHICS block → Industry and Role fields
- THEIR BIGGEST PROBLEMS (in their words) → numbered list
- LANGUAGE MAP block → Google field (minimum)

---

## CONTENT PILLARS
[Full output from content-pillars]

Required fields for pre-flight extraction:
- Pillar names (2–4 word labels)
- Pillar "Why" sentences

---

## VOICE PROFILE
[Full output from voice-analyzer]

Required fields for content generation:
- AI Prompt Framework (Part 3)
- Writing Rules (DOs and DON'Ts)

---

## PAST POSTS
[Post archive — plain text, one post per block]
[Optional but strongly recommended — required for Method 4 quality comparison]
```

### Onboarding checklist for every new client

```
□ Header block: Name, LinkedIn URL, Niche, Onboarded date
□ ICP Document          → run icp-analyzer
□ Content Pillars       → run content-pillars
□ Voice Profile         → run voice-analyzer
□ Past Posts            → paste manually, or defer to Method 4 Apify pull
```

The machine enforces this checklist silently — it only surfaces missing items when a method that needs them is triggered. It does not run a full validation on session start unprompted.

---

## Apify-Specific Notes

These apply to the four Apify-dependent methods only.

**Environment variable required:**
`APIFY_API_KEY` must be set before any Apify call. If not set:

```
APIFY_API_KEY environment variable not found.
Set it in your Claude Code environment settings.
Methods 3, 4, 5, and 5.1 cannot run until it is set.
```

**Actor IDs used (verify against Apify marketplace before first run):**
- Methods 3, 5, 5.1: `apify/linkedin-post-search-scraper`
- Method 4: `apify/linkedin-profile-posts-scraper`

**Failure rule:** If any Apify call fails for any reason (auth error, timeout, rate limit, empty results) — flag it explicitly, state which method could not run, and stop that method. Do not substitute with web search. Do not proceed silently.
