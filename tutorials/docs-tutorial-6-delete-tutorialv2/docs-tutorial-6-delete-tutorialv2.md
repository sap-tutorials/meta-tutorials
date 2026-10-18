---
title: Delete a Tutorial from QA (v2 parser)
description: Delete a tutorial from QA that is not yet in production.
time: 5
tags: [ tutorial>beginner, topic>cloud]
primary_tag: topic>cloud
parser: v2
---

# Delete a Tutorial from QA (v2 parser)

<!-- description --> Delete a tutorial from QA that is not yet in production.

**Important: Avoid deleting tutorials**

Tutorials in production are linked to missions and groups, to marketing campaigns, to a list of recommended tutorials, and are linked to from the Help Portal, Learning Journeys, Developer Center pages, Community posts, other tutorials as prerequisites, and other web pages. And, of course, they are indexed by Google.

> **Avoid deleting tutorials, if possible. Instead, update them.**

If you have to delete a tutorial, first reach out to [Daniel Wroblewski](mailto:daniel.wroblewski@sap.com). I will check dependencies and delete them for you, including groups and missions, if needed.

This tutorial explains how to delete a tutorial that you have already pushed to QA but not to production.

### Step 1: Delete tutorial files

1. Open the tutorial folder.

2. Select all the files in the folder.

3. Click **Delete**.

![Delete files](delete1.png)

### Step 2: Push to QA

Right-click the tutorial folder, and click **Push to QA**.

![Push to QA](delete2.png)

You can also select multiple folders, and then click **Push to QA**.

![Push to QA](delete2b.png)

### Step 3: Delete folders (locally)

The push will not delete the local folders you have in Atom so you need to do this manually.

>These folders do not appear in GitHub.

Right-click the folder(s) and click **Delete**.

![Push to QA](delete3.png)
