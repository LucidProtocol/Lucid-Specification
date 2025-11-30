# How to contribute to the Lucid Protocol

## General guidelines

This repository uses a fork and pull request workflow.

If you are new to git or github you can find more general information about the procedures at the following links. We suggest you read and familiarise yoursefl with these prior to following the specific instructions in the next section.

* [GitHub Docs - Contributing to a project](https://docs.github.com/en/get-started/exploring-projects-on-github/contributing-to-a-project)
* [GitHub Docs - Collaborative development models](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/getting-started/about-collaborative-development-models)
* [GitHub Docs - About forks](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/about-forks)
* [Digital Ocean - Contributing to open source projects](https://www.digitalocean.com/resources/articles/how-to-contribute-to-open-source)
* [Atlassian - Forking Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow)

**IMPORTANT** : The main item in the repository is the Word document which is the master of the protocol specification. This can only reasonably be changed by one person at a time, so we use a serial method of making changes. To minimise the chance of rework, please only prepare a pull request when invited to do so by a project maintaner.

## Specific procedure

The following is the general procedure for making changes to the protocol specification and json supporting material.

* Raise an issue in the project
* Wait for the issue to be labelled as "Change Agreed" by the project maintainers
* Once invited by the project maintainers you may commence preparing the Pull Request (PR) 
* Fork the repo to your own copy
* Clone your repository locally
* Create a new branch whose name starts with "Fix #nnn" where nnn is the fix number.
* Switch to the branch
* Make the changes described in the Issue
* Commit the change locally, using a comment starting with "Fix #nnn"
* Push the fix branch to your forked copy of the repository
* Create a pull request in the main repo based on your changed fork
* The pull request name should start with "Fix #nnn"
* Support the pull request through the process or review and acceptance

## Extra guideline for changing the document

To edit the Word document, please follow the procedure below:

* Open the Word document
* The document will contain marked up changes for the last version
* Accept all changes to the document whilst leaving tracking on
* Make the relevant changes to the document
* Check that the format does not change whilst making textual changes
* Look for red lines in the document above and below the change you have made
* Save the document
* Export the document as a PDF, in Options for the PDF Export ensure that:
  * Create Bookmarks using Headings is selected
  * Publish "document only" is selected, so that changes are not shown in the PDF
* Ensure that the document and PDF are closed (so no temporary files will be submitted)
* Modify any code within the JSON files
* Return to the normal github process above

## Notes for project maintainers

Project maintainers shoudl be aware of the following:

* Tables sometimes get centered. We have not found an appropriate solution to that yet
* Someone reviewing should click on the item, click on 3 dots and view file, download, then open the downloaded file as a reference read only copy
* The naming of branches and pull requests should ensure that they close automatically
* Check that pull request is closed on completion
* Delete the branch after the merge

 
