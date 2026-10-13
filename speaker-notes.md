# Evals 101 — slide-by-slide speaker notes

**40 minutes · 14 slides · short bullet cues**

## Slide 1: How do we know our AI is working?

**0–0:30**

- Opening: confident AI answer → later found wrong
- Key idea: evals = testing; AI adds variation and judgment
- Today: two simple examples

## Slide 2: Table of contents

**0:30–1 min**

- Basics → definitions, differences, choosing checks
- Examples → summary quality, invoice agent behavior
- Finish → EDD and practical takeaways

## Slide 3: Testing and AI evals overlap.

**1–4 min**

- Software check → precise expected behavior; calculator = **26**
- AI eval → reliability across task examples
- Overlap → exact assertions possible in both
- Transition → what changes with AI?

## Slide 4: Two challenges come with AI.

**4–7 min**

- **Variation:** same request, different outputs → repeat cases
- **Judging correctness:** several acceptable answers → define criteria
- Stable output ≠ accurate or complete output
- Ask: same answer every time—enough to trust it? **No**

## Slide 5: Different words. Same correct meaning.

**7–10 min**

- Source fact: **Thursday → Friday**
- Both summaries → same fact, different wording
- Click “Another valid phrasing”
- Main point → meaning and required facts, not preferred wording

## Slide 6: When do we use each?

**10–14 min**

- Ask first; expand each answer afterward
- Arithmetic / rounding → software test, exact value
- Missing document → loading and error-handling tests
- Summary → AI eval with factual/quality rubric
- Invoice fields → exact comparisons with reviewed answers
- Tool arguments → recorded calls against source
- Prompt/model change → rerun same evals + relevant tests
- Extra point → valid JSON can still contain invented facts

## Slide 7: Summarize this update.

**14–16 min**

- Task → teammate missed meeting; **≤50 words**
- Essentials → **19 May; payment bug; Priya by 18 May; launch conditional on fix**
- Signup status / review date → optional for this task
- Reader’s needs → what the summary must preserve

## Slide 8: Which summary would you send?

**16–19 min**

- Ask for a choice before revealing scores
- **A:** essential facts preserved
- **B:** wrong date + unsupported readiness claim
- **C:** truthful but too vague; missing essential details
- Key distinction → no false claims ≠ useful summary

## Slide 9: Define “good” before scoring.

**19–24 min**

- **Consistency:** facts supported; **relevance:** important content
- **Coherence:** logical flow; **fluency:** readable language
- Scale **1–3**; pass: consistency/relevance **3**, others **≥2**, **≤50 words**
- Reveal assessment → B and C fail despite readable wording
- Prepared teaching scores; reviewer disagreement → clarify rubric

## Slide 10: A simple invoice agent.

**24–26 min**

- One agent + one calculator tool
- Extraction → calculator → fields → application validation
- Two model steps → same agent
- Failure before arithmetic → incorrectly extracted quantities

## Slide 11: Is “needs review” a successful result?

**26–29 min**

- Ask which behavior the agent should choose
- Printed **29**; calculated **26**
- Correct choice → preserve both, flag **needs_review**
- Invoice mistake ≠ agent failure
- Silent correction → hides the source problem

## Slide 12: Check the result and the actions.

**29–32 min**

- **Output:** extracted fields match expectations
- **Tool:** quantities and prices match invoice
- **Source:** supported facts; missing number stays **null**
- Extra example: **1×20 vs 2×10** → same total, wrong arguments
- Trace → evidence of actions, beyond final answer

## Slide 13: Eval-driven development.

**32–38 min**

- Define success → cases → baseline → one change → rerun
- Example change → missing fields stay null; printed total preserved
- Compare normal, mismatch, and missing-field cases
- Improvement in target case **and** regressions elsewhere
- Fixed cases/settings; repeated runs when outputs vary
- Unseen cases → avoid tuning only for known examples

## Slide 14: Make “good” something we can check.

**38–40 min**

- Precise answers → exact checks; multiple valid answers → rubrics
- Agents → actions + final output
- Changes → same cases before/after; inspect failures
- Next session → small dataset + comparison of two runs

---

- Full explanations and citations: `evals-101-content.md`
