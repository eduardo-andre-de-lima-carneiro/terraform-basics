# 3.4 Remote State and Synchronization

When more than one person or pipeline runs Terraform, state must live in a shared backend and be protected against simultaneous writes.

## Configure and migrate

```bash
terraform init -migrate-state
```

After adding a `backend` block, this command moves the existing local state into the remote backend and confirms before overwriting anything.

## Locking and refresh

- Supported backends take a lock during `plan` and `apply` so two runs cannot corrupt state.
- `terraform plan` refreshes state from the real platform by default, revealing drift.
- Never edit the state file by hand; use `terraform state` subcommands.

## Practice

Describe, in two sentences, what could go wrong if two engineers ran `terraform apply` at the same time against a local state file. Then explain how a locking remote backend prevents it.

## References

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [State locking](https://developer.hashicorp.com/terraform/language/state/locking)
- [Command: init (backend initialization)](https://developer.hashicorp.com/terraform/cli/commands/init#backend-initialization)
- [Managing state in automation](https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform)
