# 1.3 Le modèle déclaratif de Terraform

Vous décrivez le résultat que vous voulez, pas les étapes pour l'atteindre. Terraform compare votre configuration avec le state actuel et la plateforme réelle, puis décide des actions nécessaires.

## Déclaratif contre impératif

Un script impératif dit « crée ceci, puis rattache cela ». Une configuration déclarative dit « ces ressources doivent exister avec ces paramètres ». Si une ressource correspond déjà, Terraform ne fait rien. Si elle a dérivé, Terraform propose une correction.

## Ce sont les providers qui font le travail

Terraform lui-même ne connaît aucune API cloud. Chaque provider traduit les blocs de ressources en appels d'API et reporte les résultats dans le state.

## Pratique

Lisez un court bloc de ressource et décrivez, en langage courant, l'état final qu'il déclare. Puis prédisez ce que ferait Terraform si cette ressource existait déjà, inchangée.

## Références

- [How Terraform works](https://developer.hashicorp.com/terraform/intro#how-does-terraform-work)
- [Providers (Terraform language)](https://developer.hashicorp.com/terraform/language/providers)
- [Resources overview](https://developer.hashicorp.com/terraform/language/resources)
- [Resource behavior and the dependency graph](https://developer.hashicorp.com/terraform/language/resources/behavior)
