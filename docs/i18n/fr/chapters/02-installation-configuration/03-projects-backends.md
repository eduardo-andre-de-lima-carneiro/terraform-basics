# 2.3 Répertoires de travail et backends

Un répertoire de travail Terraform est n'importe quel dossier contenant des fichiers `.tf`. Quand vous exécutez `terraform init`, Terraform prépare ce dossier : il télécharge les providers et configure le backend.

## Backend local

Par défaut, le state est écrit dans `terraform.tfstate` à l'intérieur du répertoire de travail. C'est très bien pour apprendre et pour des expérimentations en solo.

## Backend distant

Un backend distant stocke le state dans un service partagé, afin qu'une équipe ne s'écrase pas mutuellement et que les secrets ne restent pas sur un ordinateur portable :

```hcl
terraform {
  backend "s3" {
    bucket       = "example-tfstate"
    key          = "practice/terraform.tfstate"
    region       = "us-east-1"
    use_lockfile = true # verrouillage natif S3 ; aucune table DynamoDB requise
  }
}
```

Remplacez chaque valeur par des placeholders que vous contrôlez. Changer de backend nécessite un nouveau `terraform init`, et Terraform propose de migrer le state existant.

## Choisir un backend

Terraform fournit des backends pour de nombreuses cibles, pas seulement S3. Quelques exemples courants :

| Backend | Usage typique |
| --- | --- |
| `local` | Par défaut ; une seule machine, apprentissage, expérimentations en solo |
| `s3` | Équipes centrées sur AWS ; state dans un bucket S3, avec verrouillage natif S3 (`use_lockfile`) |
| `azurerm` | Équipes centrées sur Azure ; state dans un conteneur d'un compte de stockage |
| `gcs` | Équipes centrées sur Google Cloud ; state dans un bucket Cloud Storage |
| `remote` / le bloc `cloud` | HCP Terraform ou Terraform Enterprise ; state, verrouillage et historique des runs entièrement gérés (voir le Chapitre 4) |

## Verrouillage

Le verrouillage empêche deux exécutions d'écrire le state en même temps. L'option `use_lockfile = true` du backend S3 est l'approche actuellement recommandée ; le verrouillage plus ancien basé sur une table DynamoDB est obsolète. Quel que soit le backend choisi, confirmez qu'il verrouille avant de vous y fier pour une équipe.

## Pratique

Initialisez un répertoire avec le backend local par défaut et repérez `terraform.tfstate`. Ajoutez-le à `.gitignore` et notez pourquoi commiter le state n'est pas sûr : il peut contenir des secrets et provoque des conflits.

## Références

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [Backend types](https://developer.hashicorp.com/terraform/language/backend#available-backends)
- [State: sensitive data](https://developer.hashicorp.com/terraform/language/state/sensitive-data)
- [S3 backend](https://developer.hashicorp.com/terraform/language/backend/s3)
- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
