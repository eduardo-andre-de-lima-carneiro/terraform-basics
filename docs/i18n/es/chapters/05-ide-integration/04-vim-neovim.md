# 5.4 Vim y Neovim

Vim y Neovim obtienen soporte para Terraform conectándose a `terraform-ls`. Ninguno de los dos editores lo incluye, así que instala `terraform-ls` por separado y asegúrate de que está en el `PATH` (consulta [5.7](07-language-server-and-formatting.md)).

## Neovim con el cliente LSP integrado

Instala el plugin `nvim-lspconfig`, que incluye una configuración lista para usar de `terraformls` (el comando, los filetypes `terraform`/`terraform-vars` y los root markers `.terraform` y `.git`); no instala el servidor en sí, solo la conexión hacia él.

En Neovim 0.11 o superior, la configuración actual documentada habilita esa configuración directamente:

```lua
vim.lsp.enable('terraformls')

vim.api.nvim_create_autocmd('BufWritePre', {
  pattern = { '*.tf', '*.tfvars' },
  callback = function() vim.lsp.buf.format() end,
})
```

Para sobrescribir un ajuste (por ejemplo, `terraform.path` de `terraform-ls`), extiende la configuración antes de habilitarla con `vim.lsp.config('terraformls', { ... })`. El patrón anterior, `require('lspconfig').terraformls.setup({})`, sigue funcionando en versiones de Neovim más antiguas, pero está en desuso en `nvim-lspconfig` y muestra una advertencia; las configuraciones nuevas deberían usar `vim.lsp.enable`.

Esto proporciona autocompletado, diagnósticos y formato al guardar. `nvim-treesitter` añade un resaltado preciso.

## Vim

Usa un plugin como `hashivim/vim-terraform` para la detección del tipo de archivo y los comandos `:Terraform`, y añade un plugin de cliente LSP (por ejemplo `vim-lsp` o coc.nvim) para llegar a `terraform-ls`, ya que Vim puro no tiene cliente LSP integrado.

## Práctica

Abre un archivo `.tf`, confirma que `:checkhealth vim.lsp` (o `:LspInfo` en configuraciones más antiguas) muestra `terraformls` conectado, luego guarda y verifica que el búfer se reformatea.

## Referencias

- [terraform-ls](https://github.com/hashicorp/terraform-ls)
- [nvim-lspconfig: terraformls default config](https://github.com/neovim/nvim-lspconfig/blob/master/lsp/terraformls.lua)
- [nvim-lspconfig README (quickstart and `vim.lsp.config`)](https://github.com/neovim/nvim-lspconfig)
- [hashivim/vim-terraform](https://github.com/hashivim/vim-terraform)
- [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)
