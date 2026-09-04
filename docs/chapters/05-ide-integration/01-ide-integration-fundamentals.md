# 5.1 IDE Integration Fundamentals

Editor integration for Terraform is mostly one component: the Terraform language server, `terraform-ls`, maintained by HashiCorp. Editors talk to it through the Language Server Protocol.

## What the language server provides

- Syntax highlighting and structural validation of HCL.
- Completion for resource types, arguments, and attributes from installed providers.
- Hover documentation and "go to definition" for modules and variables.
- Formatting through `terraform fmt`.
- Diagnostics that surface `terraform validate` errors inline.

## What still runs in a terminal

The language server does not run `plan`, `apply`, or `destroy`. Those stay explicit commands so infrastructure changes are always deliberate. Most editors add a button or task that simply calls the Terraform CLI.

## Practice

Open a `.tf` file in your editor and confirm three things work: completion inside a resource block, an inline error when you delete a required argument, and format-on-save. If any fail, the next sections show how to enable them.

## References

- [Terraform CLI documentation](https://developer.hashicorp.com/terraform/cli)
- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [Language Server Protocol specification](https://microsoft.github.io/language-server-protocol/)
