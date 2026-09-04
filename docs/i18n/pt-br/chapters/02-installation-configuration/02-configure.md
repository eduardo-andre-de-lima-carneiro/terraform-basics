# 2.2 Configurar Providers e Credenciais

Um provider precisa de duas coisas: uma restrição de versão na sua configuração e credenciais no seu ambiente.

## Declare o provider

```hcl
terraform {
  required_version = ">= 1.6"

  required_providers {
    local = {
      source  = "hashicorp/local"
      version = "~> 2.5"
    }
  }
}

provider "local" {}
```

O `terraform init` lê esse bloco e baixa o provider fixado para o diretório de trabalho.

## Forneça credenciais com segurança

A maioria dos provedores de nuvem lê credenciais de variáveis de ambiente ou de um arquivo de credenciais compartilhado, nunca dos arquivos `.tf`. Mantenha os segredos fora da configuração e fora do controle de versão:

```bash
export EXAMPLE_API_TOKEN="REPLACE_ME"
```

## Prática

Crie um bloco `terraform` que exija o provider `local`, execute `terraform init` e confirme que um arquivo `.terraform.lock.hcl` aparece. Abra-o e observe que ele fixa a versão do provider e os checksums.

## Referências

- [Provider requirements](https://developer.hashicorp.com/terraform/language/providers/requirements)
- [Provider configuration](https://developer.hashicorp.com/terraform/language/providers/configuration)
- [Dependency lock file](https://developer.hashicorp.com/terraform/language/files/dependency-lock)
- [hashicorp/local provider docs](https://registry.terraform.io/providers/hashicorp/local/latest/docs)
- [Version constraints](https://developer.hashicorp.com/terraform/language/expressions/version-constraints)
