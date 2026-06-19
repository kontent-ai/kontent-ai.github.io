---
layout: default
nav_order: 6
---

# Guidelines for Kontent.ai-related tools

Welcome to Kontent.ai, developers! These guidelines should help you meet all the requirements for creating successful tools around Kontent.ai APIs aligned with our values. It should also give you a solid starting point for development using the framework of your choice. They apply to tools and SDKs built on any Kontent.ai API - [Delivery](https://kontent.ai/learn/reference/delivery-api), [Management](https://kontent.ai/learn/reference/management-api-v2), [Assets](https://kontent.ai/learn/reference/image-transformation), and others.

> Building a full SDK specifically for the **Delivery API**? See the dedicated [Guidelines for SDK developers](./Guidelines-for-SDK-developers.md), which add the Delivery-specific functional requirements (required endpoints, returned data format, backward compatibility) on top of the general guidance here.

First of all, you should visit the [Kontent.ai Docs](https://kontent.ai/learn) which gives you overall knowledge about Kontent.ai possibilities and existing tools. Then check out the values of [Kontent.ai GitHub organization](https://github.com/kontent-ai/.github#readme).

## Guides

Once you know what you want to achieve and you haven't found an existing tool for that, read through the [naming conventions](./Naming-conventions.md) and pick the right name for your tool. 

Before you publicly offer any tool, it is recommended to read through the [Kontent.ai Github checklist for publishing an OS project](./Checklist-for-publishing-a-new-OS-project.md) to be aware of all enhancements you could implement to make the project more credible and well-perceived by the community.

## Analytics

To be able to better measure the adoption of your project, the tool should include the `X-KC-SOURCE` header with each request sent to Kontent.ai API ([Delivery](https://kontent.ai/learn/reference/delivery-api), [Management](https://kontent.ai/learn/reference/management-api-v2), and [Assets](https://kontent.ai/learn/reference/image-transformation)). The value of the header should be formatted as follows: `<Tool identification>;<Tool version>`. For example, `Acme.Kontent.Ai.AwesomeTool;1.0.0` or `@kontent-ai/awesome-tool-kontent-ai;0.1.42`. The version itself is not required, you can use just `Tool identification` like `kontent-ai-nuxt-module`. However, we strongly recommend including the version to be able to track adoption among multiple versions.

If you follow this recommendation, we can provide you with aggregated analytic data about your tool's usage later on.

## Naming convention

You can also use the [official Naming Conventions by Kontent.ai](./Naming-conventions.md#tagging).

---

If you have any additional questions or comments, you can contact us directly at devrel@kontent.ai.
