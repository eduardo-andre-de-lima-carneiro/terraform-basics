# 4.1 Integration Fundamentals

Running Terraform on a platform adds collaboration and delivery services around a configuration. The commands remain familiar; the platform supplies identity, permissions, plan review, automation, and change visibility.

## The common flow

1. Store the configuration in a version control repository.
2. Configure a remote backend or a platform workspace for state.
3. On a pull request, run `terraform plan` automatically and post the result.
4. Require review of both the code diff and the plan.
5. Run `terraform apply` only after merge, behind an approval or protected environment.
6. Keep provider credentials in the platform's secret manager.

## Choose where the run happens

Runs can execute in a generic CI job or in a dedicated Terraform platform. A generic job is flexible; a dedicated platform adds state storage, locking, run history, and policy checks without extra scripting.

## What to configure

At minimum, agree on the default branch, branch protection, plan-on-PR, apply-on-merge, who can approve an apply, where state lives, and how secrets are injected. These policies are part of the delivery process, not optional decoration.

## References

- [Running Terraform in automation](https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform)
- [Terraform automation tutorials](https://developer.hashicorp.com/terraform/tutorials/automation)
- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [Terraform recommended practices](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices)
