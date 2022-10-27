Welcome to the Kontent.ai guidelines for JS library developers! Please, follow the guidelines when you develop a repository with a reusable library written in JS/TS (e.g. SDKs, reusable react components).

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