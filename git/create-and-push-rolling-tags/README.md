# git/update-version-tags

Creates and force-pushes major and minor version tags from a semver tag.

Given `v1.2.3`, this action will create/update:
- `v1` (major)
- `v1.2` (minor)

## Inputs

| Input | Description | Required |
|-------|-------------|----------|
| `tag` | The semver tag (e.g. `v1.2.3`) | Yes |

## Usage

```yaml
- uses: aspithost/actions/git/update-version-tags@v1
  with:
    tag: v1.2.3
```

## Required permissions

- `contents: write` — needed to force-push tags
