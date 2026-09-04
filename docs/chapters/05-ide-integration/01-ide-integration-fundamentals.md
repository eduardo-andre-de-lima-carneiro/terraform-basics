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

## What each feature actually needs

The five features above do not all need the same setup. Knowing which ones work with just the binary on `PATH`, versus which need an initialized working directory, saves time when something "doesn't work" in a fresh clone:

| Feature | Requires |
| --- | --- |
| Syntax highlighting | The editor's grammar/extension only — no binary needed |
| Formatting (`terraform fmt`) | The `terraform` binary on `PATH` |
| Diagnostics from `terraform validate` | The `terraform` binary on `PATH`; more accurate once provider schemas are available |
| Completion for resource/data source arguments | `terraform-ls` running, plus `terraform init` in that directory so provider schemas are downloaded |
| Hover documentation and "go to definition" | `terraform-ls` running, plus `terraform init` for cross-module and provider-aware results |

In practice: cloning a repo and opening a `.tf` file gets you highlighting and formatting immediately, but completion and hover stay generic (or empty) until you run `terraform init` in that directory.

## Practice

Open a `.tf` file in your editor and confirm three things work: completion inside a resource block, an inline error when you delete a required argument, and format-on-save. If any fail, the next sections show how to enable them.

## References

- [Terraform CLI documentation](https://developer.hashicorp.com/terraform/cli)
- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [Language Server Protocol specification](https://microsoft.github.io/language-server-protocol/)
