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
- **Interval:** As Needed
- **Status:** Active
- **Purpose and Objective:** - Maintain a contact list of the owner of each Tutorial GitHub Repository Group

1. Originally created to support CircleCI 'lint-in' of Tutorial repositories, We maintain a list of the team leaders responsible for maintaining editorial control of all repository groups.
2.  This JSON file is stored as `https://github.com/sap-tutorials/tutorial-checker/blob/main/data/repository.owner.json`
3. The top-level JSON object contains keys naming each repository group (e.g., "Tutorials").  Within each associated value object is the GitHub username of the owner (`name`) and SAP email address (`email`).
4. Although CircleCI is less commonly used today as a lint tool, this list remains handy as a database of owners when contact is required.

---

### Task: Import a New Tag from AEM/Semaphore to to the Tutorial system
- **Interval:** As Needed
- **Status:** Active
- **Purpose and Objective:** - Make a new or changed product name or other metadata tag available to content authors

1. Metadata tags must be defined in IMS before they can be referenced in a Tutorial.
2. The Tutorial subsystem operates under the umbrella of sap.com data governance.  By agreement with the sap.com team, all Developer Center page tagging must be consistent with wider SAP.com tagging.  To implement this across SAP sites, the sap.com team maintains a universal SAP tagging taxonomy in a system called Semaphore.  We must periodically import new tags from Semaphore into IMS to keep it up to date. We do not perform a bulk import of the full taxonomy.New Semaphore tags are identified (usually by a request to us by the owning product team), and we then import it using a custom AEM plugin module.
3. Open the [AEM Tutorial Import plugin](https://dx-prod-author.wcms-prod.c.eu-de-2.cloud.sap/etc/sapdx/tools/tutorialsAdmin/gitHubAdmin.html).
4. Activate the "TAG CONFIGURATION" tab.

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

### Task: Add a new Mission

---

### Task: Revise a Mission

---

### Task: Delete a Mission

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

### Task: Manage AEM GitHub Access Keys
- **Interval:** As Needed
- **Status:** Active
- **Purpose and Objective:** - Maintain the pool of GitHub service accounts and their associated API keys used by AEM to poll Tutorial repositories and publish content

The AEM application runs as a cron job that periodically polls Tutorial repositories for recent commits and translates them to new or updated Tutorial HTML pages. Access to these repositories is authenticated via GitHub API keys tied to the following SAP-managed service accounts:

| Account Name | SAP Shared Email Address |
|---|---|
| SAP Dev Relations 1 | sapdevrelations1@sap.com |
| SAP Dev Relations 3 | sapdevrelations3@sap.com |
| SAP Dev Relations 4 | sapdevrelations4@sap.com |
| SAP Dev Relations 5 | sapdevrelations5@sap.com |
| SAP Dev Relations 6 | sapdevrelations6@sap.com |
| SAP Dev Relations 7 | sapdevrelations7@sap.com |
| SAP Dev Relations 8 | sapdevrelations8@sap.com |
| SAP Dev Relations 9 | sapdevrelations9@sap.com |
| SAP Dev Relations 10 | sapdevrelations10@sap.com |
| SAP Dev Relations 11 | sapdevrelations11@sap.com |
| SAP Dev Relations 12 | SAPDevRelations12@global.corp.sap |
| SAP Dev Relations 13 | SAPDevRelations13@sap.com |
| SAP Dev Relations 14 | SAPDevRelations14@sap.com |
| SAP Dev Relations 15 | SAPDevRelations15@sap.com |
| SAP Dev Relations 19 | sapdevrelations19@sap.com |

> **Note:** Accounts "2" and "16"–"18" are missing from this pool.

1. To regenerate an expired or compromised API key, log in to GitHub using the shared email address for the relevant account (The password is the same for each account.  That password has been communicated via private e-mail to Thomas Jung). Login requires MFA validation via e-mail.  These e-mail addresses are SAP Outlook Shared Inboxes.  Such addresses are administered via the [RMMT Self Service Tool](https://sapit-clientservices-prod-lamb.launchpad.cfapps.eu10.hana.ondemand.com/5a44b00c-a077-47ed-99b4-acfe54a44958.rmmt.sapitrmmt2uiuser) -- [SAP Shared Mailbox Documentation available at this site](https://sap.sharepoint.com/sites/127531/SitePages/Exchange-Online-Shared-Mailbox-Self-Service.aspx?xsdata=MDV8MDJ8STgzODAzOUBnbG9iYWwuY29ycC5zYXB8YTc1YzdlOGM2N2E3NGZjMzc2NDgwOGRlYTA3ZWY4NzZ8NDJmNzY3NmNmNDU1NDIzYzgyZjZkYzJkOTk3OTFhZjd8MHwwfDYzOTEyNDY2NDU1OTA5NjUzN3xVbmtub3dufFRXRnBiR1pzYjNkOGV5SkZiWEIwZVUxaGNHa2lPblJ5ZFdVc0lsWWlPaUl3TGpBdU1EQXdNQ0lzSWxBaU9pSlhhVzR6TWlJc0lrRk9Jam9pVFdGcGJDSXNJbGRVSWpveWZRPT18NjAwMDB8fHw%3d&sdata=dER0ejNjVHFkdUFzRkhTQnhuRVY4bzdiQkVEcHJDSUI2Mkk2Vm9PcVVMcz0%3d)
2. Navigate to **Settings > Developer settings > Personal access tokens** and generate a new token with `repo` (read) scope.
3. Update the corresponding key in the AEM configuration. **Location of these keys is needed**
4. Verify the pipeline resumes normally by monitoring the next scheduled cron execution (see **Check AEM Tutorial Pipeline** task).

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