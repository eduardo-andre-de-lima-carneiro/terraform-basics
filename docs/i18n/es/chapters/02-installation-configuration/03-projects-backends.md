# 2.3 Directorios de trabajo y backends

Un directorio de trabajo de Terraform es cualquier carpeta que contenga archivos `.tf`. Cuando ejecutas `terraform init`, Terraform prepara esa carpeta: descarga los providers y configura el backend.

## Backend local

Por defecto, el state se escribe en `terraform.tfstate` dentro del directorio de trabajo. Esto está bien para aprender y para experimentos individuales.

## Backend remoto

Un backend remoto guarda el state en un servicio compartido, de modo que un equipo no se sobrescriba entre sí y los secretos no queden en una laptop:

```hcl
terraform {
  backend "s3" {
    bucket       = "example-tfstate"
    key          = "practice/terraform.tfstate"
    region       = "us-east-1"
    use_lockfile = true # locking nativo de S3; no requiere una tabla de DynamoDB
  }
}
```

Reemplaza cada valor con placeholders que controles. Cambiar un backend requiere volver a ejecutar `terraform init`, y Terraform ofrece migrar el state existente.

## Elegir un backend

Terraform incluye backends para muchos destinos, no solo S3. Algunos comunes:

| Backend | Uso típico |
| --- | --- |
| `local` | Por defecto; una sola máquina, aprendizaje, experimentos individuales |
| `s3` | Equipos centrados en AWS; state en un bucket S3, con locking nativo de S3 (`use_lockfile`) |
| `azurerm` | Equipos centrados en Azure; state en un contenedor de una storage account |
| `gcs` | Equipos centrados en Google Cloud; state en un bucket de Cloud Storage |
| `remote` / el bloque `cloud` | HCP Terraform o Terraform Enterprise; state, locking e historial de runs gestionados por ti (ver Capítulo 4) |

## Locking

El locking evita que dos ejecuciones escriban el state al mismo tiempo. La opción `use_lockfile = true` del backend S3 es el enfoque recomendado actualmente; el locking basado en una tabla de DynamoDB, más antiguo, está obsoleto. Sea cual sea el backend que elijas, confirma que hace locking antes de confiar en él para un equipo.

## Práctica

Inicializa un directorio con el backend local por defecto y localiza `terraform.tfstate`. Agrégalo a `.gitignore` y anota por qué hacer commit del state es inseguro: puede contener secretos y causa conflictos.

## Referencias

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [Backend types](https://developer.hashicorp.com/terraform/language/backend#available-backends)
- [State: sensitive data](https://developer.hashicorp.com/terraform/language/state/sensitive-data)
- [S3 backend](https://developer.hashicorp.com/terraform/language/backend/s3)
- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
