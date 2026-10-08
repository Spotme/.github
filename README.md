# Spotme/.github

Shared GitHub configuration for the Spotme org.

**This repo is public.** It must stay public: package repos read the shared
release-drafter config from here with their default workflow token, which
cannot read private repos. Never put secrets, customer names or internal URLs
here.

## pkg-* release automation

| File | Purpose |
|------|---------|
| `.github/workflows/pkg-release-drafter.yml` | Reusable workflow. On every PR merged into a package repo's default branch: derives the version label from the PR title, excludes revert pairs, freezes the draft on `release:new`, runs release-drafter, deletes empty drafts and rebuilds the `## Requires` section. |
| `.github/workflows/pkg-requires-check.yml` | Reusable workflow. Validates `### Requires` in PR descriptions. |
| `.github/pkg-release-drafter.yml` | release-drafter config shared by all package repos. A repo only needs its own `.github/pkg-release-drafter.yml` to override it. |
| `.github/PULL_REQUEST_TEMPLATE.md` | Org-wide default PR template with the `### Requires` block. Used by every repo that has no PR template of its own. |

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
