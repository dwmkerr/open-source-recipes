# open-source-recipes

<!-- vim-markdown-toc GFM -->

- [Develop Build Pipelines using GitHub Actions](#develop-build-pipelines-using-github-actions)
- [Allow people to test pipelines locally](#allow-people-to-test-pipelines-locally)
    - [🚀 Build Pipelines](#-build-pipelines)
- [Release Please](#release-please)
- [Versioning](#versioning)
- [Include versioning information](#include-versioning-information)
    - [Versioning](#versioning-1)
- [Include a link to contributing information](#include-a-link-to-contributing-information)
    - [Contributing](#contributing)
- [Include a link to troubleshooting information](#include-a-link-to-troubleshooting-information)
    - [Troubleshooting](#troubleshooting)
- [Include a link to your license](#include-a-link-to-your-license)
    - [License](#license)
- [Linting](#linting)
    - [ESLint and Prettier](#eslint-and-prettier)

<!-- vim-markdown-toc -->

Useful snippets and patterns that can be added to your open source projects to make them more contributor friendly.

⚠️  **Heads Up** ⚠️

I have not decided whether the best way track these is 'snippets' that you copy and paste, or more detailed descriptions that describe _why_ you include key elements. As I build more samples I'll make a call.

## Develop Build Pipelines using GitHub Actions

**Examples**

SharpGL: https://github.com/dwmkerr/sharpgl

- .NET Build Pipelines
- Release Please
- Testing / Code Coverage
- NuGet packaging

## Allow people to test pipelines locally

**Examples**

- SharpGL: https://github.com/dwmkerr/sharpgl#build-pipelines

### 🚀 Build Pipelines

You can test the pipelines locally using [`act`](https://github.com/nektos/act):

```bash
# Install act.
brew install act

# Run first-time setup (please make sure docker is running first).
act

# Validate that the pipelines are formatted / configured correctly.
act --dryrun

# Actually run the pipelines.
act

# Test the 'main' pipeline, providing a token for release please.
act -j main -s GITHUB_TOKEN=<token>

```

## Release Please

## Versioning

The version of the site and the code is defined in the [`package.json`](./package.json) file.

Releasing in managed via [Release Please](https://github.com/googleapis/release-please) in the [`main.yaml`](./.github/workflows/main.yaml) workflow file.

If you need to manually trigger a release, run:

```bash
git commit --allow-empty -m "chore: release 2.0.0" -m "Release-As: 2.0.0"
```


## Include versioning information

If you use semver, include something like the below:

### Versioning

This library follows [Semantic Versioning](http://semver.org/).

## Include a link to contributing information

### Contributing

Contributions welcome! See the [Contributing Guide](.github/CONTRIBUTING.md).

For more information on the design of the library, see [design](./docs/design.md).

## Include a link to troubleshooting information

### Troubleshooting

For common issues and help troubleshooting your configuration, see [Troubleshooting](./docs/troubleshooting.md).

## Include a link to your license

### License

MIT

See [LICENSE](./LICENSE)

## Linting

### ESLint and Prettier


```bash
# Install eslint and the prettier config and plugin.
npm install --save-dev eslint eslint-config-prettier eslint-plugin-prettier

# Configure eslint.
cat << EOF > ./eslintrc.yaml
extends: ["prettier"],
plugins: ["prettier"],
rules: 
  prettier/prettier: ["error"]
EOF

# Configure the 'lint' script.
jq '.scripts.lint="eslint ."' package.json | jq . | > package.json
jq '.scripts.lint:fix="eslint --fix ."' package.json | jq . | > package.json
```

To use AirBnB:

```bash
npx install-peerdeps --dev eslint-config-airbnb

# Add 'extends', 'plugin', set 'parser'.
yq e '.extends += [ "airbnb" ]' .eslintrc.yaml
```

To use TypeScript:

```bash
# Install typescript parser/plugin
npm install --save-dev @typescript-eslint/parser @typescript-eslint/eslint-plugin

# Add 'extends', 'plugin', set 'parser'.
yq e '.extends += [ "plugin:@typescript-eslint/recommended" ]' .eslintrc.yaml
yq e '.plugins += [ "@typescript-eslint" ]' .eslintrc.yaml
yq e '.parser = "@typescript-eslint/parser" .eslintrc.yaml
```

To add to Husky/lint-staged:

```
jq '.lint-staged."**/*.{js,ts}"="npm run lint:fix"' package.json | jq . | > package.json
```
