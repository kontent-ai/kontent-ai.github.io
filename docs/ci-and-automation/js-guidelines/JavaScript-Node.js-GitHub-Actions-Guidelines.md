---
layout: default
title: JS/TS actions guidelines
parent: Guidelines for JS libraries
grand_parent: CI & automation
---

# JS/TS actions guidelines

At this time, the main CI used for building and publishing projects using JavaScript/TypeScript is Github Actions.

## What the Action does
The Action install [npm](http://npmjs.com/) dependencies and can run scripts defined in `package.json` - for instance, building, testing or publishing the artifact.

## Notes
- If the project contains browser logic or browser tests,  it might be a good idea to use the Windows environment in `jobs.<name>.runs-on property`. The default virtual environments come with a lot of preinstalled [browsers and tools](https://github.com/actions/virtual-environments/releases).
- The action uses [actions/setup-node@v2](https://github.com/actions/setup-node) action for setting Node.js environment.
- Since `actions/setup-node@v2` you can set the node version using `.nvrmc file`.

## [GitHub Action example](https://github.com/kontent-ai/delivery-sdk-js/blob/master/.github/workflows/test.yml)
- This action runs a command `test:all` defined in the `package.json` file.

```yaml
name: Test
on: [pull_request]
jobs:
  build:
    runs-on: windows-latest
    steps:
    - uses: actions/checkout@v2
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v2
      with:
        node-version-file: '.nvmrc'
    - run: npm ci
    - run: npm run test:all
```

### .nvmrc example
- This file is typically at the root of the project.
```
lts/*
```

### Codecov support

You can register the repository to be used in the [Codecov](https://about.codecov.io/) test coverage.

Kontent.ai GitHub organization has the [Codecov application](https://github.com/apps/codecov) allowed for all repositories, so the only thing to do is to add additional [codecov github action](https://github.com/codecov/codecov-action) step into the github action workflow.

```yml
steps:
- uses npm run text # mostly `jest --collect-coverage` - default reporter is compatible with Codecov
- uses: codecov/codecov-action@v3
  with:
    token: ${{ secrets.CODECOV_TOKEN }} # not required for public repos
    files: ./coverage1.xml,./coverage2.xml # optional
    flags: unittests # optional
    name: codecov-umbrella # optional
    fail_ci_if_error: true # optional (default = false)
    verbose: true # optional (default = false)
``` 

> Feel free to get inpired by this [PR adding codecov to Kontent CLI](https://github.com/kontent-ai/cli/pull/46).

## Publish to npm with GitHub release

If you want to publish your package to npm, the best way it to have an action trigger when you create a GitHub release. The published package version should be taken from the release's version (tag) and the version should be commited back into the `package.json` by the action. You can find an example of such an action [here](https://github.com/kontent-ai/react-components/blob/main/.github/workflows/release.yml).

> **Warning**
> 
> During the release-triggered action the checked-out ref is the tag of the release. To be able to commit, you will need to explicitly specify to what branch you want to commit or check out the branch.

### Prerelease publishing

When creating the release in GitHub, you can opt to publish the prerelease. This can be used to define the release of the versions other than with the `latest` tag on `npm`. This is convenient for testing the versions which are not meant to be in production. To find out, in action context, whether the version is being published in prerelease use yaml condition with the following command (`${{!github.event.release.prerelease}}`). To see how to implement it check the example below.

```yaml
on:
  release:
    types: [published]

name: publish-to-npm
jobs:
  publish:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js
        uses: actions/setup-node@v2
        with:
          node-version-file: '.nvmrc'
          registry-url: 'https://registry.npmjs.org'
      - run: npm install
      - run: npm run lint
      - run: npm run build
      - run: npm publish 
        if: ${{!github.event.release.prerelease}}
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_API_KEY }}
      - run: npm publish --tag prerelease
        if: ${{github.event.release.prerelease}}
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_API_KEY }}
```
