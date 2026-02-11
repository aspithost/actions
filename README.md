# Actions

Reusable GitHub Actions workflows and composite actions.

## Workflows

### Release

Publishes an npm package and creates a git tag, only when the local version differs from the published version. Uses npm trusted publishing (OIDC) — no access token required.

```yaml
name: Release

on:
  push:
    branches: [main]

permissions:
  contents: write
  id-token: write

jobs:
  release:
    uses: aspithost/actions/.github/workflows/release.yml@v0
    with:
      package-path: ./my-package
```

#### Inputs

| Name | Description | Required |
| --- | --- | --- |
| `package-path` | The path to the package directory | Yes |

#### Permissions

The calling workflow must declare:

- `contents: write` — to allow git tag pushing
- `id-token: write` — to allow npm trusted publishing via OIDC provenance

## Actions

### npm/get-local-package-information

Reads `name` and `version` from a local `package.json`.

### npm/get-remote-package-version

Fetches the currently published version of a package from the npm registry.

### npm/publish

Publishes an npm package with `--access public` and `--provenance` via trusted publishing.

### git/create-and-push-tag

Creates and pushes a git tag.

### git/create-and-push-rolling-tags

Creates and force-pushes major and minor version tags from a semver tag (e.g. `v1.2.3` updates `v1` and `v1.2`).

### git/get-latest-semver-tag

Retrieves the latest semver tag from the repository. Defaults to `v0.0.0` if none exist.

### github/create-release

Creates a GitHub release with auto-generated notes.

### semver/bump

Calculates the next semantic version based on a bump type (major, minor, or patch).
