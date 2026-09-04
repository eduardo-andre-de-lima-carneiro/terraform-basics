# 5.6 AI-Assisted Workflows

Editor assistants can draft resource blocks, explain an error, or suggest a variable structure. They are useful for a first draft and for unfamiliar providers.

## Use them safely

- Treat generated HCL as a proposal. Run `terraform validate` and read the `terraform plan` before trusting it.
- Never paste real credentials, state files, or private identifiers into a prompt.
- Confirm argument names against the provider documentation; assistants invent plausible but wrong attributes. A common failure mode: the suggestion is valid HCL and even matches an argument that existed in an older provider version, but was renamed or removed since — it passes a casual read, fails `terraform validate` or, worse, silently changes on `plan` because the attribute is simply ignored.
- Be especially careful with anything that deletes or replaces resources.
- Some editor extensions now expose provider schemas to the assistant directly instead of relying on its training data — for example the HashiCorp Terraform VS Code extension ships an opt-in Terraform MCP server (`terraform.mcp.server.enable`) that lets a connected assistant query the Terraform Registry. That narrows the "invented attribute" failure mode above, but does not remove the need to read the plan.

## Where they help most

- Explaining a validation or plan error in plain language.
- Converting a console setup into a first-draft resource block.
- Suggesting `for_each` or `dynamic` block structure to remove repetition.

## Practice

Ask an assistant to generate a `local_file` resource, then verify every argument against the provider docs and confirm the plan does exactly what you expected.

## References

- [Terraform Registry (authoritative provider schemas)](https://registry.terraform.io/)
- [Style and validation: `terraform validate`](https://developer.hashicorp.com/terraform/cli/commands/validate)
- [`for_each` and `dynamic` blocks](https://developer.hashicorp.com/terraform/language/meta-arguments/for_each)
- [Terraform style guide](https://developer.hashicorp.com/terraform/language/style)
- [Terraform MCP server](https://github.com/hashicorp/terraform-mcp-server)
