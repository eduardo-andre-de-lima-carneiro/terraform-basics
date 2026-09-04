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
      - uses: hashicorp/setup-terraform@v4
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

## Claves estáticas frente a OIDC

Un secreto de repositorio que guarda una clave de acceso de nube es simple de configurar, pero es una credencial de larga duración: si se filtra, funciona hasta que alguien la rota, y el workflow puede usarla desde cualquier rama que pueda leer el secreto. OIDC elimina la clave almacenada por completo: el proveedor de nube confía en el emisor de tokens de GitHub para un repositorio y una rama concretos, y devuelve una credencial que expira junto con el job. La contrapartida es el costo de configuración: OIDC necesita una configuración de confianza única del lado de la nube, además de este permiso en el job o workflow para que pueda solicitar un token:

```yaml
permissions:
  id-token: write
  contents: read
```

Para un proyecto de formación o personal de vida corta, un secreto de repositorio con alcance limitado es un punto de partida razonable; para algo de mayor duración o propiedad de un equipo, OIDC vale la configuración adicional.

## Referencias

- [Automate Terraform with GitHub Actions (tutorial)](https://developer.hashicorp.com/terraform/tutorials/automation/github-actions)
- [hashicorp/setup-terraform action](https://github.com/hashicorp/setup-terraform)
- [Using secrets in GitHub Actions](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions)
- [About security hardening with OpenID Connect](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect)
- [Configuring OpenID Connect in cloud providers](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-cloud-providers)
- [Using environments for deployment](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment)
