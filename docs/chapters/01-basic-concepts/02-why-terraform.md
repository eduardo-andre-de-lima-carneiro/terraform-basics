# 1.2 Why Terraform

Terraform is a widely used tool for infrastructure as code. It reads declarative configuration, talks to platforms through providers, and keeps a state file so it knows what it already manages.

## What makes it useful

- One workflow (`init`, `plan`, `apply`) works across many providers.
- A plan shows the exact changes before anything happens.
- State lets Terraform update and delete only what it created.
- Modules package configuration for reuse across teams and environments.

## Practice

Think of two platforms your team uses, such as a cloud provider and a DNS service. With Terraform, both are managed with the same commands and the same file format. Note where that consistency would save your team time today.

## References

- [What is Terraform? (HashiCorp)](https://developer.hashicorp.com/terraform/intro)
- [Terraform use cases](https://developer.hashicorp.com/terraform/intro/use-cases)
- [Terraform Registry: providers](https://registry.terraform.io/browse/providers)
- [Terraform workflow overview](https://developer.hashicorp.com/terraform/intro/core-workflow)
