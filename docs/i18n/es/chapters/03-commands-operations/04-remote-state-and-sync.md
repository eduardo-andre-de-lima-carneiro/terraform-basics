# 3.4 State remoto y sincronización

Cuando más de una persona o pipeline ejecuta Terraform, el state debe vivir en un backend compartido y estar protegido frente a escrituras simultáneas.

## Configurar y migrar

```bash
terraform init -migrate-state
```

Después de añadir un bloque `backend`, este comando mueve el state local existente al backend remoto y pide confirmación antes de sobrescribir nada. Volver a ejecutar `terraform init` después de cambiar la configuración de un backend *ya existente* (no al añadir uno) exige `-migrate-state` o `-reconfigure`: Terraform no elige uno por ti en silencio.

| Flag | Qué hace |
| --- | --- |
| `-migrate-state` | Copia el state actual a la nueva configuración de backend |
| `-reconfigure` | Cambia a la nueva configuración de backend sin migrar el state (empieza limpio; el state antiguo se queda donde estaba) |
| `-force-copy` | Igual que `-migrate-state`, pero responde "sí" automáticamente a cada prompt de migración; para un `init` guionado/no interactivo |

## Bloqueo y refresh

- Los backends compatibles toman un bloqueo durante `plan` y `apply` para que dos ejecuciones no puedan corromper el state; no todos los backends admiten bloqueo, así que comprueba la documentación del backend concreto antes de confiar en ello para un equipo.
- `terraform plan` refresca el state desde la plataforma real por defecto, revelando la desviación (drift).
- Nunca edites el archivo de state a mano; usa los subcomandos `terraform state`.

**`terraform force-unlock <lock-id>`** elimina un bloqueo atascado sin tocar ninguna infraestructura. Alcance: solo afecta al bloqueo del state de esta configuración, no a los propios recursos. Úsalo solo después de confirmar que el proceso que tomó el bloqueo realmente se detuvo (un job de CI que se cayó, una ejecución local que mataste); Terraform imprime el lock ID y quién lo posee cuando un bloqueo está disputado. Camino de recuperación: si liberas el bloqueo mientras la ejecución de otra persona sigue realmente en curso, ambas pueden escribir el state a la vez y corromperlo; si eso pasa, restaura el último state bueno desde el propio versionado del backend (por ejemplo, el versionado de un bucket de S3) o desde una salida de `terraform state pull` guardada de antemano.

## Leer las salidas de otra configuración

Dos configuraciones de Terraform suelen necesitar compartir información —el ID de una subred de un stack de red, por ejemplo, consumido por un stack de aplicación— sin fusionarse en un único módulo raíz. La fuente de datos `terraform_remote_state` lee el state de otra configuración directamente desde su backend:

```hcl
data "terraform_remote_state" "network" {
  backend = "local"
  config = {
    path = "../network/terraform.tfstate"
  }
}

resource "local_file" "app_config" {
  filename = "app.conf"
  content  = "subnet=${data.terraform_remote_state.network.outputs.subnet_id}"
}
```

Sustituye `backend = "local"` y su `path` por el backend y el mapa `config` que realmente use la otra configuración (por ejemplo, `backend = "s3"` con `bucket`/`key`/`region`). Ten en cuenta dos cosas:

- Solo se pueden leer los valores `output` del módulo raíz de la otra configuración; nada de lo anidado en sus módulos hijos, a menos que ese módulo reexponga sus salidas en la raíz.
- Cualquiera que pueda leer estas salidas puede acceder igual a la instantánea completa del state, así que esto solo acota *qué referencias*, no quién puede ver el state subyacente; mantén los secretos fuera de los outputs igual que los mantienes fuera de todo lo demás en el state.

## Práctica

Describe, en dos frases, qué podría salir mal si dos personas de ingeniería ejecutaran `terraform apply` al mismo tiempo contra un archivo de state local. Después explica cómo lo evita un backend remoto con bloqueo.

Crea dos directorios locales, `network` y `app`, cada uno con su propia configuración del provider `local`; dale a `network` un valor `output`, luego léelo desde `app` con `terraform_remote_state` y confirma que `terraform apply` en `app` recoge ese valor.

## Referencias

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [State locking](https://developer.hashicorp.com/terraform/language/state/locking)
- [Command: init (backend initialization)](https://developer.hashicorp.com/terraform/cli/commands/init#backend-initialization)
- [Managing state in automation](https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform)
- [Command: force-unlock](https://developer.hashicorp.com/terraform/cli/commands/force-unlock)
- [`terraform_remote_state` data source](https://developer.hashicorp.com/terraform/language/state/remote-state-data)
