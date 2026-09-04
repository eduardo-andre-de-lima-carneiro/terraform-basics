# 3.2 State e historial de cambios

El state es la forma en que Terraform recuerda lo que gestiona. Asocia cada bloque de recurso con un objeto real y almacena los últimos atributos conocidos de ese objeto.

## Inspeccionar el state

```bash
terraform state list
terraform show
terraform state show <resource address>
```

`state list` nombra cada recurso rastreado, `show` imprime todos los valores registrados y `state show` se centra en uno.

## Dónde vive el historial

Terraform no mantiene un registro de cambios propio completo. El historial revisable es tu historial de control de versiones de los archivos `.tf`, más el historial de ejecuciones de la plataforma que los aplica. Confirma los cambios de configuración en pasos pequeños y descritos para que el "porqué" sea recuperable.

## Práctica

Aplica una configuración pequeña, ejecuta `terraform state list`, luego cambia un atributo y ejecuta `terraform plan`. Observa cómo el plan explica la diferencia entre el state registrado y el nuevo estado deseado.

## Referencias

- [Manipulating state](https://developer.hashicorp.com/terraform/cli/state)
- [Command: state list](https://developer.hashicorp.com/terraform/cli/commands/state/list)
- [Command: show](https://developer.hashicorp.com/terraform/cli/commands/show)
- [Inspecting state](https://developer.hashicorp.com/terraform/cli/state/inspect)
