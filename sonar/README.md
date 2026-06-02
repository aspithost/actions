# SonarQube scan

Runs a SonarQube scan using the [SonarSource/sonarqube-scan-action](https://github.com/SonarSource/sonarqube-scan-action).

## Inputs

| Input | Required | Description | Default |
|---|---|---|---|
| `sonar-token` | Yes | SonarQube authentication token | — |
| `sonar-host-url` | No | SonarQube host URL | `https://sonarcloud.io` |

## Usage

**SonarCloud**
```yaml
- uses: aspithost/actions/github/sonarqube-scan.yml@v2
  with:
    sonar-token: ${{ secrets.SONAR_TOKEN }}
  permissions:
    contents: read
```

**Self-hosted SonarQube**
```yaml
- uses: aspithost/actions/github/sonarqube-scan.yml@v2
  with:
    sonar-token: ${{ secrets.SONAR_TOKEN }}
    sonar-host-url: https://sonar.mycompany.com
  permissions:
    contents: read
```

## Permissions

- `contents: read` — to check out the repository with full history (`fetch-depth: 0`)

## Notes

- You need a `sonar-project.properties` file in the root of your repository to configure the scan (project key, sources, etc.)
- The checkout uses `fetch-depth: 0` to give SonarQube access to the full git history, which is required for accurate blame and new code detection.