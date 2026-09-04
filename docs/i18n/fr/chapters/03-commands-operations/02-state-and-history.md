# 3.2 State et historique des changements

Le state est la façon dont Terraform se souvient de ce qu'il gère. Il fait correspondre chaque bloc de ressource à un objet réel et stocke les derniers attributs connus de cet objet.

## Inspecter le state

```bash
terraform state list
terraform show
terraform state show <resource address>
```

`state list` nomme chaque ressource suivie, `show` affiche l'intégralité des valeurs enregistrées, et `state show` se concentre sur une seule.

## Où vit l'historique

Terraform ne conserve pas de journal complet des changements qui lui soit propre. L'historique consultable, c'est votre historique de contrôle de version des fichiers `.tf`, plus l'historique des exécutions dans la plateforme qui les applique. Committez les changements de configuration en petites étapes décrites afin que le « pourquoi » reste récupérable.

## Pratique

Appliquez une petite configuration, exécutez `terraform state list`, puis changez un attribut et exécutez `terraform plan`. Remarquez comment le plan explique la différence entre le state enregistré et le nouvel état souhaité.

## Références

- [Manipulating state](https://developer.hashicorp.com/terraform/cli/state)
- [Command: state list](https://developer.hashicorp.com/terraform/cli/commands/state/list)
- [Command: show](https://developer.hashicorp.com/terraform/cli/commands/show)
- [Inspecting state](https://developer.hashicorp.com/terraform/cli/state/inspect)
