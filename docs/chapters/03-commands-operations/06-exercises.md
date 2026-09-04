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

For each exercise, record the command used, the state before it, and the result shown by `terraform plan` or `terraform show`.

## References

- [hashicorp/null provider docs](https://registry.terraform.io/providers/hashicorp/null/latest/docs)
- [null_resource](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource)
- [Import usage](https://developer.hashicorp.com/terraform/cli/import/usage)
- [Get started tutorials](https://developer.hashicorp.com/terraform/tutorials)
