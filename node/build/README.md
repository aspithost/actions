# Build Node

This action installs dependencies, then builds, lints, tests, and audits a Node.js project. It can also upload the build output and a coverage report as artifacts.

## Steps

1. Set up Node.js with an npm cache keyed by `package-lock.json`
2. Install dependencies with `npm ci`
3. Run the build command when `build-command` contains a command
4. Run the lint command when `lint-command` contains a command
5. Run the test command when `test-command` contains a command
6. Audit dependencies when `audit-command` contains a command
7. Upload the build artifact when `upload-build` equals `true`
8. Upload the coverage report when `upload-coverage-report` equals `true`

## Inputs

| Name | Description | Required | Default |
| --- | --- | --- | --- |
| `node-version` | Node.js version to use | No | `26` |
| `working-directory` | The working directory to run commands in (for monorepos) | No | `.` |
| `build-command` | Build command; enter one command per line (skipped if empty) | No | `npm run build` |
| `lint-command` | Lint command; enter one command per line (skipped if empty) | No | `npm run lint` |
| `test-command` | Test command; enter one command per line (skipped if empty) | No | `''` |
| `audit-command` | Dependency audit command; enter one command per line (skipped if empty) | No | `npm audit --audit-level=critical` |
| `upload-build` | Upload the build output as an artifact (`'true'` or `'false'`) | No | `'false'` |
| `build-artifact-name` | Name for the uploaded build artifact | No | `dist` |
| `build-artifact-path` | Artifact path, relative to the repository root rather than `working-directory` | No | `dist/` |
| `build-artifact-retention-days` | Retention period for the build artifact, in days | No | `'1'` |
| `upload-coverage-report` | Upload a coverage report (`'true'` or `'false'`) | No | `'false'` |
| `coverage-report-artifact-name` | Name for the uploaded coverage artifact | No | `coverage` |
| `coverage-report-artifact-path` | Artifact path, relative to the repository root rather than `working-directory` | No | `coverage/` |
| `coverage-report-artifact-retention-days` | Retention period for the coverage artifact, in days | No | `'1'` |

## Usage

```yaml
- uses: actions/checkout@v6

- uses: aspithost/actions/node/build@v3
```

With tests, a build artifact and a coverage report:

```yaml
- uses: aspithost/actions/node/build@v3
  with:
    test-command: npm run test:coverage
    upload-build: 'true'
    upload-coverage-report: 'true'
```

In a monorepo, with multiple commands:

```yaml
- uses: aspithost/actions/node/build@v3
  with:
    working-directory: packages/my-app
    build-command: |
      npm run codegen
      npm run build
    test-command: npm test
    build-artifact-path: packages/my-app/dist/
```

## Requirements

- Check out the repository before running this action (for example, with `actions/checkout`).
- Keep a `package-lock.json` in `working-directory`. The action runs `npm ci` and keys its npm cache on that file.

## Notes

- Artifact paths start at the repository root. Include `working-directory` in paths for monorepos.
- Artifact uploads fail when the specified path contains no files (`if-no-files-found: error`).
- Set a command input to an empty string to skip its step, for example, `lint-command: ''`.
- To use the coverage report with [vitest-coverage-report](../../vitest-coverage-report/README.md), set `coverage-report-artifact-name` to match that action's `artifact-name` input (default: `vitest-coverage-report`).