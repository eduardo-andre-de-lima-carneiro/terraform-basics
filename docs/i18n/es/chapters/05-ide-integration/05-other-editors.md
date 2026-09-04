# 5.5 Otros editores

Cualquier editor con soporte para Language Server Protocol puede usar `terraform-ls`.

- **Sublime Text:** el paquete LSP más una configuración auxiliar `LSP-terraform`.
- **Emacs:** `lsp-mode` o `eglot` apuntando a `terraform-ls`, con `terraform-mode` para la sintaxis.
- **Zed:** incluye soporte de lenguaje para Terraform que usa `terraform-ls` automáticamente.
- **Helix:** configuración integrada para `terraform-ls`; instala el binario y se conecta.

Para editores sin language server, aún obtienes valor de `terraform fmt` como hook al guardar y de `terraform validate` como comando de build.

## Práctica

Elige tu editor, instala `terraform-ls` en el `PATH` y confirma que el autocompletado funciona dentro de un bloque de recurso. Si no funciona, comprueba que la carpeta se inicializó con `terraform init`.

## Referencias

- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Helix language support](https://docs.helix-editor.com/lang-support.html)
- [Zed languages](https://zed.dev/docs/languages)
