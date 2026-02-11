# Actions

Reusable GitHub Actions workflows and composite actions.

## Workflows

### Release

Publishes an npm package and creates a git tag, only when the local version differs from the published version.

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

#### Inputs

| Name | Description | Required |
| --- | --- | --- |
| `package-path` | The path to the package directory | Yes |

#### Secrets

| Name | Description | Required |
| --- | --- | --- |
| `npm-token` | NPM access token to publish the package | Yes |

#### Permissions

The calling workflow must declare `contents: write` to allow git tag pushing.

## Actions

### npm/get-local-package-information

Reads `name` and `version` from a local `package.json`.

### npm/get-remote-package-version

Fetches the currently published version of a package from the npm registry.

### npm/publish

Publishes an npm package with `--access public`.

### git/create-and-push-tag

Creates and pushes a git tag.
