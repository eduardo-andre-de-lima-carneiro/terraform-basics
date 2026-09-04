# 1.4 O Modelo Mental do Terraform

Pense em três lugares:

1. Configuração: os arquivos `.tf` que descrevem o estado desejado.
2. State: o que o Terraform registrou sobre os resources que gerencia.
3. Infraestrutura real: o que de fato existe na plataforma.

O fluxo básico é `write config -> terraform plan -> terraform apply`. O `terraform plan` compara os três lugares e mostra a diferença; ele deve ser o seu comando de diagnóstico mais frequente. Execute-o antes de cada apply.

## Referências

- [State (Terraform language)](https://developer.hashicorp.com/terraform/language/state)
- [Purpose of Terraform state](https://developer.hashicorp.com/terraform/language/state/purpose)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Core workflow](https://developer.hashicorp.com/terraform/intro/core-workflow)
