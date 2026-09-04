# 2.2 Configurer les providers et les identifiants

Un provider a besoin de deux choses : une contrainte de version dans votre configuration et des identifiants dans votre environnement.

## Déclarer le provider

```hcl
terraform {
  required_version = ">= 1.6"

  required_providers {
    local = {
      source  = "hashicorp/local"
      version = "~> 2.5"
    }
  }
}

provider "local" {}
```

`terraform init` lit ce bloc et télécharge le provider épinglé dans le répertoire de travail.

## Fournir les identifiants en toute sécurité

La plupart des fournisseurs cloud lisent les identifiants depuis des variables d'environnement ou un fichier d'identifiants partagé, jamais depuis les fichiers `.tf`. Gardez les secrets hors de la configuration et hors du contrôle de version :

```bash
export EXAMPLE_API_TOKEN="REPLACE_ME"
```

## Pratique

Créez un bloc `terraform` qui requiert le provider `local`, exécutez `terraform init` et vérifiez qu'un fichier `.terraform.lock.hcl` apparaît. Ouvrez-le et remarquez qu'il épingle la version du provider et ses sommes de contrôle.

## Références

- [Provider requirements](https://developer.hashicorp.com/terraform/language/providers/requirements)
- [Provider configuration](https://developer.hashicorp.com/terraform/language/providers/configuration)
- [Dependency lock file](https://developer.hashicorp.com/terraform/language/files/dependency-lock)
- [hashicorp/local provider docs](https://registry.terraform.io/providers/hashicorp/local/latest/docs)
- [Version constraints](https://developer.hashicorp.com/terraform/language/expressions/version-constraints)
