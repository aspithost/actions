# Vitest coverage report

Downloads a Vitest coverage artifact and uploads it as a pull request comment using [davelosert/vitest-coverage-report-action](https://github.com/davelosert/vitest-coverage-report-action).

## Inputs

| Input | Required | Description | Default |
|---|---|---|---|
| `working-directory` | No | Path to the package directory | `.` |
| `artifact-name` | No | Name of the coverage artifact to download | `vitest-coverage-report` |
| `artifact-path` | No | Path to download the coverage artifact to | `coverage` |

## Usage

```yaml
- uses: aspithost/actions/github/vitest-coverage-report.yml@v2
  if: github.event_name == 'pull_request'
  permissions:
    contents: read
    pull-requests: write
```

With a custom artifact name or working directory:

```yaml
- uses: aspithost/actions/github/vitest-coverage-report.yml@v2
  if: github.event_name == 'pull_request'
  with:
    working-directory: packages/my-app
    artifact-name: my-app-coverage
    artifact-path: coverage
  permissions:
    contents: read
    pull-requests: write
```

## Permissions

- `contents: read` — to download the artifact
- `pull-requests: write` — to comment the coverage report on the pull request

## Notes

- The coverage artifact must be uploaded in a prior job (e.g. using `actions/upload-artifact`) before this action runs.
- This action is intended to run on `pull_request` events only.
- Your Vitest config must have `coverage.reporter` set to `json-summary` (and optionally `json`) for the report to be generated correctly.
