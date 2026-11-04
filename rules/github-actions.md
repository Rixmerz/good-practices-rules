---
paths:
  - "**/.github/workflows/*.{yaml,yml}"
  - "**/.github/actions/**/action.{yaml,yml}"
  - "**/action.{yaml,yml}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: GitHub Actions

## Top rules (critical)
- **Never interpolate untrusted input into `run:`**. `${{ github.event.issue.title }}`, `...pull_request.title`, `...head_ref`, `...comment.body` and similar are attacker-controlled. Pass them through `env:` and use `"$TITLE"` in the script.
- **Pin third-party actions to a full commit SHA** with the version in a comment: `uses: some/action@8f4b7f84...  # v4.1.0`. Tags can be moved. GitHub's own `actions/*` may use a major tag if the repo accepts that.
- **Declare least-privilege `permissions:`** at the workflow level (start from `contents: read`) and widen per job only where needed.
- **Don't check out or run PR code in `pull_request_target`** (or `workflow_run`) with secrets or write permissions; it runs untrusted code with the base repo's privileges.

## Secrets
- Use `secrets.*` only in the steps that need them, via `env:`; never `echo` them or pass them on the command line.
- Mask derived secrets with `::add-mask::`.
- Prefer OIDC (`id-token: write` + cloud role) over long-lived cloud keys.
- Don't use `secrets: inherit` in reusable workflows unless every secret is needed.

## Reliability
- Set `timeout-minutes` on every job.
- Use `concurrency:` with `cancel-in-progress: true` for PR workflows.
- Cache dependencies with the setup action's built-in cache (`cache: npm`, `cache: pip`), keyed on the lockfile.
- Install with the lockfile (`npm ci`, `pip install -r requirements.txt --require-hashes`).
- Pin runner images (`ubuntu-24.04`) when builds depend on the OS.
- Use `set -euo pipefail` (or `shell: bash`, which adds `-eo pipefail`) in multi-line `run:` scripts.
- Write outputs to `$GITHUB_OUTPUT`, never the deprecated `::set-output`.

## Security
- Set `persist-credentials: false` on `actions/checkout` unless later steps push.
- Don't use self-hosted runners for public repositories.
- Don't download and execute scripts (`curl ... | bash`) without verifying a checksum.
- Review `dependabot`/`renovate` updates for actions; enable it for the `github-actions` ecosystem.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Vulnerabilities
- GitHub Actions should not be vulnerable to script injections **(critical)**
- Workflows should not rely on unverified GitHub Actions
- Write permissions should be defined at the job level
- Secrets should not be exposed to untrusted code via "pull_request_target"
- Artifacts should not contain secrets
