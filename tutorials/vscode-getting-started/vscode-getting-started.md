---
author_name: Joshua Margo
author_profile: https://github.com/jmmargo  
keywords: tutorial
auto_validation: true
time: 10
tags: [ tutorial>beginner, type>tutorial ]
primary_tag: type>tutorial
parser: v2
---

# Use Visual Studio Code to Author Tutorials
<!-- description --> Deprecated: Use the Sage VS Code Extension for most authoring tasks. This tutorial covers the basic set-up and optimization of Visual Studio Code as a developer tutorial authoring tool.


## You will learn
- How to author .md and .vr files in Visual Studio Code
- Which extensions to install to improve GitHub publishing efficiency
- How to publish your tutorials to the QA environment
- How to copy and publish your tutorials to the production environment

## Prerequisites
- Visual Studio Code (for Mac or Windows) is installed
- You have cloned the GitHub repositories in which you work


## Introduction
**This tutorial is deprecated.**  All of the actions described here are replaced with a dedicated SAP-owned VS Code extension called [Sage](https://github.tools.sap/I838039/sage-tutorial-extension/blob/main/README.md). You can review the steps for installing and using Sage in [this Tutorial](https://developers-prod-aqua.wcms-prod.c.eu-de-2.cloud.sap/tutorials/sage-getting-started.html).  **We're retaining this tutorial only for those who'd like a deeper understanding of how the Tutorial system interacts with GitHub.**

The principles in this tutorial regarding authoring and publishing are applicable to any markdown editor.  Please keep in mind that the extent to which features and functions are available across editors may vary.


### Link your cloned repositories to VS Code
1. When you first open VS Code, your explorer pane will be empty.  The explorer pane is accessible from the icon indicated in the image.
   
	![Empty explorer pane](empty-explorer-pane.png)

2. To add folders (i.e., cloned repositories) select `File` from the top menu bar and then select `Add Folder to Workspace`
   
	![Add folder to workspace](select-add-folder.png)

3. From the selection dialog box, select the folders you want to add and then click the `Add` button.
   
	![Select relevant folders](select-clones.png)

4. You will now see the selected folders in the VS Code Explorer pane.
   
	![Explorer pane with folders](explorer-with-folders.png)

### Set up useful extensions
1. Navigate to the Extensions icon on the left side of the screen.
   
	![Extension feature](go-to-extensions.png)

2. Your `INSTALLED` section counter should be blank or 0.  There are two extensions that we recommend you install to make using VS Code easy and efficient.  Click in the text box labeled with `Search Extensions in Marketplace`.
   
	![Enter extension name](enter-extension-name.png)

      - Enter `GitHub Pull Requests and Issues` and then install the extension.
  
      - Enter `File Utils` and then install the extension.
  
		![Example of extension search](get-file-utils.png) 
	
3. In your Extensions pane, you should now see the two installed extensions under the `INSTALLED` section.
   
	![Example of installed extensions](completed-extensions.png)

### Create new tutorial

>Remember to execute a pull command in each repository **BEFORE** working on your tutorials.  This is done through the Source Control pane.  You may also need to manually synchronize your fork in GitHub before pulling.

>![How to pull](pull.png)

1. To create a new tutorial, navigate to your respective Contribution folder (either `Tutorials-Contribution` or to your individual contribution repository).
   
2. Right click on the `tutorials` folder so that VS Code know where to place the folder for your new tutorial.
   
	![Create a new folder](create-new-folder.png)

3. Select `New Folder` and enter a name for the tutorial folder.
   
	![Enter folder name](new-tutorial-folder-name.png)

4. To create your new markdown file, right-click on the folder you just created and select `New File`.
   
	![Create new file](create-new-file.png)

5. Enter the file name (remember to match the folder name) and append it with ".md"  An icon will now appear to the left of the file name indicating that it is a markdown file.  You follow the same process to create your `rules.vr` file.
   
	![Enter file name](new-file-name.png)

>If you add .png files through a screen capture process, like SnagIt, and save them directly to the tutorial folder, they will appear automatically in the Explorer pane.

### Publish your tutorial to QA

1. Navigate to the `Source Control` icon.
   
	![Source Control icon](source-control-icon.png)

2. Enter a commit message.
   
	![Enter commit message](enter-commit-message.png)

3. Select the down-arrow next to the `Commit` button and select `Commit & Push`.  The system will generate a message asking if you want to stage your commits.  You can select **Yes** for a one-time staging or **Always** if you prefer not to be asked this question again.
   
	![Commit & Push](commit-and-push.png)


	![Staging confirmation](stage-confirm.png)

4. As all tutorial authors have write access to the Contribution repository, a pull request is not required at this point.  That will be covered in the next step.

### Publish your tutorial to production

1. Before publishing your tutorial to the production repository, please be sure that you check it in the QA instance of the [Tutorial Navigator](https://developers-prod-aqua.wcms-prod.c.eu-de-2.cloud.sap/tutorial-navigator.html)
   
2. Once the quality of your tutorial has been assured, you will need to copy it into your local fork of the repository.  Right click on the tutorial folder that you wish to publish to production and select `Duplicate`.
   
	![Duplicate function](duplicate.png)

3. Edit the path that is presented to you so that the folder is duplicated to the production repository.  This is typically achieved by removing the `-Contribution` from the path, but this might vary.
   
	![Duplicate path](duplicate-path.png)

4. If the tutorial already exists, then you will get a message confirming that you wish to overwrite the existing file (the answer is "Yes").  If it is a new file, it will write into the folder without a warning message.
   
5. At this point, you should follow steps 1-3 of the previous step in order to push the changes to production.
   
6. If you make changes on a forked repository, as many of us do, you will need to submit a pull request in order for your changes to be merged with the repository's main branch.
	- Navigate to Source Control and select the `Create Pull Request` button.
  
		![Create pull request](create-pull-request.png)
	
	- Enter a message in the title field (mandatory) and a description (optional), then select the `Create` button.  Typically, you will see the list of commits in the pull request as well.
  
		![Pull request data](pull-request-message.png)
>You may also create pull requests directly in your fork of GitHub as was previously done.
### Knowledge Check
