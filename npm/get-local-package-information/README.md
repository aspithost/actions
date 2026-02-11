# Get Package Information

Reads the `name` and `version` fields from a local `package.json` file.

## Inputs

| Name | Description | Required |
| --- | --- | --- |
| `working-directory` | Path to the package directory | Yes |

## Outputs

| Name | Description |
| --- | --- |
| `name` | The `name` from `package.json` |
| `version` | The `version` from `package.json` |

## Usage

```yaml
- uses: ./npm/get-local-package-information
  id: pkg
  with:
    working-directory: ./my-package

- run: echo "${{ steps.pkg.outputs.name }}@${{ steps.pkg.outputs.version }}"
```

## Requirements

Requires Node.js to be available on the runner.
