# 5.8 Exercícios Práticos

Complete estes exercícios em um diretório de trabalho temporário, usando seu editor para escrever e um terminal para verificar.

1. Dispare o autocompletar dentro de um bloco `resource` e aceite um argumento sugerido, depois confirme com `terraform validate`.
2. Exclua um argumento obrigatório e confirme que o editor mostra um diagnóstico inline antes de você executar qualquer comando.
3. Habilite a formatação ao salvar, desalinhe um bloco de propósito, salve e observe o `terraform fmt` corrigi-lo.
4. Configure o `terraform-ls` em um editor que não o inclui e confirme que o cliente reporta o servidor como conectado.
5. Adicione `terraform fmt -check -recursive` como um hook de pre-commit e confirme que um arquivo não formatado bloqueia o commit.
6. Use uma tarefa do editor para executar `terraform plan` e leia o resultado sem sair do editor.

Para cada exercício, registre a ação do editor executada e a saída do comando que confirmou o resultado.

## Referências

- [terraform-ls](https://github.com/hashicorp/terraform-ls)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [pre-commit-terraform hooks](https://github.com/antonbabenko/pre-commit-terraform)
