# 4.1 Fondamentaux de l'intégration

Exécuter Terraform sur une plateforme ajoute des services de collaboration et de livraison autour d'une configuration. Les commandes restent familières ; la plateforme fournit l'identité, les permissions, la relecture du plan, l'automatisation et la visibilité des changements.

## Le flux commun

1. Stockez la configuration dans un dépôt de contrôle de version.
2. Configurez un backend distant ou un workspace de plateforme pour le state.
3. Sur une pull request, exécutez `terraform plan` automatiquement et publiez le résultat.
4. Exigez la relecture à la fois du diff de code et du plan.
5. N'exécutez `terraform apply` qu'après la fusion, derrière une approbation ou un environnement protégé.
6. Gardez les identifiants des providers dans le gestionnaire de secrets de la plateforme.

## Choisir où l'exécution a lieu

Les exécutions peuvent se dérouler dans un job CI générique ou dans une plateforme Terraform dédiée. Un job générique est flexible ; une plateforme dédiée ajoute le stockage du state, le verrouillage, l'historique des exécutions et les contrôles de politique sans scripting supplémentaire.

| Aspect | Job CI générique (chapitres 4.3-4.5) | Plateforme Terraform dédiée (chapitre 4.2) |
| --- | --- | --- |
| Stockage du state et verrouillage | Vous configurez vous-même un backend | Intégré, aucun bloc backend nécessaire |
| Historique des exécutions et diffs de plan | Ce que conservent les logs du système CI | Conservé par workspace, consultable |
| Contrôles de politique (Sentinel/OPA) | Nécessite un outil ou un script séparé | Natif, peut bloquer un plan ou un apply |
| Effort de mise en place | Faible : réutilisez le pipeline existant | Plus élevé : une nouvelle plateforme, org et workspace à apprendre |

Aucune des deux options n'est meilleure dans l'absolu ; une petite équipe déjà installée sur GitHub ou GitLab commence souvent par un job générique, puis adopte une plateforme dédiée quand le verrouillage du state ou les contrôles de politique deviennent un vrai besoin.

## Ce qu'il faut configurer

Au minimum, mettez-vous d'accord sur la branche par défaut, la protection de branche, le plan-sur-PR, l'apply-sur-fusion, qui peut approuver un apply, où réside le state, et comment les secrets sont injectés. Ces politiques font partie du processus de livraison, ce ne sont pas des décorations optionnelles.

## Références

- [Running Terraform in automation](https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform)
- [Terraform automation tutorials](https://developer.hashicorp.com/terraform/tutorials/automation)
- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [Terraform recommended practices](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices)
