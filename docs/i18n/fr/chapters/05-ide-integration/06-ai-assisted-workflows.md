# 5.6 Workflows assistés par IA

Les assistants d'éditeur peuvent ébaucher des blocs de ressources, expliquer une erreur ou suggérer une structure de variables. Ils sont utiles pour une première ébauche et pour des providers peu familiers.

## Les utiliser en toute sécurité

- Traitez le HCL généré comme une proposition. Exécutez `terraform validate` et lisez le `terraform plan` avant de lui faire confiance.
- Ne collez jamais de vrais identifiants, fichiers de state ou identifiants privés dans un prompt.
- Vérifiez les noms d'arguments par rapport à la documentation du provider ; les assistants inventent des attributs plausibles mais faux. Un mode d'échec courant : la suggestion est du HCL valide et correspond même à un argument qui existait dans une ancienne version du provider, mais qui a depuis été renommé ou supprimé — elle passe une relecture rapide, échoue à `terraform validate` ou, pire, change silencieusement au `plan` parce que l'attribut est simplement ignoré.
- Soyez particulièrement prudent avec tout ce qui supprime ou remplace des ressources.
- Certaines extensions d'éditeur exposent désormais les schémas des providers directement à l'assistant plutôt que de s'appuyer sur ses données d'entraînement — par exemple, l'extension HashiCorp Terraform de VS Code embarque un Terraform MCP server optionnel (`terraform.mcp.server.enable`) qui permet à un assistant connecté d'interroger le Terraform Registry. Cela réduit le mode d'échec « attribut inventé » ci-dessus, mais ne supprime pas le besoin de lire le plan.

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
- [Terraform MCP server](https://github.com/hashicorp/terraform-mcp-server)
