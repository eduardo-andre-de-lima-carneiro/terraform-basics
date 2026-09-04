# 1.4 Terraform's Mental Model

Think in three places:

1. Configuration: the `.tf` files describing the desired state.
2. State: what Terraform recorded about the resources it manages.
3. Real infrastructure: what actually exists on the platform.

The basic flow is `write config -> terraform plan -> terraform apply`. `terraform plan` compares the three places and shows the difference; it should be your most frequent diagnostic command. Run it before every apply.

## References

- [State (Terraform language)](https://developer.hashicorp.com/terraform/language/state)
- [Purpose of Terraform state](https://developer.hashicorp.com/terraform/language/state/purpose)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Core workflow](https://developer.hashicorp.com/terraform/intro/core-workflow)
