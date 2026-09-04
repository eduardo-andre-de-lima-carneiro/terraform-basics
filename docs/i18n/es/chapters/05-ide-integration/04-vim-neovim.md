# 5.4 Vim y Neovim

Vim y Neovim obtienen soporte para Terraform conectándose a `terraform-ls`.

## Neovim con el cliente LSP integrado

```lua
require('lspconfig').terraformls.setup({})

vim.api.nvim_create_autocmd('BufWritePre', {
  pattern = { '*.tf', '*.tfvars' },
  callback = function() vim.lsp.buf.format() end,
})
```

Esto proporciona autocompletado, diagnósticos y formato al guardar. `nvim-treesitter` añade un resaltado preciso.

## Vim

Usa un plugin como `hashivim/vim-terraform` para la detección del tipo de archivo y los comandos `:Terraform`, y añade un plugin de cliente LSP para llegar a `terraform-ls`.

## Práctica

Abre un archivo `.tf`, confirma que `:LspInfo` (Neovim) muestra `terraformls` conectado, luego guarda y verifica que el búfer se reformatea.

## Referencias

- [terraform-ls](https://github.com/hashicorp/terraform-ls)
- [nvim-lspconfig: terraformls](https://github.com/neovim/nvim-lspconfig/blob/master/doc/configs.md#terraformls)
- [hashivim/vim-terraform](https://github.com/hashivim/vim-terraform)
- [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)
