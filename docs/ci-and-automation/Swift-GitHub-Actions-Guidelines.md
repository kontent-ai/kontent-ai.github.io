---
layout: default
title: Swift GitHub actions guidelines
parent: CI & automation
---

# Swift GitHub actions guidelines

## What the Action does
Once the release is triggered, dependencies are installed, then the project is built, tested and a new version is released to [Cocoapods](https://cocoapods.org/).

## Notes
- It is recommended to run builds for Swift/Apple platforms on a macOS machine - `jobs.<name>.runs-on` property.
- Target platforms are specified in `strategy.matrix.destination` property.
- Manual steps for the releasing of the new version - https://github.com/kontent-ai/kontent-delivery-sdk-swift#releasing-a-new-version-of-the-cocoapod-package
- Before building, the [Cocoapods package manager](https://cocoapods.org/) is installed, repo (trunk mirror) is updated and dependencies are installed.
```bash
gem install cocoapods
pod repo update
pod install --project-directory=Example
set -o pipefail && xcodebuild test -enableCodeCoverage YES -workspace Example/KontentAiDelivery.xcworkspace -scheme KontentAiDelivery-Example -sdk iphonesimulator -destination 'name=iPhone 11' ONLY_ACTIVE_ARCH=NO | xcpretty
```
- Built dependency pod is pushed to the [Cocoapod trunk](https://guides.cocoapods.org/making/getting-setup-with-trunk.html).
```bash
pod trunk push
```

## [GitHub Action example](https://github.com/kontent-ai/kontent-delivery-sdk-swift/blob/master/.github/workflows/publish.yml)
- Build test and publish to Cocoapods project written in Swift

```yaml
on:
  release:
    types: [published]
name: publish-to-cocoapods
jobs:
  publish-to-cocoapods:
    name: publish-to-cocoapods
    runs-on: macOS-latest
    strategy:
        matrix:
          destination: ['platform=iOS Simulator,OS=12.2,name=iPhone 11']
    steps:
      - name: Checkout
        uses: actions/checkout@master
      - name: Build and test
        run: |
          gem install cocoapods
          pod repo update
          pod install --project-directory=Example
          set -o pipefail && xcodebuild test -enableCodeCoverage YES -workspace Example/KontentAiDelivery.xcworkspace -scheme KontentAiDelivery-Example -sdk iphonesimulator -destination 'name=iPhone 11' ONLY_ACTIVE_ARCH=NO | xcpretty
      - name: Publish Cocoapod
        run: pod trunk push
        env:
          COCOAPODS_TRUNK_TOKEN: ${{ secrets.COCOAPODS_TRUNK_TOKEN }}
```