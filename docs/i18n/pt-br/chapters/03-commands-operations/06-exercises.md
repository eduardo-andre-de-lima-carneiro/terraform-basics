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
5. Execute `terraform fmt -check -recursive` e `terraform plan -detailed-exitcode`; anote o código de saída com e sem uma mudança pendente (`echo $?` logo após cada comando).
6. Substitua o `state rm` do passo 4 por um bloco `removed` (`lifecycle { destroy = false }`) e confirme que o `terraform plan` mostra a mesma remoção para revisão antes de aplicá-la. Depois escreva um bloco `import` no lugar do comando `terraform import`, e execute `terraform plan -generate-config-out=generated.tf` para ver o Terraform redigir o bloco `resource` para você.
7. Crie um segundo diretório `modules-remote-demo` com seu próprio `null_resource` e um `output`. A partir do diretório original, adicione uma data source `terraform_remote_state` (`backend = "local"`, apontando para o `terraform.tfstate` do outro diretório) e referencie o output dela em um valor de `triggers`.

Para cada exercício, registre o comando usado, o state antes dele e o resultado mostrado por `terraform plan` ou `terraform show`.

## Referências

- [hashicorp/null provider docs](https://registry.terraform.io/providers/hashicorp/null/latest/docs)
- [null_resource](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource)
- [Import usage](https://developer.hashicorp.com/terraform/cli/import/usage)
- [Get started tutorials](https://developer.hashicorp.com/terraform/tutorials)
- [Generating configuration with `import` blocks](https://developer.hashicorp.com/terraform/language/import/generating-configuration)
- [`terraform_remote_state` data source](https://developer.hashicorp.com/terraform/language/state/remote-state-data)
