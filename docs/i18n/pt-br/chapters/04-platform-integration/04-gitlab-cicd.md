# 4.4 GitLab CI/CD

O GitLab CI/CD executa o Terraform em estágios de pipeline e também pode hospedar o state por meio do seu recurso gerenciado de state do Terraform.

## Pipeline mínimo

```yaml
stages: [validate, plan, apply]

default:
  image:
    name: hashicorp/terraform:latest
    entrypoint: [""]

validate:
  stage: validate
  script:
    - terraform init
    - terraform validate

plan:
  stage: plan
  script:
    - terraform plan -out plan.tfplan
  artifacts:
    paths: [plan.tfplan]
    expire_in: 1 day

apply:
  stage: apply
  script:
    - terraform apply plan.tfplan
  when: manual
  only: [main]
```

O override `entrypoint: [""]` é obrigatório: a imagem `hashicorp/terraform` executa o binário do Terraform como seu entrypoint, então, sem ele, as linhas de `script` não conseguem iniciar um shell.

O GitLab não distribui mais seu próprio template de pipeline do Terraform nem a imagem de job associada; o pipeline acima usa diretamente a imagem `hashicorp/terraform` da HashiCorp, que continua sendo uma forma válida e sem dependências de rodar comandos do Terraform em um job. Para um projeto maior, a recomendação atual do GitLab é montar pipelines de validate/plan/apply a partir de [componentes de CI/CD](https://docs.gitlab.com/ee/ci/components/) reutilizáveis do seu catálogo, em vez de escrever cada estágio manualmente.

## Boas práticas

- Mantenha o `apply` como um job manual na branch padrão para que um humano o confirme.
- Passe o artefato `plan.tfplan` salvo para o `apply` para que ele execute exatamente o que foi revisado.
- Um arquivo de plano salvo guarda valores de configuração e de state em texto claro. Mantenha o artefato com vida curta (`expire_in`) e restrito ao projeto.
- Armazene as credenciais como variáveis de CI/CD mascaradas e protegidas.
- Use o state gerenciado do Terraform pelo GitLab ou um backend externo, nunca o repositório.

## Referências

- [Terraform with GitLab (Infrastructure as Code)](https://docs.gitlab.com/ee/user/infrastructure/iac/)
- [GitLab-managed Terraform state](https://docs.gitlab.com/ee/user/infrastructure/iac/terraform_state.html)
- [CI/CD variables (masked and protected)](https://docs.gitlab.com/ee/ci/variables/)
- [Job artifacts](https://docs.gitlab.com/ee/ci/jobs/job_artifacts.html)
- [hashicorp/terraform image](https://hub.docker.com/r/hashicorp/terraform)
- [CI/CD components](https://docs.gitlab.com/ee/ci/components/)
