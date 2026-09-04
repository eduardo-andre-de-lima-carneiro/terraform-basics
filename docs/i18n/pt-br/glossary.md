# Glossário

- **Configuração:** Os arquivos `.tf` que descrevem a infraestrutura desejada.
- **Provider:** Um plugin que permite ao Terraform gerenciar uma plataforma ou API específica.
- **Resource:** Um único objeto de infraestrutura gerenciado por uma configuração do Terraform.
- **Data source:** Uma consulta somente leitura de informações definidas fora da configuração.
- **State:** O arquivo em que o Terraform registra os resources que gerencia e seus últimos valores conhecidos.
- **Plano:** Uma pré-visualização das ações que o Terraform executaria para chegar ao estado desejado.
- **Arquivo de plano salvo:** A saída de `terraform plan -out`, aplicada depois para que o `apply` execute exatamente o que foi revisado.
- **Apply:** A execução de um plano para criar, atualizar ou destruir resources.
- **Módulo:** Um grupo reutilizável de arquivos de configuração chamado com variáveis de entrada.
- **Backend:** O lugar onde o Terraform armazena o state, como o disco local ou um serviço remoto.
- **Workspace:** Uma instância nomeada de state que permite a uma configuração gerenciar vários ambientes.
- **HCP Terraform:** A plataforma gerenciada da HashiCorp para execuções remotas, state e políticas (antigo Terraform Cloud).
- **Arquivo de lock de dependências:** `.terraform.lock.hcl`, que fixa versões de provider e checksums; faça commit dele no controle de versão.
- **Drift (divergência):** Uma diferença entre a infraestrutura real e o state registrado.
- **Variável:** Uma entrada nomeada que parametriza uma configuração ou módulo.
- **Output:** Um valor nomeado que uma configuração ou módulo expõe após o apply.
- **Bloco `moved`:** Configuração que informa ao Terraform que o endereço de um resource mudou, para que ele não seja destruído e recriado.

## Referências

- [Terraform glossary (HashiCorp)](https://developer.hashicorp.com/terraform/docs/glossary)
- [Terraform language documentation](https://developer.hashicorp.com/terraform/language)
- [Terraform CLI documentation](https://developer.hashicorp.com/terraform/cli)
