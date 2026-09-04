# 2.1 Installer Terraform

Terraform est un binaire unique. Installez-le avec le gestionnaire de paquets de votre plateforme ou téléchargez-le depuis les releases officielles.

## Options courantes

```bash
# macOS (Homebrew)
brew tap hashicorp/tap
brew install hashicorp/tap/terraform

# Debian and Ubuntu use the official HashiCorp apt repository
# Windows uses winget or Chocolatey
```

## Vérifier l'installation

```bash
terraform version
```

La commande affiche la version de la CLI et, une fois un répertoire de travail initialisé, les versions des providers utilisées. Si la version est ancienne, mettez à jour avant de continuer pour que les exemples se comportent comme décrit.

## Pratique

Installez Terraform, exécutez `terraform version` et notez la chaîne de version. Exécutez `terraform -help` et parcourez la liste des sous-commandes que vous rencontrerez au chapitre 3.

## Références

- [Install Terraform](https://developer.hashicorp.com/terraform/install)
- [Official packaging repository (releases.hashicorp.com)](https://releases.hashicorp.com/terraform/)
- [Command: version](https://developer.hashicorp.com/terraform/cli/commands/version)
- [Basic CLI features](https://developer.hashicorp.com/terraform/cli/commands)
