# Actions

Reusable GitHub Actions workflows and composite actions.

## Usage

Reference any workflow or action with `aspithost/actions/<path>@v3`. The `v3` and `v3.x` tags move with each release, so `@v3` picks up compatible updates.

## Workflows

Call these reusable workflows from a job with `uses:`. Each workflow lives in [.github/workflows](.github/workflows). Its `workflow_call` block lists every input, default and secret.

| Workflow | Description |
| --- | --- |
| [node-ci](#node-ci) | Reviews dependencies, then builds, lints, tests and audits a Node.js package. Optionally scans with Sonar and comments a coverage report. |
| [node-release](#node-release) | Publishes a Node.js package to npm and pushes a `<name>@<version>` git tag when the local version differs from the published version. |
| [node-cicd](#node-cicd) | Runs `node-ci`, then `node-release`. |

### node-ci

Runs on pull requests and pushes. Jobs:

1. `dependency-review` runs on pull requests when `run-dependency-review` equals `true`.
2. `build-node` runs [node/build](node/build/README.md).
3. `sonar-scan` runs [sonar](sonar/README.md) when `run-sonar` equals `true`.
4. `vitest-coverage-report` runs [vitest-coverage-report](vitest-coverage-report/README.md) when `upload-coverage-report` equals `true`.

```yaml
jobs:
  ci:
    uses: aspithost/actions/.github/workflows/node-ci.yml@v3
    permissions:
      contents: read
      pull-requests: write
    with:
      test-command: npm test
```

Required permissions: `contents: read`, `pull-requests: write`.

Pass `sonar-token` as a secret when you set `run-sonar: true`.

### node-release

Compares the local `package.json` version with the version on npm. When they differ, it downloads the build artifact, publishes the package, and creates and pushes a git tag. When they match, it skips those steps. It supports npm only and publishes through trusted publishing.

```yaml
jobs:
  release:
    uses: aspithost/actions/.github/workflows/node-release.yml@v3
    permissions:
      contents: write
      id-token: write
```

Required permissions: `contents: write`, `id-token: write`.

Set `artifact-name` to the artifact that a previous job uploaded, or to an empty string to skip the download.

### node-cicd

Runs `node-ci`, then `node-release` after CI succeeds. It accepts the inputs of both workflows and defaults `upload-build` to `true`, so the release publishes the exact build that CI tested.

```yaml
jobs:
  cicd:
    uses: aspithost/actions/.github/workflows/node-cicd.yml@v3
    permissions:
      contents: write
      id-token: write
      pull-requests: write
    secrets:
      sonar-token: ${{ secrets.SONAR_TOKEN }}
    with:
      test-command: npm test
```

Required permissions: `contents: write`, `id-token: write`, `pull-requests: write`.

In a monorepo, call the workflow once per package and set `working-directory` and `build-artifact-path` for each.

## Actions

Each action has its own README with inputs, usage and requirements.

| Action | Description |
| --- | --- |
| [git/create-and-push-tag](git/create-and-push-tag/README.md) | Creates and pushes an annotated git tag. |
| [git/create-and-push-rolling-tags](git/create-and-push-rolling-tags/README.md) | Creates and force-pushes major and minor version tags from a semver tag (for example, `v1.2.3` updates `v1` and `v1.2`). |
| [github/dependency-review](github/dependency-review/README.md) | Runs a dependency review on pull requests. |
| [node/build](node/build/README.md) | Installs dependencies, then builds, lints, tests and audits a Node.js project. Optionally uploads the build and coverage artifacts. |
| [node/get-local-package-information](node/get-local-package-information/README.md) | Reads `name` and `version` from a local `package.json`. |
| [npm/get-remote-package-version](npm/get-remote-package-version/README.md) | Fetches the currently published version of a package from the npm registry. |
| [npm/publish](npm/publish/README.md) | Publishes an npm package with `--access public` and `--provenance` through trusted publishing. |
| [sonar](sonar/README.md) | Runs a SonarQube scan. Supports both SonarCloud and self-hosted SonarQube. |
| [vitest-coverage-report](vitest-coverage-report/README.md) | Downloads a Vitest coverage artifact and comments it on a pull request using [davelosert/vitest-coverage-report-action](https://github.com/davelosert/vitest-coverage-report-action). |
