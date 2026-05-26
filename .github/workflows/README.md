# Build Node Workflow
 
Reusable workflow that builds a Node.js project. Supports npm, pnpm, yarn, and bun. Defaults to npm and npm scripts.
 
## Steps
 
1. Checks out the repository
2. Optionally runs a dependency review
3. Sets up the package manager and Node.js (or Bun)
4. Installs dependencies using a lockfile
5. Optionally runs the build (skipped if `build-command` is empty)
6. Optionally runs linting (skipped if `lint-command` is empty)
7. Optionally runs tests (skipped if `test-command` is empty)
8. Optionally uploads a Vitest coverage report artifact
9. Optionally audits dependencies (skipped if `audit-command` is empty)
10. Optionally runs a SonarQube scan
11. Optionally uploads the build output as an artifact

## Usage
 
```yaml
name: CI
 
on:
  pull_request:
 
jobs:
  build:
    uses: aspithost/actions/.github/workflows/build-node.yml@v1
```
 
With custom options:
 
```yaml
jobs:
  build:
    uses: aspithost/actions/.github/workflows/build-node.yml@v1
    with:
      package-manager: pnpm
      node-version: '22'
      upload-artifact: true
```
 
With tests:
 
```yaml
jobs:
  build:
    uses: aspithost/actions/.github/workflows/build-node.yml@v1
    with:
      test-command: npm run test
```

With Vitest coverage report:

> **Note:** expects your Vitest config to contain `test.coverage.reporter: ['json-summary', 'json']` and `test.coverageReportOnFailure: true`. Use together with the [Vitest Coverage Report workflow](#vitest-coverage-report-workflow) to post the results as a PR comment.

```yaml
jobs:
  build:
    uses: aspithost/actions/.github/workflows/build-node.yml@v1
    with:
      test-command: npx vitest --coverage
      upload-vitest-coverage-report: true

  vitest-coverage-report:
    needs: build
    uses: aspithost/actions/.github/workflows/vitest-coverage-report.yml@v1
    permissions:
      pull-requests: write
```
 
With audit disabled:
 
```yaml
jobs:
  build:
    uses: aspithost/actions/.github/workflows/build-node.yml@v1
    with:
      audit-command: ''
```
 
With SonarQube:
 
```yaml
jobs:
  build:
    uses: aspithost/actions/.github/workflows/build-node.yml@v1
    with:
      run-sonar: true
    secrets:
      sonar-token: ${{ secrets.SONAR_TOKEN }}
```
 
With a self-hosted SonarQube instance:
 
```yaml
jobs:
  build:
    uses: aspithost/actions/.github/workflows/build-node.yml@v1
    with:
      run-sonar: true
      sonar-host-url: https://sonarqube.example.com
    secrets:
      sonar-token: ${{ secrets.SONAR_TOKEN }}
```
 
## Inputs
 
| Name | Description | Default |
| --- | --- | --- |
| `node-version` | Node.js version to use | `24` |
| `run-dependency-review` | Run the dependency review action | `true` |
| `package-manager` | Package manager to use (`npm`, `pnpm`, `yarn`, or `bun`) | `npm` |
| `build-command` | Command to run for building (skipped if empty) | `npm run build` |
| `lint-command` | Command to run for linting (skipped if empty) | `npm run lint` |
| `test-command` | Command to run tests (skipped if empty) | `''` |
| `upload-vitest-coverage-report` | Upload a Vitest coverage report artifact | `false` |
| `audit-command` | Command to audit dependencies (skipped if empty) | `npm audit --audit-level=critical` |
| `upload-artifact` | Upload the build output as an artifact | `false` |
| `artifact-name` | Name of the uploaded artifact | `dist` |
| `artifact-path` | Path to upload as artifact | `dist/` |
| `artifact-retention-days` | Number of days to retain the artifact | `1` |
| `run-sonar` | Run a SonarQube scan | `false` |
| `sonar-host-url` | SonarQube host URL | `https://sonarcloud.io` |
 
## Secrets
 
| Name | Description | Required |
| --- | --- | --- |
| `sonar-token` | SonarQube authentication token | Only when `run-sonar: true` |
 
---

# Vitest Coverage Report Workflow

Reusable workflow that downloads a Vitest coverage artifact and posts the results as a pull request comment. Intended to be used together with the [Build Node workflow](#build-node-workflow) when `upload-vitest-coverage-report` is enabled.

## Steps

1. Downloads the Vitest coverage artifact produced by the build job
2. Posts the coverage results as a PR comment

## Usage

```yaml
name: CI

on:
  pull_request:

jobs:
  build:
    uses: aspithost/actions/.github/workflows/build-node.yml@v1
    with:
      test-command: npx vitest --coverage
      upload-vitest-coverage-report: true

  vitest-coverage-report:
    needs: build
    uses: aspithost/actions/.github/workflows/vitest-coverage-report.yml@v1
    permissions:
      pull-requests: write
```

With a custom artifact name or path:

```yaml
jobs:
  vitest-coverage-report:
    needs: build
    uses: aspithost/actions/.github/workflows/vitest-coverage-report.yml@v1
    permissions:
      pull-requests: write
    with:
      artifact-name: my-coverage-report
      artifact-path: coverage
```

## Inputs

| Name | Description | Default |
| --- | --- | --- |
| `package-path` | Path to the package directory | `.` |
| `artifact-name` | Name of the coverage artifact to download | `vitest-coverage-report` |
| `artifact-path` | Path to download the artifact to | `coverage` |

## Permissions

Your calling workflow must declare `pull-requests: write`.

---
 
# Release NPM Workflow
 
Reusable workflow that publishes a package to npm and creates a git tag, only when the local version differs from the published version. Uses npm trusted publishing (id-token) instead of an explicit npm token.
 
## Steps
 
1. Reads `name` and `version` from the local `package.json`
2. Fetches the currently published version from npm
3. Optionally downloads a build artifact
4. Publishes the package if versions differ
5. Creates and pushes a `<name>@<version>` git tag

## Usage
 
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
    uses: aspithost/actions/.github/workflows/release-npm.yml@v1
    with:
      package-path: ./my-package
```
 
With a build artifact:
 
```yaml
jobs:
  release:
    uses: aspithost/actions/.github/workflows/release-npm.yml@v1
    with:
      package-path: ./my-package
      artifact-name: dist
```
 
## Inputs
 
| Name | Description | Required | Default |
| --- | --- | --- | --- |
| `package-path` | The path to the package directory | Yes | |
| `node-version` | Node.js version to use | No | `24` |
| `artifact-name` | Name of the build artifact to download | No | `''` |
| `artifact-path` | Path to download the artifact to | No | `dist` |
 
## Permissions
 
Your calling workflow must declare `contents: write` and `id-token: write`.