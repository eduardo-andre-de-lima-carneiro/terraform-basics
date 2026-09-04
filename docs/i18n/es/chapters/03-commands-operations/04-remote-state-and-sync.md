# 3.4 State remoto y sincronización

Cuando más de una persona o pipeline ejecuta Terraform, el state debe vivir en un backend compartido y estar protegido frente a escrituras simultáneas.

## Configurar y migrar

```bash
terraform init -migrate-state
```

Después de añadir un bloque `backend`, este comando mueve el state local existente al backend remoto y pide confirmación antes de sobrescribir nada.

## Bloqueo y refresh

- Los backends compatibles toman un bloqueo durante `plan` y `apply` para que dos ejecuciones no puedan corromper el state.
- `terraform plan` refresca el state desde la plataforma real por defecto, revelando la desviación (drift).
- Nunca edites el archivo de state a mano; usa los subcomandos `terraform state`.

## Práctica

Describe, en dos frases, qué podría salir mal si dos personas de ingeniería ejecutaran `terraform apply` al mismo tiempo contra un archivo de state local. Después explica cómo lo evita un backend remoto con bloqueo.

## Referencias

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [State locking](https://developer.hashicorp.com/terraform/language/state/locking)
- [Command: init (backend initialization)](https://developer.hashicorp.com/terraform/cli/commands/init#backend-initialization)
- [Managing state in automation](https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform)
