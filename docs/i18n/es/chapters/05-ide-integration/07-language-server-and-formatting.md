# 5.7 Configuración del language server y del formato

Dos ajustes hacen la mayor parte del trabajo para una experiencia de edición de Terraform consistente: el language server y el formato automático.

## Instala el language server

Descarga `terraform-ls` desde las releases oficiales o instálalo con un gestor de paquetes, y asegúrate de que está en el `PATH`. La mayoría de las extensiones de editor lo incluyen, pero una copia del sistema es útil para editores que no lo hacen.

## Ajustes comunes

Los ajustes de `terraform-ls` son objetos anidados, que el editor pasa como JSON o tablas de Lua. Los tres que más probablemente vayas a tocar:

| Ajuste | Propósito |
| --- | --- |
| `terraform.path` | Usar un binario `terraform` concreto en lugar del primero que aparezca en el `PATH` |
| `indexing.ignoreDirectoryNames` | Omitir directorios (por ejemplo, módulos vendorizados) al indexar un espacio de trabajo grande |
| `validation.enableEnhancedValidation` | Activar o desactivar los diagnósticos de validación enriquecidos y conscientes del esquema (activados por defecto) |

Si copias un fragmento de configuración de una entrada de blog antigua o de una respuesta de Stack Overflow, contrástalo primero con la referencia de ajustes de abajo: las claves planas como `terraformExecPath`, `terraformLogFilePath` y `rootModulePaths` están en desuso en favor de las formas anidadas `terraform.*` e `indexing.*` mostradas arriba.

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
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [Command: validate](https://developer.hashicorp.com/terraform/cli/commands/validate)
