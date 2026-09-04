# 4.3 GitHub Actions

GitHub Actions exécute Terraform dans un workflow déclenché par des pushes et des pull requests. C'est un bon choix quand le code réside déjà sur GitHub et que vous voulez la sortie du plan sur la pull request.

## Workflow minimal

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

## Bonnes pratiques

- Exécutez `plan` sur les pull requests ; n'exécutez `apply` que sur `main`, dans un job distinct qui nécessite l'approbation d'un environnement.
- Stockez les identifiants cloud comme secrets chiffrés de dépôt ou d'environnement, ou utilisez OIDC afin qu'aucune clé à longue durée de vie ne soit stockée.
- Épinglez l'action `setup-terraform` et la version de Terraform.
- Publiez le plan en commentaire de pull request pour que les relecteurs voient l'effet, pas seulement le diff.

N'affichez jamais de secrets dans les logs et ne stockez jamais le state dans le dépôt.

## Références

- [Automate Terraform with GitHub Actions (tutorial)](https://developer.hashicorp.com/terraform/tutorials/automation/github-actions)
- [hashicorp/setup-terraform action](https://github.com/hashicorp/setup-terraform)
- [Using secrets in GitHub Actions](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions)
- [OpenID Connect in cloud providers](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect)
- [Using environments for deployment](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment)
