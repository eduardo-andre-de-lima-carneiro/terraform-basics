# 5.2 Visual Studio Code

Installez l'extension officielle **HashiCorp Terraform**. Elle embarque `terraform-ls` et ajoute des fonctionnalités de langage, le formatage et un petit ensemble de commandes.

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

## Pratique

Ouvrez un répertoire de travail, exécutez `terraform init` depuis le terminal intégré, puis supprimez un argument obligatoire et vérifiez que le soulignement rouge apparaît. Enregistrez le fichier et regardez `fmt` réaligner le bloc.

## Références

- [HashiCorp Terraform extension (Marketplace)](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)
- [vscode-terraform (source and docs)](https://github.com/hashicorp/vscode-terraform)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
