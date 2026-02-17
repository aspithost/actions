# SonarQube Scan

Runs a SonarQube scan using the [SonarSource/sonarqube-scan-action](https://github.com/SonarSource/sonarqube-scan-action).

## Inputs

| Input | Required | Description |
|---|---|---|
| `sonar-token` | Yes | SonarQube authentication token |
| `sonar-host-url` | No | SonarQube host URL. Omit for SonarCloud |

## Usage

**SonarCloud**
```yaml
- uses: your-org/actions/sonar-cloud@v1
  with:
    sonar-token: ${{ secrets.SONAR_TOKEN }}
```

**Self-hosted SonarQube**
```yaml
- uses: your-org/actions/sonar-cloud@v1
  with:
    sonar-token: ${{ secrets.SONAR_TOKEN }}
    sonar-host-url: https://sonar.mycompany.com
```

## Notes

- You need a `sonar-project.properties` file in the root of your repository to configure the scan (project key, sources, etc.)
