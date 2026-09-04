# 4.3 GitHub Actions

GitHub Actions runs Terraform in a workflow triggered by pushes and pull requests. It is a good fit when the code already lives on GitHub and you want plan output on the pull request.

## Minimal workflow

```yaml
name: terraform
on:
  pull_request:
  push:
    branches: [main]

jobs:
  terraform:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: hashicorp/setup-terraform@v3
      - run: terraform init
      - run: terraform validate
      - run: terraform plan -no-color
```

## Good practices

- Run `plan` on pull requests; run `apply` only on `main`, in a separate job that needs an environment approval.
- Store cloud credentials as encrypted repository or environment secrets, or use OIDC so no long-lived key is stored.
- Pin the `setup-terraform` action and the Terraform version.
- Post the plan as a pull request comment so reviewers see the effect, not just the diff.

Never echo secrets into logs and never store state in the repository.

## References

- [Automate Terraform with GitHub Actions (tutorial)](https://developer.hashicorp.com/terraform/tutorials/automation/github-actions)
- [hashicorp/setup-terraform action](https://github.com/hashicorp/setup-terraform)
- [Using secrets in GitHub Actions](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions)
- [OpenID Connect in cloud providers](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect)
- [Using environments for deployment](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment)
