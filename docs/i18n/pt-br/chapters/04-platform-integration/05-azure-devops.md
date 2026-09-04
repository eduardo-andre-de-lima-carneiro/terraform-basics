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

## Service connection: secret ou workload identity federation

Uma service connection do Azure Resource Manager pode guardar um secret de service principal ou usar workload identity federation. Um secret é uma credencial armazenada que um script ou um log de pipeline vazado poderia expor, e precisa de rotação manual antes de expirar. A workload identity federation não deixa nenhum secret no Azure DevOps: o Microsoft Entra confia nos tokens emitidos para aquela service connection específica e devolve uma credencial de curta duração a cada execução. A Microsoft agora recomenda workload identity federation para novas service connections do Azure Resource Manager e oferece uma conversão de um clique para as que ainda usam secret.

## Referências

- [Terraform on Azure documentation (Microsoft Learn)](https://learn.microsoft.com/en-us/azure/developer/terraform/overview)
- [Terraform extension for Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=ms-devlabs.custom-terraform-tasks)
- [Service connections](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints)
- [Use an Azure Resource Manager service connection](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/connect-to-azure)
- [Approvals and checks for environments](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/approvals)
- [Link secrets from an Azure key vault](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/azure-key-vault)
