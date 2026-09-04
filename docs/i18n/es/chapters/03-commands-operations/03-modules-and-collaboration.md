# 3.3 Módulos y colaboración

Un módulo es una carpeta de archivos `.tf` pensada para reutilizarse. La carpeta en la que ejecutas los comandos es el módulo raíz; puede invocar módulos hijos.

## Invocar un módulo

```hcl
module "notes" {
  source  = "./modules/notes"
  content = "shared example"
}
```

Las entradas se pasan como argumentos, los resultados se leen desde los valores `output` del módulo y `terraform init` instala cualquier fuente de módulo remota.

## Mover un recurso a un módulo

Mover un recurso cambia su dirección (por ejemplo, `local_file.notes` pasa a ser `module.notes.local_file.notes`). Sin ayuda, `terraform plan` mostraría ese recurso como destruido y vuelto a crear. Indícale a Terraform que es el mismo objeto con un bloque `moved`:

```hcl
moved {
  from = local_file.notes
  to   = module.notes.local_file.notes
}
```

Ahora `terraform plan` informa de que no hay cambios. (`terraform state mv` hace lo mismo como comando puntual en lugar de configuración versionada.)

## Colabora con seguridad

- Mantén los módulos pequeños y centrados en un solo propósito.
- Fija las versiones de módulos y providers para que tus compañeros obtengan el mismo resultado.
- Revisa los planes en los pull requests, no solo el diff de la configuración.
- Comparte el state a través de un backend remoto para que las ejecuciones no colisionen.

## Práctica

Mueve un recurso a una carpeta `./modules/notes`, expón una entrada y una salida, añade el bloque `moved` correspondiente y ejecuta `terraform plan`. Confirma que informa de que no hay cambios.

## Referencias

- [Modules overview](https://developer.hashicorp.com/terraform/language/modules)
- [Module blocks](https://developer.hashicorp.com/terraform/language/modules/syntax)
- [Refactoring with `moved` blocks](https://developer.hashicorp.com/terraform/language/modules/develop/refactoring)
- [Command: state mv](https://developer.hashicorp.com/terraform/cli/commands/state/mv)
- [Module creation tutorial](https://developer.hashicorp.com/terraform/tutorials/modules/module-create)
