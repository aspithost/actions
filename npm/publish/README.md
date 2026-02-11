# Publish to npm

Publishes an npm package to the registry with public access.

## Inputs

| Name | Description | Required |
| --- | --- | --- |
| `package-path` | The path to the package directory to publish | Yes |

## Usage

```yaml
- uses: actions/setup-node@v6
  with:
    registry-url: https://registry.npmjs.org

- uses: ./npm/publish
  with:
    package-path: ./my-package
  env:
    NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

## Requirements

- Node.js and npm available on the runner
- `NODE_AUTH_TOKEN` set in the environment for npm registry authentication
- `setup-node` with `registry-url` configured prior to this step
