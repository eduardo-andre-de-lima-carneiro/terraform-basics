# 5.4 Vim and Neovim

Vim and Neovim get Terraform support by attaching to `terraform-ls`.

## Neovim with the built-in LSP client

```lua
require('lspconfig').terraformls.setup({})

vim.api.nvim_create_autocmd('BufWritePre', {
  pattern = { '*.tf', '*.tfvars' },
  callback = function() vim.lsp.buf.format() end,
})
```

This gives completion, diagnostics, and format-on-save. `nvim-treesitter` adds accurate highlighting.

## Vim

Use a plugin such as `hashivim/vim-terraform` for filetype detection and `:Terraform` commands, and add an LSP client plugin to reach `terraform-ls`.

## Practice

Open a `.tf` file, confirm `:LspInfo` (Neovim) shows `terraformls` attached, then save and verify the buffer is reformatted.

## References

- [terraform-ls](https://github.com/hashicorp/terraform-ls)
- [nvim-lspconfig: terraformls](https://github.com/neovim/nvim-lspconfig/blob/master/doc/configs.md#terraformls)
- [hashivim/vim-terraform](https://github.com/hashivim/vim-terraform)
- [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)
