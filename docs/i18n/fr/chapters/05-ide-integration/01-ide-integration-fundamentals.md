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

## Pratique

Ouvrez un fichier `.tf` dans votre éditeur et vérifiez que trois choses fonctionnent : la complétion dans un bloc de ressource, une erreur en ligne lorsque vous supprimez un argument obligatoire, et le formatage à l'enregistrement. Si l'une échoue, les sections suivantes montrent comment l'activer.

## Références

- [Terraform CLI documentation](https://developer.hashicorp.com/terraform/cli)
- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [Language Server Protocol specification](https://microsoft.github.io/language-server-protocol/)
