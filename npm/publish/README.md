# Publish to npm

Publishes an npm package to the npm registry using GitHub Actions OIDC (trusted publishing). This composite action runs `npm publish --access public` in the specified `working-directory` and does not use an npm token.

## Behavior

- Runs `npm publish --access public` inside the provided `working-directory` on the runner.
- Intended for publishing public packages using OIDC provenance. For private packages or token-based publishing, see the Troubleshooting section.

## Inputs

| Name | Description | Required |
| --- | --- | --- |
| `working-directory` | The path (relative to the repository root) to the package directory to publish (where `package.json` lives). Example: `./packages/my-package` | Yes |

## Usage

Example workflow snippet that sets up Node and publishes the package using this action.

```yaml
- uses: actions/setup-node@v6
  with:
    registry-url: https://registry.npmjs.org

- uses: ./npm/publish
  with:
    working-directory: ./my-package
```

## Requirements

- `node` and `npm` must be available on the runner (the hosted GitHub runners already include these).
- You should run `actions/setup-node` before this action to ensure the registry is configured.

## Permissions

- `id-token: write` — for npm trusted publishing (OIDC provenance)
