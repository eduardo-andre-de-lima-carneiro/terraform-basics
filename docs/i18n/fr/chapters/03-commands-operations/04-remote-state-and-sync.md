# 3.4 State distant et synchronisation

Dès que plusieurs personnes ou pipelines exécutent Terraform, le state doit résider dans un backend partagé et être protégé contre les écritures simultanées.

## Configurer et migrer

```bash
terraform init -migrate-state
```

Après avoir ajouté un bloc `backend`, cette commande déplace le state local existant vers le backend distant et demande confirmation avant d'écraser quoi que ce soit.

## Verrouillage et rafraîchissement

- Les backends pris en charge posent un verrou pendant `plan` et `apply` afin que deux exécutions ne puissent pas corrompre le state.
- `terraform plan` rafraîchit par défaut le state depuis la plateforme réelle, révélant la dérive (drift).
- Ne modifiez jamais le fichier de state à la main ; utilisez les sous-commandes `terraform state`.

## Pratique

Décrivez, en deux phrases, ce qui pourrait mal tourner si deux ingénieurs exécutaient `terraform apply` en même temps sur un fichier de state local. Puis expliquez comment un backend distant avec verrouillage l'empêche.

## Références

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [State locking](https://developer.hashicorp.com/terraform/language/state/locking)
- [Command: init (backend initialization)](https://developer.hashicorp.com/terraform/cli/commands/init#backend-initialization)
- [Managing state in automation](https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform)
