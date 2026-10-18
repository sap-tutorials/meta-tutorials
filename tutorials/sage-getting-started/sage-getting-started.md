---
author_name: Riley Rainey
author_profile: https://github.com/rbrainey
keywords: tutorial
parser: v2
primary_tag: type>tutorial
tags: [ tutorial>beginner, type>tutorial ]
time: 30
---

# Install and Set Up the Sage Tutorial Authoring Extension

<!-- description --> Install the Sage VS Code extension and complete the setup wizard to configure your tutorial authoring environment with GitHub and IMS integration.

## You will learn
- How to download and install the Sage VS Code extension
- How to run the setup wizard to configure your authoring environment
- How to authenticate with GitHub and IMS
- How to use the core Sage features: My Tutorials view, preview, and publishing

## Prerequisites
- [Visual Studio Code](https://code.visualstudio.com/) (Stable Build for Mac or Windows) is installed
- You have a personal [GitHub](https://github.com/) account registered with the [sap-tutorials OSPO organization](https://portal.opensource.tools.sap/)
- You have access to the [IMS application](https://imsprod-approuter.cfapps.us30.hana.ondemand.com/ui/tutorialDashboard) as a Content Author

### Download the Sage Extension

The Sage extension is distributed as a platform-specific `.vsix` file. There are separate builds for macOS and Windows.

1. Navigate to the [Sage Tutorial Extension releases page](https://github.tools.sap/I838039/sage-tutorial-extension/releases).

2. The latest release is always listed at the top of the page.

3. Scroll past the change log to the bottom of the release entry, where you will find two `.vsix` files — one for **Mac** and one for **Windows**. Click the file for your platform to download it.

### Install the Extension in VS Code

1. Open Visual Studio Code.

2. Open the Command Palette: select **View > Command Palette...** (or press `Ctrl+Shift+P` / `Cmd+Shift+P`).

3. In the Command Palette, type `VSIX` and select **Extensions: Install from VSIX...**.

4. In the file dialog, navigate to your **Downloads** folder, select the Sage `.vsix` file you downloaded, and click **Install**.

5. After installation completes, you will see the **Sage owl icon** appear in the VS Code Activity Bar on the left side of the window. Click it to open the Sage sidebar.

### Run the Setup Wizard

When you first open the Sage sidebar after installation, you will see a **Setup Workspace** button. Click it to launch the setup wizard.

> **Note:** Wait for VS Code to finish initializing extensions (indicated by the spinning circular arrow in the status bar) before clicking Setup Workspace.

### Choose Your Sage Directory

The first step in the wizard asks you to choose a local directory where Sage will clone your tutorial repositories and store its data.

1. Click **Browse** to open a file selection dialog.

2. Navigate to the location where you want to store your tutorial repositories — typically your Documents folder.

   > If you prefer to keep GitHub repositories out of OneDrive (which syncs automatically on SAP machines), choose a directory outside your Documents folder.

3. Create a new folder (for example, `sap-tutorials`) and select it.

4. Click **Select** to confirm. The wizard will display the selected path and enable the **Next** button.

### Select Repository Groups

Each tutorial team uses a pair of GitHub repositories: a QA/Contribution repository (with a `-Contribution` suffix) and a Production repository. Sage refers to each such pair as a **repository group**.

1. The wizard displays a list of all available repository groups.

2. Select the repository group (or groups) that you work with. For most authors, this will be a single group. If you are unsure which group to select, check with your team lead.

3. Click **Next** to proceed.

### Sign In to GitHub

1. Click the **Sign in to GitHub** button.

2. VS Code will open a browser window asking for permission to authenticate. Click **Allow** (or **Yes**) in each prompt.

3. Once authenticated, the wizard displays your GitHub username and enables the next step.

   > **Multiple GitHub accounts:** If you use GitHub Copilot for Business within VS Code, you may have multiple GitHub accounts active. Sage automatically filters out the Copilot/GitHub for Business account and selects your personal tutorial-authoring GitHub ID. If the wrong account is selected, contact [Riley Rainey](mailto:riley.rainey@sap.com).

### Sign In to IMS

1. Click the **Sign in to IMS** button.

2. VS Code will open a browser window to the IMS OAuth authentication page. SAP Single Sign-On will automatically detect your SAP credentials and log you in.

3. After signing in, your browser will display a dialog asking permission to open a link in VS Code. Click **Allow**.

4. Return to VS Code and accept any remaining permission dialogs. The wizard will display your IMS username and enable the **Next** button.

   > Each dialog may include a "Don't ask again" or "Always allow" option. Selecting it streamlines future sign-ins so you do not need to confirm each step every time.

### Complete Setup

1. Review the summary of your selections, then click **Complete Setup**.

2. Sage will now:
   - Fork the Production repository for each selected group (if not already forked in your GitHub account)
   - Clone the QA (Contribution) repository for each selected group
   - Clone the Production fork for each selected group

   > The **Tutorials-Contribution** repository is very large. Cloning it may take several minutes. Sage displays progress for each repository as it works.

3. When all repositories have been cloned, **completely close VS Code and reopen it** to complete initialization.

### Initialize Your Sage Session

After reopening VS Code and clicking the Sage icon in the Activity Bar:

1. Wait for the spinning circular arrow in the status bar to disappear, indicating that extensions have finished loading.

2. If you see a **Setup Workspace** button, the setup wizard did not complete successfully — return to the previous steps.

3. If you see an **Initialize your Sage session** button, click it. Sage will re-authenticate with GitHub and IMS and then retrieve your assigned tutorials, issues, and pull requests. This takes approximately 60–90 seconds.

4. Once initialization is complete, you will see three panels in the Sage sidebar:
   - **My Tutorials** — tutorials assigned to you in IMS, plus unpublished tutorials found in your cloned repositories
   - **My GitHub Issues** — GitHub issues assigned to or mentioning you
   - **My Pull Requests** — active pull requests in your repositories

### Explore the My Tutorials View

The **My Tutorials** panel shows a distilled view of the content you work on, combining data from IMS and your cloned repositories.

1. Each entry shows the tutorial's slug (URL name). Click the **ABC** toggle button in the panel header to switch between slug view and title view.

2. Each entry also shows a status label (for example, **In Production**, **Needs Review**, **New - Unpublished**).

3. Click any tutorial entry to open its Markdown file in the editor.

4. Right-click a tutorial entry to access the context menu, which includes:
   - **QA Preview** — opens the tutorial in the QA Tutorial Navigator in your browser
   - **Preview** — opens a live preview panel directly inside VS Code
   - **Edit Validation Rules** — opens or creates the `rules.vr` file
   - **Reveal in Explorer** — switches to the Explorer view and highlights the tutorial folder
   - **Commit to QA** — stages and commits your changes to the QA repository
   - **Promote QA to Production** — automates the full publish-to-production workflow
   - **Edit Front Matter** — opens the front matter editor UI
   - **Mark as Reviewed** — marks the tutorial as reviewed in IMS

### Use the In-Editor Preview

Sage provides a high-fidelity in-editor preview of your tutorial that renders approximately 95% of the production output, including interactive validation questions.

1. With a tutorial's Markdown file open, right-click the tutorial in the **My Tutorials** panel and select **Preview**, or use the Command Palette and run **Sage: Preview Current File**.

2. The preview panel opens alongside your editor. Scroll through it to review formatting, images, and step structure.

3. To test a validation question, click answers in the preview just as a reader would. To reset the question state, click the **Reset Preview** button at the top of the preview panel.

### Use the Validation Panel

Sage validates your tutorial file every time you save it and displays results in VS Code's **Problems** panel.

1. Save your Markdown file (`Ctrl+S` / `Cmd+S`).

2. Open the **Problems** panel (**View > Problems**) to see any errors or warnings, such as:
   - Missing required front matter fields
   - Tags that do not exist in IMS
   - Image filename case mismatches (which cause images to disappear in production)
   - Code blocks with blank lines adjacent to fence markers (which break the copy button)

3. Click any problem entry to jump directly to the relevant line in your editor.

### Edit Front Matter

The front matter editor provides a form-based UI for editing the YAML header of your tutorial without editing raw text.

1. Right-click the tutorial in **My Tutorials** and select **Edit Front Matter**.

2. The editor panel opens and displays your current tags, primary tag, author information, estimated time, and other fields.

3. To search for and add a tag, type in the tag search box — Sage queries its local cache of IMS tags for fast results.

4. To change the primary tag, click any existing tag in the primary tag section.

5. Click **Save** when you are done. Sage updates the front matter in the Markdown file automatically.

### Commit Changes to QA

1. After making and saving your changes, right-click the tutorial in **My Tutorials** and select **Commit to QA**.

2. A dialog lists all changed files for this tutorial. Select the files you want to include in the commit (for example, the Markdown file and any new images).

3. Check **Automatically sync** to also push the commit to the remote repository.

4. Click **Commit Changes**. Sage stages, commits, and optionally pushes your changes in a single operation.

### Promote a Tutorial to Production

When your tutorial is ready for production, Sage automates the entire publish workflow — normally a multi-step manual process.

1. Right-click the tutorial in **My Tutorials** and select **Promote QA to Production**.

2. Sage will:
   - Sync your fork of the Production repository with the upstream
   - Copy the tutorial files from the QA repository to your Production fork
   - Commit the changes
   - Open a pull request against the main Production repository

3. Review the pull request details and click **Create** to submit.

4. Curators will review and merge your pull request to publish the tutorial to production.

