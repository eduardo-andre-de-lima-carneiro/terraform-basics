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

## Cómo es una ejecución guiada por VCS

Conecta un workspace a un repositorio (en vez del bloque `cloud` de arriba, o además de él) y cada pull request recibe un **plan especulativo**: una ejecución de solo plan que no se puede aplicar, publicada en el pull request como un status check para que quienes revisan vean el efecto antes del merge. Una ejecución sobre la rama seguida va más allá y pasa por etapas en orden: plan, luego estimación de costos si la organización la habilitó, luego cualquier comprobación de políticas, y por último apply. El apply normalmente se detiene a esperar que una persona seleccione **Confirm & Apply** en la interfaz, salvo que el workspace esté configurado para auto-apply.

## Funciones útiles

- Los workspaces gestionados por VCS ejecutan un plan en cada pull request y retienen los applies para su aprobación.
- El state se almacena, versiona, bloquea y cifra por la plataforma.
- Las políticas de Sentinel u OPA pueden bloquear un plan que infrinja una regla.
- Los conjuntos de variables mantienen las credenciales de los providers fuera del repositorio.
- La estimación de costos (desactivada por defecto; la habilita alguien con rol de propietario de la organización) muestra el delta de costo mensual estimado para recursos de AWS, GCP y Azure como un paso entre plan y apply.
- En lugar de `workspaces { name = "..." }`, una configuración puede apuntar a workspaces por `project` o por `tags`, de modo que un solo bloque coincida con varios workspaces.

Usa un token de API de equipo o una identidad con alcance de workspace y privilegios mínimos. Nunca versiones un token de `terraform login`.

## Referencias

- [HCP Terraform documentation](https://developer.hashicorp.com/terraform/cloud-docs)
- [The `cloud` block](https://developer.hashicorp.com/terraform/cli/cloud/settings)
- [VCS-driven runs](https://developer.hashicorp.com/terraform/cloud-docs/run/ui)
- [Cost estimation](https://developer.hashicorp.com/terraform/cloud-docs/cost-estimation)
- [Policy enforcement](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement)
- [API tokens](https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/api-tokens)
