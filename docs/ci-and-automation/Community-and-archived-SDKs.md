---
layout: default
title: Community & archived SDKs
parent: CI & automation
nav_order: 99
---

# Community & archived SDKs

Kontent.ai actively maintains SDKs for two primary stacks:

- **.NET** – see the [.NET GitHub actions guidelines](./net-guidelines/net-guidelines.md)
- **JavaScript / TypeScript** – see the [JS/TS actions guidelines](./js-guidelines/JavaScript-Node.js-GitHub-Actions-Guidelines.md)

For these stacks you'll find detailed, maintained CI/CD guidance in their respective subpages.

## Other language SDKs

The SDKs for the languages below are **archived and community-maintained**. They remain available for use, but Kontent.ai no longer ships updates or accepts contributions to the original repositories. If you need to keep one of them current, **fork the repository** and maintain it under your own account or organization.

| Language | Repository | Package |
| --- | --- | --- |
| PHP | [kontent-ai/delivery-sdk-php](https://github.com/kontent-ai/delivery-sdk-php) | [Packagist](https://packagist.org/packages/kontent-ai/delivery) |
| Ruby | [kontent-ai/delivery-sdk-ruby](https://github.com/kontent-ai/delivery-sdk-ruby) | [RubyGems](https://rubygems.org/gems/kontent-delivery-sdk-ruby) |

## Setting up CI on a fork

If you fork one of the archived SDKs (or build a new community SDK from scratch), the general [GitHub Actions concepts](./ci-and-automation.md) page covers the fundamentals — triggers, jobs, runners, secrets, and the basic workflow anatomy — that apply to any stack. From there, adapt the publishing step to your language's package registry.

When building any tool against the Kontent.ai APIs, please also follow the [Guidelines for SDK developers](../Guidelines-for-SDK-developers.md) and the [Checklist for publishing a new OS project](../Checklist-for-publishing-a-new-OS-project.md).
