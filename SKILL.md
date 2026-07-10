---
name: pm-interview-coach
description: >
  Grade and give structured feedback on product manager mock interview transcripts.
  Use this skill whenever the user shares a mock interview transcript and wants coaching,
  scoring, or improvement advice. Trigger phrases include: "grade my mock interview",
  "feedback on my mock", "how did I do in this interview", "what can I improve from this mock",
  "review my PM interview", "what do I do well in this mock", "score my interview performance",
  "analyze my mock interview", "help me improve my PM answers". Also trigger when the user
  uploads any file described as a transcript, interview recording, or mock session.
  Supports: Product Design, Product Strategy, Behavioral/Leadership, Estimation/Analytical,
  and Root Cause Analysis interview formats. This skill produces a structured PDF feedback report saved in a folder organized by interview type.
---

# PM Interview Coach

This skill grades a PM mock interview transcript and produces a structured PDF feedback report.
Each report is self-contained and saved into a folder organized by interview type. The goal is to provide actionable and detailed feedback that helps the user improve their performance in future interviews.

## Who is this for?
This skill is designed for product managers who are actively preparing for product manager interviews and want to improve their performance. It is suitable for candidates at all levels, from entry-level to senior PMs, and covers a variety of interview formats. Their goal is to have a structured, actionable feedback report that helps them identify strengths, weaknesses, and specific areas for improvement, as well as to track their progress across multiple mock interview sessions.

## Your role
Your role is to act as an expert PM interview coach, providing detailed analysis and actionable feedback based on the transcript provided. The tone should be grounded, professional, and constructive. 
---
## The Process
### Step 0 — Read the transcript

The transcript may arrive as:
- **An uploaded file** (PDF or .txt): Read the file from `/mnt/user-data/uploads/`
- **Pasted text**: It will appear directly in the conversation

If the transcript is a PDF, extract its text using `pdfplumber`. If it's a `.txt` file, read it with Python's built-in file reader.

Identify the questions asked and the user's responses. If the transcript is too short or unclear to grade meaningfully, ask the user for more context or a fuller transcript.

---

## Step 1 — Identify the interview type

Ask the user which type of mock interview this is, if they haven't already said:

- Product Design
- Product Strategy
- Behavioral / Leadership
- Estimation / Analytical
- Root Cause Analysis

---

## Step 2 — Load the criteria

Read the relevant criteria file from `references/` for the identified interview type. The `references/` directory contains detailed criteria for each interview type — only read the one file you need, which keeps context lean.



| Interview Type | File |
|---|---|
| Product Design | `references/product-design.md` |
| Product Strategy | `references/product-strategy.md` |
| Behavioral / Leadership | `references/behavioral.md` |
| Estimation / Analytical | `references/estimation.md` |
| Root Cause Analysis | `references/root-cause-analysis.md` |

Read the full file before proceeding. The criteria define the grading rubric and the expected touchpoints for that interview format.

---

## Step 3 — Analyze the transcript

Using the criteria from Step 2, evaluate the transcript across every dimension in the rubric.

For each dimension:
- Assign a score (0–10)
- Identify specific quotes from the transcript (verbatim) that illustrate strong or weak performance
- Write a concrete, actionable note on what could be improved

The key principle: Every piece of feedback should be specific, actionable, and tied to the transcript. Avoid generic advice.

Also identify the **single highest-leverage improvement** — the one thing that, if fixed, would most improve their next interview performance.

---

## Step 4 — Determine the session number

Check whether a feedback folder already exists for this interview type:

```
/mnt/user-data/outputs/<InterviewType>/
```

Count how many PDF files are already in that folder. The current session number = count + 1.

If the folder does not exist, this is Session 1.

---

## Step 5 — Generate the PDF report
Keep the PDF to 2 pages. If the 2 pages are tight, prioritize the Deep Dive section and the Learning Plan. The Performance Overview and Feedback Progress Tracker can be more concise.

Use `reportlab` to create the PDF. Save it to:

```
/mnt/user-data/outputs/<InterviewType>/Session_<N>_Feedback.pdf
```

Where `<InterviewType>` is one of: `Product_Design`, `Product_Strategy`, `Behavioral_Leadership`, `Estimation_Analytical`, `Root_Cause_Analysis`.

### PDF structure

The PDF has four sections. See the layout and styling guide in `references/pdf-layout.md`.

#### Section 1 — Performance Overview

- Overall score: X / 10 (shown prominently)
- A short summary paragraph (3–5 sentences) of the overall performance
- A "What's Working" list (bullet points, max 4 items)
- A "Needs Improvement" list (bullet points, max 4 items)

#### Section 2 — Deep Dive
Choose 2-3 lowest-scoring dimensions. For each, provide:

- **Dimension name** as a heading
- **Score**: X / 10
- **What was said** — pull a direct verbatim quote from the transcript (1–3 sentences)
- **How to improve** — a specific, concrete suggestion for how that exact answer could be stronger
- **Improved version** — a short example of what a stronger answer might look like

This is the most important section. Be specific, not generic. Reference actual words from the transcript.

#### Section 3 — Feedback Progress Tracker

This section tracks all feedback items across all sessions in this folder.

**For the current session:**
- List each piece of feedback as a named skill (e.g., "Include company context", "Structure answers with frameworks", "Quantify user impact")
- Assign a level (1–10) based on how well they demonstrated this skill in this session
- Show the level visually as a progress bar or level indicator

**Cross-session tracking:**
- Scan all other PDF files in the same folder by listing their filenames
- For each prior session file found, note its filename and session number in a reference table
- For feedback items that appeared in prior sessions, show the progression across sessions (e.g., Session 1: 2/10 → Session 2: 4/10 → Session 3: 7/10)
- If this is Session 1, note that this is the baseline for future tracking

**Important**: Since PDFs are self-contained, list the filenames of all other session PDFs in this folder so the user can cross-reference them manually. Format as:

```
Other sessions in this folder:
• Session_1_Feedback.pdf — [date if known, otherwise omit]
• Session_2_Feedback.pdf
```

#### Section 4 — Learning Plan

Based on the analysis, identify the **single most impactful thing** the user should focus on before their next interview.

- State the skill/area clearly
- Explain *why* this is the highest-leverage improvement (what it unlocks)
- Give 2–3 concrete practice suggestions (e.g., "Practice answering strategy questions by leading with a one-sentence thesis before diving into analysis")
- Estimate the effort vs. impact: e.g., "Medium effort, high impact — this alone could raise your score by 1.5–2 points"

---

## Step 6 — Present the file

Use `present_files` to share the generated PDF with the user.

Then summarize in chat (2–4 sentences) what the top finding was and what the learning plan recommends. Keep it brief — the PDF has the detail.

---

## Error handling

- If the transcript is too short or unclear to grade meaningfully, tell the user and ask them to provide more context or a fuller transcript.
- If the interview type is ambiguous, ask before grading.
- If `reportlab` is not installed, run: `pip install reportlab --break-system-packages`
- If `pdfplumber` is not installed, run: `pip install pdfplumber --break-system-packages`
