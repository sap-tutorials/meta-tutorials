---
parser: v2
auto_validation: true
primary_tag: tutorial>intermediate
tags: [tutorial>intermediate, software-product>sap-business-technology-platform]
time: 15
author_name: Thomas Jung
author_profile: https://github.com/jung-thomas
---

# Use [CODECHECK_N] to AI-grade your reader's code
<!-- description -->Add a single block to your rules.vr file and an LLM will grade pasted code against your reference solution — better than exact-match, kinder than no validation at all.

## You will learn
- What the `[CODECHECK_N]` directive does
- How to write the `rules.vr` block
- What good `###Goal` text and `###Hints` look like
- How to enable AI code-check on your tutorial

## Prerequisites
- A tutorial repo under `sap-tutorials` with a matching `*-Contribution` sibling
- Edit access to the contribution repo
- An admin who can flip `ChatSettings.codeCheckEnabled` if it's off

---

### What the [CODECHECK_N] directive does

When you tag a step with `[CODECHECK_N]`, the platform renders a paste-code text area at the bottom of that step. The reader writes their attempted solution, hits *Submit*, and an LLM grades the answer against a reference solution you supply. The grader returns inline feedback — what the reader got right, what's missing, and which hints to consider — without ever revealing the reference solution itself.

This sits between two extremes. Exact-string matching is too strict for real code (whitespace, variable names, equivalent constructs all vary). Skipping validation entirely teaches nothing. AI grading lands in the middle: the reader gets feedback shaped by your intent, not your literal characters.

The feature is gated by `ChatSettings.codeCheckEnabled`. If your platform admin has it off, the step renders normally and the paste area never appears. Readers see no broken UI; the directive is simply a no-op until enabled.

### The rules.vr block format

The directive lives in your tutorial's companion `rules.vr` file (in the matching `-Contribution` repo). The number after `CODECHECK_` matches the H3 step it applies to. Here's the canonical shape:

```text
[CODECHECK_3]
###Goal
A description of what the reader is supposed to write, in your own words. The AI uses this to grade. Be specific.

###Language
cds

###Hints
- First hint, shown progressively
- Second hint
- Third hint

###ReferenceSolution
entity Books : managed {
  key ID : Integer;
  title  : localized String(111);
}
```

Each section pulls its weight:

- `###Goal` is the rubric the AI grades against. Mention every required element. If you write "an entity called Books," the AI will pass anything that defines an entity called Books — even if you secretly wanted a `localized` title. Be explicit.
- `###Language` is a syntax-highlighting hint for the rendered code editor. Common values: `cds`, `js`, `ts`, `java`, `abap`, `sql`, `yaml`, `json`, `xml`, `bash`.
- `###Hints` is a bulleted list. The first hint shows immediately if the reader asks for help; subsequent hints unlock as they keep struggling. Order them general → specific.
- `###ReferenceSolution` is the canonical answer. Crucially, it's *never shown to the reader directly* — the AI uses it as the grading anchor. You can put your "ideal" solution here without leaking it.

This format is parsed by `scripts/parsers/codecheck.ts` in tutorials-ims. The trimmed spec (everything except the reference solution) ships in the Hugo frontmatter; the full spec lives only in HANA, away from the rendered HTML.

### A worked example

Suppose you want to teach tutorial frontmatter. Step 5 of your tutorial walks the reader through three fields: `parser: v2`, `auto_validation: true`, and a numeric `time`. You want to validate that they actually got all three.

In your contribution repo's `rules.vr`, you'd write:

```text
[CODECHECK_5]
###Goal
Write a YAML frontmatter block (between two --- fences) for a tutorial that sets parser to v2, sets auto_validation to true, and sets time to an integer number of minutes. All three fields must be present.

###Language
yaml

###Hints
- Frontmatter is delimited by --- fences at the very top of the file.
- All three fields are top-level YAML keys, not nested.
- The time field is an integer (no quotes, no units).

###ReferenceSolution
---
parser: v2
auto_validation: true
time: 15
---
```

Notice three authoring choices: the goal mentions all three required fields explicitly (otherwise the AI might pass a two-field answer); hints are ordered easy-to-specific so a reader who's almost there only needs the first hint; and the reference solution is just YAML — no commentary, no extra fields the goal didn't request. The AI compares the reader's answer to *the goal*, not the literal reference. The reference is the floor for grading, not the ceiling.

### Try it yourself

Below this paragraph you should see a paste-code area. Write a small CDS entity definition called `Books` that uses the `: managed` aspect, has a key field `ID : Integer`, and a localized string `title` field. Submit it; the AI will grade your answer against the spirit of the request — your variable names, formatting, and minor wording differences won't trip it up.

If the grader says you missed something, click *Show hint* for a nudge. After three hints, you'll have most of the answer; the AI never reveals the full reference solution.

The companion `rules.vr` for this tutorial (in `meta-tutorials-Contribution`) carries `[CODECHECK_4]` to wire up this step. The reader doesn't need to know that — when the step loads, the platform fetches the trimmed spec, renders the paste area, and routes submissions through `/api/codecheck` to grade.

### Enabling code-check on your tutorial

Two things have to be true for `[CODECHECK_N]` to actually grade reader submissions:

1. **`ChatSettings.codeCheckEnabled` must be `true`** in the platform's HANA chat-settings row. An admin flips this from `/admin-ui/#operations-display` (the *Joule Chat Settings* tile). When off, the directive is silently inert.
2. **Rate limits apply.** A reader can submit 5 attempts per step per 5 minutes, and 30 attempts per hour across all steps. These are per-user, IP-anchored. Most readers never hit the cap; the limits exist to protect against accidental tight-loop submissions.

Code-check shines for short, convergent snippets — define an entity, write a function signature, finish a YAML config. For "explain in your own words" prompts where there's no canonical code answer, see [Tutorial 2 — Use AI-graded VALIDATE_N for free-text answers](../use-validate-to-ai-grade-free-text-answers/). For build-time auto-generation of quiz questions, see [Tutorial 3 — Use AUTOAUTHOR](../use-autoauthor-to-generate-quiz-questions/).

For the admin-side toggle workflow and rate-limit operations, see the [Center Admin docs](https://github.com/sap-tutorials/tutorials-ims/blob/main/docs/authors/center-admin.md).
