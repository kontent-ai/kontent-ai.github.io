# Milestones:
- use [milestones](https://docs.github.com/en/github/managing-your-work-on-github/about-milestones) to track what's about to be released (merged PRs)
  - **Title:**
    - when there is no milestone, create a new one called `vNext`
    - once there are issues/PRs assigned to a milestone, keep the version up to date according to [SemVer](https://semver.org)
  - **Description:** use a milestone's description to keep track of fixed issues and breaking changes (to be used later when releasing a new version)

# Releasing a new version:
Once you're happy with the state of the `master` branch and want to release a new version:

1. Go to [Releases](https://docs.github.com/en/github/administering-a-repository/managing-releases-in-a-repository)
2. [Draft a new release](https://docs.github.com/en/github/administering-a-repository/managing-releases-in-a-repository#creating-a-release)
    - **Tag:** version according to [SemVer](https://semver.org), e.g. `4.0.0` or `v4.0.0` - both will work 
    - **Title:** can be a version number or a version number with a codename `4.0.0 - Cannonball`
    - **Description:** copy & past new features, bugfixes, and breaking changes from the milestone's description
    - **Pre-release:** check if the release is a pre-release and contains a [pre-release suffix](https://semver.org/#spec-item-9)
3. Publish the release
4. GitHub Actions will build the source using the `Release` configuration and upload the resulting artifacts to NuGet (and to the GitHub Release)
5. Close the current milestone and create a new one called `vNext`

