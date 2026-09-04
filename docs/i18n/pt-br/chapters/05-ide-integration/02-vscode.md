# 5.2 Visual Studio Code

Instale a extensão oficial **HashiCorp Terraform** (`HashiCorp.terraform` no Marketplace). É uma instalação autocontida: ela inclui o `terraform-ls`, então não é preciso instalar o language server separadamente para ter autocompletar, diagnósticos e formatação.

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

## Além da formatação

A extensão também adiciona uma view **Module and Provider Explorer**, que lista os módulos e providers referenciados pela configuração aberta, e um **Terraform MCP server** opcional (`terraform.mcp.server.enable`, desativado por padrão) que permite a assistentes de IA como o Copilot consultar o Terraform Registry e os esquemas dos providers diretamente, em vez de adivinhar. Nenhum dos dois é necessário para o fluxo de edição principal, mas vale a pena conhecê-los se sua equipe combina a extensão com um assistente de IA; veja [5.6 Fluxos de Trabalho Assistidos por IA](06-ai-assisted-workflows.md).

## Prática

Abra um diretório de trabalho, execute `terraform init` no terminal integrado, depois exclua um argumento obrigatório e confirme que o sublinhado vermelho aparece. Salve o arquivo e observe o `fmt` realinhar o bloco.

## Referências

- [HashiCorp Terraform extension (Marketplace)](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)
- [vscode-terraform (source and docs)](https://github.com/hashicorp/vscode-terraform)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [terraform-ls settings reference](https://github.com/hashicorp/terraform-ls/blob/main/docs/SETTINGS.md)
- [Terraform MCP server](https://github.com/hashicorp/terraform-mcp-server)
