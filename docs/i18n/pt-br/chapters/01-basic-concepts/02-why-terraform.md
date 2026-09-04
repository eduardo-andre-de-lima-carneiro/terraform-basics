# 1.2 Por que Terraform

O Terraform é uma ferramenta amplamente usada para infraestrutura como código. Ele lê configuração declarativa, conversa com plataformas por meio de providers e mantém um arquivo de state para saber o que já gerencia.

## O que o torna útil

- Um único fluxo de trabalho (`init`, `plan`, `apply`) funciona para muitos providers.
- Um plano mostra as mudanças exatas antes que qualquer coisa aconteça.
- O state permite que o Terraform atualize e exclua apenas o que ele criou.
- Os módulos empacotam configuração para reutilização entre equipes e ambientes.

## Prática

Pense em duas plataformas que sua equipe usa, como um provedor de nuvem e um serviço de DNS. Com o Terraform, ambas são gerenciadas com os mesmos comandos e o mesmo formato de arquivo. Anote onde essa consistência economizaria tempo da sua equipe hoje.

## Referências

- [What is Terraform? (HashiCorp)](https://developer.hashicorp.com/terraform/intro)
- [Terraform use cases](https://developer.hashicorp.com/terraform/intro/use-cases)
- [Terraform Registry: providers](https://registry.terraform.io/browse/providers)
- [Terraform workflow overview](https://developer.hashicorp.com/terraform/intro/core-workflow)
