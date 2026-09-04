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

## Proporciona las credenciales con seguridad

La mayoría de los proveedores de nube leen las credenciales desde variables de entorno o desde un archivo de credenciales compartido, nunca desde los archivos `.tf`. Mantén los secretos fuera de la configuración y fuera del control de versiones:

```bash
export EXAMPLE_API_TOKEN="REPLACE_ME"
```

## Práctica

Crea un bloque `terraform` que requiera el provider `local`, ejecuta `terraform init` y confirma que aparece un archivo `.terraform.lock.hcl`. Ábrelo y observa que fija la versión del provider y sus checksums.

## Referencias

- [Provider requirements](https://developer.hashicorp.com/terraform/language/providers/requirements)
- [Provider configuration](https://developer.hashicorp.com/terraform/language/providers/configuration)
- [Dependency lock file](https://developer.hashicorp.com/terraform/language/files/dependency-lock)
- [hashicorp/local provider docs](https://registry.terraform.io/providers/hashicorp/local/latest/docs)
- [Version constraints](https://developer.hashicorp.com/terraform/language/expressions/version-constraints)
