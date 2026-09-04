# 5.6 AI-Assisted Workflows

Editor assistants can draft resource blocks, explain an error, or suggest a variable structure. They are useful for a first draft and for unfamiliar providers.

## Use them safely

- Treat generated HCL as a proposal. Run `terraform validate` and read the `terraform plan` before trusting it.
- Never paste real credentials, state files, or private identifiers into a prompt.
- Confirm argument names against the provider documentation; assistants invent plausible but wrong attributes.
- Be especially careful with anything that deletes or replaces resources.

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
