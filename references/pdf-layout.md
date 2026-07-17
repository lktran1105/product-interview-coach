# PDF Layout & Styling Guide

This file defines the visual design and layout conventions for PM Interview Coach feedback PDFs. Follow these specs exactly when generating reports with `reportlab`.

---

## Page Setup

```python
from reportlab.lib.pagesizes import letter
from reportlab.lib import colors
from reportlab.lib.units import inch

PAGE_WIDTH, PAGE_HEIGHT = letter  # 8.5 x 11 inches
MARGIN = 0.85 * inch
CONTENT_WIDTH = PAGE_WIDTH - (2 * MARGIN)
```

---

## Color Palette

```python
# Primary brand colors
COLOR_PRIMARY    = colors.HexColor('#1B2B4B')   # Deep navy — headings, titles
COLOR_ACCENT     = colors.HexColor('#3B6BE0')   # Bright blue — scores, highlights
COLOR_SUCCESS    = colors.HexColor('#27A96C')   # Green — "what's working"
COLOR_WARNING    = colors.HexColor('#E07B3B')   # Orange — "needs improvement"
COLOR_LIGHT_BG   = colors.HexColor('#F4F6FA')   # Light blue-grey — section backgrounds
COLOR_QUOTE_BG   = colors.HexColor('#EEF2FF')   # Pale indigo — transcript quote boxes
COLOR_BORDER     = colors.HexColor('#D1D9EC')   # Muted blue-grey — dividers, borders
COLOR_TEXT       = colors.HexColor('#2D3A4A')   # Dark slate — body text
COLOR_MUTED      = colors.HexColor('#6B7B92')   # Muted blue-grey — secondary text
COLOR_WHITE      = colors.white
```

---

## Typography

Use Helvetica throughout (built into reportlab, no font install needed).

```python
# Font hierarchy
FONT_TITLE    = ('Helvetica-Bold', 22)
FONT_H1       = ('Helvetica-Bold', 16)
FONT_H2       = ('Helvetica-Bold', 13)
FONT_H3       = ('Helvetica-Bold', 11)
FONT_BODY     = ('Helvetica', 10)
FONT_SMALL    = ('Helvetica', 9)
FONT_QUOTE    = ('Helvetica-Oblique', 9.5)
FONT_LABEL    = ('Helvetica-Bold', 8)
```

---

## PDF Header

The first page is a cover with:

```
[Full-width navy banner, 1 inches tall]
  - To the left:
    - Interview type (e.g. "Product Design") in white, 28pt bold, centered
    - "Session N Feedback Report" in light blue-grey, 14pt, below title
  
  - To the right:
    - Score badge: circle, filled with accent blue, spans the full banner height, edge-to-edge. The score badge circle's diameter must be large enough that the score text (e.g., '4.5') and the '/ 10' label both fit fully inside the circle with at least 6-8pt of padding between the text and the circle's edge on all sides — the font sizes should be treated as fixed and the circle sized to fit them, not the other way around.
    - "8.3" in white, 36pt bold
    - "/ 10" in white, 16pt

```

---

## Section Headers

Each of the four sections has:
- A full-width colored banner (height: 40pt) in COLOR_PRIMARY
- Section number + title in white, Helvetica-Bold 14pt, left-aligned with 20pt padding
- Each section header uses the same banner style — regardless of whether it starts a new page or shares a page with other sections. Do not shrink, recolor, or simplify this banner style. Candidate must be able to distinguish one section from another.
- A subtitle in COLOR_MUTED, 9pt, below the banner, with at least 16pt of vertical gap between the heading baseline and the subtitle baseline

Example:
```
[███████████████████████████████████████████]
  01  Performance Overview
[Subtitle: A summary of your mock interview performance]
```
---

## Spacing Between Cover Banner and First Section

On page 1, the cover banner and the Section 1 banner must NOT be adjacent or touching.
Leave a 28pt gap of white space between the bottom edge of the cover banner and the top edge of the Section 1 banner. This gap must be visually obvious — the
two navy blocks should read as clearly separate elements, not one continuous block.
---
## Question Block

Immediately before the Section 1 banner, add a dedicated Interview Question block: a small bold label 'INTERVIEW QUESTION' (8-9pt, muted color) followed by the prompt text (9.5-10pt, regular) in a lightly shaded background box with rounded corners, spanning the content width. Cap the block at 4 lines of wrapped text; if the question is longer, truncate at the 4th line and end with an ellipsis. This block sits after the fixed 28pt gap below the cover banner (the gap stays fixed regardless of question length), and is followed by its own 16-20pt spacing before the Section 1 banner begins — so the block's height is the only variable element, not the gap itself.
---

## Score Display (used in Section 1 and Section 2)

For the overall score, use a large emphasized block:
```
┌────────────────────────────────┐
│  OVERALL SCORE                 │
│  ████████░░  8.3 / 10         │
└────────────────────────────────┘
```

For dimension scores in Section 2, use a compact row:
```
User Segmentation          ██████░░░░  6 / 10
```

Progress bar implementation:
```python
def draw_progress_bar(canvas, x, y, width, score, max_score=10):
    bar_height = 8
    filled_width = width * (score / max_score)
    
    # Background
    canvas.setFillColor(COLOR_BORDER)
    canvas.rect(x, y, width, bar_height, fill=1, stroke=0)
    
    # Fill
    if score >= 8:
        fill_color = COLOR_SUCCESS
    elif score >= 6:
        fill_color = COLOR_ACCENT
    else:
        fill_color = COLOR_WARNING
    
    canvas.setFillColor(fill_color)
    canvas.rect(x, y, filled_width, bar_height, fill=1, stroke=0)
```

---

## Quote Boxes (Section 2 — Deep Dive)

Transcript quotes should appear in a visually distinct box:

```
┌─────────────────────────────────────────────────────────────┐  ← COLOR_QUOTE_BG background
│  💬 What you said                                           │  ← FONT_LABEL, COLOR_ACCENT
│                                                             │
│  "...actual quote from the transcript here..."             │  ← FONT_QUOTE, italic
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐  ← white background
│  ✨ How to make it stronger                                 │  ← FONT_LABEL, COLOR_SUCCESS
│                                                             │
│  Specific improvement suggestion here...                    │  ← FONT_BODY
│                                                             │
│  Example: "A stronger version might say: '...'"            │  ← FONT_QUOTE, italic, muted
└─────────────────────────────────────────────────────────────┘
```

Implementation tip: Use `canvas.roundRect()` with a corner radius of 4 for the quote boxes.

---

## Feedback Tracker (Section 3)

Each feedback item is a card:

```
┌─────────────────────────────────────────────────────────────┐
│  Include Company Context               Session Progress      │
│  ─────────────────────────────────────────────────────────  │
│  Level: 3 / 10    ███░░░░░░░  (↑ from 1 in Session 1)      │
│                                                             │
│  Sessions: S1: 1  →  S2: 2  →  S3: 3                      │
│  Trend: ↑ Improving                                         │
└─────────────────────────────────────────────────────────────┘
```

For Session 1 (no prior data):
```
│  Level: 3 / 10    ███░░░░░░░  (Baseline — Session 1)        │
│  This is your first session. Track progress in future ones. │
```

Level coloring:
- 1–3: COLOR_WARNING (orange)
- 4–6: COLOR_ACCENT (blue)
- 7–10: COLOR_SUCCESS (green)

---

## Learning Plan (Section 4)

Use a prominent highlight box:

```
┌─────────────────────────────────────────────────────────────┐  ← COLOR_LIGHT_BG
│  🎯 YOUR #1 FOCUS                                           │
│                                                             │
│  [Skill Name in COLOR_PRIMARY, bold, 13pt]                 │
│                                                             │
│  Why this matters:                                          │
│  [Explanation paragraph]                                    │
│                                                             │
│  Practice suggestions:                                      │
│  • Suggestion 1                                             │
│  • Suggestion 2                                             │
│  • Suggestion 3                                             │
│                                                             │
│  ⚡ Effort: Medium  |  Impact: High  |  +1.5 pts potential  │
└─────────────────────────────────────────────────────────────┘
```

---

## Footer (every page)

```python
def draw_footer(canvas, page_num, session_num, interview_type):
    y = 0.5 * inch
    canvas.setFont('Helvetica', 8)
    canvas.setFillColor(COLOR_MUTED)
    canvas.drawString(MARGIN, y, f"PM Interview Coach  |  {interview_type}  |  Session {session_num}")
    canvas.drawRightString(PAGE_WIDTH - MARGIN, y, f"Page {page_num}")
    canvas.setStrokeColor(COLOR_BORDER)
    canvas.line(MARGIN, y + 12, PAGE_WIDTH - MARGIN, y + 12)
```

---

## Section Reference Table (inside Section 3)

List all other PDFs in the folder:

```
Other sessions in this folder:
────────────────────────────────────────────────
Session 1    Session_1_Feedback.pdf    [baseline]
Session 2    Session_2_Feedback.pdf
────────────────────────────────────────────────
Current      Session_3_Feedback.pdf   ← you are here
```

Use a simple table with COLOR_LIGHT_BG alternating rows.

---

## General Notes

- All body text: 10pt, COLOR_TEXT, 14pt line spacing
- All section content: starts 20pt below the section banner
- Paragraph spacing: 8pt between paragraphs
- Any colored box (quote box, highlight box) must be followed by at least 12pt of vertical space before the next heading or label begins
- Use `Spacer(1, 12)` between subsections when using Platypus, or manually track y-position with canvas
- Prefer canvas (low-level) over Platypus for pixel-accurate control of the quote boxes and progress bars
- Always call `canvas.showPage()` at the end of each page and `canvas.save()` at the end
- Box Sizing: Measure actual wrapped text height and size boxes dynamically — never use a fixed box height. Check this any time you add new content dimensions or longer feedback text, since fixed-height boxes will silently clip or leave awkward whitespace when text length changes.
- Bullet: Align the bullet glyph and its text on the same baseline/indent — the bullet marker and the wrapped text block should share a consistent left edge, with wrapped lines indented to match the start of the first line's text (not back to the bullet).
- No need for emojis. Users can already tell the differences from the colors and box. Keep it simple.

---
## General Rule: Preventing Element Overlap via Draw Order

PDF/canvas drawing has no z-index — elements are painted strictly in the order the code draws them, and a later, opaque element will always paint over anything beneath it, regardless of how light or pale the earlier element's background color is. A near-white box sitting under an opaque banner is invisible, not "behind" it.

Before drawing any new block, the y-coordinate for the *next* element must be derived from the full computed height of the block just drawn — not a hardcoded offset. Full height means:

- number of wrapped text lines × line height
- plus top and bottom internal padding
- plus the intended gap before the next element

Never advance y by a fixed guess (e.g., "subtract 40") that isn't actually computed from the content just drawn. Fixed offsets silently break the moment content length changes — one line of text vs. three lines produces a different actual height, and a hardcoded offset won't reflect that.

**Before finalizing any new element added between two existing ones**, explicitly trace:
- the bottom y-edge of the block above (its start y − its full computed height)
- the top y-edge of the block below (its start y)

Confirm there is a clear, intentional gap between these two values. If the bottom edge of the upper block is at or below the top edge of the lower block, they overlap — this must be checked via the actual numbers, not just a visual scan of the rendered PNG, since overlap with a pale background color is easy to miss by eye even when reviewing output.
