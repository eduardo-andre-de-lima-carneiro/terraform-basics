# 1.1 L'infrastructure as code

L'infrastructure as code (IaC) utilise un modèle descriptif et versionné pour définir et déployer l'infrastructure — réseaux, machines virtuelles, répartiteurs de charge, bases de données et la topologie qui les relie — au lieu de processus manuels et de clics dans une console. Tout comme le même code source produit toujours le même binaire, une configuration IaC produit le même environnement à chaque déploiement.

## Le problème qu'elle résout

Sans infrastructure as code, la configuration réelle ne vit que dans une console et dans la tête de quelques personnes. Chaque environnement dérive vers un « flocon de neige » unique et non documenté, impossible à reproduire automatiquement, et le reconstruire ou l'auditer devient une affaire de suppositions. Terraform et les outils similaires conservent au contraire la configuration voulue dans des fichiers structurés, si bien que les changements d'infrastructure passent par la même revue et le même historique de contrôle de version que le code applicatif.

## Exemples pratiques

- **Duplication multi-région ou multi-compte.** Appliquer la même configuration pour créer un environnement identique dans une seconde région ou un second compte cloud, au lieu de répéter la configuration manuelle à la main.
- **Environnements de test et de revue éphémères.** Provisionner une pile complète pour une pull request ou un test de charge, puis la détruire une fois le travail terminé, pour que les environnements cessent d'être rares, partagés et obsolètes.
- **Reprise après sinistre.** Reconstruire la topologie de production à partir de la configuration dans une nouvelle région après une panne, au lieu de la reconstituer de mémoire et à partir de tickets de support.
- **Landing zones standardisées.** Offrir à chaque équipe le même socle de réseau, de journalisation et de contrôle d'accès en réutilisant un module, au lieu d'intégrer chaque équipe manuellement.

## Bénéfices

- **Cohérence.** La même configuration produit toujours le même environnement, ce qui élimine la dérive de configuration (drift) et les surprises du type « ça marche dans mon environnement ».
- **Idempotence.** Appliquer une configuration à répétition fait converger l'environnement vers le même état, que la cible parte de zéro ou soit partiellement construite.
- **Vitesse à grande échelle.** Des environnements qui prenaient autrefois des jours de travail manuel peuvent être provisionnés, dupliqués ou détruits en quelques minutes.
- **Revue et retour en arrière.** Les changements d'infrastructure passent par la même pull request, le même diff et le même historique de versions que le code applicatif ; un mauvais changement peut donc être revu avant sa mise en production, puis annulé après coup.
- **Un langage commun entre les équipes.** Développeurs et opérateurs lisent les mêmes fichiers, ce qui réduit les transmissions et les erreurs de traduction du provisionnement par ticket.

## Défis

- **Une courbe d'apprentissage.** Les équipes doivent apprendre un langage de configuration, un modèle de state et le schéma de ressources d'un provider avant d'être productives.
- **Gestion du state et du drift.** Si quelqu'un modifie une ressource en dehors de l'outil, par exemple directement dans une console, le state enregistré et la réalité divergent jusqu'à ce qu'ils soient réconciliés.
- **Rayon d'impact (blast radius).** Un seul mauvais `apply` peut modifier ou détruire de nombreuses ressources à la fois ; les garde-fous abordés au Chapitre 3, comme la revue préalable d'un plan, existent à cause de ce risque.
- **Secrets et données sensibles.** La configuration et le state peuvent finir par contenir des identifiants ou d'autres valeurs sensibles s'ils ne sont pas gérés délibérément.
- **Tester le code d'infrastructure.** Valider une configuration en profondeur implique généralement de la provisionner réellement quelque part, ce qui coûte du temps et de l'argent que tester du code applicatif ne coûte pas.

## Pratique

Notez chaque étape manuelle que vous suivriez pour créer une petite ressource dans la console de votre cloud. Cette liste illustre la valeur de l'infrastructure as code : elle transforme ces étapes en un fichier que vous pouvez revoir, répéter et annuler. Choisissez ensuite un élément parmi les défis ci-dessus et notez comment le processus de votre équipe devrait changer pour y faire face.

## Références

- [What is infrastructure as code? (Microsoft Learn, Azure DevOps)](https://learn.microsoft.com/en-us/devops/deliver/what-is-infrastructure-as-code)
- [What is infrastructure as code? (AWS)](https://aws.amazon.com/what-is/iac/)
- [What is Terraform? (HashiCorp)](https://developer.hashicorp.com/terraform/intro)
- [Terraform use cases](https://developer.hashicorp.com/terraform/intro/use-cases)
