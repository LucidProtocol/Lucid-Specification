# How to contribute to the Lucid Protocol

Thank you for helping to improve the Lucid Protocol specification. This
guide explains how changes are proposed, reviewed and accepted.

## Who can contribute

- **Anyone with a GitHub account** can raise issues, comment, and propose
  changes to this repository through a fork and pull request.
- **Members of the WITS Protocol Standards Association**, which manages
  the Lucid Protocol, are also members of the LucidProtocol GitHub
  organisation. Membership gives read access to all of the organisation's
  repositories, including private ones. Members contribute to every
  repository in the same way: through forks and pull requests.
- **Project maintainers** review pull requests and merge them. Only
  maintainers can merge changes.

## Before you start

- Read the specification online: https://lucidprotocol.github.io/Lucid-Specification/
- Please follow our [Code of Conduct](CODE_OF_CONDUCT.md).
- New to git or GitHub? These guides explain the fork and pull request
  workflow used here:
  - [GitHub Docs - Contributing to a project](https://docs.github.com/en/get-started/exploring-projects-on-github/contributing-to-a-project)
  - [GitHub Docs - About forks](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/about-forks)
  - [Atlassian - Forking Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow)

## How to propose a change

1. **Raise an issue** describing the change and why it is needed. Please
   search the existing issues first.
2. **Wait for agreement.** The maintainers discuss the issue and label it
   "Change Agreed" when it should go ahead. Please don't start a pull
   request before then.
3. **Fork** this repository to your own GitHub account, and clone your fork
   to your computer.
4. **Create a branch** in your fork whose name starts with `Fix-nnn`, where
   nnn is the issue number.
5. **Make the changes** described in the issue. The specification is the
   Markdown under `docs/`; the JSON examples and JSON Schema are at the top
   level of the repository.
6. **Commit** with a message starting `Fix #nnn`.
7. **Push** the branch to your fork.
8. **Open a pull request** from your branch to this repository's `master`
   branch. Start the title with `Fix #nnn`, and write `Fixes #nnn` in the
   description so the issue closes automatically when the change is merged.
9. **Check the build.** Every pull request automatically builds the website
   and the PDF. The build must pass before the pull request can be merged;
   it fails, for example, if a link is broken. To see how your change looks,
   open the build from the pull request's **Checks** tab: the PDF is under
   **Artifacts** on the run's summary page. For some contributors, a
   maintainer has to approve the build before it runs.
10. **Respond to review.** A maintainer will review your pull request and
    may ask for changes. Push further commits to the same branch in your
    fork, and the pull request updates itself.
11. **Merge.** When the change is ready, a maintainer merges it. The website
    updates automatically a few minutes later. The change appears in a
    numbered Release, with its PDF, when the maintainers next publish one.

## Keeping your fork up to date

Before starting a new change, click **Sync fork** on your fork's GitHub
page, or pull from this repository's `master` branch, so you are working
from the latest specification.

## Changes to the build setup

Changes under `.github/` (the build workflow and the issue and pull request
templates) need approval from a maintainer before they can be merged. The
tooling that turns the Markdown into the website and PDF lives in a
separate repository,
[Lucid-Specification-Build](https://github.com/LucidProtocol/Lucid-Specification-Build).

## Licence

The specification and the other material in this repository are provided
under the terms in [LICENSE.md](LICENSE.md). By contributing, you agree
that your contribution may be included in this repository, and hence
within the specification depending upon the files changed, under those
terms.

## Notes for project maintainers

Maintainers have write access, so they can create branches directly in
this repository instead of using a fork. Everything else is the same:
changes still go through a pull request, the build must pass, and changes
under `.github/` need another maintainer's approval. After merging, check
that the issue has closed and delete the branch.
