# Dependency review

Runs a dependency review on pull requests using the [actions/dependency-review-action](https://github.com/actions/dependency-review-action).

## Inputs

| Input | Required | Description | Values | Default |
|---|---|---|---|---|
| `comment-summary-in-pr` | No | Whether to comment a summary of the dependency review in the pull request | `always`, `on-failure`, `never` | `always` |

## Usage

```yaml
- uses: aspithost/actions/github/dependency-review.yml@v2
  if: github.event_name == 'pull_request'
  permissions:
    contents: read
    pull-requests: write
```

## Permissions

- `contents: read` — to check out the repository
- `pull-requests: write` — to comment the review summary on the pull request

## Notes

- This action only needs to run on `pull_request` events.
- Set `comment-summary-in-pr: never` to disable PR comments if you don't need them.