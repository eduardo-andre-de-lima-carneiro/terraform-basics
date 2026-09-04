# 4.2 HCP Terraform

O HCP Terraform (antigo Terraform Cloud) é a plataforma gerenciada da HashiCorp para executar o Terraform. Ele combina state remoto, execuções remotas, um registro de módulos privado e aplicação de políticas.

## Conecte e execute

Adicione um bloco `cloud` à configuração e faça login:

```hcl
terraform {
  cloud {
    organization = "EXAMPLE_ORG"

    workspaces {
      name = "practice"
    }
  }
}
```

```bash
terraform login
terraform init
```

O `plan` e o `apply` agora rodam no workspace, não no seu laptop, e o state é armazenado lá automaticamente.

## Recursos úteis

- Workspaces orientados por VCS executam um plano em cada pull request e retêm os applies para aprovação.
- O state é armazenado, versionado, bloqueado com lock e criptografado pela plataforma.
- Políticas Sentinel ou OPA podem bloquear um plano que viole uma regra.
- Conjuntos de variáveis mantêm as credenciais dos providers fora do repositório.

Use um token de API de equipe ou uma identidade com escopo de workspace e privilégio mínimo. Nunca faça commit de um token de `terraform login`.

## Referências

- [HCP Terraform documentation](https://developer.hashicorp.com/terraform/cloud-docs)
- [The `cloud` block](https://developer.hashicorp.com/terraform/cli/cloud/settings)
- [VCS-driven runs](https://developer.hashicorp.com/terraform/cloud-docs/run/ui)
- [Policy enforcement](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement)
- [API tokens](https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/api-tokens)
