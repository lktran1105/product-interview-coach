# PM Interview Coach

A Claude Skill that grades product manager mock interview transcripts, generates a structured PDF feedback report, tracks your progress across sessions, and can book your next practice session on your calendar.

Drop in a transcript, get back a scored report with what's working, what isn't, and the single highest-leverage thing to fix before your next mock — plus a trend line showing whether you're actually improving.

## Who is this for?
Anyone actively prepping for PM interviews — entry-level to senior — who wants structured, specific feedback


## The problem to be solved
You're doing mocks consistently. That's not the issue. The issue is what happens after — feedback scattered across a notebook or spreadsheet, updated inconsistently, until a week later you can't tell if you're improving or just running in place.

And not all feedback is equal. Fixing your user segmentation is a reasoning gap. Fixing your pacing is a habit. Treat them the same, and you'll polish the wrong thing before your next interview.

You don't need more feedback. You need feedback that compounds — specific enough to act on now, tracked closely enough to prove it moved.

## Supported interview formats

- Product Design
- Product Strategy
- Behavioral / Leadership
- Estimation / Analytical
- Root Cause Analysis

## How it works

**1. Share a transcript**
Upload a transcript file (PDF or `.txt`) or paste the text directly, and ask Claude to grade it. Claude infers the interview type from the prompt and confirms with you before scoring.

**2. Get a scored PDF report**
Claude generates a four-section report:
- **Performance Overview** — overall score, a summary, what's working, what needs improvement
<img width="580" height="549" alt="part 1" src="https://github.com/user-attachments/assets/e1a1bd4f-65a5-42b6-882e-94490d8577d9" />

- **Deep Dive** — your 2–3 lowest-scoring dimensions, each with a verbatim quote from your answer, a concrete way to improve it, and an example of a stronger version
<img width="582" height="712" alt="part 2" src="https://github.com/user-attachments/assets/58a612c8-2945-495d-a924-983aad53b247" />

- **Feedback Progress Tracker** — named skills (e.g. "Quantify user impact") scored for this session
<img width="574" height="657" alt="part 3" src="https://github.com/user-attachments/assets/bf276338-31fa-4861-b592-8f08b35ca37b" />

- **Learning Plan** — the single most impactful thing to focus on next, with practice suggestions and an effort-vs-impact estimate
<img width="579" height="460" alt="part 4" src="https://github.com/user-attachments/assets/ff7190df-81fe-4d3b-a08c-06d8c2b43f40" />


**3. Track progress across sessions**
Run more mocks and upload prior reports (or just keep grading in the same conversation), and each new report shows how your named skills have moved — e.g. "Structure answers with frameworks: Session 1: 3/10 → Session 2: 6/10." It looks back at your 3 most recent sessions per interview type.

**4. Close the loop with a calendar hold**
If Google Calendar is connected, Claude can offer to book a one-off practice session targeted at your weakest area — proposing a time, confirming with you, then creating the event with the "why it matters" and practice suggestions in the description.
<img width="478" height="467" alt="calendar" src="https://github.com/user-attachments/assets/b1abef99-1038-44de-8cd0-0ddc03bb5025" />


## Output
- Feedback is tied to specific quotes from your transcript, not generic advice.
- If a rubric dimension wasn't addressed at all (e.g. you ran out of time), the report says so explicitly rather than inventing a quote for it.
- Calendar booking is a single one-off hold per report, not a recurring series — you're offered a fresh one after every graded session.

If in the same context window (chat session), Claude will remember the feedback from each mock and organize them by interview type (`Product_Design`, `Product_Strategy`, `Behavioral_Leadership`, `Estimation_Analytical`, `Root_Cause_Analysis`), so every session is self-contained and easy to cross-reference.
Highly recommend user to download each feedback report to refer back to. 

## How to use
**As a Claude Code skill**

1. Copy the `pm-interview-coach` folder into Claude Skills directory.
2. Share a mock interview transcript with Claude and ask it to grade your performance.
3. (Optional) Connect with Google Calendar to book practice directly on your calendar.

**Trigger phrases**
- "Grade my mock interview"
- "Feedback on my mock"
- "How did I do in this interview"
- "What can I improve from this mock"
- "Score my interview performance"
- "Help me improve my PM answers"


## Skill structure

```
pm-interview-coach/
├── SKILL.md                          # Skill definition and grading process
└── references/
    ├── product-design.md             # Rubric for Product Design interviews
    ├── product-strategy.md           # Rubric for Product Strategy interviews
    ├── behavioral.md                 # Rubric for Behavioral/Leadership interviews
    ├── estimation.md                 # Rubric for Estimation/Analytical interviews
    ├── root-cause-analysis.md        # Rubric for Root Cause Analysis interviews
    └── pdf-layout.md                 # PDF styling and layout guide
```

## Requirements

- `reportlab` — generates the PDF report
- `pdfplumber` — extracts text from uploaded PDF transcripts
- (Optional) Google Calendar connector, for the practice-session booking step

