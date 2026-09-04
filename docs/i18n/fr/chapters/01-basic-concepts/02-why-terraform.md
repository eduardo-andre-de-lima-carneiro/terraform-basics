# 1.2 Pourquoi Terraform

Terraform est un outil largement utilisé pour l'infrastructure as code. Il lit une configuration déclarative, dialogue avec les plateformes via des providers, et conserve un fichier de state pour savoir ce qu'il gère déjà.

## Ce qui le rend utile

- Un seul workflow (`init`, `plan`, `apply`) fonctionne avec de nombreux providers.
- Un plan montre les changements exacts avant que quoi que ce soit ne se produise.
- Le state permet à Terraform de ne mettre à jour et de ne supprimer que ce qu'il a créé.
- Les modules empaquettent la configuration pour la réutiliser entre équipes et environnements.

## Pratique

Pensez à deux plateformes qu'utilise votre équipe, par exemple un fournisseur cloud et un service DNS. Avec Terraform, les deux se gèrent avec les mêmes commandes et le même format de fichier. Notez là où cette cohérence ferait gagner du temps à votre équipe dès aujourd'hui.

## Références

- [What is Terraform? (HashiCorp)](https://developer.hashicorp.com/terraform/intro)
- [Terraform use cases](https://developer.hashicorp.com/terraform/intro/use-cases)
- [Terraform Registry: providers](https://registry.terraform.io/browse/providers)
- [Terraform workflow overview](https://developer.hashicorp.com/terraform/intro/core-workflow)
