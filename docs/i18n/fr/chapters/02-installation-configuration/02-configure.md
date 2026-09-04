# 2.2 Configurer les providers et les identifiants

Un provider a besoin de deux choses : une contrainte de version dans votre configuration, et des identifiants dans votre environnement.

## Déclarez le provider

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

La plupart des providers cloud lisent les identifiants depuis des variables d'environnement ou un fichier d'identifiants partagé, jamais depuis les fichiers `.tf`. Gardez les secrets hors de la configuration et hors du contrôle de version :

```bash
export EXAMPLE_API_TOKEN="REPLACE_ME"
```

## Variables d'identifiants pour les providers courants

Chaque provider suit la méthode d'authentification standard de sa propre plateforme plutôt qu'une méthode spécifique à Terraform, si bien que les mêmes variables fonctionnent aussi avec la CLI ou le SDK de cette plateforme :

| Provider | Variables d'environnement typiques | Alternative |
| --- | --- | --- |
| `hashicorp/aws` | `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN` | Fichier d'identifiants partagé (`~/.aws/credentials`) plus `AWS_PROFILE` |
| `hashicorp/azurerm` | `ARM_CLIENT_ID`, `ARM_CLIENT_SECRET`, `ARM_SUBSCRIPTION_ID`, `ARM_TENANT_ID` | `az login` interactif, que Terraform peut utiliser directement |
| `hashicorp/google` | `GOOGLE_CREDENTIALS` (JSON en ligne) ou `GOOGLE_APPLICATION_CREDENTIALS` (chemin vers un fichier de clé) | Application Default Credentials via `gcloud auth application-default login` |

Préférez une identité de courte durée (un rôle assumé, une workload identity, ou une connexion interactive par CLI) à une clé statique de longue durée chaque fois que la plateforme le propose.

## Pratique

Créez un bloc `terraform` qui requiert le provider `local`, exécutez `terraform init`, et confirmez qu'un fichier `.terraform.lock.hcl` apparaît. Ouvrez-le et notez qu'il épingle la version du provider et les sommes de contrôle.

## Références

- [Provider requirements](https://developer.hashicorp.com/terraform/language/providers/requirements)
- [Provider configuration](https://developer.hashicorp.com/terraform/language/providers/configuration)
- [Dependency lock file](https://developer.hashicorp.com/terraform/language/files/dependency-lock)
- [hashicorp/local provider docs](https://registry.terraform.io/providers/hashicorp/local/latest/docs)
- [Version constraints](https://developer.hashicorp.com/terraform/language/expressions/version-constraints)
- [AWS CLI environment variables (same variables the AWS provider reads)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)
- [Authenticate Terraform to Azure with a service principal](https://learn.microsoft.com/en-us/azure/developer/terraform/authenticate-to-azure-with-service-principle)
- [Google: Application Default Credentials](https://cloud.google.com/docs/authentication/application-default-credentials)
