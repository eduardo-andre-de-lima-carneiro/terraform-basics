# 3.5 Annuler des changements en toute sécurité

Il n'existe pas d'« annulation » unique dans Terraform. Vous choisissez une action de récupération en fonction de ce qui s'est mal passé.

## Les principales options

- **Revenir à la configuration précédente** dans le contrôle de version, puis `plan` et `apply`. C'est l'annulation normale et sûre.
- **`terraform apply -replace=<address>`** détruit et recrée une ressource qui est cassée mais toujours suivie.
- **`terraform state rm <address>`** indique à Terraform d'oublier une ressource sans la supprimer. L'objet réel reste. Si vous ne la réimportez pas, la ressource n'est plus gérée et un `apply` ultérieur de la même configuration tentera de créer un doublon, ce qui peut échouer sur un conflit de nom. À n'utiliser que juste avant un réimport.
- **`terraform import <address> <id>`** ramène un objet existant sous gestion.
- **`terraform destroy`** supprime chaque ressource enregistrée dans le state pour cette configuration. C'est destructeur et irréversible ; exécutez d'abord `terraform plan -destroy` et ne le pointez jamais vers une infrastructure partagée.

## Pratique

Dans un répertoire jetable, appliquez deux ressources, puis utilisez `-replace` sur l'une et confirmez d'après le plan que seule cette ressource est recréée. Ensuite, exécutez `terraform plan -destroy` et lisez la liste avant de décider de poursuivre ou non.

## Références

- [Command: plan (`-replace`)](https://developer.hashicorp.com/terraform/cli/commands/plan#replace-address)
- [Command: state rm](https://developer.hashicorp.com/terraform/cli/commands/state/rm)
- [Import overview](https://developer.hashicorp.com/terraform/cli/import)
- [Command: destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy)
