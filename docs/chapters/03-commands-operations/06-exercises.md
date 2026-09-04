# 3.6 Practical Exercises

Complete these in a temporary directory. Use the `null` provider so nothing real is created:

```hcl
terraform {
  required_providers {
    null = {
      source  = "hashicorp/null"
      version = "~> 3.2"
    }
  }
}

resource "null_resource" "example" {}
```

1. Run `terraform init`, then `terraform plan -out plan.tfplan` and `terraform apply plan.tfplan`.
2. Add a `triggers` argument to the resource, save a new plan with `-out`, and apply exactly that plan file.
3. Move the resource into a `./modules/example` child module, add a `moved` block for the address change, and confirm `terraform plan` reports no changes.
4. Practice recovery: run `terraform apply -replace=null_resource.example`, then `terraform state rm null_resource.example` followed by `terraform import null_resource.example demo-id`.
5. Run `terraform fmt -check -recursive` and `terraform plan -detailed-exitcode`; note the exit code with and without a pending change (`echo $?` right after each command).
6. Replace step 4's `state rm` with a `removed` block (`lifecycle { destroy = false }`) and confirm `terraform plan` shows the same removal for review before you apply it. Then write a matching `import` block instead of the `terraform import` command, and run `terraform plan -generate-config-out=generated.tf` to see Terraform draft the `resource` block for you.
7. Create a second directory `modules-remote-demo` with its own `null_resource` and an `output`. From the original directory, add a `terraform_remote_state` data source (`backend = "local"`, pointing at the other directory's `terraform.tfstate`) and reference its output in a `triggers` value.

For each exercise, record the command used, the state before it, and the result shown by `terraform plan` or `terraform show`.

## References

- [hashicorp/null provider docs](https://registry.terraform.io/providers/hashicorp/null/latest/docs)
- [null_resource](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource)
- [Import usage](https://developer.hashicorp.com/terraform/cli/import/usage)
- [Get started tutorials](https://developer.hashicorp.com/terraform/tutorials)
- [Generating configuration with `import` blocks](https://developer.hashicorp.com/terraform/language/import/generating-configuration)
- [`terraform_remote_state` data source](https://developer.hashicorp.com/terraform/language/state/remote-state-data)
