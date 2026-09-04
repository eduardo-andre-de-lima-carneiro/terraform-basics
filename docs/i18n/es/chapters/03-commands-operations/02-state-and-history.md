# 3.2 State e historial de cambios

El state es la forma en que Terraform recuerda lo que gestiona. Asocia cada bloque de recurso con un objeto real y almacena los últimos atributos conocidos de ese objeto.

## Inspeccionar el state

```bash
terraform state list
terraform show
terraform state show <resource address>
```

`state list` nombra cada recurso rastreado, `show` imprime todos los valores registrados y `state show` se centra en uno. `state list` acepta un patrón de dirección de recurso para filtrar, lo cual importa cuando un state contiene miles de recursos repartidos en muchos módulos: por ejemplo, `terraform state list module.notes` solo lista los recursos de ese módulo.

## Inspeccionar un plan guardado de la misma forma

`terraform show` no se limita al state: apuntado a un archivo guardado con `plan -out`, lo renderiza en el mismo formato legible para humanos, y `terraform show -json` produce una versión legible por máquina tanto del state como de un archivo de plan, pensada para herramientas externas. Como `-json` imprime los valores sensibles en texto plano, trata esa salida con el mismo cuidado que el propio archivo de state.

## Avanzado: leer y escribir el archivo de state en bruto

- `terraform state pull` imprime el state actual como JSON en bruto por stdout; útil para scripting o archivar una instantánea, pero es de solo lectura y seguro.
- `terraform state push` sube un archivo de state local al backend configurado, sobrescribiendo lo que haya allí. **Esto es destructivo**: reemplaza todo el state remoto de esta configuración, no solo un recurso, y un desajuste que acepte en silencio puede corromper la próxima ejecución de un compañero. Terraform rechaza el push por defecto si el destino tiene un linaje distinto o un número de serie más nuevo; pasa `-force` para saltarte esa comprobación solo si estás seguro de que la copia de destino es la que hay que descartar, y conserva una copia del archivo que estás a punto de sobrescribir (o el state actual del destino, obtenido antes con `pull`) para poder deshacer el push.

Recurre a `state pull`/`state push` solo cuando no hay otra forma de arreglar el state (por ejemplo, reparar a mano un JSON corrupto fuera de línea); para cambios cotidianos usa `terraform state list` / `show` / `mv` / `rm` e `import`, ya que cada uno toca un solo recurso a la vez y es revisable.

## Dónde vive el historial

Terraform no mantiene un registro de cambios propio completo. El historial revisable es tu historial de control de versiones de los archivos `.tf`, más el historial de ejecuciones de la plataforma que los aplica. Confirma los cambios de configuración en pasos pequeños y descritos para que el "porqué" sea recuperable.

## Práctica

Aplica una configuración pequeña, ejecuta `terraform state list`, luego cambia un atributo y ejecuta `terraform plan`. Observa cómo el plan explica la diferencia entre el state registrado y el nuevo estado deseado.

## Referencias

- [Manipulating state](https://developer.hashicorp.com/terraform/cli/state)
- [Command: state list](https://developer.hashicorp.com/terraform/cli/commands/state/list)
- [Command: show](https://developer.hashicorp.com/terraform/cli/commands/show)
- [Inspecting state](https://developer.hashicorp.com/terraform/cli/state/inspect)
- [Command: state pull](https://developer.hashicorp.com/terraform/cli/commands/state/pull)
- [Command: state push](https://developer.hashicorp.com/terraform/cli/commands/state/push)
