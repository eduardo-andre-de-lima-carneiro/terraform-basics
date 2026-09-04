# 5.5 Other Editors

Any editor with Language Server Protocol support can use `terraform-ls`.

- **Sublime Text:** the LSP package plus an `LSP-terraform` helper configuration.
- **Emacs:** `lsp-mode` or `eglot` pointed at `terraform-ls`, with `terraform-mode` for syntax.
- **Zed:** ships Terraform language support that uses `terraform-ls` automatically.
- **Helix:** built-in configuration for `terraform-ls`; install the binary and it attaches.

For editors without a language server, you still get value from `terraform fmt` as a save hook and `terraform validate` as a build command.

## Practice

Pick your editor, install `terraform-ls` on the `PATH`, and confirm completion works inside a resource block. If it does not, check that the folder was initialized with `terraform init`.

## References

- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Helix language support](https://docs.helix-editor.com/lang-support.html)
- [Zed languages](https://zed.dev/docs/languages)
