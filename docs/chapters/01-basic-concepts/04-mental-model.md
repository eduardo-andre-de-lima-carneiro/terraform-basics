# 1.4 Terraform's Mental Model

Think in three places:

1. Configuration: the `.tf` files describing the desired state.
2. State: what Terraform recorded about the resources it manages.
3. Real infrastructure: what actually exists on the platform.

The basic flow is `write config -> terraform plan -> terraform apply`. `terraform plan` compares the three places and shows the difference; it should be your most frequent diagnostic command. Run it before every apply.

## Why Terraform needs a third place

It would be simpler if Terraform only compared configuration to real infrastructure, but two problems rule that out. First, some information cannot be recovered from the platform alone — when you delete a resource from the configuration, Terraform has to know how to remove it and in what order, and a cloud API does not expose that relationship. Second, querying every resource's full attributes on every command does not scale: for a large environment it would mean constant API calls, latency, and rate limiting. State solves both: it is the cache and the map that connects a resource block like `aws_instance.web` to a real object such as instance `i-abcd1234`, without which "Terraform is unable to function," as HashiCorp puts it.

## Walking the loop with a practical example

```bash
terraform apply    # config now matches state now matches real infrastructure
# ...edit main.tf, changing one argument...
terraform plan      # compares all three places; shows exactly one attribute changing
terraform apply     # real infrastructure and state catch up to the new configuration
```

If you instead change something by hand in the provider's console, only state and real infrastructure fall out of sync; the next `terraform plan` refreshes state from the real platform by default and reports the drift, usually as a proposal to change the resource back to match the configuration.

## What lives where

| Question | Answer lives in | How to check |
| --- | --- | --- |
| What do I want to exist? | Configuration | Read the `.tf` files |
| What does Terraform believe exists? | State | `terraform show`, `terraform state list` |
| What actually exists on the platform? | Real infrastructure | The provider's console or API, or a fresh `terraform plan` |
| What will change if I apply now? | The diff between all three | `terraform plan` |

## Common pitfalls

- **Treating state as disposable.** Deleting or hand-editing the state file does not delete the real resources; it only makes Terraform forget about them, which usually leads to failed applies or duplicate resources.
- **Applying without a fresh plan.** Skipping `plan`, or applying a plan that is no longer current, means you are trusting a stale comparison of the three places.
- **Assuming state is optional for one person.** Even solo, an interrupted apply can leave state, configuration, and reality out of step; the recovery techniques in Chapter 3 exist for exactly this.
- **Forgetting that "real infrastructure" can change without Terraform.** Anything another engineer, another script, or a console session changes will not show up until the next `plan` refreshes state.

## Practice

Apply a small configuration, then change the same resource by hand outside Terraform (for example, edit a local file's content directly instead of through configuration). Run `terraform plan` and identify which of the three places moved, and which two stayed in agreement.

## References

- [State (Terraform language)](https://developer.hashicorp.com/terraform/language/state)
- [Purpose of Terraform state](https://developer.hashicorp.com/terraform/language/state/purpose)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Command: refresh](https://developer.hashicorp.com/terraform/cli/commands/refresh)
- [Core workflow](https://developer.hashicorp.com/terraform/intro/core-workflow)
