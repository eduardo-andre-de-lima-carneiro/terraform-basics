# 2.3 Working Directories and Backends

A Terraform working directory is any folder that contains `.tf` files. When you run `terraform init`, Terraform prepares that folder: it downloads providers and configures the backend.

## Local backend

By default, state is written to `terraform.tfstate` in the working directory. This is fine for learning and for solo experiments.

## Remote backend

A remote backend stores state in a shared service so a team does not overwrite each other and secrets are not left on a laptop:

```hcl
terraform {
  backend "s3" {
    bucket = "example-tfstate"
    key    = "practice/terraform.tfstate"
    region = "us-east-1"
  }
}
```

Replace every value with placeholders you control. Changing a backend requires `terraform init` again, and Terraform offers to migrate the existing state.

## Practice

Initialize a directory with the default local backend and locate `terraform.tfstate`. Add it to `.gitignore` and note why committing state is unsafe: it can contain secrets and causes conflicts.

## References

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [State: sensitive data](https://developer.hashicorp.com/terraform/language/state/sensitive-data)
- [S3 backend](https://developer.hashicorp.com/terraform/language/backend/s3)
- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
