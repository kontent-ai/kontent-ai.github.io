---
layout: default
title: .NET GitHub actions
parent: CI & automation
has_children: true
---

# .NET GitHub actions guidelines

## What the action does
The action gets the tag version, restores dependencies, and builds the project with a given configuration. The artifact is then packaged and published to [NuGet](https://www.nuget.org/) and uploaded to [GitHub Releases](https://docs.github.com/en/github/administering-a-repository/about-releases).
## Notes
- Setting up of the .NET environment is performed by the [actions/setup-dotnet@v4](https://github.com/actions/setup-dotnet) action. The .NET SDK version is pinned via `global.json` (`global-json-file: global.json`).
- The release version is derived from the Git tag (`GITHUB_REF_NAME`) directly in a shell step - no third-party action is needed.
- Dependencies are restored by the `dotnet restore` command.
- Build is performed by the `dotnet build` command provided with configuration info and the extracted tag version.
- NuGet package artifacts are uploaded using the [actions/upload-artifact@v4](https://github.com/actions/upload-artifact) action.
- Authentication to NuGet.org uses the [NuGet/login@v1](https://github.com/NuGet/login) action together with OIDC ([trusted publishing](https://learn.microsoft.com/en-us/nuget/nuget-org/trusted-publishing)), so no long-lived API key needs to be stored.
- Publishing of the NuGet package is performed by the `dotnet nuget push` command.
- Package versions are set using `${{ steps.get_version.outputs.version-without-v }}`.
- Artifacts are uploaded to the GitHub Release using the `gh release upload` command.
- Tokens and secrets are stored in the [Organization Secrets](https://github.blog/changelog/2020-05-14-organization-secrets/).

## [GitHub Action example](https://github.com/kontent-ai/delivery-sdk-net/blob/master/.github/workflows/release.yml)
```yaml
name: Publish to NuGet
on:
  release:
    types: [published]
jobs:
  publish:
    name: Pack and publish
    runs-on: ubuntu-latest
    env:
      PACK_OUTPUT: ./artifacts/release-pack
    permissions:
      id-token: write   # Required for OIDC token issuance (NuGet trusted publishing)
      contents: write   # Required for uploading release assets
    steps:
    - uses: actions/checkout@v4
    - name: Setup .NET
      uses: actions/setup-dotnet@v4
      with:
        global-json-file: global.json
    - name: Extract version from tag
      id: get_version
      run: |
        VERSION="${GITHUB_REF_NAME#v}"
        echo "version-without-v=$VERSION" >> $GITHUB_OUTPUT
    - name: Restore dependencies
      run: dotnet restore
    - name: Build
      run: dotnet build --no-restore --configuration Release /p:ContinuousIntegrationBuild=true /p:Version="${{ steps.get_version.outputs.version-without-v }}"
    - name: Pack
      run: dotnet pack --no-build --include-symbols --configuration Release --output "$PACK_OUTPUT" /p:PackageVersion="${{ steps.get_version.outputs.version-without-v }}"
    - name: Upload artifacts
      uses: actions/upload-artifact@v4
      with:
        name: "NuGet packages"
        path: ${{ env.PACK_OUTPUT }}/*.nupkg
        if-no-files-found: error
    - name: NuGet login
      uses: NuGet/login@v1
      id: nuget-login
      with:
        user: ${{ secrets.NUGET_USER }}
    - name: Publish artifacts to NuGet.org
      run: dotnet nuget push "${PACK_OUTPUT}/*.nupkg" -s https://api.nuget.org/v3/index.json -k ${{ steps.nuget-login.outputs.NUGET_API_KEY }} --skip-duplicate
    - name: Upload artifacts to the GitHub release
      run: gh release upload "$GITHUB_REF_NAME" ${PACK_OUTPUT}/*.nupkg --clobber
      env:
        GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
