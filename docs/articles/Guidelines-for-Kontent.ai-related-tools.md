Welcome to Kontent.ai, developers! These guidelines should help you meet all the requirements for creating successful tools around Kontent APIs aligned with our values. It should also give you a solid starting point for development using the framework of your choice.

First of all, you should visit the [Kontent.ai Docs](https://docs.kontent.ai) which gives you overall knowledge about Kontent.ai possibilities and existing tools. Then check out the values of [Kontent.ai GitHub organization](https://github.com/kontent-ai/.github#readme).

## Guides

Once you know what you want to achieve and you haven't found an existing tool for that, read through the [naming conventions](https://github.com/kontent-ai/.github/wiki/Naming-conventions) and pick the right name for your tool. 

Before you publicly offer any tool, it is recommended to read through the [Kontent.ai Github checklist for publishing an OS project](https://github.com/kontent-ai/.github/wiki/Checklist-for-publishing-a-new-OS-project) to be aware of all enhancements you could implement to make the project more credible and well-perceived by the community.

## Analytics

To be able to better measure the adoption of your project, the tool should include the `X-KC-SOURCE` header with each request sent to Kontent.ai API ([Delivery](https://docs.kontent.ai/reference/delivery-api), [Management](https://docs.kontent.ai/reference/management-api-v2), and [Assets](https://docs.kontent.ai/reference/image-transformation)). The value of the header should be formatted as follows: `<Tool identification>;<Tool version>`. For example, `Acme.Kontent.Ai.AwesomeTool;1.0.0` or `@kontent-ai/awesome-tool-kontent-ai;0.1.42`. The version itself is not required, you can use just `Tool identification` like `kontent-ai-nuxt-module`. However, we strongly recommend including the version to be able to track adoption among multiple versions.

If you follow this recommendation, we can provide you with aggregated analytic data about your tool's usage later on.

## Topics

If you want to, you can mark your repository by specific [GitHub topics](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics). This ensures, you get to the topic listing and your repository vill be much more accessible. 

> Feel free to use multiple topics.

* [kontent-ai](https://github.com/topics/kontent-ai) - General topic used in all repositories related to Kontent.ai.
    * [kontent-ai-integration](https://github.com/topics/kontent-ai-integration) - Repository helps to integrate with other services (see [Integrations info](./Integrations.md) for more).
    * [kontent-ai-tool](https://github.com/topics/kontent-ai-tool) Repository helps with the tooling around Kontent.ai (SDKs, CLIs, Generators, Mini apps using API etc.).
    * [kontent-ai-sample](https://github.com/topics/kontent-ai-tool) Repository is showcasing the usage of Kontent.ai.

---

If you have any additional questions or comments, you can contact us directly at devrel@kontent.ai.
