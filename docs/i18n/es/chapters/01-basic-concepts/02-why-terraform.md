# 1.2 Por qué Terraform

Terraform es una herramienta muy usada para infraestructura como código. Lee configuración declarativa, se comunica con las plataformas a través de providers y mantiene un archivo de state para saber qué gestiona ya.

## Qué lo hace útil

- Un mismo flujo de trabajo (`init`, `plan`, `apply`) funciona con muchos providers.
- Un plan muestra los cambios exactos antes de que ocurra nada.
- El state permite que Terraform actualice y elimine solo lo que creó.
- Los módulos empaquetan configuración para reutilizarla entre equipos y entornos.

## Práctica

Piensa en dos plataformas que use tu equipo, como un proveedor de nube y un servicio de DNS. Con Terraform, ambas se gestionan con los mismos comandos y el mismo formato de archivo. Anota dónde te ahorraría tiempo esa consistencia a tu equipo hoy mismo.

## Referencias

- [What is Terraform? (HashiCorp)](https://developer.hashicorp.com/terraform/intro)
- [Terraform use cases](https://developer.hashicorp.com/terraform/intro/use-cases)
- [Terraform Registry: providers](https://registry.terraform.io/browse/providers)
- [Terraform workflow overview](https://developer.hashicorp.com/terraform/intro/core-workflow)
