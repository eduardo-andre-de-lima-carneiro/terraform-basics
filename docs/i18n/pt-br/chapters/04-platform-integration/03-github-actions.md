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
      - uses: hashicorp/setup-terraform@v4
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

## Chaves estáticas ou OIDC

Um secret de repositório com uma chave de acesso de nuvem é simples de configurar, mas é uma credencial de longa duração: se vazar, continua funcionando até alguém rotacioná-la, e o workflow pode usá-la a partir de qualquer branch que consiga ler o secret. O OIDC elimina a chave armazenada por completo: o provedor de nuvem confia no emissor de tokens do GitHub para um repositório e uma branch específicos, e devolve uma credencial que expira junto com o job. A contrapartida é o custo de configuração: o OIDC exige uma configuração de confiança única do lado da nuvem, além desta permissão no job ou workflow para que ele possa solicitar um token:

```yaml
permissions:
  id-token: write
  contents: read
```

Para um projeto de treinamento ou pessoal de vida curta, um secret de repositório com escopo limitado é um ponto de partida razoável; para algo mais duradouro ou de propriedade de uma equipe, o OIDC vale o esforço extra de configuração.

## Referências

- [Automate Terraform with GitHub Actions (tutorial)](https://developer.hashicorp.com/terraform/tutorials/automation/github-actions)
- [hashicorp/setup-terraform action](https://github.com/hashicorp/setup-terraform)
- [Using secrets in GitHub Actions](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions)
- [About security hardening with OpenID Connect](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect)
- [Configuring OpenID Connect in cloud providers](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-cloud-providers)
- [Using environments for deployment](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment)
