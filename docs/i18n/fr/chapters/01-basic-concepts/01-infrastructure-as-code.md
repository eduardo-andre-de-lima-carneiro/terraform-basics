# 1.1 Infrastructure as code

L'infrastructure as code décrit les serveurs, les réseaux et les services dans des fichiers texte qui sont versionnés et relus. Elle permet à une équipe de recréer un environnement, de comparer des révisions, d'identifier des auteurs et de modifier l'infrastructure sans dépendre de la mémoire ou de clics manuels.

## Le problème qu'elle résout

Sans infrastructure as code, la configuration réelle n'existe que dans une console et dans la tête de quelques personnes. La reconstruire ou l'auditer relève de la devinette. Terraform conserve plutôt la configuration voulue dans des fichiers structurés.

## Pratique

Notez chaque étape manuelle que vous suivriez pour créer une petite ressource dans la console de votre cloud. Cette liste est la valeur qu'apporte l'infrastructure as code : elle transforme ces étapes en un fichier que vous pouvez relire, répéter et annuler.

## Références

- [What is Terraform? (HashiCorp)](https://developer.hashicorp.com/terraform/intro)
- [Terraform use cases](https://developer.hashicorp.com/terraform/intro/use-cases)
- [Infrastructure as code (AWS)](https://aws.amazon.com/what-is/iac/)
