# 5.6 Flujos de trabajo asistidos por IA

Los asistentes del editor pueden redactar bloques de recurso, explicar un error o sugerir una estructura de variables. Son útiles para un primer borrador y para providers poco conocidos.

## Úsalos con seguridad

- Trata el HCL generado como una propuesta. Ejecuta `terraform validate` y lee el `terraform plan` antes de confiar en él.
- Nunca pegues credenciales reales, archivos de state ni identificadores privados en un prompt.
- Confirma los nombres de los argumentos con la documentación del provider; los asistentes inventan atributos plausibles pero incorrectos. Un fallo típico: la sugerencia es HCL válido e incluso coincide con un argumento que existió en una versión anterior del provider, pero que desde entonces se renombró o se eliminó; pasa una lectura superficial, falla en `terraform validate` o, peor, cambia en silencio en el `plan` porque el atributo simplemente se ignora.
- Ten especial cuidado con cualquier cosa que elimine o reemplace recursos.
- Algunas extensiones de editor ahora exponen los esquemas de los providers al asistente directamente en lugar de depender de sus datos de entrenamiento; por ejemplo, la extensión HashiCorp Terraform de VS Code incluye un Terraform MCP server opcional (`terraform.mcp.server.enable`) que permite a un asistente conectado consultar el Terraform Registry. Eso reduce el fallo de "atributo inventado" anterior, pero no elimina la necesidad de leer el plan.

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
- [Terraform MCP server](https://github.com/hashicorp/terraform-mcp-server)
