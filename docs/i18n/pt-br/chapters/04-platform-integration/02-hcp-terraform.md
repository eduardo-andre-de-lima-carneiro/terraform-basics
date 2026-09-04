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

## Como é uma execução guiada por VCS

Conecte um workspace a um repositório (no lugar do bloco `cloud` acima, ou além dele) e cada pull request recebe um **plano especulativo**: uma execução somente de plan, que não pode ser aplicada, publicada no pull request como um status check para que os revisores vejam o efeito antes do merge. Uma execução na branch rastreada vai além e passa por estágios em ordem: plan, depois estimativa de custo se a organização a habilitou, depois qualquer verificação de política, depois apply. O apply normalmente pausa esperando que uma pessoa selecione **Confirm & Apply** na interface, a menos que o workspace esteja configurado para auto-apply.

## Recursos úteis

- Workspaces orientados por VCS executam um plano em cada pull request e retêm os applies para aprovação.
- O state é armazenado, versionado, bloqueado com lock e criptografado pela plataforma.
- Políticas Sentinel ou OPA podem bloquear um plano que viole uma regra.
- Conjuntos de variáveis mantêm as credenciais dos providers fora do repositório.
- A estimativa de custo (desativada por padrão; um owner da organização precisa habilitá-la) mostra o delta de custo mensal estimado para recursos AWS, GCP e Azure, como uma etapa entre o plan e o apply.
- Em vez de `workspaces { name = "..." }`, uma configuração pode apontar para workspaces por `project` ou por `tags`, de modo que um único bloco corresponda a vários workspaces.

Use um token de API de equipe ou uma identidade com escopo de workspace e privilégio mínimo. Nunca faça commit de um token de `terraform login`.

## Referências

- [HCP Terraform documentation](https://developer.hashicorp.com/terraform/cloud-docs)
- [The `cloud` block](https://developer.hashicorp.com/terraform/cli/cloud/settings)
- [VCS-driven runs](https://developer.hashicorp.com/terraform/cloud-docs/run/ui)
- [Cost estimation](https://developer.hashicorp.com/terraform/cloud-docs/cost-estimation)
- [Policy enforcement](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement)
- [API tokens](https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/api-tokens)
