# 5.2 Visual Studio Code

Instale a extensão oficial **HashiCorp Terraform**. Ela inclui o `terraform-ls` e adiciona recursos de linguagem, formatação e um pequeno conjunto de comandos.

## Depois de instalar

- Autocompletar, documentação ao passar o mouse e diagnósticos inline funcionam quando a pasta contém providers inicializados (`terraform init`).
- Habilite a formatação ao salvar para que o `terraform fmt` rode automaticamente:

```json
{
  "editor.formatOnSave": true,
  "[terraform]": { "editor.defaultFormatter": "hashicorp.terraform" },
  "[terraform-vars]": { "editor.defaultFormatter": "hashicorp.terraform" }
}
```

- A paleta de comandos oferece `Terraform: init`, `Terraform: validate` e `Terraform: plan`, que chamam a CLI no terminal integrado.

## Prática

Abra um diretório de trabalho, execute `terraform init` no terminal integrado, depois exclua um argumento obrigatório e confirme que o sublinhado vermelho aparece. Salve o arquivo e observe o `fmt` realinhar o bloco.

## Referências

- [HashiCorp Terraform extension (Marketplace)](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)
- [vscode-terraform (source and docs)](https://github.com/hashicorp/vscode-terraform)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
