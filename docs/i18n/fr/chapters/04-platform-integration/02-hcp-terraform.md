# 4.2 HCP Terraform

HCP Terraform (anciennement Terraform Cloud) est la plateforme gérée de HashiCorp pour exécuter Terraform. Elle combine le state distant, les exécutions distantes, un registre de modules privé et l'application de politiques.

## Se connecter et exécuter

Ajoutez un bloc `cloud` à la configuration et connectez-vous :

```hcl
terraform {
  cloud {
    organization = "EXAMPLE_ORG"

    workspaces {
      name = "practice"
    }
  }
}
```

```bash
terraform login
terraform init
```

`plan` et `apply` s'exécutent désormais dans le workspace, pas sur votre portable, et le state y est stocké automatiquement.

## Fonctionnalités utiles

- Les workspaces pilotés par VCS exécutent un plan à chaque pull request et retiennent les applies pour approbation.
- Le state est stocké, versionné, verrouillé et chiffré par la plateforme.
- Des politiques Sentinel ou OPA peuvent bloquer un plan qui enfreint une règle.
- Les jeux de variables gardent les identifiants des providers hors du dépôt.

Utilisez un jeton d'API d'équipe ou une identité limitée au workspace avec le moindre privilège. Ne committez jamais un jeton `terraform login`.

## Références

- [HCP Terraform documentation](https://developer.hashicorp.com/terraform/cloud-docs)
- [The `cloud` block](https://developer.hashicorp.com/terraform/cli/cloud/settings)
- [VCS-driven runs](https://developer.hashicorp.com/terraform/cloud-docs/run/ui)
- [Policy enforcement](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement)
- [API tokens](https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/api-tokens)
