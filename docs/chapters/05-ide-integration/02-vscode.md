# 5.2 Visual Studio Code

Install the official **HashiCorp Terraform** extension. It bundles `terraform-ls` and adds language features, formatting, and a small set of commands.

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

## Practice

Open a working directory, run `terraform init` from the integrated terminal, then delete a required argument and confirm the red underline appears. Save the file and watch `fmt` realign the block.

## References

- [HashiCorp Terraform extension (Marketplace)](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)
- [vscode-terraform (source and docs)](https://github.com/hashicorp/vscode-terraform)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
