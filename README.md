# Spotme/.github

Shared GitHub configuration for the Spotme org.

## pkg-* release automation

| File | Purpose |
|------|---------|
| `.github/workflows/pkg-release-drafter.yml` | Reusable workflow. On every PR merged into a package repo's default branch: derives the version label from the PR title, excludes revert pairs, freezes the draft on `release:new`, runs release-drafter, deletes empty drafts and rebuilds the `## Requires` section. |
| `.github/workflows/pkg-requires-check.yml` | Reusable workflow. Validates `### Requires` in PR descriptions. |
| `.github/pkg-release-drafter.yml` | Reference release-drafter config. Package repos keep a copy in their own `.github/`, because the workflow token cannot read this private repo. |
| `.github/PULL_REQUEST_TEMPLATE.md` | PR template with the `### Requires` block. It only becomes the org default if this repo is public. |

Package repos call the workflows with a short caller file:

```yaml
on:
  pull_request:
    types: [closed]
jobs:
  draft:
    uses: Spotme/.github/.github/workflows/pkg-release-drafter.yml@main
```

Both workflows run only in repos whose name starts with `pkg-`. The drafter
works on whatever the repo's default branch is (`dev`, `main`, ...).

Source and design notes: `pkg-release-automation` (ENGINEER-MANUAL.md, README.md).
