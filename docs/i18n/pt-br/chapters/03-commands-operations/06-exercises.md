# 3.6 Exercícios Práticos

Complete estes exercícios em um diretório temporário. Use o provider `null` para que nada real seja criado:

```hcl
terraform {
  required_providers {
    null = {
      source  = "hashicorp/null"
      version = "~> 3.2"
    }
  }
}

resource "null_resource" "example" {}
```

1. Execute `terraform init`, depois `terraform plan -out plan.tfplan` e `terraform apply plan.tfplan`.
2. Adicione um argumento `triggers` ao resource, salve um novo plano com `-out` e aplique exatamente esse arquivo de plano.
3. Mova o resource para um módulo filho `./modules/example`, adicione um bloco `moved` para a mudança de endereço e confirme que o `terraform plan` reporta nenhuma mudança.
4. Pratique a recuperação: execute `terraform apply -replace=null_resource.example`, depois `terraform state rm null_resource.example` seguido de `terraform import null_resource.example demo-id`.

Para cada exercício, registre o comando usado, o state antes dele e o resultado mostrado por `terraform plan` ou `terraform show`.

## Referências

- [hashicorp/null provider docs](https://registry.terraform.io/providers/hashicorp/null/latest/docs)
- [null_resource](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource)
- [Import usage](https://developer.hashicorp.com/terraform/cli/import/usage)
- [Get started tutorials](https://developer.hashicorp.com/terraform/tutorials)
