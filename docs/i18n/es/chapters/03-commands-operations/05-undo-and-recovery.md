# 3.5 Deshacer cambios con seguridad

No existe un único "deshacer" en Terraform. Eliges una acción de recuperación según lo que haya salido mal.

## Las opciones principales

- **Revertir la configuración** en el control de versiones, y luego `plan` y `apply`. Este es el deshacer normal y seguro.
- **`terraform apply -replace=<address>`** destruye y vuelve a crear un recurso que está roto pero sigue rastreado.
- **`terraform state rm <address>`** le dice a Terraform que olvide un recurso sin eliminarlo. El objeto real permanece. Si no lo vuelves a importar, el recurso queda sin gestionar y un `apply` posterior de la misma configuración intentará crear un duplicado, lo que puede fallar por un conflicto de nombre. Úsalo solo justo antes de volver a importarlo.
- **`terraform import <address> <id>`** vuelve a poner bajo gestión un objeto existente.
- **`terraform destroy`** elimina todos los recursos registrados en el state para esta configuración. Es destructivo e irreversible; ejecuta `terraform plan -destroy` primero y nunca lo apuntes a infraestructura compartida.

Es posible que veas material antiguo mencionar **`terraform taint`**: está en desuso (deprecated). Usa `apply -replace` en su lugar; a diferencia de `taint`, `-replace` muestra el reemplazo en un plan que apruebas, en vez de marcar el recurso en silencio para que se reemplace en el siguiente `apply` que se ejecute (posiblemente de otra persona).

## Qué comando usar según la situación

| Situación | Comando | Alcance | Camino de deshacer |
| --- | --- | --- | --- |
| Un cambio de configuración estaba mal | Revertir el cambio en el `.tf` en el control de versiones, luego `plan`/`apply` | Lo que toque el diff revertido | Volver a aplicar la configuración anterior de nuevo |
| Un recurso está roto pero Terraform lo sigue rastreando correctamente | `terraform apply -replace=<address>` | Una instancia de recurso | Volver a ejecutar `-replace`, o aceptar el nuevo objeto |
| Terraform debe dejar de gestionar un recurso, sin eliminarlo | `terraform state rm <address>` (o un bloque `removed`, ver abajo) | Un recurso, eliminado solo del state | `terraform import` para traerlo de vuelta con la misma dirección u otra |
| Un objeto existe en la plataforma pero no en el state | `terraform import <address> <id>` | Un recurso, añadido solo al state | `terraform state rm` para quitarlo de nuevo |
| Hay que eliminar todos los recursos de esta configuración | `terraform destroy` | Todo lo que hay en este state | Ninguno: los objetos se eliminan de verdad; restaura desde una copia de seguridad/instantánea si la plataforma la ofrece |

## Alternativas declarativas a los comandos de state

Dos tipos de bloque te permiten expresar `state rm` e `import` como configuración revisable en lugar de comandos puntuales, útil cuando el cambio debe pasar por la misma revisión de pull request que todo lo demás:

```hcl
# Stop managing a resource without destroying the real object,
# and see the removal in `terraform plan` before it happens.
removed {
  from = local_file.notes

  lifecycle {
    destroy = false
  }
}
```

```hcl
# Bring an existing object under management, with the plan showing
# exactly what will be imported before you approve it.
import {
  to = local_file.notes
  id = "notes.txt"
}

resource "local_file" "notes" {
  filename = "notes.txt"
  content  = "shared example"
}
```

Ambos son más seguros que sus equivalentes de línea de comandos por la misma razón por la que `-replace` es más seguro que `taint`: el efecto aparece en un plan que revisas, en lugar de aplicarse en silencio en el siguiente `apply`. `terraform plan -generate-config-out=generated.tf` incluso puede escribir por ti el bloque `resource` inicial a partir de un bloque `import`, lo cual es útil cuando todavía no conoces los atributos exactos del objeto.

## Práctica

En un directorio desechable, aplica dos recursos, luego usa `-replace` en uno y confirma con el plan que solo se vuelve a crear ese recurso. Después ejecuta `terraform plan -destroy` y lee la lista antes de decidir si continuar.

Añade un bloque `removed` para uno de los dos recursos y ejecuta `terraform plan`; confirma que el plan muestra que sale del state con `destroy = false` (el objeto real queda intacto). Después escribe un bloque `import` para ese mismo recurso, apuntando al archivo que sigue en disco, y confirma que `terraform plan` muestra que vuelve a quedar bajo gestión sin cambios.

## Referencias

- [Command: plan (`-replace`)](https://developer.hashicorp.com/terraform/cli/commands/plan#replace-address)
- [Command: state rm](https://developer.hashicorp.com/terraform/cli/commands/state/rm)
- [Import overview](https://developer.hashicorp.com/terraform/cli/import)
- [Command: destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy)
- [Command: taint (deprecated)](https://developer.hashicorp.com/terraform/cli/commands/taint)
- [Removing resources: the `removed` block](https://developer.hashicorp.com/terraform/language/state/remove)
- [Generating configuration with `import` blocks](https://developer.hashicorp.com/terraform/language/import/generating-configuration)
