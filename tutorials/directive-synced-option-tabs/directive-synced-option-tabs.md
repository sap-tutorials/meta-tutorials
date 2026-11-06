---
parser: v2
auto_validation: false
time: 10
tags: [ tutorial>beginner, software-product>sap-business-technology-platform ]
primary_tag: software-product>sap-business-technology-platform
author_name: SAP Developer Advocates
author_profile: https://github.com/sap-tutorials
---

# Synced Option Tabs
<!-- description --> Demonstrate synced OPTION tab groups that remember the reader's choice across every step of a tutorial, using the `syncTabs` frontmatter flag and the `sync=` qualifier on OPTION blocks.

## You will learn
- How to turn on synced option tabs with the `syncTabs: auto` frontmatter flag
- How to group OPTION blocks into a synchronized set with the `sync=<group>` qualifier
- How a reader's choice in one step carries to every other step automatically

## Prerequisites
- Basic familiarity with the tutorial OPTION block syntax (see [v2-example-tutorial](../v2-example-tutorial/v2-example-tutorial.md))
- Basic familiarity with CAP

---

### Step 1: Initialize the project

Pick your runtime and initialize a new CAP project.

[OPTION BEGIN [Node.js] sync=runtime]
Run `npm init -y && npm add @sap/cds-dk` to initialize a Node.js CAP project.

Then scaffold the project structure:

```bash
cds init my-cap-app
cd my-cap-app
npm install
```
[OPTION END]
[OPTION BEGIN [Java] sync=runtime]
Run `mvn archetype:generate` to initialize a Java CAP project:

```bash
mvn archetype:generate \
  -DarchetypeGroupId=com.sap.cds \
  -DarchetypeArtifactId=com.sap.cds.archetype \
  -DarchetypeVersion=RELEASE
```

Follow the prompts to set your project name and package.
[OPTION END]

### Step 2: Add a service

Define a simple CDS service, then add the runtime-specific handler.

[OPTION BEGIN [Node.js] sync=runtime]
Create `srv/hello-service.cds`:

```cds
service HelloService {
  function greet(name : String) returns String;
}
```

Then create `srv/hello-service.js`:

```js
module.exports = (srv) => {
  srv.on('greet', ({ data: { name } }) => `Hello, ${name}!`)
}
```
[OPTION END]
[OPTION BEGIN [Java] sync=runtime]
Create `Application.java` in your main source folder:

```java
@Component
public class HelloServiceHandler implements EventHandler {

  @On(event = "greet", service = "HelloService")
  public void onGreet(CdsCreateEventContext ctx) {
    String name = ctx.getData().get("name").toString();
    ctx.setResult("Hello, " + name + "!");
  }
}
```
[OPTION END]

### Step 3: Run and test

Start the server and try the endpoint.

[OPTION BEGIN [Node.js] sync=runtime]
Run the development server:

```bash
cds watch
```

Open a browser or use `curl` to test:

```bash
curl "http://localhost:4004/hello/greet(name='World')"
```
[OPTION END]
[OPTION BEGIN [Java] sync=runtime]
Run the Spring Boot server:

```bash
mvn spring-boot:run
```

Open a browser or use `curl` to test:

```bash
curl "http://localhost:8080/hello/greet(name='World')"
```
[OPTION END]

### Step 4: Review what happened

You have now tried synced option tabs. The picker bar at the top of the page remembers your runtime choice and applies it to every group automatically — each OPTION block carrying the same `sync=runtime` qualifier switches together.

- Read the [CAP documentation](https://cap.cloud.sap/docs/) for more
- Explore the other directive-syntax samples in this repository
