<!--
sword:
  schema: 1
  name: soft.vscode
  family: ecore-esync
  kind: build-tooling
  generation: shared
  shipped: false
  languages: [TypeScript, JavaScript, CSS]
  build: [npm]
  ng: [ecore-model]
  apps:
    - name: ecorium
      kind: extension
      run: "code --install-extension ecorium-<version>.vsix"
      ports: ["none"]
      launched_by: ["VS Code: the custom editor ecorium.ecoreEditor opens a .ecore file (activation is implicit, activationEvents is empty), or one of the ecorium.* commands is run"]
      artifacts: "ecorium-<version>.vsix: asset of the GitHub release vX.Y.Z of masagroup/soft.vscode, and workflow artifact ecorium-vsix of Build and Test"
      status: internal-tool
      # UNVERIFIED: publication on the Visual Studio Marketplace or Open VSX (no workflow of the repository does it)
-->

# soft.vscode

> Ecorium: a tree and properties editor for EMF Ecore metamodels (`.ecore` files) inside Visual Studio Code, built on the TypeScript Ecore runtime [soft.ts](https://github.com/masagroup/soft.ts) (`@masagroup/ecore`). No Java, no Eclipse.

[![License](https://img.shields.io/github/license/masagroup/soft.vscode.svg)](https://github.com/masagroup/soft.vscode/blob/master/LICENSE)
![Build and Test](https://github.com/masagroup/soft.vscode/actions/workflows/build_and_test.yaml/badge.svg)

| | |
|---|---|
| **Family** | Ecore / esync stack |
| **Languages** | TypeScript (extension host and webview), CSS, Node scripts (`.mjs`) |
| **Build systems** | npm scripts (`package.json`) with esbuild and `vsce` · no Ant, no Docker |

**[Architecture](ARCHITECTURE.md)** · [Contributing](CONTRIBUTING.md) · [known issues and open questions](https://github.com/masagroup/wiki/wiki/known-issues#soft.vscode) · [repository on GitHub](https://github.com/masagroup/soft.vscode)

## Overview

Ecorium edits `.ecore` files in VS Code, in the spirit of the Eclipse sample Ecore editor: a tree of the metamodel on the left, a properties panel for the selected element on the right. The model is loaded, edited and serialized with [soft.ts](https://github.com/masagroup/soft.ts), and written back as standard XMI into the text document, so undo, save and source control are the regular VS Code ones. It needs VS Code 1.140.0 or later and is the default editor for `.ecore` files once installed. Features, keyboard, commands and limitations are in the [user README](docs/USER_README.md).

## Where to start

| You want to... | Read |
|---|---|
| install Ecorium and open a model | [Run](#run) below |
| use the editor (features, keyboard, commands, settings) | [docs/USER_README.md](docs/USER_README.md) |
| build, test and package the extension | [Build](#build) and [Test](#test) below |
| understand how the editor is made (host, webview, model layer) | [ARCHITECTURE.md](ARCHITECTURE.md) |
| contribute | [CONTRIBUTING.md](CONTRIBUTING.md) |
| see what changed in each version | [CHANGELOG.md](CHANGELOG.md) |

## Build

Node.js 22 and npm. `@masagroup/ecore` is published on GitHub Packages: configure npm for the `@masagroup` scope with a `read:packages` token in your user `.npmrc` (the scope mapping is in the `.npmrc` of the repository).

### npm (Windows and Linux)

```
npm install
npm run compile      # development bundles dist/extension.js and dist/webview.js
npm run watch        # rebuild on change
npm run typecheck    # tsc --noEmit on the extension and on the webview (npm run lint is the same)
npm run package      # production build, then ecorium-<version>.vsix at the root
```

## Test

```
npm test                                      # vitest run, with a stub of the vscode module
npx vitest run test/ecoreModel.test.ts        # one test file
npx vitest run -t "<test name>"               # one test
npm run harness                               # serve the real webview in a browser, without VS Code (after npm run compile)
```

`npm run harness -- <file.ecore> --port 3939` opens a given model; edits go to `out/harness-output.ecore`.

## Run

### Ecorium

Install the extension from a `.vsix`, taken from the [releases](https://github.com/masagroup/soft.vscode/releases) or built with `npm run package`, then open any `.ecore` file (try the files of [samples](samples/)).

```
code --install-extension ecorium-<version>.vsix
```

To run it from the sources, open the repository in VS Code and press `F5`: the launch configuration "Run Ecorium" compiles the extension and starts an Extension Development Host on the `samples` folder.

## Documentation and guides

- [Architecture](ARCHITECTURE.md): position in the ecosystem, repository layout, internal structure, how it works (diagrams), dependencies, interfaces.
- [Contributing](CONTRIBUTING.md): development environment, Git and Jira workflow, conventions, CI/CD.
- [User README](docs/USER_README.md): the page of the extension shown to users in VS Code.
- [Changelog](CHANGELOG.md): the changes of each version, also the release notes of the GitHub releases.
- [Known issues and open questions](https://github.com/masagroup/wiki/wiki/known-issues#soft.vscode): the known defects and open points of the repository, kept in the SWORD wiki.
- Related repositories: [soft](https://github.com/masagroup/soft), [soft.ts](https://github.com/masagroup/soft.ts), [soft.gen](https://github.com/masagroup/soft.gen), [soft.models](https://github.com/masagroup/soft.models).
- Transverse pages: [Ecosystem next gen](https://github.com/masagroup/wiki/wiki/ecosystem-nextgen), [Ecore models, code generation and esync](https://github.com/masagroup/wiki/wiki/esync-ecore), [Glossary](https://github.com/masagroup/wiki/wiki/glossary).
- Repository on GitHub: [masagroup/soft.vscode](https://github.com/masagroup/soft.vscode).
- The same documentation in the SWORD wiki: [soft.vscode](https://github.com/masagroup/wiki/wiki/soft.vscode), [architecture](https://github.com/masagroup/wiki/wiki/soft.vscode-architecture) and [contributing](https://github.com/masagroup/wiki/wiki/soft.vscode-contributing).

License: [MPL-2.0](LICENSE), Copyright (c) MASA Group.
