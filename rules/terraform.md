---
paths:
  - "**/*.tf"
  - "**/*.tfvars"
  - "**/*.hcl"
---
<!-- Installed by good-practices-rules. Edit the package, not this copy: the next install replaces it. -->
# Good practices: Terraform

## Top rules
- **Never grant `"*"` actions or resources in IAM policies**, and never grant public access in resource-based policies; scope permissions to what the workload needs.
- **Block all public access on S3 buckets** and never grant ACLs to all users or authenticated users.
- **Keep databases and cloud resources off the public network** (`publicly_accessible = false`, public network access disabled).
- **Require authentication on API Gateway methods** (no `authorization = "NONE"`).
- Use TLS 1.2+ only; never weak SSL/TLS protocols or cipher suites, and don't use clear-text protocols.

## Security
- Storage: enforce HTTPS-only access on S3 buckets, enable versioning, and don't disable storage encryption.
- Network: restrict administration ports (SSH 22, RDP 3389) to specific IP addresses, never `0.0.0.0/0`.
- Encrypt at rest: EBS volumes, RDS, EFS, OpenSearch, SNS topics, SQS queues, SageMaker notebooks.
- Don't disable resource logging, and keep log and backup retention long enough.
- Azure: don't assign high-privilege built-in roles or scope assignments to a whole subscription; keep RBAC on, disable admin accounts and anonymous access, use managed identities, enable Key Vault purge protection.
- GCP: set a rotation period on KMS keys, require TLS on SQL instances, enable DNSSEC, don't grant highly privileged roles, enforce uniform bucket-level access, and don't use project-wide SSH keys.


## More rules to follow
Write code that already complies with each one; the ones marked **critical** break builds or production.

### Vulnerabilities
- Weak SSL/TLS protocols should not be used **(critical)**
- Clear-text protocols should not be used
- S3 buckets should enforce HTTPS-only access
- Amazon S3 buckets should have versioning enabled
- Cloud resources logging should not be disabled
- S3 buckets should not grant access to all users or authenticated users **(critical)**
- AWS resource-based policies should not grant public access **(critical)**
- EBS volumes should be encrypted
- Amazon S3 bucket public access should be fully blocked **(critical)**
- Policies should not grant all privileges **(critical)**
- Amazon RDS resources should be encrypted at rest
- IAM policies should not grant access to all account resources **(critical)**
- OpenSearch domains should have encryption at rest enabled
- AWS IAM policies should limit the scope of permissions given **(critical)**
- SageMaker notebook instances should be encrypted at rest
- Administration services access should be restricted to specific IP addresses
- Amazon SNS topics should be encrypted at rest
- Public network access to cloud resources should be disabled **(critical)**
- SQS queues should be encrypted
- Amazon EFS file systems should be encrypted
- AWS API Gateway should require authentication **(critical)**
- Backup retention period should be sufficient
- High-privilege Microsoft Entra ID built-in roles should not be assigned
- Managed Identities should be used for Azure resources that authenticate to other services
- Azure resource-specific admin accounts should not be enabled
- Azure resources should not allow anonymous access
- High-privilege Azure built-in roles should not be assigned
- Client certificate authentication should be enabled on Azure service-to-service endpoints
- Azure Role-Based Access Control should not be disabled
- Azure custom roles should not grant subscription Owner capabilities
- Azure role assignments should not be scoped to an entire subscription or management group
- Cloud storage encryption should not be disabled
- GCP IAM policies should not grant highly privileged roles
- GCP KMS symmetric keys should have a rotation period
- GCP DNS zones should have DNSSEC enabled
- GCP SQL instances should require TLS
- GCP resources should not be publicly accessible
- Project-wide SSH keys should not be used to access GCP VM instances
- GCP IAM custom roles should not grant excessive sensitive permissions
- GCP App Engine handlers should require TLS
- GCP IAM custom roles should not allow privilege escalation
- Legacy Attribute-Based Access Control should not be enabled for GKE clusters
- Google Cloud load balancers SSL policies should not offer weak cipher suites
- GCP Cloud Storage buckets should have object versioning enabled
- Log retention period should be long enough to support security investigations
- GCP Cloud Audit Logs exemptions should not be configured for project members
- Database instances should not be publicly accessible **(critical)**
- Azure Key Vaults should enable purge protection
- GCP Cloud Storage buckets should enforce uniform bucket-level access

### Code smells
- AWS resource tags should have valid format
