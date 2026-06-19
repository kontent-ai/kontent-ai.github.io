---
layout: default
title: Building SDKs & tools
nav_order: 3
---

# Building SDKs & tools

This page covers what you need to build an SDK or tool around the Kontent.ai APIs - [Delivery](https://kontent.ai/learn/reference/delivery-api), [Management](https://kontent.ai/learn/reference/management-api-v2), [Assets](https://kontent.ai/learn/reference/image-transformation), and others. For everything the APIs can do, see the [API reference](https://kontent.ai/learn/reference).

> The **tracking headers** and **naming & publishing** guidance apply to any Kontent.ai-based tool. The **functional** and **backward-compatibility** expectations are specific to **Delivery API SDKs**.

## Tracking headers

Help us measure adoption by sending an identifying header with every request to the API:

- **SDKs** - send `X-KC-SDKID` formatted as `<package origin>;<SDK name>;<SDK version>`, e.g. `nuget.org;Kontent.Ai.Delivery;19.0.0`.
- **Tools & apps** - send `X-KC-SOURCE` formatted as `<tool identification>;<tool version>`, e.g. `@kontent-ai/awesome-tool;0.1.42` (the version is optional but recommended).

With these in place, we can share aggregated usage data about your project.

> Delivery SDKs can also send `X-KC-Wait-For-Loading-New-Content: true` to bypass the Fastly CDN cache and fetch the latest content - useful when reacting to [webhooks](https://kontent.ai/learn/reference/webhooks-reference/).

## What a Delivery SDK should do

A Delivery API SDK should cover the full read surface - content items, content types, and taxonomies (including individual elements) - plus:

- Delivery **Preview** API for unpublished content
- **Filtering**, **localization** (the `language` parameter), and **pagination**
- **Rich text** rendering, link resolution, and retrieving linked items/components
- Statically typed models where the stack supports them (dynamic otherwise)

See the [Delivery API reference](https://kontent.ai/learn/reference/delivery-api/) for endpoints, response shapes, content element data types, and error formats.

> Primary SDKs are **JavaScript/TypeScript** and **.NET**; **Java** is maintained on a best-effort basis. The **PHP** and **Ruby** Delivery SDKs are archived and community-maintained.

### Rate limits

CDN-served (cached) requests aren't rate limited. Uncached requests are capped at 100/second and 2000/minute; over the limit the API returns `429` with a `Retry-After` header. All failed requests are safe to retry. GET URLs are limited to 2048 characters.

## Backward compatibility (Delivery API)

The Delivery API evolves in a backward-compatible way. Your SDK should tolerate these **non-breaking** changes:

- New properties on JSON objects, or reordered properties
- New content element types
- New HTML elements/attributes/values in Rich text, or reordered attributes
- Characters represented as HTML entities

Practical tips: ignore unknown content elements and unknown HTML elements (keep their content, drop the tags), and parse Rich text with a real HTML5 parser rather than regular expressions. You can rely on element and option order matching the Kontent.ai UI and content types. If a genuinely breaking change is ever needed, we'll ship a new API version alongside the current one - with updated guidelines and advance notice.

## Naming & publishing

- Follow the [naming conventions](./Naming-conventions.md) for the repository, namespaces, and package name. SDK namespaces use `Kontent.ai` as the main namespace and the API name (e.g. `Delivery`) as the sub-namespace.
- Before publishing, run through the [checklist for publishing an OS project](./Checklist-for-publishing-a-new-OS-project.md).

---

Questions? Reach the DevRel team at devrel@kontent.ai.
