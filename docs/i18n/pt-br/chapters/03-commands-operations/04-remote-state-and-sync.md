# 3.4 State Remoto e Sincronização

Quando mais de uma pessoa ou pipeline executa o Terraform, o state precisa viver em um backend compartilhado e ser protegido contra gravações simultâneas.

## Configure e migre

```bash
terraform init -migrate-state
```

Depois de adicionar um bloco `backend`, este comando move o state local existente para o backend remoto e confirma antes de sobrescrever qualquer coisa.

## Locking e refresh

- Os backends compatíveis adquirem um lock durante o `plan` e o `apply` para que duas execuções não corrompam o state.
- O `terraform plan` atualiza o state a partir da plataforma real por padrão, revelando drift.
- Nunca edite o arquivo de state manualmente; use os subcomandos `terraform state`.

## Prática

Descreva, em duas frases, o que poderia dar errado se dois engenheiros executassem `terraform apply` ao mesmo tempo contra um arquivo de state local. Depois, explique como um backend remoto com locking evita isso.

## Referências

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [State locking](https://developer.hashicorp.com/terraform/language/state/locking)
- [Command: init (backend initialization)](https://developer.hashicorp.com/terraform/cli/commands/init#backend-initialization)
- [Managing state in automation](https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform)
