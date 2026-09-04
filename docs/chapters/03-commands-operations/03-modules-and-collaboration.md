# 3.3 Modules and Collaboration

A module is a folder of `.tf` files meant to be reused. The folder you run commands in is the root module; it can call child modules.

## Call a module

```hcl
module "notes" {
  source  = "./modules/notes"
  content = "shared example"
}
```

Inputs are passed as arguments, results are read from the module's `output` values, and `terraform init` installs any remote module sources.

## Moving a resource into a module

Moving a resource changes its address (for example `local_file.notes` becomes `module.notes.local_file.notes`). Without help, `terraform plan` would show that resource destroyed and recreated. Tell Terraform it is the same object with a `moved` block:

```hcl
moved {
  from = local_file.notes
  to   = module.notes.local_file.notes
}
```

Now `terraform plan` reports no changes. (`terraform state mv` does the same thing as a one-off command instead of committed configuration.)

## Collaborate safely

- Keep modules small and focused on one purpose.
- Pin module and provider versions so teammates get the same result.
- Review plans in pull requests, not just the configuration diff.
- Share state through a remote backend so runs do not collide.

## Practice

Move a resource into a `./modules/notes` folder, expose one input and one output, add the matching `moved` block, and run `terraform plan`. Confirm it reports no changes.

## References

- [Modules overview](https://developer.hashicorp.com/terraform/language/modules)
- [Module blocks](https://developer.hashicorp.com/terraform/language/modules/syntax)
- [Refactoring with `moved` blocks](https://developer.hashicorp.com/terraform/language/modules/develop/refactoring)
- [Command: state mv](https://developer.hashicorp.com/terraform/cli/commands/state/mv)
- [Module creation tutorial](https://developer.hashicorp.com/terraform/tutorials/modules/module-create)
