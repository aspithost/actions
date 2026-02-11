# Release Workflow

Reusable workflow that publishes a package to npm and creates a git tag, only when the local version differs from the published version.

## Jobs

1. **Get local package information** - Reads `name` and `version` from the local `package.json`
2. **Get remote package version** - Fetches the currently published version from npm
3. **Publish to npm** - Publishes the package if versions differ
4. **Create and push tag** - Creates and pushes a `<name>@<version>` git tag after publishing

## Usage

```yaml
name: Release

on:
  push:
    branches: [main]

permissions:
  contents: write

jobs:
  release:
    uses: aspithost/actions/.github/workflows/release.yml@v0
    with:
      package-path: ./my-package
    secrets:
      npm-token: ${{ secrets.NPM_TOKEN }}
```

## Inputs

| Name | Description | Required |
| --- | --- | --- |
| `package-path` | The path to the package directory | Yes |

## Secrets

| Name | Description | Required |
| --- | --- | --- |
| `npm-token` | NPM access token to publish the package | Yes |

## Permissions

The calling workflow must declare `contents: write` to allow git tag pushing.
