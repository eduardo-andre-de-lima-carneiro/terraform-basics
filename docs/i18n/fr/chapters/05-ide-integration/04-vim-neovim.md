# 5.4 Vim et Neovim

Vim et Neovim obtiennent la prise en charge de Terraform en se rattachant à `terraform-ls`.

## Neovim avec le client LSP intégré

```lua
require('lspconfig').terraformls.setup({})

vim.api.nvim_create_autocmd('BufWritePre', {
  pattern = { '*.tf', '*.tfvars' },
  callback = function() vim.lsp.buf.format() end,
})
```

Cela apporte la complétion, les diagnostics et le formatage à l'enregistrement. `nvim-treesitter` ajoute une coloration précise.

## Vim

Utilisez un plugin tel que `hashivim/vim-terraform` pour la détection du type de fichier et les commandes `:Terraform`, et ajoutez un plugin client LSP pour atteindre `terraform-ls`.

## Pratique

Ouvrez un fichier `.tf`, vérifiez que `:LspInfo` (Neovim) montre `terraformls` rattaché, puis enregistrez et vérifiez que le buffer est reformaté.

## Références

- [terraform-ls](https://github.com/hashicorp/terraform-ls)
- [nvim-lspconfig: terraformls](https://github.com/neovim/nvim-lspconfig/blob/master/doc/configs.md#terraformls)
- [hashivim/vim-terraform](https://github.com/hashivim/vim-terraform)
- [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)
