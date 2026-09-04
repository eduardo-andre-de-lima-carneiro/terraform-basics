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

## Service connection: secreto frente a workload identity federation

Una service connection de Azure Resource Manager puede guardar un secreto de service principal o usar workload identity federation. Un secreto es una credencial almacenada que un script o un log de pipeline filtrado podría exponer, y necesita rotación manual antes de que expire. La workload identity federation no deja ningún secreto en Azure DevOps: Microsoft Entra confía en los tokens emitidos para esa service connection concreta y devuelve una credencial de corta duración en cada ejecución. Microsoft recomienda ahora la workload identity federation para las nuevas service connections de Azure Resource Manager y ofrece una conversión de un clic para las que ya usan secreto.

## Referencias

- [Terraform on Azure documentation (Microsoft Learn)](https://learn.microsoft.com/en-us/azure/developer/terraform/overview)
- [Terraform extension for Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=ms-devlabs.custom-terraform-tasks)
- [Service connections](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints)
- [Use an Azure Resource Manager service connection](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/connect-to-azure)
- [Approvals and checks for environments](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/approvals)
- [Link secrets from an Azure key vault](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/azure-key-vault)
