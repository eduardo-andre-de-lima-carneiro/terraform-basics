# 5.5 Otros editores

Cualquier editor con soporte para Language Server Protocol puede usar `terraform-ls`.

- **Sublime Text:** el paquete LSP más el paquete auxiliar `LSP-terraform`, instalados mediante Package Control.
- **Emacs:** `lsp-mode` (conectado a `terraform-mode`) o `eglot` apuntando a `terraform-ls`, con `terraform-mode` para la sintaxis y la indentación.
- **Zed:** no viene integrado; instala la [extensión Terraform](https://github.com/zed-extensions/terraform) de la comunidad desde el panel de extensiones de Zed; después ejecuta `terraform-ls` por ti.
- **Helix:** incluye una definición de lenguaje `hcl` (que también cubre los archivos `.tf`) preconfigurada para usar `terraform-ls`; instala el binario en el `PATH` y se conecta sin configuración adicional.

Para editores sin language server, aún obtienes valor de `terraform fmt` como hook al guardar y de `terraform validate` como comando de build.

## Práctica

Elige tu editor, instala `terraform-ls` en el `PATH` y confirma que el autocompletado funciona dentro de un bloque de recurso. Si no funciona, comprueba que la carpeta se inicializó con `terraform init`.

## Referencias

- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Helix language support](https://docs.helix-editor.com/lang-support.html)
- [Zed: Terraform language docs](https://zed.dev/docs/languages/terraform)
- [zed-extensions/terraform](https://github.com/zed-extensions/terraform)
