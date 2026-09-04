# 1.3 El modelo declarativo de Terraform

Describes el resultado que quieres, no los pasos para llegar a él. Terraform compara tu configuración con el state actual y la plataforma real, y luego decide qué acciones son necesarias.

## Declarativo frente a imperativo

Un script imperativo dice "crea esto y luego conecta aquello". Una configuración declarativa dice "estos recursos deben existir con estos ajustes". Si un recurso ya coincide, Terraform no hace nada. Si se ha desviado, Terraform propone una corrección.

## Los providers hacen el trabajo

Terraform en sí no conoce ninguna API de nube. Cada provider traduce los bloques de recurso en llamadas a la API y devuelve los resultados al state.

## Práctica

Lee un bloque de recurso breve y describe, en lenguaje llano, el estado final que declara. Después predice qué haría Terraform si ese recurso ya existiera sin cambios.

## Referencias

- [How Terraform works](https://developer.hashicorp.com/terraform/intro#how-does-terraform-work)
- [Providers (Terraform language)](https://developer.hashicorp.com/terraform/language/providers)
- [Resources overview](https://developer.hashicorp.com/terraform/language/resources)
- [Resource behavior and the dependency graph](https://developer.hashicorp.com/terraform/language/resources/behavior)
