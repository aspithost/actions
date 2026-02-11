# Get Remote Package Version

Retrieves the latest published version of a package from the npm registry. Returns an empty string if the package has not been published.

## Inputs

| Name | Description | Required |
| --- | --- | --- |
| `package-name` | The npm package name to look up | Yes |

## Outputs

| Name | Description |
| --- | --- |
| `version` | The remote package version, or empty string if unpublished |

## Usage

```yaml
- uses: ./npm/get-remote-package-version
  id: remote
  with:
    package-name: '@scope/my-package'

- run: echo "${{ steps.remote.outputs.version }}"
```

## Requirements

Requires Node.js and npm to be available on the runner.
