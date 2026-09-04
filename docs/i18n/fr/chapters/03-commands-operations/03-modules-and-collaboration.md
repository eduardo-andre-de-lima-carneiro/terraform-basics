# 3.3 Modules et collaboration

Un module est un dossier de fichiers `.tf` destiné à être réutilisé. Le dossier dans lequel vous exécutez les commandes est le module racine ; il peut appeler des modules enfants.

## Appeler un module

```hcl
module "notes" {
  source  = "./modules/notes"
  content = "shared example"
}
```

Les entrées sont passées en arguments, les résultats se lisent depuis les valeurs `output` du module, et `terraform init` installe toute source de module distante.

## Épingler la version d'un module

Un `source` peut pointer vers un chemin local (comme ci-dessus, sans versionnage — il utilise toujours ce qui se trouve sur le disque), ou vers une source distante comme le Terraform Registry public, qui prend en charge une contrainte `version` distincte :

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "6.0.1"   # or a range, e.g. "~> 6.0"
}
```

Il s'agit uniquement d'un exemple de syntaxe — n'exécutez pas `apply` sur un module cloud réel dans les exercices de ce cours ; servez-vous-en pour reconnaître le motif quand vous le rencontrerez dans un vrai projet. Les sources en chemin local utilisent toujours le code actuel et ignorent `version` ; seules les sources de registry, Git et autres sources distantes sont versionnées. Après avoir modifié une contrainte `version`, exécutez `terraform init -upgrade` afin que Terraform résolve à nouveau et télécharge la version nouvellement autorisée au lieu de conserver celle déjà installée.

## Déplacer une ressource dans un module

Déplacer une ressource change son adresse (par exemple `local_file.notes` devient `module.notes.local_file.notes`). Sans aide, `terraform plan` montrerait cette ressource détruite puis recréée. Indiquez à Terraform qu'il s'agit du même objet avec un bloc `moved` :

```hcl
moved {
  from = local_file.notes
  to   = module.notes.local_file.notes
}
```

Désormais, `terraform plan` ne signale aucun changement. (`terraform state mv` fait la même chose sous forme de commande ponctuelle plutôt que de configuration committée.)

## Collaborer en toute sécurité

- Gardez les modules petits et centrés sur un seul objectif.
- Épinglez les versions des modules et des providers pour que les coéquipiers obtiennent le même résultat.
- Relisez les plans dans les pull requests, pas seulement le diff de configuration.
- Partagez le state via un backend distant pour que les exécutions n'entrent pas en collision.

## Pratique

Déplacez une ressource dans un dossier `./modules/notes`, exposez une entrée et une sortie, ajoutez le bloc `moved` correspondant, et exécutez `terraform plan`. Vérifiez qu'il ne signale aucun changement.

## Références

- [Modules overview](https://developer.hashicorp.com/terraform/language/modules)
- [Module blocks](https://developer.hashicorp.com/terraform/language/modules/syntax)
- [Module sources](https://developer.hashicorp.com/terraform/language/modules/sources)
- [Version constraints](https://developer.hashicorp.com/terraform/language/expressions/version-constraints)
- [Refactoring with `moved` blocks](https://developer.hashicorp.com/terraform/language/modules/develop/refactoring)
- [Command: state mv](https://developer.hashicorp.com/terraform/cli/commands/state/mv)
- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
- [Module creation tutorial](https://developer.hashicorp.com/terraform/tutorials/modules/module-create)
