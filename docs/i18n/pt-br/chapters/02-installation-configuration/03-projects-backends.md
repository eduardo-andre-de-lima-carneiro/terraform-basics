# 2.3 Diretórios de Trabalho e Backends

Um diretório de trabalho do Terraform é qualquer pasta que contém arquivos `.tf`. Quando você executa `terraform init`, o Terraform prepara essa pasta: baixa os providers e configura o backend.

## Backend local

Por padrão, o state é gravado em `terraform.tfstate` no diretório de trabalho. Isso é adequado para aprender e para experimentos individuais.

## Backend remoto

Um backend remoto armazena o state em um serviço compartilhado para que uma equipe não sobrescreva o trabalho um do outro e para que os segredos não fiquem em um laptop:

```hcl
terraform {
  backend "s3" {
    bucket = "example-tfstate"
    key    = "practice/terraform.tfstate"
    region = "us-east-1"
  }
}
```

Substitua todos os valores por placeholders que você controla. Trocar de backend exige executar `terraform init` novamente, e o Terraform oferece migrar o state existente.

## Prática

Inicialize um diretório com o backend local padrão e localize `terraform.tfstate`. Adicione-o ao `.gitignore` e anote por que fazer commit do state não é seguro: ele pode conter segredos e causa conflitos.

## Referências

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [State: sensitive data](https://developer.hashicorp.com/terraform/language/state/sensitive-data)
- [S3 backend](https://developer.hashicorp.com/terraform/language/backend/s3)
- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
