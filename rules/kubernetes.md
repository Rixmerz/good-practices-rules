---
paths:
  - "**/k8s/**/*.{yaml,yml}"
  - "**/kubernetes/**/*.{yaml,yml}"
  - "**/manifests/**/*.{yaml,yml}"
  - "**/charts/**/*.{yaml,yml,tpl}"
  - "**/helm/**/*.{yaml,yml,tpl}"
  - "**/kustomization.{yaml,yml}"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Kubernetes and Helm

## Top rules
- **Never hard-code secrets or credentials** in manifests; use a `Secret` via `secretKeyRef` or an external secrets operator.
- **Set memory and storage limits, and CPU, memory and storage requests**, on every container.
- **Lock down the container security context**: no `privileged: true`, `allowPrivilegeEscalation: false`, and drop excessive capabilities (`capabilities: { drop: ["ALL"] }`).
- **Pin image tags** to a specific version (or digest); never `:latest` or no tag.

## Security
- Never share host namespaces (`hostNetwork`, `hostPID`, `hostIPC`) or mount sensitive host paths, and never expose the Docker socket.
- Don't run administration services (such as SSH) in containers.
- Don't mount the service account token unless the pod calls the API (`automountServiceAccountToken: false`).
- RBAC: no `"*"` verbs or resources, and don't grant `pods/exec`/`pods/attach`.
- Use TLS, not clear-text protocols, for service endpoints.
- Don't declare the same environment variable twice in a container, and don't use deprecated API versions.


## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Vulnerabilities
- Credentials should not be hard-coded
- Clear-text protocols should not be used
- Excessive Linux capabilities should not be granted
- Secrets should not be hard-coded **(critical)**
- Containers should not run in privileged mode
- Docker sockets should not be exposed
- Privilege escalation should not be allowed for containers
- Containers should not share the host operating system namespaces
- Sensitive file system paths should not be mounted in containers
- Administration services should not run in containers **(critical)**
- Memory limits should be enforced
- Service account permissions should be restricted
- Wildcards should not be used to define RBAC permissions
- Kubernetes roles should not allow command execution in pods
- Storage limits should be enforced

### Code smells
- Specific version tag for image should be used
- Memory requests should be specified
- CPU requests should be specified
- Ensure whitespace in-between braces in template directives
- Storage requests should be specified
- Environment variables for a container should not be duplicated
