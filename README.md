# ci-workflows

Reusable GitHub Actions workflows shared across metacognitive-therapy repos.

## pick-runner

Returns a runner label: GitHub-hosted (`ubuntu-24.04`) while the org has included Actions minutes left this month, `blacksmith-2vcpu-ubuntu-2404` once they are used up or usage can't be read.

```yaml
jobs:
  runner:
    uses: metacognitive-therapy/ci-workflows/.github/workflows/pick-runner.yml@v1
    secrets: inherit
  build:
    needs: runner
    runs-on: ${{ needs.runner.outputs.label }}
```

Setup in the calling repo:

- `BILLING_TOKEN` secret: fine-grained PAT, resource owner `metacognitive-therapy`, org permission **Administration: read**. Without it the picker always chooses Blacksmith.
- `ACTIONS_MINUTES_LIMIT` variable (optional, default `1950`): monthly Linux minutes after which jobs move to Blacksmith.

Inputs `github-runner` and `blacksmith-runner` override the two labels.
