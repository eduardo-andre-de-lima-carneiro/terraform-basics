# 3.1 Le workflow quotidien

Exécutez `terraform init` une fois dans un nouveau répertoire de travail (ou après avoir changé de providers ou de backend), puis utilisez une boucle courte et observable :

```bash
terraform init      # first time in the directory, or after provider/backend changes
terraform fmt
terraform validate
terraform plan -out plan.tfplan
terraform apply plan.tfplan
```

`fmt` normalise le style, `validate` vérifie que la configuration est bien formée, et `plan -out` enregistre le changement exact pour que `apply` n'exécute que ce que vous avez relu. Lire le plan avant d'appliquer est l'habitude qui évite les changements-surprises.

## Références

- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [Command: validate](https://developer.hashicorp.com/terraform/cli/commands/validate)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Command: apply](https://developer.hashicorp.com/terraform/cli/commands/apply)
