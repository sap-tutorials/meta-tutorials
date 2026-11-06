---
parser: v2
auto_validation: false
time: 10
tags: [ tutorial>beginner, software-product>sap-business-technology-platform ]
primary_tag: software-product>sap-business-technology-platform
author_name: SAP Developer Advocates
author_profile: https://github.com/sap-tutorials
placeholders: auto
---

# Code Block Enhancements
<!-- description --> Demonstrate the five code-block enhancements: console blocks that separate prompts from output, diff markers, fill-in placeholders, file-tree fences, and `:::walkthrough` code-plus-notes.

## You will learn
- How console fences separate commands from output and copy commands only
- How diff markers (`// [!code ++]` / `--` / `focus` / `highlight`) annotate changes
- How `placeholders: auto` turns `<YOUR_…>` tokens into fillable chips
- How `tree` fences render project structure, and how `:::walkthrough` pairs code with notes

## Prerequisites

- A BTP trial account (you will need `<YOUR_SUBACCOUNT_ID>` below)
- Node.js 18+ installed

---

### Step 1: Console blocks — prompts and output separation

Console blocks separate command lines from output. Tag your fence `console`, `shell-session`, or `terminal-session`. Lines starting with `$`, `>`, or `PS>` are commands; all other lines are output. The copy button copies **commands only**, without prompts.

```console
$ cf login -a https://api.cf.eu10.hana.ondemand.com
API endpoint: https://api.cf.eu10.hana.ondemand.com

Email: user@example.com
Password: ••••••••

$ cf target -s dev
API endpoint: https://api.cf.eu10.hana.ondemand.com (v3)
Org: myorg  Space: dev

$ cf apps
Getting apps in org myorg / space dev...
name            requested state   processes
tutorials-srv   started           web:1/1
```

Copying this block yields three lines: the three `cf` commands, prompts stripped.

Multi-line continuation works too:

```console
$ cf create-service hana hdi-shared \
  my-hdi-container
Creating service instance my-hdi-container...
```

### Step 2: Diff markers — annotate changes

Use inline markers to highlight what changed. Add `// [!code ++]`, `// [!code --]`, `// [!code focus]`, or `// [!code highlight]` as trailing comments (adjust for your language).

```js
function greet(name) { // [!code --]
function greet(name = 'World') { // [!code ++]
  const msg = `Hello, ${name}!`
  console.log(msg) // [!code focus]
  return msg
}
```

Copying this fence produces only the **final** version (the added line; removed line excluded):

```
function greet(name = 'World') {
  const msg = `Hello, ${name}!`
  console.log(msg)
  return msg
}
```

Use `{copy=raw}` to get the original source with markers for teaching diffs:

```js {copy=raw}
export const VERSION = '1.0.0' // [!code --]
export const VERSION = '1.1.0' // [!code ++]
```

Focus mode dims non-focused lines until the reader hovers the block:

```cds
entity Books : managed {
  key ID    : UUID; // [!code focus]
  title     : String(111); // [!code focus]
  author    : Association to Authors;
}
```

### Step 3: Fill-in placeholders

This tutorial has `placeholders: auto` in its frontmatter. Any token matching `<YOUR_…>`, `<MY_…>`, or `<THREE_UPPERCASE>` becomes a fillable chip. Fill it once and every occurrence updates.

Target your BTP subaccount:

```console
$ cf target -o <YOUR_ORG_NAME> -s <YOUR_CF_SPACE>
```

Or in YAML config:

```yaml
services:
  hana:
    subaccount: <YOUR_SUBACCOUNT_ID>
    space: <YOUR_CF_SPACE>
```

The tokens `<YOUR_ORG_NAME>`, `<YOUR_CF_SPACE>`, and `<YOUR_SUBACCOUNT_ID>` are all automatically detected. Readers who fill one field see it updated everywhere.

Unfilled tokens display and copy as their original text (e.g. `<YOUR_SUBACCOUNT_ID>`).

### Step 4: File tree fences

Show project structure with a `tree` fence. Use `+` / `~` / `-` state prefixes and `# text` annotations.

```tree
my-cap-project/
  package.json
  .cdsrc.json
  db/
    schema.cds
+ srv/
+   cat-service.cds    # new: service definition
+   cat-service.js     # new: custom handlers
~ mta.yaml              # modified: added hana binding
  app/
    fiori-ui/
- old-approuter/        # deleted: replaced by AppRouter MTA module
```

The copy button returns the **plain ASCII** source.

### Step 5: Walkthrough — code + notes

A `:::walkthrough` directive pairs a code fence with a numbered list. Each list item starts with `{N}` or `{N-M}` to reference line numbers. Clicking a note highlights the corresponding lines.

:::walkthrough
```js
import cds from '@sap/cds'

const srv = await cds.connect.to('CatalogService')
const { Books } = srv.entities

srv.on('READ', Books, async (req) => {
  const results = await srv.read(Books)
    .where({ stock: { '>': 0 } })
  return results
})
```

1. {1} Import the CAP runtime SDK.
2. {3} Connect to the catalog service — returns a proxy to the remote service.
3. {4} Destructure the `Books` entity from the service definition.
4. {6-9} Register a `READ` handler that filters to in-stock books only.
:::

Line references out of bounds (e.g. `{40}` in a 9-line fence) fail the build with a clear error.

## Next Steps

Now you know how to use all five code-block enhancements. For full reference, see the authoring docs in the [tutorials-ims repository](https://github.com/sap-tutorials/tutorials-ims/blob/main/docs/authors/writing-tutorials.md) and the related [variables personalisation feature](../use-variables-in-tutorials/use-variables-in-tutorials.md).
