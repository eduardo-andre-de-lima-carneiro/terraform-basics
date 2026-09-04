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

By default `plan` also refreshes: it reads the current state of every already-tracked object from the real platform before comparing it with your configuration, which is how drift shows up in the plan output. Skip that read with `terraform plan -refresh=false` when you deliberately want to compare against the last recorded state instead (for example, to see only the effect of a configuration edit on a huge state, without paying for a full refresh).

## Automating the workflow

The same loop runs unattended in CI with a few adjustments:

```bash
terraform fmt -check -recursive   # non-zero exit if any file is not formatted; writes nothing
terraform validate
terraform plan -out plan.tfplan -detailed-exitcode
```

`fmt -check -recursive` fails the build instead of silently rewriting files, and scans subdirectories too. `plan -detailed-exitcode` returns `0` when there are no changes, `1` on error, and `2` when the plan contains changes — a pipeline can use exit code `2` to gate a manual approval step before `terraform apply plan.tfplan` runs.

## Why review the plan matters

`apply` with no saved plan file re-runs `plan` and asks for approval, but that plan is generated moments before it is applied — anyone can type `yes` without reading it. Applying a *saved* plan file (`terraform apply plan.tfplan`) removes the temptation: the exact actions were already fixed at `plan` time, so review happens on a stable artifact instead of a live prompt, and the same file can be attached to a pull request or CI run as evidence of what will happen.

## References

- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [Command: validate](https://developer.hashicorp.com/terraform/cli/commands/validate)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Command: apply](https://developer.hashicorp.com/terraform/cli/commands/apply)
