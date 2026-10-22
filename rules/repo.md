<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: repository hygiene (every file)

- Commit the dependency lockfile of every package manager the repo uses: `package-lock.json`/`pnpm-lock.yaml`/`yarn.lock`, `poetry.lock`/`uv.lock`, `go.sum`, `composer.lock`, `Gemfile.lock`, `gradle.lockfile`, `Cargo.lock`, `pubspec.lock`. Never add them to `.gitignore`.
- Don't write bidirectional control characters, Unicode tag blocks or stacked variation selectors in any file; they hide code from reviewers.
- Never hard-code credentials in JSON or YAML config; see `secrets.md`.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Vulnerabilities
- Source code should not contain bidirectional control characters
- Unicode tag blocks should not be used
- Consecutive Unicode Variation Selectors should not be used
- JavaScript dependency lock file should be committed to source control
- Python dependency lock file should be committed to source control
- PHP dependency lock file should be committed to source control
- Ruby dependency lock file should be committed to source control
- Java and Kotlin dependency lock file should be committed to source control
- Rust dependency lock file should be committed to source control
- Dart dependency lock file should be committed to source control
