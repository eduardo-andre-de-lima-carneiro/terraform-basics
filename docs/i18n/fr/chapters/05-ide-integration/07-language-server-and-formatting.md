# 5.7 Configuration du serveur de langage et du formatage

Deux réglages font l'essentiel du travail pour une expérience d'édition Terraform cohérente : le serveur de langage et le formatage automatique.

## Installer le serveur de langage

Téléchargez `terraform-ls` depuis les releases officielles ou installez-le avec un gestionnaire de paquets, et assurez-vous qu'il est sur le `PATH`. La plupart des extensions d'éditeur l'embarquent, mais une copie système est utile pour les éditeurs qui ne le font pas.

## Imposer le formatage

`terraform fmt` est le formateur canonique. Câblez-le dans l'éditeur en formatage à l'enregistrement, et imposez-le aussi en dehors de l'éditeur :

```bash
terraform fmt -check -recursive
```

Exécutez cela en CI pour qu'un fichier non formaté fasse échouer le pipeline au lieu de provoquer des diffs bruyants.

## Validation à la demande

Associez une tâche ou une touche à `terraform validate` pour que les erreurs structurelles remontent sans un `plan` complet. Rappelez-vous que `validate` ne contacte pas les API des providers ; il vérifie seulement que la configuration est cohérente en interne.

## Pratique

Ajoutez `terraform fmt -check -recursive` à la CI de votre projet ou à un hook de pre-commit, puis committez un fichier volontairement mal aligné et confirmez que le contrôle échoue.

## Références

- [terraform-ls releases](https://releases.hashicorp.com/terraform-ls/)
- [terraform-ls settings](https://github.com/hashicorp/terraform-ls/blob/main/docs/SETTINGS.md)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [Command: validate](https://developer.hashicorp.com/terraform/cli/commands/validate)
