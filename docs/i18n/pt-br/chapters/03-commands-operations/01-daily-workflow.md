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

Por padrão, o `plan` também faz refresh: ele lê o estado atual de cada objeto já rastreado a partir da plataforma real antes de compará-lo com sua configuração, e é assim que o drift aparece no resultado do plano. Pule essa leitura com `terraform plan -refresh=false` quando você quiser deliberadamente comparar com o último state registrado em vez da plataforma real (por exemplo, para ver apenas o efeito de uma mudança de configuração em um state enorme, sem pagar o custo de um refresh completo).

## Automatizando o fluxo de trabalho

O mesmo laço roda sem supervisão em CI, com alguns ajustes:

```bash
terraform fmt -check -recursive   # non-zero exit if any file is not formatted; writes nothing
terraform validate
terraform plan -out plan.tfplan -detailed-exitcode
```

O `fmt -check -recursive` falha o build em vez de reescrever arquivos silenciosamente, e também percorre os subdiretórios. O `plan -detailed-exitcode` retorna `0` quando não há mudanças, `1` em caso de erro e `2` quando o plano contém mudanças — um pipeline pode usar o código de saída `2` para exigir uma etapa de aprovação manual antes que o `terraform apply plan.tfplan` seja executado.

## Por que revisar o plano importa

O `apply` sem um arquivo de plano salvo executa o `plan` novamente e pede aprovação, mas esse plano é gerado momentos antes de ser aplicado — qualquer pessoa pode digitar "yes" sem lê-lo. Aplicar um plano *salvo* (`terraform apply plan.tfplan`) remove essa tentação: as ações exatas já foram fixadas no momento do `plan`, então a revisão acontece sobre um artefato estável em vez de um prompt ao vivo, e esse mesmo arquivo pode ser anexado a um pull request ou a uma execução de CI como evidência do que vai acontecer.

## Referências

- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [Command: validate](https://developer.hashicorp.com/terraform/cli/commands/validate)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Command: apply](https://developer.hashicorp.com/terraform/cli/commands/apply)
