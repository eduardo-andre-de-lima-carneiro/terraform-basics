# 4.2 HCP Terraform

HCP Terraform (formerly Terraform Cloud) is HashiCorp's managed platform for running Terraform. It combines remote state, remote runs, a private module registry, and policy enforcement.

## Connect and run

Add a `cloud` block to the configuration and log in:

```hcl
terraform {
  cloud {
    organization = "EXAMPLE_ORG"

    workspaces {
      name = "practice"
    }
  }
}
```

```bash
terraform login
terraform init
```

`plan` and `apply` now execute in the workspace, not on your laptop, and state is stored there automatically.

## What a VCS-driven run looks like

Connect a workspace to a repository (instead of, or in addition to, the `cloud` block above) and every pull request gets a **speculative plan**: a plan-only run that cannot be applied, posted back to the pull request as a status check so reviewers see the effect before merge. A run against the tracked branch goes further, moving through stages in order: plan, then cost estimation if the organization enabled it, then any policy check, then apply. Apply normally pauses for a human to select **Confirm & Apply** in the UI unless the workspace is set to auto-apply.

## Useful features

- VCS-driven workspaces run a plan on every pull request and hold applies for approval.
- State is stored, versioned, locked, and encrypted by the platform.
- Sentinel or OPA policies can block a plan that violates a rule.
- Variable sets keep provider credentials out of the repository.
- Cost estimation (off by default; an organization owner enables it) shows the estimated monthly cost delta for AWS, GCP, and Azure resources as a step between plan and apply.
- Instead of `workspaces { name = "..." }`, a configuration can target workspaces by `project` or by `tags` so one block matches many workspaces.

Use a team API token or a workspace-scoped identity with least privilege. Never commit a `terraform login` token.

## References

- [HCP Terraform documentation](https://developer.hashicorp.com/terraform/cloud-docs)
- [The `cloud` block](https://developer.hashicorp.com/terraform/cli/cloud/settings)
- [VCS-driven runs](https://developer.hashicorp.com/terraform/cloud-docs/run/ui)
- [Cost estimation](https://developer.hashicorp.com/terraform/cloud-docs/cost-estimation)
- [Policy enforcement](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement)
- [API tokens](https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/api-tokens)
