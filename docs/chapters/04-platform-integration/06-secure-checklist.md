# 4.6 Secure Integration Checklist

Before calling an integration ready, check the following:

- The backend configuration is correct and contains no secret in plain text.
- Authentication uses OIDC, a service connection, or a scoped token, not a long-lived cloud key in the repository.
- State is stored in a locking remote backend or platform workspace, never in version control.
- `terraform plan` runs automatically on pull requests and its output is visible to reviewers.
- Saved plan files and plan artifacts are treated as sensitive: access-restricted, short retention, never public.
- `terraform apply` runs only after merge, behind a required approval or protected environment.
- CI/CD secrets are stored in the platform's secret manager and masked in logs.
- Provider and module versions are pinned, and the `.terraform.lock.hcl` dependency lock file is committed.
- Destroy operations are disabled or require a separate, explicit approval.
- Policy checks (Sentinel, OPA, or a linter) run before apply where appropriate.
- Access is reviewed when a person, token, runner, or service changes role.

Integration is successful when it makes infrastructure change more traceable and repeatable without making credentials or production changes easier to misuse.

## References

- [State: sensitive data](https://developer.hashicorp.com/terraform/language/state/sensitive-data)
- [Terraform recommended practices](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices)
- [Dependency lock file](https://developer.hashicorp.com/terraform/language/files/dependency-lock)
- [Policy enforcement](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement)
- [Injecting secrets into CI/CD (OIDC)](https://developer.hashicorp.com/terraform/tutorials/cloud/dynamic-credentials)
