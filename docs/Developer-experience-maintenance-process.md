# Developer experience maintenance process

This document describes the process of [repositories maintained by @kontent-ai/developer-experience team](https://github.com/search?q=org%3AKentico+%22%40Kentico%2Fdeveloper-experience%22&type=code).

Some team members have a dedicated **"maintenance day"** in their workload for handling basic operative work and issue triage. These primary contacts are mentioned explicitly in the `CODEOWNERS` file. This group of people is called "DevEx OS maintainers". 

> All [Repository Maintainer duties](./Duties-of-a-Repository-Maintainer.md) applies for DevEx OS maintainers.

## Triage

> Every issue submitted to the repository maintained by DevEx OS maintainers has to be **triaged**. 

Triage means understanding what the issue is about and decide whether this is a bug, feature request, or invalid. 

> Before triage, the maintainer is communicating with the submitter to state that the issue is comprehensible (the maintainer is able to describe the issue).

<We might use labels for triage>

The issue can be resolved as:
* Valid
  * Small enough - it is possible to handle the issue in the "maintainers day"
  * Out of "maintenance day" scope
    * Bug - the bug will be filed in the internal tracking system (Jira) as a "bug" task. These ones are prioritized on weekly basis. *Just keep in mind bug priority is [affected by the project License](https://github.com/kontent-ai/repo-template/blob/master/CONTRIBUTING.md#where-to-get-support) (which is MIT)*.
    * New feature - the feature request will be filed as an issue in GitHub, grouped using GitHub Milestones, and prioritized against other product feature priorities.
      * New feature in the REST API - see (New REST API feature)[#new-rest-api-feature]
  * Waiting for traction - the issue will remain open with [comment informing the issue is waiting for traction](https://github.com/kontent-ai/kontent-management-sdk-net/issues/50#issuecomment-770888281).
* Invalid - the issue was decided not to be implemented.

> Every state decision has to be explained in the comments.

## Sync

DevEx OS maintainers are attending regular **weekly sync** meetings with the DevRel leader to share an update between each other about issue statuses. This platform also works as a fallback when issue triage is complicated. This might lead to a longer response time.


