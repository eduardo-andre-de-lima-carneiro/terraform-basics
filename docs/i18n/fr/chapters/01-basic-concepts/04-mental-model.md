# 1.4 Le modèle mental de Terraform

Raisonnez en trois endroits :

1. Configuration : les fichiers `.tf` qui décrivent l'état souhaité.
2. State : ce que Terraform a enregistré sur les ressources qu'il gère.
3. Infrastructure réelle : ce qui existe effectivement sur la plateforme.

Le flux de base est `write config -> terraform plan -> terraform apply`. `terraform plan` compare les trois endroits et montre la différence ; ce devrait être votre commande de diagnostic la plus fréquente. Exécutez-la avant chaque apply.

## Références

- [State (Terraform language)](https://developer.hashicorp.com/terraform/language/state)
- [Purpose of Terraform state](https://developer.hashicorp.com/terraform/language/state/purpose)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Core workflow](https://developer.hashicorp.com/terraform/intro/core-workflow)
