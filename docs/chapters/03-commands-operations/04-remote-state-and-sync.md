# 3.4 Remote State and Synchronization

When more than one person or pipeline runs Terraform, state must live in a shared backend and be protected against simultaneous writes.

## Configure and migrate

```bash
terraform init -migrate-state
```

After adding a `backend` block, this command moves the existing local state into the remote backend and confirms before overwriting anything. Re-running `terraform init` after changing an *existing* backend's configuration (not adding one) requires either `-migrate-state` or `-reconfigure` — Terraform will not silently pick one for you:

| Flag | What it does |
| --- | --- |
| `-migrate-state` | Copies the current state into the new backend configuration |
| `-reconfigure` | Switches to the new backend configuration without migrating state (start clean; the old state is left where it was) |
| `-force-copy` | Same as `-migrate-state` but answers every migration prompt "yes" automatically — for scripted/non-interactive init |

## Locking and refresh

- Supported backends take a lock during `plan` and `apply` so two runs cannot corrupt state; not every backend supports locking, so check the specific backend's documentation before relying on it for a team.
- `terraform plan` refreshes state from the real platform by default, revealing drift.
- Never edit the state file by hand; use `terraform state` subcommands.

**`terraform force-unlock <lock-id>`** removes a stuck lock without touching any infrastructure. Scope: it affects only the lock on this configuration's state, not the resources themselves. Use it only after confirming the process that took the lock has actually stopped (a crashed CI job, a killed local run) — Terraform prints the lock ID and who holds it when a lock is contended. Recovery path: if you force-unlock while someone else's run is genuinely still in progress, both runs can now write state at once and corrupt it; if that happens, restore the last good state from your backend's own versioning (e.g. S3 bucket versioning) or from `terraform state pull` output saved beforehand.

## Read another configuration's outputs

Two Terraform configurations often need to share information — a networking stack's subnet ID, say, consumed by an application stack — without merging them into one root module. The `terraform_remote_state` data source reads another configuration's state directly from its backend:

```hcl
data "terraform_remote_state" "network" {
  backend = "local"
  config = {
    path = "../network/terraform.tfstate"
  }
}

resource "local_file" "app_config" {
  filename = "app.conf"
  content  = "subnet=${data.terraform_remote_state.network.outputs.subnet_id}"
}
```

Swap `backend = "local"` and its `path` for the matching backend and `config` map of whichever backend the other configuration actually uses (for example `backend = "s3"` with `bucket`/`key`/`region`). Two things to keep in mind:

- Only the other configuration's root-module `output` values are readable — nothing nested inside its child modules unless that module's outputs are re-exposed at the root.
- Anyone who can read these outputs can reach the full state snapshot the same way, so this only narrows *what you reference*, not who can see the underlying state — keep secrets out of outputs the way you keep them out of everything else in state.

## Practice

Describe, in two sentences, what could go wrong if two engineers ran `terraform apply` at the same time against a local state file. Then explain how a locking remote backend prevents it.

Set up two local directories, `network` and `app`, each with their own `local`-provider configuration; give `network` an output value, then read it from `app` with `terraform_remote_state` and confirm `terraform apply` in `app` picks up the value.

## References

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [State locking](https://developer.hashicorp.com/terraform/language/state/locking)
- [Command: init (backend initialization)](https://developer.hashicorp.com/terraform/cli/commands/init#backend-initialization)
- [Managing state in automation](https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform)
- [Command: force-unlock](https://developer.hashicorp.com/terraform/cli/commands/force-unlock)
- [`terraform_remote_state` data source](https://developer.hashicorp.com/terraform/language/state/remote-state-data)
