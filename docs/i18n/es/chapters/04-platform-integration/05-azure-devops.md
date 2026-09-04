# 4.5 Azure DevOps

Azure DevOps Pipelines ejecuta Terraform como etapas de build y de release. Es habitual en organizaciones que ya usan Azure Boards y Repos, y se integra con las service connections de Azure para las credenciales.

## Pipeline mínimo

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

`TerraformInstaller@1` proviene de la extensión de Terraform para Azure Pipelines; las imágenes gestionadas no incluyen Terraform de forma fiable.

## Buenas prácticas

- Usa una service connection o federación de identidad de carga de trabajo en lugar de almacenar claves de nube.
- Pon `apply` detrás de un entorno con una comprobación de aprobación obligatoria.
- Almacena la configuración del backend y cualquier token como variables secretas de pipeline o en un grupo de variables respaldado por un key vault.
- El artefacto de plan publicado contiene valores de configuración y de state en texto plano; restringe quién puede descargar los artefactos de pipeline y mantén una retención corta.

## Referencias

- [Terraform on Azure documentation (Microsoft Learn)](https://learn.microsoft.com/en-us/azure/developer/terraform/overview)
- [Terraform extension for Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=ms-devlabs.custom-terraform-tasks)
- [Service connections](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints)
- [Approvals and checks for environments](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/approvals)
- [Link secrets from an Azure key vault](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/azure-key-vault)
