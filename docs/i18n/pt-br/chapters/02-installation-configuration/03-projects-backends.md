# 2.3 Diretórios de trabalho e backends

Um diretório de trabalho do Terraform é qualquer pasta que contenha arquivos `.tf`. Quando você roda `terraform init`, o Terraform prepara essa pasta: baixa os providers e configura o backend.

## Backend local

Por padrão, o state é gravado em `terraform.tfstate` dentro do diretório de trabalho. Isso é suficiente para aprender e para experimentos individuais.

## Backend remoto

Um backend remoto guarda o state em um serviço compartilhado, para que uma equipe não sobrescreva o trabalho uma da outra e segredos não fiquem em um notebook:

```hcl
terraform {
  backend "s3" {
    bucket       = "example-tfstate"
    key          = "practice/terraform.tfstate"
    region       = "us-east-1"
    use_lockfile = true # locking nativo do S3; não precisa de tabela DynamoDB
  }
}
```

Substitua cada valor por placeholders que você controla. Mudar de backend exige rodar `terraform init` novamente, e o Terraform oferece migrar o state existente.

## Escolhendo um backend

O Terraform vem com backends para muitos destinos, não só o S3. Alguns comuns:

| Backend | Uso típico |
| --- | --- |
| `local` | Padrão; uma única máquina, aprendizado, experimentos individuais |
| `s3` | Equipes centradas em AWS; state em um bucket S3, com locking nativo do S3 (`use_lockfile`) |
| `azurerm` | Equipes centradas em Azure; state em um container de uma storage account |
| `gcs` | Equipes centradas em Google Cloud; state em um bucket do Cloud Storage |
| `remote` / o bloco `cloud` | HCP Terraform ou Terraform Enterprise; state, locking e histórico de runs totalmente gerenciados (veja o Capítulo 4) |

## Locking

O locking impede que duas execuções gravem o state ao mesmo tempo. A opção `use_lockfile = true` do backend S3 é a abordagem atualmente recomendada; o locking mais antigo baseado em tabela do DynamoDB está obsoleto. Seja qual for o backend escolhido, confirme que ele faz locking antes de confiar nele para uma equipe.

## Prática

Inicialize um diretório com o backend local padrão e localize o `terraform.tfstate`. Adicione-o ao `.gitignore` e anote por que fazer commit do state é inseguro: ele pode conter segredos e causa conflitos.

## Referências

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [Backend types](https://developer.hashicorp.com/terraform/language/backend#available-backends)
- [State: sensitive data](https://developer.hashicorp.com/terraform/language/state/sensitive-data)
- [S3 backend](https://developer.hashicorp.com/terraform/language/backend/s3)
- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
