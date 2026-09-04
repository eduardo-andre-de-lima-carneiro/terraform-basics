# 4.3 GitHub Actions

GitHub Actions ejecuta Terraform en un workflow disparado por pushes y pull requests. Encaja bien cuando el código ya vive en GitHub y quieres la salida del plan en el pull request.

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

## Buenas prácticas

- Ejecuta `plan` en los pull requests; ejecuta `apply` solo en `main`, en un job aparte que requiera la aprobación de un entorno.
- Almacena las credenciales de nube como secretos cifrados del repositorio o del entorno, o usa OIDC para no guardar ninguna clave de larga duración.
- Fija la action `setup-terraform` y la versión de Terraform.
- Publica el plan como comentario del pull request para que quienes revisan vean el efecto, no solo el diff.

Nunca vuelques secretos en los logs y nunca almacenes el state en el repositorio.

## Referencias

- [Automate Terraform with GitHub Actions (tutorial)](https://developer.hashicorp.com/terraform/tutorials/automation/github-actions)
- [hashicorp/setup-terraform action](https://github.com/hashicorp/setup-terraform)
- [Using secrets in GitHub Actions](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions)
- [OpenID Connect in cloud providers](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect)
- [Using environments for deployment](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment)
