# Developer Center and Tutorials - Administrative Run Book

This document is intended to document all tasks required for SAP Developer Center and Developer Tutorial system operation.

## Intended Audience
- **Developer Center Administrator**
- **Tutorial Repository Group Owner**
- **Tutorial Repository Group Curator**
- **Tutorial Analytics Administrator**

## Developer Center Administrators

- **Tools and Access** - 
  - Be a designated administrator of the sap-tutorials GitHub organization (these rights are granted via the SAP OSPO)
  - Appropriate QA-Blue and Production sap.com AEM author access (consult Ryan Sherman for access)
  - [SAP Digital Services Request Tool](https://dsrequest.int.sap/home) access (support team email contact `18db6b96.groups.sap.com@emea.teams.ms`)
  - [SAP FCMS Download Manager Cockpit](https://fcmscockpit-supportportal.dispatcher.hana.ondemand.com/index.html#) access (this should be available to all SAP employees; the FCMS site says I have `Standard profile for SAP Internal`)
  - SAP access to CircleCI - we use CircleCI for Tutorial 'lint'ing. I'd like to get away from using it as the functionality is basically provided by the Sage VS Code Extension now.


### Task: Update Developer Center Home Page
- **Interval:** Weekly
- **Status:** Active
- **Purpose and Objective:** - Update the Events section on the [Developer Center Landing Page](https://dx-prod-author.wcms-prod.c.eu-de-2.cloud.sap/sites.html/content/developers/website/languages)

1. Visit the AEM Content Navigator Page: `https://dx-prod-author.wcms-prod.c.eu-de-2.cloud.sap/sites.html/content/developers/website/languages`
2. Select the `en` page
3. Select `Edit` on the tool bar.  This action will open a new browser tag with the AEM Content Editor window.
4. Make appropriate updates to the Developer Events section
5. Preview results
6. Return to the AEM Content Navigator Page and select `Quick Publish`

---

### Task: Update Repository Group Owner List

---

### Task: Migrate a New Tag from AEM to IMS

---

### Task: Rename a Repository Group

---

### Task: Monitor and Publish Trials and Downloads updates
- **Interval:** As Required
- **Status:** Active
- **Purpose and Objective:** - Review and publish Tutorial updates

  The [SAP Digital Services Request Tool](https://dsrequest.int.sap/home) (DSRT) 

---

### Task: Check AEM Tutorial Pipeline
- **Interval:** When notified by e-mail
- **Status:** Active
- **Purpose and Objective:** - Errors or stalls may be encountered in both the QA-Blue and production Markdown to HTML processors.  These processing jobs are driven by independent `cron` jobs that sre set to execute every five minutes.

1. Each environment has its own **Tutorial Import Admintool** page.  At the bottom of this page, you can see the date (CET) of the last execution of the job.  An in-progress job will also be marked as such.  If more than ten minutes has elapsed since the last execution, it should be investigated by EPAM or the sap.com team.
2. You can also harvest log files.  All lag files are available for download at the corresponding Log Harvester page -- you only need to review the `tutorials.log` file. The hosting container logs at DEBUG level, so this log is quite verbose and challenging to interpret.  I generally look for Java exceptions - and the error associated with those may point to a formatting or structure issue with a newly committed tutorial.

Production **Tutorial Import Admintool** URL (AEM): `https://dx-prod-author.wcms-prod.c.eu-de-2.cloud.sap/etc/sapdx/tools/tutorialsAdmin/gitHubAdmin.html`

Production Log Harvester URL (AEM): `https://dx-prod-author.wcms-prod.c.eu-de-2.cloud.sap/bin/systemReport.this.html`

QA-Blue Log Harvester URL (AEM): `https://dx-qa-author-blue.wcms-nonprod.c.eu-de-2.cloud.sap/bin/systemReport.this.html`

---

### Task: Conduct Office Hours Sessions for Authors

---

### Task: Add a new Mission

---

### Task: Revise a Mission

---

### Task: Delete a Mission

---

### Task: Add a New Group

Requires Administrator access in the IMS Production Application and Production AEM Access.

1. Open the '[Groups](https://imsprod-approuter.cfapps.us30.hana.ondemand.com/ui/groups)' in the production IMS application.
2. Press the "+" in the lower right corner of the page to open the Add group panel.

...

4. Open the [production AEM authoring page](https://dx-prod-author.wcms-prod.c.eu-de-2.cloud.sap/aem/start.html)
5. Click on "Tools > Developers > Group Admin"
6. Click on "+ add group"
7. Enter the IMS ID of the Group in the ID field, AEM will respond by looking up the details for the group and filling other fields in the form.
8. In the "Page Name" field, enter a unique slug to be used to construct the URL of the group.  Use only lower case characters, numbers and '-' as word separators.  Groups from the same team should follow a similar pattern.  You'll need to look at the URLS used for existing groups to get the gist here. The URL will become `https://developers.sap.com/tutorials/group.<page-name>.html`
9. Press 'Save' when done.  The HTML Page and an associated [Tutorial Navigator](https://developers.sap.com/tutorial-navigator.html) entry will be published.

Suggested slug formulation: `<product-tag>.<group-title-summary>`

| Primary tag topic          | Value |
|----------------------------|----|
| Business Technology Platform| btp |
| Integration Suite | integration |
| ABAP | abap |
| CAP | cap |
| Kyma | kyma |
| AI Business Suite | ai |
| Joule Studio | joule |
| SAP Build | build |

     et cetera


---

### Task: Revise a Group

---

### Task: Delete a Group

---

### Task: Retiring a Tutorial

---

### Task: Define a New App Space Event

---

### Task: Monitor Tutorial Analytics

---

### Task: Designate or Change a Repository Group Owner

---

### Task: IMS Production Backup

---

### Task: IMS Production Recovery

---

### Task: Migrate Existing Tutorial(s) to Another Tutorial Repository

---

### Task: Administer CircleCI 

We access CircleCI using our personal GitHub credentials which are registered with OSPO.  The organization inside CircleCI is '[sap-tutorials](https://app.circleci.com/settings/plan/github/sap-tutorials/overview)'. There is also a '[SAP](https://app.circleci.com/projects/project-dashboard/github/SAP)' organization.  Riley Rainey and Thomas Jung are currently designated as the technical contacts for the 'sap-tutorials' organization. I am currently unsure who owns or administers the 'SAP' organization with SAP -- OSPO perhaps?

Our team maintains a [tutorial-checker](https://github.com/sap-tutorials/tutorial-checker) repository, which contains a script invoked periodically by CircleCI.  This script perform the 'lint' check on the repo. Exceptions detected on the run are logged in CircleCI.

---

### Task: Add an Author to the IMS Application

1. Visit the [BTP Users Cockpit](https://amer.cockpit.btp.cloud.sap/cockpit#/globalaccount/a0ec0d63-f690-42ab-8113-9f6567cd897e/subaccount/a3a2d7d3-941f-4518-b1af-9dcd689db3d4/usersOverview&//) for the IMS Subaccount.
2. Search for the email address of the user to be added. Verify that it does not yet exist.
3. Click 'Create' - add the user using their email address.
4. Click the '>' symbol at the right of the user's entry.  Click on "Role Collections"
5. Locate and click on the '...' - select "Assign Role Collection" - select "IMS_Content_Author_dev" and click "Assign Role Collection"

---

### Task: Handle Author Support Requests
- **Interval:** As needed
- **Status:** Active
- **Purpose and Objective:** - Provide support and guidance to tutorial authors on general questions, technical issues, and authoring best practices

1. Monitor incoming support requests via email (riley.rainey@sap.com) and other communication channels
2. Triage requests and determine appropriate response:
   - Direct technical questions about tools (VS Code, Git, GitHub)
   - Tutorial authoring best practices and style guide questions
   - Access and permission issues
   - Routing to Developer Advocates for product-specific guidance
3. Respond to authors with clear guidance or escalate to appropriate team members
4. Document recurring issues and update author documentation as needed

---

### Task: Review Tutorial Planning Outlines
- **Interval:** As requested by authors
- **Status:** Active
- **Purpose and Objective:** - Review and provide feedback on tutorial plans before authoring begins to ensure alignment with Tutorial Navigator strategy and best practices

1. Receive tutorial planning outlines from authors (typically via email)
2. Review the proposed structure:
   - Verify logical flow and chunking of content into appropriate tutorial units
   - Assess time estimates for individual tutorials and overall groups/missions
   - Evaluate proposed titles and descriptions for clarity and consistency
   - Check alignment with existing tutorial content to avoid duplication
3. Consult with Developer Advocates for the relevant product area as needed
4. Provide feedback to authors on:
   - Recommended adjustments to tutorial breakdown
   - Suggestions for grouping tutorials effectively
   - Whether content merits a Mission vs. Group designation
   - Tag recommendations and metadata considerations
5. Approve plans or request revisions before authors begin development

---


## Tutorial Repository Owners 

Repository owners are designated any time a new Tutorial Repository Group is created. Every Tutorial Repository Group must have at least one Owner. Repository owners must notify the **Developer Center Administrator** if their work assignments change, so that a new owner can be in place at all times.

- **Tools and Access** - 

  - be a member of the GitHub "admin" group associated with their owned repository(ies).

### Task: Review and Merge Pull Requests
- **Interval:** Daily
- **Status:** Active
- **Purpose and Objective:** - Review and publish Tutorial updates

---

### Task: Issue Review
- **Interval:** Bi-weekly
- **Status:** Active
- **Purpose and Objective:** - Expedite resolution of reported Tutorial Issus 

---

## References

**SAP Developer Tutorials - Author's Guides** - `https://developers-qa-blue.wcms-nonprod.c.eu-de-2.cloud.sap/tutorials/docs-tutorial-0-big-picture-2022.html`

**SAP Developer Tutorials Work Zone site** - `https://workzone.one.int.sap/site#workzone-home&/groups/A7NmsKmoG9vVaxizKhdQOk/workpage_tabs/r9dX7mvdcXW5KMmWvG2NYl`

## Terms