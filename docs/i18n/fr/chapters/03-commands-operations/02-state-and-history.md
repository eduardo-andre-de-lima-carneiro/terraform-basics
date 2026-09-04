# 3.2 State et historique des changements

Le state est la façon dont Terraform se souvient de ce qu'il gère. Il fait correspondre chaque bloc de ressource à un objet réel et stocke les derniers attributs connus de cet objet.

## Inspecter le state

```bash
terraform state list
terraform show
terraform state show <resource address>
```

`state list` nomme chaque ressource suivie, `show` affiche l'intégralité des valeurs enregistrées, et `state show` se concentre sur une seule. `state list` accepte un motif d'adresse de ressource pour filtrer, ce qui compte dès qu'un state contient des milliers de ressources réparties dans de nombreux modules — par exemple, `terraform state list module.notes` ne liste que les ressources de ce module.

## Inspecter un plan enregistré de la même façon

`terraform show` ne se limite pas au state : pointé vers un fichier enregistré avec `plan -out`, il le restitue dans le même format lisible par un humain, et `terraform show -json` produit une version lisible par une machine du state ou d'un fichier de plan, à destination d'outils externes. Comme `-json` affiche les valeurs sensibles en clair, traitez cette sortie avec la même prudence que le fichier de state lui-même.

## Avancé : lire et écrire le fichier de state brut

- `terraform state pull` affiche le state actuel en JSON brut sur la sortie standard — utile pour du scripting ou pour archiver un instantané, mais en lecture seule et sans danger.
- `terraform state push` téléverse un fichier de state local vers le backend configuré, en écrasant ce qui s'y trouve. **C'est destructeur** : cela remplace l'intégralité du state distant de cette configuration, pas seulement une ressource, et un décalage accepté silencieusement peut corrompre la prochaine exécution d'un coéquipier. Terraform refuse le push par défaut si la destination a un lineage différent ou un numéro de série plus récent ; ne passez `-force` pour contourner cette vérification que si vous êtes certain que la copie de destination est celle à écarter, et conservez une copie du fichier que vous êtes sur le point d'écraser (ou le state actuel de la destination, récupéré au préalable avec `pull`) pour pouvoir annuler le push.

Ne recourez à `state pull`/`state push` que lorsqu'il n'existe aucun autre moyen de réparer le state (par exemple, réparer à la main un JSON corrompu hors ligne) ; pour les changements du quotidien, préférez `terraform state list` / `show` / `mv` / `rm` et `import`, puisque chacun ne touche qu'une seule ressource à la fois et reste relisible.

## Où vit l'historique

Terraform ne conserve pas de journal complet des changements qui lui soit propre. L'historique consultable, c'est votre historique de contrôle de version des fichiers `.tf`, plus l'historique des exécutions dans la plateforme qui les applique. Committez les changements de configuration en petites étapes décrites afin que le « pourquoi » reste récupérable.

## Pratique

Appliquez une petite configuration, exécutez `terraform state list`, puis changez un attribut et exécutez `terraform plan`. Remarquez comment le plan explique la différence entre le state enregistré et le nouvel état souhaité.

## Références

- [Manipulating state](https://developer.hashicorp.com/terraform/cli/state)
- [Command: state list](https://developer.hashicorp.com/terraform/cli/commands/state/list)
- [Command: show](https://developer.hashicorp.com/terraform/cli/commands/show)
- [Inspecting state](https://developer.hashicorp.com/terraform/cli/state/inspect)
- [Command: state pull](https://developer.hashicorp.com/terraform/cli/commands/state/pull)
- [Command: state push](https://developer.hashicorp.com/terraform/cli/commands/state/push)
