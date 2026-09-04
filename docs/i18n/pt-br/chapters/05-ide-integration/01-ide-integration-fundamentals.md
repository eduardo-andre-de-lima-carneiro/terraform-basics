# 5.1 Fundamentos de Integração com IDE

A integração de editores com o Terraform é, na maior parte, um único componente: o language server do Terraform, `terraform-ls`, mantido pela HashiCorp. Os editores conversam com ele por meio do Language Server Protocol.

## O que o language server oferece

- Realce de sintaxe e validação estrutural de HCL.
- Autocompletar para tipos de resource, argumentos e atributos dos providers instalados.
- Documentação ao passar o mouse e "ir para a definição" de módulos e variáveis.
- Formatação por meio do `terraform fmt`.
- Diagnósticos que exibem os erros do `terraform validate` diretamente no código.

## O que ainda roda em um terminal

O language server não executa `plan`, `apply` ou `destroy`. Esses continuam sendo comandos explícitos para que as mudanças de infraestrutura sejam sempre deliberadas. A maioria dos editores adiciona um botão ou tarefa que simplesmente chama a CLI do Terraform.

## Prática

Abra um arquivo `.tf` no seu editor e confirme que três coisas funcionam: autocompletar dentro de um bloco de resource, um erro inline ao excluir um argumento obrigatório e a formatação ao salvar. Se alguma falhar, as próximas seções mostram como habilitá-la.

## Referências

- [Terraform CLI documentation](https://developer.hashicorp.com/terraform/cli)
- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [Language Server Protocol specification](https://microsoft.github.io/language-server-protocol/)
