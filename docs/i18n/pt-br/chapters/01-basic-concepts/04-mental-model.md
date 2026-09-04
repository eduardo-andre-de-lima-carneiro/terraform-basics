# 1.4 O modelo mental do Terraform

Pense em três lugares:

1. Configuração: os arquivos `.tf` que descrevem o estado desejado.
2. State: o que o Terraform registrou sobre os resources que ele gerencia.
3. Infraestrutura real: o que realmente existe na plataforma.

O fluxo básico é `escrever configuração -> terraform plan -> terraform apply`. O `terraform plan` compara os três lugares e mostra a diferença; ele deveria ser o seu comando de diagnóstico mais frequente. Rode-o antes de cada apply.

## Por que o Terraform precisa de um terceiro lugar

Seria mais simples se o Terraform apenas comparasse a configuração com a infraestrutura real, mas dois problemas impedem isso. Primeiro, algumas informações não podem ser recuperadas só a partir da plataforma — quando você remove um resource da configuração, o Terraform precisa saber como removê-lo e em que ordem, e uma API de nuvem não expõe essa relação. Segundo, consultar todos os atributos de cada resource a cada comando não escala: para um ambiente grande, isso significaria chamadas constantes de API, latência e limites de taxa. O state resolve os dois problemas: é o cache e o mapa que conecta um bloco de resource como `aws_instance.web` a um objeto real, como a instância `i-abcd1234`, sem o qual, como diz a HashiCorp, "Terraform is unable to function" (o Terraform não consegue funcionar).

## Percorrendo o ciclo com um exemplo prático

```bash
terraform apply    # a configuração agora corresponde ao state, que corresponde à infraestrutura real
# ...você edita main.tf, mudando um argumento...
terraform plan      # compara os três lugares; mostra exatamente um atributo mudando
terraform apply     # a infraestrutura real e o state se atualizam para a nova configuração
```

Se, em vez disso, você mudar algo manualmente no console do provider, só o state e a infraestrutura real ficam fora de sincronia; o próximo `terraform plan` atualiza o state a partir da plataforma real por padrão e reporta a divergência, normalmente como uma proposta de voltar a deixar o resource igual à configuração.

## O que vive onde

| Pergunta | A resposta vive em | Como verificar |
| --- | --- | --- |
| O que eu quero que exista? | Configuração | Ler os arquivos `.tf` |
| O que o Terraform acredita que existe? | State | `terraform show`, `terraform state list` |
| O que realmente existe na plataforma? | Infraestrutura real | O console ou a API do provider, ou um `terraform plan` recente |
| O que vai mudar se eu aplicar agora? | A diferença entre os três | `terraform plan` |

## Armadilhas comuns

- **Tratar o state como descartável.** Apagar ou editar manualmente o arquivo de state não apaga os resources reais; só faz o Terraform esquecê-los, o que normalmente leva a applies com falha ou resources duplicados.
- **Aplicar sem um plano atualizado.** Pular o `plan`, ou aplicar um plano que não está mais atual, significa confiar em uma comparação desatualizada dos três lugares.
- **Assumir que o state é opcional quando se trabalha sozinho.** Mesmo sozinho, um apply interrompido pode deixar configuração, state e realidade fora de sincronia; as técnicas de recuperação do Capítulo 3 existem exatamente para isso.
- **Esquecer que a "infraestrutura real" pode mudar sem o Terraform.** Qualquer coisa que outro engenheiro, outro script ou uma sessão no console mude não vai aparecer até o próximo `plan` atualizar o state.

## Prática

Aplique uma configuração pequena e depois altere manualmente esse mesmo resource fora do Terraform (por exemplo, edite diretamente o conteúdo de um arquivo local em vez de fazer isso pela configuração). Rode `terraform plan` e identifique qual dos três lugares mudou, e quais dois permaneceram de acordo.

## Referências

- [State (Terraform language)](https://developer.hashicorp.com/terraform/language/state)
- [Purpose of Terraform state](https://developer.hashicorp.com/terraform/language/state/purpose)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Command: refresh](https://developer.hashicorp.com/terraform/cli/commands/refresh)
- [Core workflow](https://developer.hashicorp.com/terraform/intro/core-workflow)
