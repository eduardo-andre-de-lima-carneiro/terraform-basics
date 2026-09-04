# 3.2 State and Change History

State is how Terraform remembers what it manages. It maps each resource block to a real object and stores that object's last known attributes.

## Inspect state

```bash
terraform state list
terraform show
terraform state show <resource address>
```

`state list` names every tracked resource, `show` prints the full recorded values, and `state show` focuses on one.

## Where the history lives

Terraform does not keep a full change log of its own. The reviewable history is your version control history of the `.tf` files, plus the run history in whatever platform applies them. Commit configuration changes in small, described steps so the "why" is recoverable.

## Practice

Apply a small configuration, run `terraform state list`, then change one attribute and run `terraform plan`. Note how the plan explains the difference between the recorded state and the new desired state.

## References

- [Manipulating state](https://developer.hashicorp.com/terraform/cli/state)
- [Command: state list](https://developer.hashicorp.com/terraform/cli/commands/state/list)
- [Command: show](https://developer.hashicorp.com/terraform/cli/commands/show)
- [Inspecting state](https://developer.hashicorp.com/terraform/cli/state/inspect)
