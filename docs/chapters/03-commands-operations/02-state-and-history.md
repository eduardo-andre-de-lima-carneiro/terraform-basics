# 3.2 State and Change History

State is how Terraform remembers what it manages. It maps each resource block to a real object and stores that object's last known attributes.

## Inspect state

```bash
terraform state list
terraform show
terraform state show <resource address>
```

`state list` names every tracked resource, `show` prints the full recorded values, and `state show` focuses on one. `state list` accepts a resource address pattern to filter, which matters once a state holds thousands of resources across many modules — for example `terraform state list module.notes` lists only that module's resources.

## Inspect a saved plan the same way

`terraform show` is not limited to state — pointed at a file saved with `plan -out`, it renders that plan in the same human-readable format, and `terraform show -json` produces a machine-readable version of either state or a plan file for external tooling. Because `-json` prints sensitive values in plain text, treat that output with the same care as the state file itself.

## Advanced: reading and writing the raw state file

- `terraform state pull` prints the current state as raw JSON to stdout — useful for scripting or archiving a snapshot, but read-only and safe.
- `terraform state push` uploads a local state file to the configured backend, overwriting what is there. **This is destructive**: it replaces the entire remote state for this configuration, not just one resource, and a mismatch it silently accepts can corrupt a teammate's next run. Terraform refuses the push by default if the destination has a different lineage or a newer serial number; only pass `-force` to override that check if you are certain the destination copy is the one to discard, and keep a copy of the file you are about to overwrite (or the destination's current state, pulled first) so the push can be undone.

Reach for `state pull`/`state push` only when there is no other way to fix the state (for example, hand-repairing corrupted JSON offline); prefer `terraform state list` / `show` / `mv` / `rm` and `import` for everyday changes, since each of those touches one resource at a time and is reviewable.

## Where the history lives

Terraform does not keep a full change log of its own. The reviewable history is your version control history of the `.tf` files, plus the run history in whatever platform applies them. Commit configuration changes in small, described steps so the "why" is recoverable.

## Practice

Apply a small configuration, run `terraform state list`, then change one attribute and run `terraform plan`. Note how the plan explains the difference between the recorded state and the new desired state.

## References

- [Manipulating state](https://developer.hashicorp.com/terraform/cli/state)
- [Command: state list](https://developer.hashicorp.com/terraform/cli/commands/state/list)
- [Command: show](https://developer.hashicorp.com/terraform/cli/commands/show)
- [Inspecting state](https://developer.hashicorp.com/terraform/cli/state/inspect)
- [Command: state pull](https://developer.hashicorp.com/terraform/cli/commands/state/pull)
- [Command: state push](https://developer.hashicorp.com/terraform/cli/commands/state/push)
