# 5.2 Visual Studio Code

Instala la extensión oficial **HashiCorp Terraform**. Incluye `terraform-ls` y añade funciones de lenguaje, formato y un pequeño conjunto de comandos.

## Después de instalar

- El autocompletado, la documentación al pasar el cursor y los diagnósticos en línea funcionan una vez que la carpeta contiene providers inicializados (`terraform init`).
- Habilita el formato al guardar para que `terraform fmt` se ejecute automáticamente:

```json
{
  "editor.formatOnSave": true,
  "[terraform]": { "editor.defaultFormatter": "hashicorp.terraform" },
  "[terraform-vars]": { "editor.defaultFormatter": "hashicorp.terraform" }
}
```

- La paleta de comandos ofrece `Terraform: init`, `Terraform: validate` y `Terraform: plan`, que invocan la CLI en la terminal integrada.

## Práctica

Abre un directorio de trabajo, ejecuta `terraform init` desde la terminal integrada, luego elimina un argumento obligatorio y confirma que aparece el subrayado rojo. Guarda el archivo y observa cómo `fmt` realinea el bloque.

## Referencias

- [HashiCorp Terraform extension (Marketplace)](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)
- [vscode-terraform (source and docs)](https://github.com/hashicorp/vscode-terraform)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
