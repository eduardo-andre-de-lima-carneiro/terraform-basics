# 2.2 Configure Providers and Credentials

A provider needs two things: a version constraint in your configuration and credentials in your environment.

## Declare the provider

```hcl
terraform {
  required_version = ">= 1.6"

  required_providers {
    local = {
      source  = "hashicorp/local"
      version = "~> 2.5"
    }
  }
}

provider "local" {}
```

`terraform init` reads this block and downloads the pinned provider into the working directory.

## Supply credentials safely

Most cloud providers read credentials from environment variables or a shared credentials file, never from the `.tf` files. Keep secrets out of the configuration and out of version control:

```bash
export EXAMPLE_API_TOKEN="REPLACE_ME"
```

## Practice

Create a `terraform` block that requires the `local` provider, run `terraform init`, and confirm a `.terraform.lock.hcl` file appears. Open it and note that it pins the provider version and checksums.

## References

- [Provider requirements](https://developer.hashicorp.com/terraform/language/providers/requirements)
- [Provider configuration](https://developer.hashicorp.com/terraform/language/providers/configuration)
- [Dependency lock file](https://developer.hashicorp.com/terraform/language/files/dependency-lock)
- [hashicorp/local provider docs](https://registry.terraform.io/providers/hashicorp/local/latest/docs)
- [Version constraints](https://developer.hashicorp.com/terraform/language/expressions/version-constraints)
