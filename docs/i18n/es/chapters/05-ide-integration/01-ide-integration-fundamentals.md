# 5.1 Fundamentos de la integración con el IDE

La integración del editor con Terraform es, sobre todo, un componente: el language server de Terraform, `terraform-ls`, mantenido por HashiCorp. Los editores se comunican con él a través del Language Server Protocol.

## Qué proporciona el language server

- Resaltado de sintaxis y validación estructural de HCL.
- Autocompletado de tipos de recurso, argumentos y atributos de los providers instalados.
- Documentación al pasar el cursor y "ir a la definición" para módulos y variables.
- Formato mediante `terraform fmt`.
- Diagnósticos que muestran los errores de `terraform validate` en línea.

## Qué sigue ejecutándose en una terminal

El language server no ejecuta `plan`, `apply` ni `destroy`. Esos siguen siendo comandos explícitos para que los cambios de infraestructura sean siempre deliberados. La mayoría de los editores añaden un botón o una tarea que simplemente invoca la CLI de Terraform.

## Práctica

Abre un archivo `.tf` en tu editor y confirma que funcionan tres cosas: el autocompletado dentro de un bloque de recurso, un error en línea al eliminar un argumento obligatorio y el formato al guardar. Si alguna falla, las siguientes secciones muestran cómo habilitarla.

## Referencias

- [Terraform CLI documentation](https://developer.hashicorp.com/terraform/cli)
- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [Language Server Protocol specification](https://microsoft.github.io/language-server-protocol/)
