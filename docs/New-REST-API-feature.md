---
layout: default
nav_order: 8
---

# New REST API feature

SDK implementation is part of the feature opportunity. The new functionality should be covered by tests and reviewed by the maintainers (ideally primary contact) defined in the [CODEOWNERS](https://help.github.com/articles/about-code-owners/) file in the SDK repo.

## Ideal process of the submission

Let's image a new entity in Kontent.ai - let's call it a "note". As a part of the opportunity, the development team wants to introduce new endpoints connected to this entity to the Delivery REST API (to allow fetching user's notes) and adjust one other endpoint connected to the new entity - user entity will have a list of notes' IDs in the response. The ideal workflow would look like this:

* Development team decides to implement new opportunity
* Dev team contacts the maintainers of the respective SDKs. In this case .NET and JS SDKs.
    * They introduce the idea to the maintainer to get early feedback and information about SDK implementation.
* Once the feature is defined and the REST API contract is known (not implemented), the development team [submits the GitHub issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue) based on [feature request template](https://github.com/kontent-ai/repo-template/blob/master/.github/ISSUE_TEMPLATE/feature_request.md) describing the new feature to the respective SDK repositories listed in [develop apps overview](https://kontent.ai/learn/tutorials/develop-apps/overview) (such as this one for [.NET](https://github.com/kontent-ai/kontent-management-sdk-net/issues/120) and [JS](https://github.com/kontent-ai/kontent-management-sdk-js/issues/58)). *You can split the functionality into logical parts as separate issues - it is up to maintainer<->dev team agreement.*
    * For primary stacks - **Javascript and .NET** - assigns issue to dev team members -> see [Implementation checklist](#implementation-checklist) for scope.
* After the implementation part is done (implemented and successfully reviewed) - the maintainer will handle the release process.

## Implementation checklist

> ⚠ To submit a PR to the repository, **you need to fork it under your GitHub account and create a PR from your fork**. It will prevent GitHub admins to keep track of the membership over the whole company, only over the maintainers.

> Based on https://github.com/kontent-ai/repo-template/blob/master/.github/PULL_REQUEST_TEMPLATE.md

* Code follows coding conventions held in this repo
    * For .NET repositories the [standard Microsoft coding conventions appies](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
    * [Kontent.ai specific guidelines for repositories](./Guidelines-for-SDK-developers.md) applies
    * If there are any other repo-specific requirements - they are described in the repo's README.
*  Automated tests have been added
    * Sufficient state is that happy path is covered.
        * Ideal state is to have the code fully covered (happy and unhappy path).
    * We do not test implementation of the REST API itself, it is possible to use Mocked server responses.
    * Repositories might contain tests for [Documentation code samples](https://github.com/Kontent-ai-Learn/kontent-ai-learn-code-samples)
* Docs have been updated
    * If new functionality in REST API is being showcased in [API reference](https://kontent.ai/learn/reference)
        * Create a pull request with a code sample based on the SDK you are implementing and for REST (as i.e. [this pull request for REST](https://github.com/Kontent-ai-Learn/kontent-ai-learn-code-samples/pull/42), [example for Javascript pull request](https://github.com/Kontent-ai-Learn/kontent-ai-learn-code-samples/pull/18)), or alternatively sync with the Customer Education to handle this separately. 
            * **⚠ Mind that the code samples should be published after the feature is released in the SDK.**
    * Validate whether there is a need for adjusting README in respective SDK repositories
* Temporary settings (e.g. variables used during development and testing) have been reverted to defaults
    * no sensitive keys have been committed to the codebase
    * no temporary project ID is being set in the pull request


## Release process

> Release process is being handled by maintainers (ideally primary contact) defined in the [CODEOWNERS](https://help.github.com/articles/about-code-owners/) file in the SDK repo.

See [Managing the releases and keeping them in sync with product development](./Duties-of-a-Repository-Maintainer.md#managing-the-releases-and-keeping-them-in-sync-with-product-development) for more information.
