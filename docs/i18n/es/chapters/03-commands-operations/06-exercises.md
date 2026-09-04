# 3.6 Ejercicios prácticos

Completa estos ejercicios en un directorio temporal. Usa el provider `null` para que no se cree nada real:

```hcl
terraform {
  required_providers {
    null = {
      source  = "hashicorp/null"
      version = "~> 3.2"
    }
  }
}

resource "null_resource" "example" {}
```

1. Ejecuta `terraform init`, luego `terraform plan -out plan.tfplan` y `terraform apply plan.tfplan`.
2. Añade un argumento `triggers` al recurso, guarda un nuevo plan con `-out` y aplica exactamente ese archivo de plan.
3. Mueve el recurso a un módulo hijo `./modules/example`, añade un bloque `moved` para el cambio de dirección y confirma que `terraform plan` informa de que no hay cambios.
4. Practica la recuperación: ejecuta `terraform apply -replace=null_resource.example`, luego `terraform state rm null_resource.example` seguido de `terraform import null_resource.example demo-id`.
5. Ejecuta `terraform fmt -check -recursive` y `terraform plan -detailed-exitcode`; anota el código de salida con y sin un cambio pendiente (`echo $?` justo después de cada comando).
6. Sustituye el `state rm` del paso 4 por un bloque `removed` (`lifecycle { destroy = false }`) y confirma que `terraform plan` muestra la misma eliminación para revisarla antes de aplicarla. Después escribe un bloque `import` en lugar del comando `terraform import`, y ejecuta `terraform plan -generate-config-out=generated.tf` para ver cómo Terraform redacta el bloque `resource` por ti.
7. Crea un segundo directorio `modules-remote-demo` con su propio `null_resource` y un `output`. Desde el directorio original, añade una fuente de datos `terraform_remote_state` (`backend = "local"`, apuntando al `terraform.tfstate` del otro directorio) y referencia su output en un valor de `triggers`.

Para cada ejercicio, anota el comando utilizado, el state anterior y el resultado mostrado por `terraform plan` o `terraform show`.

## Referencias

- [hashicorp/null provider docs](https://registry.terraform.io/providers/hashicorp/null/latest/docs)
- [null_resource](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource)
- [Import usage](https://developer.hashicorp.com/terraform/cli/import/usage)
- [Get started tutorials](https://developer.hashicorp.com/terraform/tutorials)
- [Generating configuration with `import` blocks](https://developer.hashicorp.com/terraform/language/import/generating-configuration)
- [`terraform_remote_state` data source](https://developer.hashicorp.com/terraform/language/state/remote-state-data)
