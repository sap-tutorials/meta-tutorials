---
parser: v2
auto_validation: true
primary_tag: tutorial>intermediate
tags: [tutorial>intermediate, software-product>sap-business-technology-platform]
time: 10
author_name: Thomas Jung
author_profile: https://github.com/jung-thomas
---

# Use AI-graded [VALIDATE_N] for free-text answers
<!-- description -->Ask open-ended questions; an LLM grades for correctness, not exact match.

## You will learn
- When free-text grading beats multiple choice
- How to write the `[VALIDATE_N]` text-style block
- How to author a `###Match` benchmark the AI can grade against
- What "good" criteria look like and what mistakes to avoid

## Prerequisites
- A tutorial repo under `sap-tutorials` with a matching `*-Contribution` sibling
- Familiarity with the basic `rules.vr` format
- An admin who can flip `ChatSettings.aiTextGraderEnabled` if needed

---

### Why free-text grading?

Multiple-choice questions are quick to author and quick to grade, but they have a teaching ceiling: every distractor is a hint, the reader can guess, and you can't ask "explain in your own words." Free-text plus AI grading lets you ask the questions you actually care about — *what's the difference between a group and a mission? Why would you pick CDS over raw SQL? When is a draft-enabled entity the right choice?* — and grade the spirit of the answer instead of the exact phrasing.

The trade-off is that free-text grading is non-deterministic. Two readers who write equally good answers might score slightly differently, and the AI's judgment isn't infallible. In practice that's fine for *learning* feedback (which is what tutorial validation is for); it would not be fine for *certification* grading (which this is not).

Use free-text when you want the reader to demonstrate understanding in prose. Use MCQ when there's a single correct factual answer and you want fast, deterministic feedback. Use [Tutorial 1's CODECHECK](../use-codecheck-to-ai-grade-reader-code/) when the answer is code.

### The [VALIDATE_N] text-style block

The directive is the same `[VALIDATE_N]` you'd use for MCQ — what makes it free-text is the `###Rule: text` line plus an explicit `###Grading: ai-judged` opt-in. The number after `VALIDATE_` matches the H3 step it applies to.

```text
[VALIDATE_4]
###Rule
text
###Question
In your own words, what's the difference between a group and a mission in the tutorial system?
###Match
A group is an ordered list of tutorials. A mission is one or more groups arranged into a learning journey, optionally with checkpoint steps and prizes.
###Grading
ai-judged
[VALIDATE_4]
```

Each section earns its keep:

- `###Rule: text` declares this is a free-text question. The alternative is `single-choice` (MCQ) or `multiple-choice` (multi-select); without `text`, the platform expects choices.
- `###Question` is what the reader sees. Phrase it as you would in a conversation. "In your own words" cues the reader that there isn't one right phrasing.
- `###Match` is the benchmark the AI grader compares the reader's answer to. Write it as plain prose, covering the key concepts you want the reader to demonstrate. Don't write a strict definition; write the gist.
- `###Grading: ai-judged` is the explicit opt-in. Without it, a `text`-style block falls through to legacy regex matching against `###Match` — which almost never does what you want for prose.

### Worked example: explaining "group" vs "mission"

The block above is a real example. Let's pick it apart.

The question is open-ended on purpose. We don't want the reader to recite a textbook definition; we want them to articulate the relationship between two platform concepts in their own words. That's a higher-bar learning outcome than picking the right phrase from four options.

The `###Match` text covers two concepts: that a group is an ordered list of tutorials, and that a mission is composed of one or more groups (with optional checkpoints and prizes). The AI grader scores the reader's answer for *coverage of those concepts* — not literal phrasing. A reader who writes "a group is a sequence of tutorials in a fixed order; a mission strings together groups into a longer journey, sometimes with rewards at the end" will pass with high marks. A reader who writes "they're both collections" will not — the answer doesn't distinguish the two.

The `###Match` is plain prose, not bullet points and not a strict definition. That's the format the AI grader works best with: it's looking for *concept coverage*, not literal-string overlap. If you write your match as a strict definition, the AI will tend to be too lenient (it'll pass anything that uses the same words) or too strict (it'll fail answers that paraphrase). Plain prose with multiple acceptable concepts hits the right balance.

### Try it yourself

Scroll to the input area below this step's body. Write your answer to the question — *in your own words, what's the difference between a group and a mission in the tutorial system?* — and submit. The AI grader compares your answer to the benchmark above and returns a pass/fail with feedback on what concepts you covered and what (if anything) was missing.

The companion `rules.vr` for this tutorial wires step 4 to AI grading via the block shown earlier. As with CODECHECK, the reference benchmark never appears in the reader's view; only the AI grader sees it.

### What good criteria look like

The `###Match` is the lever you have over grading quality. After authoring a few of these, the patterns become clear.

**Do** list multiple acceptable concepts in plain prose. The AI is looking for *coverage*, so the more concepts you mention, the more nuance the grader can score. "A group is an ordered list of tutorials. A mission is one or more groups arranged into a learning journey, optionally with checkpoint steps and prizes." covers four ideas: ordering, composition, journey-shape, optional rewards.

**Do** let the AI judge spirit rather than literal phrasing. A reader who writes "missions wrap groups" is saying the same thing as "a mission is composed of groups" — the grader recognizes that.

**Don't** expect exact wording. If your match is "A group is an ordered sequence of tutorials" and the reader writes "a group is a list of tutorials in order," the AI will pass — that's the point. If you find yourself wishing the grader were stricter about wording, you probably want MCQ or CODECHECK instead.

**Don't** be too vague. A `###Match` of "anything about groups and missions" tells the AI to pass any answer that mentions either word. Readers who write "groups are cool, missions are cooler" will pass. Be specific enough that wrong answers fail.

For code answers (where the right shape is "this function signature" not "this prose"), see [Tutorial 1 — Use CODECHECK_N](../use-codecheck-to-ai-grade-reader-code/). For build-time auto-generation of *both* MCQ and free-text questions, see [Tutorial 3 — Use AUTOAUTHOR](../use-autoauthor-to-generate-quiz-questions/). For a survey of all the non-AI quiz formats (MCQ, multi-select, sequence, regex), see [Tutorial 4 — the Cookbook](../tutorial-platform-feature-cookbook/).
