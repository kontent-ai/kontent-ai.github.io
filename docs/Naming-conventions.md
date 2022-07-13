## GitHub Repositories

* Repository name:
  * `kontent-ai-<project-name>` for [Kontent.ai](http://kontent.ai/) projects under the [Kontent.ai Github organization](https://github.com/kontent-ai/)
    * If the repository has any requirements for name, at least provide `kontent-ai` somewhere in the repository name (i.e. `sourcebit-source-kontent-ai`)
  * `kontent-ai-<project-name>` for any externally maintained Kontent.ai projects outside the Kontent.ai Github organization
* `<project-name>` guidelines:
  * Use a broad-to-specific convention to keep similar projects grouped together
    * e.g. kontent-ai-delivery-sdk-js + kontent-ai-delivery-sdk-net
  * If a particular project is a plugin/module/etc. for an existing technology with specific naming conventions, use those 
    * e.g. gatsby-source-kontent-ai or gridsome-source-kontent-ai
* Tagging
  * Tag with the `kontent-ai`
  * Use any other appropriate tags for the repo. (e.g. gatsby, gatsbyjs, source-plugin, etc.)

## Code

### Namespaces

We stick to [Microsoft's conventions](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/names-of-namespaces) `<Company>.(<Product>|<Technology>)[.<Feature>][.<Subnamespace>]`. Some examples:

  * `Kontent.ai.*` for Kontent.ai projects under the [Kontent.ai Github organization](https://github.com/kontent-ai/)
  * `<YourCompany>.Kontent.ai.*` for any externally maintained Kontent.ai projects 
  * `Kontent.ai.<Technology>*` for non-product related code. Eg. `Kontent.ai.AspNetCore.Http` if the code is related to `Microsoft.AspNetCore.Http`.

### Packages

* The name of the package should reflect the repo name
* When it makes sense use organization prefixes
  * e.g. @kontent-ai/kontent-ai-deliver-sdk-js
* For projects under the [Kontent.ai Github organization](https://github.com/kontent-ai/) ONLY:
  * Use appropriate icons from https://github.com/kontent-ai/.github/tree/master/images