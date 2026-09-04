# 3.4 State distant et synchronisation

Dès que plusieurs personnes ou pipelines exécutent Terraform, le state doit résider dans un backend partagé et être protégé contre les écritures simultanées.

## Configurer et migrer

```bash
terraform init -migrate-state
```

Après avoir ajouté un bloc `backend`, cette commande déplace le state local existant vers le backend distant et demande confirmation avant d'écraser quoi que ce soit. Relancer `terraform init` après avoir modifié la configuration d'un backend *déjà existant* (et non en ajouter un) exige `-migrate-state` ou `-reconfigure` — Terraform ne choisit pas l'un des deux silencieusement à votre place.

| Flag | Ce que ça fait |
| --- | --- |
| `-migrate-state` | Copie le state actuel vers la nouvelle configuration de backend |
| `-reconfigure` | Bascule vers la nouvelle configuration de backend sans migrer le state (repart de zéro ; l'ancien state reste où il était) |
| `-force-copy` | Identique à `-migrate-state`, mais répond « oui » automatiquement à chaque invite de migration — pour un `init` scripté/non interactif |

## Verrouillage et rafraîchissement

- Les backends pris en charge posent un verrou pendant `plan` et `apply` afin que deux exécutions ne puissent pas corrompre le state ; tous les backends ne prennent pas en charge le verrouillage, vérifiez donc la documentation du backend concerné avant de vous y fier pour une équipe.
- `terraform plan` rafraîchit par défaut le state depuis la plateforme réelle, révélant la dérive (drift).
- Ne modifiez jamais le fichier de state à la main ; utilisez les sous-commandes `terraform state`.

**`terraform force-unlock <lock-id>`** supprime un verrou bloqué sans toucher à l'infrastructure. Portée : cela n'affecte que le verrou du state de cette configuration, pas les ressources elles-mêmes. Ne l'utilisez qu'après avoir confirmé que le processus ayant posé le verrou est réellement arrêté (un job CI planté, une exécution locale tuée) — Terraform affiche le lock ID et son détenteur en cas de conflit de verrou. Chemin de récupération : si vous forcez le déverrouillage alors que l'exécution de quelqu'un d'autre est réellement encore en cours, les deux peuvent alors écrire le state en même temps et le corrompre ; si cela arrive, restaurez le dernier state valide grâce au versionnage propre à votre backend (par exemple le versionnage d'un bucket S3) ou à partir d'une sortie de `terraform state pull` sauvegardée au préalable.

## Lire les sorties d'une autre configuration

Deux configurations Terraform ont souvent besoin de partager des informations — l'ID d'un sous-réseau d'une stack réseau, par exemple, consommé par une stack applicative — sans les fusionner en un seul module racine. La source de données `terraform_remote_state` lit le state d'une autre configuration directement depuis son backend :

```hcl
data "terraform_remote_state" "network" {
  backend = "local"
  config = {
    path = "../network/terraform.tfstate"
  }
}

resource "local_file" "app_config" {
  filename = "app.conf"
  content  = "subnet=${data.terraform_remote_state.network.outputs.subnet_id}"
}
```

Remplacez `backend = "local"` et son `path` par le backend et la map `config` réellement utilisés par l'autre configuration (par exemple `backend = "s3"` avec `bucket`/`key`/`region`). Deux points à garder en tête :

- Seules les valeurs `output` du module racine de l'autre configuration sont lisibles — rien de ce qui est imbriqué dans ses modules enfants, à moins que ce module ne réexpose ses sorties à la racine.
- Quiconque peut lire ces sorties peut atteindre l'intégralité de l'instantané du state de la même façon ; ceci ne restreint donc que *ce que vous référencez*, pas qui peut voir le state sous-jacent — gardez les secrets hors des outputs comme vous les gardez hors de tout le reste dans le state.

## Pratique

Décrivez, en deux phrases, ce qui pourrait mal tourner si deux ingénieurs exécutaient `terraform apply` en même temps sur un fichier de state local. Puis expliquez comment un backend distant avec verrouillage l'empêche.

Créez deux répertoires locaux, `network` et `app`, chacun avec sa propre configuration du provider `local` ; donnez à `network` une valeur `output`, puis lisez-la depuis `app` avec `terraform_remote_state` et vérifiez que `terraform apply` dans `app` récupère bien cette valeur.

## Références

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [State locking](https://developer.hashicorp.com/terraform/language/state/locking)
- [Command: init (backend initialization)](https://developer.hashicorp.com/terraform/cli/commands/init#backend-initialization)
- [Managing state in automation](https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform)
- [Command: force-unlock](https://developer.hashicorp.com/terraform/cli/commands/force-unlock)
- [`terraform_remote_state` data source](https://developer.hashicorp.com/terraform/language/state/remote-state-data)
