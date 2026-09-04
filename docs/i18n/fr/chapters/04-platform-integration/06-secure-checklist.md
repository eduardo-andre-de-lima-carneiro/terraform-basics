# 4.6 Checklist d'intégration sécurisée

Avant de déclarer une intégration prête, vérifiez les points suivants :

- La configuration du backend est correcte et ne contient aucun secret en clair.
- L'authentification utilise OIDC, une service connection ou un jeton à portée limitée, pas une clé cloud à longue durée de vie dans le dépôt. **Sans cela :** une clé divulguée continue de fonctionner jusqu'à ce que quelqu'un le remarque et la fasse tourner à la main, et elle accorde généralement plus de droits que ce dont le pipeline a réellement besoin.
- Le state est stocké dans un backend distant avec verrouillage ou un workspace de plateforme, jamais dans le contrôle de version. **Sans verrouillage :** deux exécutions qui écrivent le state en même temps peuvent le corrompre ou appliquer silencieusement la moitié d'un plan obsolète.
- `terraform plan` s'exécute automatiquement sur les pull requests et sa sortie est visible par les relecteurs.
- Les fichiers de plan enregistrés et les artefacts de plan sont traités comme sensibles : accès restreint, rétention courte, jamais publics.
- `terraform apply` ne s'exécute qu'après la fusion, derrière une approbation requise ou un environnement protégé.
- Les secrets CI/CD sont stockés dans le gestionnaire de secrets de la plateforme et masqués dans les logs.
- Les versions des providers et des modules sont épinglées, et le fichier de verrouillage des dépendances `.terraform.lock.hcl` est committé. **Sans le fichier de verrouillage :** la même configuration peut résoudre une version de provider différente à l'exécution suivante et appliquer des changements que personne n'a écrits.
- Les opérations de destruction sont désactivées ou nécessitent une approbation distincte et explicite.
- Les contrôles de politique (Sentinel, OPA ou un linter) s'exécutent avant l'apply là où c'est pertinent.
- Les accès sont revus quand une personne, un jeton, un runner ou un service change de rôle.

Une intégration est réussie quand elle rend le changement d'infrastructure plus traçable et plus reproductible sans faciliter le détournement des identifiants ou des changements en production.

## Références

- [State: sensitive data](https://developer.hashicorp.com/terraform/language/state/sensitive-data)
- [Terraform recommended practices](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices)
- [Dependency lock file](https://developer.hashicorp.com/terraform/language/files/dependency-lock)
- [Policy enforcement](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement)
- [Injecting secrets into CI/CD (OIDC)](https://developer.hashicorp.com/terraform/tutorials/cloud/dynamic-credentials)
