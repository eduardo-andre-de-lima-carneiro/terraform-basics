# 1.3 Terraform's Declarative Model

You describe the result you want, not the steps to get there. Terraform compares your configuration with the current state and the real platform, then decides which actions are needed.

## Declarative versus imperative

An imperative script says "create this, then attach that." A declarative configuration says "these resources should exist with these settings." If a resource already matches, Terraform does nothing. If it drifted, Terraform proposes a correction.

```bash
# Imperative: you specify every step, in order, and re-running it blindly can fail or duplicate work
aws ec2 create-vpc ...
aws ec2 create-subnet ...
aws ec2 create-security-group ...
```

```hcl
# Declarative: you specify the end state; Terraform works out the steps and their order
resource "aws_vpc" "main" { cidr_block = "10.0.0.0/16" }
resource "aws_subnet" "app" { vpc_id = aws_vpc.main.id, cidr_block = "10.0.1.0/24" }
```

The declarative block never says "create the VPC first." Terraform figures that out because the subnet's configuration references `aws_vpc.main.id`.

## Providers do the work

Terraform itself does not know any cloud API. Each provider translates resource blocks into API calls and reports the results back into state.

## How Terraform decides the order

Every reference between resources — `aws_vpc.main.id` in the example above — becomes an edge in a dependency graph. Terraform builds that graph from your configuration, checks it has no cycles, and walks it so that a resource is only created, updated, or destroyed once everything it depends on has finished. Resources that do not depend on each other can run at the same time, up to 10 in parallel by default, which is why a declarative model tends to apply faster than an equivalent imperative script as your configuration grows. You can inspect the graph yourself with `terraform graph`, and force an explicit edge with `depends_on` when a dependency exists but does not show up as an attribute reference.

Destroying resources walks a related but separate graph, because the safe order to delete things is often the reverse of the order used to create them.

## Where the declarative model strains

The model is not free of trade-offs:

- **Truly imperative steps still exist.** A database migration, a one-time data import, or a sequence-sensitive API call does not fit neatly into "this resource should exist." HashiCorp's own guidance on provisioners — the escape hatch for running a script during create or destroy — is to treat them as a last resort: check for a provider-native way to do the same thing first, because Terraform cannot model what a provisioner actually does and cannot know if it succeeded the way it tracks a resource.
- **The real platform can disagree with the graph.** Cloud APIs are sometimes eventually consistent, so a resource can report success before it is fully usable elsewhere, which occasionally surfaces as a dependency error even though the configuration was correct.
- **Declarative does not mean automatic.** You still choose the resources, the arguments, and the module boundaries; Terraform only automates the ordering and the diffing, not the design.

## Practice

Read a short resource block and describe, in plain language, the end state it declares. Then predict what Terraform would do if that resource already existed unchanged. Finally, run `terraform graph` on a small configuration with two related resources and identify the edge that corresponds to the reference between them.

## References

- [How Terraform works](https://developer.hashicorp.com/terraform/intro#how-does-terraform-work)
- [Providers (Terraform language)](https://developer.hashicorp.com/terraform/language/providers)
- [Resources overview](https://developer.hashicorp.com/terraform/language/resources)
- [Resource behavior and the dependency graph](https://developer.hashicorp.com/terraform/language/resources/behavior)
- [The dependency graph (internals)](https://developer.hashicorp.com/terraform/internals/graph)
- [Command: graph](https://developer.hashicorp.com/terraform/cli/commands/graph)
- [The `depends_on` meta-argument](https://developer.hashicorp.com/terraform/language/meta-arguments/depends_on)
- [Provisioners: a last resort](https://developer.hashicorp.com/terraform/language/resources/provisioners/syntax)
