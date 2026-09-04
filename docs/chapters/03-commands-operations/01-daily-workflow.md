# 3.1 The Daily Workflow

Run `terraform init` once in a new working directory (or after changing providers or the backend), then use a small, observable loop:

```bash
terraform init      # first time in the directory, or after provider/backend changes
terraform fmt
terraform validate
terraform plan -out plan.tfplan
terraform apply plan.tfplan
```

`fmt` normalizes style, `validate` checks the configuration is well formed, and `plan -out` saves the exact change so `apply` runs only what you reviewed. Reading the plan before applying is the habit that prevents surprise changes.

## References

- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [Command: validate](https://developer.hashicorp.com/terraform/cli/commands/validate)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Command: apply](https://developer.hashicorp.com/terraform/cli/commands/apply)
