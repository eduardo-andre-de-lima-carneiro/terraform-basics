# 2.3 Directorios de trabajo y backends

Un directorio de trabajo de Terraform es cualquier carpeta que contiene archivos `.tf`. Cuando ejecutas `terraform init`, Terraform prepara esa carpeta: descarga los providers y configura el backend.

## Backend local

Por defecto, el state se escribe en `terraform.tfstate` dentro del directorio de trabajo. Esto vale para aprender y para experimentos en solitario.

## Backend remoto

Un backend remoto almacena el state en un servicio compartido para que un equipo no se sobrescriba entre sí y para que los secretos no queden en un portátil:

```hcl
terraform {
  backend "s3" {
    bucket = "example-tfstate"
    key    = "practice/terraform.tfstate"
    region = "us-east-1"
  }
}
```

Sustituye cada valor por placeholders que tú controles. Cambiar de backend requiere volver a ejecutar `terraform init`, y Terraform se ofrece a migrar el state existente.

## Práctica

Inicializa un directorio con el backend local por defecto y localiza `terraform.tfstate`. Añádelo a `.gitignore` y anota por qué es inseguro versionar el state: puede contener secretos y provoca conflictos.

## Referencias

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [State: sensitive data](https://developer.hashicorp.com/terraform/language/state/sensitive-data)
- [S3 backend](https://developer.hashicorp.com/terraform/language/backend/s3)
- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
