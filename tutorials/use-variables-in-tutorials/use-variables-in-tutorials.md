---
parser: v2
auto_validation: false
primary_tag: tutorial>intermediate
tags: [tutorial>intermediate, software-product>sap-business-technology-platform]
time: 15
author_name: Thomas Jung
author_profile: https://github.com/jung-thomas
variables:
  - name: groupNumber
    label: Your workshop group number
    type: string
    maxLength: 3
    required: true
    placeholder: XXX
---

# Use Variables to Personalize Your Tutorials
<!-- description -->Let readers type a per-person value once — a workshop group number, a seat, a project suffix — and have it substituted live into every prose line, inline-code snippet, and copy-pasteable code block.

> **This tutorial is self-demonstrating.** It declares one variable, `groupNumber`, and uses it throughout. Type a value into the **Your workshop group number** field at the top of the page and watch every occurrence below — including inside the code blocks — update live as you read.

## You will learn
- Why tutorial variables exist and when to reach for them
- How to declare a `variables:` block in your source frontmatter
- How to reference a variable with a `{{var:<name>}}` token in prose, inline code, and fenced code blocks
- How to pre-fill a variable from the reader's identity with a `default` regex
- How values persist per-reader and carry across the tutorials in a mission

## Prerequisites
- Familiarity with the basic tutorial markdown structure (see [writing-tutorials.md](https://github.com/sap-tutorials/tutorials-ims/blob/main/docs/authors/writing-tutorials.md))
- A tutorial source repo under the `sap-tutorials` org

---

### Understand the problem variables solve

Workshop and hands-on tutorials often contain a value that is **different for every reader** — most commonly a group or seat number handed out at the start of a session. The old approach was a manual placeholder like `XXX` that the reader was told to find-and-replace everywhere:

> Create a space named `dev-XXX`, then a user `da164-XXX@education.cloud.sap`, then a project `product-advisor-XXX`.

The reader has to remember to swap `XXX` in **every** copy-pasted command. Miss one and the exercise breaks — and the mistake usually surfaces several steps later, where it is hardest to debug.

Tutorial variables fix this. The author declares the value **once**, the reader types it **once** into a field at the top of the page, and every marked occurrence — prose, inline code, and fenced code blocks alike — updates **live**. The reader copy-pastes commands with their own value already baked in, with nothing to remember.

> **When *not* to use a variable:** values that are the same for every reader (a fixed service URL, a product name) are just prose — don't wrap them. Variables are specifically for the per-reader value.

### Declare a variable in frontmatter

Add a `variables:` key to your tutorial's frontmatter. Each entry is one variable. This tutorial's own declaration looks like this:

```yaml
variables:
  - name: groupNumber              # identifier + the body-token key; must be unique
    label: Your workshop group number   # the field label the reader sees
    type: string                   # string | number
    maxLength: 3                   # optional: caps input length + drives validation
    required: true                 # optional: marks the field required
    placeholder: XXX               # optional: shown until the reader types a value
```

Field rules:

- **`name`** (required) — must match `[A-Za-z][A-Za-z0-9_]*` and be unique within the tutorial. This is also the key you use in body tokens.
- **`label`** (required) — a non-empty string; the prompt shown above the input field.
- **`type`** (required) — `string` or `number`. Nothing else is valid.
- **`maxLength`**, **`required`**, **`placeholder`** — all optional. `placeholder` is what every unfilled occurrence shows before the reader types anything (your call — `XXX`, a sample like `001`, or anything else).

A malformed `variables:` block is a **hard build failure** — a bad declaration can never reach readers. The build fails on a duplicate `name`, an invalid `type`, a `default.from` off the allowlist, an uncompilable `default.pattern`, or a body token that references a variable you never declared. (A declared-but-unused variable is allowed — it is only a soft warning.)

### Reference the variable in your content

Write `{{var:<name>}}` (using your declared variable's `name`) anywhere you want the value substituted. The same token works in three contexts — this step uses all three with our `groupNumber` variable, so change the field at the top of the page and watch each one update.

**In prose:** your workshop group number is {{var:groupNumber}}, and it appears inline in this sentence.

**In inline code:** your space is named `dev-{{var:groupNumber}}` and your user is `da164-{{var:groupNumber}}@education.cloud.sap`.

**In a fenced code block** — the whole point, so the reader can copy-paste a ready-to-run command:

```bash
cf create-space dev-{{var:groupNumber}}
cf create-user da164-{{var:groupNumber}}@education.cloud.sap
cf set-space-role da164-{{var:groupNumber}}@education.cloud.sap <org> dev-{{var:groupNumber}} SpaceDeveloper
```

The token is intentionally neutral:

- It renders as literal text on GitHub and reads cleanly to anyone (or any AI) looking at the raw markdown source.
- The `var:` prefix keeps it clear of Hugo shortcode delimiters, so the build leaves it alone until the substitution step.
- It behaves identically inside and outside code fences — you never need a different syntax for code.

> **Tip:** build the token right into the surrounding text, e.g. `dev-{{var:groupNumber}}`, not `dev- {{var:groupNumber}}`. The substitution replaces exactly the token, so no stray spaces end up in a copied command.

### Pre-fill a value from the reader's identity (optional)

Sometimes the per-reader value can be **derived** rather than typed — for example, when the group number is already embedded in the reader's logged-in email (`da164-007@education.cloud.sap`). Add an optional `default` block to pull it in automatically:

```yaml
variables:
  - name: groupNumber
    label: Your workshop group number
    type: string
    maxLength: 3
    placeholder: XXX
    default:
      from: email                  # one of: email | id | givenName | familyName
      pattern: 'da164-(\d+)@'      # capture group 1 becomes the default value
```

How it behaves:

- On first load, if the reader has no stored value yet, the platform evaluates `pattern` against the named identity field and pre-fills **capture group 1**. For `da164-007@education.cloud.sap` the field starts at `007`.
- `from` must be one of the allowlist — `email`, `id`, `givenName`, `familyName` — read from the reader's existing session. Anything else is a build failure.
- The field **always renders and is always overridable** — the default is a convenience, not a lock. If the reader is anonymous, or the pattern doesn't match, the default simply doesn't fire and the `placeholder` shows instead.

Leave `default` off entirely (as this tutorial does) when the value can't be derived and the reader should just type it.

### Know how values persist and carry over

The reader's entered value is stored **client-side only** — there is no server-side per-user rendering and nothing is persisted on the server. That keeps the feature privacy-clean: the same built HTML is served to everyone, and substitution happens in the reader's browser.

Two things follow from that, both worth telling your readers:

- **Values survive a browser restart.** A reader who types `007` and comes back tomorrow still sees `007`.
- **Values carry across a mission.** If `groupNumber` is used in several tutorials in the same group or mission, the value typed in the first one is already filled in the rest. Carry-over is **by variable name within the mission context** — so use the *same* `name` across a mission's tutorials when you mean the same value, and a different name when you don't.

> **Authoring consequence:** because carry-over keys on the variable `name`, keep names consistent across a mission (`groupNumber` everywhere, not `group` in one tutorial and `groupNum` in the next) or readers will be prompted again.

### Verify your tutorial before you publish

Variables ride through the normal author workflow — no special publish step. Before you open a PR:

1. **Preview on the QA channel.** Push to your `-Contribution` repo and open the tutorial at `/tutorials-qa/<your-slug>`. Confirm the field appears at the top, and that typing a value updates prose, inline code, **and** the fenced code blocks.
2. **Copy-paste test.** Type a value, then copy a command out of a fenced block and confirm your value is baked in — this is the behavior that actually matters to a workshop attendee.
3. **Check the build didn't fail.** A malformed `variables:` block or a `{{var:<name>}}` token whose `<name>` you never declared fails the build loudly; a green build means your declarations and tokens are consistent.

For the full set of authoring features, see the [Tutorial Platform Feature Cookbook](../tutorial-platform-feature-cookbook/) and [writing-tutorials.md](https://github.com/sap-tutorials/tutorials-ims/blob/main/docs/authors/writing-tutorials.md).

### Test yourself

---
