# 1.1 Infrastructure as Code

Infrastructure as code (IaC) uses a descriptive, versioned model to define and deploy infrastructure — networks, virtual machines, load balancers, databases, and the topology that connects them — instead of manual processes and console clicks. Just as the same source code always produces the same binary, an IaC configuration produces the same environment every time it deploys.

## The problem it solves

Without infrastructure as code, the real configuration lives only in a console and in the heads of a few people. Each environment drifts into a unique, undocumented "snowflake" that cannot be reproduced automatically, and rebuilding or auditing it becomes guesswork. Terraform and similar tools keep the intended configuration in structured files instead, so infrastructure changes go through the same review and version-control history as application code.

## Practical examples

- **Multi-region or multi-account duplication.** Apply the same configuration to stand up an identical environment in a second region or cloud account, instead of repeating manual setup by hand.
- **Ephemeral test and review environments.** Provision a full stack for a pull request or a load test, then tear it down when the work is done, so environments stop being scarce, shared, and stale.
- **Disaster recovery.** Rebuild the production topology from configuration in a new region after an outage, instead of reconstructing it from memory and support tickets.
- **Standardized landing zones.** Give every team the same baseline network, logging, and access-control setup by reusing one module, instead of onboarding each team by hand.

## Benefits

- **Consistency.** The same configuration always produces the same environment, which removes configuration drift and "it works in my environment" surprises.
- **Idempotence.** Applying a configuration repeatedly converges the environment to the same state, whether the target starts empty or partially built.
- **Speed at scale.** Environments that used to take days of manual work can be provisioned, duplicated, or torn down in minutes.
- **Review and rollback.** Infrastructure changes go through the same pull request, diff, and version history as application code, so a bad change can be reviewed before it ships and reverted after.
- **A shared language between teams.** Developers and operators read the same files, which reduces the handoffs and translation errors of ticket-based provisioning.

## Challenges

- **A learning curve.** Teams have to learn a configuration language, a state model, and a provider's resource schema before they are productive.
- **State and drift management.** If someone changes a resource outside the tool, such as directly in a console, the recorded state and reality disagree until it is reconciled.
- **Blast radius.** A single bad `apply` can change or destroy many resources at once; the safeguards covered in Chapter 3, such as reviewing a plan first, exist because of this risk.
- **Secrets and sensitive data.** Configuration and state can end up holding credentials or other sensitive values if they are not handled deliberately.
- **Testing infrastructure code.** Validating a configuration thoroughly usually means actually provisioning it somewhere, which costs time and money that testing application code does not.

## Practice

Write down every manual step you would take to create one small resource in your cloud console. That list is the value infrastructure as code provides: it turns those steps into a file you can review, repeat, and roll back. Then pick one item from the challenges above and note how your team's process would need to change to handle it.

## References

- [What is infrastructure as code? (Microsoft Learn, Azure DevOps)](https://learn.microsoft.com/en-us/devops/deliver/what-is-infrastructure-as-code)
- [What is infrastructure as code? (AWS)](https://aws.amazon.com/what-is/iac/)
- [What is Terraform? (HashiCorp)](https://developer.hashicorp.com/terraform/intro)
- [Terraform use cases](https://developer.hashicorp.com/terraform/intro/use-cases)
