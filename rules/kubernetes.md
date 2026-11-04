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
- **Set resource `requests` and `limits`** for CPU and memory on every container.
- **Run as non-root**: `securityContext: { runAsNonRoot: true, allowPrivilegeEscalation: false, readOnlyRootFilesystem: true, capabilities: { drop: ["ALL"] } }`.
- **Pin image tags** (or digests); never `:latest` or no tag.
- **Never put secrets in a `ConfigMap`** or plain `env.value`; use `Secret` via `secretKeyRef`, or an external secrets operator. Don't commit unencrypted `Secret` manifests (use Sealed Secrets/SOPS).

## Workloads
- Define `livenessProbe` and `readinessProbe` (and `startupProbe` for slow starts); readiness must not depend on downstream services.
- Use `Deployment`/`StatefulSet`, never bare `Pod`s; set `replicas` ≥ 2 and a `PodDisruptionBudget` for services that must stay up.
- Set `imagePullPolicy: IfNotPresent` for pinned tags.
- Put every resource in an explicit `namespace`, never `default`.
- Label consistently with `app.kubernetes.io/name`, `/instance`, `/version`.

## Security
- Never use `privileged: true`, `hostNetwork`, `hostPID`, `hostIPC` or `hostPath` mounts for application workloads.
- Don't mount the service account token unless the pod calls the API (`automountServiceAccountToken: false`).
- RBAC: no `"*"` verbs or resources, and don't bind `cluster-admin` to workloads.
- Add a default-deny `NetworkPolicy` per namespace and allow only the traffic needed.
- Expose services through an Ingress with TLS; avoid `NodePort` and public `LoadBalancer` for internal services.
- Enforce the `restricted` Pod Security Standard on application namespaces.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Vulnerabilities
- Running containers in privileged mode is security-sensitive **(critical)**
- Exposing Docker sockets is security-sensitive **(critical)**
- Allowing process privilege escalations is security-sensitive
- Mounting sensitive file system paths is security-sensitive
- Using host operating system namespaces is security-sensitive
- Memory and CPU limits should be enforced
- Service account permissions should be restricted
- Wildcards should not be used to define RBAC permissions
- Secrets should not be stored in environment variables in clear text
