# 4.2 HCP Terraform

HCP Terraform (antes Terraform Cloud) es la plataforma gestionada de HashiCorp para ejecutar Terraform. Combina state remoto, ejecuciones remotas, un registro de módulos privado y la aplicación de políticas.

## Conectar y ejecutar

Añade un bloque `cloud` a la configuración e inicia sesión:

```hcl
terraform {
  cloud {
    organization = "EXAMPLE_ORG"

    workspaces {
      name = "practice"
    }
  }
}
```

```bash
terraform login
terraform init
```

`plan` y `apply` se ejecutan ahora en el workspace, no en tu portátil, y el state se almacena allí automáticamente.

## Funciones útiles

- Los workspaces gestionados por VCS ejecutan un plan en cada pull request y retienen los applies para su aprobación.
- El state se almacena, versiona, bloquea y cifra por la plataforma.
- Las políticas de Sentinel u OPA pueden bloquear un plan que infrinja una regla.
- Los conjuntos de variables mantienen las credenciales de los providers fuera del repositorio.

Usa un token de API de equipo o una identidad con alcance de workspace y privilegios mínimos. Nunca versiones un token de `terraform login`.

## Referencias

- [HCP Terraform documentation](https://developer.hashicorp.com/terraform/cloud-docs)
- [The `cloud` block](https://developer.hashicorp.com/terraform/cli/cloud/settings)
- [VCS-driven runs](https://developer.hashicorp.com/terraform/cloud-docs/run/ui)
- [Policy enforcement](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement)
- [API tokens](https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/api-tokens)
