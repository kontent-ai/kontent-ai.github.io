## GitHub Repositories

* Repository name:
  * projects under the [Kontent.ai Github organization](https://github.com/kontent-ai/) don't need any further mention of the product in their name
  * If a Kontent.ai related repository is managed in an external organization, use pattern `kontent-ai-<project-name>` or at least provide `kontent-ai` somewhere in the repository name (i.e. `sourcebit-source-kontent-ai`)
  * `kontent-ai-<project-name>` or preferably
* `<project-name>` guidelines:
  * Use a broad-to-specific convention to keep similar projects grouped together
    * e.g. delivery-sdk-js + delivery-sdk-net
* Tagging
  * Tag with the `kontent-ai` tag
  * Use any other appropriate tags for the repo. (e.g. gatsby, gatsbyjs, source-plugin, etc.)

## Code

### Namespaces

We stick to [Microsoft's conventions](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/names-of-namespaces) `<Company>.(<Product>|<Technology>)[.<Feature>][.<Subnamespace>]`. Some examples:

  * `Kontent.Ai.*` for Kontent.ai projects under the [Kontent.ai Github organization](https://github.com/kontent-ai/)
  * `<YourCompany>.Kontent.Ai.*` for any externally maintained Kontent.ai projects 
  * `Kontent.Ai.<Technology>*` for non-product related code. Eg. `Kontent.Ai.AspNetCore.Http` if the code is related to `Microsoft.AspNetCore.Http`.

### Packages

* The name of the package should reflect the repo name
* When it makes sense use organization prefixes
  * e.g. @kontent-ai/deliver-sdk-js
* For projects under the [Kontent.ai Github organization](https://github.com/kontent-ai/) ONLY:
  * Use appropriate icons from https://github.com/kontent-ai/.github/tree/master/images