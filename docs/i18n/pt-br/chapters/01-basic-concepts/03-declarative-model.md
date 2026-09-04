# 1.3 O modelo declarativo do Terraform

Você descreve o resultado que quer, não os passos para chegar até ele. O Terraform compara sua configuração com o state atual e com a plataforma real, e então decide quais ações são necessárias.

## Declarativo versus imperativo

Um script imperativo diz "crie isto, depois conecte aquilo". Uma configuração declarativa diz "estes resources devem existir com estas configurações". Se um resource já corresponde, o Terraform não faz nada. Se ele divergiu, o Terraform propõe uma correção.

```bash
# Imperativo: você especifica cada passo, em ordem, e reexecutar às cegas pode falhar ou duplicar trabalho
aws ec2 create-vpc ...
aws ec2 create-subnet ...
aws ec2 create-security-group ...
```

```hcl
# Declarativo: você especifica o estado final; o Terraform calcula os passos e a ordem
resource "aws_vpc" "main" { cidr_block = "10.0.0.0/16" }
resource "aws_subnet" "app" { vpc_id = aws_vpc.main.id, cidr_block = "10.0.1.0/24" }
```

O bloco declarativo nunca diz "crie a VPC primeiro". O Terraform deduz isso porque a configuração da subnet referencia `aws_vpc.main.id`.

## Os providers fazem o trabalho

O Terraform em si não conhece nenhuma API de nuvem. Cada provider traduz os blocos de resource em chamadas de API e reporta os resultados de volta para o state.

## Como o Terraform decide a ordem

Cada referência entre resources — `aws_vpc.main.id` no exemplo acima — vira uma aresta em um grafo de dependências. O Terraform constrói esse grafo a partir da sua configuração, verifica que não há ciclos, e o percorre de forma que um resource só é criado, atualizado ou destruído depois que tudo do que ele depende já terminou. Resources que não dependem uns dos outros podem rodar ao mesmo tempo, até 10 em paralelo por padrão, e é por isso que um modelo declarativo tende a aplicar mais rápido do que um script imperativo equivalente à medida que sua configuração cresce. Você pode inspecionar o grafo você mesmo com `terraform graph`, e forçar uma aresta explícita com `depends_on` quando existe uma dependência que não aparece como referência de atributo.

Destruir resources percorre um grafo relacionado, mas separado, porque a ordem segura para remover coisas costuma ser o inverso da ordem usada para criá-las.

## Onde o modelo declarativo tensiona

O modelo não é livre de trade-offs:

- **Ainda existem passos verdadeiramente imperativos.** Uma migração de banco de dados, uma importação de dados única, ou uma chamada de API sensível à ordem não se encaixam bem em "este resource deve existir". A orientação da HashiCorp sobre provisioners — a válvula de escape para rodar um script durante a criação ou destruição — é tratá-los como último recurso: primeiro verifique se existe uma forma nativa do provider de fazer a mesma coisa, porque o Terraform não consegue modelar o que um provisioner realmente faz nem saber se ele teve sucesso da forma como rastreia um resource.
- **A plataforma real pode discordar do grafo.** APIs de nuvem às vezes têm consistência eventual, então um resource pode reportar sucesso antes de estar totalmente utilizável em outro lugar, o que ocasionalmente aparece como um erro de dependência mesmo que a configuração estivesse correta.
- **Declarativo não significa automático.** Você ainda escolhe os resources, os argumentos e os limites dos módulos; o Terraform só automatiza a ordenação e a comparação de diferenças, não o design.

## Prática

Leia um bloco de resource curto e descreva, em linguagem simples, o estado final que ele declara. Depois preveja o que o Terraform faria se esse resource já existisse sem alterações. Por fim, rode `terraform graph` em uma configuração pequena com dois resources relacionados e identifique a aresta que corresponde à referência entre eles.

## Referências

- [How Terraform works](https://developer.hashicorp.com/terraform/intro#how-does-terraform-work)
- [Providers (Terraform language)](https://developer.hashicorp.com/terraform/language/providers)
- [Resources overview](https://developer.hashicorp.com/terraform/language/resources)
- [Resource behavior and the dependency graph](https://developer.hashicorp.com/terraform/language/resources/behavior)
- [The dependency graph (internals)](https://developer.hashicorp.com/terraform/internals/graph)
- [Command: graph](https://developer.hashicorp.com/terraform/cli/commands/graph)
- [The `depends_on` meta-argument](https://developer.hashicorp.com/terraform/language/meta-arguments/depends_on)
- [Provisioners: a last resort](https://developer.hashicorp.com/terraform/language/resources/provisioners/syntax)
