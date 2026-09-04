# 4.5 Azure DevOps

Azure DevOps Pipelines exécute Terraform sous forme d'étapes de build et de release. C'est courant dans les organisations qui utilisent déjà Azure Boards et Repos, et cela s'intègre aux service connections Azure pour les identifiants.

## Pipeline minimal

```yaml
trigger:
  branches:
    include: [main]

pool:
  vmImage: ubuntu-latest

steps:
  - task: TerraformInstaller@1
    inputs:
      terraformVersion: latest
    displayName: Install Terraform
  - script: terraform init
    displayName: Init
  - script: terraform validate
    displayName: Validate
  - script: terraform plan -out plan.tfplan
    displayName: Plan
  - publish: plan.tfplan
    artifact: tfplan
    displayName: Publish plan
  - script: terraform apply plan.tfplan
    displayName: Apply
    condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
```

`TerraformInstaller@1` provient de l'extension Terraform pour Azure Pipelines ; les images hébergées ne fournissent pas Terraform de manière fiable.

## Bonnes pratiques

- Utilisez une service connection ou la fédération d'identité de charge de travail au lieu de stocker des clés cloud.
- Placez `apply` derrière un environnement avec un contrôle d'approbation requis.
- Stockez la configuration du backend et tout jeton comme variables de pipeline secrètes ou dans un groupe de variables adossé à un key vault.
- L'artefact de plan publié contient des valeurs de configuration et de state en clair ; restreignez qui peut télécharger les artefacts de pipeline et gardez une rétention courte.

## Service connection : secret ou fédération d'identité de charge de travail

Une service connection Azure Resource Manager peut détenir un secret de service principal ou utiliser la fédération d'identité de charge de travail. Un secret est un identifiant stocké qu'un script ou un log de pipeline divulgué pourrait exposer, et il nécessite une rotation manuelle avant son expiration. La fédération d'identité de charge de travail ne laisse aucun secret dans Azure DevOps : Microsoft Entra fait confiance aux jetons émis pour cette service connection précise et renvoie un identifiant de courte durée à chaque exécution. Microsoft recommande désormais la fédération d'identité de charge de travail pour les nouvelles service connections Azure Resource Manager et propose une conversion en un clic pour celles qui utilisent encore un secret.

## Références

- [Terraform on Azure documentation (Microsoft Learn)](https://learn.microsoft.com/en-us/azure/developer/terraform/overview)
- [Terraform extension for Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=ms-devlabs.custom-terraform-tasks)
- [Service connections](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints)
- [Use an Azure Resource Manager service connection](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/connect-to-azure)
- [Approvals and checks for environments](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/approvals)
- [Link secrets from an Azure key vault](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/azure-key-vault)
