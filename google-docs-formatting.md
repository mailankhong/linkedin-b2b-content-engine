# Google Docs Formatting Procedure

When saving content to Google Docs, **never pass markdown-formatted text to `createDocument` via `initialContent` or to `appendToGoogleDoc`**. These dump raw markdown syntax (`##`, `**`, `*`, `-`) as literal text. Instead, follow this procedure to produce natively formatted Google Docs.

---

## Step-by-step

### 1. Create the document (empty)

```
createDocument(title: "...", parentFolderId: "...")
```

Do NOT pass `initialContent`. Save the returned `documentId`.

### 2. Prepare clean text

Before inserting, strip ALL markdown syntax from the content:

| Markdown | Strip to |
|----------|----------|
| `# Heading` | `Heading` |
| `## Heading` | `Heading` |
| `### Heading` | `Heading` |
| `**bold**` | `bold` |
| `*italic*` | `italic` |
| `- bullet item` | `bullet item` (prefix with `"  •  "` instead) |
| `---` (horizontal rule) | remove entirely, or replace with a blank line |
| `` `code` `` | `code` |
| `> blockquote` | `blockquote` |
| `| table |` | convert to plain text rows or use `insertTable` |

Build one single clean text string with all content, preserving line breaks between paragraphs.

### 3. Insert the clean text

```
insertText(documentId, textToInsert: cleanText, index: 1)
```

Insert all text in one call at index 1.

### 4. Apply paragraph styles (headings)

For each line that was a markdown heading, apply the corresponding native style:

| Markdown was | Apply style |
|-------------|-------------|
| `#` | `TITLE` |
| `##` | `HEADING_2` |
| `###` | `HEADING_3` |
| `####` | `HEADING_4` |

Use `textToFind` targeting to locate each heading line:

```
applyParagraphStyle(documentId, target: {textToFind: "Heading Text"}, style: {namedStyleType: "HEADING_2"})
```

If a heading text appears more than once in the document, use `matchInstance` to target the correct one.

### 5. Apply text styles (bold, italic)

For each span that was wrapped in `**...**` or `*...*`, apply native formatting:

```
applyTextStyle(documentId, target: {textToFind: "bold text"}, style: {bold: true})
applyTextStyle(documentId, target: {textToFind: "italic text"}, style: {italic: true})
```

If the same text appears multiple times, use `matchInstance` to target the right occurrence.

### 6. Apply table formatting

For markdown tables, use `insertTable` to create a real Google Docs table, then populate cells.

**Table cell index formula:**
For a table inserted at index N with C columns:
```
cell(row, col) = N + 4 + 2*col + row*(2*C + 1)
```

**Fill cells in REVERSE order** (last cell first, first cell last) so earlier cell indices don't shift.

Example: 3x2 table at index 50 (C=2):
- cell(2,1) = 50 + 4 + 2 + 2*5 = 66  ← fill first
- cell(2,0) = 50 + 4 + 0 + 10 = 64
- cell(1,1) = 50 + 4 + 2 + 5 = 61
- cell(1,0) = 50 + 4 + 0 + 5 = 59
- cell(0,1) = 50 + 4 + 2 + 0 = 56
- cell(0,0) = 50 + 4 + 0 + 0 = 54  ← fill last

After the table is populated, use `applyTextStyle` with `textToFind` to bold the header row text.

**Inserting content after a table:** Use `appendToGoogleDoc` to add text after the table, since calculating the post-table index is error-prone.

---

## Bullet points

Google Docs MCP does not have a native bullet list style. Use this convention:

- Prefix each bullet line with `"  •  "` (space bullet space) in the clean text
- This renders visually as a bulleted list in the document

---

## Em-dash reduction

Minimize em dashes in Google Docs output. They pile up fast and look cluttered.

| Instead of | Use |
|------------|-----|
| `Report — Client — April 2026` | `Report: Client, April 2026` |
| `seen 4 times — Confidence: HIGH` | `seen 4 times / Confidence: HIGH` or `seen 4 times. Confidence: HIGH` |
| `was in voice profile — not heard in 4 weeks` | `was in voice profile, not heard in 4 weeks` |

Replacements: `:`, `,`, `.`, `/`, or restructure the sentence. One em dash per paragraph max, zero if possible.

---

## Practical example

Markdown input:
```
## New Patterns Found

**Phrases and Expressions**
- "the thing is", seen 4 times / Confidence: HIGH
- "what most people miss", seen 3 times / Confidence: HIGH

### Suggested Additions

Add these to the *use* list in voice-profile.md.
```

Procedure:
1. Create empty document
2. Insert clean text:
   ```
   New Patterns Found
   Phrases and Expressions
     •  "the thing is", seen 4 times / Confidence: HIGH
     •  "what most people miss", seen 3 times / Confidence: HIGH
   Suggested Additions
   Add these to the use list in voice-profile.md.
   ```
3. Apply styles:
   - `applyParagraphStyle("New Patterns Found", HEADING_2)`
   - `applyTextStyle("Phrases and Expressions", bold: true)`
   - `applyParagraphStyle("Suggested Additions", HEADING_3)`
   - `applyTextStyle("use", italic: true)`

---

## When to use this procedure

Any time a skill saves content to Google Docs — including but not limited to:
- `monthly-voice-refresh` (Step 5 — Google Drive Save)
- `lead-magnet-builder` (when output format is Google Doc)
- Any future skill that creates Google Docs output

**Reference this file:** `google-docs-formatting.md` from any skill step that writes to Google Docs.
