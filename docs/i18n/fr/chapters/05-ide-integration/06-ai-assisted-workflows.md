# 5.6 Workflows assistés par IA

Les assistants d'éditeur peuvent ébaucher des blocs de ressources, expliquer une erreur ou suggérer une structure de variables. Ils sont utiles pour une première ébauche et pour des providers peu familiers.

## Les utiliser en toute sécurité

- Traitez le HCL généré comme une proposition. Exécutez `terraform validate` et lisez le `terraform plan` avant de lui faire confiance.
- Ne collez jamais de vrais identifiants, fichiers de state ou identifiants privés dans un prompt.
- Vérifiez les noms d'arguments par rapport à la documentation du provider ; les assistants inventent des attributs plausibles mais faux.
- Soyez particulièrement prudent avec tout ce qui supprime ou remplace des ressources.

## Là où ils aident le plus

- Expliquer une erreur de validation ou de plan en langage clair.
- Convertir une configuration faite en console en un bloc de ressource de première ébauche.
- Suggérer une structure de bloc `for_each` ou `dynamic` pour supprimer la répétition.

## Pratique

Demandez à un assistant de générer une ressource `local_file`, puis vérifiez chaque argument par rapport à la documentation du provider et confirmez que le plan fait exactement ce que vous attendiez.

## Références

- [Terraform Registry (authoritative provider schemas)](https://registry.terraform.io/)
- [Style and validation: `terraform validate`](https://developer.hashicorp.com/terraform/cli/commands/validate)
- [`for_each` and `dynamic` blocks](https://developer.hashicorp.com/terraform/language/meta-arguments/for_each)
- [Terraform style guide](https://developer.hashicorp.com/terraform/language/style)
