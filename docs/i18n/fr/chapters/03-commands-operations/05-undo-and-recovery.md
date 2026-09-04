# 3.5 Annuler des changements en toute sécurité

Il n'existe pas d'« annulation » unique dans Terraform. Vous choisissez une action de récupération en fonction de ce qui s'est mal passé.

## Les principales options

- **Revenir à la configuration précédente** dans le contrôle de version, puis `plan` et `apply`. C'est l'annulation normale et sûre.
- **`terraform apply -replace=<address>`** détruit et recrée une ressource qui est cassée mais toujours suivie.
- **`terraform state rm <address>`** indique à Terraform d'oublier une ressource sans la supprimer. L'objet réel reste. Si vous ne la réimportez pas, la ressource n'est plus gérée et un `apply` ultérieur de la même configuration tentera de créer un doublon, ce qui peut échouer sur un conflit de nom. À n'utiliser que juste avant un réimport.
- **`terraform import <address> <id>`** ramène un objet existant sous gestion.
- **`terraform destroy`** supprime chaque ressource enregistrée dans le state pour cette configuration. C'est destructeur et irréversible ; exécutez d'abord `terraform plan -destroy` et ne le pointez jamais vers une infrastructure partagée.

Vous verrez peut-être d'anciens contenus mentionner **`terraform taint`** : cette commande est dépréciée. Utilisez `apply -replace` à la place — contrairement à `taint`, `-replace` affiche le remplacement dans un plan que vous approuvez, plutôt que de marquer silencieusement une ressource pour remplacement au prochain `apply` (potentiellement celui de quelqu'un d'autre).

## Quelle commande pour quelle situation

| Situation | Commande | Portée | Chemin d'annulation |
| --- | --- | --- | --- |
| Un changement de configuration était erroné | Revenir sur le changement `.tf` dans le contrôle de version, puis `plan`/`apply` | Tout ce que touche le diff annulé | Réappliquer la configuration précédente |
| Une ressource est cassée mais toujours correctement suivie par Terraform | `terraform apply -replace=<address>` | Une instance de ressource | Relancer `-replace`, ou accepter le nouvel objet |
| Terraform doit arrêter de gérer une ressource, sans la supprimer | `terraform state rm <address>` (ou un bloc `removed`, voir ci-dessous) | Une ressource, retirée du state uniquement | `terraform import` pour la ramener sous la même adresse ou une nouvelle |
| Un objet existe sur la plateforme mais pas dans le state | `terraform import <address> <id>` | Une ressource, ajoutée au state uniquement | `terraform state rm` pour la retirer à nouveau |
| Chaque ressource de cette configuration doit être supprimée | `terraform destroy` | Tout ce qui est dans ce state | Aucun — les objets sont réellement supprimés ; restaurez depuis une sauvegarde/un instantané si la plateforme en propose un |

## Alternatives déclaratives aux commandes state

Deux types de bloc vous permettent d'exprimer `state rm` et `import` sous forme de configuration relisible plutôt que de commandes ponctuelles — utile lorsque le changement doit passer par la même relecture en pull request que tout le reste :

```hcl
# Stop managing a resource without destroying the real object,
# and see the removal in `terraform plan` before it happens.
removed {
  from = local_file.notes

  lifecycle {
    destroy = false
  }
}
```

```hcl
# Bring an existing object under management, with the plan showing
# exactly what will be imported before you approve it.
import {
  to = local_file.notes
  id = "notes.txt"
}

resource "local_file" "notes" {
  filename = "notes.txt"
  content  = "shared example"
}
```

Les deux sont plus sûrs que leurs équivalents en ligne de commande, pour la même raison que `-replace` est plus sûr que `taint` : l'effet apparaît dans un plan que vous relisez, au lieu de prendre effet silencieusement au prochain `apply`. `terraform plan -generate-config-out=generated.tf` peut même écrire le bloc `resource` de départ à votre place à partir d'un bloc `import`, ce qui est pratique quand vous ne connaissez pas encore les attributs exacts de l'objet.

## Pratique

Dans un répertoire jetable, appliquez deux ressources, puis utilisez `-replace` sur l'une et confirmez d'après le plan que seule cette ressource est recréée. Ensuite, exécutez `terraform plan -destroy` et lisez la liste avant de décider de poursuivre ou non.

Ajoutez un bloc `removed` pour l'une des deux ressources et exécutez `terraform plan` ; vérifiez que le plan montre bien sa sortie du state avec `destroy = false` (l'objet réel reste intact). Écrivez ensuite un bloc `import` pour cette même ressource, en visant le fichier toujours présent sur le disque, et vérifiez que `terraform plan` montre son retour sous gestion sans aucun changement.

## Références

- [Command: plan (`-replace`)](https://developer.hashicorp.com/terraform/cli/commands/plan#replace-address)
- [Command: state rm](https://developer.hashicorp.com/terraform/cli/commands/state/rm)
- [Import overview](https://developer.hashicorp.com/terraform/cli/import)
- [Command: destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy)
- [Command: taint (deprecated)](https://developer.hashicorp.com/terraform/cli/commands/taint)
- [Removing resources: the `removed` block](https://developer.hashicorp.com/terraform/language/state/remove)
- [Generating configuration with `import` blocks](https://developer.hashicorp.com/terraform/language/import/generating-configuration)
