# open-source-recipes

Useful snippets and patterns that can be added to your open source projects to make them more contributor friendly.

## Allow people to test pipelines locally

You can test the pipelines locally using [`act`](https://github.com/nektos/act):

```bash
# Install act.
brew install act

# Run first-time setup (please make sure docker is running first).
act

# Now dry run the pipelines.
act --dry-run
```
