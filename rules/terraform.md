---
paths:
  - "**/*.tf"
  - "**/*.tfvars"
  - "**/*.hcl"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Terraform

## Top rules
- **Never commit state** (`*.tfstate`, `*.tfstate.backup`, `.terraform/`) or secret `*.tfvars`; use a remote backend with locking and encryption.
- **Never write secrets in `.tf` files**; read them from a secret manager data source or a variable marked `sensitive = true`.
- **Pin versions**: `required_version` for Terraform and `required_providers` with a version constraint; commit `.terraform.lock.hcl`.
- Pin module sources to a version or tag, never a branch.

## Structure
- Give every variable a `type` and `description`, and add `validation` blocks for constrained values.
- Don't hard-code regions, account IDs or AMI IDs; use variables or data sources.
- Use `for_each` over `count` when items have identities, so removing one doesn't recreate the rest.
- Tag every resource that supports tags (owner, environment), ideally via provider `default_tags`.
- Format with `terraform fmt` and check with `terraform validate` and `tflint`.
- Protect stateful resources with `lifecycle { prevent_destroy = true }`.

## Security
- Storage: block public access on S3 buckets/GCS buckets/Azure containers, enable encryption at rest and versioning, and deny non-HTTPS requests.
- Network: never open SSH (22), RDP (3389) or database ports to `0.0.0.0/0` or `::/0`.
- IAM: no `"*"` actions or resources in policies; grant least privilege, and don't attach admin policies to workloads.
- Databases: not publicly accessible, encrypted, with backups and deletion protection.
- Load balancers and APIs: HTTPS only, TLS 1.2+, HTTP redirects to HTTPS.
- Enable logging: CloudTrail/Audit Logs, VPC flow logs, bucket access logs.
- Encrypt queues, topics, disks and snapshots; use customer-managed keys where required.
- Don't disable key rotation on KMS keys.

## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Vulnerabilities
- Allowing public network access to cloud resources is security-sensitive **(critical)**
- Policies granting all privileges are security-sensitive **(critical)**
- Policies authorizing public access to resources are security-sensitive **(critical)**
- Using unencrypted cloud storages is security-sensitive
- Using clear-text protocols is security-sensitive
- Disabling server-side encryption of S3 buckets is security-sensitive
- Disabling versioning of S3 buckets is security-sensitive
- Administration services access should be restricted to specific IP addresses
- Weak SSL/TLS protocols should not be used
- Disabling logging is security-sensitive
- Using unencrypted databases is security-sensitive
- Creating public APIs without authentication is security-sensitive
