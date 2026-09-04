# 5.2 Visual Studio Code

Installez l'extension officielle **HashiCorp Terraform** (`HashiCorp.terraform` sur le Marketplace). C'est une installation autonome : elle embarque `terraform-ls`, donc vous n'avez pas besoin d'installer le serveur de langage séparément pour obtenir la complétion, les diagnostics et le formatage.

## Après l'installation

- La complétion, la documentation au survol et les diagnostics en ligne fonctionnent dès que le dossier contient des providers initialisés (`terraform init`).
- Activez le formatage à l'enregistrement pour que `terraform fmt` s'exécute automatiquement :

```json
{
  "editor.formatOnSave": true,
  "[terraform]": { "editor.defaultFormatter": "hashicorp.terraform" },
  "[terraform-vars]": { "editor.defaultFormatter": "hashicorp.terraform" }
}
```

- La palette de commandes propose `Terraform: init`, `Terraform: validate` et `Terraform: plan`, qui appellent la CLI dans le terminal intégré.

## Au-delà du formatage

L'extension ajoute aussi une vue **Module and Provider Explorer** qui liste les modules et providers référencés par la configuration ouverte, ainsi qu'un **Terraform MCP server** optionnel (`terraform.mcp.server.enable`, désactivé par défaut) qui permet à des assistants IA comme Copilot d'interroger directement le Terraform Registry et les schémas des providers plutôt que de deviner. Aucun des deux n'est nécessaire au flux d'édition principal, mais les deux méritent d'être connus si votre équipe associe l'extension à un assistant IA — voir [5.6 Workflows assistés par IA](06-ai-assisted-workflows.md).

## Pratique

Ouvrez un répertoire de travail, exécutez `terraform init` depuis le terminal intégré, puis supprimez un argument obligatoire et vérifiez que le soulignement rouge apparaît. Enregistrez le fichier et regardez `fmt` réaligner le bloc.

## Références

- [HashiCorp Terraform extension (Marketplace)](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)
- [vscode-terraform (source and docs)](https://github.com/hashicorp/vscode-terraform)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [terraform-ls settings reference](https://github.com/hashicorp/terraform-ls/blob/main/docs/SETTINGS.md)
- [Terraform MCP server](https://github.com/hashicorp/terraform-mcp-server)
