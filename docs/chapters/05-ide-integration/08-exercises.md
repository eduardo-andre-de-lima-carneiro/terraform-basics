# 5.8 Practical Exercises

Complete these in a temporary working directory, using your editor for authoring and a terminal to verify.

1. Trigger completion inside a `resource` block and accept a suggested argument, then confirm with `terraform validate`.
2. Delete a required argument and confirm the editor shows an inline diagnostic before you run any command.
3. Enable format-on-save, misalign a block on purpose, save, and watch `terraform fmt` fix it.
4. Configure `terraform-ls` in an editor that does not bundle it, and confirm the client reports the server as attached.
5. Add `terraform fmt -check -recursive` as a pre-commit hook and confirm an unformatted file blocks the commit.
6. Use an editor task to run `terraform plan` and read the result without leaving the editor.

For each exercise, record the editor action taken and the command output that confirmed the result.

## References

- [terraform-ls](https://github.com/hashicorp/terraform-ls)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [pre-commit-terraform hooks](https://github.com/antonbabenko/pre-commit-terraform)
