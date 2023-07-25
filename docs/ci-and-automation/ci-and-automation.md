---
layout: default
title: CI & automation
has_children: true
nav_order: 9
---

# Continuous Integration and Continuous Delivery
> GitHub Actions are our primary automation platform - whenever it's possible, we recommend using [GitHub Actions](https://github.com/features/actions) - this will helps us with better maintenance. This page contains general info about GitHub Actions and their usage. Subpages focus on the specific stacks.

**Continuous integration** is the automation of building and testing. This means that typically, with some code changes, the machine checks whether it’s possible to build the project and whether your tests are passing. **Continuous delivery** is a little bit broader. To sum it up, this automation process will deploy or publish your code to various environments. This might be a little bit abstract and connected with the nature of your project, but you can visualize it as publishing to package registries like NPM or Nuget—or deploying your site to a staging or production environment.

For most Kontent.ai repositories, [we highly recommend](../Checklist-for-publishing-a-new-OS-project.md#continuous-integration) adding automation workflows. With some basic automation, we want to perform automatic checks, tests and builds. Moreover, when it makes sense, it's a good practice to automate deploying or publishing processes to the respective registry. With this initiative, we aim to reduce human errors and increase productivity.

## GitHub Actions
GitHub Actions are the feature of GitHub that consists of the API and an environment for running your tasks. You just need to create a YAML file at a specific location in the repository. The configuration .yml file contains all the specific information about the environment, such as [trigger events](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions), [jobs](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobs), or [strategies](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstrategy). Additionally, you can choose the environment where you want to run your tasks—it might be Linux, Windows, or even macOS.

## The anatomy of the Action
Each action is represented by a `.yml` file located directly in the repository at the `.github/workflows` location. To be able to run it, you need to allow the Actions feature in the repository’s Settings section.

## Secrets
It's common, the Action needs API keys or secrets to be able to release the version or perform some automation tasks. The practice is to use the [Repository](https://docs.github.com/en/actions/reference/encrypted-secrets) or [Organization secrets](https://github.blog/changelog/2020-05-14-organization-secrets/).

## YAML Cheatsheet
If you are not familiar with the `YAML` format, for the purposes of the GitHub Actions, you just need to know that key-value pairs are represented by `key: value`. The quotes for string values are optional. Nesting of objects is done by indentation (typically two spaces), and items of the array are denoted by a dash.

```yaml
key: value
  nested_property: Nested value
  my_array:
    - First Item
    - Second Item
```

### GitHub Actions basic syntax keys
* [name](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#name) - The name of the workflow.
* [on](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#on) - Is required, specifies events when you want to run your action.
* [jobs](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobs) - A job is a unit of work; jobs run in parallel by default. Each job runs in a runner environment specified by runs-on.
* [jobs.<job_id>.runs-on](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) - Is required, specifies the type of the machine to run the job.
* [jobs.<job_id>.steps](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idsteps) - A step is an individual task that can run commands in a job. A step can be either an action or a shell command. Each step in a job executes on the same runner, allowing the actions in that job to share data with each other ([definition by GitHub](https://docs.github.com/en/actions/learn-github-actions/introduction-to-github-actions#steps)).
* [jobs.<job_id>.steps[*].uses](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstepsuses) - Selects an action to run as part of a step in your job. This is beneficial for reusing existing actions. You can check existing actions in the official [actions repository](https://github.com/actions) or on the [marketplace](https://github.com/marketplace?type=actions).
* [jobs.<job_id>.runs-on](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on) - Provides a shell where you can run your commands.

### Hello World Action
Let’s write the action that runs the code analysis tool every week. This action will run on Ubuntu, build the project, and will analyze code from our repository. The explanation of each line is described in the respective comment directly in the code. This example comes from the [.NET Delivery SDK repository](https://github.com/kontent-ai/kontent-delivery-sdk-net/blob/master/.github/workflows/codeql-analysis.yml).
```yaml
name: "Code scanning - action"   # Represents the name of the whole action.
on:   # Specifies the section where we describe our build triggers.
  push: # The action runs with push.
  pull_request: # The action runs with a pull request.
  schedule: # Specifies the section where we describe the schedule of running the action.
    - cron: '0 18 * * 1' # The CRON expression describing when to run the action.
jobs: # Specifies the section where we describe our jobs.
  CodeQL-Build: # Specific job section.
    runs-on: ubuntu-latest   # Describes the environment.
    steps: # Specifies the section where we describe the job's steps.
    - name: Checkout repository # The name of the step.
      uses: actions/checkout@v2 # Using already existing action actions/checkout@v2. This action provides us with access to the code of the repository.
      with: # Configuration of the action.
        fetch-depth: 2 # We must fetch at least the immediate parents so that if this is a pull request then we can checkout the head.
    - run: git checkout HEAD^2   # If this run was triggered by a pull request event, then checkout the head of the pull request instead of the merge commit.
      if: ${{ github.event_name == 'pull_request' }}   
    - name: Initialize CodeQL   # Initializes the CodeQL tools for scanning.
      uses: github/codeql-action/init@v1
      with:   # Override language selection by uncommenting this and choosing your languages
        languages: csharp
    - name: Setup .NET Core
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: 5.0.x
    - name: Install dependencies
      run: dotnet restore   # Command for the shell environment.
    - name: Build
      run: dotnet build --configuration Release --no-restore
    - name: Perform CodeQL Analysis
      uses: github/codeql-action/analyze@v1
```

### Commits to protected branches
You might come up with a script that tries to commit to the main branch which should be under protection rules. For instance, a workflow that at first changes the version of the package and then commits it into the package.json). These steps would require setting up [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) and enabling force pushes into the branch. Due to these inconveniences, we decided to not use this solution and rather find other ways to release via action (e.g. set the version of the package manually and then only do the release in Github Action).

### Real-world examples and syntax
It’s not possible to cover all capabilities and combinations for each stack here. Nevertheless, in the subpages, you can find some real-world examples—you can choose them for scaffolding your Action quickly for your specific stack and use case. They showcase configurations for various languages, platforms, and publishing to external package repositories. However, before you start with more advanced Actions, you'll need some more commands.
* [on.release.types](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#release) - Runs the action when a specific release event occurs.
* [on.<push|pull_request>.<branches|tags>](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#onpushpull_requestbranchestags) - Runs an action when push or pull request event occurs on a specific branch.
* [jobs.<job_id>.strategy.matrix](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix) - Allows to set different job configurations.
* [env](https://docs.github.com/en/actions/reference/context-and-expression-syntax-for-github-actions#env-context) - Context for specified environment variables.
* [env.NODE_AUTH_TOKEN](https://docs.github.com/en/actions/guides/publishing-nodejs-packages#publishing-packages-to-the-npm-registry) - Specifies the location of stored [NPM_TOKEN](https://docs.npmjs.com/creating-and-viewing-access-tokens). It’s a good practice to store secrets and keys to [Encrypted Secrets](https://docs.github.com/en/actions/reference/encrypted-secrets) storage of the repository.
* [jobs.<job_id>.if](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idif) - The job will run if the condition is met.

## Resources
* GitHub Actions [documentation](https://docs.github.com/en/actions)
* An article about GitHub Actions containing Kontent.ai specific examples: [With GitHub Actions, you don’t have to do boring tasks manually ever again](https://hackernoon.com/with-github-actions-you-dont-have-to-do-boring-tasks-manually-ever-again-301p356e)
