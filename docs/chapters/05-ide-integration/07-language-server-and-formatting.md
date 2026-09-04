# 5.7 Language Server and Formatting Configuration

Two settings do most of the work for a consistent Terraform editing experience: the language server and automatic formatting.

## Install the language server

Download `terraform-ls` from the official releases or install it with a package manager, and make sure it is on the `PATH`. Most editor extensions bundle it, but a system copy is useful for editors that do not.

## Common settings

`terraform-ls` settings are nested objects, passed by the editor as JSON or Lua tables. The three you are most likely to touch:

| Setting | Purpose |
| --- | --- |
| `terraform.path` | Use a specific `terraform` binary instead of the first one on `PATH` |
| `indexing.ignoreDirectoryNames` | Skip directories (for example vendored modules) when indexing a large workspace |
| `validation.enableEnhancedValidation` | Toggle the richer, schema-aware validation diagnostics (on by default) |

If you copy a configuration snippet from an older blog post or Stack Overflow answer, check it against the settings reference below first: flat keys like `terraformExecPath`, `terraformLogFilePath`, and `rootModulePaths` are deprecated in favor of the nested `terraform.*` and `indexing.*` forms shown above.

## Enforce formatting

`terraform fmt` is the canonical formatter. Wire it into the editor as format-on-save, and also enforce it outside the editor:

```bash
terraform fmt -check -recursive
```

Run that in CI so an unformatted file fails the pipeline instead of causing noisy diffs.

## Validation on demand

Bind a task or key to `terraform validate` so structural errors surface without a full `plan`. Remember `validate` does not contact the provider APIs; it only checks the configuration is internally consistent.

## Practice

Add `terraform fmt -check -recursive` to your project's CI or a pre-commit hook, then commit an intentionally misaligned file and confirm the check fails.

## References

- [terraform-ls releases](https://releases.hashicorp.com/terraform-ls/)
- [terraform-ls settings](https://github.com/hashicorp/terraform-ls/blob/main/docs/SETTINGS.md)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [Command: validate](https://developer.hashicorp.com/terraform/cli/commands/validate)
