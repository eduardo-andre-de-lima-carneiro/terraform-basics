# 4.5 Azure DevOps

Azure DevOps Pipelines runs Terraform as build and release stages. It is common in organizations already using Azure Boards and Repos, and it integrates with Azure service connections for credentials.

## Minimal pipeline

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

`TerraformInstaller@1` comes from the Terraform extension for Azure Pipelines; hosted images do not ship Terraform reliably.

## Good practices

- Use a service connection or workload identity federation instead of storing cloud keys.
- Put `apply` behind an environment with a required approval check.
- Store the backend configuration and any tokens as secret pipeline variables or in a variable group backed by a key vault.
- The published plan artifact contains configuration and state values in clear text; restrict who can download pipeline artifacts and keep retention short.

## References

- [Terraform on Azure documentation (Microsoft Learn)](https://learn.microsoft.com/en-us/azure/developer/terraform/overview)
- [Terraform extension for Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=ms-devlabs.custom-terraform-tasks)
- [Service connections](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints)
- [Approvals and checks for environments](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/approvals)
- [Link secrets from an Azure key vault](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/azure-key-vault)
