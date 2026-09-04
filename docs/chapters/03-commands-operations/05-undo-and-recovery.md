# 3.5 Undoing Changes Safely

There is no single "undo" in Terraform. You choose a recovery action based on what went wrong.

## The main options

- **Revert the configuration** in version control, then `plan` and `apply`. This is the normal, safe undo.
- **`terraform apply -replace=<address>`** destroys and recreates one resource that is broken but still tracked.
- **`terraform state rm <address>`** tells Terraform to forget a resource without deleting it. The real object stays. If you do not re-import it, the resource is now unmanaged and a later `apply` of the same configuration will try to create a duplicate, which may fail on a name conflict. Use it only right before re-importing.
- **`terraform import <address> <id>`** brings an existing object back under management.
- **`terraform destroy`** deletes every resource recorded in the state for this configuration. It is destructive and irreversible; run `terraform plan -destroy` first and never point it at shared infrastructure.

## Practice

In a disposable directory, apply two resources, then use `-replace` on one and confirm from the plan that only that resource is recreated. Then run `terraform plan -destroy` and read the list before deciding whether to proceed.

## References

- [Command: plan (`-replace`)](https://developer.hashicorp.com/terraform/cli/commands/plan#replace-address)
- [Command: state rm](https://developer.hashicorp.com/terraform/cli/commands/state/rm)
- [Import overview](https://developer.hashicorp.com/terraform/cli/import)
- [Command: destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy)
