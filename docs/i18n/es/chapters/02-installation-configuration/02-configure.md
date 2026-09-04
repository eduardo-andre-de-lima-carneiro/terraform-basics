# 2.2 Configurar providers y credenciales

Un provider necesita dos cosas: una restricción de versión en tu configuración y credenciales en tu entorno.

## Declara el provider

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

`terraform init` lee este bloque y descarga el provider fijado en el directorio de trabajo.

## Proporciona credenciales de forma segura

La mayoría de los providers de nube leen credenciales desde variables de entorno o un archivo de credenciales compartido, nunca desde los archivos `.tf`. Mantén los secretos fuera de la configuración y fuera del control de versiones:

```bash
export EXAMPLE_API_TOKEN="REPLACE_ME"
```

## Variables de credenciales para providers comunes

Cada provider sigue el método de autenticación estándar de su propia plataforma en lugar de uno específico de Terraform, así que las mismas variables también funcionan con la CLI o el SDK de esa plataforma:

| Provider | Variables de entorno típicas | Alternativa |
| --- | --- | --- |
| `hashicorp/aws` | `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN` | Archivo de credenciales compartido (`~/.aws/credentials`) más `AWS_PROFILE` |
| `hashicorp/azurerm` | `ARM_CLIENT_ID`, `ARM_CLIENT_SECRET`, `ARM_SUBSCRIPTION_ID`, `ARM_TENANT_ID` | `az login` interactivo, que Terraform puede usar directamente |
| `hashicorp/google` | `GOOGLE_CREDENTIALS` (JSON en línea) o `GOOGLE_APPLICATION_CREDENTIALS` (ruta a un archivo de clave) | Application Default Credentials desde `gcloud auth application-default login` |

Prefiere una identidad de corta duración (un rol asumido, una workload identity o un login interactivo por CLI) sobre una clave estática de larga duración siempre que la plataforma lo ofrezca.

## Práctica

Crea un bloque `terraform` que requiera el provider `local`, ejecuta `terraform init` y confirma que aparece un archivo `.terraform.lock.hcl`. Ábrelo y observa que fija la versión del provider y los checksums.

## Referencias

- [Provider requirements](https://developer.hashicorp.com/terraform/language/providers/requirements)
- [Provider configuration](https://developer.hashicorp.com/terraform/language/providers/configuration)
- [Dependency lock file](https://developer.hashicorp.com/terraform/language/files/dependency-lock)
- [hashicorp/local provider docs](https://registry.terraform.io/providers/hashicorp/local/latest/docs)
- [Version constraints](https://developer.hashicorp.com/terraform/language/expressions/version-constraints)
- [AWS CLI environment variables (same variables the AWS provider reads)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)
- [Authenticate Terraform to Azure with a service principal](https://learn.microsoft.com/en-us/azure/developer/terraform/authenticate-to-azure-with-service-principle)
- [Google: Application Default Credentials](https://cloud.google.com/docs/authentication/application-default-credentials)
