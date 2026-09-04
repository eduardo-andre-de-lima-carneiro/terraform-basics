# Les bases de Terraform

> Apprenez Terraform en comprenant ce qu'il est, en pratiquant ce qu'il fait, et en gagnant en confiance une petite étape à la fois.

Les bases de Terraform est un cours pratique et guidé destiné aux personnes qui débutent avec Terraform, qui migrent depuis des consoles cloud manuelles ou des scripts de provisionnement, ou qui cherchent un modèle mental plus clair de l'infrastructure as code au quotidien.

[Commencer le cours](menu.md) | [Choisir votre langue](#langues) | [Contribuer](../../../CONTRIBUTING.md)

## Pourquoi ce cours existe

La documentation de Terraform peut être techniquement exacte tout en restant difficile d'accès. Ce projet transforme les idées essentielles en un parcours guidé : des explications courtes, de vraies commandes, des résultats visibles, et des exercices que l'on peut pratiquer dans un répertoire de travail temporaire.

L'objectif n'est pas de mémoriser une liste de commandes. L'objectif est de comprendre l'état de votre infrastructure, d'apporter des changements intentionnels, et de récupérer sereinement quand quelque chose se passe mal.

## Ce que vous allez apprendre

- Comment l'infrastructure as code protège et explique l'historique d'un environnement.
- Comment la configuration, les providers, les ressources, le state et les modules de Terraform s'articulent.
- Comment installer et configurer Terraform pour des projets personnels ou d'équipe.
- Comment relire un plan avant de l'appliquer.
- Comment organiser des modules, stocker le state à distance et collaborer en toute sécurité.
- Comment choisir la bonne commande de récupération face à un changement indésirable.

## Plan du cours

| Chapitre                                                                                   | Objectif                                                     | Ce que vous allez pratiquer                                                     |
| ------------------------------------------------------------------------------------------ | ----------------------------------------------------------- | ----------------------------------------------------------------------------- |
| [1. Concepts de base](chapters/01-basic-concepts/README.md)                                | Les idées derrière l'infrastructure as code et Terraform     | Penser en état souhaité, en plans et en graphes de ressources                  |
| [2. Installation et configuration](chapters/02-installation-configuration/README.md)       | Préparer Terraform à l'usage                                 | Vérifier l'installation, les providers, les identifiants et les backends       |
| [3. Commandes et opérations](chapters/03-commands-operations/README.md)                    | Bâtir un workflow quotidien fiable                           | Init, plan, apply, state, modules, state distant et récupération               |
| [4. Intégration aux plateformes](chapters/04-platform-integration/README.md)               | Exécuter Terraform sur des plateformes hébergées et CI/CD    | Exécutions distantes, pipelines, permissions, secrets et livraison sécurisée   |
| [5. Intégration aux IDE et éditeurs](chapters/05-ide-integration/README.md)                | Utiliser Terraform à travers les éditeurs de code et les IDE | Rédaction, validation, formatage, navigation et configuration des outils       |

## Une première pratique rapide

Une fois Terraform installé, créez un répertoire de pratique jetable :

```bash
mkdir terraform-practice
cd terraform-practice
cat > main.tf <<'EOF'
resource "local_file" "notes" {
  content  = "My first Terraform file\n"
  filename = "${path.module}/notes.txt"
}
EOF
terraform init
terraform plan
terraform apply
terraform destroy   # clean up: the whole directory is disposable
```

Vous venez de créer une configuration, d'initialiser le répertoire de travail, de prévisualiser le changement, de l'appliquer, puis de le supprimer à nouveau. Le chapitre 1 explique ce qui s'est passé à chaque étape.

## Comment utiliser la documentation

1. Commencez par le [menu de la documentation](menu.md).
2. Lisez le chapitre 1 avant de vous lancer dans la mémorisation des commandes.
3. Réalisez les étapes de configuration du chapitre 2.
4. Parcourez le chapitre 3 dans un répertoire de travail jetable.
5. Explorez le chapitre 4 pour la plateforme utilisée par votre équipe.
6. Lisez le chapitre 5 pour votre éditeur de code ou votre IDE.
7. Utilisez le [glossaire](glossary.md) dès qu'un terme ne vous est pas familier.

Chaque leçon est un fichier Markdown autonome, relié par des chemins relatifs afin d'être lu directement sur GitHub.

## Langues

Le cours est disponible en quatre langues :

- [English](../../../README.md)
- [Français](README.md)
- [Português (Brasil)](../pt-br/README.md)
- [Español](../es/README.md)

## Valeurs du projet

- **Pratique :** les exemples doivent aboutir à quelque chose que l'apprenant peut observer.
- **Accessible :** expliquer l'idée avant d'introduire la commande.
- **Sûr :** utiliser des répertoires de travail jetables et rendre les opérations destructrices explicites.
- **Ouvert :** garder la documentation gratuite, réutilisable et facile à améliorer.

## Contribuer

Vous avez trouvé une explication confuse, un exercice manquant ou un lien cassé ? Lisez le [guide de contribution](../../../CONTRIBUTING.md) et aidez à améliorer la première expérience Terraform du prochain apprenant.

## Origine

Ce cours est né d'une expérience DevSecOps auprès d'équipes qui migraient de changements cloud manuels vers l'infrastructure as code. La documentation officielle et les sites de référence étaient utiles, mais certains apprenants avaient besoin d'un chemin d'entrée plus guidé et plus pratique. Les bases de Terraform a été créé pour offrir ce chemin et pour rendre le processus d'apprentissage plus facile à partager.

Le projet est délibérément collaboratif. Les retours, corrections, exemples et traductions sont les bienvenus.
