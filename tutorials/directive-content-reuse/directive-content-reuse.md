---
parser: v2
auto_validation: false
time: 20
tags: [ tutorial>beginner, topic>cloud ]
primary_tag: topic>cloud
author_name: SAP Developer Advocates
author_profile: https://github.com/sap-tutorials
testedWith:
  "@sap/cds": "10.x"
  node: "22"
requires:
  - hana-cloud-mission-trial-1
---

# Content Reuse and References
<!-- description --> Demonstrate the four content-reuse features available to tutorial authors: `::include` snippets with parameters, smart reference links (`:note`, `:help`, `:api`, `:tutorial`, `:since`), the `testedWith` panel, and `::requires` prerequisite cards.

## You will learn
- How to inline reusable snippet content with `::include[name]{params}`
- How to link to SAP Notes, Help Portal, APIs, and other tutorials with smart reference directives
- How to declare tested tool versions with `testedWith` frontmatter
- How to render a prerequisite card with `::requires`

## Prerequisites
- Basic familiarity with the tutorial markdown structure (see [v2-example-tutorial](../v2-example-tutorial/v2-example-tutorial.md))

---

### Step 1: Prerequisites and Versions

This tutorial requires :tutorial[hana-cloud-mission-trial-1] to be completed first.

The tested environment is declared in the `testedWith` frontmatter above — you will see a collapsible "Tested with" panel at the top of the rendered tutorial.

### Step 2: Inline Snippets with Parameters

Reusable snippets let you share common setup steps across tutorials. The snippet below is fetched from the `_snippets/` directory at the root of the source repository.

::include[btp-trial-setup]{region=eu10}

> The `{region=eu10}` parameter substitutes `{{param:region}}` in the snippet with `eu10`.

### Step 3: Smart Reference Links

Reference SAP resources inline without hard-coding URLs:

- SAP Note: :note[2471802] — links to the SAP Note on me.sap.com
- Help Portal: :help[cap/docs/about-cap] — links to help.sap.com/docs/cap/docs/about-cap
- API reference: :api[cds.ql.SELECT]{lib=cap-node} — links to the CAP Node.js API docs
- Tutorial cross-reference: :tutorial[hana-cloud-mission-trial-1] — links to another tutorial
- Version badge: :since[CAP 10] — renders a version tag

### Step 4: Prerequisite Cards

The `::requires` directive renders a card linking to a prerequisite tutorial.

::requires[hana-cloud-mission-trial-1]

When you are authenticated, the card shows your completion status (Completed, In progress, or Start).

### Step 5: Summary

You have seen how to:

1. Inline reusable snippet content with `::include[name]{params}`
2. Link to SAP Notes, Help Portal, APIs, and other tutorials using `:note`, `:help`, `:api`, `:tutorial`
3. Declare tested tool versions with `testedWith` frontmatter
4. Show prerequisite cards with `::requires`
