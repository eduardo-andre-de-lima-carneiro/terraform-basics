# 5.2 Visual Studio Code

Instala la extensión oficial **HashiCorp Terraform** (`HashiCorp.terraform` en el Marketplace). Es una instalación autocontenida: incluye `terraform-ls`, así que no necesitas instalar el language server por separado para obtener autocompletado, diagnósticos y formato.

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

## Más allá del formato

La extensión también añade una vista **Module and Provider Explorer** que lista los módulos y providers referenciados por la configuración abierta, y un **Terraform MCP server** opcional (`terraform.mcp.server.enable`, desactivado por defecto) que permite a asistentes de IA como Copilot consultar directamente el Terraform Registry y los esquemas de los providers en lugar de adivinar. Ninguno de los dos es necesario para el flujo de edición principal, pero ambos vale la pena conocerlos si tu equipo combina la extensión con un asistente de IA; consulta [5.6 Flujos de trabajo asistidos por IA](06-ai-assisted-workflows.md).

## Práctica

Abre un directorio de trabajo, ejecuta `terraform init` desde la terminal integrada, luego elimina un argumento obligatorio y confirma que aparece el subrayado rojo. Guarda el archivo y observa cómo `fmt` realinea el bloque.

## Referencias

- [HashiCorp Terraform extension (Marketplace)](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)
- [vscode-terraform (source and docs)](https://github.com/hashicorp/vscode-terraform)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [terraform-ls settings reference](https://github.com/hashicorp/terraform-ls/blob/main/docs/SETTINGS.md)
- [Terraform MCP server](https://github.com/hashicorp/terraform-mcp-server)
