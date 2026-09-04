# 3.6 Exercices pratiques

Réalisez-les dans un répertoire temporaire. Utilisez le provider `null` afin que rien de réel ne soit créé :

```hcl
terraform {
  required_providers {
    null = {
      source  = "hashicorp/null"
      version = "~> 3.2"
    }
  }
}

resource "null_resource" "example" {}
```

1. Exécutez `terraform init`, puis `terraform plan -out plan.tfplan` et `terraform apply plan.tfplan`.
2. Ajoutez un argument `triggers` à la ressource, enregistrez un nouveau plan avec `-out`, et appliquez exactement ce fichier de plan.
3. Déplacez la ressource dans un module enfant `./modules/example`, ajoutez un bloc `moved` pour le changement d'adresse, et vérifiez que `terraform plan` ne signale aucun changement.
4. Entraînez-vous à la récupération : exécutez `terraform apply -replace=null_resource.example`, puis `terraform state rm null_resource.example` suivi de `terraform import null_resource.example demo-id`.
5. Exécutez `terraform fmt -check -recursive` et `terraform plan -detailed-exitcode` ; notez le code de sortie avec et sans changement en attente (`echo $?` juste après chaque commande).
6. Remplacez le `state rm` de l'étape 4 par un bloc `removed` (`lifecycle { destroy = false }`) et vérifiez que `terraform plan` affiche la même suppression pour relecture avant de l'appliquer. Écrivez ensuite un bloc `import` à la place de la commande `terraform import`, et exécutez `terraform plan -generate-config-out=generated.tf` pour voir Terraform rédiger le bloc `resource` à votre place.
7. Créez un second répertoire `modules-remote-demo` avec son propre `null_resource` et un `output`. Depuis le répertoire d'origine, ajoutez une source de données `terraform_remote_state` (`backend = "local"`, pointant vers le `terraform.tfstate` de l'autre répertoire) et référencez sa sortie dans une valeur `triggers`.

Pour chaque exercice, notez la commande utilisée, le state avant celle-ci, et le résultat affiché par `terraform plan` ou `terraform show`.

## Références

- [hashicorp/null provider docs](https://registry.terraform.io/providers/hashicorp/null/latest/docs)
- [null_resource](https://registry.terraform.io/providers/hashicorp/null/latest/docs/resources/resource)
- [Import usage](https://developer.hashicorp.com/terraform/cli/import/usage)
- [Get started tutorials](https://developer.hashicorp.com/terraform/tutorials)
- [Generating configuration with `import` blocks](https://developer.hashicorp.com/terraform/language/import/generating-configuration)
- [`terraform_remote_state` data source](https://developer.hashicorp.com/terraform/language/state/remote-state-data)
