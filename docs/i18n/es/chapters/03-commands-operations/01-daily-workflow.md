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

## Referencias

- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [Command: validate](https://developer.hashicorp.com/terraform/cli/commands/validate)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Command: apply](https://developer.hashicorp.com/terraform/cli/commands/apply)
