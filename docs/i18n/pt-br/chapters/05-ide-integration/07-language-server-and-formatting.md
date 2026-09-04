# 5.7 Configuração do Language Server e da Formatação

Duas configurações fazem a maior parte do trabalho para uma experiência de edição consistente com Terraform: o language server e a formatação automática.

## Instale o language server

Baixe o `terraform-ls` dos releases oficiais ou instale-o com um gerenciador de pacotes, e garanta que ele esteja no `PATH`. A maioria das extensões de editor já o inclui, mas uma cópia no sistema é útil para editores que não incluem.

## Imponha a formatação

O `terraform fmt` é o formatador canônico. Ligue-o ao editor como formatação ao salvar e também imponha-o fora do editor:

```bash
terraform fmt -check -recursive
```

Execute isso na CI para que um arquivo não formatado falhe o pipeline em vez de causar diffs ruidosos.

## Validação sob demanda

Associe uma tarefa ou tecla ao `terraform validate` para que erros estruturais apareçam sem um `plan` completo. Lembre-se de que o `validate` não contata as APIs dos providers; ele apenas verifica se a configuração é internamente consistente.

## Prática

Adicione `terraform fmt -check -recursive` à CI do seu projeto ou a um hook de pre-commit, depois faça commit de um arquivo desalinhado de propósito e confirme que a verificação falha.

## Referências

- [terraform-ls releases](https://releases.hashicorp.com/terraform-ls/)
- [terraform-ls settings](https://github.com/hashicorp/terraform-ls/blob/main/docs/SETTINGS.md)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [Command: validate](https://developer.hashicorp.com/terraform/cli/commands/validate)
