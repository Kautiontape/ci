# ci

Reusable pull-request checks for Kautiontape repos, so Renovate PRs show
pass/fail before merge. Each app repo calls one from its own
`.github/workflows/ci.yml`:

```yaml
name: ci
on:
  pull_request:
  workflow_dispatch:
permissions:
  contents: read
jobs:
  check:
    uses: Kautiontape/ci/.github/workflows/node.yml@main
    with:
      run: pnpm check && pnpm build
```

| Workflow | Does | Inputs |
|---|---|---|
| `node.yml` | frozen install (pnpm/yarn/npm from the lockfile), then `run` | `run`, `working-directory`, `node-version`, `pnpm-version`, `submodules` |
| `python.yml` | `uv sync --frozen` or pip requirements, then `run` | `run`, `working-directory`, `python-version`, `uv-sync-args`, `submodules` |
| `docker-build.yml` | build the image, never push | `context`, `file`, `submodules` |
| `actionlint.yml` | lint workflow files | none |

Everything runs on `ubuntu-latest`, never on a self-hosted runner.
