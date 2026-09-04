# 5.8 Exercices pratiques

Réalisez-les dans un répertoire de travail temporaire, en utilisant votre éditeur pour la rédaction et un terminal pour vérifier.

1. Déclenchez la complétion dans un bloc `resource` et acceptez un argument suggéré, puis confirmez avec `terraform validate`.
2. Supprimez un argument obligatoire et confirmez que l'éditeur affiche un diagnostic en ligne avant que vous n'exécutiez la moindre commande.
3. Activez le formatage à l'enregistrement, désalignez un bloc volontairement, enregistrez, et regardez `terraform fmt` le corriger.
4. Configurez `terraform-ls` dans un éditeur qui ne l'embarque pas, et confirmez que le client signale le serveur comme rattaché.
5. Ajoutez `terraform fmt -check -recursive` comme hook de pre-commit et confirmez qu'un fichier non formaté bloque le commit.
6. Utilisez une tâche de l'éditeur pour exécuter `terraform plan` et lisez le résultat sans quitter l'éditeur.

Pour chaque exercice, notez l'action de l'éditeur effectuée et la sortie de commande qui a confirmé le résultat.

## Références

- [terraform-ls](https://github.com/hashicorp/terraform-ls)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [pre-commit-terraform hooks](https://github.com/antonbabenko/pre-commit-terraform)
