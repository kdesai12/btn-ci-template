# btn-ci-template

Pipeline source of truth for the btn platform CI (reusable workflow). No
secrets live here — auth is the automatic `GITHUB_TOKEN` plus OIDC keyless
(GHCR + cosign), same as the private source.

## Use

```yaml
jobs:
  pipeline:
    uses: kdesai12/btn-ci-template/.github/workflows/pipeline.yml@<full-SHA> # v1
    with:
      image: ghcr.io/kdesai12/<app>
      runner: hosted
    secrets: inherit
```

- Pin callers by full commit SHA (Dependabot maintains the action pins inside
  `pipeline.yml` from here).
- Each repo provides `scripts/ci-test.sh`; the template owns everything else.
- Caller must grant the permissions the template requests (`contents: read`,
  `packages: write`, `id-token: write`, `pull-requests: read`) — a reusable
  workflow may not request a permission its caller lacks (startup_failure).

## Source

This repo is the pipeline source of truth. It is synced from the private
`kdesai12/btn-bedrock` pipeline (`.github/workflows/pipeline.yml`); that repo
stays private. Callers pin `.github/workflows/pipeline.yml` in this repo by
full commit SHA.
