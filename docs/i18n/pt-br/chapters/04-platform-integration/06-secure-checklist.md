# 4.6 Checklist de Integração Segura

Antes de considerar uma integração pronta, verifique o seguinte:

- A configuração do backend está correta e não contém nenhum segredo em texto simples.
- A autenticação usa OIDC, uma service connection ou um token com escopo, não uma chave de nuvem de longa duração no repositório.
- O state é armazenado em um backend remoto com locking ou em um workspace da plataforma, nunca no controle de versão.
- O `terraform plan` roda automaticamente em pull requests e sua saída é visível para os revisores.
- Arquivos de plano salvos e artefatos de plano são tratados como sensíveis: acesso restrito, retenção curta, nunca públicos.
- O `terraform apply` roda somente após o merge, atrás de uma aprovação obrigatória ou ambiente protegido.
- Os segredos de CI/CD são armazenados no gerenciador de segredos da plataforma e mascarados nos logs.
- As versões de providers e módulos estão fixadas, e o arquivo de lock de dependências `.terraform.lock.hcl` passou por commit.
- As operações de destroy estão desativadas ou exigem uma aprovação separada e explícita.
- Verificações de política (Sentinel, OPA ou um linter) rodam antes do apply quando apropriado.
- O acesso é revisado quando uma pessoa, token, runner ou serviço muda de papel.

A integração é bem-sucedida quando torna a mudança de infraestrutura mais rastreável e repetível sem tornar mais fácil o uso indevido de credenciais ou de mudanças em produção.

## Referências

- [State: sensitive data](https://developer.hashicorp.com/terraform/language/state/sensitive-data)
- [Terraform recommended practices](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices)
- [Dependency lock file](https://developer.hashicorp.com/terraform/language/files/dependency-lock)
- [Policy enforcement](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement)
- [Injecting secrets into CI/CD (OIDC)](https://developer.hashicorp.com/terraform/tutorials/cloud/dynamic-credentials)
