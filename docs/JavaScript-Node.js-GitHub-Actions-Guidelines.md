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