# Skill: Fireflies Transcript Extractor

**Purpose:** Extract a specific person's lines (verbatim + timestamps) from a Fireflies call transcript. For transcript collection only — not for idea extraction.

> **Note:** For idea extraction, always use the full original transcript. The client's lines alone don't always give enough context for clear content ideas.

---

## When to Use
When a new Fireflies call recording is available and you need to add a client's (or Mai Lan's) spoken lines to their running transcript doc.

**Trigger phrase:** Any variation of "Extract [Person]'s lines from this transcript", "Pull [Person]'s lines", "Get [Person]'s lines from the call", or similar phrasing that names a person and references extracting their lines from a transcript. Provide the .docx file path and the call/meeting name.

---

## What You Need to Provide
1. Path to the Fireflies .docx file (e.g., `/Users/Mailan/Downloads/call-2026-03-06.docx`)
2. The name of the person to extract (must match their name label in the transcript)
3. The name of the call/meeting (this becomes the tab name in their transcript doc)

---

## Steps Claude Will Follow

### Step 1: Convert DOCX to text
Use macOS `textutil` to extract readable text from the .docx file:
```bash
textutil -convert txt "/path/to/transcript.docx" -output "/tmp/transcript_extracted.txt"
```
Then read the resulting .txt file.

### Step 2: Extract the target person's lines
- Scan for all lines where the speaker label matches the target person's name
- Extract each line verbatim with its timestamp
- Do not clean up, paraphrase, or remove filler words — verbatim only

### Step 3: Output in this format

```
# [Call/Meeting Name]
**Date:** [date from transcript if available]
**Speaker extracted:** [Person's Name]

---

[HH:MM:SS] [Their line verbatim]

[HH:MM:SS] [Their line verbatim]

[HH:MM:SS] [Their line verbatim]

---
```

### Step 4: Tell Mai Lan where to save it
Remind her:
- Open **[Person's Name]'s transcript** doc in their Claude Project
- Add a **new tab** named: `[Call/Meeting Name]`
- Paste the extracted content into that tab

---

## Output Rules
- Verbatim only — no edits, no cleanup, no summarizing
- Timestamps preserved exactly as they appear in the source
- If the speaker name doesn't match exactly (e.g., first name only vs. full name), flag it and ask Mai Lan to confirm before extracting
- If multiple people have similar names in the transcript, flag the ambiguity

---

## What This Skill Does NOT Do
- Does not extract ideas — that uses the full original transcript
- Does not edit or improve the transcript
- Does not save directly to Google Docs (Mai Lan pastes it manually into the tab)
