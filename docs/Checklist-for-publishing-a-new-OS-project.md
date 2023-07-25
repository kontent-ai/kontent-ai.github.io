---
layout: default
nav_order: 2
---

# Checklist for publishing a new OS project

Before you publish any repository under Kontent.ai organization on GitHub, please go through this checklist and make sure the repository is up to standards.

* [Repository name](#Repository-name)
* [Dedicate a maintainer](#dedicate-a-maintainer)🔒
* [Fulfill Community profile](#community-profile)🔒
* [Set up GitHub features](#github-features)🔒
* [Set Expectations](#expectations)🔒
* [Add badges](#badges)
* [Add automatic tests](#tests)❔
* [Set up Continuous Integration](#continuous-integration)❔
* [Protect Master Branch](#protect-the-master-branch)
* [Add collaborating teams](#add-collaborating-teams)🔒
* [Define release process](#releases)

## Repository name

Set repository name according to the [Naming conventions](./Naming-conventions.md).

## Dedicate a maintainer
- _🔒 Required for private repositories too_

It's essential to decide who's going to be responsible for the repository. Every repository needs to have an owner (a person or a team) who will actively:
- set the right expectations about the project
- keep the repository in a good shape

Read more on [the duties of a maintainer](./Duties-of-a-Repository-Maintainer.md).

Mark this user into the [CODEOWNERS](https://help.github.com/articles/about-code-owners/) file. See an [example](https://github.com/kontent-ai/delivery-sdk-net/blob/master/.github/CODEOWNERS).

## [Community profile](https://help.github.com/articles/about-community-profiles-for-public-repositories/)
- should be "all green"
- please note that it's available only for public repos and not for forked repos
![Green community profile](https://i.imgur.com/DVRjA41.png)

### Description, website, and topics
- _🔒 Required for private repositories too_

Fill in basic information about the project to make it easy to find it.
![Topics](https://i.imgur.com/4lNqMK6.png)

⚠ Tag the repository based on the division according to [Naming conventions](./Naming-conventions.md#github-repositories).

In the case of private repositories, add a "private-repository" tag.

### README (Documentation)
- _🔒 Required for private repositories too_

README should contain:

- installation instructions
- basic demonstration of usage
- code examples (if applicable)

More complex topics and examples can be covered in separate articles in GitHub Wiki (or an external system such as ReadTheDocs).

> The template of the README file is stored in [repo-template](https://github.com/kontent-ai/repo-template).

### Contributing
From the README or CONTRIBUTING files, it should be clear:

- how to set up the project in order to contribute
  - this may include creating a PowerShell or other (e.g. build) script to make it easy for the contributors
- what kind of contributions are accepted and welcome
- what's the definition of done (use PR templates)
- which communication channels should be used to get in touch with the maintainer

> The template of CONTRIBUTING file is stored in [repo-template](https://github.com/kontent-ai/repo-template).

### License

Use the MIT license and set "Kontent.ai" as the copyright holder. If you want to use a different license, please contact the [DevRel team](mailto:devrel@kontent.ai).

Store the license in the "LICENSE.md" file in the root of the repository because it is being linked from "CONTRIBUTING.md" document.

> The template of the LICENSE file is stored in [repo-template](https://github.com/kontent-ai/repo-template).

### Issue & pull request templates & Code of Conduct

Use [repo-template](https://github.com/kontent-ai/repo-template) for the templates and Code of Conduct.

### Security policy

Use [repo-template](https://github.com/kontent-ai/repo-template) for the template of the "SECURITY.md" file.

## GitHub features
- _🔒 Required for private repositories too_
Decide which features you turn on or off. This will help set expectations.

![GH Features](https://i.imgur.com/i6PICQv.png)

## Expectations
- _🔒 Required for private repositories too_

You should make clear:
- what kind of support users can expect (README)
  - GH issues vs. StackOverflow, etc.
- how to submit bugs (README + Issue/PR templates)
- what the future of the project is and whether it's actively developed (set up a [project/backlog](https://github.com/kontent-ai/delivery-sdk-net/projects) or [archive](https://help.github.com/articles/archiving-a-github-repository/) a repo that's no longer being developed)

In case of private repos, please add the following note to the top of the README:
> 🛈 This repository contains Kontent.ai's internal code that is of no use to the general public. Please explore our [other repositories](https://github.com/kontent-ai).

Set up an issue tracker. Most likely, you'll use GitHub issues. Take your time to set up labels and milestones.

## Badges

Use badges to make it easy to find basic information about the status of the project.

Pro tip: generate custom badges via https://shields.io/ ![Custom Badge](https://img.shields.io/badge/hellow-world-yellowgreen.svg?style=popout&logo=github)

Examples:
* Continuous Integration
    * [![Build & Test](https://github.com/Kentico/kontent-delivery-sdk-net/actions/workflows/integrate.yml/badge.svg)](https://github.com/Kentico/kontent-delivery-sdk-net/actions/workflows/integrate.yml)
* Test coverage
    * [Code Climate](https://codeclimate.com/github/codeclimate/codeclimate/badges) ![Code Climate Coverage](https://api.codeclimate.com/v1/badges/a99a88d28ad37a79dbf6/test_coverage)
* Static code analysis result
    * [Code Climate](https://codeclimate.com/github/codeclimate/codeclimate/badges) ![Code Climate](https://api.codeclimate.com/v1/badges/a99a88d28ad37a79dbf6/maintainability)
    * [SonarCloud](https://sonarcloud.io/documentation/user-guide/project-page/)
* Deployment/Package status
    * [Netlify](https://www.netlify.com/blog/2019/01/29/sharing-the-love-with-netlify-deployment-badges/)
    * [npm](https://docs.npmjs.com/)
    * [nuget](https://docs.microsoft.com/en-us/nuget/)
* Chat
    * [Stack Overflow](https://stackoverflow.com/) [![Stack Overflow](https://img.shields.io/badge/Stack%20Overflow-ASK%20NOW-FE7A16.svg?logo=stackoverflow&logoColor=white)](https://stackoverflow.com/tags/kontent-ai)
    * [Discord](https://discord.gg/SKCxwPtevJ) [![Discord](https://img.shields.io/discord/821885171984891914?label=Discord&logo=Discord&logoColor=white)](https://discord.gg/SKCxwPtevJ) (![Konten Discord](https://img.shields.io/discord/821885171984891914?color=%237289DA&label=Kontent%20Discord&logo=discord))

> ⚠ Try to unify the style of the badge statuses. If it is not possible group the stypes per line.

## Tests
- _❔ Optional, but highly recommended._

Include at least a basic set of (unit) tests.

## Review
- Ask your colleagues to do a code review, basic testing, and proofreading before you publish any project. The [DevRel team](mailto:devrel@kontent.ai) may also help.

## Continuous Integration
- _❔ Optional, but highly recommended._

Setting up CI, makes it easy for contributors to know whether their code works as expected. You can find more info about CI practices in the [separate article](./CI-and-Automation-Guidelines.md).

- Set up a build agent - [GitHub Actions](https://docs.github.com/en/actions)
  - Make it run tests
  - Fail builds on failed tests
- Set up [status checks](https://docs.github.com/en/github/administering-a-repository/about-protected-branches#require-status-checks-before-merging) via webhooks

## Protect the master branch
You can learn more about branch protection in the [documentation](https://docs.github.com/en/github/administering-a-repository/managing-a-branch-protection-rule#about-branch-protection-rules).
![Branch protection](https://github.com/kontent-ai/kontent-ai.github.io/assets/52500882/d4d9ba86-a1d0-488f-a7db-300c47f38298)


## Add collaborating teams
- _🔒 Required for private repositories too_
![Collaborators](https://i.imgur.com/0qkbWe1.png)
>In most cases, it'll be `Admin` permission for the Developer Relations team and `Write` permission for the Employees team.

## Releases
- Create an initial release
- **Always** follow [Semantic Versioning](http://semver.org/)

## Want to make the repo even more friendly?
- Read the [Pre-launch checklist](https://opensource.guide/starting-a-project/#your-pre-launch-checklist).


