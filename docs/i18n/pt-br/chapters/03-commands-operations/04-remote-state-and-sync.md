# 3.4 State Remoto e Sincronização

Quando mais de uma pessoa ou pipeline executa o Terraform, o state precisa viver em um backend compartilhado e ser protegido contra gravações simultâneas.

## Configure e migre

```bash
terraform init -migrate-state
```

Depois de adicionar um bloco `backend`, este comando move o state local existente para o backend remoto e confirma antes de sobrescrever qualquer coisa. Executar o `terraform init` novamente depois de mudar a configuração de um backend *já existente* (não ao adicionar um) exige `-migrate-state` ou `-reconfigure` — o Terraform não escolhe um dos dois em silêncio por conta própria.

| Flag | O que faz |
| --- | --- |
| `-migrate-state` | Copia o state atual para a nova configuração de backend |
| `-reconfigure` | Muda para a nova configuração de backend sem migrar o state (começa do zero; o state antigo fica onde estava) |
| `-force-copy` | O mesmo que `-migrate-state`, mas responde "sim" automaticamente a cada prompt de migração — para um `init` scriptado/não interativo |

## Locking e refresh

- Os backends compatíveis adquirem um lock durante o `plan` e o `apply` para que duas execuções não corrompam o state; nem todo backend suporta locking, então verifique a documentação do backend específico antes de confiar nele para um time.
- O `terraform plan` atualiza o state a partir da plataforma real por padrão, revelando drift.
- Nunca edite o arquivo de state manualmente; use os subcomandos `terraform state`.

**`terraform force-unlock <lock-id>`** remove um lock travado sem tocar em nenhuma infraestrutura. Escopo: afeta apenas o lock do state desta configuração, não os resources em si. Use apenas depois de confirmar que o processo que segurava o lock realmente parou (um job de CI que travou, uma execução local que você matou) — o Terraform imprime o lock ID e quem o detém quando um lock está em disputa. Caminho de recuperação: se você forçar o desbloqueio enquanto a execução de outra pessoa ainda está genuinamente em andamento, as duas podem gravar o state ao mesmo tempo e corrompê-lo; se isso acontecer, restaure o último state bom a partir do próprio versionamento do seu backend (por exemplo, o versionamento de um bucket S3) ou a partir de uma saída de `terraform state pull` salva com antecedência.

## Lendo os outputs de outra configuração

Duas configurações Terraform frequentemente precisam compartilhar informações — o ID de uma subnet de um stack de rede, digamos, consumido por um stack de aplicação — sem se fundirem em um único módulo raiz. A data source `terraform_remote_state` lê o state de outra configuração diretamente do backend dela:

```hcl
data "terraform_remote_state" "network" {
  backend = "local"
  config = {
    path = "../network/terraform.tfstate"
  }
}

resource "local_file" "app_config" {
  filename = "app.conf"
  content  = "subnet=${data.terraform_remote_state.network.outputs.subnet_id}"
}
```

Troque `backend = "local"` e seu `path` pelo backend e pelo mapa `config` que a outra configuração realmente usa (por exemplo, `backend = "s3"` com `bucket`/`key`/`region`). Dois pontos para ter em mente:

- Só é possível ler os valores de `output` do módulo raiz da outra configuração — nada que esteja aninhado dentro dos módulos filhos dela, a menos que esse módulo reexponha seus outputs na raiz.
- Qualquer pessoa que consiga ler esses outputs consegue acessar o snapshot completo do state da mesma forma, então isso só limita *o que você referencia*, não quem consegue ver o state subjacente — mantenha segredos fora dos outputs assim como você os mantém fora de tudo mais no state.

## Prática

Descreva, em duas frases, o que poderia dar errado se dois engenheiros executassem `terraform apply` ao mesmo tempo contra um arquivo de state local. Depois, explique como um backend remoto com locking evita isso.

Crie dois diretórios locais, `network` e `app`, cada um com sua própria configuração do provider `local`; dê ao `network` um valor de `output`, depois leia-o a partir do `app` com `terraform_remote_state` e confirme que o `terraform apply` em `app` capta esse valor.

## Referências

- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [State locking](https://developer.hashicorp.com/terraform/language/state/locking)
- [Command: init (backend initialization)](https://developer.hashicorp.com/terraform/cli/commands/init#backend-initialization)
- [Managing state in automation](https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform)
- [Command: force-unlock](https://developer.hashicorp.com/terraform/cli/commands/force-unlock)
- [`terraform_remote_state` data source](https://developer.hashicorp.com/terraform/language/state/remote-state-data)
