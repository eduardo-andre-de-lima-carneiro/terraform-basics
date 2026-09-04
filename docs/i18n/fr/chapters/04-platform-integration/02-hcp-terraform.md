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

## À quoi ressemble une exécution pilotée par VCS

Connectez un workspace à un dépôt (à la place du bloc `cloud` ci-dessus, ou en plus) et chaque pull request reçoit un **plan spéculatif** : une exécution en mode plan seul qui ne peut pas être appliquée, publiée sur la pull request comme un status check afin que les relecteurs voient l'effet avant la fusion. Une exécution sur la branche suivie va plus loin et traverse des étapes dans l'ordre : plan, puis estimation des coûts si l'organisation l'a activée, puis un éventuel contrôle de politique, puis apply. L'apply s'arrête normalement en attendant qu'une personne sélectionne **Confirm & Apply** dans l'interface, sauf si le workspace est configuré en auto-apply.

## Fonctionnalités utiles

- Les workspaces pilotés par VCS exécutent un plan à chaque pull request et retiennent les applies pour approbation.
- Le state est stocké, versionné, verrouillé et chiffré par la plateforme.
- Des politiques Sentinel ou OPA peuvent bloquer un plan qui enfreint une règle.
- Les jeux de variables gardent les identifiants des providers hors du dépôt.
- L'estimation des coûts (désactivée par défaut ; un propriétaire de l'organisation doit l'activer) affiche le delta de coût mensuel estimé pour les ressources AWS, GCP et Azure, comme une étape entre le plan et l'apply.
- Au lieu de `workspaces { name = "..." }`, une configuration peut cibler des workspaces par `project` ou par `tags`, afin qu'un seul bloc corresponde à plusieurs workspaces.

Utilisez un jeton d'API d'équipe ou une identité limitée au workspace avec le moindre privilège. Ne committez jamais un jeton `terraform login`.

## Références

- [HCP Terraform documentation](https://developer.hashicorp.com/terraform/cloud-docs)
- [The `cloud` block](https://developer.hashicorp.com/terraform/cli/cloud/settings)
- [VCS-driven runs](https://developer.hashicorp.com/terraform/cloud-docs/run/ui)
- [Cost estimation](https://developer.hashicorp.com/terraform/cloud-docs/cost-estimation)
- [Policy enforcement](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement)
- [API tokens](https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/api-tokens)
