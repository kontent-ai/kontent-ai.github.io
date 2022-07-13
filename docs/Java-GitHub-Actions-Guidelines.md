## What the Action does
In this Action, the package is built and published using [Gradle](https://gradle.org/). The package is deployed to the [Nexus registry](https://www.sonatype.com/products/repository-oss).

## Notes
- It's recommended to use a Linux machine for building - `jobs.<name>.runs-on` property.
- For setting up Java environment, the action uses [actions/setup-java@v1](https://github.com/actions/setup-java) action.
- It's essential to grant execute permission for `gradlew` with `chmod +x gradlew`.
- The release is triggered by the `./gradlew publish` command.
- Secrets, keys, and tokens are stored in .env variables and secret properties.

## [GitHub Action example](https://github.com/kontent-ai/kontent-ai-java-packages/blob/master/.github/workflows/publish.yml)
- Build, test, and publish Java project to Nexus registry
```yaml
name: Publish package to the Maven Central Repository
on:
  release:
    types: [created]
jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Java
        uses: actions/setup-java@v1
        with:
          java-version: 1.8
      - name: Grant execute permission for gradlew
        run: chmod +x gradlew
      - name: Version release
        run: echo Releasing verion ${{ github.event.release.tag_name }}
      - name: Publish package
        run: ./gradlew publish
        env:
          RELEASE_TAG: ${{ github.event.release.tag_name }}
          NEXUS_USERNAME: ${{ secrets.NEXUS_USERNAME }}
          NEXUS_PASSWORD: ${{ secrets.NEXUS_PASSWORD }}
          SIGNING_KEY_ID: ${{ secrets.SIGNING_KEY_ID }}
          SIGNING_KEY: ${{ secrets.SIGNING_KEY }}
          SIGNING_PASSWORD: ${{ secrets.SIGNING_PASSWORD }}
```