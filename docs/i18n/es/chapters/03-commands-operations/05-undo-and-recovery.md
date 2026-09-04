# 3.5 Deshacer cambios con seguridad

No existe un único "deshacer" en Terraform. Eliges una acción de recuperación según lo que haya salido mal.

## Las opciones principales

- **Revertir la configuración** en el control de versiones, y luego `plan` y `apply`. Este es el deshacer normal y seguro.
- **`terraform apply -replace=<address>`** destruye y vuelve a crear un recurso que está roto pero sigue rastreado.
- **`terraform state rm <address>`** le dice a Terraform que olvide un recurso sin eliminarlo. El objeto real permanece. Si no lo vuelves a importar, el recurso queda sin gestionar y un `apply` posterior de la misma configuración intentará crear un duplicado, lo que puede fallar por un conflicto de nombre. Úsalo solo justo antes de volver a importarlo.
- **`terraform import <address> <id>`** vuelve a poner bajo gestión un objeto existente.
- **`terraform destroy`** elimina todos los recursos registrados en el state para esta configuración. Es destructivo e irreversible; ejecuta `terraform plan -destroy` primero y nunca lo apuntes a infraestructura compartida.

## Práctica

En un directorio desechable, aplica dos recursos, luego usa `-replace` en uno y confirma con el plan que solo se vuelve a crear ese recurso. Después ejecuta `terraform plan -destroy` y lee la lista antes de decidir si continuar.

## Referencias

- [Command: plan (`-replace`)](https://developer.hashicorp.com/terraform/cli/commands/plan#replace-address)
- [Command: state rm](https://developer.hashicorp.com/terraform/cli/commands/state/rm)
- [Import overview](https://developer.hashicorp.com/terraform/cli/import)
- [Command: destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy)
