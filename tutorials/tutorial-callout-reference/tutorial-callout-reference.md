---
parser: v2
auto_validation: false
primary_tag: tutorial>beginner
tags: [tutorial>beginner, software-product>sap-business-technology-platform]
time: 10
author_name: Thomas Jung
author_profile: https://github.com/jung-thomas
---

# Alert Callout Reference
<!-- description -->Every alert and blockquote style the platform renders, side by side in a single step, so you can see what each one looks like and pick the right emphasis for your own tutorials.

> **A note on maintenance:** this tutorial is a living example of the platform's alert syntax. If you spot drift between what renders here and what's documented in [writing-tutorials.md](https://github.com/sap-tutorials/tutorials-ims/blob/main/docs/authors/writing-tutorials.md) §"Alert callouts", treat what actually renders on this page as the source of truth and open an issue.

## You will learn
- Which GitHub-style alert designators the platform recognizes
- What color each alert renders in the Horizon theme
- When to reach for each one — by intent, not by loudness
- How a plain blockquote differs from a designated alert

## Prerequisites
- Familiarity with the basic tutorial markdown structure (see [writing-tutorials.md](https://github.com/sap-tutorials/tutorials-ims/blob/main/docs/authors/writing-tutorials.md))

---

### Every alert style at a glance

The platform recognizes [GitHub-style alerts](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#alerts): a blockquote whose **first line** is an alert designator in the form `> [!TYPE]`. The rest of the blockquote is the alert body. Rendering is **color-only** — the border and background change to a SAP MessageStrip color, but there is no icon and no heading label, so write a body that reads on its own.

Each block below shows the exact markdown followed by how it renders. Read this step on the published page to see the colors.

#### Plain blockquote (no designator) → blue (Information)

A blockquote with no designator is the default. Use it for an ordinary aside that doesn't need special emphasis.

```markdown
> This is a plain blockquote. It renders in the default Information blue.
```

> This is a plain blockquote. It renders in the default Information blue.

#### `> [!NOTE]` → blue (Information)

Use for supplementary information the reader should notice but that carries no risk. Renders the same blue as a plain blockquote, but makes your *intent* explicit in the source.

```markdown
> [!NOTE]
> Highlights information that users should take into account, even when skimming.
```

> [!NOTE]
> Highlights information that users should take into account, even when skimming.

#### `> [!TIP]` → green (Success)

Use for helpful advice, shortcuts, or a better way to do something. The green reads as "this is a good idea," not "danger."

```markdown
> [!TIP]
> Prefer the frontmatter `video:` field over an inline iframe — it gets the lightbox and responsive sizing for free.
```

> [!TIP]
> Prefer the frontmatter `video:` field over an inline iframe — it gets the lightbox and responsive sizing for free.

#### `> [!IMPORTANT]` → purple (Emphasis)

Use for information that is **crucial** to the reader succeeding — a key prerequisite, a value they must note now and reuse later. The purple is distinct from NOTE's blue so the two don't blur together.

```markdown
> [!IMPORTANT]
> Copy the service key's `clientid` now — you'll paste it into the destination configuration two steps from here.
```

> [!IMPORTANT]
> Copy the service key's `clientid` now — you'll paste it into the destination configuration two steps from here.

#### `> [!WARNING]` → amber (Warning)

Use for something that could cause a problem if the reader isn't careful — a destructive flag, an irreversible setting, a common foot-gun. Amber signals "slow down and read."

```markdown
> [!WARNING]
> Running `cds deploy --to hana` against a production binding will drop and recreate the schema. Confirm your target with `cf target` first.
```

> [!WARNING]
> Running `cds deploy --to hana` against a production binding will drop and recreate the schema. Confirm your target with `cf target` first.

#### `> [!CAUTION]` → red (Error)

Reserve for genuine danger — data loss, security exposure, or an action that cannot be undone. Red is the loudest callout; using it for a mere tip trains readers to ignore it.

```markdown
> [!CAUTION]
> This command deletes every row in the `Progress` table and cannot be undone. There is no soft-delete and no backup unless you took one.
```

> [!CAUTION]
> This command deletes every row in the `Progress` table and cannot be undone. There is no soft-delete and no backup unless you took one.

#### Styled plain blockquote (lead-in bold)

If you want an emphasized aside *without* a semantic color — just a labeled note — a plain blockquote with a bold lead-in works and stays Information blue. This is the pattern used by the maintenance note at the top of this tutorial.

```markdown
> **Heads up:** a bold lead-in gives you a labeled aside without reaching for an alert color.
```

> **Heads up:** a bold lead-in gives you a labeled aside without reaching for an alert color.

### Quick reference

| You write | Rendered emphasis | Color |
|---|---|---|
| `> text` (no designator) | Information (default) | blue |
| `> [!NOTE]` | Information | blue |
| `> [!TIP]` | Success | green |
| `> [!IMPORTANT]` | Emphasis | purple |
| `> [!WARNING]` | Warning | amber |
| `> [!CAUTION]` | Error | red |

Two things to remember:

- **Pick by intent, not loudness.** An ordinary note can stay a plain `>` or `[!NOTE]` (blue). Save `[!WARNING]`/`[!CAUTION]` for real risk — if everything is a caution, nothing is.
- **The designator must be the first line**, on its own, inside the blockquote. An unknown designator (anything other than the five above) silently falls through and renders as a plain blue blockquote.
