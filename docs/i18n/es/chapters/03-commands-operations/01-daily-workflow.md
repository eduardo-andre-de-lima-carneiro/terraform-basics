# 3.1 El flujo de trabajo diario

Ejecuta `terraform init` una vez en un directorio de trabajo nuevo (o después de cambiar los providers o el backend), y luego usa un bucle pequeño y observable:

```bash
terraform init      # first time in the directory, or after provider/backend changes
terraform fmt
terraform validate
terraform plan -out plan.tfplan
terraform apply plan.tfplan
```

`fmt` normaliza el estilo, `validate` comprueba que la configuración está bien formada y `plan -out` guarda el cambio exacto para que `apply` ejecute solo lo que revisaste. Leer el plan antes de aplicarlo es el hábito que evita cambios inesperados.

Por defecto, `plan` también refresca: lee el estado actual de cada objeto ya rastreado desde la plataforma real antes de compararlo con tu configuración, y así es como el drift aparece en el resultado del plan. Omite esa lectura con `terraform plan -refresh=false` cuando quieras comparar deliberadamente contra el último state registrado en lugar de la plataforma real (por ejemplo, para ver solo el efecto de un cambio de configuración en un state enorme, sin pagar el costo de un refresh completo).

## Automatizar el flujo de trabajo

El mismo bucle se ejecuta sin supervisión en CI con algunos ajustes:

```bash
terraform fmt -check -recursive   # non-zero exit if any file is not formatted; writes nothing
terraform validate
terraform plan -out plan.tfplan -detailed-exitcode
```

`fmt -check -recursive` falla la build en lugar de reescribir archivos en silencio, y también recorre los subdirectorios. `plan -detailed-exitcode` devuelve `0` cuando no hay cambios, `1` si hay un error y `2` cuando el plan contiene cambios; un pipeline puede usar el código de salida `2` para exigir una aprobación manual antes de ejecutar `terraform apply plan.tfplan`.

## Por qué importa revisar el plan

`apply` sin un archivo de plan guardado vuelve a ejecutar `plan` y pide aprobación, pero ese plan se genera justo antes de aplicarse: cualquiera puede escribir "yes" sin leerlo. Aplicar un plan *guardado* (`terraform apply plan.tfplan`) elimina esa tentación: las acciones exactas ya quedaron fijadas en el momento de `plan`, así que la revisión ocurre sobre un artefacto estable en lugar de un prompt en vivo, y ese mismo archivo puede adjuntarse a un pull request o a una ejecución de CI como evidencia de lo que va a pasar.

## Referencias

- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [Command: validate](https://developer.hashicorp.com/terraform/cli/commands/validate)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Command: apply](https://developer.hashicorp.com/terraform/cli/commands/apply)
