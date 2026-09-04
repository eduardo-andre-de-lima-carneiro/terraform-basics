# 3.2 State e Histórico de Mudanças

O state é como o Terraform lembra o que gerencia. Ele mapeia cada bloco de resource para um objeto real e armazena os últimos atributos conhecidos desse objeto.

## Inspecione o state

```bash
terraform state list
terraform show
terraform state show <resource address>
```

O `state list` nomeia cada resource rastreado, o `show` imprime todos os valores registrados e o `state show` foca em um único.

## Onde o histórico vive

O Terraform não mantém um log completo de mudanças próprio. O histórico revisável é o seu histórico de controle de versão dos arquivos `.tf`, mais o histórico de execuções da plataforma que os aplica. Faça commit das mudanças de configuração em passos pequenos e descritos para que o "porquê" seja recuperável.

## Prática

Aplique uma configuração pequena, execute `terraform state list`, depois altere um atributo e execute `terraform plan`. Observe como o plano explica a diferença entre o state registrado e o novo estado desejado.

## Referências

- [Manipulating state](https://developer.hashicorp.com/terraform/cli/state)
- [Command: state list](https://developer.hashicorp.com/terraform/cli/commands/state/list)
- [Command: show](https://developer.hashicorp.com/terraform/cli/commands/show)
- [Inspecting state](https://developer.hashicorp.com/terraform/cli/state/inspect)
