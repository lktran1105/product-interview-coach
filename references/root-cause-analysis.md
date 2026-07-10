# Root Cause Analysis Interview — Grading Criteria

## What a Great Root Cause Analysis Interview Looks Like

The candidate should approach a metric change or product issue like a detective: structured, methodical, hypothesis-driven, and data-informed. They should resist jumping to conclusions and demonstrate the ability to rule out causes systematically.

---

## Grading Dimensions (each scored 0–10)

### 1. Clarifying Questions (Weight: High)
**What to look for:**
- Did the candidate ask targeted clarifying questions before hypothesizing?
- Did they ask about: time frame, magnitude, affected segments, geography, and data quality?
- Did they avoid jumping to explanations before understanding the facts?

**Strong**: Asks about when the change started, how large it is (absolute vs. relative), whether it's across all users or a segment, and whether the data pipeline is healthy.
**Weak**: Jumps straight to "maybe it's a bug" or "maybe we changed something" without asking about the context.

---

### 2. Ruling Out Data / Instrumentation Issues (Weight: High)
**What to look for:**
- Did the candidate first check whether the metric change is real or an artifact?
- Did they ask about logging errors, instrumentation changes, or dashboard bugs?
- Did they treat this as a mandatory first step?

**Strong**: "Before hypothesizing causes, I'd first check whether the data is trustworthy — are there any logging gaps, pipeline delays, or instrumentation changes in this timeframe?"
**Weak**: Skips data quality check and jumps directly to product/business hypotheses.

---

### 3. Internal vs. External Factors (Weight: High)
**What to look for:**
- Did the candidate separate internal causes (product changes, bugs, experiments) from external causes (seasonality, competition, macro events)?
- Did they check for recent product changes, experiments, or releases?

**Strong**: Explicitly creates two buckets — "First I'd look at internal factors (did we ship something? run an experiment? have an outage?), then external factors (competitor moves, seasonality, news events)."
**Weak**: Only considers one type of cause (e.g., only internal bugs or only external factors).

---

### 4. Hypothesis Generation (Weight: High)
**What to look for:**
- Did the candidate generate multiple distinct hypotheses?
- Were hypotheses specific enough to be testable?
- Did they cover the problem space (not just the first thing that came to mind)?

**Strong**: Generates 4–6 hypotheses covering data quality, internal changes, external factors, and user behavior changes.
**Weak**: Only generates 1–2 hypotheses, or hypotheses are too vague ("something went wrong with the product").

---

### 5. Prioritization of Hypotheses (Weight: High)
**What to look for:**
- Did the candidate prioritize which hypothesis to investigate first?
- Was the prioritization based on likelihood and ease of validation?
- Did they avoid investigating all hypotheses equally?

**Strong**: "I'd start with data quality (fastest to check, falsifies immediately if true), then check for recent product changes (most likely internal cause), then look at segment breakdown."
**Weak**: Lists hypotheses but doesn't prioritize them, or says "I'd check everything simultaneously."

---

### 6. Segmentation (Weight: Medium)
**What to look for:**
- Did the candidate suggest breaking down the metric by segment to isolate the cause?
- Did they consider: platform (iOS/Android/web), geography, user cohort (new vs. returning), feature area?

**Strong**: "I'd break this down by platform, user type, and geography to see if the drop is concentrated somewhere specific — that would narrow the hypotheses significantly."
**Weak**: Treats the metric as monolithic without suggesting any segmentation.

---

### 7. Proposed Next Steps (Weight: Medium)
**What to look for:**
- Did the candidate propose clear, concrete next steps for each hypothesis?
- Did they describe what data or query they'd run?
- Were their steps actionable?

**Strong**: "To test the data quality hypothesis, I'd pull the raw event logs and compare daily counts for the past 30 days. To test the experiment hypothesis, I'd check the experiment dashboard for any active tests with launch timing in this window."
**Weak**: "I'd investigate further" without specifying how.

---

### 8. Communication & Structure (Weight: Medium)
**What to look for:**
- Was the candidate's process easy to follow?
- Did they signal their framework before diving in?
- Did they avoid jumping back and forth between steps?

**Strong**: "I'll approach this in three steps: (1) verify the data, (2) check internal changes, (3) explore external factors. Let me start with step 1."
**Weak**: Meanders through hypotheses without a clear structure, hard to follow.

---

### 9. Calm Under Ambiguity (Weight: Low)
**What to look for:**
- Was the candidate comfortable with not having all the data?
- Did they make reasonable assumptions when needed?
- Did they avoid panicking or getting stuck when information was missing?

**Strong**: "I don't have that data, so I'll assume X for now and flag it as something I'd verify first."
**Weak**: Gets stuck when a data point isn't available, or needs the interviewer to give them all the information.

---

## Common Failure Modes

- **Skipping data quality**: Jumping to product/business hypotheses without validating the metric
- **Single hypothesis**: Only proposing one cause and going deep on it without exploring alternatives
- **No segmentation**: Treating a single aggregate metric drop without breaking it down
- **Vague next steps**: Saying "I'd investigate" without specifying how
- **Premature conclusion**: Committing to a root cause before ruling out alternatives
- **Ignoring external factors**: Only looking internally for causes

---

## Score Benchmarks

| Score | Meaning |
|---|---|
| 9–10 | Excellent: data-first, systematic hypotheses, smart segmentation, clear next steps |
| 7–8 | Good: solid structure and hypothesis coverage, minor gaps in segmentation or prioritization |
| 5–6 | Borderline: reasonable hypotheses but skips data quality check or lacks prioritization |
| 3–4 | Weak: jumps to conclusions, few hypotheses, vague investigation plan |
| 0–2 | Not ready: no structure, confused by the problem, premature commitment to one cause |
