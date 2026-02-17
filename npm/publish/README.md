# Publish to npm

Publishes an npm package to the registry with public access. This action only supports [trusted publishing](https://docs.npmjs.com/generating-provenance-statements#publishing-packages-with-provenance-via-github-actions) (OIDC provenance) and does not accept an npm token.

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
```

## Requirements

- You need Node.js and npm on the runner
- Run `setup-node` with `registry-url` before this step

## Permissions

- `id-token: write` — for npm trusted publishing (OIDC provenance)
