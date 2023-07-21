---
layout: default
title: Guidelines for JS libraries
parent: CI & automation
has_children: true
---

# Code style

## Formatting

We use [dprint](dprint.dev) as a formatter. Use our [shared configuration](https://github.com/kontent-ai/dprint-config) to format your project. Readme in the repository should guide you through the setup process.

## Linting

We use [eslint](https://eslint.org/) as a linter. Extend our [shared configuration](https://github.com/kontent-ai/eslint-config). Readme in the repository should guide you through the setup process. 

# Guidelines for JS apps

Welcome to the Kontent.ai guidelines for JS app developers! Please, follow the guidelines when you develop a repository with an app written in JS/TS (e.g. sample app, integration template...).

## dependencies and devDependencies

For apps there are no significant reasons to split dependencies into dependencies and dev dependencies. There are some not-too-important reasons and different views on how to split them for apps. We suggest to keep all dependencies in the dependencies field.

# Guidelines for JS libraries

Welcome to the Kontent.ai guidelines for JS library developers! Please, follow the guidelines when you develop a repository with a reusable library written in JS/TS (e.g. SDKs, reusable react components).

## dependencies and devDependencies

Separate dependencies and devDependencies based on which dependencies are needed for the code in the library to run in the clients code-base and which is for the development of your library. See [this article for more detailed explanation](https://betterprogramming.pub/package-jsons-dependencies-in-depth-a1f0637a3129).

## Use peerDependencies

Remember to put dependencies into peer dependencies if the dependency should belong there. For example React often belongs to peer dependencies.
If you are not sure, whether some dependency belongs to peer dependencies, please check out [this post](https://nodejs.org/en/blog/npm/peer-dependencies/) or consult someone from code owners or @kontent-ai/developer-relations.

```jsonc
{
  // package.json
  // ...
  peerDependencies: {
    "react": "^17.0.0 || ^18.0.0",
    "react-dom": "^17.0.0 || ^18.0.0"
  },
  // ...
}
```

## Tree shaking

Try to support tree shaking if possible. If you are not sure how to support tree shaking in you package, check out [this post](https://webpack.js.org/guides/tree-shaking/).

```jsonc
{
  // package.json
  // ...
  "sideEffects": false,
  // ...
}
```
Make sure that any module (marked as pure) can be safely removed if not used by the user (doesn't make any side effects upon evaluation).

## Version of dependencies

Don't pin dependencies to a specific version as it makes the users download multiple versions of the same package unnecessarily. Restrict the dependencies only to major version if possible.

```jsonc
{
  // package.json
  // ...
  dependencies: {
    "some-great-package": "^4.0.0",
  },
  // ...
}
```

Keep the library's dependencies up to date. Try to check them at least once or twice a year.
