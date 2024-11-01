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
# If you use npm
npm i --save-dev @izmirteknoloji/style-guide

# If you use pnpm
pnpm i --save-dev @izmirteknoloji/style-guide

# If you use Yarn
yarn add --dev @izmirteknoloji/style-guide
```

Some of our ESLint configs require peer dependencies. We'll note those
alongside the available configs in the [ESLint](#eslint) section.

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

> You'll need to use `require.resolve` to provide ESLint with absolute paths,
> due to an issue around ESLint config resolution (see
> [eslint/eslint#9188](https://github.com/eslint/eslint/issues/9188)).

For example, use the shared ESLint config(s) in a Next.js project, set the
following in `.eslintrc.cjs`.

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

### Configuring ESLint for TypeScript

Some of the rules enabled in the TypeScript config require additional type
information, you'll need to provide the path to your `tsconfig.json`.

For more information, see: https://typescript-eslint.io/docs/linting/type-linting

```js
const { resolve } = require('node:path');

const project = resolve(__dirname, 'tsconfig.json');

module.exports = {
  root: true,
  extends: [
    require.resolve('@izmirteknoloji/style-guide/eslint/node'),
    require.resolve('@izmirteknoloji/style-guide/eslint/typescript'),
  ],
  parserOptions: {
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

### Configuring custom components for `jsx-a11y`

It's common practice for React apps to have shared components like `Button`,
which wrap native elements. You can pass this information along to `jsx-a11y`
via the `components` setting.

The below list is not exhaustive.

```js
module.exports = {
  root: true,
  extends: [require.resolve('@izmirteknoloji/style-guide/eslint/react')],
  settings: {
    'jsx-a11y': {
      components: {
        Article: 'article',
        Button: 'button',
        Image: 'img',
        Input: 'input',
        Link: 'a',
        Video: 'video',
      },
    },
  },
};
```

### Scoped configuration with `overrides`

ESLint configs can be scoped to include/exclude specific paths. This ensures
that rules don't "leak" into places where those rules don't apply.

In this example, Jest rules are only being applied to files matching Jest's
default test match pattern.

```js
module.exports = {
  extends: [require.resolve('@izmirteknoloji/style-guide/eslint/node')],
  overrides: [
    {
      files: ['**/__tests__/**/*.[jt]s?(x)', '**/?(*.)+(spec|test).[jt]s?(x)'],
      extends: [require.resolve('@izmirteknoloji/style-guide/eslint/jest')],
    },
  ],
};
```

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
