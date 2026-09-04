# 5.4 Vim and Neovim

Vim and Neovim get Terraform support by attaching to `terraform-ls`. Neither editor bundles it, so install `terraform-ls` separately and make sure it is on `PATH` (see [5.7](07-language-server-and-formatting.md)).

## Neovim with the built-in LSP client

Install the `nvim-lspconfig` plugin, which ships a ready-made `terraformls` configuration (the command, filetypes `terraform`/`terraform-vars`, and root markers `.terraform` and `.git`) — it does not install the server itself, only the wiring to it.

On Neovim 0.11 or newer, the current documented setup enables that configuration directly:

```lua
vim.lsp.enable('terraformls')

vim.api.nvim_create_autocmd('BufWritePre', {
  pattern = { '*.tf', '*.tfvars' },
  callback = function() vim.lsp.buf.format() end,
})
```

To override a setting (for example `terraform-ls`'s `terraform.path`), extend the config before enabling it with `vim.lsp.config('terraformls', { ... })`. The older pattern, `require('lspconfig').terraformls.setup({})`, still works on older Neovim releases but is deprecated in `nvim-lspconfig` and prints a warning; new setups should use `vim.lsp.enable`.

This gives completion, diagnostics, and format-on-save. `nvim-treesitter` adds accurate highlighting.

## Vim

Use a plugin such as `hashivim/vim-terraform` for filetype detection and `:Terraform` commands, and add an LSP client plugin (for example `vim-lsp` or coc.nvim) to reach `terraform-ls`, since plain Vim has no built-in LSP client.

## Practice

Open a `.tf` file, confirm `:checkhealth vim.lsp` (or `:LspInfo` on older setups) shows `terraformls` attached, then save and verify the buffer is reformatted.

## References

- [terraform-ls](https://github.com/hashicorp/terraform-ls)
- [nvim-lspconfig: terraformls default config](https://github.com/neovim/nvim-lspconfig/blob/master/lsp/terraformls.lua)
- [nvim-lspconfig README (quickstart and `vim.lsp.config`)](https://github.com/neovim/nvim-lspconfig)
- [hashivim/vim-terraform](https://github.com/hashivim/vim-terraform)
- [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)
