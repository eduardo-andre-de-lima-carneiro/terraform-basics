# 3.5 Desfazendo Mudanças com Segurança

Não existe um único "desfazer" no Terraform. Você escolhe uma ação de recuperação com base no que deu errado.

## As principais opções

- **Reverter a configuração** no controle de versão e, então, `plan` e `apply`. Este é o desfazer normal e seguro.
- **`terraform apply -replace=<address>`** destrói e recria um resource que está quebrado, mas ainda rastreado.
- **`terraform state rm <address>`** diz ao Terraform para esquecer um resource sem excluí-lo. O objeto real permanece. Se você não reimportá-lo, o resource passa a não ser gerenciado e um `apply` posterior da mesma configuração tentará criar uma duplicata, o que pode falhar por conflito de nome. Use isso apenas imediatamente antes de reimportar.
- **`terraform import <address> <id>`** traz um objeto existente de volta para o gerenciamento.
- **`terraform destroy`** exclui todos os resources registrados no state para esta configuração. É destrutivo e irreversível; execute `terraform plan -destroy` primeiro e nunca o aponte para infraestrutura compartilhada.

## Prática

Em um diretório descartável, aplique dois resources, depois use `-replace` em um e confirme pelo plano que apenas esse resource é recriado. Em seguida, execute `terraform plan -destroy` e leia a lista antes de decidir se prossegue.

## Referências

- [Command: plan (`-replace`)](https://developer.hashicorp.com/terraform/cli/commands/plan#replace-address)
- [Command: state rm](https://developer.hashicorp.com/terraform/cli/commands/state/rm)
- [Import overview](https://developer.hashicorp.com/terraform/cli/import)
- [Command: destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy)
