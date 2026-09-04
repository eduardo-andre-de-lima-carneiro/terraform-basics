# 5.4 Vim e Neovim

O Vim e o Neovim ganham suporte a Terraform ao se conectarem ao `terraform-ls`.

## Neovim com o cliente LSP embutido

```lua
require('lspconfig').terraformls.setup({})

vim.api.nvim_create_autocmd('BufWritePre', {
  pattern = { '*.tf', '*.tfvars' },
  callback = function() vim.lsp.buf.format() end,
})
```

Isso oferece autocompletar, diagnósticos e formatação ao salvar. O `nvim-treesitter` adiciona realce preciso.

## Vim

Use um plugin como o `hashivim/vim-terraform` para detecção de filetype e comandos `:Terraform`, e adicione um plugin de cliente LSP para alcançar o `terraform-ls`.

## Prática

Abra um arquivo `.tf`, confirme que o `:LspInfo` (Neovim) mostra o `terraformls` conectado, depois salve e verifique que o buffer é reformatado.

## Referências

- [terraform-ls](https://github.com/hashicorp/terraform-ls)
- [nvim-lspconfig: terraformls](https://github.com/neovim/nvim-lspconfig/blob/master/doc/configs.md#terraformls)
- [hashivim/vim-terraform](https://github.com/hashivim/vim-terraform)
- [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)
