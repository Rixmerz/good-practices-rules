---
paths:
  - "**/docker-compose*.{yaml,yml}"
  - "**/compose*.{yaml,yml}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Docker Compose

- **Never write secrets in the file**: use `env_file:` pointing to a `.env` that is in `.gitignore`, `${VAR}` substitution, or Compose `secrets:`.
- **Pin image tags**; never `latest` or no tag.
- Don't publish database, cache or admin ports to all interfaces; bind them to localhost (`"127.0.0.1:5432:5432"`) or don't publish them at all and use the internal network.
- Never mount the Docker socket (`/var/run/docker.sock`) into a service unless that service is a trusted Docker tool.
- Don't use `privileged: true`, `network_mode: host` or `pid: host` for application services.
- Run services as non-root (`user:`), and add `read_only: true` and `cap_drop: [ALL]` where the image allows it.
- Add a `healthcheck` and use `depends_on: { db: { condition: service_healthy } }` instead of sleeping in the entrypoint.
- Set `restart: unless-stopped` (or `on-failure`) for long-running services.
- Use named volumes for data; don't bind-mount source code in production files.
- Drop the obsolete top-level `version:` key.
