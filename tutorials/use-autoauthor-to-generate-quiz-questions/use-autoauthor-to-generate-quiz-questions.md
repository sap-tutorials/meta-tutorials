---
parser: v2
auto_validation: true
primary_tag: tutorial>intermediate
tags: [tutorial>intermediate, software-product>sap-business-technology-platform]
time: 15
author_name: Thomas Jung
author_profile: https://github.com/jung-thomas
---

# Use [AUTOAUTHOR_*] to generate quiz questions at build time
<!-- description -->Tag a step in your rules.vr with [AUTOAUTHOR_N] and the build pipeline writes the quiz for you, with a per-tutorial cache so subsequent builds cost zero LLM calls.

## You will learn
- What `[AUTOAUTHOR_*]` does and when it runs
- The directive variants and their precedence
- How the build cap and per-tutorial cache protect your spend
- When to keep AI-generated questions and when to hand-author

## Prerequisites
- A tutorial repo under `sap-tutorials` with a matching `*-Contribution` sibling
- A platform admin who can set `AI_AUTHOR_ENABLED=true` on the build runner
- Familiarity with the basic `rules.vr` format

---

### What [AUTOAUTHOR_*] does

Single-sentence summary: the build pipeline calls an LLM with the step body and writes a `ValidationQuestion` for you.

This is build-time, not runtime. CODECHECK and the free-text grader (Tutorials 1 and 2) run when the *reader* submits an answer; AUTOAUTHOR runs when the *build* fetches your tutorial. By the time a reader sees the page, the question is already a regular `[VALIDATE_N]` block — they can't tell whether you wrote it or the LLM did.

Hand-authored `[VALIDATE_N]` blocks always win. AUTOAUTHOR only fills steps that *don't already have* a hand-authored question. That means you can sprinkle AUTOAUTHOR across a tutorial to get coverage, then hand-author the specific steps where you want a particular question, and the two coexist without conflict.

### The directive forms

The directive supports per-step and tutorial-wide forms, with optional type bias:

- `[AUTOAUTHOR_N]` — per-step, default mix (the LLM picks MCQ or free-text based on what fits the step body)
- `[AUTOAUTHOR_N:mcq]` — per-step, force MCQ
- `[AUTOAUTHOR_N:text]` — per-step, force free-text
- `[AUTOAUTHOR_ALL]` — tutorial-wide, default mix; applies to every step lacking a hand-authored question
- `[AUTOAUTHOR_ALL:mcq]` — tutorial-wide, force MCQ for every auto-authored step
- `[AUTOAUTHOR_ALL:text]` — tutorial-wide, force free-text for every auto-authored step

Precedence, highest to lowest: hand-authored `[VALIDATE_N]` > per-step `[AUTOAUTHOR_N]` > tutorial-wide `[AUTOAUTHOR_ALL]` > nothing. So if a tutorial has `[AUTOAUTHOR_ALL]` plus a hand-authored `[VALIDATE_3]` plus an `[AUTOAUTHOR_5:mcq]`: step 3 gets the hand-authored question, step 5 gets an LLM-generated MCQ, every other step gets an LLM-generated default-mix question.

### The build cap and cache

Spend protection has three layers.

**Default-OFF behind a build flag.** Nothing happens unless `AI_AUTHOR_ENABLED=true` is set on the build runner. In CI, this is gated by a workflow secret; locally, it's an env var. New tutorials don't accidentally generate questions; the platform team flips the flag for an authored opt-in.

**Per-tutorial content-hash cache.** After the first generation, the build writes `.tutorial-cache/<slug>.ai-quiz-cache.json` keyed by step content hash. Subsequent builds against unchanged steps cost zero LLM calls. Edit a step's body and only that one step regenerates; touch a `[CODECHECK_N]` block elsewhere and nothing else recomputes.

**Hard cap on calls per build.** The default is 200 LLM calls per build run, configurable via `AI_AUTHOR_BUILD_CAP`. If you somehow add `[AUTOAUTHOR_ALL]` to 500 tutorials at once, the build pipeline stops at 200 and logs the rest as "deferred." A subsequent build picks up where it left off.

**Cache-bust gotcha.** Switching the runtime model (e.g. upgrading from one LLM family to another) does *not* invalidate the cache automatically. Cached questions are model-specific only insofar as the model that wrote them; once they're in the cache, no model decision invalidates them. If you want to regenerate under a new model, manually delete `.tutorial-cache/<slug>.ai-quiz-cache.json` for the slugs you want to refresh.

For the first-time bulk-seed pass against a large catalog, use `npm run seed-ai-quizzes`. It raises the cap to 10000 for that single run and writes the cache so subsequent regular builds are fast.

### Demo: an auto-authored multiple-choice question

The current parser is V2 — it uses H3 headings (`###`) to delimit steps. Each H3 becomes a navigable step in the rendered tutorial. The previous parser, V1, used `[ACCORDION-BEGIN]` / `[ACCORDION-END]` markers to delimit steps. V1 is still supported for legacy content but is not used for new tutorials.

After this build runs with `AI_AUTHOR_ENABLED=true`, you'll see a multiple-choice question below this paragraph generated from this body. The directive that produced it lives in our companion `rules.vr`:

```text
[AUTOAUTHOR_4:mcq]
```

If the build runs without the flag, no question appears here — the directive is silently inert, exactly like a CODECHECK with `codeCheckEnabled` off.

### Demo: an auto-authored free-text question

Authors can opt into AI question generation per-step (`[AUTOAUTHOR_N]`) or tutorial-wide (`[AUTOAUTHOR_ALL]`). A type suffix (`:mcq` or `:text`) biases the output. Hand-authored `[VALIDATE_N]` blocks always take precedence — the AI generator skips any step that already has a hand-authored question.

The directive for this step is `[AUTOAUTHOR_5:text]`, so the AI generates a free-text question. The free-text grader from [Tutorial 2](../use-validate-to-ai-grade-free-text-answers/) then grades the reader's answer at runtime.

![build-time AI quiz generation](001-build-time-generation.png)

### When to use AUTOAUTHOR vs hand-authored

AUTOAUTHOR shines for *coverage*. If you have a 20-step tutorial and you want every step to have a quiz but you don't want to spend an afternoon writing 20 questions, slap `[AUTOAUTHOR_ALL]` on it and the build does it for you. The questions are usually decent — not great, not bad — and the cache means once you accept them they're stable across builds.

Hand-authored is better when:

- You want to test a specific edge case the step body doesn't emphasize. The LLM grades by *what's in the step*, not what you wished the reader would notice.
- The step body is too short for the LLM to write a useful question. Two-sentence steps generate weak questions.
- The step is conceptual rather than instructional, and you want a particular phrasing the LLM won't reach for.

The pattern that scales: AUTOAUTHOR_ALL across the tutorial for default coverage, then hand-author `[VALIDATE_N]` for the three or four steps where you want a specific question. Hand-authored wins precedence, so you don't need to remove the AUTOAUTHOR_ALL — it just steps aside on those steps.

For runtime AI grading of code, see [Tutorial 1 — CODECHECK](../use-codecheck-to-ai-grade-reader-code/). For runtime AI grading of free-text answers, see [Tutorial 2 — VALIDATE with ai-judged](../use-validate-to-ai-grade-free-text-answers/). For non-AI quiz formats and other authoring features, see [Tutorial 4 — the Cookbook](../tutorial-platform-feature-cookbook/).
