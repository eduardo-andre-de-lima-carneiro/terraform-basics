# 5.6 Flujos de trabajo asistidos por IA

Los asistentes del editor pueden redactar bloques de recurso, explicar un error o sugerir una estructura de variables. Son útiles para un primer borrador y para providers poco conocidos.

## Úsalos con seguridad

- Trata el HCL generado como una propuesta. Ejecuta `terraform validate` y lee el `terraform plan` antes de confiar en él.
- Nunca pegues credenciales reales, archivos de state ni identificadores privados en un prompt.
- Confirma los nombres de los argumentos con la documentación del provider; los asistentes inventan atributos plausibles pero incorrectos.
- Ten especial cuidado con cualquier cosa que elimine o reemplace recursos.

## Dónde ayudan más

- Explicar un error de validación o de plan en lenguaje llano.
- Convertir una configuración hecha en la consola en un primer borrador de bloque de recurso.
- Sugerir una estructura de bloque `for_each` o `dynamic` para eliminar repetición.

## Práctica

Pide a un asistente que genere un recurso `local_file`, luego verifica cada argumento con la documentación del provider y confirma que el plan hace exactamente lo que esperabas.

## Referencias

- [Terraform Registry (authoritative provider schemas)](https://registry.terraform.io/)
- [Style and validation: `terraform validate`](https://developer.hashicorp.com/terraform/cli/commands/validate)
- [`for_each` and `dynamic` blocks](https://developer.hashicorp.com/terraform/language/meta-arguments/for_each)
- [Terraform style guide](https://developer.hashicorp.com/terraform/language/style)
