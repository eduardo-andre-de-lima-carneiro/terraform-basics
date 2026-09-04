# 4.1 Fundamentos de Integração

Executar o Terraform em uma plataforma adiciona serviços de colaboração e entrega em torno de uma configuração. Os comandos continuam familiares; a plataforma fornece identidade, permissões, revisão de plano, automação e visibilidade de mudanças.

## O fluxo comum

1. Armazene a configuração em um repositório de controle de versão.
2. Configure um backend remoto ou um workspace da plataforma para o state.
3. Em um pull request, execute `terraform plan` automaticamente e publique o resultado.
4. Exija a revisão tanto do diff do código quanto do plano.
5. Execute `terraform apply` somente após o merge, atrás de uma aprovação ou ambiente protegido.
6. Mantenha as credenciais dos providers no gerenciador de segredos da plataforma.

## Escolha onde a execução acontece

As execuções podem rodar em um job de CI genérico ou em uma plataforma dedicada ao Terraform. Um job genérico é flexível; uma plataforma dedicada adiciona armazenamento de state, locking, histórico de execuções e verificações de política sem scripting extra.

## O que configurar

No mínimo, entre em acordo sobre a branch padrão, a proteção de branch, o plan em PR, o apply no merge, quem pode aprovar um apply, onde o state vive e como os segredos são injetados. Essas políticas fazem parte do processo de entrega, não são um enfeite opcional.

## Referências

- [Running Terraform in automation](https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform)
- [Terraform automation tutorials](https://developer.hashicorp.com/terraform/tutorials/automation)
- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [Terraform recommended practices](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices)
