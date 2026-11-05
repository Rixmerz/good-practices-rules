---
paths:
  - "**/.github/workflows/*.{yaml,yml}"
  - "**/.github/actions/**/action.{yaml,yml}"
  - "**/action.{yaml,yml}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# GitHub Actions good practices, based on zizmor and actionlint

## Top rules (critical)
- **Never interpolate untrusted input into `run:`**. `${{ github.event.issue.title }}`, `...pull_request.title`, `...head_ref`, `...comment.body` and similar are attacker-controlled. Pass them through `env:` and use `"$TITLE"` in the script (zizmor `template-injection`, actionlint script injection check).
- **Pin actions to a full commit SHA** with the version in a comment: `uses: some/action@8f4b7f84...  # v4.1.0`, and keep the comment matching the SHA (zizmor `unpinned-uses`, `ref-version-mismatch`, `impostor-commit`).
- **Declare least-privilege `permissions:`** at the workflow level (start from `contents: read`) and widen per job only where needed (zizmor `excessive-permissions`).
- **Avoid `pull_request_target` and `workflow_run`**; if you must use them, never check out or run PR code with secrets or write permissions (zizmor `dangerous-triggers`).

## Secrets
- Pass only the secrets a step needs, by name; never `toJSON(secrets)` or `secrets: inherit` (zizmor `overprovisioned-secrets`, `secrets-inherit`).
- Use secrets only in jobs that run in a dedicated `environment:` (zizmor `secrets-outside-env`).
- Publish packages with Trusted Publishing (OIDC), not long-lived tokens (zizmor `use-trusted-publishing`).
- Never hardcode credentials in `container:`/`services:` or anywhere in the workflow (zizmor `hardcoded-container-credentials`, actionlint hardcoded credentials check).

## Reliability and security
- Set `persist-credentials: false` on `actions/checkout` unless later steps push (zizmor `artipacked`).
- Don't use self-hosted runners for public repositories (zizmor `self-hosted-runner`).
- Use `concurrency:` with `cancel-in-progress: true` for PR workflows (zizmor `concurrency-limits`).
- Write outputs to `$GITHUB_OUTPUT`, never the deprecated `::set-output` (actionlint deprecated workflow commands check).
- Make multi-line `run:` scripts pass shellcheck (actionlint shellcheck integration).

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### zizmor audits
- template-injection: Detects potential sources of code injection via template expansion **(critical)**
- unpinned-uses: Detects "unpinned" `uses:` clauses **(critical)**
- impostor-commit: Detects commits within a repository's Git network that are not present on any of its branches or tags **(critical)**
- dangerous-triggers: Detects fundamentally dangerous GitHub Actions workflow triggers **(critical)**
- excessive-permissions: Detects excessive permissions in workflows, both at the workflow level and the job level **(critical)**
- artipacked: Detects local filesystem `git` credential storage on GitHub Actions, as well as potential avenues for unintentional persistence of credentials in artifacts **(critical)**
- cache-poisoning: Detects common cache-poisoning primitives in GitHub Actions **(critical)**
- github-env: Detects dangerous writes to the `GITHUB_ENV` and `GITHUB_PATH` environment variables **(critical)**
- github-app: Detects dangerous usages of GitHub App installation tokens **(critical)**
- bot-conditions: Detects potentially spoofable bot conditions **(critical)**
- hardcoded-container-credentials: Detects Docker credentials (usernames and passwords) hardcoded in various places **(critical)**
- insecure-commands: Detects opt-in for executing insecure workflow commands **(critical)**
- known-vulnerable-actions: Detects actions with known, publicly disclosed vulnerabilities **(critical)**
- typosquat-uses: Detects `uses:` clauses and pre-commit `repo:` URLs whose names look like typosquats of popular actions **(critical)**
- unpinned-images: Checks for `container.image` values where the image is not pinned by either a tag (other than `latest`) or SHA256 **(critical)**
- unsound-condition: Detects conditions that are inadvertently always true despite containing an expression **(critical)**
- unsound-contains: Detects conditions that use the `contains()` function in a way that can be bypassed **(critical)**
- dependabot-execution: Detects usages of `insecure-external-code-execution` in Dependabot configuration **(critical)**
- insecure-url-scheme: Detects uses of insecure schemes in URLs **(critical)**
- overprovisioned-secrets: Detects excessive sharing of the `secrets` context
- secrets-inherit: Detects excessive secret inheritance between calling workflows and reusable workflows
- secrets-outside-env: Detects usage of the `secrets` context in jobs without a dedicated environment
- unredacted-secrets: Detects potential secret leakage via redaction failures
- ref-confusion: Detects actions that are pinned to confusable symbolic refs (i.e. branches and tags)
- ref-version-mismatch: Detects `uses:` clauses where a hash-pinned action's version comment does not match the commit
- self-hosted-runner: Detects self-hosted runner usage within workflows
- unpinned-tools: Detects `uses:` steps where the referenced action may use an unpinned external tool
- archived-uses: Detects `uses:` clauses that reference archived repositories
- dependabot-cooldown: Detects missing or insufficient `cooldown` settings in Dependabot configuration
- concurrency-limits: Detects insufficient concurrency limits in workflows
- adhoc-packages: Detects `run:` steps that install or manipulate packages in an ad-hoc manner
- unsound-ternary: Detects expressions that use `&&` and `||` as a pseudo-ternary
- stale-action-refs: Checks for `uses:` clauses which pin an action using a SHA reference that does not point to a tag
- misfeature: Checks for usages of GitHub Actions features that are considered "misfeatures"
- obfuscation: Checks for obfuscated usages of GitHub Actions features
- self-repository: Detects uses of in-repo actions or reusable workflows that don't use GitHub's local reference syntax
- undocumented-permissions: Detects explicit permissions blocks that lack explanatory comments
- anonymous-definition: Detects workflows or action definitions that lack a `name:` field
- superfluous-actions: Detects actions that perform an operation already provided by GitHub's own runner images

### actionlint checks
- Script injection by potentially untrusted inputs
- Hardcoded credentials
- Permissions
- shellcheck integration for `run:`
- pyflakes integration for `run:`
- Syntax check for expression `${{ }}`
- Type checks for expression syntax in `${{ }}`
- Contexts and built-in functions
- Availability of contexts and special functions
- Strict type checks for comparison operators
- Constant conditions at `if:`
- Job dependencies validation
- Job ID and step ID uniqueness
- Reusable workflows
- Webhook events validation
- Workflow dispatch event validation
- CRON syntax and IANA timezone string at `on.schedule`
- Glob filter pattern syntax validation
- Runner labels
- Action format in `uses:`
- Local action inputs validation at `with:`
- Popular action inputs validation at `with:`
- Outdated popular actions detection at `uses:`
- Shell name validation at `shell:`
- Environment variable names
- Check deprecated workflow commands
- Deprecated inputs usage
- Action metadata syntax validation
- Unexpected keys
- Missing required keys and key duplicates
