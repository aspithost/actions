# Create and Push Tag

Creates and pushes an annotated git tag to the remote.

## Inputs

| Name | Description | Required |
| --- | --- | --- |
| `tag` | The tag name to create | Yes |
| `message` | The tag message. Defaults to `Release <tag>`. | No |

## Usage

```yaml
- uses: ./git/create-and-push-tag
  with:
    tag: '@scope/my-package@1.0.0'
    message: 'Release my-package 1.0.0'
```

If `message` is omitted or empty, the tag message defaults to `Release <tag>`.

## Permissions

Your job needs `contents: write` permission on `GITHUB_TOKEN` to push tags.
