---
layout: default
title: Maintainer duties & SLAs
nav_order: 5
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
- for larger or high-traffic repositories, a curated [`CLAUDE.md`](./Checklist-for-publishing-a-new-OS-project.md#ai-agent-context-claudemd) is present and kept in sync with the codebase
- adherence to standards and platform idioms (e.g. semantic versioning, using the right code style, targeting the right platform versions, adhering to conventions)
- making sure obsolete and broken packages are marked as deprecated and unlisted from package repositories (such as NuGet, npm, etc.)
- adherence to the [internal standards checklist](./Checklist-for-publishing-a-new-OS-project.md)

### Security Vulnerability

> ℹ These SLAs apply only to a curated subset of repositories deemed important - not to every repository in the Kontent.ai GitHub organization. The list of covered repositories is maintained by the developers themselves; repositories are added or removed on a case-by-case basis.

The primary maintainer (first defined in the `CODEOWNERS` file or a leader of the team)
- is responsible for keeping [Dependabot updates](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-supply-chain-security#what-are-dependabot-updates) and [Dependabot alerts](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/about-supply-chain-security#what-are-dependabot-alerts) (or equivalent vulnerability reporting tools) enabled
- is responsible for checking those tools on a regular basis - often enough to keep remediation within the SLAs below

#### Remediation SLAs

Identified vulnerabilities for which a patch is available are patched; where no patch is available, a workaround is developed and deployed instead. Open-source repositories follow these internal SLAs, measured from the time of internal discovery:

| Severity | Remediation SLA |
| --- | --- |
| Critical | 2 weeks |
| High | 1 month |
| Medium | 2 months |
| Low | No specific requirement |

#### Process
- Regularly review the vulnerability reports (e.g. Dependabot alerts) for the covered repositories - frequently enough to stay within the [remediation SLAs](#remediation-slas)
- Address each finding according to its severity, within the corresponding SLA
- Release a patch version (or follow [SemVer](https://semver.org), if there are any breaking changes)
- Record the outcome where applicable (e.g. the internal wiki log)

## Managing the releases and keeping them in sync with product development
The maintainer is responsible for making sure:
- critical issues get planned and addressed within the standard development process (e.g. bring issues to grooming/sprint planning)
- any product updates affecting the OS project get reflected (e.g. a new API endpoint should be reflected in SDKs and samples) - see [New REST API feature](https://github.com/kontent-ai/.github/wiki/New-REST-API-feature) for more information.
- versions of the OS project are being released regularly to reflect the needs of customers

The release process consists of two steps:

1. Publishing new version on the package manager (.NET -> NuGet, node/js -> npm, ...)
1. Ensuring the proper announcement
    * New feature is described in release changelog
    * Decide whether to announce new version in [product changelog](https://kontent.ai/learn/changelog)/newsletter

## 💡 Make your life easier
Many of the tasks can be automated - e.g. [stale pull requests can be closed automatically](https://probot.github.io/apps/stale/), version management to be automated to a large extent, keeping things in sync with other systems like Jira can be automated. Be smart and automate!
