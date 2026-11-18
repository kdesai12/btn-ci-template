# btn-ci-template

Public shared CI pipeline (reusable workflow). No secrets live here — auth is
the automatic `GITHUB_TOKEN` plus OIDC keyless (GHCR + cosign), same as the
private source.

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

Extracted from the private `kdesai12/btn-bedrock` pipeline
(`.github/workflows/pipeline.yml`). That repo stays private; this repo carries
the shared steps only.
