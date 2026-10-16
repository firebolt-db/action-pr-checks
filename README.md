# action-pr-checks

Checks that a pull request title follows the Firebolt PR convention: a
[conventional commit](https://www.conventionalcommits.org/) title whose subject doesn't start with an
uppercase character. It wraps
[amannn/action-semantic-pull-request](https://github.com/amannn/action-semantic-pull-request).

Allowed types: `build`, `chore`, `ci`, `docs`, `feat`, `fix`, `perf`, `refactor`, `revert`, `style`,
`test`. A scope is optional.

A ticket reference such as `(FB-123)` at the end of the title is optional by default, because OSS
contributors have no access to our ticket tracker. Set `require-ticket: 'true'` to require it.

PRs opened by bots (Dependabot, release-please, the CI app) are skipped.

## Usage

Every public repo uses the same workflow, `.github/workflows/pr-title.yml`, and requires its
`check-title` job as a status check:

```yaml
name: PR title

on:
  pull_request:
    types: [opened, edited, reopened, synchronize]

concurrency:
  group: ${{ github.workflow }}-${{ github.event.pull_request.number }}
  cancel-in-progress: true

permissions: {}

jobs:
  check-title:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: read
    steps:
      - uses: firebolt-db/action-pr-checks@<sha> # v2.0.0
```

## Inputs

| Input | Default | Description |
|---|---|---|
| `github-key` | `${{ github.token }}` | Token used to read the pull request; needs `pull-requests: read`. |
| `require-ticket` | `'false'` | Require a ticket reference such as `(FB-123)` at the end of the title. |

## v1 → v2

v1 used `jef/conventional-commits-pr-action` and always required a ticket or `NoTicket` in the
title. v2 uses amannn/action-semantic-pull-request. The ticket is optional
unless `require-ticket` is set, and bot PRs are skipped. The `github-key` input still works.
