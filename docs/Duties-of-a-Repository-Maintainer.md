---
layout: default
nav_order: 7
---

# Duties of a repository maintainer
## The main duties
The maintainer is responsible for:
- [setting **the right expectations** about the project](#setting-expectations)
- [keeping the repository **in a good shape**](#keeping-the-repository-in-a-good-shape)

Repository ownership doesn't mean that the owner needs to do all the required work, it means that they ensure the work gets done.

## Setting expectations
### Responding to issues and pull requests
To set the expectations correctly, the maintainer will make sure that when an issue or a pull request is submitted, the issuer gets notified that a **human has seen the submission** and **what happens next**. This can mean:
  - telling the consumer whether the issue/PR will be fixed/merged or not
  - ETA of how long it will take until the next response or until the issue resolution/PR review
  - asking for more details

### Defining and communicating the vision
It's necessary to clearly express what's the **future of the project**. A good idea is to groom the backlog and decide on what the next major steps will be. A good tool to communicate the vision is the [GitHub Milestones](https://docs.github.com/en/github/managing-your-work-on-github/about-milestones) or [GitHub Projects](https://github.com/features/project-management/).

#### Obsolete projects
If the project is not to be maintained any longer, it's necessary to [archive](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/archiving-a-github-repository) it and ideally, add a disclaimer explaining why the repo is archived.

### SLA
**The first response** time and **any follow-up to a customer's reply** should not exceed **5 business days**. If a reply will take longer, it's necessary to update the expectation of the person waiting for the reply.

**The resolution time** depends on an actual issue/PR. Again, keep customer's expectations about the resolution time up to date.

## Keeping the repository in a good shape
The maintainer must ensure the repository is in a healthy state. This includes:
- the CI is present, running, and tests are passing
- there are no stale pull requests - they either get merged (after a review and testing) or closed
- the backlog of issues is accurate, up-to-date
  - the issue is labeled correctly, the descriptions are sufficient for anyone from the community to pick them up and process
- the repo is well documented (READMEs and Wikis are up to date and correspond with the latest version)
- adherence to standards and platform idioms (e.g. semantic versioning, using the right code style, targeting the right platform versions, adhering to conventions)
- making sure obsolete and broken packages are marked as deprecated and unlisted from package repositories (such as NuGet, npm, etc.)
- adherence to [internal standards checklist](Checklist-for-publishing-a-new-OS-project)

### Regular vulnerability check

> ℹ Following section applies only to the repositories that are labeled `check-vulnerabilities` under the Kontent.ai GitHub organization.

The primary maintainer (first defined in the `CODEOWNERS` file or a leader of the team)
- is responsible for performing security updates every first week of the month
- is responsible to have the (Dependabot updates)[https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-supply-chain-security#what-are-dependabot-updates] and (Dependabot alerts)[https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-supply-chain-security#what-are-dependabot-alerts] turned on

#### Process
- Every first Monday of the month a notification is poping for the vulnerability check
- Primary maintainer will go through the assigned repositories and check Dependabot alerts
- Once done - the progress is submitted to the list in the notification (internal wiki log)

## Managing the releases and keeping them in sync with product development
The maintainer is responsible for making sure:
- critical issues get planned and addressed within the standard development process (e.g. bring issues to grooming/sprint planning)
- any product updates affecting the OS project get reflected (e.g. a new API endpoint should be reflected in SDKs and samples) - see [New REST API feature](https://github.com/kontent-ai/.github/wiki/New-REST-API-feature) for more information.
- versions of the OS project are being released regularly to reflect the needs of customers

The release process consists of two steps:

1. Publishing new version on the package manager (.NET -> Nuget, node/js -> npm, ...)
1. Ensuring the proper announcement
    * New feature is described in release changelog
    * Decide whether to announce new version in [product changelog](https://docs.kontent.ai/changelog/product-changelog)/discord/newsletter

## 💡 Make your life easier
Many of the tasks can be automated - e.g. [stale pull requests can be closed automatically](https://probot.github.io/apps/stale/), version management to be automated to a large extent, keeping things in sync with other systems like Jira can be automated. Be smart and automate!
