# 3.5 Desfazendo Mudanças com Segurança

Não existe um único "desfazer" no Terraform. Você escolhe uma ação de recuperação com base no que deu errado.

## As principais opções

- **Reverter a configuração** no controle de versão e, então, `plan` e `apply`. Este é o desfazer normal e seguro.
- **`terraform apply -replace=<address>`** destrói e recria um resource que está quebrado, mas ainda rastreado.
- **`terraform state rm <address>`** diz ao Terraform para esquecer um resource sem excluí-lo. O objeto real permanece. Se você não reimportá-lo, o resource passa a não ser gerenciado e um `apply` posterior da mesma configuração tentará criar uma duplicata, o que pode falhar por conflito de nome. Use isso apenas imediatamente antes de reimportar.
- **`terraform import <address> <id>`** traz um objeto existente de volta para o gerenciamento.
- **`terraform destroy`** exclui todos os resources registrados no state para esta configuração. É destrutivo e irreversível; execute `terraform plan -destroy` primeiro e nunca o aponte para infraestrutura compartilhada.

Você pode encontrar material antigo mencionando o **`terraform taint`**: ele está descontinuado (deprecated). Use `apply -replace` no lugar dele — diferente do `taint`, o `-replace` mostra a substituição em um plano que você aprova, em vez de marcar silenciosamente um resource para substituição no próximo `apply` que rodar (possivelmente o de outra pessoa).

## Qual comando para qual situação

| Situação | Comando | Escopo | Caminho de recuperação |
| --- | --- | --- | --- |
| Uma mudança de configuração estava errada | Reverter a mudança no `.tf` no controle de versão, depois `plan`/`apply` | O que quer que o diff revertido afete | Reaplicar a configuração anterior novamente |
| Um resource está quebrado, mas o Terraform ainda o rastreia corretamente | `terraform apply -replace=<address>` | Uma instância de resource | Rodar `-replace` de novo, ou aceitar o novo objeto |
| O Terraform deve parar de gerenciar um resource, sem excluí-lo | `terraform state rm <address>` (ou um bloco `removed`, veja abaixo) | Um resource, removido apenas do state | `terraform import` para trazê-lo de volta com o mesmo endereço ou um novo |
| Um objeto existe na plataforma, mas não no state | `terraform import <address> <id>` | Um resource, adicionado apenas ao state | `terraform state rm` para removê-lo de novo |
| Todos os resources desta configuração precisam ser excluídos | `terraform destroy` | Tudo o que está neste state | Nenhum — os objetos são realmente excluídos; restaure a partir de um backup/snapshot se a plataforma oferecer um |

## Alternativas declarativas aos comandos de state

Dois tipos de bloco permitem expressar o `state rm` e o `import` como configuração revisável, em vez de comandos pontuais — útil quando a mudança deve passar pela mesma revisão de pull request que tudo o mais:

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

Ambos são mais seguros que seus equivalentes de linha de comando pelo mesmo motivo que o `-replace` é mais seguro que o `taint`: o efeito aparece em um plano que você revisa, em vez de acontecer silenciosamente no próximo `apply`. O `terraform plan -generate-config-out=generated.tf` pode até escrever o bloco `resource` inicial para você a partir de um bloco `import`, o que é útil quando você ainda não conhece os atributos exatos do objeto.

## Prática

Em um diretório descartável, aplique dois resources, depois use `-replace` em um e confirme pelo plano que apenas esse resource é recriado. Em seguida, execute `terraform plan -destroy` e leia a lista antes de decidir se prossegue.

Adicione um bloco `removed` para um dos dois resources e execute `terraform plan`; confirme que o plano mostra ele saindo do state com `destroy = false` (o objeto real permanece intacto). Depois escreva um bloco `import` para esse mesmo resource, apontando para o arquivo que ainda está no disco, e confirme que o `terraform plan` mostra ele voltando a ser gerenciado sem nenhuma mudança.

## Referências

- [Command: plan (`-replace`)](https://developer.hashicorp.com/terraform/cli/commands/plan#replace-address)
- [Command: state rm](https://developer.hashicorp.com/terraform/cli/commands/state/rm)
- [Import overview](https://developer.hashicorp.com/terraform/cli/import)
- [Command: destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy)
- [Command: taint (deprecated)](https://developer.hashicorp.com/terraform/cli/commands/taint)
- [Removing resources: the `removed` block](https://developer.hashicorp.com/terraform/language/state/remove)
- [Generating configuration with `import` blocks](https://developer.hashicorp.com/terraform/language/import/generating-configuration)
