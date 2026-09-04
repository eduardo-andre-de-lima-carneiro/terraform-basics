# 1.1 Infrastructure as Code

Infrastructure as code describes servers, networks, and services in text files that are version controlled and reviewed. It lets a team recreate an environment, compare revisions, identify authors, and change infrastructure without relying on memory or manual clicks.

## The problem it solves

Without infrastructure as code, the real configuration lives only in a console and in the heads of a few people. Rebuilding or auditing it becomes guesswork. Terraform keeps the intended configuration in structured files instead.

## Practice

Write down every manual step you would take to create one small resource in your cloud console. That list is the value infrastructure as code provides: it turns those steps into a file you can review, repeat, and roll back.

## References

- [What is Terraform? (HashiCorp)](https://developer.hashicorp.com/terraform/intro)
- [Terraform use cases](https://developer.hashicorp.com/terraform/intro/use-cases)
- [Infrastructure as code (AWS)](https://aws.amazon.com/what-is/iac/)
