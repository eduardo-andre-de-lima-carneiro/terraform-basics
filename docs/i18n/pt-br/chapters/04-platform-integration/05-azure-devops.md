# 4.5 Azure DevOps

O Azure DevOps Pipelines executa o Terraform como estágios de build e release. É comum em organizações que já usam o Azure Boards e Repos, e ele se integra às service connections do Azure para credenciais.

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

O `TerraformInstaller@1` vem da extensão do Terraform para o Azure Pipelines; as imagens hospedadas não trazem o Terraform de forma confiável.

## Boas práticas

- Use uma service connection ou workload identity federation em vez de armazenar chaves de nuvem.
- Coloque o `apply` atrás de um environment com uma verificação de aprovação obrigatória.
- Armazene a configuração do backend e quaisquer tokens como variáveis de pipeline secretas ou em um variable group respaldado por um key vault.
- O artefato de plano publicado contém valores de configuração e de state em texto claro; restrinja quem pode baixar os artefatos de pipeline e mantenha a retenção curta.

## Referências

- [Terraform on Azure documentation (Microsoft Learn)](https://learn.microsoft.com/en-us/azure/developer/terraform/overview)
- [Terraform extension for Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=ms-devlabs.custom-terraform-tasks)
- [Service connections](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints)
- [Approvals and checks for environments](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/approvals)
- [Link secrets from an Azure key vault](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/azure-key-vault)
