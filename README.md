# Design Tokens

Open Forms projects follow the [NL Design System](https://github.com/nl-design-system).
We organize the design tokens in JSON files and use them in downstream projects like the
SDK and the Open Forms backend project.

## How it works

Specify the design tokens in JSON files, which are picked up and merged using the
[style-dictionary](https://www.npmjs.com/package/style-dictionary) library. The resulting
packages include various build targets, such as ES6 modules, CSS variables files, SASS
vars... to be consumed in downstream projects.

## Usage

**Using tokens**

If you are only _consuming_ the design tokens, the easiest integration path is adding
the NPM package as dependency to your project:

```bash
npm install --save-dev @open-formulieren/design-tokens
```

Then, import the desired build target artifact and run your usual build chain.

**Developing and using tokens**

If you actively need to add or change design tokens, we recommend installing the package
locally and using npm workspaces or `npm link` for the least-friction experience. For
Open Forms specifically, we include the package as a git-submodule and leverage
npm workspaces with instructions in the downstream projects.

This allows you to create atomic PRs with design token changes, while being able to
develop against the newest changes.
