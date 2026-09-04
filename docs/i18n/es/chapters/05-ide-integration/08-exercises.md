# 5.8 Ejercicios prácticos

Completa estos ejercicios en un directorio de trabajo temporal, usando tu editor para redactar y una terminal para verificar.

1. Dispara el autocompletado dentro de un bloque `resource` y acepta un argumento sugerido, luego confírmalo con `terraform validate`.
2. Elimina un argumento obligatorio y confirma que el editor muestra un diagnóstico en línea antes de que ejecutes ningún comando.
3. Habilita el formato al guardar, desalinea un bloque a propósito, guarda y observa cómo `terraform fmt` lo corrige.
4. Configura `terraform-ls` en un editor que no lo incluya, y confirma que el cliente informa de que el servidor está conectado.
5. Añade `terraform fmt -check -recursive` como hook de pre-commit y confirma que un archivo sin formatear bloquea el commit.
6. Usa una tarea del editor para ejecutar `terraform plan` y lee el resultado sin salir del editor.

Para cada ejercicio, anota la acción del editor realizada y la salida del comando que confirmó el resultado.

## Referencias

- [terraform-ls](https://github.com/hashicorp/terraform-ls)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [pre-commit-terraform hooks](https://github.com/antonbabenko/pre-commit-terraform)
