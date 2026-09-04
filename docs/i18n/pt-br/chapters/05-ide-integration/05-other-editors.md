# 5.5 Outros Editores

Qualquer editor com suporte ao Language Server Protocol pode usar o `terraform-ls`.

- **Sublime Text:** o pacote LSP mais o pacote auxiliar `LSP-terraform`, instalados via Package Control.
- **Emacs:** `lsp-mode` (conectado ao `terraform-mode`) ou `eglot` apontados para o `terraform-ls`, com `terraform-mode` para a sintaxe e a indentação.
- **Zed:** não vem embutido — instale a [extensão Terraform](https://github.com/zed-extensions/terraform) da comunidade pelo painel de extensões do Zed; ela então roda o `terraform-ls` para você.
- **Helix:** traz uma definição de linguagem `hcl` (que também cobre arquivos `.tf`) já configurada para usar o `terraform-ls`; instale o binário no `PATH` e ele se conecta sem configuração extra.

Para editores sem language server, você ainda tem valor com o `terraform fmt` como hook de salvamento e o `terraform validate` como comando de build.

## Prática

Escolha o seu editor, instale o `terraform-ls` no `PATH` e confirme que o autocompletar funciona dentro de um bloco de resource. Se não funcionar, verifique se a pasta foi inicializada com `terraform init`.

## Referências

- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Helix language support](https://docs.helix-editor.com/lang-support.html)
- [Zed: Terraform language docs](https://zed.dev/docs/languages/terraform)
- [zed-extensions/terraform](https://github.com/zed-extensions/terraform)
