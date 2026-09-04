# 2.3 Working Directories and Backends

A Terraform working directory is any folder that contains `.tf` files. When you run `terraform init`, Terraform prepares that folder: it downloads providers and configures the backend.

## Local backend

By default, state is written to `terraform.tfstate` in the working directory. This is fine for learning and for solo experiments.

## Remote backend

A remote backend stores state in a shared service so a team does not overwrite each other and secrets are not left on a laptop:

```hcl
terraform {
  backend "s3" {
    bucket       = "example-tfstate"
    key          = "practice/terraform.tfstate"
    region       = "us-east-1"
    use_lockfile = true # native S3 locking; no DynamoDB table required
  }
}
```

Replace every value with placeholders you control. Changing a backend requires `terraform init` again, and Terraform offers to migrate the existing state.

## Choosing a backend

Terraform ships backends for many targets, not just S3. A few common ones:

| Backend | Typical use |
| --- | --- |
| `local` | Default; single machine, learning, solo experiments |
| `s3` | AWS-centric teams; state in an S3 bucket, with native S3 locking (`use_lockfile`) |
| `azurerm` | Azure-centric teams; state in a storage account container |
| `gcs` | Google Cloud–centric teams; state in a Cloud Storage bucket |
| `remote` / the `cloud` block | HCP Terraform or Terraform Enterprise; state, locking, and run history all managed for you (see Chapter 4) |

## Locking

Locking prevents two runs from writing state at the same time. The S3 backend's `use_lockfile = true` option is the current recommended approach; the older DynamoDB-table-based locking is deprecated. Whichever backend you pick, confirm it locks before relying on it for a team.

## Practice

Initialize a directory with the default local backend and locate `terraform.tfstate`. Add it to `.gitignore` and note why committing state is unsafe: it can contain secrets and causes conflicts.

## References

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [Backend types](https://developer.hashicorp.com/terraform/language/backend#available-backends)
- [State: sensitive data](https://developer.hashicorp.com/terraform/language/state/sensitive-data)
- [S3 backend](https://developer.hashicorp.com/terraform/language/backend/s3)
- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
