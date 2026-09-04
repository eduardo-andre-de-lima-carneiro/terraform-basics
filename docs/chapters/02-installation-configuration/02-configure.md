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

## Credential variables for common providers

Each provider follows its own platform's standard authentication method rather than a Terraform-specific one, so the same variables also work with that platform's own CLI or SDK:

| Provider | Typical environment variables | Alternative |
| --- | --- | --- |
| `hashicorp/aws` | `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN` | Shared credentials file (`~/.aws/credentials`) plus `AWS_PROFILE` |
| `hashicorp/azurerm` | `ARM_CLIENT_ID`, `ARM_CLIENT_SECRET`, `ARM_SUBSCRIPTION_ID`, `ARM_TENANT_ID` | Interactive `az login`, which Terraform can use directly |
| `hashicorp/google` | `GOOGLE_CREDENTIALS` (inline JSON) or `GOOGLE_APPLICATION_CREDENTIALS` (path to a key file) | Application Default Credentials from `gcloud auth application-default login` |

Prefer a short-lived identity (an assumed role, a workload identity, or an interactive CLI login) over a long-lived static key whenever the platform offers one.

## Practice

Create a `terraform` block that requires the `local` provider, run `terraform init`, and confirm a `.terraform.lock.hcl` file appears. Open it and note that it pins the provider version and checksums.

## References

- [Provider requirements](https://developer.hashicorp.com/terraform/language/providers/requirements)
- [Provider configuration](https://developer.hashicorp.com/terraform/language/providers/configuration)
- [Dependency lock file](https://developer.hashicorp.com/terraform/language/files/dependency-lock)
- [hashicorp/local provider docs](https://registry.terraform.io/providers/hashicorp/local/latest/docs)
- [Version constraints](https://developer.hashicorp.com/terraform/language/expressions/version-constraints)
- [AWS CLI environment variables (same variables the AWS provider reads)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)
- [Authenticate Terraform to Azure with a service principal](https://learn.microsoft.com/en-us/azure/developer/terraform/authenticate-to-azure-with-service-principle)
- [Google: Application Default Credentials](https://cloud.google.com/docs/authentication/application-default-credentials)
