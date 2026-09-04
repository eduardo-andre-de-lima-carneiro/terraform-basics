# 1.2 Pourquoi Terraform

Terraform est un outil largement utilisé pour l'infrastructure as code. Il lit une configuration déclarative, dialogue avec les plateformes via des providers, et conserve un fichier de state pour savoir ce qu'il gère déjà.

## Ce qui le rend utile

- Un seul workflow (`init`, `plan`, `apply`) fonctionne sur de nombreux providers.
- Un plan montre les changements exacts avant que rien ne se produise.
- Le state permet à Terraform de mettre à jour et de supprimer uniquement ce qu'il a créé.
- Les modules regroupent de la configuration réutilisable entre équipes et environnements.

## Terraform face aux autres outils

Terraform est l'un des nombreux outils d'automatisation de l'infrastructure, et chacun optimise pour un usage différent. Le tableau ci-dessous compare les outils que vous êtes le plus susceptible de rencontrer dans une équipe qui a déjà de l'automatisation en place.

| Outil | Catégorie | Approche | Langage de configuration | Modèle d'exécution | Périmètre | Suivi du state |
| --- | --- | --- | --- | --- | --- | --- |
| **Terraform** | Provisionnement (IaC) | Déclaratif, état souhaité | HCL (ou JSON) | Sans agent ; vous ou un pipeline exécutez `plan`/`apply` | De nombreux providers : plateformes cloud, SaaS, on-prem, Kubernetes | Un fichier de state explicite enregistre chaque ressource gérée |
| **Ansible** | Gestion de configuration | Playbooks surtout procéduraux, avec des modules idempotents | YAML | Sans agent ; envoie des modules par SSH/WinRM depuis un nœud de contrôle | Configurer et mettre à jour des machines déjà existantes ; peut aussi appeler des API cloud pour provisionner | Pas de fichier de state dédié ; chaque exécution inspecte le système réel |
| **Chef / Puppet** | Gestion de configuration | Ressources déclaratives à l'intérieur d'un DSL (Ruby pour Chef, DSL propre à Puppet) | Ruby (Chef) / DSL Puppet | Basé sur un agent, classiquement un modèle pull depuis un serveur central | Configurer et mettre à jour des machines déjà existantes | Pas de fichier de state d'infrastructure ; les agents réconcilient la machine locale à chaque exécution |
| **AWS CloudFormation** | Provisionnement (IaC) | Déclaratif, état souhaité | Modèles JSON ou YAML | Sans agent ; un service AWS managé exécute le déploiement | AWS uniquement | Le state est la stack CloudFormation, gérée par AWS |
| **Pulumi** | Provisionnement (IaC) | État souhaité, exprimé avec du code généraliste | TypeScript, Python, Go, C#, Java ou YAML | Sans agent ; un moteur CLI exécute votre programme et applique le diff | De nombreux providers, un périmètre proche de celui de Terraform | Fichier de state, stocké localement ou dans Pulumi Cloud |

## Comment lire cette comparaison

- **Provisionnement contre gestion de configuration.** Terraform, CloudFormation et Pulumi créent et modifient l'infrastructure elle-même : la VM, la base de données, le réseau. Ansible, Chef et Puppet configurent surtout des logiciels sur une machine déjà existante. HashiCorp précise explicitement que « Terraform is not a configuration management tool » (Terraform n'est pas un outil de gestion de configuration), et les deux catégories sont généralement complémentaires plutôt que concurrentes : Terraform provisionne un serveur, puis une étape de bootstrapping (ou un playbook Ansible ultérieur) configure le logiciel qui y tourne.
- **Multi-cloud contre cloud unique.** Terraform, Ansible et Pulumi fonctionnent sur de nombreux providers avec un seul workflow. CloudFormation est déclaratif et fiable, mais ne gère qu'AWS.
- **Langage spécifique au domaine contre code généraliste.** Le HCL de Terraform et le JSON/YAML de CloudFormation sont volontairement restreints, ce qui garde un plan facile à lire et à revoir dans une pull request. Pulumi échange cette restriction contre toute la puissance d'un langage généraliste : boucles, tests et gestionnaires de paquets que vous utilisez peut-être déjà, au prix d'une surface plus large à raisonner.
- **Le state comme source de vérité.** Terraform, CloudFormation et Pulumi comparent tous un state enregistré à la réalité avant de rien changer, ce qui est précisément ce qui rend un « plan » possible. Ansible et les outils classiques de gestion de configuration ne conservent généralement pas ce type de fichier de state d'infrastructure ; ils inspectent la cible à chaque exécution et comptent sur l'idempotence de chaque module.

Rien de tout cela ne fait de Terraform le bon outil pour chaque tâche. Une équipe déjà investie dans Ansible pour la gestion de configuration n'a pas besoin de le remplacer ; Terraform se place généralement en dessous, en provisionnant les machines qu'Ansible configure ensuite.

## Pratique

Pensez à deux plateformes utilisées par votre équipe, par exemple un fournisseur cloud et un service DNS. Avec Terraform, les deux sont gérées avec les mêmes commandes et le même format de fichier. Notez où cette cohérence ferait gagner du temps à votre équipe dès aujourd'hui, et identifiez une tâche de votre stack actuelle qu'un outil de gestion de configuration ferait encore mieux que Terraform.

## Références

- [What is Terraform? (HashiCorp)](https://developer.hashicorp.com/terraform/intro)
- [Terraform use cases](https://developer.hashicorp.com/terraform/intro/use-cases)
- [Terraform Registry: providers](https://registry.terraform.io/browse/providers)
- [Terraform workflow overview](https://developer.hashicorp.com/terraform/intro/core-workflow)
- [Terraform vs. other software (comparison index)](https://developer.hashicorp.com/terraform/intro/vs)
- [Terraform vs. Chef, Puppet, and other configuration management tools](https://developer.hashicorp.com/terraform/intro/vs/chef-puppet)
- [Terraform vs. CloudFormation](https://developer.hashicorp.com/terraform/intro/vs/cloudformation)
- [How Ansible works (Red Hat)](https://www.redhat.com/en/ansible-collaborative/how-ansible-works)
- [How Pulumi works](https://www.pulumi.com/docs/iac/concepts/how-pulumi-works/)
- [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
