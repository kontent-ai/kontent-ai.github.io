---
layout: default
title: Ruby GitHub actions guidelines
parent: CI & automation
---

# Ruby GitHub actions guidelines

## What the Action does
The action runs with the GitHub release. The action installs dependencies, runs tests, and publishes gem to [Rubygems](https://rubygems.org/gems/kontent-delivery-sdk-ruby).

## Notes
- It's recommended to use Linux machine for building - `jobs.<name>.runs-on` property.
- For publishing, the action uses 3rd party [dawidd6/action-publish-gem@v1](https://github.com/dawidd6/action-publish-gem) action.
- The SDK uses GitHub releases.

## [GitHub Action example](https://github.com/kontent-ai/kontent-delivery-sdk-ruby/blob/master/.github/workflows/publish-gem.yml)
- Build test and publish gem to Rubygems
```yaml
name: publish-gem

on:
  release:
    types: [published]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up Ruby
      uses: ruby/setup-ruby@v1
      with:
        ruby-version: 2.6
    - name: Install dependencies
      run: bundle install
    - name: Run tests
      run: bundle exec rake
    - name: Publish gem
      uses: dawidd6/action-publish-gem@v1
      with:
        api_key: ${{secrets.RUBYGEMS_API_KEY}}
```