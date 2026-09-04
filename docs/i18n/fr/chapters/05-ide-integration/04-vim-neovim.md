# 5.4 Vim et Neovim

Vim et Neovim obtiennent la prise en charge de Terraform en se rattachant à `terraform-ls`. Aucun des deux éditeurs ne l'embarque : installez `terraform-ls` séparément et assurez-vous qu'il est sur le `PATH` (voir [5.7](07-language-server-and-formatting.md)).

## Neovim avec le client LSP intégré

Installez le plugin `nvim-lspconfig`, qui fournit une configuration `terraformls` prête à l'emploi (la commande, les filetypes `terraform`/`terraform-vars` et les root markers `.terraform` et `.git`) — il n'installe pas le serveur lui-même, seulement le câblage vers celui-ci.

Sur Neovim 0.11 ou plus récent, la configuration actuellement documentée active directement cette configuration :

```lua
vim.lsp.enable('terraformls')

vim.api.nvim_create_autocmd('BufWritePre', {
  pattern = { '*.tf', '*.tfvars' },
  callback = function() vim.lsp.buf.format() end,
})
```

Pour surcharger un réglage (par exemple `terraform.path` de `terraform-ls`), étendez la configuration avant de l'activer avec `vim.lsp.config('terraformls', { ... })`. L'ancien schéma, `require('lspconfig').terraformls.setup({})`, fonctionne encore sur les versions plus anciennes de Neovim, mais il est déprécié dans `nvim-lspconfig` et affiche un avertissement ; les nouvelles configurations devraient utiliser `vim.lsp.enable`.

Cela apporte la complétion, les diagnostics et le formatage à l'enregistrement. `nvim-treesitter` ajoute une coloration précise.

## Vim

Utilisez un plugin tel que `hashivim/vim-terraform` pour la détection du type de fichier et les commandes `:Terraform`, et ajoutez un plugin client LSP (par exemple `vim-lsp` ou coc.nvim) pour atteindre `terraform-ls`, puisque Vim seul n'a pas de client LSP intégré.

## Pratique

Ouvrez un fichier `.tf`, vérifiez que `:checkhealth vim.lsp` (ou `:LspInfo` sur les configurations plus anciennes) montre `terraformls` rattaché, puis enregistrez et vérifiez que le buffer est reformaté.

## Références

- [terraform-ls](https://github.com/hashicorp/terraform-ls)
- [nvim-lspconfig: terraformls default config](https://github.com/neovim/nvim-lspconfig/blob/master/lsp/terraformls.lua)
- [nvim-lspconfig README (quickstart and `vim.lsp.config`)](https://github.com/neovim/nvim-lspconfig)
- [hashivim/vim-terraform](https://github.com/hashivim/vim-terraform)
- [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)
