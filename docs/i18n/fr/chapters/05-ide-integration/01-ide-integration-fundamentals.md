# 5.1 Fondamentaux de l'intégration aux IDE

L'intégration des éditeurs pour Terraform repose surtout sur un composant : le serveur de langage Terraform, `terraform-ls`, maintenu par HashiCorp. Les éditeurs dialoguent avec lui via le Language Server Protocol.

## Ce que fournit le serveur de langage

- La coloration syntaxique et la validation structurelle du HCL.
- La complétion des types de ressources, des arguments et des attributs à partir des providers installés.
- La documentation au survol et le « aller à la définition » pour les modules et les variables.
- Le formatage via `terraform fmt`.
- Des diagnostics qui font remonter les erreurs de `terraform validate` en ligne.

## Ce qui reste dans un terminal

Le serveur de langage n'exécute pas `plan`, `apply` ni `destroy`. Celles-ci restent des commandes explicites afin que les changements d'infrastructure soient toujours délibérés. La plupart des éditeurs ajoutent un bouton ou une tâche qui appelle simplement la CLI Terraform.

## Ce dont chaque fonctionnalité a réellement besoin

Les cinq fonctionnalités ci-dessus ne demandent pas toutes la même préparation. Savoir lesquelles fonctionnent avec seulement le binaire sur le `PATH`, par opposition à celles qui exigent un répertoire de travail initialisé, fait gagner du temps quand quelque chose « ne marche pas » dans un clone fraîchement récupéré :

| Fonctionnalité | Nécessite |
| --- | --- |
| Coloration syntaxique | Seulement la grammaire/l'extension de l'éditeur — aucun binaire requis |
| Formatage (`terraform fmt`) | Le binaire `terraform` sur le `PATH` |
| Diagnostics de `terraform validate` | Le binaire `terraform` sur le `PATH` ; plus précis une fois les schémas des providers disponibles |
| Complétion des arguments de resource/data source | `terraform-ls` en cours d'exécution, plus `terraform init` dans ce répertoire pour télécharger les schémas des providers |
| Documentation au survol et « aller à la définition » | `terraform-ls` en cours d'exécution, plus `terraform init` pour des résultats tenant compte des modules et des providers |

En pratique : cloner un dépôt et ouvrir un fichier `.tf` donne immédiatement la coloration syntaxique et le formatage, mais la complétion et le survol restent génériques (ou vides) tant que vous n'avez pas exécuté `terraform init` dans ce répertoire.

## Pratique

Ouvrez un fichier `.tf` dans votre éditeur et vérifiez que trois choses fonctionnent : la complétion dans un bloc de ressource, une erreur en ligne lorsque vous supprimez un argument obligatoire, et le formatage à l'enregistrement. Si l'une échoue, les sections suivantes montrent comment l'activer.

## Références

- [Terraform CLI documentation](https://developer.hashicorp.com/terraform/cli)
- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [Language Server Protocol specification](https://microsoft.github.io/language-server-protocol/)
