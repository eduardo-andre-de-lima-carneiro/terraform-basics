# 4.3 GitHub Actions

O GitHub Actions executa o Terraform em um workflow disparado por pushes e pull requests. É uma boa escolha quando o código já vive no GitHub e você quer a saída do plano no pull request.

## Workflow mínimo

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

## Boas práticas

- Execute `plan` em pull requests; execute `apply` apenas na `main`, em um job separado que exija a aprovação de um environment.
- Armazene as credenciais de nuvem como secrets criptografados de repositório ou de environment, ou use OIDC para que nenhuma chave de longa duração seja armazenada.
- Fixe a versão da action `setup-terraform` e a versão do Terraform.
- Publique o plano como um comentário no pull request para que os revisores vejam o efeito, não apenas o diff.

Nunca ecoe segredos nos logs e nunca armazene o state no repositório.

## Referências

- [Automate Terraform with GitHub Actions (tutorial)](https://developer.hashicorp.com/terraform/tutorials/automation/github-actions)
- [hashicorp/setup-terraform action](https://github.com/hashicorp/setup-terraform)
- [Using secrets in GitHub Actions](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions)
- [OpenID Connect in cloud providers](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect)
- [Using environments for deployment](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment)
