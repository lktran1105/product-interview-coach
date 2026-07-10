# Estimation / Analytical Interview — Grading Criteria

## What a Great Estimation Interview Looks Like

The candidate should demonstrate structured decomposition, reasonable assumption-making, numerical comfort, and the ability to sanity-check their own answers. The goal is never the exact number — it's the quality of the thinking.

---

## Grading Dimensions (each scored 0–10)

### 1. Problem Clarification (Weight: High)
**What to look for:**
- Did the candidate ask 1–2 clarifying questions to scope the problem?
- Did they define the geography, time period, or population before estimating?
- Did they avoid over-clarifying?

**Strong**: Defines scope clearly ("I'll estimate for the US, in a given year, for adults 18+").
**Weak**: Jumps into numbers without defining the scope, or asks too many unnecessary questions.

---

### 2. Framework / Decomposition (Weight: High)
**What to look for:**
- Did the candidate break the problem into logical sub-components?
- Was the decomposition MECE (mutually exclusive, collectively exhaustive)?
- Did the structure make the problem tractable?

**Strong**: Clearly states the formula/approach before calculating. "I'll estimate this as: [number of X] × [rate of Y] × [average Z]."
**Weak**: Jumps into a single number without breaking it down, or decomposition is redundant/overlapping.

---

### 3. Assumptions (Weight: High)
**What to look for:**
- Did the candidate state their assumptions explicitly?
- Were assumptions reasonable and grounded?
- Did they flag which assumptions are most important / most uncertain?

**Strong**: "I'll assume roughly 330M people in the US, ~75% adults. I'm less certain about the usage rate — I'll estimate 15% but flag that as the key variable."
**Weak**: Uses numbers without explaining where they came from, or makes unreasonable assumptions without acknowledgment.

---

### 4. Numerical Comfort (Weight: Medium)
**What to look for:**
- Did the candidate do the arithmetic accurately and confidently?
- Did they use round numbers to keep math manageable?
- Did they avoid getting bogged down in false precision?

**Strong**: Rounds intelligently ("let's call that ~100M for simplicity"), does mental math out loud, comfortable with magnitudes.
**Weak**: Gets lost in arithmetic, uses overly precise numbers that make the math unwieldy, or makes calculation errors.

---

### 5. Sanity Check (Weight: High)
**What to look for:**
- Did the candidate check their answer at the end?
- Did they compare to a known reference point?
- Did they catch if their number was wildly off?

**Strong**: "So I got ~$50B market. That feels reasonable — I know Uber's revenue is ~$30B, so this is in the right ballpark."
**Weak**: States a final answer without checking whether it makes sense, or gives an obviously unreasonable answer without noticing.

---

### 6. Structured Communication (Weight: Medium)
**What to look for:**
- Did the candidate narrate their thinking out loud, step by step?
- Could the interviewer follow along easily?
- Did they signpost each step?

**Strong**: "First I'll estimate the population, then I'll apply a penetration rate, then I'll calculate average spend." Talks through each step.
**Weak**: Jumps between steps, does math silently, hard to follow.

---

### 7. Creativity & Insight (Weight: Low)
**What to look for:**
- Did the candidate offer any interesting insight about the structure of the problem?
- Did they flag any counterintuitive aspects?
- Did they show any genuine curiosity about the number?

**Strong**: "Interestingly, the market is probably heavily concentrated in 3–4 cities, so the per-capita estimate would be misleading."
**Weak**: Treats it as a pure arithmetic exercise with no commentary on what the number means.

---

### 8. Adaptability (Weight: Medium — only if applicable)
**What to look for:**
- If the interviewer changed a constraint or challenged an assumption, did the candidate update cleanly?
- Did they stay calm and recalculate without getting flustered?

**Strong**: "Good point — if I use 10% penetration instead of 15%, my estimate drops to ~$40B. Still feels plausible."
**Weak**: Gets defensive about their assumptions, or can't update the calculation when prompted.

---

## Common Failure Modes

- **No structure**: Going straight to a single number without decomposition
- **Mystery numbers**: Using figures without explaining the reasoning behind them
- **No sanity check**: Stating a final answer without checking if it's reasonable
- **False precision**: Calculating to 3 decimal places with rough assumptions
- **Silent math**: Doing calculations in their head without explaining to the interviewer
- **Off-by-order-of-magnitude**: Final answer is wildly unreasonable and not caught

---

## Score Benchmarks

| Score | Meaning |
|---|---|
| 9–10 | Excellent: clear structure, reasonable assumptions, sanity check, fluent with numbers |
| 7–8 | Good: solid structure and assumptions, minor arithmetic issues or weaker sanity check |
| 5–6 | Borderline: some structure but key steps missing (no sanity check, unclear assumptions) |
| 3–4 | Weak: jumps to a number, assumptions unstated or unreasonable |
| 0–2 | Not ready: no framework, confused by the problem, gives an obviously wrong answer |
