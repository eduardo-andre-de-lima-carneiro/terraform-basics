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

## Qué necesita realmente cada función

Las cinco funciones anteriores no requieren la misma preparación. Saber cuáles funcionan solo con el binario en el `PATH`, frente a cuáles necesitan un directorio de trabajo inicializado, ahorra tiempo cuando algo "no funciona" en un clon recién hecho:

| Función | Requiere |
| --- | --- |
| Resaltado de sintaxis | Solo la gramática/extensión del editor; no necesita ningún binario |
| Formato (`terraform fmt`) | El binario `terraform` en el `PATH` |
| Diagnósticos de `terraform validate` | El binario `terraform` en el `PATH`; más preciso una vez disponibles los esquemas de los providers |
| Autocompletado de argumentos de resource/data source | `terraform-ls` en ejecución, más `terraform init` en ese directorio para descargar los esquemas de los providers |
| Documentación al pasar el cursor y "ir a la definición" | `terraform-ls` en ejecución, más `terraform init` para resultados conscientes de módulos y providers |

En la práctica: clonar un repositorio y abrir un archivo `.tf` te da resaltado y formato de inmediato, pero el autocompletado y el hover siguen siendo genéricos (o vacíos) hasta que ejecutas `terraform init` en ese directorio.

## Práctica

Abre un archivo `.tf` en tu editor y confirma que funcionan tres cosas: el autocompletado dentro de un bloque de recurso, un error en línea al eliminar un argumento obligatorio y el formato al guardar. Si alguna falla, las siguientes secciones muestran cómo habilitarla.

## Referencias

- [Terraform CLI documentation](https://developer.hashicorp.com/terraform/cli)
- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [Language Server Protocol specification](https://microsoft.github.io/language-server-protocol/)
