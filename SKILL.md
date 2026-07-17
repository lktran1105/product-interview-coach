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

First, infer the likely interview type yourself from the transcript — the prompt phrasing is usually enough to tell:

- Product Design
- Product Strategy
- Behavioral / Leadership
- Estimation / Analytical
- Root Cause Analysis

Do not skip straight to grading based on this inference. Always confirm it with the user before proceeding, even when it seems obvious — state which type you inferred and briefly why (e.g., "This reads as a Product Strategy round — the prompt asks what you'd build/prioritize rather than a specific design or a past experience. Confirm before I grade?"). Wait for the user to confirm or correct it before moving to Step 2.

If the user has already stated the interview type explicitly, skip the inference and confirmation and just proceed with what they said.

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

If the candidate never addressed a dimension at all (e.g., ran out of time before naming metrics), do not substitute the interviewer's closing remarks or any other stand-in text as the "What was said" quote. Instead, write "None — not addressed in this session" and adjust "How to improve" to focus on building in time/awareness for that dimension next time.

---

## Step 4 — Determine the session number and gather prior progress

First, check the local output folder for this interview type:

​```
/mnt/user-data/outputs/<InterviewType>/
​```

**If PDFs already exist in this folder**, use them directly — this means prior sessions happened
earlier in this same conversation. Count them; the current session number = count + 1. Extract
each prior PDF's Feedback Progress Tracker data (see extraction method below) to build cross-session
progression.

**If the folder does not exist or is empty**, do not assume this is Session 1 automatically — the
user may have done mock interviews in an earlier conversation and downloaded the PDFs elsewhere.
Ask the user once, before grading:

> "Is this your first [Interview Type] mock, or do you have a previous feedback PDF I should
> factor in for progress tracking? If you have one, feel free to upload it."

Handle the response:
- **User has no prior PDF / this is their first attempt** → proceed as Session 1, baseline.
- **User uploads one or more prior PDFs** → look at only the **3 most recent** (by session number
  or upload order, whichever is clearer from what's provided — if more than 3 are uploaded, use
  the 3 highest session numbers and ignore the rest). For each of those, read the PDF from
  `/mnt/user-data/uploads/` using `pdfplumber` and extract **only the Feedback Progress Tracker
  section** — do not extract or return the full PDF text (Deep Dive quotes, summary paragraphs,
  cover page, etc.) into context. Locate the section by searching for the "Feedback Progress
  Tracker" heading and reading only the text between it and the next section banner. From that
  section, pull:
  - The session number
  - The named skills and levels listed
  The current session number = the highest session number found across the uploaded PDFs, plus 1.
- **User says they have one but can't find/upload it** → proceed as if this is the first *tracked*
  session (note this in Section 3 rather than blocking progress).

Do not re-ask this question if prior PDFs were already found locally in this same folder — only
ask when the folder is empty or missing.

---

## Step 5 — Generate the PDF report
There are no page limit. However, keep the feedback concise.

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
- If prior session data came from an **uploaded PDF** rather than a local folder scan, only its Feedback Progress Tracker section was read (not the full PDF) — merge those extracted skill names/levels with this session's skill list by name, using judgment to match skills that are worded slightly differently across sessions (e.g. "Quantify user impact" and "Quantifying impact" are the same skill).
- If more than 3 prior PDFs exist or were uploaded, only the 3 most recent are reflected in the progression — note this in the reference table if it applies (e.g., "Showing your 3 most recent sessions").
- Note in the reference table whether each prior session's data came from a local file or an upload, so the user understands where the history came from.

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
## Step 7 — Offer a calendar practice hold (optional)

After presenting the PDF to the user, check whether a Google Calendar connector is available.

**If no Google Calendar connector is available:**
Use `search_mcp_registry` to check, then `suggest_connectors` to offer connecting it. If the
user declines or doesn't connect, skip this step entirely — do not block or repeat the offer.

**If a Google Calendar connector is available (connected or just connected):**
Ask a single yes/no question:

> "Want me to set up a calendar hold to practice before your next mock?"

- **If no** → stop here. No further action.
- **If yes** → ask for their scheduling preference, accepting either a specific date/time or a
  range (e.g., "Thursday afternoon," "between 8-10pm for the next three days"). Accept whatever
  format they give; don't force a specific structure.

**Finding a slot:**
Use the Calendar connector to check events within the stated window.
- If a free slot of the requested duration (default 30 min if unspecified) exists within the window, propose the earliest one.
- If no slot is free anywhere in the stated window, propose the **closest available slot outside** the window instead, and clearly flag that it's outside what they asked for. Only propose slots between 9am–9pm local time, unless the user explicitly requests otherwise.
- Always show the proposed slot and ask for confirmation before creating anything — never create the event silently, even if the user has already said "yes" to the general idea.

**Creating the event:**
Once confirmed, create a **single, one-off** event (no recurrence) using the Calendar connector:
- **Title:** `Practice: [Interview Type] — [short Learning Plan focus]`
- **Description:** the "why this matters" + practice suggestions from that session's Learning Plan
- **Duration:** as discussed with the user (default 30 min)

This is a single-session offer — do not ask about recurring holds or attempt to track/update this
event in future sessions. Each new report gets its own independent offer.

Do not attach the PDF report to the event unless the user explicitly asks — this requires first
uploading the PDF to Google Drive to get a shareable link, since the Calendar connector's
attachment field only accepts a URL, not a local file.

---
## Error handling

- If the transcript is too short or unclear to grade meaningfully, tell the user and ask them to provide more context or a fuller transcript.
- If the interview type is ambiguous, ask before grading.
- If `reportlab` is not installed, run: `pip install reportlab --break-system-packages`
- If `pdfplumber` is not installed, run: `pip install pdfplumber --break-system-packages`
