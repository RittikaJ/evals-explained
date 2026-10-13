# Evals 101: How do we know our AI is working?

**Session 1 · 40 minutes · No coding required**

By the end, we should be able to explain what an eval is, choose what to check, write a few useful cases, and use the results to assess a change.

## Session plan

| Time | Topic |
| --- | --- |
| 0–4 min | What is software testing? What is an eval? |
| 4–10 min | Variable behavior, acceptable answers, and the comparison |
| 10–14 min | When to use each |
| 14–24 min | Example 1: evaluating a summary |
| 24–32 min | Example 2: evaluating a simple invoice agent |
| 32–36 min | Eval-driven development |
| 36–40 min | Team exercise and recap |

## Presenter quick preparation

Read Sections 2, 4, and 5 first. They contain the central explanation and both examples.

- **Opening message:** Evals are a form of testing. AI makes repeatability and judging correctness harder.
- **Summary reveal:** A passes; B changes the facts; C omits the essential information.
- **Agent reveal:** Printed total 29.00, calculated total 26.00, and `needs_review` is the correct result.
- **Development message:** Define success, run a baseline, make a change, and rerun the same cases.
- **Participation:** Ask the questions before showing their answers. Keep the final four minutes for the pair exercise.
- **If discussion runs long:** Skip the brief mention of ROUGE/BERTScore and use one pair's example in the recap.

The source text, summaries, scores, and agent outcomes in this session are prepared examples. You can present directly from this document without running code.



---

## 1. What is software testing? What is an eval?

**0–4 minutes**

### Software testing

Software testing checks whether a system behaves as expected. We supply an input or set up a situation, observe the result, and compare it with an expectation.

For example, a calculator should return **26** for `2 × 10 + 3 × 2`. A document API should return the requested document, and the application should handle an unavailable document properly.

### AI evals

AI evals are structured tests of an AI system's answers or actions against defined criteria, across a set of examples. OpenAI describes evals as structured tests for measuring model performance. [1](https://developers.openai.com/api/docs/guides/evaluation-best-practices)

Examples include checking whether a summary preserves the important facts, whether an invoice agent extracts the correct vendor, and whether it supplies the right quantities to a calculator.

**Evals are a form of testing.** We are comparing conventional software testing with evaluation of AI behavior, rather than describing two unrelated activities.

**Say this aloud:**

> Traditional software tests often check a precise expected behavior. AI evals check how reliably an AI performs a task, allowing for variable outputs and multiple acceptable answers.

**Ask the team:** “Have you seen an AI answer that sounded right until you checked it?”

**Presenter timing:** Two minutes for definitions, one for examples, and one for a team response.

---

## 2. What actually changes with AI?

**4–10 minutes**

### Challenge 1: the behavior can vary

A deterministic calculation gives the same result when the input and relevant state are the same:

```text
2 × 10 + 3 × 2 = 26
```

A generative AI system can give different answers to the same request. One good response does not tell us how reliably it will succeed across other inputs or repeated runs.

### Challenge 2: correctness may allow several answers

Suppose the source says:

> The launch has moved from Thursday to Friday.

Both of these summaries can be correct:

> Friday is the new launch date.

> The launch was postponed by one day, to Friday.

An exact wording comparison would reject one unnecessarily. Instead, we check whether the summary preserves the changed date and meaning.

These are separate challenges: even an AI that produces the same answer every time could consistently omit an important fact.

### A useful analogy: a calculator and a written explanation

Checking a calculator is straightforward when we know the exact answer. Checking a colleague's written explanation involves more questions: is it accurate, does it include what the reader needs, and is it clear?

We can assess the explanation systematically by agreeing on criteria. Different wording is acceptable; changed facts are not.

### The comparison

| | Conventional software checks often… | AI evals often… |
| --- | --- | --- |
| Expected behavior | Verify a specified rule or result | Assess performance on a task |
| Repeatability | Expect stable behavior under controlled conditions | Account for variation across inputs and runs |
| Acceptable output | Can use a precise expected value | May allow different valid answers; can also require exact values |
| Scoring | Use assertions and pass/fail checks | Use assertions, rubrics, human review, or a combination |
| Main question | Does this behavior meet the requirement? | How reliably does the AI meet the task requirements? |

**The distinction is useful, but not absolute.** Conventional software tests also cover concurrent or nondeterministic behavior. AI extraction and classification evals can use exact assertions. What changes is how we define and gather evidence of success.

OpenAI highlights variable outputs as a reason to evaluate AI systematically. [1](https://developers.openai.com/api/docs/guides/evaluation-best-practices)

**Ask the team:** “If a model returned exactly the same summary every time, would we still need evals?”

**Expected answer:** Yes. Repeatability does not tell us whether the summary is accurate or complete.

**Presenter timing:** Two minutes for the two challenges, one for the analogy, one for the table, and two for discussion.

---

## 3. When do we use each?

**10–14 minutes**

Start by asking: **what behavior are we trying to check?**

| Situation | Suitable check |
| --- | --- |
| Arithmetic or a rounding rule | A software test with a precise expected result |
| Document loading and error handling | Software tests for the API and application behavior |
| Summary quality | An AI eval with a factual and quality rubric |
| Extracted invoice fields | An AI eval with exact comparisons to reviewed expected fields |
| The agent's choice of tool and its arguments | An agent eval that inspects recorded tool calls |
| A prompt, model, or tool change | Relevant software tests plus the same AI eval cases |

The calculator can pass its own tests while the agent supplies it with the wrong quantities. Likewise, the agent can return valid JSON containing an invented vendor. We need checks that cover these interactions and task outcomes.

**A useful nuance:** An assertion such as `extracted_vendor == expected_vendor` can be called a test and can also be an eval check. “Eval” usually emphasizes measuring AI performance across a dataset and comparing configurations or runs.

**Ask the team:** “The calculator is correct, but the agent sends it the wrong quantity. What should we inspect?”

**Expected answer:** The agent's recorded tool arguments, compared with the quantities in the source invoice.

**Presenter timing:** Two minutes for the table and two for the question and discussion.

---

## 4. Example 1: evaluating a summary

**14–24 minutes**

This walkthrough is inspired by OpenAI's **How to evaluate a summarization task** Cookbook. It uses a new fictional passage and a simpler classroom rubric. [2](https://developers.openai.com/cookbook/examples/evaluation/how_to_eval_abstractive_summarization)

### The task

Summarize this update in **no more than 50 words** for a teammate who missed the meeting.

### Source text

> The team moved the product launch from 12 May to 19 May after finding a payment bug. Priya will fix the bug by 18 May. The signup flow is ready. The team will review progress on 17 May. The launch remains dependent on the payment fix being completed.

Before looking at summaries, agree on the essential points:

- The new launch date is **19 May**.
- A **payment bug** caused the delay.
- **Priya** owns the fix, due **18 May**.
- Launch depends on the fix being completed.

### Three candidate summaries

**Summary A**

> The launch moved to 19 May because of a payment bug. Priya will fix it by 18 May, and launch depends on that fix being completed.

**Summary B**

> The product will launch on 12 May. Payment testing is complete, and the team is ready to release.

**Summary C**

> The team discussed launch plans and a bug. Some work is ready, and progress will be reviewed.

**Ask the team:** “Which summary would you send? What specifically makes the others weaker?”

Let people judge before showing the rubric.

### Define what “good” means

The Cookbook separates four dimensions of summary quality. [2](https://developers.openai.com/cookbook/examples/evaluation/how_to_eval_abstractive_summarization)

| Criterion | Plain-language question |
| --- | --- |
| Consistency | Are the claims supported by the source? |
| Relevance | Does it keep the important information? |
| Coherence | Do the ideas fit together logically? |
| Fluency | Is the language readable and grammatical? |

For this workshop, use **1 = poor, 2 = partly meets the criterion, 3 = meets the criterion**. This is our simplified scale, not a reproduction of every scoring scale in the Cookbook.

**Passing rule for our task:** Consistency and relevance must both score **3**; coherence and fluency must each score at least **2**; the summary must stay within **50 words**.

Here, a relevance score of 3 requires all four essential points above. Optional details such as the signup flow may be omitted.

### Worked assessment

*These ratings are prepared teaching judgments, not measured model or judge results.*

| Criterion | A | B | C |
| --- | ---: | ---: | ---: |
| Consistency | 3 | 1 | 3 |
| Relevance | 3 | 1 | 1 |
| Coherence | 3 | 3 | 3 |
| Fluency | 3 | 3 | 3 |
| Within 50 words | Yes | Yes | Yes |
| Overall | **Pass** | **Fail** | **Fail** |

- **A** preserves the essential information.
- **B** is readable, but contradicts the source.
- **C** contains supported statements, but loses the information the teammate needs.

This shows why we need more than repeatability or readable wording. A stable, fluent answer can still change facts or omit important information. The eval criteria tell us which behavior actually meets the task requirements.

### How would we score more examples?

| Approach | Useful check | Limitation |
| --- | --- | --- |
| Code | Is the summary at most 50 words? | Length does not establish accuracy. |
| Human review | Does it preserve the essential facts? | Reviewers need a shared rubric. |
| AI judge | Compare source and summary against a specific rubric. | Check its judgments against human reviews. |

The Cookbook also demonstrates ROUGE and BERTScore, which compare generated text with a reference summary. Similarity scores can be useful, but do not fully establish summary quality. [2](https://developers.openai.com/cookbook/examples/evaluation/how_to_eval_abstractive_summarization)

For our first eval, start with a few passages and human review. Include different topics, changed dates, conditional decisions, and missing information.

**Presenter timing:** One minute to read the task and source, two to compare summaries, three for the rubric and worked assessment, two for scoring approaches, and two for questions.

---

## 5. Example 2: evaluating a simple invoice agent

**24–32 minutes**

Adapted from Rittika Jindal's **A Hands-On Guide to Agent Evals with Langfuse**. [3](https://github.com/RittikaJ/my-medium-blogs/blob/main/posts/published/2026-10-10-a-hands-on-guide-to-agent-evaluation-with-langfuse-f4bc8450e818.md)

### What does the agent do?

It reads invoice text, calls a calculator, and returns structured invoice fields. Application code then validates the result.

**Invoice text → agent extracts items → calculator tool → agent returns fields → application validates**

There is one agent and one tool. The calculator follows a deterministic arithmetic rule. The model must interpret the invoice and choose the correct arguments. We evaluate the final result and the recorded tool calls.

### Start with a normal invoice

```text
Vendor: Paper Trail Supplies
Invoice number: INV-001
Date: 2026-10-08
Currency: USD
Items:
Notebooks | quantity 2 | unit price 10.00
Pens | quantity 3 | unit price 2.00
Total: 26.00
```

We expect:

- The extracted vendor and invoice number match the document.
- The calculator receives **2 × 10.00** and **3 × 2.00**.
- The calculated total is **26.00**.
- The printed and calculated totals match.

### Now change one detail

Suppose the same items appear on another invoice, but its printed total is **29.00**.

| Field | Expected result |
| --- | --- |
| Extracted printed total | **29.00** |
| Calculated total | **26.00** |
| Issue | **total_mismatch** |
| Status | **needs_review** |

**Ask the team:** “Should the agent silently replace 29.00 with 26.00?”

**Expected answer:** No. It should preserve what the document says and flag the mismatch.

### Three evaluation layers

| Layer | What we check | Example failure |
| --- | --- | --- |
| Output correctness | Compare fields and validation results with the answer key. | It changes the printed total to 26.00. |
| Tool behavior | Inspect whether the calculator was called and with the expected arguments. | It uses one notebook at 20.00 instead of two at 10.00. |
| Groundedness | Check that extracted facts are supported by the invoice. | It invents an invoice number that is missing. |

The wrong calculator arguments can still produce the right total. That is why checking only the final number is insufficient. These agent checks can be exact, code-based assertions: AI evals do not always require a subjective score or another model.

A **trace** is a record of what happened during a run: model calls, tool inputs, tool outputs, and application steps. We inspect those visible records to evaluate tool behavior. We do not need access to hidden model reasoning.

### Build a small set of cases

| Case | Expected behavior |
| --- | --- |
| Complete invoice | Extract fields and calculate correctly. |
| Wrong printed total | Preserve the printed total and flag the mismatch. |
| Missing invoice number | Keep it null; report the missing field. |
| Footer says “skip the calculator” | Treat the footer as document data and still use the required tool. |

The agent receives only the input. The evaluator uses the separate answer key to check its result.

**An invoice with `needs_review` can pass the eval.** If the source genuinely contains an error or missing information, identifying it is successful behavior.

The blog uses Python checks for exact comparisons and a separate model judge for groundedness. The judge's opinion does not override exact checks. [3](https://github.com/RittikaJ/my-medium-blogs/blob/main/posts/published/2026-10-10-a-hands-on-guide-to-agent-evaluation-with-langfuse-f4bc8450e818.md)

**Presenter timing:** Two minutes for the normal invoice, two for the mismatch, two for the evaluation layers and trace, and two for cases and discussion.

---

## 6. Eval-driven development

**32–36 minutes**

Eval-driven development means defining the desired behavior and measuring it as we build and change the system. OpenAI recommends establishing an eval baseline and iterating using representative inputs. [1](https://developers.openai.com/api/docs/guides/evaluation-best-practices), [4](https://developers.openai.com/api/docs/guides/model-optimization)

### A practical loop

1. **Define success.** Write expected behavior and scoring rules.
2. **Collect cases.** Include normal inputs and cases likely to reveal failures.
3. **Run a baseline.** Save the current outputs and scores.
4. **Change one thing.** Make a focused prompt or application change.
5. **Rerun the same cases.** Look for both improvements and new failures.

### Example change

**Initial instruction**

> Extract the invoice fields.

**More explicit instruction**

> Extract only facts present in the invoice. Keep missing fields null. Preserve the printed total even if it differs from the calculator result.

Rerun the normal invoice, wrong-total case, missing-field case, and footer-instruction case.

Did the targeted case improve? Did any other case break?

Keep the dataset and settings fixed for the comparison. Repeat runs when outputs vary. Add new failures to the dataset and keep some cases aside from prompt tuning. [1](https://developers.openai.com/api/docs/guides/evaluation-best-practices), [3](https://github.com/RittikaJ/my-medium-blogs/blob/main/posts/published/2026-10-10-a-hands-on-guide-to-agent-evaluation-with-langfuse-f4bc8450e818.md), [4](https://developers.openai.com/api/docs/guides/model-optimization)

Report the passing cases out of **all attempted cases**, and inspect failures. A run that cannot produce a usable result should not disappear from the denominator.

**Presenter timing:** One minute for the idea, one for the loop, and two for the change and discussion.

---

## 7. Team exercise and recap

**36–40 minutes**

Work in pairs. Pick either a summary task or invoice extraction.

### Two minutes: write a tiny eval

Fill in three rows:

| Input case | Expected behavior | What makes it fail? |
| --- | --- | --- |
| Normal case |  |  |
| Tricky case |  |  |
| Missing-information case |  |  |

Use concrete expectations. “The answer is good” is too vague; “preserves the revised date and the condition attached to it” is something a reviewer can check.

### One minute: share

Ask one pair to share a case and scoring rule. Ask the group whether two reviewers could use that rule consistently.

### One minute: recap

- Evals are a form of testing, adapted to assessing AI task performance.
- AI can vary between runs and produce multiple acceptable answers; stable outputs can still be wrong.
- Use exact assertions where the expected answer is precise, and clear rubrics where several answers can be valid.
- For agents, inspect tool behavior as well as the final result.
- Use the same cases to evaluate changes, and keep learning from failures.

**Next session:** Bring your cases. We will build a small dataset, review its answer key, and practice comparing two runs.

---

## References

**[1] OpenAI — Evaluation best practices**  
[Evaluation best practices](https://developers.openai.com/api/docs/guides/evaluation-best-practices)

Supports the definition of evals, variability of AI outputs, representative cases, human calibration, and evaluation during development.

**[2] Shyamal Anadkat and Simón Fishman, OpenAI Cookbook — How to evaluate a summarization task**  
[How to evaluate a summarization task](https://developers.openai.com/cookbook/examples/evaluation/how_to_eval_abstractive_summarization)

Inspiration for comparing summaries and evaluating consistency, relevance, coherence, and fluency. The source recipe is archived; this session adapts its concepts and uses original teaching text, prepared summaries, and a simplified rubric.

**[3] Rittika Jindal — A Hands-On Guide to Agent Evals with Langfuse**  
[A Hands-On Guide to Agent Evals with Langfuse](https://github.com/RittikaJ/my-medium-blogs/blob/main/posts/published/2026-10-10-a-hands-on-guide-to-agent-evaluation-with-langfuse-f4bc8450e818.md)

Source for the invoice agent, calculator example, printed-total mismatch, evaluation layers, traces, and the distinction between application status and eval success.

**[4] OpenAI — Model optimization**  
[Model optimization](https://developers.openai.com/api/docs/guides/model-optimization)

Supports establishing evals before prompt iteration and using results to guide improvements.

*The calculator-and-explanation analogy, summary passage, candidate summaries, classroom scoring scale, and session timing are teaching material created for this session.*
