# Glossaire

- **Configuration :** les fichiers `.tf` qui décrivent l'infrastructure souhaitée.
- **Provider :** un plugin qui permet à Terraform de gérer une plateforme ou une API spécifique.
- **Ressource :** un objet d'infrastructure unique géré par une configuration Terraform.
- **Source de données (data source) :** une consultation en lecture seule d'informations définies en dehors de la configuration.
- **State :** le fichier dans lequel Terraform enregistre les ressources qu'il gère et leurs dernières valeurs connues.
- **Plan :** un aperçu des actions que Terraform entreprendrait pour atteindre l'état souhaité.
- **Fichier de plan enregistré :** la sortie de `terraform plan -out`, appliquée plus tard pour que `apply` exécute exactement ce qui a été relu.
- **Apply :** l'exécution d'un plan pour créer, mettre à jour ou détruire des ressources.
- **Module :** un groupe réutilisable de fichiers de configuration appelé avec des variables d'entrée.
- **Backend :** l'endroit où Terraform stocke le state, comme un disque local ou un service distant.
- **Workspace :** une instance de state nommée qui permet à une même configuration de gérer plusieurs environnements.
- **HCP Terraform :** la plateforme gérée de HashiCorp pour les exécutions distantes, le state et les politiques (anciennement Terraform Cloud).
- **Fichier de verrouillage des dépendances :** `.terraform.lock.hcl`, qui épingle les versions des providers et leurs sommes de contrôle ; committez-le dans le contrôle de version.
- **Drift (dérive) :** une différence entre l'infrastructure réelle et le state enregistré.
- **Variable :** une entrée nommée qui paramètre une configuration ou un module.
- **Output (sortie) :** une valeur nommée qu'une configuration ou un module expose après l'apply.
- **Bloc `moved` :** une configuration qui indique à Terraform que l'adresse d'une ressource a changé afin qu'elle ne soit pas détruite puis recréée.

## Références

- [Terraform glossary (HashiCorp)](https://developer.hashicorp.com/terraform/docs/glossary)
- [Terraform language documentation](https://developer.hashicorp.com/terraform/language)
- [Terraform CLI documentation](https://developer.hashicorp.com/terraform/cli)
