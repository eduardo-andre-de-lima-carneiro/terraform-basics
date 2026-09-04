# 1.3 Le modèle déclaratif de Terraform

Vous décrivez le résultat que vous voulez, pas les étapes pour y arriver. Terraform compare votre configuration au state actuel et à la plateforme réelle, puis décide des actions nécessaires.

## Déclaratif contre impératif

Un script impératif dit « crée ceci, puis attache cela ». Une configuration déclarative dit « ces ressources doivent exister avec ces réglages ». Si une ressource correspond déjà, Terraform ne fait rien. Si elle a dérivé, Terraform propose une correction.

```bash
# Impératif : vous précisez chaque étape, dans l'ordre, et le rejouer à l'aveugle peut échouer ou dupliquer le travail
aws ec2 create-vpc ...
aws ec2 create-subnet ...
aws ec2 create-security-group ...
```

```hcl
# Déclaratif : vous précisez l'état final ; Terraform détermine les étapes et leur ordre
resource "aws_vpc" "main" { cidr_block = "10.0.0.0/16" }
resource "aws_subnet" "app" { vpc_id = aws_vpc.main.id, cidr_block = "10.0.1.0/24" }
```

Le bloc déclaratif ne dit jamais « crée d'abord le VPC ». Terraform le déduit parce que la configuration du subnet référence `aws_vpc.main.id`.

## Les providers font le travail

Terraform lui-même ne connaît aucune API cloud. Chaque provider traduit les blocs de ressources en appels d'API et rapporte les résultats dans le state.

## Comment Terraform décide de l'ordre

Chaque référence entre ressources — `aws_vpc.main.id` dans l'exemple ci-dessus — devient une arête d'un graphe de dépendances. Terraform construit ce graphe à partir de votre configuration, vérifie qu'il ne contient pas de cycle, puis le parcourt de sorte qu'une ressource n'est créée, modifiée ou détruite qu'une fois que tout ce dont elle dépend est terminé. Les ressources qui ne dépendent pas les unes des autres peuvent s'exécuter en même temps, jusqu'à 10 en parallèle par défaut, ce qui explique pourquoi un modèle déclaratif tend à s'appliquer plus vite qu'un script impératif équivalent à mesure que votre configuration grandit. Vous pouvez inspecter le graphe vous-même avec `terraform graph`, et forcer une arête explicite avec `depends_on` lorsqu'une dépendance existe sans apparaître comme une référence d'attribut.

Détruire des ressources parcourt un graphe apparenté mais distinct, car l'ordre sûr pour supprimer les choses est souvent l'inverse de l'ordre utilisé pour les créer.

## Où le modèle déclaratif montre ses limites

Le modèle n'est pas exempt de compromis :

- **Certaines étapes restent vraiment impératives.** Une migration de base de données, un import de données ponctuel, ou un appel d'API sensible à l'ordre ne s'insère pas proprement dans « cette ressource doit exister ». La recommandation de HashiCorp sur les provisioners — l'échappatoire pour exécuter un script à la création ou à la destruction — est de les traiter comme un dernier recours : vérifiez d'abord s'il existe un moyen natif au provider de faire la même chose, car Terraform ne peut pas modéliser ce qu'un provisioner fait réellement, ni savoir s'il a réussi comme il le fait pour une ressource.
- **La plateforme réelle peut ne pas être d'accord avec le graphe.** Les API cloud sont parfois à cohérence éventuelle, si bien qu'une ressource peut signaler un succès avant d'être pleinement utilisable ailleurs, ce qui se manifeste parfois par une erreur de dépendance alors même que la configuration était correcte.
- **Déclaratif ne veut pas dire automatique.** Vous choisissez toujours les ressources, les arguments et les limites des modules ; Terraform n'automatise que l'ordonnancement et le calcul de différence, pas la conception.

## Pratique

Lisez un court bloc de ressource et décrivez, en langage simple, l'état final qu'il déclare. Prédisez ensuite ce que ferait Terraform si cette ressource existait déjà sans changement. Enfin, exécutez `terraform graph` sur une petite configuration comportant deux ressources liées et identifiez l'arête correspondant à la référence entre elles.

## Références

- [How Terraform works](https://developer.hashicorp.com/terraform/intro#how-does-terraform-work)
- [Providers (Terraform language)](https://developer.hashicorp.com/terraform/language/providers)
- [Resources overview](https://developer.hashicorp.com/terraform/language/resources)
- [Resource behavior and the dependency graph](https://developer.hashicorp.com/terraform/language/resources/behavior)
- [The dependency graph (internals)](https://developer.hashicorp.com/terraform/internals/graph)
- [Command: graph](https://developer.hashicorp.com/terraform/cli/commands/graph)
- [The `depends_on` meta-argument](https://developer.hashicorp.com/terraform/language/meta-arguments/depends_on)
- [Provisioners: a last resort](https://developer.hashicorp.com/terraform/language/resources/provisioners/syntax)
