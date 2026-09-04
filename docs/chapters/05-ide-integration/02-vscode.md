# 5.2 Visual Studio Code

Install the official **HashiCorp Terraform** extension (`HashiCorp.terraform` on the Marketplace). It is a self-contained install: it bundles `terraform-ls`, so you do not need to install the language server separately to get completion, diagnostics, and formatting.

## After installing

- Completion, hover docs, and inline diagnostics work once the folder contains initialized providers (`terraform init`).
- Enable formatting on save so `terraform fmt` runs automatically:

```json
{
  "editor.formatOnSave": true,
  "[terraform]": { "editor.defaultFormatter": "hashicorp.terraform" },
  "[terraform-vars]": { "editor.defaultFormatter": "hashicorp.terraform" }
}
```

- The command palette offers `Terraform: init`, `Terraform: validate`, and `Terraform: plan`, which call the CLI in the integrated terminal.

## Beyond formatting

The extension also adds a **Module and Provider Explorer** view that lists the modules and providers referenced by the open configuration, and an optional **Terraform MCP server** (`terraform.mcp.server.enable`, disabled by default) that lets AI assistants like Copilot query the Terraform Registry and provider schemas directly instead of guessing. Neither is required for the core editing workflow, but both are worth knowing about if your team pairs the extension with an AI assistant — see [5.6 AI-assisted workflows](06-ai-assisted-workflows.md).

## Practice

Open a working directory, run `terraform init` from the integrated terminal, then delete a required argument and confirm the red underline appears. Save the file and watch `fmt` realign the block.

## References

- [HashiCorp Terraform extension (Marketplace)](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)
- [vscode-terraform (source and docs)](https://github.com/hashicorp/vscode-terraform)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [terraform-ls settings reference](https://github.com/hashicorp/terraform-ls/blob/main/docs/SETTINGS.md)
- [Terraform MCP server](https://github.com/hashicorp/terraform-mcp-server)
