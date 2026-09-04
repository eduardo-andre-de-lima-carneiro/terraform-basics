# 1.4 Le modèle mental de Terraform

Pensez en trois endroits :

1. Configuration : les fichiers `.tf` décrivant l'état souhaité.
2. State : ce que Terraform a enregistré sur les ressources qu'il gère.
3. Infrastructure réelle : ce qui existe réellement sur la plateforme.

Le déroulé de base est `écrire la configuration -> terraform plan -> terraform apply`. `terraform plan` compare les trois endroits et affiche la différence ; ce devrait être votre commande de diagnostic la plus fréquente. Exécutez-la avant chaque apply.

## Pourquoi Terraform a besoin d'un troisième endroit

Ce serait plus simple si Terraform comparait seulement la configuration à l'infrastructure réelle, mais deux problèmes l'en empêchent. D'abord, certaines informations ne peuvent pas être récupérées depuis la seule plateforme : quand vous supprimez une ressource de la configuration, Terraform doit savoir comment la retirer et dans quel ordre, et une API cloud n'expose pas cette relation. Ensuite, interroger tous les attributs de chaque ressource à chaque commande ne passe pas à l'échelle : pour un grand environnement, cela signifierait des appels d'API constants, de la latence et des limitations de débit. Le state résout les deux : c'est le cache et la carte qui relient un bloc de ressource comme `aws_instance.web` à un objet réel, telle l'instance `i-abcd1234`, sans lequel, comme le dit HashiCorp, « Terraform is unable to function » (Terraform ne peut pas fonctionner).

## Parcourir la boucle avec un exemple pratique

```bash
terraform apply    # la configuration correspond maintenant au state, qui correspond à l'infrastructure réelle
# ...vous modifiez main.tf, en changeant un argument...
terraform plan      # compare les trois endroits ; montre exactement un attribut qui change
terraform apply     # l'infrastructure réelle et le state rattrapent la nouvelle configuration
```

Si à la place vous changez quelque chose à la main dans la console du provider, seuls le state et l'infrastructure réelle se désynchronisent ; le `terraform plan` suivant rafraîchit par défaut le state depuis la plateforme réelle et signale la dérive, généralement sous la forme d'une proposition de remettre la ressource en conformité avec la configuration.

## Ce qui vit où

| Question | La réponse vit dans | Comment vérifier |
| --- | --- | --- |
| Que est-ce que je veux voir exister ? | Configuration | Lire les fichiers `.tf` |
| Que croit Terraform qui existe ? | State | `terraform show`, `terraform state list` |
| Qu'est-ce qui existe réellement sur la plateforme ? | Infrastructure réelle | La console ou l'API du provider, ou un `terraform plan` récent |
| Qu'est-ce qui va changer si j'applique maintenant ? | La différence entre les trois | `terraform plan` |

## Pièges courants

- **Traiter le state comme jetable.** Supprimer ou modifier à la main le fichier de state ne supprime pas les ressources réelles ; cela fait seulement en sorte que Terraform les oublie, ce qui mène généralement à des applies en échec ou à des ressources dupliquées.
- **Appliquer sans plan à jour.** Sauter `plan`, ou appliquer un plan qui n'est plus à jour, revient à faire confiance à une comparaison périmée des trois endroits.
- **Supposer que le state est optionnel en solo.** Même seul, un apply interrompu peut laisser la configuration, le state et la réalité désynchronisés ; les techniques de récupération du Chapitre 3 existent précisément pour cela.
- **Oublier que l'« infrastructure réelle » peut changer sans Terraform.** Tout ce qu'un autre ingénieur, un autre script ou une session console modifie n'apparaîtra qu'au prochain `plan` rafraîchissant le state.

## Pratique

Appliquez une petite configuration, puis modifiez à la main cette même ressource en dehors de Terraform (par exemple, modifiez directement le contenu d'un fichier local plutôt que via la configuration). Exécutez `terraform plan` et identifiez lequel des trois endroits a bougé, et lesquels des deux autres restent en accord.

## Références

- [State (Terraform language)](https://developer.hashicorp.com/terraform/language/state)
- [Purpose of Terraform state](https://developer.hashicorp.com/terraform/language/state/purpose)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Command: refresh](https://developer.hashicorp.com/terraform/cli/commands/refresh)
- [Core workflow](https://developer.hashicorp.com/terraform/intro/core-workflow)
