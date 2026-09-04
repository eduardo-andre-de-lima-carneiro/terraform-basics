# 5.3 JetBrains IDEs

IntelliJ IDEA, GoLand, PyCharm, and the other JetBrains IDEs support Terraform through the **Terraform and HCL** plugin, which can use `terraform-ls` for richer completion.

## After installing

- Enable the plugin in Settings, Plugins, then point it at the `terraform` binary in Settings, Tools, Terraform.
- Completion, structure view, and rename refactoring work across a module.
- Set `terraform fmt` as a File Watcher or enable "Reformat on save" so style stays consistent.
- Run configurations can wrap `terraform plan` and `terraform apply` if you want them in the Run panel.

## Practice

Create a variable, reference it in a resource, then use "Find usages" and "Rename" to confirm the IDE tracks the reference across files.

## References

- [Terraform and HCL plugin (JetBrains Marketplace)](https://plugins.jetbrains.com/plugin/7808-terraform-and-hcl)
- [Terraform support in IntelliJ IDEA (help)](https://www.jetbrains.com/help/idea/terraform.html)
- [terraform-ls](https://github.com/hashicorp/terraform-ls)
