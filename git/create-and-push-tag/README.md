# Create and Push Tag

Creates and pushes a git tag to the remote.

## Inputs

| Name | Description | Required |
| --- | --- | --- |
| `tag` | The tag name to create | Yes |

## Usage

```yaml
- uses: ./git/create-and-push-tag
  with:
    tag: '@scope/my-package@1.0.0'
```

## Permissions

Requires `contents: write` permission on `GITHUB_TOKEN` to push tags.
