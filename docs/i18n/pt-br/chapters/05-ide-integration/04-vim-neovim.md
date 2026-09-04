# 5.4 Vim e Neovim

O Vim e o Neovim ganham suporte a Terraform ao se conectarem ao `terraform-ls`. Nenhum dos dois editores o inclui, então instale o `terraform-ls` separadamente e garanta que ele esteja no `PATH` (veja [5.7](07-language-server-and-formatting.md)).

## Neovim com o cliente LSP embutido

Instale o plugin `nvim-lspconfig`, que já traz uma configuração pronta de `terraformls` (o comando, os filetypes `terraform`/`terraform-vars` e os root markers `.terraform` e `.git`) — ele não instala o servidor em si, apenas a conexão até ele.

No Neovim 0.11 ou mais recente, a configuração atualmente documentada habilita essa configuração diretamente:

```lua
vim.lsp.enable('terraformls')

vim.api.nvim_create_autocmd('BufWritePre', {
  pattern = { '*.tf', '*.tfvars' },
  callback = function() vim.lsp.buf.format() end,
})
```

Para sobrescrever um ajuste (por exemplo, `terraform.path` do `terraform-ls`), estenda a configuração antes de habilitá-la com `vim.lsp.config('terraformls', { ... })`. O padrão antigo, `require('lspconfig').terraformls.setup({})`, ainda funciona em versões mais antigas do Neovim, mas está descontinuado no `nvim-lspconfig` e exibe um aviso; novas configurações devem usar `vim.lsp.enable`.

Isso oferece autocompletar, diagnósticos e formatação ao salvar. O `nvim-treesitter` adiciona realce preciso.

## Vim

Use um plugin como o `hashivim/vim-terraform` para detecção de filetype e comandos `:Terraform`, e adicione um plugin de cliente LSP (por exemplo, `vim-lsp` ou coc.nvim) para alcançar o `terraform-ls`, já que o Vim puro não tem cliente LSP embutido.

## Prática

Abra um arquivo `.tf`, confirme que o `:checkhealth vim.lsp` (ou `:LspInfo` em configurações mais antigas) mostra o `terraformls` conectado, depois salve e verifique que o buffer é reformatado.

## Referências

- [terraform-ls](https://github.com/hashicorp/terraform-ls)
- [nvim-lspconfig: terraformls default config](https://github.com/neovim/nvim-lspconfig/blob/master/lsp/terraformls.lua)
- [nvim-lspconfig README (quickstart and `vim.lsp.config`)](https://github.com/neovim/nvim-lspconfig)
- [hashivim/vim-terraform](https://github.com/hashivim/vim-terraform)
- [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)
