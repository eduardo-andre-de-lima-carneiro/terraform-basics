# 5.5 Other Editors

Any editor with Language Server Protocol support can use `terraform-ls`.

- **Sublime Text:** the LSP package plus an `LSP-terraform` helper package, installed through Package Control.
- **Emacs:** `lsp-mode` (hooked to `terraform-mode`) or `eglot` pointed at `terraform-ls`, with `terraform-mode` for syntax and indentation.
- **Zed:** not built in — install the community [Terraform extension](https://github.com/zed-extensions/terraform) from Zed's extension panel; it then runs `terraform-ls` for you.
- **Helix:** ships a `hcl` language definition (which also covers `.tf` files) preconfigured to use `terraform-ls`; install the binary on `PATH` and it attaches with no extra config.

For editors without a language server, you still get value from `terraform fmt` as a save hook and `terraform validate` as a build command.

## Practice

Pick your editor, install `terraform-ls` on the `PATH`, and confirm completion works inside a resource block. If it does not, check that the folder was initialized with `terraform init`.

## References

- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Helix language support](https://docs.helix-editor.com/lang-support.html)
- [Zed: Terraform language docs](https://zed.dev/docs/languages/terraform)
- [zed-extensions/terraform](https://github.com/zed-extensions/terraform)
