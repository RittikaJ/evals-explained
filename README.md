# Evals explained

A practical introduction to AI evaluations, with an interactive presentation, worked examples, and speaker notes.

**[View the presentation →](https://rittikaj.github.io/evals-explained/)**

## About

This repository contains a **40-minute Evals 101 session** for teams getting started with LLM and agent evaluation. It explains how AI evals relate to software testing, what makes AI behavior harder to assess, and how to use evidence to improve an application.

The central idea: **evals are a form of testing**. AI introduces two challenges—outputs can vary, and several different answers may satisfy the task. The workshop shows how to define clear expectations and choose checks that match the behavior.

The material is designed for engineers, product teams, and anyone building or assessing AI features. You can follow the session without writing code.

## What the session covers

- **Software testing and AI evals:** shared goals, practical differences, and when to use each.
- **Evaluating LLM outputs:** factual consistency, relevance, coherence, and fluency.
- **Evaluating a simple agent:** output correctness, tool arguments, and support from the source.
- **Eval-driven development:** define success, run a baseline, make a focused change, and compare results.

## Two worked examples

### 1. Summary evaluation

Compare three summaries of the same update. One preserves the essential information, one changes the facts, and one leaves out what the reader needs.

A simple rubric makes those differences explicit and shows why readable text is not enough.

### 2. Invoice agent evaluation

Follow one agent that extracts invoice fields and uses a calculator. The items total **26.00**, but the invoice prints **29.00**.

The expected behavior is to preserve the printed value and flag the mismatch. A result of `needs_review` can therefore represent success. The example also shows why a correct final number does not prove that the agent supplied the right tool arguments.

*The presentation uses prepared teaching examples and scores; it does not run live model evaluations.*

## Using the presentation

Open the [live site](https://rittikaj.github.io/evals-explained/) or open `index.html` locally in a browser.

- Navigate the **14 slides** with the buttons or **← / →** keys.
- Use **Home / End** to jump to the first or last slide.
- Select a topic in the **table of contents** or use the slide picker.
- Expand the **Q&A answers** and summary assessment when ready.
- Try the invoice choices to reveal their evaluation.
- Switch between **light and dark themes**; your preference is saved in your browser.
- Use **Full screen** for presenting and **Sources** for references.

## Repository contents

| File | Purpose |
| --- | --- |
| [index.html](index.html) | Audience presentation, interactive visuals, navigation, and custom theme |
| [speaker-notes.md](speaker-notes.md) | Concise bullet cues matched to the 14 slides |
| [evals-101-content.md](evals-101-content.md) | Detailed background draft and references used to develop the session |
| [.nojekyll](.nojekyll) | Serves the static HTML directly through GitHub Pages |

Keep the speaker notes beside the presentation when facilitating. They are separate from the audience slides.

## Running locally and updating

```bash
git clone https://github.com/RittikaJ/evals-explained.git
cd evals-explained
```

Open `index.html` in your browser. The page is self-contained: no build step or package installation is required.

To adapt the workshop, edit the slides and theme in `index.html`, then update `speaker-notes.md` to match the slide order and examples.

GitHub Pages publishes from **`main` → `/ (root)`**. Pushing changes to `main` triggers a deployment.

## References

- [OpenAI — Evaluation best practices](https://developers.openai.com/api/docs/guides/evaluation-best-practices)
- [OpenAI Cookbook — How to evaluate a summarization task](https://developers.openai.com/cookbook/examples/evaluation/how_to_eval_abstractive_summarization)
- [Rittika Jindal — A Hands-On Guide to Agent Evals with Langfuse](https://github.com/RittikaJ/my-medium-blogs/blob/main/posts/published/2026-10-10-a-hands-on-guide-to-agent-evaluation-with-langfuse-f4bc8450e818.md)
- [OpenAI — Model optimization](https://developers.openai.com/api/docs/guides/model-optimization)

The summary passage, candidate summaries, and simplified scoring scale are teaching adaptations. Source links are also provided alongside the relevant presentation content.

## Next session

A hands-on follow-up is planned: evaluate LLM outputs and a simple agent, build a small dataset, score a baseline, and compare a change.

---

Created by [Rittika Jindal](https://github.com/RittikaJ).
