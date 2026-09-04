# 5.5 Outros Editores

Qualquer editor com suporte ao Language Server Protocol pode usar o `terraform-ls`.

- **Sublime Text:** o pacote LSP mais uma configuração auxiliar `LSP-terraform`.
- **Emacs:** `lsp-mode` ou `eglot` apontados para o `terraform-ls`, com `terraform-mode` para a sintaxe.
- **Zed:** já vem com suporte à linguagem Terraform que usa o `terraform-ls` automaticamente.
- **Helix:** configuração embutida para o `terraform-ls`; instale o binário e ele se conecta.

Para editores sem language server, você ainda tem valor com o `terraform fmt` como hook de salvamento e o `terraform validate` como comando de build.

## Prática

Escolha o seu editor, instale o `terraform-ls` no `PATH` e confirme que o autocompletar funciona dentro de um bloco de resource. Se não funcionar, verifique se a pasta foi inicializada com `terraform init`.

## Referências

- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Helix language support](https://docs.helix-editor.com/lang-support.html)
- [Zed languages](https://zed.dev/docs/languages)
