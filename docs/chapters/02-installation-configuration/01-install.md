# 2.1 Install Terraform

Terraform is a single binary. Install it with your platform's package manager or download it from the official releases.

## Common options

```bash
# macOS (Homebrew)
brew tap hashicorp/tap
brew install hashicorp/tap/terraform

# Debian and Ubuntu use the official HashiCorp apt repository
# Windows uses winget or Chocolatey
```

## Verify the installation

```bash
terraform version
```

The command prints the CLI version and, once a working directory is initialized, the provider versions in use. If the version is old, upgrade before continuing so the examples behave as described.

## Practice

Install Terraform, run `terraform version`, and record the version string. Run `terraform -help` and skim the list of subcommands you will meet in Chapter 3.

## References

- [Install Terraform](https://developer.hashicorp.com/terraform/install)
- [Official packaging repository (releases.hashicorp.com)](https://releases.hashicorp.com/terraform/)
- [Command: version](https://developer.hashicorp.com/terraform/cli/commands/version)
- [Basic CLI features](https://developer.hashicorp.com/terraform/cli/commands)
