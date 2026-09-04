# 3.5 Undoing Changes Safely

There is no single "undo" in Terraform. You choose a recovery action based on what went wrong.

## The main options

- **Revert the configuration** in version control, then `plan` and `apply`. This is the normal, safe undo.
- **`terraform apply -replace=<address>`** destroys and recreates one resource that is broken but still tracked.
- **`terraform state rm <address>`** tells Terraform to forget a resource without deleting it. The real object stays. If you do not re-import it, the resource is now unmanaged and a later `apply` of the same configuration will try to create a duplicate, which may fail on a name conflict. Use it only right before re-importing.
- **`terraform import <address> <id>`** brings an existing object back under management.
- **`terraform destroy`** deletes every resource recorded in the state for this configuration. It is destructive and irreversible; run `terraform plan -destroy` first and never point it at shared infrastructure.

You may see older material mention **`terraform taint`**: it is deprecated. Use `apply -replace` instead — unlike `taint`, `-replace` shows the replacement in a plan you approve, rather than silently marking a resource for replacement on whatever `apply` runs next (possibly someone else's).

## Which command for which situation

| Situation | Command | Scope | Undo path |
| --- | --- | --- | --- |
| Configuration change was wrong | Revert the `.tf` change in version control, then `plan`/`apply` | Whatever the reverted diff touches | Re-apply the previous configuration again |
| One resource is broken but Terraform still tracks it correctly | `terraform apply -replace=<address>` | One resource instance | Re-run `-replace` again, or accept the new object |
| Terraform should stop managing a resource, without deleting it | `terraform state rm <address>` (or a `removed` block, see below) | One resource, removed from state only | `terraform import` it back under the same or a new address |
| An object exists in the platform but not in state | `terraform import <address> <id>` | One resource, added to state only | `terraform state rm` it again |
| Every resource in this configuration should be deleted | `terraform destroy` | Everything in this state | None — objects are actually deleted; restore from backup/snapshot if the platform offers one |

## Declarative alternatives to the state commands

Two block types let you express `state rm` and `import` as reviewable configuration instead of one-off commands — useful when the change should go through the same pull-request review as everything else:

```hcl
# Stop managing a resource without destroying the real object,
# and see the removal in `terraform plan` before it happens.
removed {
  from = local_file.notes

  lifecycle {
    destroy = false
  }
}
```

```hcl
# Bring an existing object under management, with the plan showing
# exactly what will be imported before you approve it.
import {
  to = local_file.notes
  id = "notes.txt"
}

resource "local_file" "notes" {
  filename = "notes.txt"
  content  = "shared example"
}
```

Both are safer than their command-line equivalents for the same reason `-replace` is safer than `taint`: the effect shows up in a plan you review, instead of taking effect silently on the next `apply`. `terraform plan -generate-config-out=generated.tf` can even write the starting `resource` block for you from an `import` block, which is handy when you do not already know the object's exact attributes.

## Practice

In a disposable directory, apply two resources, then use `-replace` on one and confirm from the plan that only that resource is recreated. Then run `terraform plan -destroy` and read the list before deciding whether to proceed.

Add a `removed` block for one of the two resources and run `terraform plan`; confirm the plan shows it leaving state with `destroy = false` (the real object is untouched). Then write an `import` block for the same resource, targeting the file that is still on disk, and confirm `terraform plan` shows it coming back under management with no changes.

## References

- [Command: plan (`-replace`)](https://developer.hashicorp.com/terraform/cli/commands/plan#replace-address)
- [Command: state rm](https://developer.hashicorp.com/terraform/cli/commands/state/rm)
- [Import overview](https://developer.hashicorp.com/terraform/cli/import)
- [Command: destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy)
- [Command: taint (deprecated)](https://developer.hashicorp.com/terraform/cli/commands/taint)
- [Removing resources: the `removed` block](https://developer.hashicorp.com/terraform/language/state/remove)
- [Generating configuration with `import` blocks](https://developer.hashicorp.com/terraform/language/import/generating-configuration)
