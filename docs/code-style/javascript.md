---
layout: default
title: JavaScript / TypeScript
parent: Code style
---

# JavaScript / TypeScript code style

Conventions for Kontent.ai JavaScript/TypeScript projects - both **apps** (sample apps, integration templates) and **libraries** (SDKs, reusable components).

## Formatting & linting

We use [Biome](https://biomejs.dev/) for both formatting and linting, configured through shared presets so every project stays consistent:

- **[kontent-ai/biome-config](https://github.com/kontent-ai/biome-config)** - the shared Biome configuration to extend in your `biome.json`.
- **[kontent-ai/eslint-config](https://github.com/kontent-ai/eslint-config)** - the shared [ESLint](https://eslint.org/) configuration, for projects that still need ESLint for rules Biome doesn't cover yet (it can run alongside Biome).

Follow each repository's README for the current package name, install steps, and exactly what to extend - that's the source of truth, so these guidelines don't go stale when the setup changes.

Wire the tooling into your `package.json` scripts (e.g. `biome check` locally and `biome ci` in the pipeline) and run it as part of your CI workflow.

## Dependencies

How you split `dependencies` and `devDependencies` depends on what you're shipping:

- **Libraries** - split them properly. Anything required at runtime in the consumer's codebase goes in `dependencies`; anything used only to build or test the library goes in `devDependencies`.
- **Apps** - the split has little practical impact, since nothing installs the app as a package. Keep it simple and put everything in `dependencies`.

Don't pin to an exact version - it forces consumers to download duplicate copies of the same package. Restrict to the major version where possible, and keep dependencies reasonably up to date (review them at least once or twice a year).

```jsonc
{
  "dependencies": {
    "some-great-package": "^4.0.0"
  }
}
```

## peerDependencies (libraries)

Put a dependency in `peerDependencies` when the consumer should provide it rather than have your library bundle its own copy - React is the classic example. If you're unsure whether something belongs there, see the [npm `peerDependencies` docs](https://docs.npmjs.com/cli/v10/configuring-npm/package-json#peerdependencies) or ask in `@kontent-ai/developer-relations`.

```jsonc
{
  "peerDependencies": {
    "react": "^18.0.0 || ^19.0.0",
    "react-dom": "^18.0.0 || ^19.0.0"
  }
}
```

## Tree shaking (libraries)

Support [tree shaking](https://webpack.js.org/guides/tree-shaking/) so consumers bundle only what they use. When your package has no import-time side effects, mark it side-effect-free:

```jsonc
{
  "sideEffects": false
}
```

Make sure any module you mark as pure can be safely dropped when it isn't used - it must not run side effects on evaluation.
