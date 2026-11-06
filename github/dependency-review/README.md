# Dependency review

Runs a dependency review on pull requests using the [actions/dependency-review-action](https://github.com/actions/dependency-review-action).

## Inputs

| Input | Required | Description | Values | Default |
|---|---|---|---|---|
| `comment-summary-in-pr` | No | Whether to comment a summary of the dependency review in the pull request | `always`, `on-failure`, `never` | `always` |

## Usage

```yaml
jobs:
  dependency-review:
    if: github.event_name == 'pull_request'
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: actions/checkout@v7
      - uses: aspithost/actions/github/dependency-review@v3
```

## Permissions

- `contents: read` — to check out the repository
- `pull-requests: write` — to comment the review summary on the pull request

## Notes

- Check out the repository before running this action.
- This action only needs to run on `pull_request` events.
- Set `comment-summary-in-pr: never` to disable PR comments if you don't need them.