# 5.7 Configuración del language server y del formato

Dos ajustes hacen la mayor parte del trabajo para una experiencia de edición de Terraform consistente: el language server y el formato automático.

## Instala el language server

Descarga `terraform-ls` desde las releases oficiales o instálalo con un gestor de paquetes, y asegúrate de que está en el `PATH`. La mayoría de las extensiones de editor lo incluyen, pero una copia del sistema es útil para editores que no lo hacen.

## Impón el formato

`terraform fmt` es el formateador canónico. Conéctalo al editor como formato al guardar, y también imponlo fuera del editor:

```bash
terraform fmt -check -recursive
```

Ejecútalo en CI para que un archivo sin formatear haga fallar el pipeline en lugar de provocar diffs ruidosos.

## Validación bajo demanda

Vincula una tarea o una tecla a `terraform validate` para que los errores estructurales aparezcan sin un `plan` completo. Recuerda que `validate` no contacta con las API de los providers; solo comprueba que la configuración es internamente consistente.

## Práctica

Añade `terraform fmt -check -recursive` a la CI de tu proyecto o a un hook de pre-commit, luego confirma un archivo mal alineado a propósito y verifica que la comprobación falla.

## Referencias

- [terraform-ls releases](https://releases.hashicorp.com/terraform-ls/)
- [terraform-ls settings](https://github.com/hashicorp/terraform-ls/blob/main/docs/SETTINGS.md)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [Command: validate](https://developer.hashicorp.com/terraform/cli/commands/validate)
