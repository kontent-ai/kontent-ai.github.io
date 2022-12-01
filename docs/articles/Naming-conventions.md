## GitHub Repositories

### Repository name:

* projects under the [Kontent.ai Github organization](https://github.com/kontent-ai/) don't need any further mention of the product in their name
* If a Kontent.ai related repository is managed in an external organization, use pattern `kontent-ai-<project-name>` or at least provide `kontent-ai` somewhere in the repository name (i.e. `sourcebit-source-kontent-ai`)

### `<project-name>` guidelines:

* Use a broad-to-specific convention to keep similar projects grouped together (e.g. delivery-sdk-js + delivery-sdk-net)

### Tagging

If you want to, you can mark your repository by specific [GitHub topics](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics). This ensures, you get to the topic listing and your repository vill be much more accessible. 

> Feel free to use multiple topics.
> Use any other appropriate tags for the repo. (e.g. cli, dotnet, plugin, etc.)

* [kontent-ai](https://github.com/topics/kontent-ai) - General topic used in all repositories related to Kontent.ai.
    * [kontent-ai-integration](https://github.com/topics/kontent-ai-integration) - Repository helps to integrate with other services (see [Integrations info](./Integrations.md) for more).
    * [kontent-ai-tool](https://github.com/topics/kontent-ai-tool) Repository helps with the tooling around Kontent.ai (SDKs, CLIs, Generators, Mini apps using API etc.).
    * [kontent-ai-sample](https://github.com/topics/kontent-ai-tool) Repository is showcasing the usage of Kontent.ai.

## Code

### Namespaces

We stick to [Microsoft's conventions](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/names-of-namespaces) `<Company>.(<Product>|<Technology>)[.<Feature>][.<Subnamespace>]`. Some examples:

  * `Kontent.Ai.*` for Kontent.ai projects under the [Kontent.ai Github organization](https://github.com/kontent-ai/)
  * `<YourCompany>.Kontent.Ai.*` for any externally maintained Kontent.ai projects 
  * `Kontent.Ai.<Technology>*` for non-product related code. Eg. `Kontent.Ai.AspNetCore.Http` if the code is related to `Microsoft.AspNetCore.Http`.

### Packages

* Package names should omit mentions of the technology stack, assuming it can be derived from the package manager (for example `@kontent-ai/delivery-sdk-js` package hosted on npm should lose the `-js` suffix)
* When it makes sense, use organization prefixes (e.g. @kontent-ai/deliver-sdk-js)
* For projects under the [Kontent.ai Github organization](https://github.com/kontent-ai/) ONLY: Use appropriate icons from [https://github.com/kontent-ai/.github/tree/main/logos](https://github.com/kontent-ai/.github/tree/main/logos)
