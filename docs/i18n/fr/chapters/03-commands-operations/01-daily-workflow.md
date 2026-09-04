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

Par défaut, `plan` rafraîchit aussi : il lit l'état actuel de chaque objet déjà suivi depuis la plateforme réelle avant de le comparer à votre configuration, et c'est ainsi que la dérive (drift) apparaît dans le résultat du plan. Sautez cette lecture avec `terraform plan -refresh=false` lorsque vous voulez délibérément comparer par rapport au dernier state enregistré plutôt qu'à la plateforme réelle (par exemple, pour ne voir que l'effet d'une modification de configuration sur un state volumineux, sans payer le coût d'un rafraîchissement complet).

## Automatiser le workflow

La même boucle s'exécute sans supervision en CI, avec quelques ajustements :

```bash
terraform fmt -check -recursive   # non-zero exit if any file is not formatted; writes nothing
terraform validate
terraform plan -out plan.tfplan -detailed-exitcode
```

`fmt -check -recursive` fait échouer le build au lieu de réécrire silencieusement les fichiers, et parcourt aussi les sous-répertoires. `plan -detailed-exitcode` renvoie `0` quand il n'y a aucun changement, `1` en cas d'erreur, et `2` quand le plan contient des changements — un pipeline peut utiliser le code de sortie `2` pour déclencher une étape d'approbation manuelle avant que `terraform apply plan.tfplan` ne s'exécute.

## Pourquoi relire le plan compte

`apply` sans fichier de plan enregistré relance `plan` et demande une approbation, mais ce plan est généré juste avant d'être appliqué — n'importe qui peut taper « yes » sans le lire. Appliquer un plan *enregistré* (`terraform apply plan.tfplan`) supprime cette tentation : les actions exactes ont déjà été figées au moment du `plan`, donc la relecture porte sur un artefact stable plutôt que sur une invite en direct, et ce même fichier peut être joint à une pull request ou à une exécution CI comme preuve de ce qui va se passer.

## Références

- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
- [Command: fmt](https://developer.hashicorp.com/terraform/cli/commands/fmt)
- [Command: validate](https://developer.hashicorp.com/terraform/cli/commands/validate)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Command: apply](https://developer.hashicorp.com/terraform/cli/commands/apply)
