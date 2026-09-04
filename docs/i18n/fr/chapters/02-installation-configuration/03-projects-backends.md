# 2.3 Répertoires de travail et backends

Un répertoire de travail Terraform est n'importe quel dossier qui contient des fichiers `.tf`. Quand vous exécutez `terraform init`, Terraform prépare ce dossier : il télécharge les providers et configure le backend.

## Backend local

Par défaut, le state est écrit dans `terraform.tfstate` dans le répertoire de travail. C'est suffisant pour l'apprentissage et pour des expériences en solo.

## Backend distant

Un backend distant stocke le state dans un service partagé afin qu'une équipe ne s'écrase pas mutuellement et que les secrets ne restent pas sur un portable :

```hcl
terraform {
  backend "s3" {
    bucket = "example-tfstate"
    key    = "practice/terraform.tfstate"
    region = "us-east-1"
  }
}
```

Remplacez chaque valeur par des placeholders que vous contrôlez. Changer de backend nécessite de relancer `terraform init`, et Terraform propose de migrer le state existant.

## Pratique

Initialisez un répertoire avec le backend local par défaut et localisez `terraform.tfstate`. Ajoutez-le à `.gitignore` et notez pourquoi committer le state n'est pas sûr : il peut contenir des secrets et provoque des conflits.

## Références

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [State: sensitive data](https://developer.hashicorp.com/terraform/language/state/sensitive-data)
- [S3 backend](https://developer.hashicorp.com/terraform/language/backend/s3)
- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
