# 1.3 Terraform's Declarative Model

You describe the result you want, not the steps to get there. Terraform compares your configuration with the current state and the real platform, then decides which actions are needed.

## Declarative versus imperative

An imperative script says "create this, then attach that." A declarative configuration says "these resources should exist with these settings." If a resource already matches, Terraform does nothing. If it drifted, Terraform proposes a correction.

## Providers do the work

Terraform itself does not know any cloud API. Each provider translates resource blocks into API calls and reports the results back into state.

## Practice

Read a short resource block and describe, in plain language, the end state it declares. Then predict what Terraform would do if that resource already existed unchanged.

## References

- [How Terraform works](https://developer.hashicorp.com/terraform/intro#how-does-terraform-work)
- [Providers (Terraform language)](https://developer.hashicorp.com/terraform/language/providers)
- [Resources overview](https://developer.hashicorp.com/terraform/language/resources)
- [Resource behavior and the dependency graph](https://developer.hashicorp.com/terraform/language/resources/behavior)
