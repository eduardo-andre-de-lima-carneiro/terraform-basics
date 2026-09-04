# 1.3 O Modelo Declarativo do Terraform

Você descreve o resultado que quer, não os passos para chegar até ele. O Terraform compara sua configuração com o state atual e a plataforma real e, então, decide quais ações são necessárias.

## Declarativo versus imperativo

Um script imperativo diz "crie isto e depois anexe aquilo". Uma configuração declarativa diz "estes resources devem existir com estas definições". Se um resource já corresponde, o Terraform não faz nada. Se ele sofreu drift, o Terraform propõe uma correção.

## Os providers fazem o trabalho

O Terraform em si não conhece nenhuma API de nuvem. Cada provider traduz os blocos de resource em chamadas de API e reporta os resultados de volta ao state.

## Prática

Leia um bloco de resource curto e descreva, em linguagem simples, o estado final que ele declara. Depois, preveja o que o Terraform faria se esse resource já existisse sem alterações.

## Referências

- [How Terraform works](https://developer.hashicorp.com/terraform/intro#how-does-terraform-work)
- [Providers (Terraform language)](https://developer.hashicorp.com/terraform/language/providers)
- [Resources overview](https://developer.hashicorp.com/terraform/language/resources)
- [Resource behavior and the dependency graph](https://developer.hashicorp.com/terraform/language/resources/behavior)
