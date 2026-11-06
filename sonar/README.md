# SonarQube scan

Runs a SonarQube scan with [SonarSource/sonarqube-scan-action](https://github.com/SonarSource/sonarqube-scan-action). The action attempts to download an LCOV coverage artifact before the scan; if the download fails, the scan still runs without the downloaded coverage report.

## Inputs

| Input | Required | Description | Default |
|---|---|---|---|
| `sonar-token` | Yes | SonarQube authentication token | — |
| `artifact-name` | No | Name of the uploaded artifact that contains LCOV coverage data | `coverage` |
| `artifact-path` | No | Relative destination for the downloaded artifact and LCOV report lookup | `coverage/` |
| `sonar-host-url` | No | SonarQube server URL | `https://sonarcloud.io` |
| `working-directory` | No | Directory prefix for artifact download and LCOV report lookup; does not change the Sonar scan root | `.` |

## Usage

Upload the coverage artifact in an earlier job in the same workflow run. The artifact must contain `lcov.info` at its root.

```yaml
jobs:
  sonar:
    runs-on: ubuntu-latest
    permissions:
      contents: read
    steps:
      - uses: actions/checkout@v7
        with:
          fetch-depth: 0
      - uses: aspithost/actions/sonar@v3
        with:
          sonar-token: ${{ secrets.SONAR_TOKEN }}
```

For a self-hosted SonarQube server or non-default artifact location:

```yaml
jobs:
  sonar:
    runs-on: ubuntu-latest
    permissions:
      contents: read
    steps:
      - uses: actions/checkout@v7
        with:
          fetch-depth: 0
      - uses: aspithost/actions/sonar@v3
        with:
          artifact-name: my-app-coverage
          artifact-path: reports/
          sonar-token: ${{ secrets.SONAR_TOKEN }}
          sonar-host-url: https://sonar.mycompany.com
          working-directory: packages/my-app
```

With those paths, the action expects the downloaded report at `packages/my-app/reports/lcov.info`.


## Notes

- Upload the artifact before this action runs, and use the same workflow run for both jobs.
- The coverage artifact download uses `continue-on-error`, so a missing or unavailable artifact does not prevent the SonarQube scan from running. Coverage will not be available to the scan in that case.
- Place `lcov.info` at the root of the uploaded artifact. The action passes its downloaded path to SonarQube as `sonar.javascript.lcov.reportPaths`.
- Add `sonar-project.properties` at the repository root for project settings such as the project key and source paths.
- Check out the repository before running this action. Set `fetch-depth: 0` to give SonarQube full git history for blame and new-code detection.
