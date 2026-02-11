# Create and Push Tag

Creates a git tag in the format `<package-name>@<package-version>` and pushes it to the remote.

## Inputs

| Name | Description | Required |
| --- | --- | --- |
| `package-name` | Name of the package | Yes |
| `package-version` | Version of the package | Yes |

## Outputs

| Name | Description |
| --- | --- |
| `tag` | The created tag name (e.g. `@scope/package@1.0.0`) |

## Usage

```yaml
- uses: ./git/create-and-push-tag
  with:
    package-name: '@scope/my-package'
    package-version: '1.0.0'
```

## Permissions

Requires `contents: write` permission on `GITHUB_TOKEN` to push tags.
