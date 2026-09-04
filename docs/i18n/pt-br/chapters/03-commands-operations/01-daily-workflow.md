# 3.1 O Fluxo de Trabalho Diário

Execute `terraform init` uma vez em um novo diretório de trabalho (ou depois de mudar de providers ou de backend) e, então, use um laço pequeno e observável:

```bash
terraform init      # first time in the directory, or after provider/backend changes
terraform fmt
terraform validate
terraform plan -out plan.tfplan
terraform apply plan.tfplan
```

O `fmt` normaliza o estilo, o `validate` verifica se a configuração está bem formada e o `plan -out` salva a mudança exata para que o `apply` execute apenas o que você revisou. Ler o plano antes de aplicar é o hábito que evita mudanças surpresa.

## Referências

- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [Command: validate](https://developer.hashicorp.com/terraform/cli/commands/validate)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Command: apply](https://developer.hashicorp.com/terraform/cli/commands/apply)
