# open-source-recipes

<!-- vim-markdown-toc GFM -->

- [Develop Build Pipelines using GitHub Actions](#develop-build-pipelines-using-github-actions)
- [Allow people to test pipelines locally](#allow-people-to-test-pipelines-locally)
    - [🚀 Build Pipelines](#-build-pipelines)
- [Release Please](#release-please)
- [Releasing](#releasing)
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
- [Testing](#testing)
    - [Jest](#jest)
- [TypeScript](#typescript)
- [Commit Linting](#commit-linting)
    - [Setup Instructions](#setup-instructions)
    - [Readme](#readme)
- [Add Funding](#add-funding)

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


Under Settings > Actions > Workflow Permissions, enable the option "Allow GitHub Actions to create and approve pull requests".

The version of the site and the code is defined in the [`package.json`](./package.json) file.

Releasing in managed via [Release Please](https://github.com/googleapis/release-please) in the [`main.yaml`](./.github/workflows/main.yaml) workflow file.

If you need to manually trigger a release, run:

```bash
git commit --allow-empty -m "chore: release 2.0.0" -m "Release-As: 2.0.0"
```

Add release please documentation to your README with the snippet below:

## Releasing

This project uses [Release Please](https://github.com/googleapis/release-please) to manage releases. As long as you use [Conventional Commit messages](https://www.conventionalcommits.org/en/v1.0.0/), release please will open up a 'release' pull request on the `main` branch when changes are merged. Merging the release pull request will trigger a full release to NPM.

```bash
VERSION="0.1.0" git commit --allow-empty -m "chore: release ${VERSION}" -m "Release-As: ${VERSION}"
```

## Versioning


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
cat << EOF > ./.eslintrc.yml
extends: ["prettier"]
plugins: ["prettier"]
rules: 
  prettier/prettier: ["error"]
EOF

# Configure the 'lint' script.
cp package.json package.json.backup
jq '.scripts.lint="eslint ." | .scripts["lint:fix"]="eslint --fix ."' package.json.backup > package.json

# Add some basic documentation.
echo "| Command | Description |" >> README.md
echo "| ------- | ----------- |" >> README.md
echo "| npm run lint | Lint the code with eslint/prettier |" >> README.md
echo "| npm run lint:fix | Fix the code with eslint/prettier |" >> README.md

```

To use AirBnB:

```bash
npx install-peerdeps --dev eslint-config-airbnb

# Add 'extends', 'plugin', set 'parser'.
yq e '.extends += [ "airbnb" ]' .eslintrc.yml
```

## Testing

### Jest

Install and setup [Jest](https://jestjs.io):

```bash
# Install Jest.
npm install --save-dev jest

# Configure test and test:debug commands.
cp package.json package.json.backup
jq '.scripts.test="jest" | .scripts["test:debug"]="node --inspect-brk node_modules/.bin/jest --runInBand"' package.json.backup > package.json

# Create a basic test.
cat << EOF > ./index.test.js
test('adds 1 + 2 to equal 3', () => {
  expect(1 + 2).toBe(3);
});
EOF

# Setup Jest coverage.
cp package.json package.json.backup
jq '.scripts["test:cov"]="jest --coverage --coverageDirectory=artifacts/coverage"' package.json.backup > package.json

# Add some basic documentation.
echo "| Command | Description |" >> README.md
echo "| ------- | ----------- |" >> README.md
echo "| npm run test | Run tests. |" >> README.md
echo "| npm run test:debug | Debug tests. |" >> README.md
echo "| npm run test:cov | Run tests with coverage reported. |" >> README.md
```


Add test commands:

## TypeScript

Good reference at https://khalilstemmler.com/blogs/typescript/node-starter-project/

```bash
npm i -D typescript ts-node
```

Package.json needs:

```
"build": "tsc"
"start": "ts-node ./src/index.ts"
```



Create the `tsconfig.json` file:

```bash
npx tsc --init \
    --rootDir src \
    --outDir build \
    --esModuleInterop \
    --resolveJsonModule \
    --lib es2021 \
    --module commonjs \
    --allowJs true \
    --noImplicitAny true
```

Details of this configuration:

 - `rootDir`: source at `src`
 - `outDir`: built code at `build`
 - `esModuleInterop`: Enable common.js modules
 - `resolveJsonModule`: allow us to import `json` files
 - `lib`: Adds ambient types to our project, allowing us to rely on features from different Ecmascript versions, testing libraries, use es6 language features but compile down to ES5.
 - `module`: commonjs is the standard Node module system in 2019
 - `allowJs`: allow us to include plain JS
 - `noImplicitAny`: enforce explicit types

To use TypeScript for ESLint:

```bash
# Install typescript parser/plugin
npm install --save-dev @typescript-eslint/parser @typescript-eslint/eslint-plugin

# Add 'extends', 'plugin', set 'parser'.
cp .eslintrc.yml .eslintrc.yml.backup
yq -i e '.extends += [ "plugin:@typescript-eslint/recommended" ]' .eslintrc.yml
yq -i e '.plugins += [ "@typescript-eslint" ]' .eslintrc.yml
yq -i e '.parser = "@typescript-eslint/parser"' .eslintrc.yml
```

To add support for TypeScript to Jest:

```bash
npm i -D jest typescript ts-jest @types/jest
npx ts-jest config:init
```

To add to Husky/lint-staged:

```
jq '.lint-staged."**/*.{js,ts}"="npm run lint:fix"' package.json | jq . | > package.json
```

## Commit Linting

### Setup Instructions

[commitlint](https://github.com/conventional-changelog/commitlint) is used for commit linting.

```bash
npm install --save-dev @commitlint/{config-conventional,cli}
npx husky add .husky/commit-msg  'npx --no -- commitlint --edit ${1}'
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js

# or commitlint.config.cjs if you get Error [ERR_REQUIRE_ESM].
```

### Readme

Commit messages are linted with [commitlint](https://github.com/conventional-changelog/commitlint) and should match the [Conventional Commits Specification](https://www.conventionalcommits.org/en/v1.0.0/).

## Add Funding

```bash
mkdir -p .github/

cat << EOF > ./.github/FUNDING.yml
# Support 'GitHub Sponsors' funding.
github: dwmkerr
EOF
```
