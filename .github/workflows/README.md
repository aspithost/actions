# Build Workflow

Reusable workflow that builds a TypeScript project. Supports npm, pnpm, yarn, and bun.

## Steps

1. Checks out the repository
2. Sets up the package manager and Node.js (or Bun)
3. Installs dependencies using a lockfile
4. Runs linting
5. Runs the build
6. Optionally runs test commands
7. Optionally uploads the build output as an artifact

## Usage

```yaml
name: CI

on:
  pull_request:

jobs:
  build:
    uses: aspithost/actions/.github/workflows/build-ts.yml@v0
```

With custom options:

```yaml
jobs:
  build:
    uses: aspithost/actions/.github/workflows/build-ts.yml@v0
    with:
      package-manager: pnpm
      node-version: '22'
      upload-artifact: true
```

With tests:

```yaml
jobs:
  build:
    uses: aspithost/actions/.github/workflows/build-ts.yml@v0
    with:
      test-commands: |
        npm run test:unit
        npm run test:e2e
```

## Inputs

| Name | Description | Default |
| --- | --- | --- |
| `node-version` | Node.js version to use | `24` |
| `package-manager` | Package manager to use (`npm`, `pnpm`, `yarn`, or `bun`) | `npm` |
| `lint-command` | Command to run for linting | `npm run lint` |
| `build-command` | Command to run for building | `npm run build` |
| `test-commands` | Commands to run tests, one per line (skipped if empty) | `''` |
| `upload-artifact` | Upload the build output as an artifact | `false` |
| `artifact-name` | Name of the uploaded artifact | `dist` |
| `artifact-path` | Path to upload as artifact | `dist/` |
| `artifact-retention-days` | Number of days to retain the artifact | `1` |

---

# Release Workflow

Reusable workflow that publishes a package to npm and creates a git tag, only when the local version differs from the published version.

## Steps

1. Reads `name` and `version` from the local `package.json`
2. Fetches the currently published version from npm
3. Publishes the package if versions differ
4. Creates and pushes a `<name>@<version>` git tag

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
