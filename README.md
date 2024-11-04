# Izmir Teknoloji Style Guide

## Introduction

This repository is the home of Izmir Teknoloji's style guide, which includes configs for
popular linting and styling tools.

The following configs are available, and are designed to be used together.

- [Prettier](#prettier)
- [ESLint](#eslint)
- [TypeScript](#typescript)

## Installation

All of our configs are contained in one package, `@izmirteknoloji/style-guide`. To install:

```sh
pnpm add -D @izmirteknoloji/style-guide
```

## Prettier

> Note: Prettier is a peer-dependency of this package, and should be installed
> at the root of your project.
>
> See: https://prettier.io/docs/en/install.html

To use the shared Prettier config, set the following in `package.json`.

```json
{
  "prettier": "@izmirteknoloji/style-guide/prettier"
}
```

## Examples

### Next.js

For example, to use the Next.js config, set the following in `.eslintrc.js`.

```js
const { resolve } = require('node:path');

const project = resolve(__dirname, 'tsconfig.json');

module.exports = {
  root: true,
  extends: [
    require.resolve('@izmirteknoloji/style-guide/eslint/typescript'),
    require.resolve('@izmirteknoloji/style-guide/eslint/react'),
    require.resolve('@izmirteknoloji/style-guide/eslint/next'),
  ],
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    project,
  },
  settings: {
    'import/resolver': {
      typescript: {
        project,
      },
    },
  },
};
```

Some of our ESLint configs require peer dependencies. We'll note those
alongside the available configs in the [ESLint](#eslint) section.

## ESLint

> Note: ESLint is a peer-dependency of this package, and should be installed
> at the root of your project.
>
> See: https://eslint.org/docs/user-guide/getting-started#installation-and-usage

This ESLint config is designed to be composable.

The following base configs are available. You can use one or both of these
configs, but they should always be first in `extends`:

- `@izmirteknoloji/style-guide/eslint/browser`
- `@izmirteknoloji/style-guide/eslint/node`

Note that you can scope configs, so that configs only target specific files.
For more information, see: [Scoped configuration with `overrides`](#scoped-configuration-with-overrides).

The following additional configs are available:

- `@izmirteknoloji/style-guide/eslint/jest`
- `@izmirteknoloji/style-guide/eslint/jest-react` (includes rules for `@testing-library/react`)
- `@izmirteknoloji/style-guide/eslint/next` (requires `@next/eslint-plugin-next` to be installed at the same version as `next`)
- `@izmirteknoloji/style-guide/eslint/playwright-test`
- `@izmirteknoloji/style-guide/eslint/react`
- `@izmirteknoloji/style-guide/eslint/typescript` (requires `typescript` to be installed and [additional configuration](#configuring-eslint-for-typescript))
- `@izmirteknoloji/style-guide/eslint/vitest`

#### A note on file extensions

By default, all TypeScript rules are scoped to files ending with `.ts` and
`.tsx`.

However, when using overrides, file extensions must be included or ESLint will
only include `.js` files.

```js
module.exports = {
  overrides: [
    { files: [`directory/**/*.[jt]s?(x)`], rules: { 'my-rule': 'off' } },
  ],
};
```

## TypeScript

This style guide provides multiple TypeScript configs. These configs correlate to the LTS Node.js versions, providing the appropriate `lib`, `module`, `target`, and `moduleResolution` settings for each version. The following configs are available:

| Node.js Version | TypeScript Config                               |
| --------------- | ----------------------------------------------- |
| v16             | `@izmirteknoloji/style-guide/typescript/node16` |
| v18             | `@izmirteknoloji/style-guide/typescript/node18` |
| v20             | `@izmirteknoloji/style-guide/typescript/node20` |

To use the shared TypeScript config, set the following in `tsconfig.json`.

```json
{
  "extends": "@izmirteknoloji/style-guide/typescript/node16"
}
```

The base TypeScript config is also available as [`@izmirteknoloji/style-guide/typescript`](./typescript/tsconfig.base.json) which only specifies a set of general rules. You should inherit from this file when setting custom `lib`, `module`, `target`, and `moduleResolution` settings.
