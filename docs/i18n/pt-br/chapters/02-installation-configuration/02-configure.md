# 2.2 Configurar providers e credenciais

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

A maioria dos providers de nuvem lê credenciais de variáveis de ambiente ou de um arquivo de credenciais compartilhado, nunca dos arquivos `.tf`. Mantenha segredos fora da configuração e fora do controle de versão:

```bash
export EXAMPLE_API_TOKEN="REPLACE_ME"
```

## Variáveis de credenciais para providers comuns

Cada provider segue o método de autenticação padrão da própria plataforma em vez de um específico do Terraform, então as mesmas variáveis também funcionam com a CLI ou o SDK daquela plataforma:

| Provider | Variáveis de ambiente típicas | Alternativa |
| --- | --- | --- |
| `hashicorp/aws` | `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN` | Arquivo de credenciais compartilhado (`~/.aws/credentials`) mais `AWS_PROFILE` |
| `hashicorp/azurerm` | `ARM_CLIENT_ID`, `ARM_CLIENT_SECRET`, `ARM_SUBSCRIPTION_ID`, `ARM_TENANT_ID` | `az login` interativo, que o Terraform pode usar diretamente |
| `hashicorp/google` | `GOOGLE_CREDENTIALS` (JSON inline) ou `GOOGLE_APPLICATION_CREDENTIALS` (caminho para um arquivo de chave) | Application Default Credentials via `gcloud auth application-default login` |

Prefira uma identidade de curta duração (uma role assumida, uma workload identity, ou um login interativo via CLI) a uma chave estática de longa duração sempre que a plataforma oferecer essa opção.

## Prática

Crie um bloco `terraform` que exija o provider `local`, rode `terraform init` e confirme que um arquivo `.terraform.lock.hcl` aparece. Abra-o e note que ele fixa a versão do provider e os checksums.

## Referências

- [Provider requirements](https://developer.hashicorp.com/terraform/language/providers/requirements)
- [Provider configuration](https://developer.hashicorp.com/terraform/language/providers/configuration)
- [Dependency lock file](https://developer.hashicorp.com/terraform/language/files/dependency-lock)
- [hashicorp/local provider docs](https://registry.terraform.io/providers/hashicorp/local/latest/docs)
- [Version constraints](https://developer.hashicorp.com/terraform/language/expressions/version-constraints)
- [AWS CLI environment variables (same variables the AWS provider reads)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)
- [Authenticate Terraform to Azure with a service principal](https://learn.microsoft.com/en-us/azure/developer/terraform/authenticate-to-azure-with-service-principle)
- [Google: Application Default Credentials](https://cloud.google.com/docs/authentication/application-default-credentials)
