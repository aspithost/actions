# Actions

Reusable GitHub Actions workflows and composite actions.

### git/create-and-push-tag

Creates and pushes a git tag.

### git/create-and-push-rolling-tags

Creates and force-pushes major and minor version tags from a semver tag (e.g. `v1.2.3` updates `v1` and `v1.2`).

### github/dependency-review

Runs a dependency review on pull requests

### node/get-local-package-information

Reads `name` and `version` from a local `package.json`.

### npm/get-remote-package-version

Fetches the currently published version of a package from the npm registry.

### npm/publish

Publishes an npm package with `--access public` and `--provenance` via trusted publishing.

### sonar

Runs a SonarQube scan. Supports both SonarCloud and self-hosted SonarQube.

### vitest-coverage-report

Downloads a Vitest coverage artifact and uploads it as a pull request comment using [davelosert/vitest-coverage-report-action](https://github.com/davelosert/vitest-coverage-report-action).
