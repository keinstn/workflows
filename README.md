# workflows
Reusable GitHub Actions workflows

## flutter-upgrade

Opens a PR bumping the fvm-pinned Flutter version (`app/.fvmrc` by default) to
the latest stable release, then starts the caller's CI workflow on the PR branch.

```yaml
name: flutter-upgrade
on:
  schedule:
    - cron: "0 0 * * 1" # Mondays 09:00 JST
  workflow_dispatch:
jobs:
  upgrade:
    permissions:
      contents: write
      pull-requests: write
      actions: write
    uses: keinstn/workflows/.github/workflows/flutter-upgrade.yml@main
    with:
      ci-workflow: ci-flutter.yml
```

Inputs: `fvmrc-path` (default `app/.fvmrc`), `ci-workflow` (default empty = skip).

Caller repo requirements:

- Settings → Actions → General → enable "Allow GitHub Actions to create and
  approve pull requests".
- The `ci-workflow` must have a `workflow_dispatch:` trigger. PRs opened with
  `GITHUB_TOKEN` don't trigger `pull_request` workflows, so CI is dispatched on
  the PR branch instead; its checks show on the PR.
