---
layout: default
title: JS/TS actions guidelines
parent: Guidelines for JS libraries
grand_parent: CI & automation
---

# JS/TS actions guidelines

At this time, the main CI used for building and publishing projects using JavaScript/TypeScript is [CircleCI](https://circleci.com/). The main target is to migrate all automation tasks to GitHub Actions. The research is being conducted, nevertheless, without any specific ETA yet. Right now, releasing the packages is performed manually.

## What the Action does
The Action install [npm](http://npmjs.com/) dependencies and run script defined in packages.json - it might be building testing or publishing of the artifact.

## Notes
- If the project contains browser logic or browser tests,  it might be a good idea to use the Windows environment in `jobs.<name>.runs-on property`. The default virtual environments come with a lot of preinstalled [browsers and tools](https://github.com/actions/virtual-environments/releases).
- The action uses [actions/setup-node@v1](https://github.com/actions/setup-node) action for setting Node.js environment.
- `node-version` array in the `strategy` section can be easily extended with the new versions.

## [GitHub Action example](https://github.com/kontent-ai/kontent-ai-delivery-sdk-js/blob/master/.github/workflows/test.yml)
- The action runs a command defined in the `package.json` file.
```yaml
name: Test
on: [pull_request]
jobs:
  build:
    runs-on: windows-latest
    strategy:
      matrix:
        node-version: [14.x]
    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm ci
    - run: npm run test:all
```

## Publish to npm with GitHub release

If you want to publish your package to npm, the best way it to have an action trigger when you create a GitHub release. The published package version should be taken from the release's version (tag) and the version should be commited back into the `package.json` by the action. You can find an example of such an action [here](https://github.com/kontent-ai/react-components/blob/main/.github/workflows/release.yml).

> **Warning**
> 
> During the release-triggered action the checked-out ref is the tag of the release. To be able to commit, you will need to explicitly specify to what branch you want to commit or check out the branch.