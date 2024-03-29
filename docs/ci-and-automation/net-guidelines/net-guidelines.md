---
layout: default
title: .NET GitHub actions
parent: CI & automation
has_children: true
---

# .NET GitHub actions guidelines

## What the action does
The action gets the tag version, restores dependencies, and builds the project with a given configuration. Then the tests and code coverage tools are run. Eventually, the artifact is packaged and uploaded to the [Nuget](https://www.nuget.org/) and [GitHub Releases](https://docs.github.com/en/github/administering-a-repository/about-releases).
## Notes
- Setting up of .NET environment is performed by [actions/setup-dotnet@v3](https://github.com/actions/setup-dotnet) action.
- Version tag is extracted using 3rd party [battila7/get-version-action@v2](https://github.com/battila7/get-version-action) action.
- Dependencies are restored by `run: dotnet restore` command.
- Build is performed by `dotnet build` command provided with configuration info and extracted tag version.
- Tests are performed by `dotnet test` command with respective parameters.
- Nuget packages artifacts are uploaded using [actions/upload-artifact@v3](https://github.com/actions/upload-artifact) action.
- Publishing of the Nuget Package is Performed by `dotnet nuget push` command with respective parameters and API keys.
- Package versions are set using `${{ steps.get_version.outputs.version-without-v }}` - you can find more info in the [documentation of the Action](https://github.com/marketplace/actions/get-version#version-without-v).
- Artifact is uploaded to GitHub Releases by [Roang-zero1/github-upload-release-artifacts-action@v3.0.0](https://github.com/Roang-zero1/github-upload-release-artifacts-action) action.
- Tokens and secrets are stored in the [Organization Secrets](https://github.blog/changelog/2020-05-14-organization-secrets/).

## [GitHub Action example](https://github.com/kontent-ai/kontent-ai-delivery-sdk-net/blob/master/.github/workflows/release.yml)
```yaml
name: Publish to NuGet
on:
  release:
    types: [published]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: 5.0.x
    - name: Extract version from tag
      id: get_version
      uses: battila7/get-version-action@v2
    - name: Restore dependencies
      run: dotnet restore
    - name: Build
      run: dotnet build --no-restore --configuration Release /p:ContinuousIntegrationBuild=true /p:Version="${{ steps.get_version.outputs.version-without-v }}"
    - name: Test
      run: dotnet test --no-build --verbosity normal --configuration Release /p:CollectCoverage=true /p:CoverletOutputFormat=opencover
    - name: Codecov
      uses: codecov/codecov-action@v3
    - name: Pack
      run: dotnet pack --no-build --include-symbols --verbosity normal --configuration Release --output ./artifacts /p:PackageVersion="${{ steps.get_version.outputs.version-without-v }}"
    - name: Upload artifacts
      uses: actions/upload-artifact@v3
      with:
        name: "NuGet packages"
        path: ./artifacts
    - name: Publish artifacts to NuGet.org
      run: dotnet nuget push './artifacts/*.nupkg' -s https://api.nuget.org/v3/index.json -k ${NUGET_API_KEY} --skip-duplicate
      env:
        NUGET_API_KEY: ${{ secrets.NUGET_API_KEY }}
    - name: Upload artifacts to the GitHub release
      uses: Roang-zero1/github-upload-release-artifacts-action@v3.0.0
      with:
        args: ./artifacts
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
