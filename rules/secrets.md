<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: secrets (critical in every language)

Secret scanners read every file in the repo, including `.env`, YAML, JSON, Dockerfiles, tests and docs. Once a secret reaches git, **it must be revoked and rotated**; deleting it from the code isn't enough, because it stays in history.

- **Never write a real secret in a repo file.** Read it from an environment variable or the environment's secret manager, and keep only the variable name in the repo (`os.environ["DB_PASSWORD"]`, `process.env.API_KEY`, `${DB_PASSWORD}`).
- This covers: database passwords (Postgres, MySQL, SQL Server, MongoDB, Redis), connection strings with a user and password, private keys (SSH, PGP, JWK, `.p12`/`.jks`), JWTs, Bearer tokens, `user:pass@` in URLs, password hashes (bcrypt), GCP service account keys, Google/AWS/Azure API keys, GitHub/GitLab/Bitbucket/npm tokens, CI and code-analysis tokens, AI provider keys (OpenAI, Anthropic, Gemini and others), and Slack/Teams/Discord webhooks.
- In `.env.example`, tests and docs, use obviously fake values (`changeme`, `<API_KEY>`, `xxxx`), never one that looks real or was copied from another environment.
- Never commit `.env`, `*.pem`, `*.key`, `*.p12`, `*.jks`, `credentials.json` or `service-account*.json`; make sure they're in `.gitignore`.
- If you find a secret that's already committed, **tell the user** it needs rotating. Don't quietly replace it.

Common findings: Slack webhooks, Basic Auth in URLs, bcrypt hashes, GCP service account keys and Google API keys.
