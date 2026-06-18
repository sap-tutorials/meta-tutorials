---
parser: v2
auto_validation: false
primary_tag: tutorial>intermediate
tags: [tutorial>intermediate, software-product>sap-business-technology-platform]
time: 20
author_name: Thomas Jung
author_profile: https://github.com/jung-thomas
---

# Tutorial Platform Feature Cookbook
<!-- description -->Six new authoring features at a glance, with copy-pasteable demos that you can crib for your own tutorials.

> **A note on maintenance:** this tutorial is a living example of the platform's authoring syntax. If you spot drift between the syntax shown here and what's documented in [writing-tutorials.md](https://github.com/sap-tutorials/tutorials-ims/blob/main/docs/authors/writing-tutorials.md), treat the docs as the source of truth and open an issue.

## You will learn
- How to use OS-conditional content blocks (with the global OS picker)
- How to use generic option blocks (per-step tabs)
- How to author branched tutorials with `[BRANCH_BEGIN ...]`
- How to mark a step as auto-skippable
- How to embed mermaid diagrams
- How codetabs work across multiple language fences
- How glossary tooltips appear on first SAP-acronym mentions
- How the lightbox image viewer works

## Prerequisites
- Familiarity with the basic tutorial markdown structure (see [writing-tutorials.md](https://github.com/sap-tutorials/tutorials-ims/blob/main/docs/authors/writing-tutorials.md))

---

### OS-conditional content

When a step's variants are about the operating system, the platform auto-detects this and wires every OS block on the page to a single global picker at the top of the tutorial. The reader picks their OS once, and that choice persists across tutorials.

The body of *this* step uses the syntax — change the OS picker at the top of the tutorial and the visible block below changes too:

[OPTION BEGIN [Windows]]
Open PowerShell and run `cd $HOME\projects`.
[OPTION END]

[OPTION BEGIN [Mac and Linux]]
Open a terminal and run `cd ~/projects`.
[OPTION END]

The platform recognizes Windows, macOS / Mac OS / Mac / OS X / Darwin, Linux / Ubuntu / Debian / Fedora / Unix, and BAS / Business Application Studio / SAP BAS as OS labels. Combined labels like `Mac and Linux` or `Mac & Linux` match both. For the full taxonomy and override syntax, see [writing-tutorials.md](https://github.com/sap-tutorials/tutorials-ims/blob/main/docs/authors/writing-tutorials.md) §3.5.2.

### Generic option blocks

When the variants are *not* about the OS — JSON vs XML, Java vs Node, Cloud vs On-premise — the platform renders them as per-step tabs that don't wire into the global picker. Each step's tabs are independent of every other step's tabs.

The block source looks identical to the OS form; the only difference is the labels. The platform's auto-detection differentiates OS vs generic blocks *by label content* — labels matching the OS taxonomy auto-wire to the global picker, any other labels render as per-step tabs.

[OPTION BEGIN [JSON]]
```json
{ "type": "object", "properties": { "id": { "type": "integer" } } }
```
[OPTION END]

[OPTION BEGIN [XML]]
```xml
<object><id type="integer"/></object>
```
[OPTION END]

If you have a tab that's *named* like an OS but isn't actually OS-conditional (e.g. a "Linux" tab that's about a Linux container product, not the operating system), use the `osOverrides` frontmatter key documented in [writing-tutorials.md](https://github.com/sap-tutorials/tutorials-ims/blob/main/docs/authors/writing-tutorials.md) §3.5.2 to force a step's group back to per-step tabs.

### Branched tutorials with [BRANCH_BEGIN ...]

When a tutorial has a fork that the reader picks — HANA vs PostgreSQL, Java vs Node, on-prem vs cloud — use BRANCH markers to define alternative step-runs. Unlike OPTION blocks (which split a single step into per-tab variants), BRANCH blocks split a *run* of subsequent content into pickable paths. See the full syntax reference at <https://github.com/sap-tutorials/tutorials-ims/blob/main/docs/authors/branched-tutorials.md>.

You'll deploy to either HANA Cloud or PostgreSQL. Pick the runtime your team uses.

[BRANCH_BEGIN group="deployment" key="hana" label="HANA Cloud"]

### Provision HANA Cloud
Open the BTP cockpit and create a HANA Cloud instance. Wait for the instance to reach `RUNNING` state, then download the binding to your local environment.

[BRANCH_END]

[BRANCH_BEGIN group="deployment" key="postgres" label="PostgreSQL"]

### Install PostgreSQL
`brew install postgresql@16` (macOS) or use the installer from postgresql.org (Windows/Linux). Start the service and create a local database `cap_dev` for your project.

[BRANCH_END]

Each branch starts a fresh sub-step; consecutive sibling BRANCH blocks with matching `group=` form one pickable group. The reader picks once per group, and their pick persists across page loads via local storage.

### Skip-runs with skipIf

When the learner has already completed prerequisite content elsewhere — finished a Node Getting Started tutorial earlier in the mission, say — you can mark a step as auto-skippable. The reader sees a *Skip this step* button and an explanation of why, but always has the option to read through anyway.

The syntax is a per-step HTML-comment frontmatter block:

```markdown
### Install Node.js

<!--
skipIf: "completed:node-getting-started"
skipLabel: "Already installed"
skipReason: "You completed the Node Getting Started tutorial earlier."
-->

Step body...
```

`skipIf` is a predicate against completion state; `skipLabel` is the text on the button; `skipReason` is the explanation text. Within a single tutorial there's no other step to refer to, so this cookbook can only show the syntax — full skip-run behavior requires multiple tutorials in a mission, where one tutorial's completion state can satisfy another's `skipIf`.

### Mermaid diagrams

Use the `mermaid` shortcode to embed flowcharts, sequence diagrams, ER diagrams, and so on. The platform renders them inline using the Horizon palette so they match the rest of your tutorial visually.

{{< mermaid >}}
flowchart LR
  Author -->|writes markdown| GitHub
  GitHub -->|repo dispatch| CI
  CI -->|fetch + build + publish| HANA
  HANA -->|serves HTML| Reader
{{< /mermaid >}}

The shortcode renders the diagram lazily — it's not loaded on first paint, so a tutorial without diagrams pays no cost. Tutorials with diagrams load the mermaid library only when the diagram scrolls into view.

### Codetabs (multi-language code blocks)

The platform syncs language selection across consecutive code fences with different language tags. Pick `js` once and the next code block in the same step also defaults to `js`; the reader's choice persists across the tutorial via local storage.

```js
// Define a CDS service handler in JavaScript
module.exports = (srv) => {
  srv.before('CREATE', 'Books', (req) => {
    if (!req.data.title) req.reject(400, 'Title required');
  });
};
```

```ts
// The same handler in TypeScript — picking 'js' above defaults this block to 'ts'
import cds from '@sap/cds';
export default (srv: cds.Service) => {
  srv.before('CREATE', 'Books', (req) => {
    if (!req.data.title) req.reject(400, 'Title required');
  });
};
```

```cds
// And the CDS model the handler operates on
entity Books : managed {
  key ID : Integer;
  title  : localized String(111);
}
```

The cross-block sync is keyed on language *kind* (e.g. all `js`/`ts` blocks share state). CDS blocks render with dedicated highlighting that won't sync with the JS/TS pair — they're independent.

### Glossary tooltips

The first mention of an SAP acronym (CDS, CAP, BTP, HANA, and a handful of others) auto-decorates with a popover. Hover the term and the reader sees a definition without leaving the page.

This sentence is the first time CDS, CAP, and BTP appear in this tutorial. Hover any of those three terms and you should see a small tooltip with the expansion and a one-line definition.

The auto-decoration is keyed on the *first* occurrence per page. Subsequent mentions are not decorated, both to keep the page from looking spammy and to nudge the reader toward learning the term — once they hover once, they shouldn't need to hover again.

### Lightbox on images

When you embed an image in a tutorial, like `![alt text](image.png)`, the platform automatically wraps it so a click opens it in a full-size lightbox dialog with zoom and pan controls. There's no shortcode and no syntax to remember — every embedded image gets the treatment.

Try it on any image in this tutorial set — for example, the screenshots in [Tutorial 1](../use-codecheck-to-ai-grade-reader-code/) or [Tutorial 2](../use-validate-to-ai-grade-free-text-answers/). Click any image; the lightbox opens with a deep-link URL you can share, plus zoom, pan, and gallery navigation if there are sibling images on the page. ESC closes it; the URL deep-link reopens it on a fresh page load.

The cookbook itself ships no images on purpose — the entire body is text demonstrating syntax — so the demo for this step lives in the screenshot-heavy sibling tutorials linked above.
