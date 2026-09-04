# 1.2 Why Terraform

Terraform is a widely used tool for infrastructure as code. It reads declarative configuration, talks to platforms through providers, and keeps a state file so it knows what it already manages.

## What makes it useful

- One workflow (`init`, `plan`, `apply`) works across many providers.
- A plan shows the exact changes before anything happens.
- State lets Terraform update and delete only what it created.
- Modules package configuration for reuse across teams and environments.

## Terraform compared to other tools

Terraform is one of several infrastructure automation tools, and each one optimizes for a different job. The table compares the tools you are most likely to meet on a team that already has some automation in place.

| Tool | Category | Approach | Configuration language | Execution model | Scope | State tracking |
| --- | --- | --- | --- | --- | --- | --- |
| **Terraform** | Provisioning (IaC) | Declarative, desired-state | HCL (or JSON) | Agentless; you or a pipeline run `plan`/`apply` | Many providers: cloud platforms, SaaS, on-prem, Kubernetes | Explicit state file records every managed resource |
| **Ansible** | Configuration management | Mostly procedural playbooks, with idempotent modules | YAML | Agentless; pushes modules over SSH/WinRM from a control node | Configuring and updating machines that already exist; can also call cloud APIs to provision | No dedicated state file; each run inspects the live system |
| **Chef / Puppet** | Configuration management | Declarative resources inside a DSL (Ruby for Chef, Puppet's own DSL) | Ruby (Chef) / Puppet DSL | Agent-based, classically a pull model from a central server | Configuring and updating machines that already exist | No infrastructure state file; agents reconcile the local machine each run |
| **AWS CloudFormation** | Provisioning (IaC) | Declarative, desired-state | JSON or YAML templates | Agentless; a managed AWS service runs the deployment | AWS only | State is the CloudFormation stack, managed by AWS |
| **Pulumi** | Provisioning (IaC) | Desired-state, expressed with general-purpose code | TypeScript, Python, Go, C#, Java, or YAML | Agentless; a CLI engine runs your program and applies the diff | Many providers, similar breadth to Terraform | State file, stored locally or in Pulumi Cloud |

## Reading the comparison

- **Provisioning versus configuration management.** Terraform, CloudFormation, and Pulumi create and change the infrastructure itself: the VM, the database, the network. Ansible, Chef, and Puppet mainly configure software on a machine that already exists. HashiCorp is explicit that "Terraform is not a configuration management tool," and the two categories are usually complementary rather than competing: Terraform provisions a server, then a bootstrapping step (or a follow-up Ansible playbook) configures the software running on it.
- **Multi-cloud versus single-cloud.** Terraform, Ansible, and Pulumi work across many providers with one workflow. CloudFormation is declarative and reliable, but it only manages AWS.
- **Domain-specific language versus general-purpose code.** Terraform's HCL and CloudFormation's JSON/YAML are intentionally narrow, which keeps a plan easy to read and review in a pull request. Pulumi trades that narrowness for the full power of a general-purpose language: loops, tests, and package managers you may already use, at the cost of a larger surface area to reason about.
- **State as the source of truth.** Terraform, CloudFormation, and Pulumi all compare a recorded state against reality before changing anything, which is what makes a "plan" possible. Ansible and classic configuration management tools generally do not keep that kind of infrastructure state file; they inspect the target each run and rely on each module being idempotent.

None of this makes Terraform the right tool for every job. A team already invested in Ansible for configuration management does not need to replace it; Terraform typically sits underneath, provisioning the machines Ansible then configures.

## Practice

Think of two platforms your team uses, such as a cloud provider and a DNS service. With Terraform, both are managed with the same commands and the same file format. Note where that consistency would save your team time today, and identify one task in your current stack that a configuration management tool would still do better than Terraform.

## References

- [What is Terraform? (HashiCorp)](https://developer.hashicorp.com/terraform/intro)
- [Terraform use cases](https://developer.hashicorp.com/terraform/intro/use-cases)
- [Terraform Registry: providers](https://registry.terraform.io/browse/providers)
- [Terraform workflow overview](https://developer.hashicorp.com/terraform/intro/core-workflow)
- [Terraform vs. other software (comparison index)](https://developer.hashicorp.com/terraform/intro/vs)
- [Terraform vs. Chef, Puppet, and other configuration management tools](https://developer.hashicorp.com/terraform/intro/vs/chef-puppet)
- [Terraform vs. CloudFormation](https://developer.hashicorp.com/terraform/intro/vs/cloudformation)
- [How Ansible works (Red Hat)](https://www.redhat.com/en/ansible-collaborative/how-ansible-works)
- [How Pulumi works](https://www.pulumi.com/docs/iac/concepts/how-pulumi-works/)
- [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
