# Design Tokens

[![NPM package](https://img.shields.io/npm/v/@open-formulieren/design-tokens.svg)](https://www.npmjs.com/package/@open-formulieren/design-tokens)

Open Forms projects follow the [NL Design System](https://github.com/nl-design-system). We organize
the design tokens in JSON files and use them in downstream projects like the SDK and the Open Forms
backend project.

## How it works

Specify the design tokens in JSON files, which are picked up and merged using the
[style-dictionary](https://www.npmjs.com/package/style-dictionary) library. The resulting packages
include various build targets, such as ES6 modules, CSS variables files, SASS vars... to be consumed
in downstream projects.

The draft [Design Token Format](https://design-tokens.github.io/community-group/format/) drives the
structure of these design tokens.

## Usage

**Using tokens**

If you are only _consuming_ the design tokens, the easiest integration path is adding the NPM
package as dependency to your project:

```bash
npm install --save-dev @open-formulieren/design-tokens
```

Then, import the desired build target artifact and run your usual build chain.

**Developing and using tokens**

If you actively need to add or change design tokens, we recommend installing the package locally and
using npm workspaces or `npm link` for the least-friction experience. For Open Forms specifically,
we include the package as a git-submodule and leverage npm workspaces with instructions in the
downstream projects.

This allows you to create atomic PRs with design token changes, while being able to develop against
the newest changes.

Run:

```bash
npm start
```

to start the watcher which will re-build on changes.

## Naming pattern

Because of the way style-dictionary works, you have to pay close attention to the structure of the
tokens. E.g. if you have two tokens definition files like:

```json
{
  "of": {
    "color": {
      "fg": {"value": "#000000"}
    }
  }
}
```

```json
{
  "of": {
    "color": {
      "fg": {
        "muted": {"value": "#000000"}
      }
    }
  }
}
```

Then only `--of-color-fg` will be emitted since the merged object sees a `value` key at the
`of.color.fg` path.

You can usually avoid this by sticking to a structure adhering to:

```
<prefix>.<component>.<modifier>.<UIState>.<CSSProperty>
```

Where `UIState` can be blank or a value like `hover`, `active`...

Alternatively, if the structure is not that important, you can put the tokens on the same level,
e.g.:

```json
{
  "of": {
    "color": {
      "fg-muted": {"value": "#000000"}
    }
  }
}
```

The latter form is harder to keep track off across files though.

## Release flow

We don't let `npm` apply the git tags when releasing a new version, instead follow this process:

```bash
npm version minor
git commit -am ":bookmark: Bump to version <newVersion>"
git tag "<newVersion>"
git push origin main --tags
```

If you have PGP keys set up, you can use them for the git tag operation.

The CI pipeline will then publish the new version to npmjs.
