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

- Node.js and npm available on the runner
- `setup-node` with `registry-url` configured prior to this step
- `id-token: write` permission on the job for npm trusted publishing (OIDC provenance)
