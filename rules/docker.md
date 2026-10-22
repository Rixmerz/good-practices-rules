---
paths:
  - "**/Dockerfile"
  - "**/Dockerfile.*"
  - "**/*.dockerfile"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Dockerfile

- **Declare `ARG` in every stage that uses it.** In a multi-stage build, an `ARG` declared before the first `FROM` or in another stage isn't available.
- Double-quote variables: `"$VAR"`.
- Install without running package scripts: `npm ci --ignore-scripts` and `pip install --only-binary :all: -r requirements.txt`. If a package has no wheel, exempt only that package with `--no-binary <package>`.
- Require the lockfile when installing: `npm ci`, never `npm install`, and `pip install --require-hashes -r requirements.txt`.
- Pin a specific image tag, never `latest` or no tag, e.g. `node:22.11-alpine`.
- **Don't run the container as root**: add `USER` with an unprivileged user.
- Don't `COPY . .` the whole context without a `.dockerignore` that excludes `.env`, `.git` and keys.
- Merge consecutive `RUN` instructions with `&&`. Split long `RUN` lines with `\` and sort package lists alphabetically.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Bugs
- Malformed JSON in Exec form leads to unexpected behavior
- A space before the equal sign in key-value pair may lead to unintended behavior
- Dockerfile should only have one ENTRYPOINT and CMD instruction

### Vulnerabilities
- Dockerfile ARG and ENV instructions should not be used to handle secrets **(critical)**
- Avoid executing downloaded artifacts without verification **(critical)**
- Permissions of sensitive mount points should be restrictive
- Administration services should not run in containers
- Builder sandboxes should not be disabled
- File permissions should not be set to world-accessible values
- Containers should not share the host operating system namespaces
- HTTPS should be enforced on HTTP clients following redirects
- Swift dependencies should be locked to verified versions
- Go dependencies should be locked to verified versions
- PHP dependencies should be locked to verified versions
- Ruby dependencies should be locked to verified versions
- Rust dependencies should be locked to verified versions
- Dart dependencies should be locked to verified versions
