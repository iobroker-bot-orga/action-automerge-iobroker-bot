# action-automerge-iobroker-bot

Automatically merges pull requests created by `ioBroker-Bot`.

## Behavior

This action is based on `iobroker-bot-orga/action-automerge-dependabot` with these differences:

- no Dependabot metadata is fetched
- no dependency/update-type handling is done
- no config file is required
- only PRs created by `ioBroker-Bot` are eligible
- PRs containing `do not automerge` (case-insensitive) in the PR description are never merged
- an optional `pull-request-ref` input can be used to explicitly pass the PR reference

## Usage

```yaml
name: Auto-Merge ioBroker-Bot PRs

on:
  pull_request_target:
    types: [opened, synchronize, reopened, edited]

jobs:
  auto-merge:
    runs-on: ubuntu-latest
    if: github.actor == 'ioBroker-Bot'

    permissions:
      contents: write
      pull-requests: write

    steps:
      - name: Auto-merge ioBroker-Bot PRs
        uses: iobroker-bot-orga/action-automerge-iobroker-bot@main
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          pull-request-ref: ${{ github.event.pull_request.number }}
```

## Inputs

- `github-token` (required): GitHub token with permission to merge pull requests.
- `pull-request-ref` (optional): PR number or `refs/pull/<number>/head|merge`.
- `merge-method` (optional, default `merge`): `merge`, `squash`, or `rebase`.
- `wait-for-checks` (optional, default `true`): wait for checks before merging.
- `max-wait-time` (optional, default `3600`): maximum waiting time in seconds.
- `poll-interval-seconds` (optional, default `45`): polling interval while waiting for checks.
