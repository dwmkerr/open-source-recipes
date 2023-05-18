# open-source-recipes

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
