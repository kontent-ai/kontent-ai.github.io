---
layout: default
title: Publishing checklist
nav_order: 1
---

# Checklist for publishing a new OS project

Before you publish any repository under Kontent.ai organization on GitHub, please go through this checklist and make sure the repository is up to standards.

> 💡 **Starting a new repository?** Create it from our [`repo-template`](https://github.com/kontent-ai/repo-template) (hit **Use this template** on GitHub). It comes pre-loaded with the LICENSE, a README template, CONTRIBUTING and CODE_OF_CONDUCT files, a SECURITY policy, and issue/PR templates - covering a large part of this checklist out of the box.

* [Repository name](#repository-name)
* [Dedicate a maintainer](#dedicate-a-maintainer)🔒
* [Fulfill Community profile](#community-profile)🔒
* [Set up GitHub features](#github-features)🔒
* [Set Expectations](#expectations)🔒
* [Add badges](#badges)
* [Add automatic tests](#tests)❔
* [Set up Continuous Integration](#continuous-integration)❔
* [Add AI agent context (CLAUDE.md)](#ai-agent-context-claudemd)❔
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

### Description, website, and topics
- _🔒 Required for private repositories too_

Fill in basic information about the project to make it easy to find it.

⚠ Tag the repository based on the division according to [Naming conventions](./Naming-conventions.md#github-repositories).

In the case of private repositories, add a "private-repository" tag.

### README (Documentation)
- _🔒 Required for private repositories too_

README should contain:

- installation instructions
- basic demonstration of usage
- code examples (if applicable)

More complex topics and examples can be covered in separate articles in GitHub Wiki (or an external system such as ReadTheDocs).

> The template of the README file is stored in [special ".github" repository](https://github.com/kontent-ai/.github).

### Contributing
From the README or CONTRIBUTING files, it should be clear:

- how to set up the project in order to contribute
  - this may include creating a PowerShell or other (e.g. build) script to make it easy for the contributors
- what kind of contributions are accepted and welcome
- what's the definition of done (use PR templates)
- which communication channels should be used to get in touch with the maintainer

> The template of CONTRIBUTING file is stored in [repo-template](https://github.com/kontent-ai/repo-template).

### License

Use the MIT license and set "Kontent s.r.o." as the copyright holder. If you want to use a different license, please contact the [DevRel team](mailto:devrel@kontent.ai).

Store the license in the "LICENSE.md" file.

> The template of the LICENSE file is stored in [special ".github" repository](https://github.com/kontent-ai/.github).

### Issue & pull request templates & Code of Conduct

Automatically used from [special ".github" repository](https://github.com/kontent-ai/.github), [see the docs for more details](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file).

### Security policy

Automatically used from [special ".github" repository](https://github.com/kontent-ai/.github), [see the docs for more details](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file).

## GitHub features
- _🔒 Required for private repositories too_
Decide which features you turn on or off. This will help set expectations.

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
    * [![Build & Test](https://github.com/kontent-ai/delivery-sdk-net/actions/workflows/integrate.yml/badge.svg)](https://github.com/kontent-ai/delivery-sdk-net/actions/workflows/integrate.yml)
* Test coverage
    * [Code Climate](https://codeclimate.com/github/codeclimate/codeclimate/badges) ![Code Climate Coverage](https://api.codeclimate.com/v1/badges/a99a88d28ad37a79dbf6/test_coverage)
* Static code analysis result
    * [Code Climate](https://codeclimate.com/github/codeclimate/codeclimate/badges) ![Code Climate](https://api.codeclimate.com/v1/badges/a99a88d28ad37a79dbf6/maintainability)
    * [SonarCloud](https://sonarcloud.io/documentation/user-guide/project-page/)
* Deployment/Package status
    * [Netlify](https://www.netlify.com/blog/2019/01/29/sharing-the-love-with-netlify-deployment-badges/)
    * [npm](https://docs.npmjs.com/)
    * [nuget](https://learn.microsoft.com/en-us/nuget/)
* Chat
    * [Stack Overflow](https://stackoverflow.com/) [![Stack Overflow](https://img.shields.io/badge/Stack%20Overflow-ASK%20NOW-FE7A16.svg?logo=stackoverflow&logoColor=white)](https://stackoverflow.com/tags/kontent-ai)
    * [Discord](https://discord.gg/SKCxwPtevJ) [![Discord](https://img.shields.io/discord/821885171984891914?label=Discord&logo=Discord&logoColor=white)](https://discord.gg/SKCxwPtevJ) (![Kontent Discord](https://img.shields.io/discord/821885171984891914?color=%237289DA&label=Kontent%20Discord&logo=discord))

> ⚠ Try to unify the style of the badge statuses. If it is not possible, group the styles per line.

## Tests
- _❔ Optional, but highly recommended._

Include at least a basic set of (unit) tests.

## Review
- Ask your colleagues to do a code review, basic testing, and proofreading before you publish any project. The [DevRel team](mailto:devrel@kontent.ai) may also help.

## Continuous Integration
- _❔ Optional, but highly recommended._

Setting up CI makes it easy for contributors to know whether their code works as expected. We recommend [GitHub Actions](https://docs.github.com/en/actions) as the automation platform.

- Set up a build agent - [GitHub Actions](https://docs.github.com/en/actions)
  - Make it run tests
  - Fail builds on failed tests
- Set up [status checks](https://docs.github.com/en/github/administering-a-repository/about-protected-branches#require-status-checks-before-merging) via webhooks

## AI agent context (CLAUDE.md)
- _❔ Optional, but recommended for larger repositories or those with significant expected developer traffic._

Repositories with a non-trivial codebase or active contribution should include a curated [`CLAUDE.md`](https://docs.claude.com/en/docs/claude-code/memory) file in the repository root. It gives AI coding agents (and new human contributors) a fast, accurate overview of the project so they can be productive without reverse-engineering the whole codebase.

A good `CLAUDE.md` is concise and **curated** - not auto-generated boilerplate - and typically covers:

- **Purpose** - what the repository is and what problem it solves
- **Architecture overview** - the main projects/packages/modules and how they fit together
- **Common commands** - how to build, test, lint, and run the project locally (including non-obvious flags used in CI)
- **Key patterns & conventions** - architectural patterns, code-style rules, and project-specific idioms an agent should follow
- **Gotchas** - anything surprising: required environment variables, services needed for integration tests, generated code, etc.

Keep it in sync with the codebase as the project evolves - an outdated `CLAUDE.md` is worse than none. For examples, see the `CLAUDE.md` files in [management-sdk-net](https://github.com/kontent-ai/management-sdk-net/blob/master/CLAUDE.md) (.NET) or [rich-text-resolver-js](https://github.com/kontent-ai/rich-text-resolver-js/blob/main/CLAUDE.md) (JS/TS).

> The file is named `CLAUDE.md` for Claude Code; the emerging tool-agnostic equivalent is `AGENTS.md`. You can maintain one and symlink the other if you want to support multiple agents from a single source of truth.

## Protect the master branch
You can learn more about branch protection in the [documentation](https://docs.github.com/en/github/administering-a-repository/managing-a-branch-protection-rule#about-branch-protection-rules).
![Branch protection](https://github.com/kontent-ai/kontent-ai.github.io/assets/52500882/d4d9ba86-a1d0-488f-a7db-300c47f38298)


## Add collaborating teams
- _🔒 Required for private repositories too_
>In most cases, it'll be `Admin` permission for the Developer Relations team and `Write` permission for the Employees team.

## Releases
- Create an initial release
- **Always** follow [Semantic Versioning](https://semver.org/)

## Want to make the repo even more friendly?
- Read the [Pre-launch checklist](https://opensource.guide/starting-a-project/#your-pre-launch-checklist).


